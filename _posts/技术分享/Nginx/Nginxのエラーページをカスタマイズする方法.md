---
title: Nginxのエラーページをカスタマイズする方法
date: 2017-01-19 08:06:00
tags: [nginx]
keywords: [nginx]
categories: 
- 技术分享
- Nginx
thumbnail: https://3.bp.blogspot.com/-GTMV1FZM2Gc/WXxmfIiqmcI/AAAAAAAADGw/waxjX4NS9C4xF2-rnTU4ZU8fnSjvqT1MACKgBGAs/s1600/Nginx-server-compile-list.jpg
---

>Nginxのデフォルトのエラーページは格好良くないし、Nginxを使ってることがバレバレです。それが嫌だったので以下の方法でカスタマイズしました。

## 基本

>Nginxではエラーページのカスタマイズにはerror_pageディレクティブを使います。
>カスタマイズの基本的な方法は、custom_404.htmlを作って、nginxの設定ファイルを以下の様な感じにします。
>これで、Nginxを再起動して、存在しないファイルへリクエストするとcustom_404.htmlが帰ってくるようになります。

```
server {

    # 中略

    error_page 404 /custom_404.html;
    location = /custom_404.html {
        root /opt/nginx/html;
        internal;
    }

    # 中略
}
```

## 応用

>以下では、よりより実践的な例を紹介します。

>IP直打ちでのアクセスは全てエラーページを表示

>IPアドレスを直接入力してアクセスしてきた場合や想定外のホスト名でアクセスしてきた場合に常に``custom_404.html``を表示したければ以下の様にします。

```
server {
    listen 80 default_server;
    error_page 404 /custom_404.html;
    location / {
        return 404;
    }
    location = /custom_404.html {
        root /opt/nginx/html;
        internal;
    }
}
```

どんなエラーが発生しても404ページを返す

実際に発生しているエラーが403だろうが503だろうが、ユーザーには404エラーが発生しているように見せかけます。
下の例では400, 401, 403, 500, 502, 503のいずれが発生してもクライアントに返すHTTPステータスコードを404に書き換え、custom_404.htmlを返します。

```
server {
    listen 80;
    server_name example.com;
    error_page 400 401 403 404 500 502 503 =404 /custom_404.html;
    location = /custom_404.html {
        root /opt/nginx/html;
        internal;
    }
}
```

管理画面を隠す

特定のIP以外から管理画面へアクセスされた時は404画面を出します。
この例では``192.168.0.1``からの管理画面へのリクエストは許可し、それ以外の場合は404ステータスとcustom_404.htmlを返します。