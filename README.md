# DailyReportApplication - Deployment Environment

## 概要
このリポジトリは、日報管理システムをDockerコンテナ上で動作させるための環境構築用プロジェクトです。
Webサーバー（Apache 2 + Java）とDBサーバー（MySQL）をマルチコンテナ構成で構築します。

## ディレクトリ構成
```text
.
├── docker-compose.yml      # コンテナ全体の定義
├── web/                    # Webサーバー用設定
│   ├── Dockerfile          # Apache 2 + Java 実行用イメージ
│   ├── startup.sh          # アプリケーション起動スクリプト
│   └── DailyReportSystemApplication-0.0.1-SNAPSHOT.jar.jar  # ビルド済みのJARファイル（要配置）
└── db/                     # データベース用設定
    ├── Dockerfile          # MySQL用イメージ
    ├── mysql_setup.sql     # データベース・ユーザー初期設定用SQL
    └── startup.sh          # DB起動・初期化スクリプト
