## 概要

ruby-vips gemを使ってPDFのページ数を取得する方法。


## サンプル

```rb
Vips::Image.pdfload(pdf_path).get("pdf-n_pages")
```
