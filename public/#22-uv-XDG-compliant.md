---
title: 'uvの環境変数のインストール先の変更とXDG準拠'
tags:
  - Python
  - UV
private: false
updated_at: '2025-03-30T17:06:55+09:00'
id: b990b577907ed76d5aec
organization_url_name: null
slide: false
ignorePublish: false
---

# この記事について
私は、Dockerで`uv`を使用してPython環境を構築しようとした際に発生した環境変数についての問題を記事にしたものです。

# `uv`とは
Rust で書かれた、非常に高速な Python パッケージおよびプロジェクト マネージャーです。
Rustで使用されているパッケージ管理システムであるCargoをPythonに導入したもので、`pip`準拠なより高速なインストールを可能としています。

# `uv`のインストール先の変更
以下のような過去の記事を参考にDockerfileを作成し、`uv`を使用したPython環境を構築しました。
```Dockerfile
FROM python:3.12-slim-bookworm

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# Download the latest installer
ADD https://astral.sh/uv/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.cargo/env/:$PATH"

COPY --from=ghcr.io/astral-sh/uv:0.6.10 /uv /uvx /bin/
```

今回このコンテナをビルドすると
```
$ docker compose up --build

 => ERROR [app 6/6] RUN . $HOME/.bashrc && uv sync                                                                                0.2s
------                                                                                                                                 
 > [app 6/6] RUN . $HOME/.bashrc && uv sync:
0.121 /bin/sh: 21: .: cannot open /root/.cargo/env: No such file
------
failed to solve: process "/bin/sh -c . $HOME/.bashrc && uv sync" did not complete successfully: exit code: 2
```
となりました。

以前のリポジトリでは問題なく使用できていることから、`uv`のインストール先について何かしらの変更が加えられている可能性がありました。

するとこのようなPRが見つかりました。

https://github.com/astral-sh/uv/pull/8420

このIssueは、`uv`のインストール先を`.cargo/env`から`.local/bin`に変更されたということが書かれています。これは、`uv`がRustで使用されていた背景があり、インストール先も`.cargo/env`配下になっていました。しかしこのPRではXDGに準拠するためにインストール先を変更したと記述されています。では、このXDGとはなんでしょうか？

# XDGとは
> Various specifications specify files and file formats. This specification defines where these files should be looked for by defining one or more base directories relative to which files should be located.

*XDG Base Directory Specification ~Introduction~[^1]*

XDGは、様々な仕様でファイルやファイル形式が指定されている中で、ファイルの配置場所の基準として定義されたものです。`dotfile`など、設定ファイルとかデータなどの保存場所がアプリケーション毎にバラバラだと、`$home`などディレクトリが汚染されてしまいます。今回の環境変数については、このXDGに準拠するために変更されたものです。

https://github.com/astral-sh/uv/pull/2236#issue-2171078128

この話のきっかけとしては、ユーザがRustを使用していないために`.cargo`ではない別の空間に保存する必要があるということで始まりました。使用しているマシンによって異なるディレクトリを指定すると混乱を生じさせるため、変更が行われました。




[^1]:[XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/latest/#introduction)