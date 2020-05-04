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
psql -U postgres -c "SELECT func_d6();"
```
