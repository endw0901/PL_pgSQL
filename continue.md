# スキップ

## continue

```
// continue
CREATE OR REPLACE FUNCTION func4_8(integer)
RETURNS void AS $$
        DECLARE
                var ALIAS FOR $1;
        BEGIN
                FOR i IN 1 .. var LOOP
                        CONTINUE WHEN mod(i,2) = 0;
                        RAISE NOTICE '%', i;
                END LOOP;
                RETURN;
        END;
$$ LANGUAGE plpgsql;

// 起動
psql -U postgres -c "SELECT func4_8(10);"
```

## continueをifに置き換え（分かりやすい）

```
// continue if
CREATE OR REPLACE FUNCTION func4_9(integer)
RETURNS void AS $$
        DECLARE
                var ALIAS FOR $1;
        BEGIN
                FOR i IN 1 .. var LOOP
                        IF mod(i,2) != 0 THEN
                                RAISE NOTICE '%', i;
                        END IF;
                END LOOP;
                RETURN;
        END;
$$ LANGUAGE plpgsql;

// 起動
psql -U postgres -c "SELECT func4_9(10);"
```
