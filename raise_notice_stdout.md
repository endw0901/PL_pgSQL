# 標準出力メッセージ

## RAISE

```
// % に次の引数('PL/pgSQL')を代入できるテキストテンプレート的な使い方ができる
CREATE OR REPLACE FUNCTION func2_4()
RETURNS void AS $$
  BEGIN
    RAISE NOTICE '本書は%です', 'PL/pgSQL';
    RETURN;
  END;
$$ LANGUAGE plpgsql;

// 起動
SELECT func2_4();
```

