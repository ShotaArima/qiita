---
# title: jupyter-docker
title: Dockerでローカルに分析環境を整える方法
tags:
  - 'docker'
  - 'python'
  - 'jupyter'
  - 'データ分析'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: True
---
# 1. 概要
このリポジトリは、DockerでJupyter Notebookを起動し、エディタをVScodeで使えるようにしたものです。

<!-- https://github.com/ShotaArima -->

# 2. 対象者、メリット
**対象者**
>- 分析の書籍や研究などで、Pythonのライブラリ環境を設定したい人
>- Google Colabではなく、ローカルで実行したい人
>- ソースコードをgitで簡単に管理したい人

**メリット**
>- ライブラリの設定を自分好みにカスタマイズすることができる
>- 書籍や研究のソースコードの再現する際の雛形として簡単に環境構築できる
>- ローカルに保存されてるgitや補完機能などをそのまま使用できる

# 3. 構成
Dokcerfileには、直接書かず、environment.ymlにライブラリの情報を書きます。ライブラリについて

# 4. 実装

## i. VScode、Dockerのインストール
VScodeとDockerのインストールを行なってください
### VScode
- インストール


- 拡張機能
Jupyter


### Docker
お手元のPCのOSに合わせてインストールを行なってください
**Windows**


**MacOS**


## ii. Dockerイメージの作成
- Docekrfile

- condaの場合
  environment.yml

- 
  requirment.txt

- Docker compose


## iii. パスワードの作成
ここで一時的にPython環境を用意します。
Google Colabを使用することをお勧めします。
```python

```
Dokcer composeの`CMD`行にハッシュ値を貼り付けていきます。

## iv. コンテナの起動
```shell
docker compose build # コンテナのビルド
docker compose -t up # コンテナの起動
```

## v. VScodeの設定

## vi. 実行確認

# 5. 終わりに
このリポジトリが分析の研究を行う際の環境構築で悩む方の助けになればと思い作成しました。また、Pythonを扱う上で、ライブラリ管理について触れてもらう機会になればと思います。自分自身もまだまだ未熟者ですが、今後も記事を執筆できればと思います。よろしくお願いします。

https://twitter.com/live_in_2107/
