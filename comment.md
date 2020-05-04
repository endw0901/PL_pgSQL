# コメント

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

