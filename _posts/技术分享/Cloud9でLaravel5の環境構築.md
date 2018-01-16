---
title: Cloud9でLaravel5の環境構築
categories:
  - 技术分享
date: 2017-02-03 04:58:48
tags: [Cloud9]
thumbnail: https://lh3.googleusercontent.com/-SmYaIiM-R1w/WJQOzBxyyrI/AAAAAAAAAzU/-kZytHD_DqY/s0/2017-02-03_14-02-02.png
---
<!--excerpt-->

## プロジェクト作成

![](https://lh3.googleusercontent.com/-SXJRijcQ4F4/WJQPLCrNGrI/AAAAAAAAAzY/liQI8sZCmkU/s0/2017-02-03_14-03-41.png)
`Workspace`名とテンプレートに`PHP`を選択します。`Hosted workspace`は`Private`でも`Public`でもどちらも構いませんが`Public`の場合はすべての人に公開されてしまいます。またフリー版では`Private`の`Workspace`は１つだけとなっています。

## `Laravel5`のプロジェクト作成

新しいタブでターミナルを開いてください。
以下のコマンドを実行してプロジェクトを作成してください。

```
rm README.md php.ini hello-world.php
sudo composer self-update
composer create-project laravel/laravel ./laravel --prefer-dist
shopt -s dotglob
mv laravel/* ./
rm -rf laravel

```
完了まで数分かかかります。

## Apacheのconf設定

以下のファイルを`vi`で開いて、`DocumentRoot`を以下に変更してください。

```
sudo vim /etc/apache2/sites-enabled/001-cloud9.conf
```

編集箇所

```
DocumentRoot /home/ubuntu/workspace/public
```

## アップデート 

```
composer update
```

## DB設定

プロジェクトを作成した際に`MySQL`の`DB`が同時に作成されています。
まずは下記コマンドを実行して接続先のホスト名を確認しましょう。

```
mysql-ctl cli
use c9;
select @@hostname;　　←ホスト名が表示される
exit
```
## `Laravel`の`DB`接続設定 

 ワークスペースの直下に`「.env」`ファイルがありますが、エディタのエクスプローラーからは見えないです。（設定で表示可能かもしれませんが分かりませんでした。）ターミナルからファイルをvimで開いて編集してください。

```
DB_HOST=HOSTNAME　　←上で確認したホスト名をセット
DB_DATABASE=c9　　　  ←初期値
DB_USERNAME=USERNAME　←cloud9のユーザー名
DB_PASSWORD=　　　　　　←初期値は空でOK
```

## 起動確認

ひと通り設定は完了したので、`Apache`を起動して確認してみましょう。
`Run`ボタンをクリックして画面下部に表示されたurlをクリックすると初期画面が表示されます。下記のように表示されていれば完了です。（`Laravel5`の場合）

![](https://lh3.googleusercontent.com/-iYRTE3BEO3o/WJQQf8428QI/AAAAAAAAAzo/FSuSCDBxU50/s0/2017-02-03_14-09-19.png)

手軽に環境を作成したいと思いましたが、テンプレートが用意されていないとまだまだ手間がかかります。ここはもう少し改善を期待したいです。


また、`Laravel`のプロジェクトとしてアプリを利用するためには、権限の変更などまだいくつか設定が 必要になりますが、そこは下記リンクや他で色々と紹介されていますので参照してみてください。