# 繰り返し

## LOOP

```
// LOOP EXIT
CREATE OR REPLACE FUNCTION func4_4(integer)
RETURNS integer AS $$
        DECLARE
                var ALIAS FOR $1;
                num integer := 0;
                gokei integer := 0;
        BEGIN
                LOOP
                        num = num + 1;
                        gokei = gokei + num;
                        IF num = var THEN
                                EXIT;
                        END IF;
                END LOOP;
                RETURN gokei;
        END;
$$ LANGUAGE plpgsql;

// 起動
psql -U postgres -c "SELECT func4_4(10);"
```


## WHILE LOOP

```
// LOOP EXIT
CREATE OR REPLACE FUNCTION func4_5(integer)
RETURNS integer AS $$
        DECLARE
                var ALIAS FOR $1;
                num integer := 0;
                gokei integer := 0;
        BEGIN
                WHILE num < var LOOP
                        num = num + 1;
                        gokei = gokei + num;
                END LOOP;
                RETURN gokei;
        END;
$$ LANGUAGE plpgsql;

// 起動
psql -U postgres -c "SELECT func4_5(10);"
```
