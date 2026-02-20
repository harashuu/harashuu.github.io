---
title: "uv入門①：uvとは何か・特徴・導入と基本的な使い方"
date: 2026-02-04
lastmod: 2026-02-04
categories:
  - programming
tags:
  - Python
  - uv
  - uv.lock
  - 開発環境
  - パッケージ管理
series:
  - Python開発環境とuv
draft: false
---

## この記事について
この記事では、Pythonの環境・依存関係管理ツールである **[uv](https://docs.astral.sh/uv/)** について、

- uv とは何か
- どうやって導入するのか
- インストールしてすぐPythonファイルを実行する方法

を、ざっくり把握することを目的とします。

詳細な使い分けや実務的な運用については、続編の記事で扱います。

---

## uvとは
uv は、Python のバージョン管理・パッケージ管理・仮想環境の作成・実行を高速かつ一貫した形で扱うためのツールです。

pip や venv を個別に使うのではなく、

- 依存関係の管理
- 仮想環境の作成
- スクリプトやコマンドの実行

を一つのソフトウェアで、かつ高速に扱える点が特徴です。

---

## uv の特徴（ざっくり）
- 処理が高速（pip より体感で速い）
- pyproject.toml / lock ファイルによる再現性
- 新規プロジェクトでも既存プロジェクトでも使える
- CI/CDとの連携も可能。チームで開発環境を合わせたい場合もOK

---

## 導入方法
[uv の公式インストール方法](https://docs.astral.sh/uv/getting-started/installation/)に従って導入できます。（※ 詳細は公式ドキュメントを参照）
WindowsならPowerShellで以下を実行
```bash
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
実行時の出力イメージ
```
PS C:\WINDOWS\System32> powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
Downloading uv 0.10.4 (x86_64-pc-windows-msvc)
Installing to C:\Users\〇◇△▽\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!

To add C:\Users\〇◇△▽\.local\bin to your PATH, either restart your shell or run:

    set Path=C:\Users\〇◇△▽\.local\bin;%Path%   (cmd)
    $env:Path = "C:\Users\〇◇△▽\.local\bin;$env:Path"   (powershell)
```
言われたとおりに以下を実行しパスを追加
```bash
$env:Path = "C:\Users\〇◇△▽\.local\bin;$env:Path"
```

PowerShellの再起動後、以下のコマンドが使えるようになっていれば準備完了です。

```bash
uv --version
```

## ひとまずすぐ使うには
たとえば
- もしあなたがPython初心者で
- 手元にpythonファイル（～.pyファイル）があって
- ひとまずuvのインストールが終わった
という状況で、そのpythonファイルを動かしたいとします。
詳しいシーン別の使い方は次の記事に書くとして、ここではサクッと実行する方法を見てみましょう。

1. 外部ライブラリを使わないpythonファイルの場合
例えば以下のような内容の`no_external_library.py`があるとき、
```python
print("Hello, world!!")
```
PowerShellまたはコマンドプロンプトで当該ファイルのある階層にて以下のように実行します[^1]。
```bash
uv run no_external_library.py
```

2. 外部ライブラリを使うpythonファイルの場合
例えば以下のような内容の`with_external_library.py`があるとき[^2]、
```python
from rich import print
from rich.panel import Panel
print("[bold green]Hello, uv + rich![/bold green]")
print(Panel("外部ライブラリが使えました 🎉"))
```
PowerShellまたはコマンドプロンプトで当該ファイルのある階層にて以下のように実行します。
```bash
uv run --with rich with_external_library.py
```
[^1]: 初めて実行するときには自動でPythonがインストールされます。```uv python list```で確認できます。
[^2]: `rich`はコンソール出力を修飾するライブラリです