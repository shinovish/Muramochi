# 卒業記念サイト（GitHub Pages向け）

スマホ向けの静的サイトです。  
以下の3ページで構成しています。

- `index.html` … トップページ
- `messages.html` … ファンメッセージ一覧
- `album.html` … ファンごとのJPEGアルバム一覧

## フォルダ構成

```text
graduation-site/
├─ index.html
├─ messages.html
├─ album.html
├─ style.css
├─ data/
│  ├─ fans.json
│  ├─ messages.json
│  └─ albums/
│     └─ hiiragi.json
└─ assets/
   ├─ top/
   │  └─ idol-main.jpg
   └─ albums/
      └─ hiiragi/
         ├─ album-01.jpg
         └─ album-02.jpg
```

## 要件への対応

- トップページに **「たくさんの思い出をありがとう」** を表示
- トップページに画像掲載
- トップページに **「メッセージ」** リンクを配置
- メッセージページに、ファン名 **「ひいらぎ」** とメッセージ  
  **「おんちゃんのことを考えている毎日は幸せでした！これからもずっと大好きだよ！」** を表示
- メッセージの下に **「アルバム」** リンクを配置
- アルバムページでJPEGファイルを一覧表示
- トップ用画像フォルダ `assets/top/` と、ファン別画像フォルダ `assets/albums/<fanId>/` を分離
- データ構造は以下の関係で管理
  - ファン名とメッセージ: 1対1
  - ファン名とJPEGファイル: 1対n

## データ構造

### 1. ファン一覧
`data/fans.json`

```json
[
  { "id": "hiiragi", "name": "ひいらぎ" }
]
```

### 2. メッセージ一覧（fanIdごとに1件）
`data/messages.json`

```json
[
  {
    "fanId": "hiiragi",
    "message": "おんちゃんのことを考えている毎日は幸せでした！これからもずっと大好きだよ！"
  }
]
```

### 3. アルバム一覧（fanIdごとに複数画像）
`data/albums/hiiragi.json`

```json
{
  "fanId": "hiiragi",
  "images": [
    {
      "src": "assets/albums/hiiragi/album-01.jpg",
      "alt": "ひいらぎのアルバム画像 1",
      "caption": "album-01.jpg"
    }
  ]
}
```

## 写真の差し替え方法

### トップページの画像
以下のファイルを差し替えてください。

- `assets/top/idol-main.jpg`

### ひいらぎさんのアルバム画像
以下のフォルダにJPEGを追加してください。

- `assets/albums/hiiragi/`

追加後、`data/albums/hiiragi.json` の `images` 配列に画像パスを追記してください。

例:

```json
{
  "src": "assets/albums/hiiragi/album-03.jpg",
  "alt": "ひいらぎのアルバム画像 3",
  "caption": "album-03.jpg"
}
```

## 新しいファンを追加する方法

### 1. `data/fans.json` に追記
```json
{ "id": "sakura", "name": "さくら" }
```

### 2. `data/messages.json` に追記
```json
{
  "fanId": "sakura",
  "message": "卒業おめでとう！これからも応援しています。"
}
```

### 3. アルバムJSONを作成
`data/albums/sakura.json`

```json
{
  "fanId": "sakura",
  "images": [
    {
      "src": "assets/albums/sakura/album-01.jpg",
      "alt": "さくらのアルバム画像 1",
      "caption": "album-01.jpg"
    }
  ]
}
```

### 4. 画像フォルダを作成
- `assets/albums/sakura/`

## GitHub Pagesで公開する手順

1. このファイル一式をGitHubリポジトリにアップロード
2. `Settings` → `Pages` を開く
3. `Build and deployment` の `Source` を **Deploy from a branch** に設定
4. Branch を `main`、フォルダを `/ (root)` に設定
5. 保存後、数分で公開されます

## 補足

GitHub Pages は静的サイトのため、サーバー側でフォルダ内JPEGを自動走査して一覧化する仕組みは使えません。  
そのため、このサンプルでは **JSONに登録されたJPEGファイルを一覧表示する方式** にしています。  
JPEGを追加したら、対応する `data/albums/<fanId>.json` も更新してください。
