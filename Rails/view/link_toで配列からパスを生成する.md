## 概要

`link_to`の第2引数に配列を渡すこともできる。

配列の最初の要素によってパスのname spaceを分けたり、任意のアクションを指定することができる。

パラメータを渡したい場合は配列の最後の要素にハッシュで渡す。

## サンプル

```ruby
<%= link_to 'Sample Path', [:sample, @hoge, { fuga: :foo }] %>
# => sample/hoge/:id?fuga=foo
```


## 参考

- [Rails のルーティング \- Railsガイド](https://railsguides.jp/routing.html#%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%8B%E3%82%89%E3%83%91%E3%82%B9%E3%81%A8url%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)
- [Rails link\_to/form\_for/etc\. with array for path and URL query parameters \- Stack Overflow](https://stackoverflow.com/questions/43419563/rails-link-to-form-for-etc-with-array-for-path-and-url-query-parameters)
