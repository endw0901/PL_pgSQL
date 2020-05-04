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

