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

## Services you learn with this curriculum

- IAM
- VPC
- EC2/EBS
- RDS
- ELB
- Route53
- S3
- CloudFront
- CloudWatch

# Steps

## Step 1: Prepare privilege

### 1-1: Create IAM policy

- Create a IAM policy for this curriculum
- Privilege must be minimum (or simply `IAM Full Access`)
- When you have done Step 1, please fix it to `Administrator Access`

###1-2: Create IAM group

- Create a IAM group for this curriculum
- Attach IAM policy that you have created to this group

### 1-3: Create IAM user

- Create a IAM user for this curriculum using password
 - with Access key, Secret key
- Attach this user to IAM group that you have created

### 1-4: Configure MFA

- Study about MFA
- Configure MFA to IAM user you have created with Google Authenticator(or Authy)

### 1-5: Check privilege

- Install AWS CLI into your local environment with pip
- Check privilege with following command:
 - `aws s3 ls` -> Must not do (it will show `permission denied`)
 - `aws iam list-groups` -> Must do (it will show iam groups)

## Step 2: Prepare network

### 2-1: Create VPC

- Study about valid range of IP address for AWS VPC
- Create VPC with any CIDR

### 2-2: Create subnet

- Create 2 subnets into VPC (one is `SubnetA`, the other is `SubnetB`)
- SubnetA and SubnetB must be placed different AZ from each other
- Each subnet must have only 128 IP addresses

### 2-3: Create public subnet

- Study about what `Public subnet` means in AWS
- Make SubnetA `Public subnet`

### 2-4: Create private subnet

#### 2-4-1: Create NAT gateway

- Study what NAT is
- Study what NAT Gateway is
- Create NAT Gateway into Subnet A

#### 2-4-2: Create private subnet

- Study what `Private subnet` means in AWS
- Make SubnetB `Private subnet` with NAT gateway

### 2-5: IP Address of Subnet

- 5 IP Address per subnet cannot be used. Study why.

### 2-6: Create security group

- Create security group into VPC
- Inbound traffic must be authorized from only your local IP address
- Outbound traffic must be globally opened

## Step 3: Create virtual server

### 3-1: Create EC2

- Study about diferrence between EC2 instance types
- Study what `AZ` is
- Create EC2 at SubnetA like this:
 - OS: Ubuntu LTS
 - Storage: 50GB
 - Instance type: t2.small
 - Use tag to name instance
 - Attach security group you have created at step 2-6
 - Use userdata to install nginx (any version is ok)
 - Create key pair

### 3-2: Login EC2

#### 3-2-1:  Check server spec
- Log in EC2 by SSH
- Do `nginx -v` to make sure nginx is installed
- Do `df -h` to make sure storage is 50GB
- Do `cat /etc/lsb-release` to make sure linux distribution is Ubuntu

#### 3-2-2: Use nginx
- Create `index.html` like below and deliver it to internet with nginx

index.html
```
Hello World! by EC2a
```

- Access and see above index.html with your web browser

## Step 4: Create database

### 4-1: Create RDS
- Study what `Multi-AZ` is
- Study what `failover` is
- Create RDS at SubnetB like this:
 - Security group: Create new security group and allow traffic only from EC2 you have created
 - MySQL5.6
 - instance type: db.t2.medium
 - Multi-AZ: No
 - master user: root
 - master user password: LzXyDQbYYfTn
 - public access: No
 - name of database: curriculum_db
 - encryption: disables

### 4-2: Use RDS

- Login RDS from EC2
- Create new database user
- Import [curriculum_db.dump](https://github.com/ygnmhdtt/AWS_Lv1/blob/master/curriculum_db.dump) into database by new user

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
