## 概要

system specでCKEditorへの入力がうまくいかずFlakyになっていた際に用意したヘルパー。

## サンプル

下記のようなヘルパーを用意した。

```rb
# spec/support/ckeditor_helper.rb

module CkeditorHelper
  def fill_in_ckeditor(locator, opts)
    content = opts.fetch(:with).to_json
    page.execute_script <<-SCRIPT
      CKEDITOR.instances['#{locator}'].setData(#{content});
    SCRIPT
  end

  def wait_for_ckeditor(locator)
    Timeout.timeout(Capybara.default_max_wait_time) do
      loop until page.evaluate_script <<-SCRIPT
        CKEDITOR.instances['#{locator}'].status === 'ready';
      SCRIPT
    end
  end

  RSpec.configure do |config|
    config.include CkeditorHelper, type: :system
  end
end
```

- fill_in_ckeditorメソッド
  - 指定したCKEditor上のlocatorにwithの引数で渡した値をスクリプトで入力する
- wait_for_ckeditorメソッド
  - 上記の入力の際、指定のlocatorが描画・入力準備完了になるまで時間がかかると入力ができずFlakyになるためlocatorの描画を待つ
  - 待ち時間の最長は`Capybara.default_max_wait_time`が設定されていればそれを使うとテストコード内でタイムアウト時間の一貫性が保てる

テストコードで使う例。

```rb
# system spec内
require 'support/ckeditor_helper'

# 省略
wait_for_ckeditor(:target_locator)
fill_in_ckeditor :target_locator, with: '入力値のテスト'
# 省略
```

## 参考

[How to fill ckeditor from capybara with webkit or selenium driver](https://stackoverflow.com/questions/10957869/how-to-fill-ckeditor-from-capybara-with-webkit-or-selenium-driver)
