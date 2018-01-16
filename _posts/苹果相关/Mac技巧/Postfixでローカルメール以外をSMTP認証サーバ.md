---
title: Postfixでローカルメール以外をSMTP認証サーバやGMAILにリレーするための設定を記載します
categories:
- 苹果相关
- Mac技巧
date: 2017-09-05 09:47:40
tags: [Mac技巧,mail,php]
thumbnail: https://lh3.googleusercontent.com/-VaSX6FxYHOQ/Wai3loGBtnI/AAAAAAAADbQ/oyqO2I5bwPkzZpMvCsDU1F7I3AQ86_8uQCHMYCw/s0/2017-09-01_10-27-41.png
---
<!--excerpt-->

リレー先メールサーバがSMTP認証しか受け付けないだとか、TLS/SSLだったりとかで設定がいろいろ変わります

ここではそれぞれのケースに応じた設定を記載していきます

## SMTP認証のない相手へのリレー設定

リレー先メールサーバが特に認証がない場合は設定は簡単です

Postfix設定ファイル(/etc/postfix.main.cf)に以下を設定するだけです
```
relayhost = [SMTPホスト]:ポート番号
relayhost = [IPアドレス]:ポート番号
relayhost = SMTPホスト名:ポート番号
```
ポート番号は25番や587(サブミッションポート)が一般的です
[]で囲むとMX検索しません
Postfixをリロードして設定を有効にします
```
/etc/rc.d/init.d/postfix reload
```
リレー先メールサーバにSMTP Auth(SMTP認証)が必要な場合

## 自身のメールサーバをSMTPクライアントしてリレーサーバにメールをリレーする様に設定します

Postfix設定ファイル(/etc/postfix.main.cf)に以下を設定します
```
relayhost = [SMTPホスト]:ポート番号
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/relay_password
smtp_sasl_security_options = noanonymous
```
* relaygostはリレー先サーバの情報を設定します
* smtp_sasl_password_mapsで、SMTP認証情報を記述したファイルを指定します(後述)
* smtp_sasl_security_optionsの規定値は 「noplaintext, noanonymous」です
続いてSMTP認証情報を記述したファイルを作成します

作成するファイルは、/etc/posfix/relay_passwordです
```
[SMTPホスト名]:ポート番号   SMTP認証ユーザ名:SMTP認証パスワード
   :       :     :
   :       :     :
```
* SMTP認証情報をホスト毎に1行ずつ記載します
* []で囲むとMX検索しません
* /etc/posfix/relay_passwordはパスワードが丸見えなのでパーミッションを変更(600)してrootしか読めないようにしておきましょう
ファイル作成後は、postmapコマンドでデータベース化し、/etc/postfix/relay_password.db を作成します
```
postmap hash:/etc/postfix/relay_password
```
Postfixをリロードして設定を有効にして完了です
```
/etc/rc.d/init.d/postfix reload
```
## GMAILをリレーサーバにする場合

GmailのようなTLS/SSLなメールサーバにリレーする為には少し設定が多くなります

### パッケージのインストール
処理に必要となるcyrusパッケージ群をインストールします
```
yum install cyrus-*
```
### Postfixの設定
Postfix設定ファイル(/etc/postfix.main.cf)に以下を設定します
```
relayhost = [smtp.gmail.com]:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_tls_security_options = noanonymous
smtp_sasl_mechanism_filter = plain
smtp_tls_CApath = /etc/pki/tls/certs/ca-bundle.crt
```
続いてGMAIL認証情報を記述したファイルを作成します

作成するファイルは、/etc/posfix/sasl_passwordです
```
[smtp.gmail.com]:587   GMAILメールアドレス:パスワード
```
ファイル作成後は、postmapコマンドでデータベース化し、/etc/postfix/sasl_password.db を作成します
```
postmap hash:/etc/postfix/relay_password
```
Postfixをリロードして設定を有効にして完了です
```
/etc/rc.d/init.d/postfix reload
```



