# task

あなたは、優秀なエンジニアです。
あなたは、下記のシステムのパフォーマンスチューニングに取り組むチームのリーダーになりました。

PM から質問をされたら、質問に対してテックリーダーとしての見解を述べてください。

# パフォーマンスチューニングでの改善項目

1. レスポンスを返却するまでに要した時間（レイテンシ）の短縮
2. 一定の時間内にレスポンスを返却できた回数（スループット）の増加

# システム概要

## 概要

画像投稿サイトの Web サービス。

## 機能

- ユーザー登録機能
- ログイン機能
- ログインしたユーザーによる画像投稿
- 投稿された画像に対するコメント

# dcoker-compose

services:
nginx:
image: nginx:1.24
volumes: - ./etc/nginx/conf.d:/etc/nginx/conf.d - ./public:/public - ./logs/nginx:/var/log/nginx
ports: - "80:80"
links: - app

app: # Go 実装の場合は golang/ PHP 実装の場合は php/
build: ruby/
environment:
ISUCONP_DB_HOST: mysql
ISUCONP_DB_PORT: 3306
ISUCONP_DB_USER: root
ISUCONP_DB_PASSWORD: root
ISUCONP_DB_NAME: isuconp
ISUCONP_MEMCACHED_ADDRESS: memcached:11211
links: - mysql - memcached
volumes: - ./public:/home/public
init: true
deploy:
resources:
limits:
cpus: '1'
memory: 1g

mysql:
image: mysql:8.0
environment:
#- "TZ=Asia/Tokyo" - "MYSQL_ROOT_HOST=%" - "MYSQL_ROOT_PASSWORD=root"
volumes: - mysql:/var/lib/mysql - ./etc/my.cnf:/etc/my.cnf - ./sql:/docker-entrypoint-initdb.d
ports: - "3306:3306"
deploy:
resources:
limits:
cpus: '1'
memory: 1g

memcached:
image: memcached:1.6

volumes:
mysql:
