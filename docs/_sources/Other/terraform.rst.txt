=========================
Terraform実行環境構築
=========================


前提
========
- 以下の環境で構築を実施
	- WorkSpaces（Amazon Linux release 2 (Karoo)）
- Sphinxで作成したページをGithub Pagesで公開することを目標としている



事前準備
================
- AWSアカウントの作成

構築手順
================
AWS CLIインストール
---------------------

1. AWS CLI インストールファイルDL

.. code-block:: shell
	
	curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o ~/awscliv2.zip

2. インストールファイル展開
.. code-block:: shell
	
	unzip ~/awscliv2.zip

3. AWS CLI インストール

.. code-block:: shell
	
	sudo ~/aws/install

