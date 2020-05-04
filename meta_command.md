# メタコマンド

## ヘルプ
* \?

## テーブル定義の確認

```
// DB一覧
psql -U postgres -c "\l"

// テーブル一覧
psql -U postgres -c "\dt"

// テーブル定義（テーブルを指定）
psql -U postgres -c "\d TYPE_SAMPLE"
```
