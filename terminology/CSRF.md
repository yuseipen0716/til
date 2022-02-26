## CSRF(クロスサイト・リクエスト・フォージェリ)

> ログイン機能を設けているサイトにおいて、ログインした利用者からのリクエストについて、その利用者が意図したリクエストであるかどうかを識別する仕組みを持たないウェブサイトは、
外部サイトを経由した悪意のあるリクエストを受け入れてしまう可能性があります。  (引用: IPA 安全なウェブサイトの作り方 -1.6 CSRF)

こんな感じのサイトにログインしたユーザーが悪意のある罠にかかって意図しない処理がなされてしまう問題をCSRF（クロスサイト・リクエスト・フォージェリ）の脆弱性と呼ばれる。

### どんな問題が？
- ユーザーがログインした後で、ログインユーザーのみが利用できるサービスの悪用
- ログインユーザーのみが編集可能な情報の改ざんや新規登録等

### 注意が必要なウェブサイト
- Cookieを用いたセッション管理
- Basic認証
- SSLクライアント認証
- （ログイン機能をもつサイト）
