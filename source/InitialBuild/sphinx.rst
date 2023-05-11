=====================
sphinx構築手順
=====================
前提
========
- 以下の環境で構築を実施
	- WorkSpaces（Amazon Linux release 2 (Karoo)）
- Sphinxで作成したページをGithub Pagesで公開することを目標としている

手順
========
1.sphinxのインストール
--------
1. pythonのバージョン確認
pythonのバージョンを確認する

.. code-block:: python

	python -V

2. 各種インストール
sphinxとそれに付随したパッケージをインストールする

.. code-block:: python

	pip install sphinx furo
今回は"furo"というプロジェクトテンプレートを使用している。

.. note:: 

	プロジェクトテンプレートはこちら（https://sphinx-themes.org/）から選択できます。
	もちろん、デフォルトのままでもOK！
	
3. クイックスタート
sphinxプロジェクトを新規作成をする。
.. code-block:: python

	sphinx-quickstart
	
.. code-block:: shell

	Separate source and build directories (y/n) [n]: y
	Project name:{プロジェクト名}
	Author name(s): {著者名}
	Project release []: ｛リリースバージョン｝
	Project language [en]: jp

2.sphinxの設定変更
--------
1. copy.py



2. Makefile





3.git設定
--------
4.github pagesの公開設定
--------

reference
========

* `Sphinx の使い方 <https://zenn.dev/y_mrok/books/sphinx-no-tsukaikata/viewer/chapter1>`_