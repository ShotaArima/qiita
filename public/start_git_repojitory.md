---
title: GitHubでリポジトリを始める方法
tags:
  - Git
  - 初心者
private: false
updated_at: '2023-12-02T14:39:08+09:00'
id: c09f67d192e19762c2fa
organization_url_name: null
slide: false
ignorePublish: false
---
# 

## ローカルリポジトリにファイルがない場合
### 1.自分のGitHubアカウントを開き、リポジトリの作成を開始する
### 2.「README.md」にチェックを入れ、作成する
### 3.自分のローカルリポジトリを開き、以下のコマンドを実行する
  `git clone https://github.com/(自分のGitHubのアカウント名)/(リポジトリ名).git`
  `git config --global --add safe.directory '(ローカルリポジトリのアドレス)`

## ローカルリポジトリにファイルがある場合
### 1.自分のGitHubアカウントを開き、リポジトリの作成を開始する
### 2.「README.md」にチェックを入れないで作成する
### 3.自分のローカルリポジトリを開き、以下のコマンドを実行する
  - リポジトリの初期化を行う
    `git init`
  - README.mdファイルを作成する
    `echo "# (リポジトリ名)" >> README.md`
  - READMEをaddする
    `git add README.md`
  - READMEをcommitする
    `git commit -m "upload README.md"`
  - mainブランチに切り替える
    `git branch -M main`
  - リモート接続する
    `git remote add origin https://github.com/(アカウント名)/(リポジトリ名)`
  - pushする
    `git push -u origin main`

## 最初のpushが完了した後、コミットする場合
  - addする
    `git add (ファイル名)`
  - commitする
    `git commit -m "(コミットメッセージ)"`
  - pushする
    `git push`

