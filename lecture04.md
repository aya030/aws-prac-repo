# AWSフルコース 第4回課題

 ## やったこと
 - AWS上に新しく VPC を作成
 - EC2 と RDS を構築する
 - EC2 から RDS へ接続をし、正常であることを確認


## VPC・サブネット・インターネットゲートウェイの設定
#### 1.  VPCの作成
  - VPC作成画面を表示
  - 名前タグ
      - 任意の名前を入力
  - IPv4 CIDR ブロック
      - IPv4 CIDR の手動入力 : 10.0.0.0/16
  - IPv6 CIDR ブロック
      - Pv6 CIDR ブロックなし
  - `VPCを作成`

#### 2. サブネットの作成
  - サブネット作成画面を表示
  - VPC
      - VPCIDに作成したVPCを選択
  - サブネットの設定
      - サブネット名を入力 
      - IPv4 CIDRブロック : 10.0.0.0/24
      - `サブネットを作成`

#### 3. ルートテーブルの作成
  - ルートテーブル作成画面を表示
  - 名前
      - 任意の名前を入力
  - VPC
      - 作成したVPCを選択
  - `ルートテーブルを作成`

#### 4. インターネットゲートウェイ(IGW)作成
  - インターネットゲートウェイ作成画面を表示
  - 名前タグ
      - 任意の名前を入力
  - `インターネットゲートウェイを作成`

#### 5.  VPCにアタッチ
  - VPCにアタッチ画面を表示
  - VPCに作成したVPCを選択する
  - `インターネットゲートウェイのアタッチ`

#### 6. ルートテーブルに先ほど作成したIGWをルートの編集から追加
  - 下段のルート→ルートの編集→ルートを追加
  - 送信先 : 0.0.0.0/0
  - ターゲット : 作成したインターネットゲートウェイ
  - `変更を保存`

#### 7. サブネットにルートテーブルを紐づけ
  - サブネット画面を表示
  - 作成したサブネットを選択
  - 下段の”ルートテーブル”→ルートテーブルの関連付けを編集
  - ルートテーブル ID
       - 作成したルートテーブルを選択
  - `保存`

#### 8. VPCの作成完了
<img width="600" alt="VPC作成" src="https://user-images.githubusercontent.com/101713134/236634114-145522e4-a6a2-4182-bc52-dd2d986efe9a.png">


## EC2の設定
  - コンソールからEC2の管理画面を表示
  - `インスタンスを起動`を選択
  - 名前
      - 任意の名前を入力
  - AMI
      - `Amazon Linux 2 AMI`を選択(無料枠)
  - インスタンスタイプ
      -   `t2.micro`を選択(無料枠)
  - キーペアを作成
  - キーペア名
      - 作成したキーペアを選択
  - ネットワーク設定
      - セキュリティグループを作成する
      - からのSSHトラフィックを許可する
      - 自分のIPを選択 
  - `EC2を起動`
  
## EC2起動方法
  - `ssh ec2-user@ec2-××-××-××-××.ap-northeast-1.compute.amazonaws.com -i アクセスキー`
  - 起動！
<img width="600" alt="EC2接続" src="https://user-images.githubusercontent.com/101713134/236633940-d0c4fbbd-a88e-4dc5-9e5c-6baae6658cea.png">

  - `exit`で起動終了
  
## 作成したEC2のVPC設定
<img width="600" alt="EC2インスタンス" src="https://user-images.githubusercontent.com/101713134/236634377-88e7700a-f485-4aed-8f05-1ddc9a6ab9c9.png">

## RDSの設定
#### 1. DB用のセキュリティグループの作成
 - EC2→セキュリティグループ→セキュリティグループの作成を押下
 - セキュリティグループ名
      - 任意の名前を入力
 - EC2があるVPCを選択
 - タイプ
      - MYSQL/Auroraを選択
 - ソース
      - EC2のセキュリティグループ
 - `セキュリティグループを作成`

#### 2. DB用のサブネットグループの作成
 - RDS→サブネットグループ→DB サブネットグループの作成
 - 名前・説明
      - 任意の名前を入力
 - VPC・サブネット
      - EC2があるところを選択
 - エラー①
      - `申し訳ありませんが、DB サブネットグループ db-subnetgroup の作成リクエストは失敗しました。
The DB subnet group doesn't meet Availability Zone (AZ) coverage requirement. Current AZ coverage: ap-northeast-1a. Add subnets to cover at least 2 AZs.`
      - サブネットをもう1つ作成する。サブネット設定にサブネットを2こ選択する
  - エラー②
      - `CIDR アドレスは既存のサブネット CIDR と重複しています: 10.0.0.0/24`
      - サブネットは①VPCのホストアドレス範囲内であること、②サブネット同士でホストアドレス範囲が重複しないことの2つの要件を満たす必要がある
      - 計算して、`10.0.1.0/24`を指定する
  
#### 3. データベースの作成
- マネジメントコンソールからRDS→データベースの選択
- エンジンのタイプ
     - MySQL
- DB インスタンス識別子
     - 任意の名前を入力
- SQLのパスワードを入力
- インスタンス
     - db.t2.micro
- ストレージ 
     - 20GB
- VPC
     - 作成したEC2を選択
- `データベースの作成`

#### 4. EC2からRDSに接続
- EC2にログイン
- mySQLをインストール(Amazon Linux 2 AMIのとき)
     - `mysql -u admin -p -h RDSのエンドポイントを張り付け`
- mySQLをインストール(Amazon Linux 2023 AMIのとき)
     - `sudo dnf -y localinstall https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm`
     - `sudo dnf -y install mysql mysql-community-client`
     - `sudo yum install mysql -y`
- エラー③
     - `ERROR 2003 (HY000): Can't connect to MySQL server on 〜〜 (110)`
     - RDSのセキュリティグループ(VPCセキュリティグループ→インバウンドルール)の設定を変更
     - EC2とRDSでVPC、セキュリティグループ、サブネット等があっているかを確認する
- 繋がった！
<img width="600" alt="EC2とRDSを接続" src="https://user-images.githubusercontent.com/101713134/236634778-6b8c05b6-eeaa-4868-bb42-a95d6d004f6d.png">

#### 5. 作成したRDSのVPC設定
<img width="600" alt="RDS設定" src="https://user-images.githubusercontent.com/101713134/236634594-3cf01083-d1dd-4200-a084-db0130ce27f9.png">

## ハマったポイント
- EC2とRDSで違う設定したセキュリティグループになっていたことでセキュリティグループの設定を見直してもエラーで繋がらなかった。
      - EC2とRDSでVPC、セキュリティグループ、サブネット等をちゃんと合わす必要がある。

