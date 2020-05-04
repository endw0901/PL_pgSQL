# テーブル作成、値insert

## PostgreSQL起動

```
// コンテナID取得して,開始
docker ps
docker start xxxxxx(コンテナID)

// コンテナnameでコンテナにアクセス
docker container ls
docker exec -it (コンテナname)dev-postgres bash

// SQLコマンド作成
cd /usr/postgre_test
touch XXXX
vim xxxxx

// コンテナ上のpsqlコマンド
psql -U postgres -f ファイル名
```

## テーブル作成

```

// テーブル作成
CREATE TABLE TYPE_SAMPLE(
        user_id numeric,
        user_name text,
        primary key(user_id)
);

// テーブル作成
CREATE TABLE TYPE_SAMPLE(
        user_id numeric,
        user_name text,
        primary key(user_id)
);
```

## テーブルに値INSERT

```
// テーブル確認
psql -U postgres -c "\dt;"

// INSERT
psql -U postgres -c "INSERT INTO TYPE_SAMPLE VALUES (1,'john');";

// 値確認
psql -U postgres -c "SELECT * FROM TYPE_SAMPLE;"


```

