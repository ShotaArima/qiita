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

https://github.com/ShotaArima/docker-jupyter

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
以下の図をご覧ください。
![JupyterーDocker drawio](https://github.com/ShotaArima/qiita/assets/130956497/528539af-7d34-4d2a-b3c6-b984015f92e2)
*アイコン提供元[^1]*

今回の実現するものとして、DockerでJupyterの環境を構築し、それを
Dokcerfileには、直接書かず、environment.ymlにライブラリの情報を書きます。ブラウザでJupyterを操作せず、VScodeでの実行ができるようにしています。上記のJupyterのファイルとVScodeのファイルは保存すると同期されるようになっています。
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
続いて今回のコンテナを作成するためのfileを作成します。Dokcer
- Docekrfile
`Dockerfile`という名前のfileを作成してください。
作成したファイルの中に以下のコードを記述してください
```
# ベースイメージの指定
FROM continuumio/miniconda3:latest

# wgetとunzipをインストール
RUN apt-get update && apt-get install -y wget unzip

# environment.ymlをコピーして環境を作成
COPY environment.yml /tmp/
RUN conda env create -f /tmp/environment.yml

# デフォルトでAnaconda環境をアクティブ化
SHELL ["/bin/bash", "-c"]

# 作業ディレクトリを設定
WORKDIR /src/

# matplotlibの設定ファイルをコピー
COPY matplotlibrc /src/.config/matplotlib/

# ポートの公開
EXPOSE 8888

# 日本語フォントの設定
RUN wget -O font.zip "https://moji.or.jp/wp-content/ipafont/IPAexfont/ipaexg00401.zip"
RUN unzip font.zip
RUN cp ipaexg00401/ipaexg.ttf /opt/conda/envs/pymc_env/lib/python3.11/site-packages/matplotlib/mpl-data/fonts/ttf/ipaexg.ttf
RUN echo "font.family : IPAexGothic" >>  /opt/conda/envs/pymc_env/lib/python3.11/site-packages/matplotlib/mpl-data/matplotlibrc
# RUN rm -r ./.cache
```

また、コンテナで使用するライブラリの種類とバージョンは以下のファイルに記述します。
- ライブラリ管理を`conda`で行う場合(今回はこちら)
  `environment.yml`というファイルを書きます。
  ```yml
  name: (任意)
  channels:
    - conda-forge
  dependencies:
    - python
    - numpy
    - pandas
    - matplotlib
    - scipy
    - jupyter
  ```
  バージョンの指定は`python>=3.8`のようにdependenciesタグの中のライブラリについて`=`, `=>`などを使用してください。

- ライブラリ管理を`pip`で行う場合
  `requirment.txt`というファイルを作成します。
  ```txt
  numpy
  pandas
  matplotlib
  scipy
  jupyter
  pymc
  ```

- Docker compose
最後に、compose.yamlを記述します。
```compose.yaml
services:
  jupyter:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ./src:/src
    ports:
      - "8888:8888"
    environment:
      - JUPYTER_ENEBLE_LAB=yes
    command: ["conda", "run", "-n", "pymc_env", "jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--allow-root", "--no-browser", "--NotebookApp.password={$hashedpass}"]
```


## iii. パスワードの作成
ここで、パスワードを作成します。Google Colabでハッシュ化されたパスワードを作成していきます。
```python
from notebook.auth import passwd
passwd()
```
上記のコマンドを実行すると、以下のように`Enter password:`と出力されるので、任意のパスワードを
<img width="420" alt="パスワードハッシュ化入力" src="https://github.com/ShotaArima/qiita/assets/130956497/a98687bd-7b6c-4865-b290-4ce9d3cc69c2">

<img width="805" alt="パスワードハッシュ化出力結果" src="https://github.com/ShotaArima/qiita/assets/130956497/54e1cf40-f708-4b6c-a97b-47117851bc26">

Dokcer composeの`CMD`行にハッシュ値を貼り付けていきます。
```compose.yaml
    command: ["conda", "run", "-n", "pymc_env", "jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--allow-root", "--no-browser", "--NotebookApp.password={$hashedpass}"]
```
の`{$hashedpass}`の部分を先ほどのGoogle Colabで作成したハッシュ化したパスワードに書き換えてください。

## iv. コンテナの起動
```shell
docker compose build # コンテナのビルド
docker compose -t up # コンテナの起動
```
これにより、コンテナを起動することができます。
コンテナのビルド時にエラーが発生する可能性があるので、その際はenvironment.ymlの中に記載しているライブラリのバージョンなど確認し、エラーの解決を行ってください。

起動が行うことができましたら、[http://127.0.0.1:8888](http://127.0.0.1:8888)へアクセスし、Jupyter Notebookがブラウザで起動できれば、分析環境のコンテナの起動は完了です。
次にVScodeでの実行できるように設定を行います。

## v. VScodeの設定
- dockerfileのあるディレクトリをVScodeで開きます。
- `.ipynb`ファイルを作成してください。
- 作成したファイルの右側に`カーネルの選択`と書いているところをクリックします。
- `既存のJupyterサーバー`を選択し、`http://127.0.0.1:8888`と入力してください。
- パスワードが要求されるので、先ほど作成したパスワードを入力してください。
- カーネルを選択してくださいと表示されるので、自分が指定したPython
バージョンを選択してください。

これで、準備完了です。


## vi. 実行確認
最後に、実行確認を行います。
```python
print("Hello World!")
```
を実行し、実行結果が反映されれば問題なく動作しています。
また、各ライブラリが使えるかどうか確認してみてください。


# 5. 終わりに
このリポジトリが分析の研究を行う際の環境構築で悩む方の助けになればと思い作成しました。また、Pythonを扱う上で、ライブラリ管理について触れてもらう機会になればと思います。自分自身もまだまだ未熟者ですが、今後も記事を執筆できればと思います。よろしくお願いします。

https://twitter.com/live_in_2107/

[^1]:[SVG PORN](https://svgporn.com/)