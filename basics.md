# Basics


## 関数作成

```
// 関数作成
CREATE OR REPLACE FUNCTION helloplpgsql()
RETURNS text
AS $$
BEGIN
RETURN 'Hello PL/pgSQL';
END;
$$
LANGUAGE plpgsql;

// 起動
SELECT helloplpgsql();

```

## 関数確認メタコマンド

```
// 関数確認
\df

```


## コメント

```
CREATE OR REPLACE FUNCTION func2_2()
RETURNS text AS $$
  BEGIN
    /* ここからコメント
    コメントです
    ここまでコメントです */
    
    -- コメント
      RETURN 'Hello PL/pgSQL'; -- ここから行末までコメントです。
  END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func2_2();
```

