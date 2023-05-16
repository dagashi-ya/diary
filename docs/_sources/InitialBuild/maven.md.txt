# Maven設定手順
## 前提
WorkSpaces（Amazon Linux release 2 (Karoo)）

## 概要
Amazon Linux 2にMavenをインストールしてmvnコマンドが使えるようにする

## 手順
### 1.JDK8をインストール
WorkSpacesはデフォルトでjava11が入っているようなので、java8をインストールする。
バージョンを切り替えたい場合は、alternativesコマンドを実行する。

```
sudo yum install -y java-1.8.0-openjdk-devel.x86_64
sudo alternatives --config java
java -version
```
### 2.Mavenをダウンロード
```
cd /usr/local/lib/
sudo wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
```

### 3.Mavenを展開して配置する
```
sudo tar -xzvf apache-maven-3.8.8-bin.tar.gz 
sudo mv apache-maven-3.8.8 /opt/
cd /opt/
sudo ln -s /opt/apache-maven-3.8.8 apache-maven
ls -l
```

### 4.MavenへのPATHを追加する
```
cd
vi .bash_profile
```
以下の内容を追記する
```
MVN_HOME=/opt/apache-maven
PATH=$MVN_HOME/bin:$PATH:$HOME/.local/bin:$HOME/bin
```
設定を反映する
```
source .bash_profile
```

### 5.mvnコマンドが使えることを確認する
```
mvn --version
```

## reference
[Downloading Apache Maven 3.9.2](https://maven.apache.org/download.cgi)  
[【AWS EC2】Amazon Linux 2にMavenをインストールする方法](https://qiita.com/tamorieeeen/items/bcdf9729a5e9194c5d20)