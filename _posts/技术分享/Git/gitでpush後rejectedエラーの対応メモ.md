---
title: GitHub初心者の僕が、初めてGitHubリポジトリにpushしたら、rejectedエラーになったので、ちゃんとpushできるようになるまでの対応をメモしました。
categories:
- 技术分享
- Git
date: 2017-02-09 01:35:00
tags: [Git,GitHub]
thumbnail: https://lh3.googleusercontent.com/-nnwlkrohkHo/WJvpWp8lRGI/AAAAAAAABcw/bl95WI6WWwI/s0/2017-02-09_13-00-25.png
---
<!--excerpt-->

## はじめに
-----------------------------
GitHub初心者の僕が、初めてGitHubリポジトリにpushしたら、rejectedエラーになったので、ちゃんとpushできるようになるまでの対応をメモしました。
## Gitバージョン

```
% git --version
git version 2.9.2
```
## 作業の流れ
GitHubからリポジトリを作成

「Initialize this repository with a README」にチェック

ローカルリポジトリに追加
```
% git add -A
```

ロカールリポジトリにコミット
```
% git commit -m "xxxxx"
```
リモートリポジトリ（GitHub）の情報を追加
```
% git remote add origin https://github.com/xxxx/xxxxx.git
```
ローカルリポジトリをリモートリポジトリへ反映させる → ``rejectedエラー``

```
% git push origin master
To https://github.com/xxxx/xxxxx.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/xxxx/xxxxx.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

ググってみたら「git fetch && git merge origin/master」してからpushすればOKと書いてあったので試してみる → ``mergeエラー``
```
% git fetch && git merge origin/master
fatal: refusing to merge unrelated histories
```
さらにググってみたらmergeのオプションに「--allow-unrelated-histories」をつければOKと書いてあったので試してみる → ``merge OK!``

```
% git merge --allow-unrelated-histories origin/master
Merge made by the 'recursive' strategy.
 README.md | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 README.md
 ```
 
再度、ローカルリポジトリをリモートリポジトリへ反映させる → ``push OK!``

一件落着

## 「--allow-unrelated-histories」について
----------------------------------------
ちゃんと調べてみたところ
Git 2.9から mergeコマンドとpullコマンドでは，--allow-unrelated-historiesを指定しない限り，無関係なヒストリを持つ２つのブランチをマージすることはできなくなった。
とありました。

## 考察
----------------------------------------
まだ、GitHub、Gitの理解が浅いので見当違いなことを言っているかもしれませんが、GitHub上でリポジトリを作成したタイミングで、README.mdがコミットされているのに、それをローカルリポジトリに取り込まずに、ローカルのソースをコミットしてしまったのが原因と思われる。
結果、それぞれが別ヒストリとなり、mergeエラーになったのかなと。