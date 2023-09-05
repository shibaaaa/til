## 概要

Railsコンソール場でDBの接続情報を表示する方法。

## サンプル

```rb
# database.ymlの内容を表示
Rails.application.config.database_configuration

# 現在の環境の設定のみを表示

Rails.application.config.database_configuration[Rails.env]
```
