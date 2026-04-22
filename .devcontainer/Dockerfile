FROM node:20-slim

WORKDIR /workspace

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

RUN useradd -m -s /bin/bash devuser
USER devuser

EXPOSE 3000
# Node.js 開発環境用コンテナ（最小限）
FROM node:20-slim

# 開発用ツールをインストール
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

# openspec をインストール
RUN npm install -g @fission-ai/openspec@latest

# 作業ディレクトリを設定
WORKDIR /workspace

# 通常ユーザーを作成（root で実行しない）
RUN useradd -m -s /bin/bash devuser

# ユーザーを切り替え
USER devuser

# ポート
EXPOSE 3000

# デフォルトコマンド
CMD ["node", "index.js"]
