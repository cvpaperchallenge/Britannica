---
layout: cover
download: true
highlighter: shiki
fonts:
  mono: 'Fira Mono'
info: |
  ## Poetry 101

  Basics and stumbling points

  [Yoshihiro Fukuhara](https://gatheluck.net/home/)

  - [Source code](https://github.com/cvpaperchallenge/Britannica/tree/master/src/poetry101/)
---

# Poetry 101

Basics and stumbling points

<div class="uppercase text-sm tracking-widest">
Yoshihiro Fukuhara
</div>

<div class="abs-bl mx-14 my-12 flex">
  <img src="http://xpaperchallenge.org/_nuxt/img/fa53e31.png" class="h-8">
  <div class="ml-3 flex flex-col text-left">
    <div>cvpaper.challenge</div>
    <div class="text-sm opacity-50">May. 30th, 2022</div>
  </div>
</div>

---
name: 'content'
layout: 'default'
---

## Content

<div class="text-xl">

- What is Poetry?

- How Poetry work?

- Frequent commands

- Dependency specification

- Frequently faced error

</div>

---
name: 'what is poetry?'
---

## What is Poetry?

<br>

<div class="opacity-50">
Pythonパッケージの依存関係管理, パッケージ作成を行うためのツール.
</div>

<div class="pt-20">
<div class="grid grid-cols-2 gap-x-4">
<div class="text-center">

  <img class="h-50 inline-block" src="https://python-poetry.org/images/logo-origami.svg">
  <div class="opacity-50 mb-2 text-sm mt-2">
    Dependency Management for Python
  </div>
  <div>
    <!-- version -->
    <a class="!border-none" href="https://pypi.org/project/poetry/" target="__blank">
      <img class="h-4 inline mx-0.5" src="https://img.shields.io/pypi/v/poetry?label=" alt="Version">
    </a>
    <!-- downloads -->
    <a class="!border-none" href="https://pypistats.org/packages/poetry" target="__blank">
      <img class="h-4 inline mx-0.5" alt="Downloads" src="https://img.shields.io/pypi/dm/poetry?color=50a36f&label=">
    </a>
    <br>
    <!-- github stars -->
    <a class="!border-none" href="https://github.com/python-poetry/poetry" target="__blank">
      <img class="mt-2 h-4 inline mx-0.5" alt="GitHub stars" src="https://img.shields.io/github/stars/python-poetry/poetry?style=social">
    </a>
  </div>

</div>
<div class="">

###### Feature

- パッケージの依存関係管理
- 仮想環境の構築
- 作成したパッケージのPyPIへのパブリッシュ

</div>
</div>
</div>


---
name: 'how poetry work?'
---

## How Poetry work? <MarkerStumblingPoints />

<v-clicks :every='1'>
<div>

- 2つの重要なファイル :

  - `pyproject.toml` :（制約）ユーザーが`poetry add`コマンド等によって変更を行うファイル. インストールしたいパッケージのバージョンに対する制約が記載される.

  - `poetry.lock` :（状態）Poetryが自動変更するファイル. `pyproject.toml`に記載されているバージョン制約を元に, 実際にインストールしたパッケージの情報と, 依存パッケージの情報が記載される.

</div>
<div>

- Poetryはパッケージをインストール・アップデートするとき, 下記の1.と2.を両方満たすバージョンが存在するかを確認し, 見つかれば実行する.（本スライドでは, 1.と2.を合わせて**更新可能要件**と表現する.）

  1. **全てのパッケージが`pyproject.toml`に記載されたバージョン制約を満たすこと.**

  1. **既にインストールされているパッケージと依存パッケージの競合がないこと.**

</div>
<div>

- `poetry.lock`ファイルを含めてgithub等で共有することで, チーム間で同じパッケージの環境を共有することが出来る.

</div>
</v-clicks>


---
name: 'frequent commands'
layout: 'iframe-right'
url: https://python-poetry.org/
---

## Frequent commands

- 頻繁に使用するコマンド:

  - install
  - add
  - update
  - remove
  - run
  - show

- 詳しい情報は[offical docs](https://python-poetry.org/docs/cli/)に記載がある.

---
name: 'frequent commands: install'
---

## Frequent commands

<br>

### [install](https://python-poetry.org/docs/cli/#install)

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all}
$ poetry install

# dev dependenciesを除いてインストールを行う. 
$ poetry install --no-dev
```

</div>
<div class="text-base">

- `pyproject.toml`に記載されているバージョン制約を参照して, 更新可能要件を満たす各パッケージのバージョンを探索し, 見つかればインストールする. 同時に`poetry.lock`を作成し, インストールしたパッケージの情報を書き込む.

- `poetry.lock`が既に存在する場合は`poetry.lock`に記載されているものと全く同じバージョンのパッケージをインストールする.

- configの[`virtualenvs.create`](https://python-poetry.org/docs/configuration/#virtualenvscreate)が`True`になっていると仮想環境を作成し, パッケージを仮想環境内にインストールする.

</div>
</div>

---
name: 'frequent commands: add'
---

## Frequent commands

<br>

### [add](https://python-poetry.org/docs/cli/#add) <MarkerStumblingPoints />

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all|1-4|5-8|9-12|13-15|16-19|all}
# numpyの最新のバージョンが更新可能要件を満たすかを確認し, 
# もし満たしていればインストールする.
$ poetry add numpy

# numpyのx.y未満のバージョンで更新可能要件を満たすバージョンを
# 順次探して, もし見つかればインストールする.
$ poetry add "numpy<x.y"

# (Caret記法) x.y.z以上, x+1.0.0未満（厳密には正確では無い）の
# 範囲でバージョンを順次探して, もし見つかればインストールする.
$ poetry add "numpy^x.y.z"

# cvpaperchallenge/melonのdevelopブランチのインストールを
# 試みる.
$ poetry add git@github.com:cvpaperchallenge/melon.git#develop

# ローカルディレクトリ./my-package/下のインストールを試みる.
$ poetry add ./my-package/
```

</div>
<div class="text-base">

- 更新可能要件を満たすバージョンが見つかれば, パッケージの追加を行う. (見つからなかった場合は`SolverProblemError`になる.)

- パッケージであればgithubの特定のブランチやローカルディレクトリ/ファイルもインストール出来る.

- 独特なバージョン指定の記法があるので, 少し覚える必要がある(後述).

- 便利な一方で, エラーで躓き安いコマンドでもある (エラーが出た場合の対処方は後述).

</div>
</div>

---
name: 'frequent commands: update'
---

##  Frequent commands

<br>

### [update](https://python-poetry.org/docs/cli/#update)

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all|1-3|4-5|all}
# 更新可能要件を満たしているパッケージを全てアップデートする.
$ poetry update

# 特定のパッケージのみに対して使用することも可能.
$ poetry update numpy
```

</div>
<div class="text-base">

- 更新可能要件を満たしていれば, パッケージのアップデートを行う.

- アップデート可能なパッケージの一覧は後述の`poetry show --latest`などで一覧出来る.

- `pyproject.toml`に記載されているバージョン制約以上のアップデートを行いたい場合は`poetry add`を使用して再度パッケージの追加を行う.

</div>
</div>

---
name: 'frequent commands: remove'
---

## Frequent commands

<br>

### [remove](https://python-poetry.org/docs/cli#remove)

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all}
# numpyをアンインストールする.
$ poetry remove numpy
```

</div>
<div class="text-base">

- `poetry remove`が実行されると, `pyproject.toml`に記載されている対象のパッケージのバージョン制約の情報が削除される.

- 対象のパッケージに依存している他のパッケージが1つも無ければパッケージはアンインストールされる.

</div>
</div>

---
name: 'frequent commands: run'
---

## Frequent commands

<br>

### [run](https://python-poetry.org/docs/cli/#run)

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all|1-3|4-5|all}
# poetryの作成した仮想環境内でPython3を実行.
$ poetry run python3

