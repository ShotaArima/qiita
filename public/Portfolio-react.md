---
title: Dockerを用いたReactポートフォリオサイトの構築(Github Pages)
tags:
  - Docker
  - ポートフォリオ
  - React
  - GitHubPages
private: false
updated_at: '2025-06-19T21:13:51+09:00'
id: 5fc728012c753e7fe245
organization_url_name: null
slide: false
ignorePublish: false
---

## 対象者
- これからポートフォリオサイトを作ろうとしている方
  - 特にテック企業を目指している就活前の方など
- React, Dockerを勉強・使用したい方

# 背景
就職活動に向けて、自分の実績をまとめたポートフォリオサイトを作成しています。
今回は、開発環境をローカルに直接インストールせず、Docker を活用して構築することにしました。手元で変更をすぐに確認できる環境を整えるのが目的です。

React アプリの Docker 環境構築や、GitHub Pages へのデプロイに関する記事はそれぞれ存在しますが、両者を一貫して解説する記事は意外と少ないと感じました。  

そこで本記事では、「Docker 上で React アプリを開発し、それを GitHub Pages にデプロイする」までの一連の手順を、再現しやすく丁寧にまとめていきます。

## 参考記事
- [GitHub Pages でReact Appを公開する方法](https://note.com/wecken/n/n73196eb22a51#e234cb9f-37e0-4ffb-a438-19f1730ce70b)
- [Reactの基礎【環境構築編】](https://zenn.dev/web_tips/articles/abad1a544f3643)

# 開発
## 作業環境
- MacBook Air M2
- Docker v28.2.2
- Docker Desktop (Mac) v4.42.0
    - windowsの方はWindows版のDockerをインストールしてください
## 1. 事前準備
- Dockerのインストール
    - [Windows に Docker Desktop をインストール](https://docs.docker.jp/desktop/install/windows-install.html)
    - [Docker を使ってみる](https://www.docker.com/ja-jp/get-started/)
- Githubのリポジトリ作成

## 2. Reactプロジェクトの生成
- 以下のものをルートディレクトリに作成して下さい
    - ポートフォリオサイトのソースコードを格納するディレクトリを作成
        - 今回は`my-resume-site`と命名します
    - `compose.yaml`というファイルを作成
        - 以下のコードを記述してください
        ```yaml
        services:
        resume-app:
            image: node:24-bookworm-slim
            volumes:
            - ./:/app
            stdin_open: true
            tty: true
            working_dir: /app/
        ```

- ターミナルからこの`compose.yaml`を元に、コンテナを起動します
```bash
$ docker compose up -d
```

- このコンテナに入り、Reactのプロジェクトを生成します
```bash
$ docker compose exec resume-app bash # コンテナ侵入
---
$ npx create-react-app my-resume cd my-resume # JavaScriptで構築
$ npx create-react-app my-resume --template typescript # typescriptで構築
```

- `my-resume-site`の中に以下のようなフィアルが作成されているか確認
```bash
my-resume-site
├── build/
├── node_modules/                  
├── package-lock.json
├── package.json
├── src
│   └── App.js etc...
├── public
│   └── index.html etc...
└── README.md
```

- 上記のファイル群があるかどうかを確認後、コンテナから退出
```bash
$ exit # コンテナ内
---
$      # ローカル環境
```

## 3. Dockerコンテナ上でテスト環境を構築
- `compose.yaml`を修正します
    - `image`を`build`に変更し、カレントディレクトリを指定
    - `volumes`の`:` の前にreactのプロジェクト名を指定
        - 今回は`- ./my-resume-site:/app`としている
    - `port`の3000番を開放する
        - これにより、ブラウザで確認することができる
        - `http://localhost:3000`
```yaml
services:
  resume-app:
    build: .
    volumes:
      - ./my-resume-site:/app
    stdin_open: true
    tty: true
    working_dir: /app/
    ports:
    - 3000:3000
```

- Dockerfileを用意し、以下のコードを記述する
```Dockerfile
FROM node:24-bookworm-slim

WORKDIR /app
# 必要なツールをインストール
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

RUN npm install -g create-vite
```

- 再度コンテナを起動する
```bash
$ doker compose build # Dockerfileをもとにビルド
$ docker compose up -d # ビルドしたコンテナイメージを起動する
$ docker compose exec resume-app bash # コンテナに侵入
---
$ npm run build # reactアプリケーションのビルド
$ npm start # reactアプリケーションの起動
```

- 上記の作業が完了すると、ブラウザで確認することができます
    - `http://localhost:3000`
    - 以下のような画面が現れると思います
    ![スクリーンショット 2025-06-15 18 40 05](https://github.com/user-attachments/assets/a9b5d49e-45c9-467b-9ce0-11b10c8d3945)
- 終了時は`Ctrl+C`で終了できます。

- `compose.yaml`を修正し、Docker起動と同時にreactを起動するように設定する
```yaml
services:
  resume-app:
    build: .
    volumes:
      - ./my-resume:/app
    stdin_open: true
    tty: true
    working_dir: /app/
    ports:
    - 3000:3000
    command: sh -c "npm install && npm start" # 追加する
```

## 4. デプロイを行う
- 目標はreactの最初の画面をGitHub Pagesで閲覧できる状態
- package.jsonの修正
    - GitHub PagesでReactアプリをビルドする際に生成するHTML, CSS, JavaScriptのパスを調整する役割がある
```json
{
  "name": "my-resume-site",
  "version": "0.1.0",
  "homepage": "https://username.github.io/Portfolio/", // 追加
}
```

- GitHubのリモートリポジトリの設定

- workflowの構築
```yml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          cache-dependency-path: my-resume/package-lock.json

      - name: Install dependencies
        run: npm ci
        working-directory: ./my-resume-site #自分で命名したディレクトリ名

      - name: Build site
        run: npm run build
        working-directory: ./my-resume-site # 自分で命名したディレクトリ名

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./my-resume-site/build #自分で命名したディレクトリ名
```

- GitHubのリポジトリの設定を行う
    - 「設定」から「Pages」のセクションを開く
    - 「Source」セクションで、**gh-pages**ブランチを公開ソースとして選択し、保存する

## 5. 確認
- 公開されているページを確認する
    - `https://username.github.io/(リポジトリ名)`にアクセスし確認する
    - 変更が反映されるまで少々時間がかかりますので、ご注意ください

## 更新手順について
- ローカルで変更の際にブランチを作成し、変更作業を行います。
- Pull Requestを作成し、mergeしたタイミングでdeployを行う設定にしています。
1. 修正前にお手持ちのリポジトリでブランチを作成する
```bash
$ git  checkout -b "ユニークなブランチ名"
```

2. 変更を行う
- `http:localhost:3000`で変更内容を視覚的に確認する
3. 変更をcommitする
```bash
$ git add ~~
$ git commit -m "commitメッセージ"
$ git push -u origin "ブランチ名" #初回のみ
$ git push #2回目以降
```
4. Pull Requestの作成
- GitHubのリモートリポジトリを開き、「create Pull Request」を選択
- 変更内容を確認
5. Mergeする
- Pull Request内になる「Merge」を選択
- **デプロイ**開始
6. 公開されているページを確認する
- `https://username.github.io/(リポジトリ名)`にアクセスし確認する

## おわりに
- 以上の手順により、ローカル環境(自分のパソコン内)に直接`npm`をinstallせずにreact環境を構築し、GitHub Pagesにデプロイすることができました。今後は、自分でサイトのデザインや実装などを行って行く流れになりますので、自分自身のサイトデザインを作成してみてください！
