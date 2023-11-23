## 概要

ActiveRecordのorメソッドを使って複数条件のクエリを組み立てる際、mergeメソッドを使う場合と使わない場合の挙動が異なる。

## サンプル

下記のモデルをサンプルとして使う。
scopeとしてorのクエリが存在し、このscopeと他の条件を組み合わせた際のSQLを見ていく。

```rb
class User < ApplicationRecord
  scope :sample_or_condition, -> {
    where(email: 'hoge@example.com').or(User.where(name: 'fuga'))
  }
end
```

上記のscopeのみを呼び出すと下記のSQLとなる。

```io
User.sample_or_condition.to_sql
=> "SELECT \"users\".* FROM \"users\" WHERE (\"users\".\"email\" = 'hoge@example.com' OR \"users\".\"name\" = 'fuga')"
```

### mergeを使わない場合

`(created_atの条件 AND emailの条件) or nameの条件`という絞り込みとなる。

```io
User.where(created_at: Time.current).sample_or_condition.to_sql
=> "SELECT \"users\".* FROM \"users\" WHERE (\"users\".\"created_at\" = '2023-11-23 01:55:06.247990' AND \"users\".\"email\" = 'hoge@example.com' OR \"users\".\"name\" = 'fuga')"
```

### mergeを使う場合

`created_atの条件 AND (emailの条件 or nameの条件)`という絞り込みとなる。

```io
User.where(created_at: Time.current).merge(User.sample_or_condition).to_sql
=> "SELECT \"users\".* FROM \"users\" WHERE \"users\".\"created_at\" = '2023-11-23 01:55:35.477641' AND (\"users\".\"email\" = 'hoge@example.com' OR \"users\".\"name\" = 'fuga')"
```

ほとんどの場合はこちらのmergeを使うケースのSQLを想定していることが多いと思うので、orメソッドの呼び出しを他の絞り込みメソッドと同時に呼び出す際には気を付ける。


