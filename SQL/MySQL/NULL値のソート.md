## 概要

ORDER BYによるソート時にNULL値を先頭にするか最後にするか指定する方法。

何も指定がなければNULLは先頭になり、`IS NULL ASC`を指定するとNULLが最後になる。

`IS NULL`を使うと、対象のカラムがNULLなら1(true)、それ以外なら0(false)が得られるため、ASCを指定することでNULLが最後になる。

> MySQL では、0 や NULL は false を意味し、それ以外はすべて true を意味します。 ブール演算のデフォルトの真理値は 1 です。

## サンプル

以下のようなサンプルテーブルがあるとする。

```sql
CREATE TABLE `users` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `age` varchar(255),
  PRIMARY KEY (`id`)
);
```

ageカラムの昇順でソートし、NULLは最後にしたい場合

```sql
SELECT *
FROM users
ORDER BY age IS NULL ASC, age ASC;
```

## 参考

- [MySQL :: MySQL 8\.0 リファレンスマニュアル :: 3\.3\.4\.6 NULL 値の操作](https://dev.mysql.com/doc/refman/8.0/ja/working-with-null.html)
