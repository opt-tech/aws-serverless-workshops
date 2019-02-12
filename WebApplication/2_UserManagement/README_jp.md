# Module 2: User Authentication and Registration with Amazon Cognito User Pools

このモジュールでは、ユーザーのアカウントを管理するためのAmazon Cognitoユーザープールを作成します。 顧客が新規ユーザーとして登録し、自分のEメールアドレスを確認し、そしてサイトにサインインできるようにするページを配置します。

> In this module you'll create an Amazon Cognito user pool to manage your users' accounts. You'll deploy pages that enable customers to register as a new user, verify their email address, and sign into the site.

次のモジュールにスキップしたい場合は、必要なリソースを自動的に構築するために、選択した地域でこれらのAWS CloudFormationテンプレートの1つを起動できます。

> If you want to skip ahead to the next module, you can launch one of these AWS CloudFormation templates in the Region of your choice in order to build the necessary resources automatically.

Region| Launch
------|-----
US East (N. Virginia) | [![Launch Module 2 in us-east-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-us-east-1/WebApplication/2_UserManagement/user-management.yaml)
US East (Ohio) | [![Launch Module 2 in us-east-2](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-us-east-2/WebApplication/2_UserManagement/user-management.yaml)
US West (Oregon) | [![Launch Module 2 in us-west-2](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-us-west-2/WebApplication/2_UserManagement/user-management.yaml)
EU (Frankfurt) | [![Launch Module 2 in eu-central-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-eu-central-1/WebApplication/2_UserManagement/user-management.yaml)
EU (Ireland) | [![Launch Module 2 in eu-west-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-eu-west-1/WebApplication/2_UserManagement/user-management.yaml)
EU (London) | [![Launch Module 2 in eu-west-2](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-eu-west-2/WebApplication/2_UserManagement/user-management.yaml)
Asia Pacific (Tokyo) | [![Launch Module 2 in ap-northeast-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-ap-northeast-1/WebApplication/2_UserManagement/user-management.yaml)
Asia Pacific (Seoul) | [![Launch Module 2 in ap-northeast-2](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-2#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-ap-northeast-2/WebApplication/2_UserManagement/user-management.yaml)
Asia Pacific (Sydney) | [![Launch Module 2 in ap-southeast-2](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-ap-southeast-2/WebApplication/2_UserManagement/user-management.yaml)
Asia Pacific (Mumbai) | [![Launch Module 2 in ap-south-1](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cloudformation-launch-stack-button.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-south-1#/stacks/new?stackName=wildrydes-webapp-2&templateURL=https://s3.amazonaws.com/wildrydes-ap-south-1/WebApplication/2_UserManagement/user-management.yaml)

<details>
<summary><strong>CloudFormation Launch Instructions (expand for details)</strong></summary><p>

1. Choose the **Launch Stack** link above for the region of your choice.

1. Choose **Next** on the Select Template page.

1. Provide the name of your website bucket from module 1 for the  **Website Bucket Name** (e.g. `wildrydes-yourname`) and choose **Next**.

    **Note:** You must specify the same bucket name you used in the previous module. If you provide a bucket name that does not exist or that you do not have write access to, the CloudFormation stack will fail during creation.

    ![Speficy Details Screenshot](../images/module2-cfn-specify-details.png)

1. On the Options page, leave all the defaults and choose **Next**.

1. On the Review page, check the box to acknowledge that CloudFormation will create IAM resources and choose **Create**.
    ![Acknowledge IAM Screenshot](../images/cfn-ack-iam.png)

    This template uses custom resources to create an Amazon Cognito user pool and client as well as generate a configuration file with the details needed to connect to this user pool and upload it to your website bucket. The template will create a role that provides access for creating these resources and uploading the config file to your bucket.

1. Wait for the `wildrydes-webapp-2` stack to reach a status of `CREATE_COMPLETE`.

1. Follow the steps outlined in the [Implementation Verification](#implementation-verification) section to confirm you are ready to move on to the next module.

</p></details>

## Architecture Overview

ユーザーがあなたのウェブサイトにアクセスすると、まず新しいユーザーアカウントを登録します。このワークショップの目的上、登録のためにEメールアドレスとパスワードを提供するように彼らに要求するだけです。ただし、独自のアプリケーションで追加の属性を要求するようにAmazon Cognitoを設定できます。

> When users visit your website they will first register a new user account. For the purposes of this workshop we'll only require them to provide an email address and password to register. However, you can configure Amazon Cognito to require additional attributes in your own applications.

ユーザーが登録を送信した後、Amazon Cognitoは提供されたアドレスに確認コードを記載した確認Eメールを送信します。自分のアカウントを確認するには、ユーザーはあなたのサイトに戻って自分のメールアドレスと彼らが受け取った確認コードを入力します。テストに偽のEメールアドレスを使用する場合は、Amazon Cognitoコンソールを使用してユーザーアカウントを確認することもできます。

> After users submit their registration, Amazon Cognito will send a confirmation email with a verification code to the address they provided. To confirm their account, users will return to your site and enter their email address and the verification code they received. You can also confirm user accounts using the Amazon Cognito console if you want to use fake email addresses for testing.

ユーザーが確認済みアカウント（Eメールの確認プロセスまたはコンソールによる手動確認のいずれかを使用）を作成したら、サインインできます。サインインするときに、ユーザー名（またはEメール）とパスワードを入力します。その後、JavaScript関数がAmazon Cognitoと通信し、セキュアリモートパスワードプロトコル（SRP）を使用して認証し、一連のJSON Webトークン（JWT）を受け取ります。 JWTにはユーザーのIDに関するクレームが含まれており、Amazon API Gatewayで構築したRESTful APIに対して認証するために次のモジュールで使用されます。

> After users have a confirmed account (either using the email verification process or a manual confirmation through the console), they will be able to sign in. When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.

![Authentication architecture](../images/authentication-architecture.png)

## Implementation Instructions

以下の各セクションでは、実装の概要と詳細な手順を説明します。 すでにAWSマネジメントコンソールに精通している場合、またはウォークスルーに従わずにサービスを自分で探索したい場合は、概要に実装の完了に十分なコンテキストを記載してください。

> Each of the following sections provides an implementation overview and detailed, step-by-step instructions. The overview should provide enough context for you to complete the implementation if you're already familiar with the AWS Management Console or you want to explore the services yourself without following a walkthrough.

最新バージョンのChrome、Firefox、またはSafari Webブラウザを使用している場合は、セクションを展開するまでステップバイステップの説明は表示されません。

> If you're using the latest version of the Chrome, Firefox, or Safari web browsers the step-by-step instructions won't be visible until you expand the section.

### 1. Create an Amazon Cognito User Pool (Amazon Cognitoユーザープールを作成する)

#### Background

Amazon Cognitoには、ユーザーを認証するための2つの異なるメカニズムがあります。 Cognitoユーザープールを使用してアプリケーションにサインアップおよびサインイン機能を追加したり、Cognito IDプールを使用してFacebook、Twitter、AmazonなどのソーシャルIDプロバイダーを通じて、またはSAML IDソリューションを使用してユーザーを認証できます。 アイデンティティシステム このモジュールでは、提供された登録ページとサインインページのバックエンドとしてユーザープールを使用します。

> Amazon Cognito provides two different mechanisms for authenticating users. You can use Cognito User Pools to add sign-up and sign-in functionality to your application or use Cognito Identity Pools to authenticate users through social identity providers such as Facebook, Twitter, or Amazon, with SAML identity solutions, or by using your own identity system. For this module you'll use a user pool as the backend for the provided registration and sign-in pages.

#### High-Level Instructions

Amazon Cognitoコンソールを使用して、デフォルト設定を使用して新しいユーザープールを作成します。 プールを作成したら、プールIDを書き留めます。 この値は後のセクションで使用します。

> Use the Amazon Cognito console to create a new user pool using the default settings. Once your pool is created, note the Pool Id. You'll use this value in a later section.

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. From the AWS Console click **Services** then select **Cognito** under Security, Identity & Compliance.

1. Choose **Manage your User Pools**.

1. Choose **Create a User Pool**

1. Provide a name for your user pool such as `WildRydes`, then select **Review Defaults**

    ![Create a user pool screenshot](../images/create-a-user-pool.png)

1. On the review page, click **Create pool**.

1. Note the **Pool Id** on the Pool details page of your newly created user pool.

</p></details>

### 2. Add an App Client to Your User Pool (ユーザープールにアプリクライアントを追加する)

Amazon Cognitoコンソールからユーザープールを選択し、次に[App clients]セクションを選択します。 新しいアプリを追加して、[クライアントシークレットの生成]オプションが選択されていないことを確認します。 クライアント秘密はJavaScript SDKではサポートされていません。 生成されたシークレットでアプリを作成した場合は、それを削除して正しい設定で新しいアプリを作成してください。

> From the Amazon Cognito console select your user pool and then select the **App clients** section. Add a new app and make sure the Generate client secret option is deselected. Client secrets aren't supported with the JavaScript SDK. If you do create an app with a generated secret, delete it and create a new one with the correct configuration.

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. From the Pool Details page for your user pool, select **App clients** from the **General settings** section in the left navigation bar.

1. Choose **Add an app client**.

1. Give the app client a name such as `WildRydesWebApp`.

1. **Uncheck** the Generate client secret option. Client secrets aren't supported for use with browser-based applications.

1. Choose **Create app client**.

   <kbd>![Create app client screenshot](../images/add-app.png)</kbd>

1. Note the **App client id** for the newly created application.

</p></details>

### 3. Update the config.js File in Your Website Bucket

`/js/config.js` ファイルには、ユーザープールID、アプリクライアントID、および地域の設定が含まれています。 前の手順で作成したユーザープールとアプリの設定でこのファイルを更新し、ファイルをバケットにアップロードします。

> The [/js/config.js](../1_StaticWebHosting/website/js/config.js) file contains settings for the user pool ID, app client ID and Region. Update this file with the settings from the user pool and app you created in the previous steps and upload the file back to your bucket.

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. Download the [config.js](../1_StaticWebHosting/website/js/config.js) file from the website directory of the first module in this repository to your local machine.

1. Open the downloaded file using the text editor of your choice.

1. Update the `cognito` section with the correct values for the user pool and app you just created.

    You can find the value for `userPoolId` on the Pool details page of the Amazon Cognito console after you select the user pool that you created.

    ![Pool ID](../images/pool-id.png)

    You can find the value for `userPoolClientId` by selecting **App clients** from the left navigation bar. Use the value from the **App client id** field for the app you created in the previous section.

    ![Pool ID](../images/client-id.png)

    The value for `region` should be the AWS Region code where you created your user pool. E.g. `us-east-1` for the N. Virginia Region, or `us-west-2` for the Oregon Region. If you're not sure which code to use, you can look at the Pool ARN value on the Pool details page. The Region code is the part of the ARN immediately after `arn:aws:cognito-idp:`.

    The updated config.js file should look like this. Note that the actual values for your file will be different:
    ```JavaScript
    window._config = {
        cognito: {
            userPoolId: 'us-west-2_uXboG5pAb', // e.g. us-east-2_uXboG5pAb
            userPoolClientId: '25ddkmj4v6hfsfvruhpfi7n4hv', // e.g. 25ddkmj4v6hfsfvruhpfi7n4hv
            region: 'us-west-2' // e.g. us-east-2
        },
        api: {
            invokeUrl: '' // e.g. https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod',
        }
    };
    ```

1. Save the modified file making sure the filename is still `config.js`.

1. Open the Amazon S3 console by visiting [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/).

1. Select your Wild Rydes website bucket that you created in the previous module.

1. Browse to the `js` prefix.

1. Choose **Upload**, then **Add Files**.

1. Browse to the directory where you saved your locally modified version of the config.js file, select it, and choose **Open**.

    ![s3-upload.png](../images/s3-upload.png)

1. Choose **Upload** on the left side of the dialog.

</p></details>

<p>

注：登録、検証、およびサインインフローを管理するためのブラウザサイドコードを作成してもらう代わりに、最初のモジュールでデプロイしたアセットに実用的な実装を提供します。 `cognito-auth.js` ファイルには、UIイベントを処理して適切なAmazon Cognito Identity SDKメソッドを呼び出すコードが含まれています。 SDKの詳細については、GitHubのプロジェクトページを参照してください。

> **Note:** Instead of having you write the browser-side code for managing the registration, verification, and sign in flows, we provide a working implementation in the assets you deployed in the first module. The [cognito-auth.js](../1_StaticWebHosting/website/js/cognito-auth.js) file contains the code that handles UI events and invokes the appropriate Amazon Cognito Identity SDK methods. For more information about the SDK, see the [project page on GitHub](https://github.com/aws/amazon-cognito-identity-js).

</p>

## Implementation Validation

1. あなたのウェブサイトのドメインの下にある `/register.html` にアクセスするか、Giddy Upを選択してください。あなたのサイトのホームページのボタンをクリックしてください。

1. 登録フォームに記入して、「Let's Ryde」を選択してください。あなたはあなた自身のEメールを使うか、偽のEメールを入力することができます。少なくとも1つの大文字、数字、および特殊文字を含むパスワードを必ず選択してください。後で入力したパスワードを忘れないでください。ユーザーが作成されたことを確認するアラートが表示されます。

1. 次の2つの方法のいずれかを使用して、新しいユーザーを確認してください。

1. 管理しているEメールアドレスを使用した場合は、Webサイトドメインの下にある/verify.htmlにアクセスし、確認コードを入力してアカウント確認プロセスを完了できます。確認メールはスパムフォルダに保存される可能性があります。実際のデプロイでは、Amazon Simple Email Serviceを使用して、所有するドメインからEメールを送信するようにユーザープールを設定することをお勧めします。

1. ダミーのEメールアドレスを使用した場合は、Cognitoコンソールから手動でユーザーを確認する必要があります。

    1. AWSコンソールから、[Services]をクリックし、[Security、Identity＆Compliance]の下の[** Cognito **]を選択します。

    1. [ユーザープールの管理]を選択します。

    1.  `WildRydes`ユーザープールを選択して、左側のナビゲーションバーの** Users and groups **をクリックします。

    1. 登録ページから送信したEメールアドレスに対応するユーザーが表示されます。 そのユーザー名を選択してユーザー詳細ページを表示します。

    1. **ユーザーの確認**を選択してアカウント作成プロセスを終了します。

1. `/verify.html` ページまたはCognitoコンソールを使用して新しいユーザーを確認した後、`/signin.html` にアクセスして、登録手順で入力した電子メールアドレスとパスワードを使用してログインします。

1. 成功すれば `/ride.html` にリダイレクトされるはずです。 APIが設定されていないという通知が表示されます。

(Original)

1. Visit `/register.html` under your website domain, or choose the **Giddy Up!** button on the homepage of your site.

1. Complete the registration form and choose **Let's Ryde**. You can use your own email or enter a fake email. Make sure to choose a password that contains at least one upper-case letter, a number, and a special character. Don't forget the password you entered for later. You should see an alert that confirms that your user has been created.

1. Confirm your new user using one of the two following methods.

1. If you used an email address you control, you can complete the account verification process by visiting `/verify.html` under your website domain and entering the verification code that is emailed to you. Please note, the verification email may end up in your spam folder. For real deployments we recommend [configuring your user pool to use Amazon Simple Email Service](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-message-customizations.html#cognito-user-pool-settings-ses-authorization-to-send-email) to send emails from a domain you own.

1. If you used a dummy email address, you must confirm the user manually through the Cognito console.

    1. From the AWS console, click Services then select **Cognito** under Security, Identity & Compliance.

    1. Choose **Manage your User Pools**

    1. Select the `WildRydes` user pool and click **Users and groups** in the left navigation bar.

    1. You should see a user corresponding to the email address that you submitted through the registration page. Choose that username to view the user detail page.

    1. Choose **Confirm user** to finalize the account creation process.

1. After confirming the new user using either the `/verify.html` page or the Cognito console, visit `/signin.html` and log in using the email address and password you entered during the registration step.

1. If successful you should be redirected to `/ride.html`. You should see a notification that the API is not configured.

    ![Successful login screenshot](../images/successful-login.png)

Webアプリケーションに正常にログインしたら、次のモジュールであるServerless Backendに進むことができます。

> After you have successfully logged into your web application, you can proceed to the next module, [Serverless Backend](../3_ServerlessBackend/README_jp.md).

### Extra

* 受け取った **auth_token** をコピーしてオンラインのJWT Decoderに貼り付けて、このトークンがアプリケーションにとって何を意味するのかを理解してください。

> * Try copying the **auth_token** you've received and paste that into an [online JWT Decoder](https://jwt.io/) to understand what this token means for your application
