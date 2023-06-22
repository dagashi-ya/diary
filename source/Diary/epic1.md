
# マイクロサービスにおけるCI/CDのノウハウ整理
## 取り組みの目的

近年、マイクロサービスアーキテクチャに基づいた新規サービスの開発が活発に行われており、AWS等のクラウド上でサービスを構築されることが多い。そこでは、複数の独立したサービスが協調動作し、一つの大規模なシステムを形成するという新たなパラダイムが見受けられる。

マイクロサービスの利点の一つとして、それぞれのサービスが独立しているために、新機能のリリースや既存機能の修正が容易であるという特性である。これにより、開発チームは週次、あるいは隔週といった頻度で高速にリリースを行うことが可能となる。

しかし、高頻度のリリースは課題もあり、具体的にはリリースの頻度が高まるほど、それぞれのリリースで新たな障害が発生する可能性が増大するという問題がある。リリース頻度を低減することで対応すると、マイクロサービスの利点を十分に活かすことができずビジネススピードも低減してしまう。

したがって、リリースの頻度を維持しつつ、サービスの品質を確保するためには、CI/CDという開発プロセスの中でマイクロサービスを適切に運用するための専門的な知識やノウハウが必要となっている。

## 取り組み内容

### チュートリアル
1. awsのチュートリアル

### 仕様調査
1. マイクロサービスで使用されるAWSの仕様調査
2. CI/CDで使われるツールの仕様調査
3. アプリ資材のリリース方式の整理

### サービスの実装
1. マイクロサービスのアプリ実装
2. マイクロサービスのインフラ構築
3. ②のIaC化（want）

### パイプラインの実装
1. マイクロサービスのアプリのCI/CD
2. マイクロサービスのインフラのCI/CD（want）