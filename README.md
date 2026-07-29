# 負けて、負けて…負けて、勝って ― 成長の心理学

アニメ『ラグナクリムゾン』第1話のセリフ「負けて、負けて…負けて、勝って」をモチーフにした、1ページ完結のインタラクティブWebアニメーションです。

「勝率が試行回数とともに上がっていく」という学習曲線の研究(S字曲線・練習のべき乗則など)に基づいたロジスティック関数で100試行分の勝敗確率を計算し、その確率に従って毎回ランダムに(乱数で)実際の勝敗を生成しています。そのため負けの連続がだんだん短くなり、勝ちの連続がだんだん長くなっていく――というパターンが、単なる決め打ちの演出ではなく統計モデルのシミュレーション結果として自然に立ち上がります。スクロール後には、これを裏付ける心理学の研究を出典付きのカードで解説します。

- 単一HTMLファイル(`index.html`)、外部ビルド不要
- HTML / CSS / Vanilla JavaScriptのみ(外部ライブラリ・外部フォント読み込みなし)
- スマホ・PC両対応のレスポンシブレイアウト
- ダークテーマ + グロー/パーティクル演出(アニメ部分)、可読性優先の落ち着いた配色(解説カード部分)

## 曲線のモデルについて

ページ内の「この曲線の根拠について」からも確認できますが、要点は以下の通りです。

- 各試行 `n`(1〜100)の勝率は、ロジスティック関数(S字曲線) `P(n) = P_min + (P_max − P_min) / (1 + e^(−k(n − n0)))` に従う
  - `P_min = 8%`(序盤はおよそ10回に1回勝つ)/ `P_max = 93%`(終盤の到達勝率)/ `n0 = 55`(勝率が50%を超える転換点)/ `k = 0.10`(曲線の急峻さ)
- 各試行の勝敗そのものは、この確率を使ったベルヌーイ試行(重み付き乱数)で決まる。**そのため再生するたびに実際の勝敗パターンは毎回変わる**(=同じ結果は二度と出ない)が、根底にある「勝率が上がっていく形」は共通
- テキストが切り替わる速さ(演出のテンポ)は、練習を重ねるほど反応が速くなる「練習のべき乗則」`delay(n) = 100 + 360 × n^(−0.35)` [ms] に沿って加速する

これらの**関数の形**(S字カーブになること、べき乗則で速くなること)は下記の心理学研究に基づいています。ただし `P_min` や `n0` などの**具体的な数値**は、その形状を再現するために本作品用に選んだ例示的なパラメータであり、特定の実験データに直接フィットさせたものではありません(そのように現時点で公開されている「100試行の勝率推移」そのものを報告した論文は存在しないため)。

- Thurstone, L. L. (1919). The Learning Curve Equation. *Psychological Monographs*, 26(3).
- Newell, A., & Rosenbloom, P. S. (1981). Mechanisms of Skill Acquisition and the Law of Practice.
- Fitts, P. M., & Posner, M. I. (1967). *Human Performance*.
- Ericsson, K. A., Krampe, R. T., & Tesch-Römer, C. (1993). The Role of Deliberate Practice in the Acquisition of Expert Performance. *Psychological Review*, 100(3), 363–406.
- Bandura, A. (1977). Self-Efficacy: Toward a Unifying Theory of Behavioral Change. *Psychological Review*, 84(2), 191–215.
- Groves, P. M., & Thompson, R. F. (1970). Habituation: A Dual-Process Theory. *Psychological Review*, 77(5), 419–450.
- Dweck, C. S., & Leggett, E. L. (1988). A Social-Cognitive Approach to Motivation and Personality. *Psychological Review*, 95(2), 256–273.

(完全な書誌情報はページ最下部の「参考文献」欄にも掲載しています)

## ローカルで確認する

ビルド不要です。`index.html` をブラウザで直接開くか、簡易サーバーで確認してください。

```bash
cd ragna-growth-curve
python3 -m http.server 8000
```

`http://localhost:8000` にアクセスして確認できます。

## Cloudflare Pagesとは

**Cloudflare Pages** は、GitHub等のリポジトリに置いた静的なWebサイト(今回のような HTML/CSS/JS だけのサイト)を、無料で世界中に高速配信してくれるホスティングサービスです。「Cloudflare」という会社が提供する、いくつものサービス群のうちの一つです。

- サイトは `https://<プロジェクト名>.pages.dev` という無料URLで公開されます
- クレジットカード登録なしの無料プランで、今回のような1ページサイトは十分に運用できます
- 独自ドメイン(自分で取得した `example.com` など)を後から接続することも可能です

以下に、初めての方でも迷わないよう2つの方法をかなり細かく書きます。**方法1(ダッシュボード連携)** はブラウザの画面操作だけで完結するため、初めての方にはこちらがおすすめです。**方法2(Wrangler CLI)** はターミナル操作に慣れている場合に、より少ない手順で素早くデプロイできます。

---

## 方法1: Cloudflareダッシュボードからのデプロイ(GitHub連携・自動デプロイ)

### 事前準備

