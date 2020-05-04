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



```
// ELSIFでもELSEIFでもOK
CREATE OR REPLACE FUNCTION func4_1(integer)
RETURNS text AS $$
        DECLARE
                var ALIAS FOR $1;
        BEGIN
                IF var > 10 THEN
                        RETURN '10以上です';
                ELSEIF var = 10 THEN
                        RETURN '10です';
                ELSE
                        RETURN '10未満です';
                END IF;
        END;
$$ LANGUAGE plpgsql;

// 起動

   
psql -U postgres -c "SELECT func4_1(8);"
psql -U postgres -c "SELECT func4_1(10);"
psql -U postgres -c "SELECT func4_1(11);"
```
