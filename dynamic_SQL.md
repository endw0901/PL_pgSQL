# 動的SQL

## テーブル名や条件に変数を使う

* テーブル名はformat、条件はUSING

* 実行時オーバーヘッドを防止するとともに、引用符付けするとか、エスケープをする必要がないため、SQLインジェクション攻撃に対してより襲われにくくなります。

* 例：43.5.4

https://www.postgresql.jp/document/11/html/plpgsql-statements.html


```
EXECUTE format('SELECT count(*) FROM %I '
   'WHERE inserted_by = $1 AND inserted <= $2', tabname)
   INTO c
   USING checked_user, checked_date;
```

## 例

* より長大な例

https://www.postgresql.jp/document/11/html/plpgsql-porting.html#PLPGSQL-PORTING-EX2


### FUNCTIONから別のFUNCTIONをCALL

* 戻りがないときはperformを使う(select => perform)
* 複数のfunctionを呼び出せる

```
// call対象のfunction　d2, d4は同じようなもの
CREATE OR REPLACE FUNCTION func_d2()
RETURNS void AS $$
        BEGIN
                DROP TABLE IF EXISTS TYPE_SAMPLE;
                CREATE TABLE TYPE_SAMPLE(
                        user_id numeric,
                        user_name text,
                        primary key(user_id)
                );
                RETURN;
        END;
$$ LANGUAGE plpgsql;

// create
psql -U postgres -f func_d2


// call元 ※戻り値がない場合はselectの代わりにperformとする。selectはエラーとなる
CREATE OR REPLACE FUNCTION func_d3()
RETURNS void AS $$
        BEGIN
                PERFORM func_d2();
                PERFORM func_d4();
                RETURN;
        END;
$$ LANGUAGE plpgsql;

// create・起動
psql -U postgres -f func_d3
psql -U postgres -c "SELECT func_d3();"
```



### FUNCTIONから直接EXECUTE（テーブル名を動的SQL)

* 動的SQLでテーブル名も条件も変数とする
* 公式の方法でオーバーヘッドを小さくする


```
// テーブル名を文字列の引数で受け取り、EXECUTE+formatで変数付きの実行
CREATE OR REPLACE FUNCTION func_d5(char)
RETURNS void AS $$
        DECLARE
                var_tblname ALIAS FOR $1;
        BEGIN
                EXECUTE format('DROP TABLE IF EXISTS %I ', var_tblname);
                EXECUTE format('CREATE TABLE %I(
                        user_id numeric,
                        user_name text,
                        primary key(user_id)
                ) ', var_tblname);
                RETURN;
        END;
$$ LANGUAGE plpgsql;

// create・起動(文字列でテーブル名を引き渡す)
psql -U postgres -f func_d5
psql -U postgres -c "SELECT func_d5('TYPE_SAMPLE5');"
```


### 条件も変数とする

* 上のfunc_d5を別の関数からcallして、その後selectする


```
EXECUTE format('SELECT count(*) FROM %I '
   'WHERE inserted_by = $1 AND inserted <= $2', tabname)
   INTO c
   USING checked_user, checked_date;
```

```
// テーブル名を文字列の引数で受け取り、EXECUTE+formatで変数付きの実行
CREATE OR REPLACE FUNCTION func_d6(char, char, integer)
RETURNS void AS $$
        DECLARE
                var_tblname ALIAS FOR $1;
                var_OUT ALIAS FOR $2;
                var_user_id ALIAS FOR $3;
        BEGIN
                /** 引数1のテーブル名でdrop＆create **/
                PERFORM func_d5(var_tblname);
                /** サンプルデータ挿入 **/
                EXECUTE format('INSERT INTO %I VALUES (1,''john'')', var_tblname);
                /** 引数2のkeyで表示 **/
                EXECUTE format('DROP TABLE IF EXISTS %I ', var_OUT);
                EXECUTE format('
                        CREATE TABLE %I AS
                        SELECT
                                *
                        FROM %I
                        WHERE user_id = $1
                        ', var_OUT, var_tblname)
                        USING var_user_id;
                RETURN;
        END;
$$ LANGUAGE plpgsql;

// create
psql -U postgres -f func_d6
// 起動＋引数（入力テーブル、出力テーブル、key）
psql -U postgres -c "select func_d6('type_sample6', 't6', 1);"
// 作成したテーブルを確認
psql -U postgres -c "select * from t6;"
```

* %I, %I→formatで指定した順に指定（上記例参照）