1. **GitHubアカウント**を持っていない場合は [github.com](https://github.com/) で作成してください(無料)
2. **Cloudflareアカウント**を持っていない場合は [dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up) で作成してください(無料。メールアドレスとパスワードだけで登録できます)
3. このプロジェクトをGitHubにpushしておく(手順は本READMEの「GitHubリポジトリの作成」を参照してください。**先にこちらを終わらせてから**方法1に進んでください)

### 手順

1. ブラウザで [dash.cloudflare.com](https://dash.cloudflare.com/) を開き、作成したアカウントでログインします
2. ログインすると、左側に縦のメニューが並んだ「Cloudflareダッシュボード」というトップ画面が出ます。左メニューの中から **「Workers & Pages」** という項目を探してクリックします(スパナ/歯車のようなアイコンが目印です)
3. 画面右上あたりにある **「Create application」**(または単に **「Create」**)という青系のボタンをクリックします
4. 画面上部にタブが並んでいるので、**「Pages」** タブを選びます(デフォルトで「Workers」が選ばれていることが多いので切り替えが必要です)
5. **「Connect to Git」** というボタンをクリックします
6. 初回はGitHubとの連携を求められます。**「Connect GitHub」**(または「GitHubに接続」)をクリックし、GitHubのログイン画面が別ウィンドウ/別タブで開いたらログインし、Cloudflareからのアクセスを **「Authorize」**(許可)します
   - このとき「All repositories」(すべてのリポジトリ)か「Only select repositories」(特定のリポジトリのみ)を選べます。迷ったら今回作成したリポジトリだけを選ぶ「Only select repositories」で問題ありません
7. Cloudflareの画面に戻ると、連携したリポジトリの一覧が表示されるので、今回作成したリポジトリ(例: `ragna-growth-curve`)を選び、**「Begin setup」** をクリックします
8. 「Set up builds and deployments」という設定画面になります。以下のように入力・確認してください
   - **Project name**: そのままでも、好きな名前に変えてもOK(このプロジェクト名がそのまま公開URL `https://<プロジェクト名>.pages.dev` になります)
   - **Production branch**: `main`(pushしたブランチ名と一致しているか確認)
   - **Framework preset**: `None`(プルダウンから選択。何もフレームワークを使っていないため)
   - **Build command**: 空欄のまま(何も入力しない)
   - **Build output directory**: `/`(ルートのまま。`index.html` を直接置いているため)
9. 一番下の **「Save and Deploy」** ボタンをクリックします
10. 1分程度でビルド(といっても今回は単にファイルをコピーするだけ)とデプロイが完了し、「Success! Your site is live」のような画面とともに公開URL(`https://<プロジェクト名>.pages.dev`)が表示されます。そのリンクをクリックすれば、実際に公開されたページが確認できます

以降は、GitHubリポジトリの `main` ブランチに `git push` するたびに、Cloudflareが自動的に検知して再デプロイしてくれます。手動での再デプロイ操作は不要です。

---

## 方法2: Wrangler CLIから直接デプロイ

`Wrangler` はCloudflareが公式に提供しているコマンドラインツールです。GitHub連携をしなくても、手元のフォルダから直接コマンド一発で公開できます。Node.js(と `npx`)さえ入っていれば、Wrangler自体を事前にインストールする必要はありません。

```bash
cd ragna-growth-curve

# 初回のみ実行。ブラウザが自動的に開き、Cloudflareアカウントでのログイン・認可を求められます
npx wrangler login

# デプロイ実行(現在のフォルダ "." をそのまま公開する指定)
npx wrangler pages deploy . --project-name=ragna-growth-curve
```

- `npx wrangler login` を実行すると、ブラウザが開いて「Wranglerに◯◯への権限を許可しますか?」という確認画面が出るので、**「Allow」**(許可)をクリックしてください。ターミナルに戻ると「Successfully logged in」と表示されます
- `npx wrangler pages deploy . --project-name=ragna-growth-curve` を実行すると、初回はCloudflare上に自動的に同名のPagesプロジェクトが作成され、そのままファイルがアップロード・公開されます
  - `--project-name` の値は好きな名前に変更できます(公開URLの `<プロジェクト名>.pages.dev` 部分になります)
- コマンドが完了すると、ターミナルに公開URL(`https://<プロジェクト名>.pages.dev`)が表示されます
- 2回目以降は、ファイルを更新したあと同じ `npx wrangler pages deploy .` コマンドを実行するだけで再デプロイされます(GitHub連携と違い、pushだけでは自動反映されない点に注意してください)

---

## GitHubリポジトリの作成

このフォルダは `git init` 済みです。GitHub上にリポジトリを作成してpushしてください。

`gh` コマンド(GitHub公式CLI)が使える場合は以下の1コマンドで作成からpushまで完了します。

```bash
cd ragna-growth-curve
gh repo create <リポジトリ名> --public --source=. --remote=origin --push
```

`gh` CLIが無い場合は、[github.com/new](https://github.com/new) でブラウザから空のリポジトリを作成した後(README等は追加せず「空」のまま作成してください)、以下を実行してください。

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
