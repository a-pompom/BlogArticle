# 概要

AWSに入門する土台をつくるために、AWS CLIの最初の一歩を踏み出します。

## ゴール

AWS CLIを動かす環境をつくり、Hello WorldがてらVPCを構築することを目指します。

## 前提

```bash
$ aws --version
aws-cli/2.17.42 Python/3.11.9 Windows/10 exe/AMD64
```

## 環境構築

ここでは、AWS CLIをインストールし、動かせる状態をつくっていきます。

### インストール

[手順](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)に従ってAWS CLIを導入します。
Windowsだと[Scoop](https://scoop.sh/)を使うとサクッと構築できました。

#### 疎通確認

`aws --version`コマンドから、インストールに成功したか確認しておきます。

```bash
$ aws --version
aws-cli/2.17.42 Python/3.11.9 Windows/10 exe/AMD64
```

無事インストールに成功したので、自分が所有するAWS環境を操作するために認証しておきます。

### 認証

ここでは、公式にて推奨されているIAM Identity Centerを使って認証していきます。
[参考](https://docs.aws.amazon.com/cli/v1/userguide/cli-authentication-user.html)

とはいえ、AWSに入門したてで理解の浅い状態なので、ひとまずは手順を解説した記事を参考リンクとして載せておくに留めておきます。
(いずれIAMにも入門したい...。)
[参考](https://dev.classmethod.jp/articles/aws-cli-for-iam-identity-center-sso/)

#### トークン取得

諸々設定が終わったら、実際に認証してみます。

最初に、`aws configure sso`コマンドでクレデンシャルを取得するための設定を登録しておきます。
[参考](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/configure/sso.html)

```bash
$ aws configure sso
SSO session name (Recommended): SESSION_NAME
SSO start URL [None]: PORTAL_URL
SSO region [None]: ap-northeast-1
SSO registration scopes [sso:account:access]:
Attempting to automatically open the SSO authorization page in your default browser.
...
```

そして、`aws sso login`コマンドでトークンを取得します。以降はトークンで認証し、AWSのリソースを操作していきます。
[参考](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sso/login.html)

```bash
$ aws sso login --profile PROFILE
```

`PROFILE`オプションには、認証プロファイル名を指定しておきます。認証プロファイル名は、`~/.aws/config`ファイルの`[profile]`の項から参照・編集できます。

最後に、認証できているか動作確認しておきます。公式で推奨されているコマンドを叩いてみます。

```bash
$ aws sts get-caller-identity --profile PROFILE
{
    "Result": "SomeResult"
}
```

なんらかの結果が返ってきたことから、成功していそうです。

## VPCの構築

AWS CLIを使う準備が整いました。ここではHello WorldがてらVPCをつくってみます。

### ドキュメント

AWS CLIからリソースを操作するときは、コマンドの使い方を理解することが重要です。コマンドの詳細は公式ドキュメントにまとまっているので、公式ドキュメントをどうやって読めば良いのか、簡単な地図をつくっておきます。

### リファレンスの構造を読み解く

VPC作成コマンドを例に見てみます。
[参考](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/create-vpc.html)

#### Description

リソース作成系のコマンドは、操作対象のリソースの補足説明が主に書かれています。具体的には、コマンドのオプションと関わりが深いAWS公式ドキュメントへのリンクが載っていることが多いようです。
AWS公式ドキュメントは膨大でどこから見れば良いか迷子になりやすいと感じています。
ですので、こうやって必要最小限のリンクだけまとまっていると、入門にはちょうど良いボリュームになりそうです。

#### Synopsis

Linuxコマンドのマニュアルのように、使えるオプションがまとまっています。`[]`で囲まれたものは任意なので、まずは必須オプションだけ押さえておきたいです。

#### Options

Synopsisの項で見たオプションの詳細説明が書かれています。

#### Examples

ユースケース別のコマンドの実行例が載っています。あわせて、コマンドを実行した結果操作されたリソースの情報がOutputとして出力されています。
ドキュメントを見るときは、最初にこの辺りを眺めておけば、コマンドが何をしているのかイメージを掴むのに良さそうです。

### コマンド実行

リファレンスに従い、実際にVPCをつくってみます。
今回はHello World的な位置づけなので、とりあえずコマンド例をもとに見よう見まねでコマンドを叩いてみます。

```bash
$ aws ec2 create-vpc \
     --cidr-block '10.0.0.0/16' \
     --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=HelloAWSCLI}]' \
     --profile 'PROFILE'

# output example
{
    "Vpc": {
        "CidrBlock": "10.0.0.0/16",
        "DhcpOptionsId": "dopt-0b115d2f27d7dd260",
        "State": "pending",
        "VpcId": "VPC_ID",
        "OwnerId": "OWNER_ID",
        "InstanceTenancy": "default",
        "Ipv6CidrBlockAssociationSet": [],
        "CidrBlockAssociationSet": [
            {
                "AssociationId": "vpc-cidr-assoc-02df3e0e0aea59f92",
                "CidrBlock": "10.0.0.0/16",
                "CidrBlockState": {
                    "State": "associated"
                }
            }
        ],
        "IsDefault": false,
        "Tags": [
            {
                "Key": "Name",
                "Value": "HelloAWSCLI"
            }
        ]
    }
}
```

出力のJSONにはVPCの特徴を掴むのに良さそうな情報がまとまっています。CLIはGitやDocker・Linuxコマンドなどで慣れ親しんだインタフェースで操作できるので、AWSへ入門するのにも良さそうです。

コマンドを実行すると、VPCが作成されたことをコンソール上でも確認できました。

![VPC作成](https://s3.ap-northeast-1.amazonaws.com/a-pompom.net.images/2024/09/aws-cli-hello-world-create-vpc.PNG)

### 後片付け

今後のことを考えると、実験用につくったリソースで不要な料金が発生しないよう、後片付けを習慣づけておきたいです。
ということで、つくったVPCを消しておきます。

VPCは、`aws ec2 delete-vpc`コマンドで削除することができます。
[参考](https://awscli.amazonaws.com/v2/documentation/api/2.1.21/reference/ec2/delete-vpc.html)

削除対象のVPCのIDを明示してコマンドを実行します。

```bash
aws ec2 delete-vpc \
  --vpc-id 'VPC_ID' \
  --profile 'PROFILE'
```

コンソールにて、VPCが削除されたことを確認しました。

![VPC削除](https://s3.ap-northeast-1.amazonaws.com/a-pompom.net.images/2024/09/aws-cli-hello-world-delete-vpc.PNG)

## まとめ

本記事では、AWS CLIに入門してみました。CLIは思ったよりもとっつきやすいと感じたので、ここから普段触っているリソースを構築してみて、AWSへの理解を深めていきたいです。
