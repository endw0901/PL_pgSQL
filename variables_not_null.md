# NOT NULL・デフォルト値

## NOT NULL不許可でデフォルト値設定

```
// 
CREATE OR REPLACE FUNCTION func3_2()
RETURNS integer AS $$
  DECLARE
    var integer NOT NULL DEFAULT 2;
    var2 integer NOT NULL := 8;
  BEGIN
    RETURN var + VAR2;
  END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func3_2();
```

