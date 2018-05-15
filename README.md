# AWS Begginers Challange

# About

This is a curriculum for learning basic knowledge of AWS services that are frequently used.

# License

MIT.

# In Japanese / 日本語版

[README_ja.md](https://github.com/ygnmhdtt/AWS_Lv1/blob/master/README_ja.md) is written in Japanese.(The contents are the same.)

# Abstraction

## Purpose

Purpose of this curriculum is learning AWS basic services for AWS begginers.
So, you shouldn't get on next step if you could not master it. Please read AWS official document or ask other skilled person.

## How to get on steps

- Steps are 1 to 9. You should do in order.
- Please follow these rule about IAM user:
 - When step 1 : You should use IAM user which you already created
 - After step 1: You should use IAM user which you created at step 1
- Please do at your living region

## 本カリキュラムで習得するAWSサービス

本カリキュラムでは、以下のAWSマネージドサービスの基礎について習得します。

- IAM
- VPC
- EC2/EBS
- RDS
- ELB
- Route53
- S3
- CloudFront
- CloudWatch

# 課題ステップ

## ステップ1: 権限の整備をしよう

### 1-1: IAMポリシーの作成

- 本カリキュラム用のIAMポリシーを作成してください。
- 権限は、本ステップで必要とする最低限にしてください。(IAMフルアクセスのみ)
- また、ステップ1完了時にAdministrator Accessを付与してください。

###1-2: IAMグループの作成

- 本カリキュラム用のIAMグループを作成してください。
- 1-1で作成したIAMポリシーをアタッチしてください。

### 1-3: IAMユーザーの作成

- 本カリキュラム用のIAMユーザーを作成してください。
 - アクセスキー・シークレットキーも併せて発行してください。
- 1-2で作成したIAMグループに所属させてください。

### 1-4: MFAの設定

- 作成したIAMユーザーにMFAの設定を行ってください。

### 1-5: 権限の確認

- AWS CLIをローカル環境にインストールしてください。
- AWS CLIを使って、作成したIAMユーザーの権限が絞られていることを確認してください。

## ステップ2: ネットワークを作成しよう

### 2-1: VPCの作成

-  VPCに指定できるIPアドレスの範囲について調査してください。
- 任意のアドレスでVPCを作成してください。

### 2-2: サブネットの作成

- VPC内に、サブネットをふたつ作ってください。(それぞれサブネットA、サブネットBと呼称します)
- サブネットAとサブネットBは別AZに作成してください。
- サブネットは、利用可能なIPアドレスが128個になるように作ってください。

### 2-3: パブリックサブネット整備

- AWSにおいてパブリックサブネットとは何を意味するか調査してください。
- 作成したサブネットAをパブリックサブネットにしてください。

### 2-4: プライベートサブネット整備

#### 2-4-1: NATゲートウェイ作成

- NATとはなにかを調査してください。
- NATゲートウェイとはなにかを調査してください。
- サブネットAにNATゲートウェイを作成してください。

#### 2-4-2: プライベートサブネット構築

- AWSにおいてプライベートサブネットとは何を意味するかを調査してください。
- 作成したNATゲートウェイを使って、サブネットBをプライベートサブネットにしてください。

### 2-5: サブネットのIPアドレス

- サブネット内のIPアドレスのうち、5IPは使用できません。理由を調査してください。

### 2-6: セキュリティグループ作成

- VPCにセキュリティグループを作成してください。
- インバウンドはマイIPからのSSH接続、HTTP接続のみとしてください。
- アウトバウンドはグローバルに開放してください。

## ステップ3: 仮想サーバーを作成しよう

### 3-1: EC2作成

- EC2のインスタンスタイプごとの違いについて調査してください。
- AZとはなにか？について調査してください。
- 以下の要件のEC2をサブネットAに作成してください。
 - OS: Ubuntu LTS
 - ストレージ: 50GB(エフェメラルストレージは初期設定のままとする)
 - インスタンスタイプ: t2.small
 - タã°を活用して、カリキュラム用ということがわかるような名前をつけてください
 - 2-6で作成したセキュリティグループを付けてください
 - 起動時にnginxがインストールされるようにしてください(ユーザーデータを使う)(バージョンは任意)
 - キーペアは個別で発行してください

### 3-2: EC2にログイン

#### 3-2-1:  サーバースペックの確認
- 作成したEC2にSSHログインしてください。
- `nginx -v` でnginxがインストールされていることを確認してください。
- `df -h` でストレージが50GBなことを確認してください。
- `cat /etc/lsb-release` でディストリビューションがUbuntuなことを確認してください。

#### 3-2-2: nginxの活用
- インストールされているnginxを使って、以下のindex.htmlを配信してください。

index.html
```
Hello World! by EC2a
```

- ブラウザから、上記のindex.htmlにアクセスしてください。

## ステップ4: データベースを作成しよう

### 4-1: RDS作成
- マルチAZとはなにか？について調査してください。
- フェイルオーバーとはなにか？について調査してください。
- 以下の要件のRDSをサブネットBに作成してください。
 - セキュリティグループ: 作成したEC2からのみ開放する(新規にSGを作成してください)
 - MySQL互換(バージョンは任意)
 - インスタンスタイプ: db.t2.medium
 -マルチAZ: いいえ
 - マスターユーザー: root
 - マスターユーザーパスワード: LzXyDQbYYfTn
 - パブリックアクセシビリティ: いいえ
 - データベースの名前: curriculum_db
 - 暗号化: 無効

### 4-2: RDS活用

- 作成したEC2からRDSにログインしてください。
- データベースにユーザーを新たに作成してください。
- 作成してユーザーを使って、データベースに[curriculum_db.dump](https://github.com/ygnmhdtt/AWS_Lv1/blob/master/curriculum_db.dump)をimportしてください。

## ステップ5: 負荷分散しよう

### 5-1: EC2作成

- すでに作成したEC2のAMIを取得し、もう1台EC2を作成してください。(すでに作成されたものをEC2a、今回作るものをEC2bと呼称します)
- EC2bにインストールされているnginxを使って、以下のindex.htmlを配信してください。

index.html
```
Hello World! by EC2b
```

- ブラウザから、上記のindex.htmlにアクセスしてください。

###5-2: ALB作成

- サーバの負荷分散について調査してください。
- ELBとはなにか？について調査してください。
- CLB、ALB、NLBの違いについて調査してください。
- 以下の要件のALBを作成してください。
 - スキーム: インターネット向け
 - IPアドレスタイプ: IPv4
 - リスナー: HTTP
 - ターゲットグループにはEC2a、EC2bを登録
 - セキュリティグループはマイIPからのHTTP接続のみをインバウンド許可してください

### 5-3: ALB活用

- 正しく負荷分散されていれば、

```
Hello World! by EC2a
```

と

```
Hello World! by EC2b
```

の両方が配信されます。ブラウザからこのことを確認してください。

### 5-4: HTTPS化

作成したALBを、HTTPSでアクセスできるようにしてください。

## ステップ6: S3を使ってみよう

### 6-1: コンテンツアップロード

- S3が `イレブン・ナイン` と呼ばれる理由を調査してください。
- バケットポリシー・ACLの違いについて調査してください。
- S3の結果整合性について調査してください。
- S3にcurriculumバケットを作成してください。
- 作成したバケットに以下のindex.htmlをアップロードしてください。

index.html

```
Hello World! by S3
```

### 6-2: 静的サイトホスティング

- 作成したバケットで静的サイトホスティングを有効化してください。
- ブラウザからアクセスし、作成したindex.htmlが返ってくることを確認してください。
- 静的サイトホスティングするメリットを調査してください。

##ステップ7: CDNを使ってみよう

### 7-1: ディストリビューション作成

- CloudFront(CDN)を使用するメリット・その仕組みを調査してください。
- 以下の要件でCloudFrontディストリビューションを作成してください。
 - オリジン: 作成したS3バケット
 - オリジンパス: 作成したindex.html
 - Restrict Bucket Access: No
 - キャッシュは1時間(オリジン側は一旦考えなくて大丈夫です)
 - HTTPS化はしない

### 7-2: ディストリビューション活用

- 作成したディストリビューションにアクセスし、index.htmlが返ってくることを確認してください。

## ステップ8: 独自ドメインを使ってみよう

### 8-1: ELB用サブドメイン作成

- Route53について調査してください。
- DNSレコードについて調査してください。
- ALIASレコードについて調査してください。
- TTLについて調査してください。
- 適当なドメインを作成し、ELBへのALIASレコードを作成してください。
- DNSの浸透について調査してください。
- 作成したドメインにブラウザからアクセスして、ELBからのデータが返ってくることを確認してください。

### 8-2: CloudFront用サブドメイン作成

- 8-1で作成したドメインと別のドメインを作成し、CLoudFrontへのALIASレコードを作成してください。
- 作成したドメインにブラウザからアクセスして、CloudFrontからのデータが返ってくることを確認してください。

## ステップ9: メトリクスを見てみよう

### 9-1: EC2メトリクス

- 作成したEC2について、以下のメトリクスをCloudWatchから確認してください。(それぞれが何を意味するかも調査してください)
 - CPUUtilization
 - CPUCreditBalance
 - NetworkIn
 - NetworkOut
 - DiskReadOps
 - DiskWriteOps
 - NetworkPacketsIn
 - NetworkPacketsOut
- EC2メトリクスではメモリ使用率を計測できません。どうすれば計測できるかを調査してください。

### 9-2: RDSメトリクス

- 作成したRDSについて、以下のメトリクスをCloudWatchから確認してください。(それぞれが何を意味するかも調査してください)
 - CPUUtilization
 - FreeableMemory
 - SwapUsage
 - DatabaseConnections
 - BurstBalance

## ステップ10: 後片付けをしよう

- 今回作成した全てのAWSリソースにかかったコストをコストエクスプローラから確認してください。
- 作成した全てのリソースを削除してください :cry:  ( **以下の番外編に進む方は、削除しないでください!!!!** )

# あとがき
お疲れさまでした。 あなたはAWS入門カリキュラム基礎編を一通り完遂しました!!
AWSは非常にサービスが多く、全てを把握するのは非常に難しいですが、
AWSでよく使うサービスについての基礎知識は学べたかと思います。

余裕があれば、以下の `番外編` にもチャレンジしてみてください!!!

### 番外編

ここからはオプションなため、実施するかどうかは自分で決めてください。

- SSLターミネーションについて調査しよう
- CloudFrontもHTTPS化してみよう
- スポットインスタンスを使ってみよう
- RDSをマルチAZにして、強制フェイルオーバーしてみよう
- EC2にEIPをつけてみよう
- ELBでweighted round robinしてみよう
