---
title: Dockerを用いたReactポートフォリオサイトの構築(Github Pages)
tags:
  - 'React'
  - 'Docker'
  - 'Github Pages'
  - 'ポートフォリオ'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# 背景
- 就活用にポートフォリオサイトを作成するため
- ローカルにインストールしたくなかったため、Dockerを使用して進めていた
- 既存の記事は、reactアプリケーションのDocekr環境構築とGithub pagesへのデプロイと独立していたので、その中間をうめていくような記事にする

## 対象者
- これからポートフォリオサイトを作ろうとしている方
  - 特にテック企業を目指している就活前の方など
- reactを勉強したい初心者
- 何か作ってみたいという人

## 参考記事
- [GitHub Pages でReact Appを公開する方法](https://note.com/wecken/n/n73196eb22a51#e234cb9f-37e0-4ffb-a438-19f1730ce70b)
- [Reactの基礎【環境構築編】](https://zenn.dev/web_tips/articles/abad1a544f3643)



# 開発
## 作業環境
- MacBook Air M2
- Docker v28.2.2
- Docker Desktop (Mac) v4.42.0

## 1. 準備
- Dockerのインストール
- Githubのリポジトリ作成
- エディターの用意

## 2. Reactプロジェクトの生成
- compose.yamlでreactのファイルを作成する

## 3. Dockerコンテナ上でテスト環境を構築
- Dockerfileを用意し、プロジェクトを起動する
- ポート開放を行い、ブラウザからアクセスする

## 4. デプロイを行う
- 目標はreactの最初の画面をgtihub pagesで閲覧できる状態
- package.jsonの修正
- github側の設定
- workflowの構築

## 5. 確認
