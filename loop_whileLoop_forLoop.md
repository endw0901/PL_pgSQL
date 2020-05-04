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


## FOR LOOP

```
// FOR LOOP
CREATE OR REPLACE FUNCTION func4_6(integer)
RETURNS integer AS $$
        DECLARE
                var ALIAS FOR $1;
                gokei integer := 0;
        BEGIN
                FOR i IN 1 .. var LOOP
                        gokei = gokei + i;
                END LOOP;
                RETURN gokei;
        END;
$$ LANGUAGE plpgsql;


// FOR LOOP
CREATE OR REPLACE FUNCTION func4_7(integer)
RETURNS SETOF integer AS $$
        DECLARE
                var ALIAS FOR $1;
                gokei integer := 0;
        BEGIN
                FOR i IN REVERSE var .. 1 BY 2 LOOP
                        RETURN NEXT i;
                END LOOP;
                RETURN;
        END;
$$ LANGUAGE plpgsql;

// 起動
psql -U postgres -c "SELECT func4_5(10);"
```

