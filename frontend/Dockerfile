# Node.jsの公式イメージをベースにするよ
FROM node:20-alpine

# コンテナ内の作業ディレクトリ
WORKDIR /app

# package.jsonとpackage-lock.jsonを先にコピーして依存関係をインストール
# これもキャッシュを効かせるための工夫だよ
COPY package*.json ./
RUN npm install

# プロジェクトのファイルを全部コピー
COPY . .

# Vue CLIの開発サーバーを起動するコマンド
CMD ["npm", "run", "serve"]