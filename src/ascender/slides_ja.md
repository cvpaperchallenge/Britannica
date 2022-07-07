---
layout: cover
download: true
highlighter: shiki
routerMode: hash
fonts:
  mono: 'Fira Mono'
info: |
  ## Ascender

  Accelerator of Scientific Development and Research

  [Yoshihiro Fukuhara](https://gatheluck.net/home/)

  - [Source code](https://github.com/cvpaperchallenge/Ascender)
  - [Slide source code](https://github.com/cvpaperchallenge/Britannica/tree/master/src/ascender/)
---

# Ascender

Accelerator of Scientific Development and Research

<div class="uppercase text-sm tracking-widest">
XCCV Group
</div>

<div class="abs-bl mx-14 my-12 flex">
  <img src="http://xpaperchallenge.org/_nuxt/img/fa53e31.png" class="h-8">
  <div class="ml-3 flex flex-col text-left">
    <div>cvpaper.challenge</div>
    <div class="text-sm opacity-50">Jul. 7th, 2022</div>
  </div>
</div>

---
name: 'content'
layout: 'default'
---

## Content

<div class="text-xl">

- What is Ascender?

- Create repo from Ascender

- Directory structure

- How to use Ascender

- Technical stack of Ascender

- Upcoming feature

</div>

---
name: 'what is ascender?'
---

## What is Ascender?

<br>

<div>

- 主にPythonを開発言語として使用した研究プロジェクト向けの[GitHubのリポジトリテンプレート](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository).

</div>
<div>

- 開発時に有用な機能を多数サポート:

  <div>

  - コンテナ仮想化: [Docker](https://www.docker.com/)を使用することで開発環境依存を減らし, コードの移植性を向上.
  - 仮想環境構築 / パッケージ管理: [Poetry](https://python-poetry.org/)を使用したパッケージ管理により, 同一環境の再現性を向上.
  - コードスタイル: [Black](https://github.com/psf/black), [Flake8](https://github.com/pycqa/flake8), [isort](https://github.com/PyCQA/isort)を使用してコードスタイルを統一.
  - 静的型チェック: [Mypy](https://github.com/python/mypy)による静的型チェックによりコードの信頼性を向上.
  - テスト: [pytest](https://github.com/pytest-dev/pytest/)を使用したテストコードを簡単に追加可能.
  - GitHub関連機能: [Pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)時のスタイル確認・テストの[workflow](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)や[issueテンプレート](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)等を実装済.

  </div>
</div>
<div class="pt-2">

- https://github.com/cvpaperchallenge/Ascender からアクセス可能.

</div>

---
name: 'create repo from ascender'
layout: image-right
image: /ascender-github-root-use-this-template.png
---

## Create repo from Ascender

以下の手順でAscenderをテンプレートとしてGitHubリポジトリが作成可能.

- [AscenderのGitHubリポジトリ](https://github.com/cvpaperchallenge/Ascender)にアクセス.
- "Use this template"ボタンをクリック.
- 作成するリポジトリの情報を入力し, "Create repository from template"をクリック.

テンプレートからリポジトリを作成する方法は[GitHubのドキュメント](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)にも説明がある.

---
name: 'directory structure'
---

## Directory structure

`src`ディレクトリ下にパッケージの形でコードを追加していく構成.

```shell {all}
$ tree -L 1 --dirsfirst  # カレントディレクトリ下のディレクトリとファイルを, 1階層の深さだけ, ディレクトリから先に表示するコマンド.

.
├── data           # <- データセットを格納するためのディレクトリ. 
├── environments   # <- Docker関連のファイルを格納するディレクトリ.
├── models         # <- 学習済みのMLモデル等を格納するためのディレクトリ.
├── notebooks      # <- Jupyter Notebookを格納するためのディレクトリ. 
├── outputs        # <- コード実行時の出力を格納するためのディレクトリ. 
├── src            # <- メインの開発物を格納するディレクトリ. このディレクトリ下がPythonパッケージとなることを想定.
├── tests          # <- pytestのテストを格納するディレクトリ. 全てのテストはこのディレクトリ下に追加.
├── LICENSE        # <- ライセンス情報を記載するファイル.
├── Makefile       # <- タスクランナーとして使用しているMakeの設定ファイル. 頻出のタスクを短いコマンドで実行可能.
├── poetry.lock    # <- Poetryが自動変更するファイル. IMPORTANT: 直接編集しないで下さい!
├── pyproject.toml # <- Projectの設定を記載するファイル. Poetry, Black, Flake8, isort, Mypy等の設定が記載.
└── README.md      # <- Ascenderについての基本的な説明が記載されているファイル.
```

---
name: 'how to use ascender: start development'
---

## How to use Ascender

<br>

### Start development

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all}
# cpu環境の場合は`cd environments/cpu`
$ cd environments/gpu

# Dockerfileからイメージをビルドし, 
# ビルドされたイメージからコンテナを作成. 
# dオプションをつけるとコンテナをバックグラウンドで実行.
$ sudo docker compose up -d

# コンテナ内でbashを実行(コンテナ内に入る).
$ sudo docker compose exec core bash

# Poetryで仮想環境を構築し, 依存パッケージをインストール.
$ poetry install
```

</div>
<div class="text-base">

- 初回の実行時は`sudo docker compose up -d`に時間がかかる.（環境によっては`docker`の実行に`sudo`が必要ない場合がある.）

- `sudo docker compose up -d`によってAscenderのルートディレクトリ以下がコンテナ内のワーキングディレクトリにマウントされる.

- `poetry install`は初回のみ実行すれば良い.（Poetryの使い方については[別スライド](https://cvpaperchallenge.github.io/Britannica/poetry101/ja)に記載.）

</div>
</div>

---
name: 'how to use ascender: during development'
---

## How to use Ascender

<br>

### During development

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all}
# Poetryを使用して新しいパッケージを仮想環境にインストール.
$ poetry add torch

# 仮想環境内でコマンドを実行するときは`poetry run`を使用. 
# 例. 仮想環境内でPythonのインタラクティブシェルを起動.
$ poetry run python3

# Makefile内に頻出するタスクの短縮コマンドを定義.
# `src`と`test`下のファイルのコードスタイルを自動整形.
$ make format

# `src`と`test`下のファイルのコードスタイルチェックと
# 静的型チェックを行い, pytestでテストコードを実行.
$ make test-all
```

</div>
<div class="text-base">

- 開発時はコンテナ内にPoetryが作成した仮想環境を使用.（Poetryの使い方については[別スライド](https://cvpaperchallenge.github.io/Britannica/poetry101/ja)に記載.）

- Makeをタスクランナーとして使用し, 頻出するタスクを`make`コマンドで簡単に実行可能.

- GitHubでPull Requestを作成時にスタイルチェック, 静的型チェック, テストコード実行を行うCI用のworkflowも実装済.

</div>
</div>

---
name: 'how to use ascender: stop development'
---

## How to use Ascender

<br>

### Stop development

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all}
# cpu環境の場合は`cd environments/cpu`
$ cd environments/gpu

# コンテナを停止.
$ sudo dokcer compose stop

# もしコンテナを削除したい場合は停止と削除を一度に行うことも可能.
$ sudo docker compose down
```

</div>
<div class="text-base">

- 基本的にコンテナをバックグラウンドで実行し続けていても問題無いが, コンテナを停止したいときは`docker compose stop`を使用. 再度立ち上げるときは"Start development"のページの処理を繰り返せば良い.

- `docker compose down`を使用するとコンテナを削除可能.

</div>
</div>

---
name: 'technical stack of ascender'
---

## Technical stack of Ascender

<div class="pt-1">

**Basic knowledge is required**

- <img class="h-6 inline-block" src="https://python-poetry.org/images/logo-origami.svg"> [Poetry](https://python-poetry.org): Pythonパッケージの依存関係管理, パッケージ作成を行うためのツール.

  <div class="text-sm">
  
  - 基本的な使い方は別スライド"Poetry101"（[En](https://cvpaperchallenge.github.io/Britannica/poetry101/en), [Ja](https://cvpaperchallenge.github.io/Britannica/poetry101/ja)）を参照.

  </div>

- <img class="h-6 inline-block" src="https://cdn-icons-png.flaticon.com/512/5969/5969059.png"> [Docker](https://www.docker.com/): コンテナ仮想化を使用した開発を行うためのツール.

  <div class="text-sm">
    
    - よく分からなくても, Ascenderの`README.md`と本スライドに記載の手順に沿うだけで基本的な開発は可能.

  </div>

</div>
<div class="pt-1">

**Better to know**

- <img class="h-6 inline-block" src="https://matangover.gallerycdn.vsassets.io/extensions/matangover/mypy/0.2.2/1645584655993/Microsoft.VisualStudio.Services.Icons.Default"> [Mypy](http://www.mypy-lang.org): Pythonで静的型チェックを行うためのツール.

- <img class="h-6 inline-block" src="https://t2.gstatic.com/faviconV2?client=SOCIAL&type=FAVICON&fallback_opts=TYPE,SIZE,URL&url=https://docs.pytest.org/en/stable/"> [pytest](https://docs.pytest.org): Pythonでテストを簡単に書くためのツール.
</div>
<div class="pt-4 opacity-60 text-sm">
※ Poetry以外のツールの基本的な使い方のスライドも順次作成予定.
</div>

---
name: 'upcoming feature'
layout: 'iframe-right'
url: https://www.matthewtancik.com/nerf
---

## Upcoming feature

<br>

### Project page template

<div class="pt-2">

- カッコよくて, 見やすいプロジェクトページを短時間で作りたいが, フロントエンド開発の学習にかけたくない.
- SPA形式のテンプレート:
  
  <div class="text-sm">

  - 頻出する機能をコンポーネントとして実装済.
  - HTML + CSS の知識のみで編集可能.（コンポーネントによってはJavaScriptも必要.）
  - マルチデバイス対応済.

  </div>

- 2022年7月末に追加予定.

</div>

---
name: 'cvpaper.challenge'
layout: 'center'
---

<img class="h-100 -mt-10" src="http://xpaperchallenge.org/cv/_nuxt/img/4ef71b1.png" /><br>
<div class="text-center text-xs opacity-50 -mt-20 hover:opacity-100">
  <a href="http://xpaperchallenge.org/cv/" target="_blank">
    Visit our page
  </a>
</div>

---
name: 'end'
layout: 'end'
---