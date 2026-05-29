# ShelfSnap 📸

売場写真をフォルダ別にOneDriveへ自動保存するPWAアプリ

## 対応端末
- iPhone / iPad (Safari)
- Android (Chrome)
- PC ブラウザ

---

## セットアップ手順

### 1. GitHubリポジトリを作成
1. GitHub (github.com) にログイン
2. 「New repository」でリポジトリ作成（例：`shelfsnap`）
3. このフォルダのファイルを全てアップロード

### 2. GitHub Pagesを有効化
1. リポジトリの Settings → Pages
2. Source: `main` ブランチ / `/ (root)` を選択
3. Save → URLが発行される（例：`https://yourname.github.io/shelfsnap/`）

### 3. Azure App Registration（OneDrive連携）
1. [Azure Portal](https://portal.azure.com) にアクセス
2. Azure Active Directory → アプリの登録 → **新規登録**
3. 設定：
   - 名前：ShelfSnap
   - サポートされるアカウントの種類：**任意の組織のディレクトリ内のアカウントと個人の Microsoft アカウント**
   - リダイレクトURI：`https://yourname.github.io/shelfsnap/`（SPAを選択）
4. 登録後、**アプリケーション（クライアント）ID** をコピー
5. APIのアクセス許可 → アクセス許可の追加 → Microsoft Graph → 委任されたアクセス許可
   - `Files.ReadWrite` ✅
   - `User.Read` ✅
   - 「管理者の同意を与える」をクリック

### 4. index.html を更新
```javascript
const MSAL_CONFIG = {
  clientId: 'ここに取得したクライアントID', // ← 変更
  redirectUri: 'https://yourname.github.io/shelfsnap/', // ← 変更
  scopes: ['Files.ReadWrite', 'User.Read'],
};
```

### 5. iPhoneでホーム画面に追加
1. Safari でアプリのURLを開く
2. 共有ボタン（四角から矢印）→「ホーム画面に追加」
3. アプリアイコンが追加される

---

## ファイル構成
```
shelfsnap/
├── index.html      # メインアプリ
├── manifest.json   # PWA設定
├── sw.js           # Service Worker（オフライン対応）
├── icon-192.png    # アプリアイコン（要作成）
├── icon-512.png    # アプリアイコン（要作成）
└── README.md
```

## アイコン画像について
`icon-192.png` と `icon-512.png` はアプリのホーム画面アイコンです。
icon_preview.html で確認した案Bのデザインを画像として保存して配置してください。

---

## 使い方
1. アプリを開く → Microsoftアカウントでサインイン
2. ホーム画面でフォルダを選択（例：A店）
3. 「撮影する」をタップ → 連続撮影
4. 写真が自動で `OneDrive/ShelfSnap/A店/A店_20260529_143022.jpg` に保存

## フォルダ管理
- 下部ナビ「フォルダ」→ 追加・削除が可能
- 追加するとOneDrive上にも自動でフォルダ作成
