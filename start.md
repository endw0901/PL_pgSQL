# 変数

## 参考
https://qiita.com/spaciba_h_t/items/e9dac4de27c7d44c5be6

## PostgreSQLコンテナにアクセス
* pull済みの前提

```
// コンテナID取得して,開始
docker ps
docker start xxxxxx(コンテナID)

// コンテナnameでコンテナにアクセス
docker container ls
docker exec -it (コンテナname)dev-postgres bash
```


## コンテナ上でファイルからSQL実行
* docker上でvimを使えるようにする

https://qiita.com/YumaInaura/items/3432cc3f8a8553e05a6e


```
// SQLコマンド作成
cd /usr/postgre_test
touch XXXX
vim xxxxx

// コンテナ上のpsqlコマンド
psql -U postgres -f ファイル名

```
