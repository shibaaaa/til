## 概要

Amazon RedshiftでUNIXタイムとTIMESTAMP型との相互変換をする方法。
RedshiftはPostgreSQLベースだが、UNIXTIMEの取り扱いはポスグレとは別。

## サンプル

### UNIXTIME → TIMESTAMP

秒数まで含んでいる場合

```sql
TIMESTAMP 'epoch' + 1548295768 * INTERVAL '1 second'
```

### TIMESTAMP → UNIXTIME

```sql
extract(epoch FROM timestamp '2022-06-01 00:00:00')
```

## 参考

- [How does it feel?: redshiftのSQLでunixtimeをtimestamp型に変換](http://itsneatlife.blogspot.com/2013/09/redshiftsqlunixtimetimestamp.html)
- [Amazon Redshift: unixtime形式のタイムスタンプをTIMESTAMP型に変換する方法x2 \| DevelopersIO](https://dev.classmethod.jp/articles/amazon-redshift-how-to-convert-unixtime-format-data-to-timestamp/)
- [Manipulating Dates, Datetimes, Unix Timestamps, And Other Date Formatting Tricks In Redshift \- Sisense Support Knowledge Base](https://support.sisense.com/kb/en/article/manipulating-dates-datetimes-unix-timestamps-and-other-date-formatting-tricks-in-redshift)
