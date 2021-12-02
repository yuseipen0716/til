## 環境変数について

コマンドには外部コマンドといって、ファイルシステム上に実行ファイルが置かれているもの（cpとかrmみたいなもの)と、builtinの物（cdとかsetみないな）がある。

（これを確認したいときは`type cd`のように入力すれば良い）

*シェル変数は外部コマンドでは参照できない。*

外部コマンドを実行する際に参照したい変数は多くあり、それらを外部コマンドからも参照できるようにした変数のことを**環境変数**という。

現時点でシェルに設定されている環境変数を確認するときは`printenv`コマンドを入力

### 環境変数の設定　export


環境変数は次のようにして設定する。

`export <シェル変数名>`

たとえば、

`PS1=\u`

`export PS1`

のようにすれば環境変数の設定はできるが、これらをまとめて、

`export PS1=\u`

としても環境変数の設定は行える。

