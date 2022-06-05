## 概要

`EXPLAIN`で実行計画を出せるが、実際にクエリを投げるわけではないため、実際にクエリを投げた際と実行計画でINDEXなど差異が出ることがある。

`EXPLAIN ANALYZE`を使うと実際にクエリを投げた結果を返すので、実行計画からは発見できないクエリのボトルネックを見つけられる。


## サンプル

PostgreSQLで実施。

booksテーブルがあったとする。

```sql
EXPLAIN ANALYZE SELECT * FROM books;
                                             QUERY PLAN
----------------------------------------------------------------------------------------------------
 Seq Scan on books  (cost=0.00..15.30 rows=530 width=124) (actual time=0.016..0.017 rows=2 loops=1)
 Planning Time: 0.531 ms
 Execution Time: 0.043 ms
(3 rows)
```

### MySQLの対応

バージョン8.0.18以降で使えるので注意。

## 参考

- [MySQLのEXPLAIN ANALYZEの読み方を勉強したよ \- Qiita](https://qiita.com/Nyokki/items/c2d95cb2a75d3c0acb64)
- [ANALYZE](https://www.postgresql.org/docs/current/sql-explain.html)
- [EXPLAIN ANALYZE による情報の取得
](https://dev.mysql.com/doc/refman/8.0/ja/explain.html#explain-analyze)
