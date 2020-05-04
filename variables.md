# 変数

## 宣言部で定義

```
// % に次の引数('PL/pgSQL')を代入できるテキストテンプレート的な使い方ができる
CREATE OR REPLACE FUNCTION func3_1()
RETURNS integer AS $$
  DECLARE
    var integer;
  BEGIN
    var := 10;
    RETURN var;
  END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func3_1();
```

