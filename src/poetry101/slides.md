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
A tool for dependency management and packaging in Python.
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

- Dependency management by exhaustive resolver
- Environment isolation by virtualenvs
- Easily buid, package and publish projects to PyPI

</div>
</div>
</div>


---
name: 'how poetry work?'
---

## How Poetry work? <MarkerStumblingPoints />

<v-clicks :every='1'>
<div>

- Two important files:

  - `pyproject.toml`: (constraint) A file that is modified by the user; e.g. `poetry add` command. It contains constraints on the version of the package **to be installed**.

  - `poetry.lock`: (state) A file that is automatically modified by Poetry. It contains info about the packages **actually installed** and their dependencies, based on the constraints in `pyproject.toml`.

</div>
<div>

- When Poetry installs or updates a package, it checks whether a version that satisfies both 1. and 2. below exists, and executes it if it is found. (In this slide, 1. and 2. are combined and referred to as **updateability requirements**.)

  1. **All packages must sutisfy the version constraints described in `pyproject.toml`.**

  1. **No conflicts with already installed packages and their dependent packages.**

</div>
<div>

- By including the `poetry.lock` file and sharing it on github etc., you can share the exactly same Python package environment among your team.

</div>
</v-clicks>


---
name: 'frequent commands'
layout: 'iframe-right'
url: https://python-poetry.org/
---

## Frequent commands

- example commands:

  - install
  - add
  - update
  - remove
  - run
  - show

- For detail please check [offical docs](https://python-poetry.org/docs/cli/).

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

# install packages except dev dependencies. 
$ poetry install --no-dev
```

</div>
<div class="text-base">

- Refer to the version constraints in `pyproject.toml`, search for the version of each package that satisfies the updatable requirement, and install if it found. At the same time, create `poetry.lock` and write the information of the installed packages.

- If `poetry.lock` already exists, install the exact same version of the package as described in `poetry.lock`.

- If a config [`virtualenvs.create`](https://python-poetry.org/docs/configuration/#virtualenvscreate) is True, create a virtual environment and packages are installed in it.

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
# Check if the latest version of numpy satisfies the
# updatable requirements. If so, then install it.
$ poetry add numpy

# Search for versions of numpy less than x.y that meets
# the updatable requirements. If it found, install it.
$ poetry add "numpy<x.y"

# (Caret requirements) Search for versions in the range
# of x.y.z or greater, and less than x+1.0.0.
$ poetry add "numpy^x.y.z"

# Attempt to install the master branch of pytorch.
$ poetry add git@github.com:pytorch/pytorch.git#master

# Attempt to install local directory /my-package.
$ poetry add ./my-package/
```

</div>
<div class="text-base">

- Add a package if the version that satisfies the updatable requirements is found. (If not, a `SolverProblemError` is raised.)

- If it is a package, you can also install specific branches of github or local directories/files.

- There are several unique notations for version specification (explain later).

- While useful, it is also a command that can be easily stumbled over due to errors (explain how to deal with errors later).

</div>
</div>

---
name: 'frequent commands: update'
---

## Frequent commands

<br>

### [update](https://python-poetry.org/docs/cli/#update)

<div class="grid grid-cols-2 gap-x-4">
<div class="pt-4">

```shell {all|1-4|5-6|all}
# Update all packages that satisfies the updatable
# requirements.
$ poetry update

# It can also be used for specific packages only.
$ poetry update numpy
```

</div>
<div class="text-base">

- Update the package if it satisfies the updatable requirements.

- A list of packages that can be updated is able to be found by `poetry show --latest` which is explained later.

- When you want to update exceeding the version constraints listed in `pyproject.toml`,  need to use `poetry add` to add the package again.

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
# uninstall numpy.
$ poetry remove numpy
```

</div>
<div class="text-base">

- When `poetry remove` is executed, the version constraint information of the target package in `pyproject.toml` is removed.

- If no other package depends on the target package, the package will be uninstalled.

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

```shell {all|1-3|4-6|all}
# Run Python3 in the virtual environment created by Poetry.
$ poetry run python3

# Apply black to the src directory in the virtual
# environment created by Poetry.
$ poetry run black src
```

</div>
<div class="text-base">

- Running commands in the Poetry virtual environment. To use packages installed by Poetry, you need to execute the commands in the Poetry virtual environment.

- If you have installed a package but it is not found, you might forgot to use this `poetry run`.

- If you don't want to use `poetry run` each time, you can use the [`poetry shell`](https://python-poetry.org/docs/cli/#shell) command to start a new shell in a virtual environment.

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
# Shows the list of currently installed packages.
$ poetry show

# Shows package dependencies as a tree.
$ poetry show --tree

# Shows the latest version of the package.
$ poetry show --latest
```

</div>
<div class="text-base">

- Shows various information about the packages installed by Poetry.

- In particular, `poetry show --latest` is useful in combination with `poetry update`.

</div>
</div>

---
name: 'off-topic: python module/package/library'
---

## Off-topic

<br>

### Python module / package / library

- [Python module](https://docs.python.org/3.10/tutorial/modules.html#modules) means a `.py` file.

- [Python package](https://docs.python.org/3.10/tutorial/modules.html#packages) is a way of structuring a module and means a directory containing `__init__.py` and `.py` files. A package may contain subordinate packages within it.

- The definition of "library" is not mentioned in the official Python documentation, but it often refers to a package or a set of packages published to PyPI etc.

---
name: 'off-topic: semantic versioning'
---

## Off-topic

<br>

### [Semantic versioning](https://semver.org/)

- Specify the software version in the form of `x.y.z`.

- `x` is called the **major version** and is incremented when the API changes incompatibly.

- `y` is called the **minor version** and is incremented when backward-compatible functionality is added.

- `z` is called the **patch version**, and is incremented when a bug fix with backward compatibility is made.

---
name: 'dependency specification: caret requirements'
---

## Dependency specification

<br>

### [Caret requirements](https://python-poetry.org/docs/dependency-specification/#caret-requirements) <MarkerStumblingPoints />

<div class="text-base">

- Specify version using `^`.

- Allows a range of non-zero most-left digit is not change.

</div>

| Requirement | Versions allowed |
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

- Specify version using `~`.

- The meaning differs depending on its format:

  - When in `~x.y.z` or `~x.y` format, allow a range of patch version changes.
  - When in `~x` format, allow a range of minor version changes.

</div>

| Requirement | Versions allowed |
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
# Attempt to install two packages related to AWS
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

- `s3fs` depends on `botocore(>=1.24.21,<1.24.22)` and `boto3` depends on `botocore (>=1.19.43,<1.20.0)` respectively. This raises a `SolverProblemError` because the updateability requirement 2. is not satisfied.

- To install both `s3fs` and `boto3`, you need to adjust version constraints to avoid `botocore` conflicts.

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
# Attempt to install two packages related to AWS
$ poetry add "boto3==1.16.43"
$ poetry add "s3fs<=2022.5.0" # relax the constraint of s3fs

Updating dependencies
Resolving dependencies... (8.4s)

Writing lock file

Package operations: 2 installs, 0 updates, 0 removals

  • Installing fsspec (2022.5.0)
  • Installing s3fs (0.4.2)
```

</div>
<div class="text-base">

- For example, by relaxing the version constraint of `s3fs`, Poetry will search for a version of `s3fs` that does not conflict with the version of `botocore` on which `boto3` depends.

- If you loosen the `s3fs` version constraint as above and still cannot find a version that does not conflict, you will get a `SolverProblemError` as well. In such cases, it is necessary to consider relaxing the `boto3` version constraint as well.

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