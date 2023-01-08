## 概要

`bundle update`で任意のgemを指定した際、直接依存しないgemもupdateされることがあるので、`--conservative`オプションを指定すると直接依存のあるgemのみをアップデートする。

## サンプル

```sh
bundle update rails --conservative
```


## 参考

- [bundle update](https://bundler.io/v2.4/man/bundle-update.1.html)
