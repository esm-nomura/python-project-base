# Python開発環境のセットアップとプロジェクト作成ガイド

このドキュメントは、プロジェクト「python-project-base」のためのPython開発環境のセットアップとプロジェクト作成手順を詳細に説明します。このガイドは、Mac（zshを使用）およびWindows（Git PowerShellを使用）の両方のOSでのPython開発環境をカバーしています。

## 目次

1. [導入](#1-導入)
2. [前提条件](#2-前提条件)
3. [Pythonの開発環境インストール](#3-Pythonの開発環境インストール)
4. [Poetryを用いたプロジェクトのセットアップ](#4-Poetryを用いたプロジェクトのセットアップ)
5. [開発ツールのインストールと設定](#5-開発ツールのインストールと設定)
6. [VSCode設定と拡張機能](#6-VSCode設定と拡張機能)
7. [開発環境のテスト](#7-開発環境のテスト)
8. [トラブルシューティング](#8-トラブルシューティング)
9. [追加リソースと学習材料](#9-追加リソースと学習材料)
10. [まとめ](#10-まとめ)

## 1. 導入

このセクションでは、このガイドの目的と、セットアッププロセス全体の概要を提供します。Python開発のための効率的な環境構築が主な焦点です。

## 2. 前提条件

- 基本的なコマンドライン操作の知識。
- Gitのインストール済み。
- MacまたはWindowsのOS。

## 3. Pythonの開発環境インストール

### Macでのpyenvのインストール

1. Homebrewのインストール（まだインストールしていない場合）。

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. Homebrewを使用してpyenvをインストール。

   ```bash
   brew update
   brew install pyenv
   ```

3. .zshrcファイルにpyenvの初期化コードを追加。

   ```bash
   echo 'eval "$(pyenv init --path)"' >> ~/.zshrc
   echo 'eval "$(pyenv init -)"' >> ~/.zshrc
   source ~/.zshrc
   ```

### Windowsでのpyenvのインストール

1. Git PowerShellのインストール。
   - [Gitの公式サイト](https://git-scm.com/)からダウンロードしてインストール。
2. pyenv-winをGit PowerShellを使用してインストール。

   ```powershell
   git clone https://github.com/pyenv-win/pyenv-win.git "$HOME/.pyenv"
   ```

3. 環境変数の設定。
   - システムのプロパティから環境変数を設定する。

### pyenvを用いたPythonのインストールと設定

1. pyenvを使用してPython 3.12.1をインストール。

   ```bash
   pyenv install 3.12.1
   pyenv global 3.12.1
   ```

2. プロジェクトディレクトリで使用するPythonバージョンを設

定。

   ```bash
   pyenv local 3.12.1
   ```

## 4. Poetryを用いたプロジェクトのセットアップ

### MacでのPoetryのインストール

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

インストール後、ターミナルを再起動するか、以下のコマンドを実行してPoetryのコマンドを有効化します。

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source $HOME/.zshrc
```

### WindowsでのPoetryのインストール

```powershell
(Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | python -
```

### Poetryプロジェクトの初期化

1. 新規Pythonプロジェクトの作成。

   ```bash
   mkdir python-project-base
   cd python-project-base
   ```

2. `poetry init` コマンドを実行してプロジェクトを初期化。

   ```bash
   poetry init
   ```

   - このコマンドは、プロジェクトの依存関係やメタデータを設定するための対話型プロンプトを提供します。
   - プロジェクトの詳細（名前、バージョン、説明、作者など）を入力します。
   - 必要な依存関係を追加するオプションがあります。このステップはスキップして後で`pyproject.toml`を手動で編集することもできます。

3. `pyproject.toml` ファイルを編集してプロジェクトの依存関係を管理。

## 5. 開発ツールのインストールと設定

### Ruffのインストールと設定

1. Ruffのインストール。

   ```bash
   poetry add ruff --dev
   ```

2. Ruffの基本的な設定と使用方法の説明。

### pydanticのインストールと基本的な使用方法

1. pydanticのインストール。

   ```bash
   poetry add pydantic
   ```

2. pydanticを使用してデータモデルを定義する基本的な例。

### pytestのインストールと設定

1. pytestのインストール。

   ```bash
   poetry add pytest --dev
   ```

2. pytestを使用した基本的なテストの作成と実行。

## 6. VSCode設定と拡張機能

### `settings.json`

プロジェクトルートに`.vscode`ディレクトリを作成し、以下の内容で`settings.json`ファイルを作成します。

```json
{
  // ********************************************************************************
  // python
  // ********************************************************************************
  "[python]": {
    "editor.formatOnSave": true,
    "editor.formatOnPaste": false,
    "editor.formatOnType": false,
    "editor.formatOnSaveMode": "file",
    "editor.defaultFormatter": "charliermarsh.ruff"
  },
  "ruff.organizeImports": false,
  "ruff.lint.args": ["--config=pyproject.toml"],
  "ruff.format.args": ["--config=pyproject.toml"],
  // ********************************************************************************
  // json
  // ********************************************************************************
  "[json][jsonc]": {
    "editor.formatOnSave": true,
    "editor.formatOnPaste": false,
    "editor.formatOnType": false,
    "editor.formatOnSaveMode": "file",
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

### `launch.json`

デバッグ設定のために、以下の内容で`launch.json`ファイルを作成します。

```json
{
    // IntelliSense を使用して利用可能な属性を学べます。
    // 既存の属性の説明をホバーして表示します。
    // 詳細情報は次を確認してください: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python デバッガー: 現在のファイル",
            "type": "debugpy",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal"
        }
    ]
}
```

### `extensions.json`

必要な拡張機能を提示するために、以下の内容で`extensions.json`ファイルを作成します。

```json
{
  "recommendations": [
    "ms-python.python",
    "ms-python.vscode-pylance",
    "kamilturek.vscode-pyproject-toml-snippets",
    "charliermarsh.ruff",
    "esbenp.prettier-vscode"
  ]
}

```

## 7. 開発環境のテスト

1. インストールしたツールの動作確認。
2

. サンプルコードを実行して環境が正しく機能していることを確認。

## 8. トラブルシューティング

このセクションでは、インストールプロセス中に発生する可能性のある一般的な問題とその解決策を提供します。

## 9. 追加リソースと学習材料

Python開発に関する追加のリソースや推奨される学習材料をリストアップします。

## 10. まとめ

このガイドの要約と、Python開発の次のステップについてのアドバイス。

---

これで、プロジェクト「python-project-base」のための完全なREADME.mdの内容が完成しました。
