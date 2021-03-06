## （オプション）管理画面上に登録したバンドルバージョンに応じた処理の振り分け

F.O.X の管理画面上に登録しているバージョンと実行中のアプリのバージョンとが一致するかどうかに応じて処理を振り分けることができます。本機能を利用することで、例えばリリース中とテスト中のバージョンの処理の振り分けをF.O.Xを利用して制御し、テスト中のみ特定の処理を実行するなどが可能となります。
本機能の実装は必須ではありません。

なお、F.O.X で利用するバンドルバージョンとは、CFBundleShortVersionString に相当する値です。


バンドルバージョンに応じた処理の振り分けを行うには、checkVersionWithDelegate()メソッドを利用します。バージ ョンチェックの問い合わせを F.O.X サーバーに対して行い、結果を受信すると didLoadVersion()がコールされます。デ リゲートメソッド didLoadVersion()を実装してください。didLoadVersion()のコール時に、管理画面上に登録したバンドルバージョンが実行中のアプリのバージョンと一致するかどうかの判定結果を引数に渡します。一致した場合には”YES”、一致しなかった場合には”NO”を返します。


以下、本機能の実装例です。
checkVersionWithDelegate()メソッドをコールし、サーバーに問い合わせを行います。

```cs
FoxPlugin.setListenerGameObject(this.gameObject.name);FoxPlugin.checkVersionWithDelegate();
```

デリゲートメソッドであるdidLoadVersion()を実装します。

```cs
public void didLoadVersion(string result){	// 一致しなかった場合(例えばテスト中のバージョン)の処理の記述。
	if (result=="NO") {		....	}}
```

>本メソッドによる F.O.X サーバーへの問い合わせは、負荷軽減のため1クライアントにおいて各 バージョンごとに5回までに制限されます。5 回を超えるとサーバーへの問い合わせは行われず、 didLoadVersion()がコールされません。バンドルバージョンを更新することで再度 5 回を上限 にサーバーへ問い合わせが行われます。

---
[iOS TOP](/lang/ja/doc/integration/ios/README.md)

[TOP](/lang/ja/README.md)
