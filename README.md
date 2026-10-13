# Synthetic Data Playground

syntheticというのは、日本語で「合成された」や「人工的な」という意味です。  
名前の通り、人工的にデータを生成して、データ分析やデータ分析基盤、データパイプライン等の構築やテスト、検証を行うためのライブラリについて調査しています。


また、このリポジトリを発展させて、AWSによるダミーデータ構築基盤の構築についても別のリポジトリで検討しています。

## Python環境

- Python 3.12 / uv / `.venv`
- データ生成・確認: Faker、Mimesis、pandas
- 開発用: ipykernel、Ruff、pytest
- Notebook: VS CodeのPython・Jupyter拡張機能

### セットアップ

uvをインストールした状態で、リポジトリのルートで実行します。

```sh
uv sync --locked
uv run python --version
```

`.python-version`でPython 3.12を指定し、`uv.lock`で依存バージョンを固定しています。
`uv sync`は`.venv`を作成し、開発用依存関係もインストールします。

### VS CodeでNotebookを使う

1. このフォルダをVS Codeで開き、推奨されるPython・Jupyter拡張機能をインストールします。
2. コマンドパレットから「Create: New Jupyter Notebook」を実行します。
3. 以下のコマンドで、プロジェクトの仮想環境を名前付きカーネルとして登録します。

```sh
uv run --locked python -m ipykernel install --user --name synthetic-data-playground --display-name "Python 3.12 (synthetic-data-playground)"
```

4. Notebook右上のカーネル選択で「Python 3.12 (synthetic-data-playground)」を選択します。

カーネル登録は各PCで一度実行します。リポジトリを移動した場合は、移動先で再登録してください。
登録せずに「Python Environments」→ `.venv/bin/python`を選択する方法でも利用できます。

参考: [uv公式のVS Code Jupyterガイド](https://docs.astral.sh/uv/guides/integration/jupyter/#using-jupyter-from-vs-code)

### 依存関係の追加・開発コマンド

Notebook内では`pip install`を使わず、ターミナルから`uv add`で追加します。
変更した`pyproject.toml`と`uv.lock`は一緒にGit管理します。

```sh
uv add <パッケージ名>
uv add --dev <開発用パッケージ名>
uv run ruff check .
uv run ruff format .
# テストを追加した後に実行
uv run pytest
```

将来SDVを試す際は、専用グループに追加できます。

```sh
uv add --group sdv sdv
uv sync --group sdv
uv run --group sdv python
```

SDVは現時点では未追加です。参考: [uv公式の依存関係管理ガイド](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-groups)

### Git管理

`pyproject.toml`、`uv.lock`、`.python-version`、コード・Notebook、READMEをGit管理します。
`.venv/`、Pythonキャッシュ、Notebookのチェックポイント、`outputs/`の生成データは除外します。
Notebookをコミットする際は、不要な実行結果をクリアします。
