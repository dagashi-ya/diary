# API GatewayとLambdaを使ったサーバレスSpringアプリケーション(2)

## 前提
以下ページを参考にAPI GatewayとLambdaを使用したspringアプリケーションを作成する。  
[API GatewayとLambdaを使ったサーバレスSpringアプリケーション(2)](https://news.mynavi.jp/techplus/article/techp4316/)  
基本的に、上記のページをトレースするが、学習のため本ページにまとめる。

## サーバレス開発 - Spring Cloud Funtionとは
- サーバレス開発とは、サーバや実行環境をほぼ意識することなく、最少限度のプログラムを用いて、アプリケーション実行環境を構築する開発方法を指している
- AWSでは、リクエストを受け付ける(APサーバの役割を代替する)API Gatewayと、アプリケーションの実行ランタイムを提供するAWS Lamdaを用いて、標準的なサーバレス開発を実現できる
	- API Gateway
	- AWS Lamda
- Spring Cloud Functionは関数型のビジネスロジック実行をベースとしたフレームワーク

## サーバレスアプリケーションの実装
### Mavenとは
- Javaプログラムをビルドするためのツール  
（C言語でいうと、makeのイメージ）
- 類似ツールに、AntやGradleがある
### intellijでSpringBootのmavenプロジェクトを作成するには
- SpringBootのmavenプロジェクトを作成
	- タブから[File]をクリック
	- [New]から[Project]をクリック
	- 左のタブから[Spring Initializr]を選択
	- [Next]をクリック
	- [Name]と[Location]を入力/選択
	- [Language]をJava、[Type]をMavenに設定
	- [Group]と[Artifact]を任意で設定
- 依存関係設定
	- pom.xmlの設定（以下の内容を追記）
	```
	<dependencies>
	    <dependency>
	        <groupId>org.springframework.cloud</groupId>
	        <artifactId>spring-cloud-function-web</artifactId>
	    </dependency>
	    <dependency>
	        <groupId>org.springframework.cloud</groupId>
	        <artifactId>spring-cloud-function-adapter-aws</artifactId>
	    </dependency>
	    <dependency>
	        <groupId>com.amazonaws</groupId>
	        <artifactId>aws-lambda-java-events</artifactId>
	    </dependency>
	    <dependency>
	        <groupId>com.amazonaws</groupId>
	        <artifactId>aws-lambda-java-core</artifactId>
	    </dependency>
	</dependencies>
	```

- 各クラスの実装
	- メイン実行クラス

	```
	package com.sample.tutorialawslambda;

	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.SpringBootApplication;

	@SpringBootApplication
	public class App {

		public static void main(String[] args) {
			SpringApplication.run(App.class, args);
		}

	}
	```
	SpringAppcaliton.run()メソッドを実行するmain()メソッドを持つ実行クラスに、＠SpringBootAppcalitonアノテーションを付与するだけで、SpringBootによりSpringFramework実行に必要な設定が自動実行されていきます。DB接続設定を追加するなど必要に応じてカスタマイズできますが、最低限、このクラスを作成しておくことが必要です。

	- Handlerクラス
	
	```
	package com.sample.tutorialawslambda;

	import org.springframework.cloud.function.adapter.aws.SpringBootApiGatewayRequestHandler;

	public class ApiGatewayEventHandler extends SpringBootApiGatewayRequestHandler {
	}
	```

	- ビジネスロジッククラス
	```
	package com.sample.tutorialawslambda.functions;

	import java.util.HashMap;
	import java.util.Map;
	import java.util.function.Function;

	import lombok.extern.slf4j.Slf4j;
	import com.sample.tutorialawslambda.model.Input;
	import org.springframework.messaging.Message;
	import org.springframework.messaging.MessageHeaders;

	@Slf4j
	public class ApiGatewayEventFunction implements Function<Message<Input>, Message<String>> {

	    @Override
	    public Message<String> apply(Message<Input> inputMessage) {
	        log.info("Message : " + inputMessage.getPayload().getTest());
	        return new Message<String>() {
	            @Override
	            public String getPayload() {
	                return "Complete!";
	            }

	            @Override
	            public MessageHeaders getHeaders() {
	                Map<String, Object> headers = new HashMap<>();
	                return new MessageHeaders(headers);
	            }
	        };
	    }
	}
	```
	
	- inputデータ（今回はinputクラスを作成）
	
	```
	package com.sample.tutorialawslambda.model;

	import lombok.AllArgsConstructor;
	import lombok.Builder;
	import lombok.Data;
	import lombok.NoArgsConstructor;

	import java.io.Serializable;

	@AllArgsConstructor
	@NoArgsConstructor
	@Builder
	@Data
	public class Input implements Serializable {

	    private String test;

	}
	```
	

- JAR形式に変換
	- JARファイルを作成するには、通常、Mavenでゴールをpackageに設定し、mvn packageコマンドを実行すれば良いのですが、AWS Lambdaへアップロードするには、これまで作成した3つのクラスに加えて、実行に必要な全ての依存JARライブラリを1つにまとめてアップロードする必要があります。
	- maven-shade-pluginの定義等を追加しておけば、Mavenのビルド時に必要なJARファイルを全て含めた形で1つのJAR形式のファイルを作成できます。
	
	```
	<build>
	    <plugins>
	        <plugin>
	            <groupId>org.apache.maven.plugins</groupId>
	            <artifactId>maven-deploy-plugin</artifactId>
	            <configuration>
	                <skip>true</skip>
	            </configuration>
	        </plugin>
	        <plugin>
	            <groupId>org.springframework.boot</groupId>
	            <artifactId>spring-boot-maven-plugin</artifactId>
	            <dependencies>
	                <dependency>
	                    <groupId>org.springframework.boot.experimental</groupId>
	                    <artifactId>spring-boot-thin-layout</artifactId>
	                </dependency>
	            </dependencies>
	        </plugin>
	        <plugin>
	            <groupId>org.apache.maven.plugins</groupId>
	            <artifactId>maven-shade-plugin</artifactId>
	            <configuration>
	                <createDependencyReducedPom>false</createDependencyReducedPom>
	                <shadedArtifactAttached>true</shadedArtifactAttached>
	                <shadedClassifierName>aws</shadedClassifierName>
	            </configuration>
	        </plugin>
	    </plugins>
	</build>
	```
	
	- maven packegeコマンド（pom.xmlが存在するディレクトリで以下コマンドを事項）
	```
	mvn package
	```
## エラー対処
### maven packegeコマンドを実行すると以下のエラーが発生(pom.xml周り)
> [ERROR] [ERROR] Some problems were encountered while processing the POMs:  
> [ERROR] 'dependencies.dependency.version' for com.amazonaws:aws-lambda-java-events:jar is missing. @ line 43, column 15  
> [ERROR] 'dependencies.dependency.version' for com.amazonaws:aws-lambda-java-core:jar is missing. @ line 47, column 15  

pom.xmlの内容を確認する

```
<dependency>
	<groupId>org.springframework.boot.experimental</groupId>
	<artifactId>spring-boot-thin-layout</artifactId>
	<version>1.0.9.RELEASE</version> #バージョンの指定が漏れていた
</dependency>
```
→バージョン指定したほうがよさそうだったので、バージョン指定

### maven packegeコマンドを実行すると以下のエラーが発生(コンパイルエラー)  
> [INFO] -------------------------------------------------------------  
> [ERROR] COMPILATION ERROR :   
> [INFO] -------------------------------------------------------------  
> [ERROR] /home/xxxxxxx/IdeaProjects/tutorial-aws-lambda/src/main/java/com/sample/tutorialawslambda/app.java:[3,32] org.springframework.boot.SpringApplicationにアクセスできません  
>   クラス・ファイル/home/xxxxxxx/.m2/repository/org/springframework/boot/spring-boot/3.0.6/spring-boot-3.0.6.jar(org/springframework/boot/SpringApplication.class)は不正です  
>     クラス・ファイルのバージョン61.0は不正です。52.0である必要があります  
>     削除するか、クラスパスの正しいサブディレクトリにあるかを確認してください。  

→spring bootのバージョンが高かったので、ダウングレードして解決


## 疑問点
- pom.xmlに入れるレポジトリを選定する方法について
- Spring Milestonesの用途は？


## reference
- [IntelliJでSpringFramework(MVC)のmavenプロジェクトHelloWorld(2020/3)](https://qiita.com/kasa_le/items/6aaf17823db67c951fb0)