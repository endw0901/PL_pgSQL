# 定数

## CONSTANT

```
// DECLAREで宣言したCONSTANTは、BEGINで変更するとエラーとなる
CREATE OR REPLACE FUNCTION func3_8(integer)
RETURNS integer AS $$
        DECLARE
                num ALIAS FOR $1;
                cons_num CONSTANT integer := 5;
                rtn integer;
        BEGIN
                rtn := num * cons_num;
                RETURN rtn;
        END;
$$ LANGUAGE plpgsql;

// 起動
psql -U postgres -c "SELECT func3_8(6);"
```
