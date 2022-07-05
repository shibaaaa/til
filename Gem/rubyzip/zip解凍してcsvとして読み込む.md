## 概要

rubyzipを使ってzipファイルを解凍し、解凍後のファイルをcsvとして読み込みたい場合。

## サンプル

下記のファイル構成を例にする
- example.zip
  - hoge.csv
  - fuga.csv

```rb
# requireは省略

# zipファイルを解凍してファイル名を取得
file_names = Zip::File.open('example.zip') { |zip_file| zip_file.map(&:name) }
# => ['hoge.csv', 'fuga.csv']
 
csvs = []
Zip::File.open('example.zip') do |zip_file|
　　　　file_names.each do |file_name|
    csvs << CSV.new(zip_file.get_entry(file_name).get_input_stream.read, encoding: Encoding::UTF_8, row_sep: "\n")
  end
end
```

## 参考

- https://github.com/rubyzip/rubyzip
- https://teratail.com/questions/152321#reply-228935
