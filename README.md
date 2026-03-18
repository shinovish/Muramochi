# 村望しおん卒業記念サイト

GitHub Pages で公開できる、スマホ向けの静的サイトです。  
トップページ、メッセージ一覧、ファン別アルバムの3ページで構成しています。

## ページ構成

- `index.html`  
  トップページです。  
  「たくさんの思い出をありがとう」を表示し、トップ画像と「メッセージ」リンクを掲載しています。

- `messages.html`  
  ファン名とメッセージを一覧表示するページです。  
  各メッセージの下に、そのファンのアルバムページへのリンクがあります。

- `album.html`  
  `?fan=<fanId>` を受け取り、対応するファンのJPEG画像一覧を表示するページです。

## 現在のフォルダ構成

```text
Muramochi-main/
├─ README.md
├─ index.html
├─ messages.html
├─ album.html
├─ style.css
├─ data/
│  ├─ fans.json
│  ├─ messages.json
│  └─ albums/
│     ├─ shinovish.json
│     ├─ rabbit.json
│     └─ hanamaru.json
└─ assets/
   ├─ top/
   │  └─ idol-main.jpg
   └─ albums/
      ├─ shinovish/
      │  ├─ IMG_6161.jpeg
      │  ├─ IMG_6162.jpeg
      │  ├─ ...
      │  └─ IMG_6178.jpeg
      ├─ rabbit/
      │  ├─ IMG_6229.jpeg
      │  └─ IMG_6230.jpeg
      └─ hanamaru/
         └─ IMG_6228.jpeg
```

## データ設計

このサイトでは、以下の関係でデータを管理しています。

- ファン名とメッセージ: **1対1**
- ファン名とアルバム画像: **1対n**

### 1. ファン一覧
`data/fans.json`

```json
[
  {
    "id": "shinovish",
    "name": "ひいらぎ"
  },
  {
    "id": "rabbit",
    "name": "つむ"
  },
  {
    "id": "hanamaru",
    "name": "はなまるおばけ"
  }
]
```

### 2. メッセージ一覧
`data/messages.json`

```json
[
  {
    "fanId": "shinovish",
    "message": "おんちゃんのことを考えている毎日は幸せでした！これからもずっと応援してます！"
  },
  {
    "fanId": "rabbit",
    "message": "おなかすいた"
  },
  {
    "fanId": "hanamaru",
    "message": "はなまるなの！"
  }
]
```

### 3. ファン別アルバム
`data/albums/<fanId>.json`

```json
{
  "images": [
    "assets/albums/shinovish/IMG_6161.jpeg",
    "assets/albums/shinovish/IMG_6162.jpeg"
  ]
}
```

アルバムJSONは、**画像パスだけを並べるシンプルな形式**です。  
ファンとの紐づけはファイル名で表現します。

- `data/albums/shinovish.json` → `shinovish` のアルバム
- `data/albums/rabbit.json` → `rabbit` のアルバム
- `data/albums/hanamaru.json` → `hanamaru` のアルバム

## 画像表示仕様

- トップページ用画像と、ファンのアルバム画像は**別フォルダ**で管理しています
- アルバム画像は**元のアスペクト比のまま**表示します
- アルバム画像に**角丸はつけていません**
- アルバム画像をタップすると、画像ファイルを別タブで開けます

## 画像やメッセージを追加・更新する方法

### トップページ画像を差し替える
以下の画像を置き換えてください。

- `assets/top/idol-main.jpg`

### 既存ファンのメッセージを更新する
`data/messages.json` の該当ファンの `message` を書き換えてください。

### 既存ファンのアルバム画像を増やす
1. 対象フォルダにJPEGを追加します
2. 対応する `data/albums/<fanId>.json` の `images` 配列に追記します

例: `shinovish` の画像を増やす場合

```json
{
  "images": [
    "assets/albums/shinovish/IMG_6161.jpeg",
    "assets/albums/shinovish/IMG_6162.jpeg",
    "assets/albums/shinovish/IMG_6179.jpeg"
  ]
}
```

### 新しいファンを追加する

#### 1. `data/fans.json` に追加
```json
{
  "id": "sakura",
  "name": "さくら"
}
```

#### 2. `data/messages.json` に追加
```json
{
  "fanId": "sakura",
  "message": "卒業おめでとう！これからも応援しています。"
}
```

#### 3. `data/albums/sakura.json` を作成
```json
{
  "images": [
    "assets/albums/sakura/IMG_0001.jpeg"
  ]
}
```

#### 4. 画像フォルダを作成
- `assets/albums/sakura/`

## 動作確認の方法

このサイトは `fetch()` でJSONを読み込んでいるため、**HTMLファイルをダブルクリックして `file://` で開くと正常に動かないことがあります。**

確認方法は次のどちらかを使ってください。

- GitHub Pages に公開して確認する
- ローカルサーバーを立てて確認する

Python が使える場合の例:

```bash
python3 -m http.server 8000
```

その後、ブラウザで `http://localhost:8000/` を開いて確認してください。

## GitHub Pages で公開する手順

1. このファイル一式をリポジトリに配置する
2. GitHub の `Settings` → `Pages` を開く
3. `Build and deployment` の `Source` を **Deploy from a branch** にする
4. Branch を `main`、フォルダを `/ (root)` にする
5. 保存後、公開URLで動作確認する

リポジトリ名が `Muramochi` の場合、公開URLは次の形になります。

```text
https://<GitHubユーザー名>.github.io/Muramochi/
```

## トラブルシューティング

### メッセージやアルバムが表示されない
以下を確認してください。

- `data/fans.json` や `data/messages.json` に **構文エラーがないか**
- JSON の末尾に **余分なカンマ** がないか
- `messages.html` のリンク先 `album.html?fan=<fanId>` と、`data/albums/<fanId>.json` の `<fanId>` が一致しているか
- JSONに書いた画像パスと、実際のファイル配置が一致しているか
- `file://` ではなく、GitHub Pages かローカルサーバーで確認しているか
- ブラウザキャッシュが古い場合は、スーパーリロードやシークレットウィンドウで確認する

### GitHub Pages 上で古いページが出る
ブラウザキャッシュが残っていることがあります。  
シークレットウィンドウで開くか、URLに `?v=20260319` のようなクエリを付けて確認してください。

## 補足

GitHub Pages は静的サイトのため、サーバー側でフォルダ内のJPEGを自動走査して一覧化することはできません。  
そのため、このサイトでは **JSONに登録した画像だけを一覧表示する方式** にしています。
