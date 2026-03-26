# 宿題進捗管理＆シミュレーションアプリ

PyScript（ブラウザPython）+ Google Calendar API で動作する宿題トラッカーです。
`index.html` をブラウザで開くだけで使えます（サーバー不要）。

---

## 使い方

### 1. index.html をブラウザで開く

```
C:\Users\rinku\Downloads\homework-tracker\index.html
```

- 初回ロード時に PyScript（約10MB）を CDN からダウンロードするため、ネット接続が必要です。
- 「Python エンジン起動中…」の表示が消えたら使用開始できます。

### 2. タスクを登録する

「＋ タスク追加」ボタンを押して、教科名・ページ数・期限を入力します。

### 3. 実績を入力する

各通科カードの「✏️ 実績入力」ボタンを押し、今日進んだページ数と学習時間（分）を記録します。
「このペースで行くと〇月〇日に完了」というシミュレーション結果が表示されます。

---

## Googleカレンダー連携の設定（任意）

カレンダー連携を使うには、Google Cloud Console でプロジェクトを作成し、`index.html` 内の `GOOGLE_CLIENT_ID` を設定する必要があります。

### ステップ 1: Google Cloud Console でプロジェクト作成

1. [console.cloud.google.com](https://console.cloud.google.com) にアクセス
2. 「新しいプロジェクト」を作成（名前は任意）

### ステップ 2: Google Calendar API を有効化

1. 「APIとサービス」→「ライブラリ」
2. 「Google Calendar API」を検索して「有効にする」

### ステップ 3: OAuth 2.0 クライアントIDを取得

1. 「APIとサービス」→「認証情報」
2. 「認証情報を作成」→「OAuthクライアントID」
3. アプリの種類: **ウェブアプリケーション**
4. 承認済みの JavaScript 生成元に以下を追加:
   - `http://localhost`（ローカルファイルで開く場合）
   - `http://127.0.0.1`
   - または使用するドメイン
5. 「作成」を押し、表示された **クライアントID** をコピー

### ステップ 4: index.html にクライアントIDを設定

```javascript
// index.html 内のこの行を書き換えてください（約185行目付近）
const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com';
//  ↓ 取得したクライアントIDに置き換え
const GOOGLE_CLIENT_ID = '1234567890-abcdef.apps.googleusercontent.com';
```

> **注意**: ローカルファイル (`file://`) から OAuth を使用するには  
> VS Code の [Live Server 拡張機能](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) で `http://localhost` 経由で開いてください。

---

## データについて

- タスクデータはブラウザの `localStorage` に保存されます
- ブラウザのキャッシュ・サイトデータを削除するとリセットされます
- 手動でリセットするには、ブラウザの開発者ツール（F12）→ Application → Local Storage → `hw_tasks` を削除

---

## 計算式

| 計算項目 | 式 |
|---|---|
| 今日のノルマ | 残りページ ÷ 残り日数 |
| 1ページの所要時間 | 累計学習時間 ÷ 累計完了ページ |
| 残り必要時間 | 残りページ × 1ページ所要時間 |
| 推定完了日数 | 残り必要時間 ÷ 1日の学習時間 |
