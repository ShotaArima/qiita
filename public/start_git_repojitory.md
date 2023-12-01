---
title: GitHubでリポジトリを始める方法
tags:
  - 'Git'
  - '初心者'
private: false
updated_at: ''
id: null
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
    `echo "# (リポジトリ名)"`
  `git clone https://github.com/(自分のGitHubのアカウント名)/(リポジトリ名).git`
  `git config --global --add safe.directory '(ローカルリポジトリのアドレス)`


