## bashの設定（エイリアス）

### エイリアスを設定する

`alias ls='ls -F'`のようにすると、lsコマンドを入力したときに`ls -F`の挙動をするようにできる

### エイリアスかどうか確かめる

このとき、`type ls`と入力すると、`ls is aliased to 'ls -F'`というようにメッセージが返ってくる。

aliasでない生のコマンドに`type cp`のようにしてあげると、`cp is hashed (/usr/bin/cp)というふうにフルパスが返される。

### エイリアスを削除
`unalias ls`

### エイリアスを一時的に無効に

- `/bin/ls`のようにフルパスで指定してあげる
- `command ls`
- `\ls`

上記はどれも同じ挙動。