# poetryの作成した仮想環境内でblackをsrcディレクトリに適用.
$ poetry run black src
```

</div>
<div class="text-base">

- Poetryの仮想環境内でコマンドを実行する. Poetryでインストールしたパッケージを使用するためにはpoetryの仮想環境内でコマンドを実行する必要がある.

- 「Poetryでパッケージをインストールしたのに, それが見つからないと怒られる.」という場合は大体この`poetry run`をつけ忘れていることが多い.

- その都度`poetry run`をつけるのが面倒という方は[`poetry shell`](https://python-poetry.org/docs/cli/#shell)コマンドを使って仮想環境の中でshellを立ち上げることも出来る.

</div>
</div>

---
name: 'frequent commands: show'
---

## Frequent commands

<br>

### [show](https://python-poetry.org/docs/cli/#show)

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all|1-3|4-6|7-8|all}
# 現在インストールされているパッケージの一覧を表示.
$ poetry show

# パッケージの依存関係をツリーとして表示.
$ poetry show --tree

# パッケージの最新のバージョンを表示.
$ poetry show --latest
```

</div>
<div class="text-base">

- Poetryでインストールしたパッケージの様々な情報を表示する.

- 特に`poetry show --latest`は`poetry update`と組み合わせて使用すると便利.

</div>
</div>

---
name: 'off-topic: python module/package/library'
---

## Off-topic

<br>

### Python module / package / library

