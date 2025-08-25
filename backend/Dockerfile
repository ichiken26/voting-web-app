# ベースになるPythonのイメージを指定するよ
FROM python:3.10-slim

# コンテナの中での作業ディレクトリを設定
WORKDIR /app

# 最初に依存関係のファイルだけコピーする
# こうすると、ライブラリが変わらない限りキャッシュが使われてビルドが速くなるんだ
COPY requirements.txt .

# requirements.txtに書かれたライブラリをインストール
RUN pip install --no-cache-dir -r requirements.txt

# ソースコードをコンテナにコピー
COPY ./src .

# コンテナが起動した時に実行するコマンド
# aiohttpを 0.0.0.0 の 8080番ポートで起動するよ
CMD ["python", "main.py"]