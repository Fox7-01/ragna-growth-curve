# 負けて、負けて…負けて、勝って ― 成長の心理学

アニメ『ラグナクリムゾン』第1話のセリフ「負けて、負けて…負けて、勝って」をモチーフにした、1ページ完結のインタラクティブWebアニメーションです。

負けの連続がだんだん短くなり、勝ちの連続がだんだん長くなっていく様子をタイポグラフィアニメーションとグラフで可視化し、これが実際の心理学(学習曲線のS字カーブ、自己効力感、成長マインドセットなど)と一致する現象であることをスクロール後のカードで解説します。

- 単一HTMLファイル(`index.html`)、外部ビルド不要
- HTML / CSS / Vanilla JavaScriptのみ(外部ライブラリ・外部フォント読み込みなし)
- スマホ・PC両対応のレスポンシブレイアウト
- ダークテーマ + グロー/パーティクル演出(アニメ部分)、可読性優先の落ち着いた配色(解説カード部分)

## ローカルで確認する

ビルド不要です。`index.html` をブラウザで直接開くか、簡易サーバーで確認してください。

```bash
cd ragna-growth-curve
python3 -m http.server 8000
```

`http://localhost:8000` にアクセスして確認できます。

## Cloudflare Pagesへのデプロイ

ビルドコマンドは不要、出力ディレクトリはルート(`/`)のままでOKです。

### 方法1: ダッシュボードからGitHub連携で自動デプロイ

1. GitHubにこのリポジトリを作成し、push しておく(下記「GitHubリポジトリの作成」参照)
2. [Cloudflareダッシュボード](https://dash.cloudflare.com/)にログイン
3. 左メニューの **Workers & Pages** → **Create** → **Pages** タブ → **Connect to Git**
4. 対象のGitHubリポジトリを選択して連携を許可
5. ビルド設定を以下のように入力
   - **Framework preset**: `None`
   - **Build command**: (空欄のまま)
   - **Build output directory**: `/`
6. **Save and Deploy** をクリック

以降は `main` ブランチへの push のたびに自動的に再デプロイされます。

### 方法2: Wrangler CLIから直接デプロイ

Node.jsが入っていれば、リポジトリ以下で以下を実行するだけでデプロイできます(事前インストール不要、`npx` で都度実行)。

```bash
cd ragna-growth-curve
npx wrangler login          # 初回のみ。ブラウザでCloudflareアカウント認証
npx wrangler pages deploy . --project-name=ragna-growth-curve
```

- `--project-name` は任意の名前に変更可能です(初回実行時にCloudflare上にPagesプロジェクトが作成されます)
- 2回目以降は同じコマンドを実行するだけで再デプロイされます
- デプロイ完了後、コンソールに公開URL(`https://<project-name>.pages.dev`)が表示されます

## GitHubリポジトリの作成

このフォルダは `git init` 済みです。GitHub上にリポジトリを作成してpushしてください。

```bash
cd ragna-growth-curve
gh repo create <リポジトリ名> --public --source=. --remote=origin --push
```

`gh` CLIが無い場合は、GitHub上で空リポジトリを作成した後に以下を実行してください。

```bash
git remote add origin https://github.com/<ユーザー名>/<リポジトリ名>.git
git branch -M main
git push -u origin main
```

### リポジトリ名の候補

- `ragna-growth-curve`
- `makezu-tsuzukeru-kyokusen`(負け続ける曲線)
- `lose-to-win-curve`
- `seichou-kyokusen`(成長曲線)

## ファイル構成

```
ragna-growth-curve/
├── index.html   # ページ本体(HTML/CSS/JS全て内包)
└── README.md    # このファイル
```
