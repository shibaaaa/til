## 概要

UNIXタイムとTIMESTAMP型との相互変換をする方法

## サンプル

### UNIXタイム→TIMESTAMP型

`FROM_UNIXTIME`を使う

```sql
mysql> SELECT FROM_UNIXTIME(1196440219);
        -> '2007-11-30 10:30:19'

-- 指定したフォーマットに変換できる
mysql> SELECT FROM_UNIXTIME(UNIX_TIMESTAMP(),
    ->                      '%Y %D %M %h:%i:%s %x');
        -> '2007 30th November 10:30:59 2007'
```

### TIMESTAMP型→UNIXタイム

`UNIX_TIMESTAMP`を使う

```sql
mysql> SELECT UNIX_TIMESTAMP('2007-11-30 10:30:19');
        -> 1196440219
```


## 参考

- [MySQL :: MySQL 5\.6 リファレンスマニュアル :: 12\.7 日付および時間関数](https://dev.mysql.com/doc/refman/5.6/ja/date-and-time-functions.html#function_from-unixtime)
- [MySQL :: MySQL 5\.6 リファレンスマニュアル :: 12\.7 日付および時間関数](https://dev.mysql.com/doc/refman/5.6/ja/date-and-time-functions.html#function_unix-timestamp)
