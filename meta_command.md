# メタコマンド

## ヘルプ
* \?

## テーブル定義の確認

```
// テーブル一覧
psql -U postgres -c "\dt"

// テーブル定義（テーブルを指定）
psql -U postgres -c "\d TYPE_SAMPLE"
```
