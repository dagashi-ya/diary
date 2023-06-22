# AWS ALB (Application Load Balancer)

## 概要

AWS ALB (Application Load Balancer) は、AWSが提供するフルマネージド型のロードバランサであり、HTTPとHTTPSのトラフィックを効率的に分散することができます。

## 主な特徴

- **レイヤ7のロードバランシング**： ALBは、ネットワークのトランスポート層（レイヤ4）ではなく、アプリケーション層（レイヤ7）でトラフィックをロードバランシングすることができます。これにより、特定のURLパス（例えば、/imagesや/api）やHTTPヘッダー、リクエストメソッド（GET、POSTなど）に基づいてリクエストをルーティングすることができます。

- **コンテナベースのアプリケーションのサポート**： ALBは、1つのEC2インスタンス上で複数のコンテナを稼働させることができます。それぞれのコンテナは独自のポートを使用できますので、特にDockerなどのコンテナ技術を使用したマイクロサービスアーキテクチャに適しています。

- **自動スケーリング**： ALBは、トラフィックの増減に応じて自動的にスケーリングします。これにより、リクエストのピーク時でもパフォーマンスを維持することができます。

- **ウェブソケットおよびHTTP/2プロトコルのサポート**： ALBは、ウェブソケットとHTTP/2の両方のプロトコルをサポートしています。これにより、リアルタイムの双方向通信や効率的な通信が可能となります。

- **統合されたSSL/TLS証明書の管理とサポート**： ALBは、SSL/TLS証明書の管理を簡素化します。AWS Certificate Manager（ACM）で証明書を発行・管理し、ALBに関連付けることで、セキュアなSSL/TLS通信を確立することができます。

- **ヘルスチェック**： ALBは、対象となるEC2インスタンスまたはコンテナが正常に稼働しているかを定期的にチェックし、問題がある場合はそのインスタンスやコンテナへのルーティングを停止します。これにより、サービスの可用性を向上させることができます。

## 主な用途

- Webアプリケーションの負荷分散とスケーリング
- マイクロサービスアーキテクチャにおけるコンテナのロードバランシング
- ウェブソケットを使用するリアルタイムアプリケーションのサポート
- セキュアなHTTPS通信の提供
- 大規模なトラフィックの管理と制御

## AWS ALBの設定手順

1. AWS Management Consoleにログインし、「EC2」を選択する。

2. EC2ダッシュボードで、「Load Balancers」を選択する。

3. 「Create Load Balancer」をクリックする。

4. 「Load Balancer Type」で「Application Load Balancer」を選択し、「Create」をクリックする。

5. 「Configure Load Balancer」セクションで以下の設定を行う：
   - 「Load Balancer name」に適切な名前を入力する。
   - 「Scheme」でパブリックまたはインターナルのどちらかを選択する。
   - 「Listeners」セクションで、必要なポートとプロトコルを指定する。

6. 「Availability Zones」セクションで、ALBを配置するアベイラビリティーゾーンを選択する。

7. 「Security settings」セクションで、必要なセキュリティグループを選択する。

8. 「Configure Routing」セクションで以下の設定を行う：
   - 「Target group」で新しいターゲットグループを作成するか、既存のターゲットグループを選択する。
   - 必要に応じて「Path-based routing」や「Host-based routing」を構成する。

9. 「Register Targets」セクションで、ターゲットグループにターゲット（インスタンス、コンテナなど）を登録する。

10. 「Review」セクションで設定内容を確認し、「Create」をクリックする。

11. ALBが作成されたら、必要に応じてDNS名を使用してアプリケーションにアクセスできることを確認する。

以上で、AWS Management Consoleを使用してALBの設定が完了する。設定内容に応じてALBがトラフィックを受け取り、ターゲットグループに転送するように構成される。

| パラメータ名 | 説明 |
|:-----------|:------------|
| Name | 作成するALBの名称。この名前は、AWS内で一意でなければならない。 |
| Scheme | ALBの公開範囲を指定する。`internet-facing`はインターネットからアクセス可能なALBを、`internal`はVPC内部からのみアクセス可能なALBを作成する。 |
| Listeners | ALBが接続要求を受け入れるためのポートとプロトコルを指定する。HTTPおよびHTTPSが選択可能である。 |
| Availability Zones | ALBを配置するVPC内のAvailability Zoneと、それぞれに関連付けられるサブネットを指定する。 |
| Security groups | ALBに関連付けるセキュリティグループを指定する。これにより、ALBへのインバウンドトラフィックの許可/拒否を制御する。 |
| Target group | ALBがトラフィックをルーティングする対象の一覧。このグループには、リクエストを処理するEC2インスタンスなどが含まれる。 |
| Health checks | ALBがターゲットグループのヘルスチェックを実行するための設定。例えば、チェックするパスや、成功と判定するHTTPステータスコードなどを設定する。 |