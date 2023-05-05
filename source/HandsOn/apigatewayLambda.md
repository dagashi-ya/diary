# API GatewayとLambdaを使ったサーバレスSpringアプリケーション

## 前提
以下ページを参考にAPI GatewayとLambdaを使用したspringアプリケーションを作成する。
https://news.mynavi.jp/techplus/article/techp4316/
基本的に、上記のページをトレースするが、学習のため本ページにまとめる。

## サーバレス開発 - Spring Cloud Funtionとは
- サーバレス開発とは、サーバや実行環境をほぼ意識することなく、最少限度のプログラムを用いて、アプリケーション実行環境を構築する開発方法を指している
- AWSでは、リクエストを受け付ける(APサーバの役割を代替する)API Gatewayと、アプリケーションの実行ランタイムを提供するAWS Lamdaを用いて、標準的なサーバレス開発を実現できる
	- API Gateway
	- AWS Lamda
- Spring Cloud Functionは関数型のビジネスロジック実行をベースとしたフレームワーク


