## 概要

CSVをBOM付きで出力することでExcelで開く際にUTF-8で開くことができ、文字化けを防ぐ。

## サンプル

BOMは`"\uFEFF"`なので`CSV.generate`の返り値に付与し、それを`File.open`に渡すことで出力できる。

Railsでダウンロードなどで使う場合は`send_data`メソッドの引数にそのまま渡す。

```rb
require "csv"

text = <<~CSV
id,name
1,sato
CSV

csv = CSV.generate(text, headers: true, encoding: Encoding::UTF_8) do |csv|
  csv.add_row(["2", "suzuki"])
end

csv_with_bom = "\uFEFF" + csv
#=> "id,name\n1,sato\n2,suzuki\n"

# File.openで出力

File.open("with_bom.csv","w") do |file|
  file.write(csv_with_bom)
end

# Railsのコントローラでダウンロードに使う場合

send_date csv_with_bom, file_name: with_bom.csv, type: :csv
```