- [モジュール (module) ](https://docs.python.org/3.10/tutorial/modules.html#modules)とは`.py`ファイルのこと.

- [パッケージ (package) ](https://docs.python.org/3.10/tutorial/modules.html#packages)とはモジュールを構造化する手段で, `__init__.py`ファイルと`.py`ファイルを含んだディレクトリのこと. パッケージはその中に下位のパッケージを含むこともある.

- ライブラリ (library) の定義についてはPythonの公式ドキュメントには記載が無いと思われるが, [PyPI](https://pypi.org/)等に公開されているパッケージ, もしくはパッケージの集合を意味することが多い.

---
name: 'off-topic: semantic versioning'
---

## Off-topic

<br>

### [Semantic versioning](https://semver.org/)

- ソフトウェアのバージョンを`x.y.z`の形で指定する.

- `x`はメジャーバージョンと呼ばれ, APIの変更に互換性のない場合にインクリメントする.

- `y`はマイナーバージョンと呼ばれ, 後方互換性があり機能性を追加した場合にインクリメントする.

- `z`はパッチバージョンと呼ばれ, 後方互換性を伴うバグ修正をした場合にインクリメントする.

---
name: 'dependency specification: caret requirements'
---

## Dependency specification

<br>

### [Caret requirements](https://python-poetry.org/docs/dependency-specification/#caret-requirements)

<div class="text-base">

- `^`を用いてバージョン制約を記述する.

- ゼロでない再左の数字を変更しない範囲を表す.

</div>

| 例 | 表す範囲 |
| --- | --- |
| `^1.2.3` | `>=1.2.3,<2.0.0` |
| `^1.2`   | `>=1.2.0,<2.0.0` |
| `^1`     | `>=1.0.0,<2.0.0` |
| `^0.2.3` | `>=0.2.3,<0.3.0` |
| `^0.0.3` | `>=0.0.3,<0.0.4` |

---
name: 'dependency specification: tilde requirements'
---

## Dependency specification

<br>

### [Tilde requirements](https://python-poetry.org/docs/dependency-specification/#tilde-requirements)

<div class="text-base">

- `~`を用いてバージョン制約を記述する.

- 形式によって意味が異なる.

  - `~x.y.z`または`~x.y`の形式のときパッチバージョンの変更の範囲を表す.
  - `~x`の形式のときマイナーバージョンの変更の範囲を表す.

</div>

| 例 | 表す範囲 |
| --- | --- |
| `~1.2.3` | `>=1.2.3,<1.3.0` |
| `~1.2`   | `>=1.2.0,<1.3.0` |
| `~1`     | `>=1.0.0,<2.0.0` |

---
name: 'frequently faced error: solver_problem_error 1'
---

## Frequently faced error

<br>

### SolverProblemError <MarkerStumblingPoints />

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all|1-3|8-15|all}
# AWSに関連する２つのパッケージのインストールを試みる
$ poetry add "boto3==1.16.43"
$ poetry add "s3fs^2022.5.0"

Updating dependencies
Resolving dependencies... (0.4s)

  SolverProblemError

  (途中略)
  Thus, s3fs (>=2022.5.0,<2023.0.0) requires botocore (>=1.24.21,<1.24.22).
  And because boto3 (1.16.43) depends on botocore (>=1.19.43,<1.20.0), s3fs (>=2022.5.0,<2023.0.0) is incompatible with boto3 (1.16.43).
  So, because ascender depends on both boto3 (1.16.43) and s3fs (^2022.5.0), version solving failed.
```

</div>
<div class="text-base">

- `s3fs`は`botocore(>=1.24.21,<1.24.22)`に, `boto3`は`botocore (>=1.19.43,<1.20.0)`にそれぞれ依存しており, 依存パッケージが競合している. これは, 更新可能要件の2.を満たしていないため`SolverProblemError`が発生する.

- `s3fs`と`boto3`を両方インストールするにはバージョン制約を調整して, `botocore`の競合が発生しないようにする必要がある.

</div>
</div>

---
name: 'frequently faced error: solver_problem_error 2'
---

## Frequently faced error

<br>

### SolverProblemError <MarkerStumblingPoints />

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all|1-3|13|all}
# AWSに関連する２つのパッケージのインストールを試みる
$ poetry add "boto3==1.16.43"
$ poetry add "s3fs<=2022.5.0" # s3fsの制約を緩める

Updating dependencies
Resolving dependencies... (8.4s)

Writing lock file

Package operations: 2 installs, 0 updates, 0 removals

  • Installing fsspec (2022.5.0)
  • Installing s3fs (0.4.2)
```

</div>
<div class="text-base">

- 例えば, `s3fs`のバージョン制約を緩めることで, `boto3`の依存している`botocore`のバージョンと競合しないバージョンの`s3fs`のバージョンを探索するようになる.

- 上のように`s3fs`バージョン制約を緩めても競合しないバージョンがもし見つからない場合は同様に`SolverProblemError`が発生する. このような場合は`boto3`側の制約も緩めることを考える必要がある.

</div>
</div>

---
name: 'presenter'
layout: 'intro'
---

# Yoshihiro Fukuhara

<div class="leading-8 opacity-80">
ML / MLOps engineer.<br>
HQ member and XCCV group lead of cvpaper.challenge.<br>
</div>

<div class="my-10 grid grid-cols-[40px,1fr] w-min gap-y-4">
  <ri-github-line class="opacity-50"/>
  <div>
    <a href="https://github.com/gatheluck" target="_blank">gatheluck</a>
  </div>
  <ri-twitter-line class="opacity-50"/>
  <div>
    <a href="https://twitter.com/gatheluck" target="_blank">gatheluck</a>
  </div>
  <ri-user-3-line class="opacity-50"/>
  <div>
    <a href="https://gatheluck.net/home/" target="_blank">gatheluck.net</a>
  </div>
</div>

<img src="https://avatars.githubusercontent.com/u/21344007?v=4" class="rounded-full w-40 abs-tr mt-16 mr-12"/>

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