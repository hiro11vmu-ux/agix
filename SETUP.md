# LINEユーザーID確認ツール（Vercel版）

LINEで送ったメッセージに対して「あなたのユーザーIDはこちらです」と
ボット自身が返信してくれる、一時的な仕組みです。

## 手順

### 1. GitHubに新しいリポジトリを作る

1. GitHubで新規リポジトリを作成（例: `line-userid-checker`）
   - 前回の `agix-signal-bot` とは別の、新しい小さなリポジトリでOKです
2. `api/webhook.js` と `package.json` をアップロード
   - `api` フォルダの中に `webhook.js` を置く必要があります
   - ファイル名の欄に `api/webhook.js` と入力して「Create new file」で作るのが確実です

### 2. Vercelにログインしてデプロイ

1. https://vercel.com を開く
2. 「Sign Up」→ **Continue with GitHub** を選んでGitHubアカウントでログイン
3. ログイン後、ダッシュボードで **「Add New...」→「Project」**
4. さきほど作った `line-userid-checker` リポジトリを選択して **Import**
5. 設定はそのままで **Deploy** をクリック
6. 1分程度でデプロイが完了し、`https://line-userid-checker-xxxx.vercel.app` のようなURLが発行されます

### 3. 環境変数を設定（チャネルアクセストークン）

1. デプロイ後、プロジェクトの **Settings** タブ → **Environment Variables**
2. 以下を追加:
   - Key: `LINE_CHANNEL_ACCESS_TOKEN`
   - Value: 以前発行したチャネルアクセストークン
3. 保存後、もう一度デプロイし直す（Settingsの追加は再デプロイしないと反映されないことがあります）
   - 「Deployments」タブ → 最新のデプロイの「...」→「Redeploy」

### 4. LINE Developers ConsoleでWebhook URLを設定

1. https://developers.line.biz/console/ → 対象チャネルを選択
2. 「Messaging API設定」タブ
3. Webhook URLに、VercelのURLの末尾に `/api/webhook` を付けて入力
   例: `https://line-userid-checker-xxxx.vercel.app/api/webhook`
4. 「検証」して成功すればOK
5. 「Webhookの利用」をオンにする
6. **「応答メッセージ」をオフ**にしておく（LINEのデフォルト自動応答と混ざらないように）
   - 「LINE公式アカウント機能」の項目内にあります

### 5. 自分のLINEから送ってみる

1. スマホのLINEで公式アカウントのトーク画面を開く
2. 「テスト」など何か送る
3. 数秒後、ボットから **「あなたのユーザーIDはこちらです: U....」** という返信が届きます
4. そのIDをコピーして保存

これでユーザーIDの取得は完了です。お疲れさまでした。
GitHubのSecretsに `LINE_USER_ID` として登録する準備が整いました。
