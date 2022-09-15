## 概要

`eager_load`で指定したassociation名は`eager_load_values`に持っており、この値を参照することでクエリをキャッシュしている。



## サンプル

以下のようなモデルがある場合。

```rb
class User < ApplicationRecord
  has_many :articles
end

class Article < ApplicationRecord
  belongs_to :user
end
```

```rb
User.eager_load(:articles).eager_load_values
=> [:articles]
```

### 注意点

以下のようにassociationにscopeを適用してる場合とそうでない場合の2つがある場合、eager_loadで指定したassociation名で参照しないとキャッシュできずN+1となってしまう。

```rb
class User < ApplicationRecord
  has_many :articles
  has_many :published_articles, -> { where(published: true) }, class_name: 'Article'
end

class Article < ApplicationRecord
  belongs_to :user
end
```

```rb
User.eager_load(:published_articles).eager_load_values
=> [:published_articles]

users = User.eager_load(:published_articles)
# N+1にならない
users.each { |user| user.published_articles }
# eager_loadしたassociation名と違うのでN+1になる
users.each { |user| user.articles }
```

## 参考

- [ActiveRecord::QueryMethods\#eager\_loadがeager loadingを行う仕組み \- Qiita](https://qiita.com/k0kubun/items/b4730dd79420bb342f17)
- https://github.com/rails/rails/blob/00676252a65900bbf9c9f08abbcafbaa71bfd390/activerecord/lib/active_record/relation/query_methods.rb#L206-L220
