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
以下の記事を参考にお手持ちのPCにインストールを行ってください。
https://qiita.com/psychoroid/items/7d85ae6bade4a67aedb1

- 拡張機能
VScodeを開き拡張機能から「Jupyter」を選択し、インストールを行ってください。
<img width="1470" alt="スクリーンショット 2024-05-22 9 29 14" src="https://github.com/ShotaArima/docker-jupyter/assets/130956497/f5db0733-1490-42e9-a7a4-3956e2199933">


### Docker
お手元のPCのOSに合わせてインストールを行なってください
**Windows**
お手持ちのPCの仮想化機能を有効にしてください
これによりDockerを動かすことができます。
- 仮想化が有効かどうかの確認
タスクマネージャーを開き、パフォーマンスタブ内のCPUの欄に有効と書かれれていれば問題ないです。
もし、無効の場合、お手持ちのPCのBIOSに合わせた設定を行ってください。

- dockerのインストール
https://docs.docker.jp/desktop/install/windows-install.html

**MacOS**
- dockerのインストール
https://docs.docker.jp/desktop/install/mac-install.html

#### Dockerの起動の確認
「Hello-world」と呼ばれるコンテナを起動します。
このコンテナは、Dockerを新しくインストールした際の動作確認としてよく用いられます。
まず、Docker Desktopを開いてください。
Windowsの方はコマンドプロンプトをMacの方はターミナルを起動し、以下のコマンドを入力してください。
```shell
docker run hello-world
```
実行すると以下のような出力が行われれば問題ないです。
```shell
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
d1725b59e92d: Pull complete
Digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

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
