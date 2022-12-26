![](https://s3.amazonaws.com/aws-mobile-hub-images/aws-amplify-logo.png)
# AWS Amplify 工作坊
> 2022/12/29 18:00 @AWS台北辦公室
>
> presented / written by Wama (AWS Educate Cloud Ambassador, 技術支援)

這份文件主要是基於 [Amplify API 官方文件](https://docs.amplify.aws/start/getting-started/installation/q/integration/js/) 所延伸撰寫，如有發現錯誤歡迎指教

## 工作坊成品
跟隨本次工作坊，將可以學習及完成：
1. 基礎的網頁前端畫面
2. 利用 [AWS Amplify SDK](https://docs.amplify.aws/lib/graphqlapi/getting-started/q/platform/js/) 建立 GraphQL API 及 Database（serverless backend）
3. 用建立的 GraphQL API 連接前端及 Database
4. 最後，使用 Amplify 得到一個屬於自己的網頁🎉 （Amplify Deploy & Hosting）
![Screenshot of Amplify](https://docs.amplify.aws/images/browser-vanillajs-hosting.png)

## 開始之前...
### 環境需求
- [Node.js](https://nodejs.org/) v14.x or later
  - `node -v` 可以看目前的版本
- [npm](https://www.npmjs.com/) v6.14.4 or later
  - `npm -v` 可以看目前的版本
- [git](https://git-scm.com/) v2.14.1 or later
  - `git --version` 可以看目前的版本

以上三個都可以直接去官網下載並安裝即可

### 註冊 / 登入 AWS 帳號
- Option 1:
  如果你想保留今天做出來的成果，你可以使用 [你自己的 AWS 帳號](https://portal.aws.amazon.com/billing/signup?redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)（需要綁定信用卡）
- Option2:
  使用今日工作坊的 [Event Engine!!!!!!!!//TODO: Add link](//TODO)（工作坊結束後會自動銷毀）
## AWS Amplify 是什麼？
> [AWS Amplify](https://aws.amazon.com/tw/amplify/) 是一種完整的解決方案，讓前端 Web 和行動開發人員可以快速在 AWS 上建置、推出和託管完整堆疊應用程式，並且靈活地利用廣泛的 AWS 服務以因應使用案例的發展。不需要雲端專業技能。

AWS 擁有眾多產品，例如 EC2、S3、Dynamo DB、IAM、SageMaker 等等。
如果把每一個產品想像成一種食材，那解決方案就像是食譜，它告訴你如何組合各種食材，完成你想要的菜。

如果你是廚師，你很了解各種食材，那你當然可以很自由的運用每一種食材（AWS 的產品），完美的將它們組合成為一道美味的菜餚（也就是你運行在 AWS 上的服務）。

那萬一你了解的服務不多，不會煮菜怎麼辦？

AWS Amplify 就是為了可能比較不了解的 AWS 產品們的開發人員而生。AWS Amplify 就像一家賣料理包的商店，如果想吃咖哩，就買咖哩的料理包回家加熱一下拌飯就可以吃了。你根本不需要知道咖哩是怎麼煮出來的。
例如今天的工作坊，透過 AWS Amplify，就算我不知道 AWS 其他任何的產品怎麼用，我們還是可以在 AWS 上建置一個全端的網頁。但是它實際上就是運用 AWS 上各種產品來組合而成的。（如果你知道 AWS CloudFormation 的話，Amplify 的背後其實就是利用 CloudFormation 來建置和串起各個 AWS 的服務）

![](https://i.imgur.com/gkYbcYW.png)
> image source: [What is AWS Amplify? Pros and Cons?](https://www.youtube.com/watch?v=HkbjHtG_d7w)

## 下載及設定 Amplify CLI
- 打開 Terminal/終端機(MacOS) 或 PowerShell(Windows)
(如果你的作業系統是這兩個以外的話，我相信你知道要開什麼 XD)
- 用 npm 下載 Amplify CLI
```bash
npm install -g @aws-amplify/cli
```
`-g` 是指 global，在這台電腦上的任何地方都可以使用
如果沒有加 `-g` 的話，

- 下載完畢後，設定 Amplify CLI
```bash
amplify configure
```
接下來會跳出一個瀏覽器視窗，請你登入 AWS。

回到 CLI，他會請你選擇 Region 和新增新的 IAM User
```
Specify the AWS Region
? region:  <Your preferred region>
```
接著 Amplify CLI 會請你新增一個 [IAM](https://aws.amazon.com/tw/iam/) User
> Amazon IAM (Identity and Access Management) 用於管理在 AWS 中的 users 和權限

輸入完 username 後，就會開啟新增 IAM user 的瀏覽器視窗（在這裡輸入的 username 就是在 AWS IAM 中新增的 user 名稱）
```
Specify the username of the new IAM user:
? user name:  <User name for Amplify IAM user>
Complete the user creation using the AWS console: <URL>
```

![](https://docs.amplify.aws/images/user-creation.gif)

新增完 IAM user 後，要記下 `accessKeyId` 和 `secretAccessKey`（可以將 IAM 視窗留著先別關）
```
Enter the access key of the newly created user:
? accessKeyId:  <這裡輸入剛剛新增 user 的 accessKeyId>
? secretAccessKey:  <這裡輸入剛剛新增 user 的 secretAccessKey>
```
接下來 amplify 會把剛剛建好的 user 資料記在我們自己的電腦上（可以按 `Enter` 使用預設值就好）
```
This would update/create the AWS Profile in your local machine
? Profile Name: default
```
這樣就設定好 amplify CLI 了！可以開始使用 amplify 了！

## 建立前端
在你電腦上打開 Terminal/終端機(MacOS) 或 PowerShell(Windows)

接下來要建立這個專案的架構
```bash
mkdir -p amplify-js-app/src && cd amplify-js-app
```
上面這行指令建立了一個叫 amplify-js-app 的資料夾
在 amplify-js-app 這個資料夾中建立了一個叫 src 的資料夾
然後進入 amplify-js-app 這個資料夾中
```bash
touch index.html src/app.js webpack.config.js
```
接下來，建立了 `index.html` `webpack.config.js` 還有在 src 中的 `app.js` 三個檔案

然後使用 `npm` 這個套件管理工具下載相依的套件（包含 amplify 的 SDK）
（一行一行跑）
`npm init` 會問一些設定，基本上都用預設的就可以了
```shell
npm init
npm install aws-amplify
npm install webpack webpack-cli webpack-dev-server copy-webpack-plugin --save-dev
```
> 可以看到我們下載了 webpack 以及相關的套件。
> webpack 是用來「打包」的，他可以在複雜的前端結構，將多個 js 檔打包成一個單一檔案

> `--save-dev` 指的是這是開發時才會用到的，在正式 production 環境中不需要

載完後，整個專案的架構會長得像這樣
```
amplify-js-app
├── node_modules/
├── src/
│   └── app.js
├── index.html
├── package-lock.json
├── package.json
└── webpack.config.js
```
`package.json` 是 npm 用來記錄專案相關的設定的檔案
接下來，在 `package.json` 中**加入**
```javascript
"scripts": {
    "start": "webpack && webpack-dev-server --mode development",
    "build": "webpack"
}
```
意思是在執行 `npm start` 的時候，實際上會執行 `npm webpack && webpack-dev-server --mode`
執行 `npm build` 的時候，實際上會執行 `npm webpack`

再來在 `index.html` 裡新增：
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Amplify</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style>
      html,
      body {
        font-family: 'Amazon Ember', 'Helvetica', 'sans-serif';
        margin: 0;
      }
      a {
        color: #ff9900;
      }
      h1 {
        font-weight: 300;
      }
      hr {
        height: 1px;
        background: lightgray;
        border: none;
      }
      .app {
        width: 100%;
      }
      .app-header {
        color: white;
        text-align: center;
        background: linear-gradient(30deg, #f90 55%, #ffc300);
        width: 100%;
        margin: 0 0 1em 0;
        padding: 3em 0 3em 0;
        box-shadow: 1px 2px 4px rgba(0, 0, 0, 0.3);
      }
      .app-logo {
        width: 126px;
        margin: 0 auto;
      }
      .app-body {
        width: 400px;
        margin: 0 auto;
        text-align: center;
      }
      .app-body button {
        background-color: #ff9900;
        font-size: 14px;
        color: white;
        text-transform: uppercase;
        padding: 1em;
        border: none;
      }
      .app-body button:hover {
        opacity: 0.8;
      }
    </style>
  </head>

  <body>
    <div class="app">
      <div class="app-header">
        <div class="app-logo">
          <img
            src="https://aws-amplify.github.io/images/Logos/Amplify-Logo-White.svg"
            alt="AWS Amplify"
          />
        </div>
        <h1>Welcome to Amplify</h1>
      </div>
      <div class="app-body">
        <h1>Mutation Results</h1>
        <button id="MutationEventButton">Add data</button>
        <div id="MutationResult"></div>
        <hr />

        <h1>Query Results</h1>
        <div id="QueryResult"></div>
        <hr />

        <h1>Subscription Results</h1>
        <div id="SubscriptionResult"></div>
      </div>
    </div>
    <script src="main.bundle.js"></script>
  </body>
</html>
```
> html 給了網頁畫面基礎的架構，而 \<style> \</style> 之間包的 CSS 語法，則可以美化 html 中的架構

接下來撰寫 `webpack.config.js`：
```javascript
const CopyWebpackPlugin = require('copy-webpack-plugin');
const webpack = require('webpack');
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/app.js',
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/
      }
    ]
  },
  devServer: {
    client: {
      overlay: true
    },
    hot: true,
    watchFiles: ['src/*', 'index.html']
  },
  plugins: [
    new CopyWebpackPlugin({
      patterns: ['index.html']
    }),
    new webpack.HotModuleReplacementPlugin()
  ]
};
```
> `webpack.config.js` 是用來設定 webpack 用的。
> 例如 `entry: './src/app.js'` 就是告訴 webpack 程式是從 `./src/app.js` 這個檔案開始執行的，在打包時 webpack 才知道要從 `./src/app.js` 開始

這樣，一個簡單的前端就建好了 🎉

最後，回到 Terminal 執行
```shell
npm start
```
> 這個指令就會在你的電腦上架一個 HTTP Server 讓你自己可以連上來，並且看到前端畫面
> 而 `npm start` 實際上執行的指令，就是我們之前在 `package.json` 中設定的

指令執行後 Terminal 會停在執行的畫面，然後將畫面切回瀏覽器，打開 http://localhost:8080
就可以看到剛剛用好的前端畫面了！

但因為現在還沒有後端，所以 Add data 這個按鈕按下去是沒有反應的

## 建立後端
我們使用 Amplify 來建立後端，所以一開始先初始化 Amplify

### Amplify 專案初始化
在 Terminal 輸入並在前面建立好的專案根目錄（.../amplify-js-app/）下執行：
```shell
amplify init
```

接著 Amplify 會問一些問題（可以照下面的範例回答就好）
```shell
? Enter a name for the project: amplifyjsapp
The following configuration will be applied:

Project information
| Name: amplifyjsapp
| Environment: dev
| Default editor: Visual Studio Code
| App type: javascript
| Javascript framework: none
| Source Directory Path: src
| Distribution Directory Path: dist
| Build Command: npm run-script build
| Start Command: npm run-script start

? Initialize the project with the above configuration? Yes
Using default provider  awscloudformation
? Select the authentication method you want to use: AWS profile

For more information on AWS Profiles, see:
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html

? Please choose the profile you want to use: default
```

完成上面的動作後，這個 Amplify 專案就初始化完成了！
初始化的過程它做了以下幾個動作：
1. 在專案的根目錄（`.../amplify-js-app/`）下新增一個叫 `amplify` 的資料夾，用來儲存後端的定義，包含後面我們會加進來的 GraphQL API 和 web hosting。
2. 在 `amplify-js-app/src` 中新增 `aws-exports.js`，有了存在這個檔案裡的資料，這個專案中的 Amplify JavaScript client library（前面 `npm install aws-amplify` 時下載的，可以參考 [GitHub 上的 aws-amplify/amplify.js](https://github.com/aws-amplify/amplify-js)）才能和 AWS 溝通。
3. 修改 `.gitignore`，將不需要被 git 紀錄的檔案加進去
4. 在 AWS 中新增一個 Amplify 專案，使用 `amplify console` 可以在 AWS Console 中看到

```
amplify-js-app
├── node_modules/
├── amplify *
├── src/
│   ├── aws-exports.js *
│   └── app.js
├── index.html
├── package-lock.json
├── package.json
└── webpack.config.js
```

### 建立 API 和 Database

這個專案會使用的 API 是 GraphQL API。
Amplify 會用 AWS AppSync 來建立一個 GraphQL API。而 Database 會用到 Amazon DynamoDB（NoSQL 的 database）。

> API 就是 Application Programming Interface，是前端和後端溝通的橋樑。在這個專案中，前端透過 API 就可以拿到或修改 Database 中的資料。
> 而 GraphQL API 就是一種 API 的形式。（補充：如果想知道 REST API 和 GraphQL API 的差別，可以參考 [What Is GraphQL? REST vs. GraphQL](https://youtu.be/yWzKJPw_VzM)）

#### 建立 API 和 Database
用 amplify 建立 API 和 Database 非常簡單。
在專案的根目錄輸入並執行：
```
amplify add api
```
這個指令會將 GraphQL API 加進這個專案，並自動把 Database 準備好

他會請你輸入一些選項，在工作坊選項請根據以下來選
```
? Select from one of the below mentioned services:
# GraphQL
? Here is the GraphQL API that we will create. Select a setting to edit or continue
# Continue
? Choose a schema template:
# Single object with fields (e.g., “Todo” with ID, name, description)
✔ Do you want to edit the schema now? (Y/n)
# Yes
```
接下來他會自動幫你打開你的編輯器，要給 GraphQL schema
**amplify/backend/api/amplifyjsapp/schema.graphql**
```graphql
# This "input" configures a global authorization rule to enable public access to
# all models in this schema. Learn more about authorization rules here: https://docs.amplify.aws/cli/graphql/authorization-rules
input AMPLIFY { globalAuthRule: AuthRule = { allow: public } } # FOR TESTING ONLY!

type Todo @model {
  id: ID!
  name: String!
  description: String
}
```
> @model 是 Amplify CLI [GraphQL transformer](https://docs.amplify.aws/cli/graphql/data-modeling/) 的 feature 之一

回到 command line，按 Enter 繼續。

接著我們要把 API 部署到 AWS 上，在 command line 輸入：
```shell
amplify push
```
一樣輸入以下的設定，Amplify 會根據輸入的設定，自動幫你產生 code，讓我們能更簡單的執行 GraphQL 操作
```
? Do you want to generate code for your newly created GraphQL API (Yes)
? Choose the code generation language target (javascript)
? Enter the file name pattern of graphql queries, mutations and subscriptions (src/graphql/**/*.js)
? Do you want to generate/update all possible GraphQL operations - queries, mutations and subscriptions (Yes)
? Enter maximum statement depth [increase from default if your schema is deeply nested] (2)
```

這樣 API 就完成了 🎉
> 如果想要測試 API 的話，可以用 `amplify console api` 這個指令，使用 AWS console 來測試

可以在 command line 使用 `amplify status` 查看 Amplify 的狀態
```shell
amplify status
```
目前的狀態會長得像這樣
```
Current Environment: dev

| Category | Resource name | Operation | Provider plugin   |
| -------- | ------------- | --------- | ----------------- |
| Api      | amplifyjsapp  | No Change | awscloudformation |

GraphQL endpoint: https://•••••••••••.appsync-api.us-east-1.amazonaws.com/graphql
GraphQL API KEY: ••••••••••
```

## 用 API 連接前端與後端
接下來要修改一開始建立前端時的檔案 `src/app.js`（原本應該是空的）
將以下的 code 加進 `src/app.js`
> `Amplify.configure()` 會用之前儲存的 Amplify 設定，來設定好 library
> `API.graphql()` 是用來透過 GraphQL API 來對後端 database 操作
```javascript
import { Amplify, API, graphqlOperation } from "aws-amplify";

import awsconfig from "./aws-exports";
import { createTodo } from "./graphql/mutations";
import { listTodos } from "./graphql/queries";
import { onCreateTodo } from "./graphql/subscriptions";

Amplify.configure(awsconfig);

async function createNewTodo() {
  const todo = {
    name: "Use AppSync",
    description: `Realtime and Offline (${new Date().toLocaleString()})`,
  };

  return await API.graphql(graphqlOperation(createTodo, { input: todo }));
}

async function getData() {
  API.graphql(graphqlOperation(listTodos)).then((evt) => {
    evt.data.listTodos.items.map((todo, i) => {
      QueryResult.innerHTML += `<p>${todo.name} - ${todo.description}</p>`;
    });
  });
}

const MutationButton = document.getElementById("MutationEventButton");
const MutationResult = document.getElementById("MutationResult");
const QueryResult = document.getElementById("QueryResult");
const SubscriptionResult = document.getElementById("SubscriptionResult");

MutationButton.addEventListener("click", (evt) => {
  createNewTodo().then((evt) => {
    MutationResult.innerHTML += `<p>${evt.data.createTodo.name} - ${evt.data.createTodo.description}</p>`;
  });
});

API.graphql(graphqlOperation(onCreateTodo)).subscribe({
  next: (evt) => {
    const todo = evt.value.data.onCreateTodo;
    SubscriptionResult.innerHTML += `<p>${todo.name} - ${todo.description}</p>`;
  },
});

getData();
```

接著在 command line 輸入
```shell
npm start
```
就可以在自己的電腦的瀏覽器上看到成品啦 🤩
