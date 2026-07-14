# ccp-py

解析・研究開発向けの Python Docker イメージです。Python 3.11、JupyterLab、科学計算・機械学習・ベイズモデリング・ネットワーク分析向けのパッケージ、および再現可能なレポート作成のための Quarto を提供します。

公開イメージは GitHub Container Registry (GHCR) で配布しています。

```bash
docker pull ghcr.io/ykunisato/ccp-py:latest
```

## 含まれるもの

- Python 3.11
- Jupyter Notebook / JupyterLab
- Quarto CLI
- 数値・データ分析: NumPy、SciPy、pandas、Matplotlib、seaborn
- 機械学習・最適化: scikit-learn、JAX、Equinox、Optax、MCTS (`mctx`)
- ベイズモデリング・能動推論: PyMC、ArviZ、`inferactively-pymdp`、PyHGF
- ネットワーク分析: NetworkX、igraph、leidenalg、python-louvain、PyVis
- 開発・実行環境: Git、`tini`

Python パッケージの完全な一覧は [requirements.txt](requirements.txt) を参照してください。

## 使い方

### JupyterLab を起動する

作業ディレクトリをコンテナ内の `/workspace` にマウントして起動します。

```bash
docker run --rm -it \
  --name ccp-py \
  -p 8888:8888 \
  -v "$(pwd):/workspace" \
  ghcr.io/ykunisato/ccp-py:latest
```

起動ログに表示されるトークン付き URL をブラウザで開いてください。通常は `http://127.0.0.1:8888/lab?token=...` です。コンテナを停止すると、`--rm` によりコンテナ自体は削除されますが、マウントした作業ディレクトリ内のファイルは残ります。

### Quarto でレポートを作成する

コンテナ内では `quarto` コマンドをそのまま利用できます。JupyterLab または VS Code のターミナルで、作業ディレクトリ内の Quarto ドキュメントを HTML にレンダリングするには次を実行します。

```bash
quarto render report.qmd --to html
```

ホストのターミナルから直接レンダリングする場合は、次のように実行します。

```bash
docker run --rm -it \
  -v "$(pwd):/workspace" \
  ghcr.io/ykunisato/ccp-py:latest \
  quarto render report.qmd --to html
```

Quarto のバージョンは次のコマンドで確認できます。

```bash
quarto --version
```

Jupyter カーネルを使う `.qmd` ファイルも、必要な Python 環境が同じイメージに含まれているため、コンテナ内で実行・レンダリングできます。

### VS Code から接続する

上記のようにコンテナを起動した後、VS Code の Dev Containers 拡張機能で実行中のコンテナにアタッチしてください。作業ファイルは `/workspace` にあります。

## ローカルでビルドする

Dockerfile と `requirements.txt` を変更した場合は、リポジトリのルートでローカルイメージをビルドできます。

```bash
docker build -t ccp-py:local .
```

ローカルイメージは公開イメージと同じ方法で起動できます。

```bash
docker run --rm -it -p 8888:8888 -v "$(pwd):/workspace" ccp-py:local
```

## 対応プラットフォーム

公開イメージの `latest` タグは次のプラットフォームに対応しています。

- `linux/amd64`
- `linux/arm64`

Quarto もビルド対象プラットフォームに対応する公式 Debian パッケージをインストールします。

## イメージの公開

`main` ブランチへの push により GitHub Actions が各プラットフォーム向けイメージをビルドし、`ghcr.io/ykunisato/ccp-py:latest` のマルチアーキテクチャイメージとして公開します。

## メンテナ

Yoshihiko Kunisato (<kunisato@psy.senshu-u.ac.jp>)
