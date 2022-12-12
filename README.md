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

