## 概要

Request Specでリダレクト先のFlashメッセージをテストする。


## サンプル

```ruby
expect(flash[:alert]).to eq('Alert!')

expect(flash[:notice]).to eq('Updated.')
```

## 参考

- [RSpec: How to test the content of a flash message in a request spec \- makandra dev](https://makandracards.com/makandra/495974-rspec-how-to-test-the-content-of-a-flash-message-in-a-request-spec)
