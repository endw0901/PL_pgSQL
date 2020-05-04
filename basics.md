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

// 関数確認
\df


```

