## hamlで後置ifを書いてクラスの出しわけ

``` ruby
%li.navbar-default
```

のようなクラスを書いているときに、条件によって`.navbar-default`を出し分けたい場合に後置ifを使おうと思った。

hamlで後置ifを書くのは初めて、というかできるかわからなかったけど、一応やりかたはあるようだった。

``` ruby
%li{ class: "#{'navbar-default' if foo}" }
```

みたいに書いたらできました。

正直、可読性は下がると思うのでご利用は計画的に。あと、ちゃんとコメント入れた方がいいかも