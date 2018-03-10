# 入門AWS Lv.1

## 対象者

* AWSに全く触ったことがない人
* IT/システム開発の基礎知識を持つ人

## 目標

AWSにおいて最も基本的なサービス郡の概要を知り、実際に使ってみることで、  
AWSってそんなに難しくないということを知る

## 本課題で触れられるAWSのサービス

* IAM
* VPC
* EC2/EBS
* S3
* RDS
* ELB
* CloudFront
* Route53
* CloudWatch


## 手順

以下の課題をこなして、AWS

IAM
 - IAMユーザー作成
 - MFA設定
 - IAMロール作成
 - IAMポリシーの作成(余裕があったら)
 - 設定した権限が効いているかの確認
- VPC
 - VPCアドレス帯設計
 - サブネット設計
 - ルートテーブル構築
 - IGW構築
 - セキュリティグループ(mmmプロキシからの22/80のみ開ける)
- EC2/EBS
 - EC2セットアップ(上記で作ったパブリックサブネットにデプロイ)
 - SSHログイン
 - scpで【自主規制】動画をEC2に持っていく
 - IAMロール経由でS3に【自主規制】動画をアップロード
- S3
 - アップロードされた【Slack社の規約に抵触するため削除】動画を性的ホスティング
- RDS
 - インスタンス作成(Aurora最新)
 - プライベートサブネットにデプロイ
- ELB
 - EC2もう1台構築
 - EC2のどっかにindex.html作成(中身はHelloWorldでOK)(vimでやること)
 - aptでnginxインストール
 - nginx起動し、特定のエンドポイントでindex.htmlを返す
 - ローカルからHelloWorldが返ることを確認
 - nginxのログを確認し、アクセスが来ていることを確認
 - CLB構築、EC2 2台を登録
- Cloud Front
 - S3から【Slack社の規約に抵触するため削除】動画を配信
 - CloudFrontのCNAMEにアクセスして【Slack社の規約に抵触するため削除】動画が見られることを確認
- Route53
 - CFのエンドポイントにxxx.mmmcorp.co.jpドメインをつけて、ドメイン名でアクセスできることを確認(ALIASレコードでやる)
- CloudWatch
 - CloudWatchメトリクスから、EC2/RDS/ELBの数値を確認

## ライセンス

MIT
