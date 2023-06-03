# AWS VPC (Virtual Private Cloud)

## 概要

AWSのVPCはユーザが定義する仮想的なプライベートネットワーク空間であり、AWSリソースをセキュアに起動するためのフレームワークを提供する。ユーザはこのネットワークのIPアドレス範囲を選択し、サブネットを作成し、セキュリティ設定やルートテーブルの定義などのネットワーク環境を完全に制御することができる。

## 主な特徴

- **セキュリティ**: VPCはネットワークアクセス制御リストとセキュリティグループを使用してインバウンドおよびアウトバウンドのトラフィックを制御する。
- **IPアドレス範囲**: VPC内のIPv4とIPv6のアドレス範囲をユーザ自身で選択することができる。
- **サブネット**: ユーザは1つ以上のサブネットを作成し、これらのサブネットをパブリック (インターネット接続可能) またはプライベート (インターネット接続不可) に設定することができる。
- **ルーティング**: ルートテーブルを用いてサブネット内のネットワークトラフィックのフローを制御することができる。
- **VPN接続**: VPCとオンプレミスネットワーク間にVPN接続を構築し、セキュアな接続を実現することが可能である。

## 主な用途

VPCは以下のような用途で広く利用されている。

- **EC2インスタンスの起動**: EC2インスタンスをVPC内に起動し、他のAWSサービスやインターネットとのやり取りをセキュアに行う。
- **RDSデータベースのホスティング**: VPC内にRDSデータベースを設定し、アクセス制御を強化する。
- **プライベートな通信**: オンプレミスネットワークとVPC間でVPN接続を構築し、プライベートな通信を実現する。

VPCはAWSにおけるネットワーキングの基本的な要素であり、セキュアなクラウドリソースのデプロイメントと管理に不可欠である。

## 基本的な設定方法

| ステップ | 内容 |
|--------|-----|
| 1.     | VPCの作成: AWSマネジメントコンソールから新しいVPCを作成し、適切なIPv4 CIDRブロックを設定する。 |
| 2.     | サブネットの作成: VPC内に一つ以上のサブネットを作成する。各サブネットにはVPCのIPアドレス範囲から一部を割り当てる。 |
| 3.     | インターネットゲートウェイの接続: VPCにインターネットゲートウェイをアタッチし、VPCのリソースがインターネットにアクセスできるようにする。 |
| 4.     | ルートテーブルの設定: パブリックサブネットのルートテーブルを設定し、インターネットゲートウェイへのルートを追加する。 |
| 5.     | セキュリティグループの設定: サーバのアクセスを制御するためのセキュリティグループを設定する。 |
| 6.     | ネットワークACLの設定: サブネットレベルのセキュリティを提供するネットワークACLを設定する。 |

## 用語説明
| 用語 | 説明 |
|------|-----|
| VPC (Virtual Private Cloud) | AWSの仮想的なプライベートネットワーク。AWSリソースをセキュアにローンチできる独自の定義された仮想ネットワーク空間。 |
| サブネット | VPC内のIPアドレス範囲の一部。サブネットを用いてVPC内のネットワークをセグメント化することができる。 |
| インターネットゲートウェイ | VPCとインターネットを接続するためのネットワークゲートウェイ。これにより、VPC内のリソースがインターネットと通信することができる。 |
| ルートテーブル | ネットワークトラフィックがどのようにルーティングされるかを制御するためのルールのセット。各サブネットはルートテーブルに関連付けられている。 |
| セキュリティグループ | インスタンスレベルでのインバウンドおよびアウトバウンドトラフィックを制御するための仮想ファイアウォール。|
| ネットワークACL (Access Control List) | サブネットレベルでのインバウンドおよびアウトバウンドトラフィックを制御するための仮想ファイアウォール。|

## terraformでのサンプル作成
以下に、指定された条件に基づいてVPCを作成するTerraformコードのサンプルを示します。このコードはAWS東京リージョン (ap-northeast-1) でVPC、インターネットゲートウェイ、サブネット、ルートテーブル、セキュリティグループ、およびネットワークACLを作成します。

```hcl
# Provider configuration
provider "aws" {
  region = "ap-northeast-1"  # 東京リージョンを選択
}

# Create a VPC
resource "aws_vpc" "example" {
  cidr_block = "10.0.0.0/16"  # VPCのCIDRブロックを設定
  enable_dns_support   = true  # DNSサポートを有効化
  enable_dns_hostnames = true  # DNSホスト名を有効化

  tags = {
    Name = "example"
  }
}

# Create an internet gateway and attach it to the VPC
resource "aws_internet_gateway" "example" {
  vpc_id = aws_vpc.example.id  # VPCへのインターネットゲートウェイを作成し接続

  tags = {
    Name = "example"
  }
}

# Create a subnet
resource "aws_subnet" "example" {
  vpc_id     = aws_vpc.example.id  # VPC内にサブネットを作成
  cidr_block = "10.0.1.0/24"  # サブネットのCIDRブロックを設定
  map_public_ip_on_launch = true  # 自動的にパブリックIPを割り当てる

  tags = {
    Name = "example"
  }
}

# Create a route table and a route
resource "aws_route_table" "example" {
  vpc_id = aws_vpc.example.id  # VPCにルートテーブルを作成

  route {
    cidr_block = "0.0.0.0/0"  # すべてのアドレスを対象とするルートを設定
    gateway_id = aws_internet_gateway.example.id  # インターネットゲートウェイをデフォルトゲートウェイとして設定
  }

  tags = {
    Name = "example"
  }
}

# Associate the subnet with the route table
resource "aws_route_table_association" "a" {
  subnet_id      = aws_subnet.example.id  # サブネットにルートテーブルを関連付け
  route_table_id = aws_route_table.example.id
}

# Create a security group
resource "aws_security_group" "example" {
  name        = "example"  # セキュリティグループを作成
  description = "Example security group"
  vpc_id      = aws_vpc.example.id

  ingress {
    from_port   = 0  # 入力トラフィックの設定
    to_port     = 0  
    protocol    = "-1"  
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0  # 出力トラフィックの設定
    to_port     = 0  
    protocol    = "-1"  
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "example"
  }
}

# Create a network ACL and associate it with the subnet
resource "aws_network_acl" "example" {
  vpc_id     = aws_vpc.example.id  # VPCにネットワークACLを作成
  subnet_ids = [aws_subnet.example.id]  # サブネットにネットワークACLを関連付け

  ingress {
    protocol   = "6"
    rule_no    = 100
    action     = "allow"
    cidr_block = "0.0.0.0/0"
    from_port  = 0
    to_port    = 65535
  }

  egress {
    protocol   = "6"
    rule_no    = 100
    action     = "allow"
    cidr_block = "0.0.0.0/0"
    from_port  = 0
    to_port    = 65535
  }

  tags = {
    Name = "example"
  }
}
\`\`\`
