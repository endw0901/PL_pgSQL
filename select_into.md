# 関数の本体でSQL実行

* SELECT INTO
* CREATE TABLE (DROP IF EXIST)
* RETURNのないプロシージャ－で実行

## SELECT INTO 変数へ

```
// テーブル作成
CREATE TABLE TYPE_SAMPLE(
        user_id numeric,
        user_name text,
        primary key(user_id)
);

// SELECT INTO
CREATE OR REPLACE FUNCTION func_d1(integer)
RETURNS text AS $$
        DECLARE
                var_id ALIAS FOR $1;
                var_user_name TYPE_SAMPLE.user_name%TYPE;
                var integer NOT NULL DEFAULT 0;
        BEGIN
                SELECT user_name INTO var_user_name FROM TYPE_SAMPLE WHERE user_id = var_id;
                RETURN var_user_name;
        END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func_d1(1);
```


## テーブル作成

```
// 関数
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

// プロシージャ
CREATE OR REPLACE PROCEDURE proc_d2()
        BEGIN
                DROP TABLE IF EXISTS TYPE_SAMPLE;
                CREATE TABLE TYPE_SAMPLE(
                        user_id numeric,
                        user_name text,
                        primary key(user_id)
                );
        END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func_d2();
SELECT proc_d2();
```
