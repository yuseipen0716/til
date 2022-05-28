## パフォーマンス向上委員会

個人開発の小規模なプロダクトであれば、それほど影響はないが、レコード数が〇万件のような大きなサービスを扱う場合、
繰り返しの処理の仕方やデータの読み書きの仕方などで処理に掛かる時間が大きく変わることがある。

今回、ちょっとそれを痛感したので自戒の念も含めて、パフォーマンスが少しでも良くなる書き方等をメモしていきたい。

### でかいデータにeachはまずい

レコードが何万件もあるテーブルに対してeachを回そうとすると、データを全てメモリ上に展開することになり、大変。

find_eachやin_batchesなどのメソッドを利用すればメモリ上に展開するデータを絞った状態でループを回すことができる。

#### find_eachメソッド

#### in_batchesメソッド

### たくさんのデータを保存、更新する場合はbulk insert

通常、

```ruby
book_names.each do |book_name|
  book.create(name: book_name)
end
```

のように保存すると1件1件クエリが発行されるようになる。これが数件ならまだいいかもしれないが、100件とか1000件とか
大量になったり、保存先のテーブルのレコード数がとんでもないことになっていたら大変。

bulk insertという方法では1件1件保存していくのではなく、ループ処理の中で新規作成したデータを配列に格納し、配列に
格納されたデータを一気に保存する方法。

この方法だと、INSERTが走るのは1回のみ。

下準備として、activerecord-importのgemを入れないといけないので、

```
gem 'activerecord-import'
```

して`bundle install`しておく

具体的な方法は以下の通り

```ruby
add_books = []
book_names.each do |book_name|
  add_books << book.new(name: book_name)
end
Book.import add_books
```

```ruby
update_books = []
books.each do |book|
  book.name = "hoge"
  update_books << book
end
Book.import update_books, on_duplicate_key_update: [:name]
```

という風に`on_duplicate_key_update`オプションをつけるだけでいいみたい。

PostgreSQLなら関連テーブルもupdateを行ってくれるそうだけど、MySQLだと無理らしい。

関連テーブルも書き換えたいときはどうすればええねんってなったけど、今回はArticlesテーブルと
Categoriesテーブル、その中間テーブルとしてArticleCategoriesテーブルがあったので、
Articleをbulk_insertでたくさん作成して、作成したArticleたちのidをpluckで抜いて、抜いてきたidたちで
eachを回してArticleCategoriesのインスタンスを作成→bulk_insertっていう感じの方法をとった。

#### rails6.0以降

rails 5.2まではbulk_insertするためにactiverecord_importのgemをinstallする必要があったが、6.0からは新しく
`insert_all`, `insert_all!`, `upsert_all`というメソッドが追加されたらしい。

コチラについてはまだ触ったことがないので、rails6.0以降の環境で使ってみよう。

> 参考Rails6.0におけるbulk insert](https://qiita.com/taiteam/items/1b1be0578d1dc6e00a17)

#### bulk_insert仕様上の注意点

update_allとかもそうだけど、こういうまとめて実行する系の方法だと、レコード保存に対するコールバックやバリデーションが発火しないらしい。

たとえば、記事データ(plain)を取得してきて、コールバック処理をかけてリッチなテキストにお化粧してフロントに
表示するような方法で記事を表示しているブログサイトなどで、この方法を使ってしまうとすっぴんのまま表に出ることに
なったり、そもそもshowアクションで拾えず、エラーが出てしまうかもしれない。

これらの心配がないときは、どんどん活用していきたい。また、条件分岐等を工夫して上記の問題点を回避するような実装を
組んでもいいかもしれない。

### 適切にindexを貼る

とんでもなくレコード数の多いテーブルに対して、特定のカラムを参照する処理は結構時間を食うみたい。

頻繁に参照しないといけないカラム等はindexを貼るなどの対策で、劇的に実行速度が変わる場合がある。

それじゃ、全部のカラムにindexを貼ればいいじゃん、って思うけど、indexにも副作用があるみたい。データを保存
する際に、indexを貼る、という作業が増えることにより、書き込み速度が落ちるんだと。

むやみやたらに貼るのはよくないけれど、適切に使っていきたい。

indexについての理解、まだまだなので、この機会によく調べておこう。

## 特定columnのvalueを抜き出す際にはpluckを使う




