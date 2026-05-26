# 法的文書ホスティング

このフォルダにはアプリの公開用法的文書(プライバシーポリシー・利用規約・特商法表記)が入っています。
App Store Connect / Google Play Console の「プライバシーポリシー URL」「利用規約 URL」に登録するため、
公開 URL が必要です。

## ファイル一覧

| ファイル | 用途 |
|---|---|
| `index.html` | ランディング(各文書へのリンク集) |
| `privacy.html` | プライバシーポリシー |
| `terms.html` | 利用規約 |
| `tokushoho.html` | 特定商取引法に基づく表記 |
| `style.css` | 共通スタイル |

アプリ内の表示は `src/lib/legal.ts` にミラーされています。**内容を変更したら必ず両方を更新すること**。

## 公開前の差し替え

各 HTML と `src/lib/legal.ts` には `__SUPPORT_EMAIL__` プレースホルダーが入っています。
Gmail アカウント作成後、以下のコマンドで一括置換:

```bash
# legal/ 配下の HTML 置換 (Windows PowerShell)
Get-ChildItem -Path legal -Filter *.html | ForEach-Object {
  (Get-Content $_.FullName) -replace '__SUPPORT_EMAIL__', 'your-actual@gmail.com' | Set-Content $_.FullName
}

# legal.ts も忘れず手動置換 (CONTACT_EMAIL の値を書き換え)
```

## GitHub Pages へのデプロイ

### オプション A: 専用リポジトリを作る(推奨)

1. GitHub で新しい**パブリック**リポジトリ `study-ai-legal` を作成
2. このフォルダ(`legal/`)の中身をそのリポジトリのルートに push:
   ```bash
   cd legal/
   git init
   git add .
   git commit -m "Initial legal docs"
   git remote add origin https://github.com/<your-username>/study-ai-legal.git
   git push -u origin main
   ```
3. リポジトリの **Settings → Pages**:
   - Source: `Deploy from a branch`
   - Branch: `main` / `/ (root)`
   - Save
4. 2-3 分待つと URL が発行される: `https://<your-username>.github.io/study-ai-legal/`
5. 確認用 URL:
   - プライバシーポリシー: `https://<your-username>.github.io/study-ai-legal/privacy.html`
   - 利用規約: `https://<your-username>.github.io/study-ai-legal/terms.html`
   - 特商法: `https://<your-username>.github.io/study-ai-legal/tokushoho.html`

### オプション B: 本リポジトリの `/docs` で配信

`study-ai-app` リポジトリをパブリックにできる場合に限る(現状はプライベートのはず)。
プライベートでは GitHub Pages 配信不可なので、無料運用したいならオプション A 一択。

## ストアへの登録

### App Store Connect

- App 情報 → プライバシーポリシー URL: `https://<your-username>.github.io/study-ai-legal/privacy.html`
- App 情報 → 利用規約 URL: `https://<your-username>.github.io/study-ai-legal/terms.html`
- App 情報 → サポート URL: `https://<your-username>.github.io/study-ai-legal/` (トップでも OK)

### Google Play Console

- アプリのコンテンツ → プライバシーポリシー: `https://<your-username>.github.io/study-ai-legal/privacy.html`
- ストアの掲載情報 → ウェブサイト: `https://<your-username>.github.io/study-ai-legal/`

## アプリ内リンクも合わせる

アプリ内の「設定 → 利用規約」「設定 → プライバシーポリシー」は現在
`src/app/(app)/legal/{terms,privacy}.tsx` でローカルテキストを表示しています。
公開後は WebView で公開 URL を開くように切り替えても OK ですが、オフライン対応のため
**現状のローカル表示で問題ありません**。
