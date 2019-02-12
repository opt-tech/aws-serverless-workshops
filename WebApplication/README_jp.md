# Serverless Web Application Workshop

このワークショップでは、ユーザーがWild Rydesの艦隊からユニコーンライドを要求できるようにする簡単なWebアプリケーションをデプロイします。 アプリケーションは、ピックアップしたい場所を示すためのHTMLベースのユーザーインターフェースをユーザーに提示し、リクエストを送信して近くのユニコーンをディスパッチするためのRESTful Webサービスとバックエンドでインターフェースします。 アプリケーションはまた、ユーザーがサービスに登録して乗り物を要求する前にログインするための機能を提供します。

> In this workshop you'll deploy a simple web application that enables users to request unicorn rides from the [Wild Rydes](http://www.wildrydes.com/) fleet. The application will present users with an HTML based user interface for indicating the location where they would like to be picked up and will interface on the backend with a RESTful web service to submit the request and dispatch a nearby unicorn. The application will also provide facilities for users to register with the service and log in before requesting rides.

アプリケーションアーキテクチャは、AWS Lambda、Amazon API Gateway、Amazon S3、Amazon DynamoDB、およびAmazon Cognitoを使用します。 S3は、HTML、CSS、JavaScript、およびユーザーのブラウザに読み込まれる画像ファイルなどの静的Webリソースをホストします。 ブラウザで実行されるJavaScriptは、LambdaとAPI Gatewayを使用して構築されたパブリックバックエンドAPIとデータを送受信します。 Amazon Cognitoは、バックエンドAPIを保護するためのユーザー管理および認証機能を提供します。 最後に、DynamoDBは、APIのLambda関数によってデータを格納できる永続層を提供します。

> The application architecture uses [AWS Lambda](https://aws.amazon.com/lambda/), [Amazon API Gateway](https://aws.amazon.com/api-gateway/), [Amazon S3](https://aws.amazon.com/s3/), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [Amazon Cognito](https://aws.amazon.com/cognito/). S3 hosts static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a  persistence layer where data can be stored by the API's Lambda function.

完全なアーキテクチャーの描写については、下の図を参照してください。

> See the diagram below for a depiction of the complete architecture.

![Wild Rydes Web Application Architecture](images/wildrydes-complete-architecture.png)

あなたが飛び込んで始めたいのであれば、ワークショップを始めるために静的ウェブホスティングモジュールのページを訪問してください。

> If you'd like to jump in and get started please visit the [Static Web hosting](1_StaticWebHosting/README_jp.md) module page to begin the workshop.

## Prerequisites (前提条件)

### AWS Account

このワークショップを完了するには、AWS IAM、S3、DynamoDB、Lambda、API Gateway、およびCognitoリソースを作成するためのアクセス権を持つAWSアカウントが必要です。 このワークショップのコードと手順では、一度に1人の学生のみが特定のAWSアカウントを使用していると想定しています。 他の生徒とアカウントを共有しようとすると、特定のリソースで名前の競合が発生します。 競合のために作成に失敗したリソースに固有のサフィックスを追加することでこれらを回避することができますが、説明にはこの作業を行うために必要な変更の詳細が記載されていません。

> In order to complete this workshop you'll need an AWS Account with access to create AWS IAM, S3, DynamoDB, Lambda, API Gateway and Cognito resources. The code and instructions in this workshop assume only one student is using a given AWS account at a time. If you try sharing an account with another student, you'll run into naming conflicts for certain resources. You can work around these by appending a unique suffix to the resources that fail to create due to conflicts, but the instructions do not provide details on the changes required to make this work.

このワークショップの一環としてあなたが立ち上げるすべてのリソースは、あなたのアカウントが12ヶ月未満であればAWSの無料利用枠の対象となります。 詳細についてはAWS無料利用枠ページをご覧ください。

> All of the resources you will launch as part of this workshop are eligible for the AWS free tier if your account is less than 12 months old. See the [AWS Free Tier page](https://aws.amazon.com/free/) for more details.

### Browser

このワークショップを完了するには、最新バージョンのChromeを使用することをお勧めします。

> We recommend you use the latest version of Chrome to complete this workshop.

### Text Editor

設定ファイルを少し更新するには、ローカルのテキストエディタが必要です。

> You will need a local text editor for making minor updates to configuration files.

## Modules

このワークショップは複数のモジュールに分割されています。 次に進む前に各モジュールを完成させる必要があります。ただし、モジュール1と2にはAWS CloudFormationテンプレートが用意されており、必要に応じて手動で作成しなくても必要なリソースを起動できます。

> This workshop is broken up into multiple modules. You must complete each module before proceeding to the next, however, modules 1 and 2 have AWS CloudFormation templates available that you can use to launch the necessary resources without manually creating them yourself if you'd like to skip ahead.

1. [Static Web hosting](1_StaticWebHosting/README_jp.md)
2. [User Management](2_UserManagement/README_jp.md)
3. [Serverless Backend](3_ServerlessBackend/README_jp.md)
4. [RESTful APIs](4_RESTfulAPIs/README_jp.md)

ワークショップを終了したら、クリーンアップガイドに従って作成されたすべてのリソースを削除できます。

> After you have completed the workshop you can delete all of the resources that were created by following the [cleanup guide](9_CleanUp).
