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

	pip install sphinx furo sphinx-autobuild myst_parser

| 今回は"furo"というプロジェクトテンプレートを使用している。 
| sphinx-autobuildは自動ビルド、自動リロードが出来るツール

.. note:: 

	| プロジェクトテンプレートはこちら（https://sphinx-themes.org/）から選択できます。
	| もちろん、デフォルトのままでもOK！
	
3. クイックスタート
sphinxプロジェクトを新規作成をする。

.. code-block:: shell

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
./souce/copy.py に以下を追記する

.. code-block:: python

	extensions = [
	    'myst_parser',
	    'sphinx.ext.githubpages', #github pages
	    "sphinx_copybutton",
	]
	source_suffix = [
	    '.rst',
	    '.md'
	]
	source_parsers = {
	    '.rst': 'restructuredtext',
	    '.md': 'markdown'
	}


2. Makefile
./Makefile に以下のように修正する
github pagesの仕様でdocsディレクトリに作成されるhtmlを公開することになるため、ビルドの向き先をdocsにする

.. code-block:: shell

	BUILDDIR      = docs

.. code-block:: shell

	#	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)
		$(SPHINXBUILD) -b html $(SOURCEDIR) $(BUILDDIR)
	
自動ビルドを実行するコマンドを定義する

.. code-block:: shell

	livehtml:
    	sphinx-autobuild -b html $(SOURCEDIR) $(BUILDDIR)

3.git設定
--------


4.github pagesの公開設定
--------

reference
========

* `Sphinx の使い方 <https://zenn.dev/y_mrok/books/sphinx-no-tsukaikata/viewer/chapter1>`_
* `sphinx-autobuildで再ビルド、ブラウザの再リロードの手間を省いてSphinx文書をサクサク作成 <https://qiita.com/mikanbako/items/28a3fc5d1da42939f941>`_
