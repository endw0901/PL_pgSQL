# 関数の本体でSQL実行

* SELECT INTO
* CREATE TABLE (DROP IF EXIST)
* RETURNのないプロシージャ－で実行

## SELECT INTO 変数へ

```
// 
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

## NOT NULL不許可でデフォルト値設定

```
// 
CREATE OR REPLACE FUNCTION func3_2()
RETURNS integer AS $$
  DECLARE
    var integer NOT NULL DEFAULT 2;
    var2 integer NOT NULL := 8;
  BEGIN
    RETURN var + var2;
  END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func3_2();
```

## 引数で変数を定義

```
// 
CREATE OR REPLACE FUNCTION func3_3(var integer)
RETURNS integer AS $$
  BEGIN
    RETURN var;
  END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func3_3(5);
```



## 引数で変数の型のみ指定＆本文で引数の位置と変数名を指定

```
// 
CREATE OR REPLACE FUNCTION func3_4(integer)
RETURNS integer AS $$
  DECLARE
    var ALIAS FOR $1;
  BEGIN
    RETURN var;
  END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func3_4(6);
```


## 変数宣言時に、変数の型を指定したテーブルの型と同じものを%TYPE引用して宣言

```
// テーブル作成
CREATE TABLE TYPE_SAMPLE(
        user_id numeric,
        user_name text,
        primary key(user_id)
);

// %TYPEで型をテーブルから引用
CREATE OR REPLACE FUNCTION func3_6()
RETURNS text AS $$
  DECLARE
    name TYPE_SAMPLE.user_name%TYPE;
  BEGIN
    name := 'sample text';
    RETURN name;
  END;
$$ LANGUAGE plpgsql;

// 起動(コンテナ上で)
psql -U postgres -c "SELECT func3_6();"
```
