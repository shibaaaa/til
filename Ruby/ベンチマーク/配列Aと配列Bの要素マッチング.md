## 概要

配列Aの要素から配列Bの要素と、値を比較してマッチングする要素を抽出する処理を書きたい。

マッチする組み合わせはユニークで欲しい場合に、マッチ済みかどうかを判定する方法を以下2パターン検討し、ベンチマークを取ってみた。
(単純にdelete_atのベンチマークも気になったので)
- Hashでマッチ済みのものを持っておき、値の比較時にこのHashに値がないことを見る
- マッチ済みの要素は配列から削除しておき、そもそもマッチ候補に上がらないようにする

Ruby 3.3.6で検証。

## 検証コード

```rb
# maching_benchmark.rb
require 'benchmark'

class Hoge
  attr_reader :id, :name, :age

  def initialize(id:, name:, age:)
    @id = id
    @name = name
    @age = age
  end
end

def delete_at_ver
  ints = (1..10000).to_a
  hoges_1 = ints.map { |i| Hoge.new(id: i, name: "name_#{i}", age: i) }
  hoges_2 = ints.shuffle.map { |i| Hoge.new(id: i, name: "name_#{i}", age: i) }

  hoge_dup = hoges_1.dup
  matched_arry = []
  hoges_2.each do |hoge|
    idx = nil
    matched_hoge = nil
    hoge_dup.each_with_index do |h, i|
      if h.id == hoge.id
        idx = i
        matched_hoge = h
        break
      end
    end

    if matched_hoge
      matched_arry << matched_hoge
      hoge_dup.delete_at(idx)
    end
  end

  matched_arry
end

def mached_hash_ver(hoges_1, hoges_2)
  ints = (1..10000).to_a
  hoges_1 = ints.map { |i| Hoge.new(id: i, name: "name_#{i}", age: i) }
  hoges_2 = ints.shuffle.map { |i| Hoge.new(id: i, name: "name_#{i}", age: i) }

  hoge_dup = hoges_1.dup
  matched_hoge = {}
  matched_arry = []
  hoges_2.each do |hoge|
    hoge_dup.each do |h|
      if h.id == h.id && !matched_hoge[h.id]
        matched_hoge[h.id] = hoge
        matched_arry << h
      end
    end
  end

  matched_arry
end

Benchmark.bm do |x|
  x.report('delete_at使う') { delete_at_ver }
  x.report('hashでマッチを管理') { mached_hash_ver }
end
```

## 結果

```sh
%ruby maching_benchmark.rb
       user     system      total        real
delete_at使う  1.526164   0.002770   1.528934 (  1.529047)
hashでマッチを管理  2.981097   0.005135   2.986232 (  2.986266)
ruby maching_benchmark.rb  4.56s user 0.04s system 98% cpu 4.687 total
```

delete_atメソッドで要素を削除する方が速かった。

可読性はHashでマッチ結果を保持する方が良い印象。

## 参考

- [Ruby でベンチマークを取る方法 \#Ruby \- Qiita](https://qiita.com/scivola/items/c5b2aeaf7d67a9ef310a)
