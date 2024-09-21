# 概要

AWSに入門する土台をつくるためにAWS CLIの最初の一歩を踏み出します。

## ゴール

AWS CLIを動かす環境をつくり、Hello WorldがてらVPCを構築することを目指します。

## 前提

環境やAWS CLIのバージョンは以下の通りです。

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

IAMユーザと紐づくアクセスキーによる認証は、非推奨のようです。ここでは、推奨されているIAM Identity Centerを使って認証していきます。
[参考](https://docs.aws.amazon.com/cli/v1/userguide/cli-authentication-user.html)

とはいえ、AWSに入門したてで理解の浅い状態なので、ここでは手順を解説した記事を参考リンクとして載せておくに留めておきます。
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

そして、`aws sso login`コマンドでクレデンシャルをもとにトークンを取得します。以降はトークンで認証し、AWSのリソースを操作していきます。
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

### リファレンスの構造を読み解く

### コマンド実行

### 後片付け

## まとめ