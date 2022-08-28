## 概要

moduleの単体テストを書く際、テスト用にクラスをオープンして対象のmoduleをinclude or extendすることでmoduleに定義したメソッドをテストする。

## サンプル

```rb
# moduleの定義
module TestModule
  def test
    'test'
  end
end
```

```rb
# rspec

describe TestModule do
  context '特異メソッドとして使う場合' do
    let(:test_class) { Class.new { extend TestModule } }

    it { expext(test_class.test).to eq 'test' }
  end

  context 'インスタンスメソッドとして使う場合' do
    let(:test_class) { Class.new { include TestModule } }

    it { expect(test_class.new.test).to eq 'test' }
  end
end
```
