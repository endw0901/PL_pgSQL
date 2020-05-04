# 制御

## IF

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


## CASE

```
// case 変数 when 評価値
CREATE OR REPLACE FUNCTION func4_2(integer)
RETURNS text AS $$
        DECLARE
                var ALIAS FOR $1;
        BEGIN
                CASE var
                        WHEN 10,11,12,13,14,15,16,17,18,19 THEN
                                RETURN '10代です';
                        WHEN 20,21,22,23,24,25,26,27,28,29 THEN
                                RETURN '20代です';
                        ELSE
                                RETURN 'それ以外です';
                END CASE;
        END;
$$ LANGUAGE plpgsql;

// 起動
psql -U postgres -c "SELECT func4_2(21);"

// case when 評価式

CREATE OR REPLACE FUNCTION func4_3(integer)
RETURNS text AS $$
        DECLARE
                var ALIAS FOR $1;
        BEGIN
                CASE
                        WHEN var > 10 THEN
                                RETURN '10より大です';
                        WHEN var = 10 THEN
                                RETURN '10です';
                        ELSE
                                RETURN '10未満です';
                END CASE;
        END;
$$ LANGUAGE plpgsql;
```


