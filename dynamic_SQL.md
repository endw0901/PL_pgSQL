# 動的SQL

## 条件に変数を使う

* USING

* 実行時オーバーヘッドを防止するとともに、引用符付けするとか、エスケープをする必要がないため、SQLインジェクション攻撃に対してより襲われにくくなります。

* 例：43.5.4

https://www.postgresql.jp/document/11/html/plpgsql-statements.html


```
EXECUTE format('SELECT count(*) FROM %I '
   'WHERE inserted_by = $1 AND inserted <= $2', tabname)
   INTO c
   USING checked_user, checked_date;
```

* より長大な例

https://www.postgresql.jp/document/11/html/plpgsql-porting.html#PLPGSQL-PORTING-EX2


```

```
