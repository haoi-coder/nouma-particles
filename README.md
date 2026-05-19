# nouma-particles

NoumaサービスページのSTUDIOサイトにiframeで埋め込む静的HTML群。
パーティクル背景・サービスフロー・フェーズ進化ビューなど、STUDIO単独では実装が重い部分を切り出してホスティングする。

## 構成

| ファイル | 用途 |
|---|---|
| `index.html` | パーティクル背景（軽量・自動パフォーマンス調整付き） |
| `nouma-flow.html` | ご相談〜導入までの流れ |
| `nouma-phase-evolution.html` | フェーズ進化ビュー（サービスページ用） |
| `nouma-phase-evolution-home.html` | フェーズ進化ビュー（ホーム埋め込み用、リンクは `/service#anchor`） |

すべて単一HTML（CSS・JSはインライン）。ビルド不要。

## Vercelデプロイ

GitHub連携でそのままデプロイできる。

1. [Vercel](https://vercel.com/) にログイン
2. 「Add New Project」→ `haoi-coder/nouma-particles` をImport
3. Framework Preset: **Other**（自動検出される）
4. Build Command / Output Directory はデフォルト（空）のままでOK
5. Deploy

`vercel.json` で以下を設定済み：

- `cleanUrls: true` — `/nouma-flow.html` を `/nouma-flow` でアクセス可能
- `Cache-Control` — CDN側で5分キャッシュ + stale-while-revalidate 24h（更新は数分で反映、配信は高速）
- `Content-Security-Policy: frame-ancestors *` — STUDIOからのiframe埋め込みを許可

## STUDIO側の埋め込み

iframeの `src` をVercelのデプロイURLに差し替える。

```html
<!-- 例 -->
<iframe src="https://nouma-particles.vercel.app/nouma-phase-evolution" ...></iframe>
```

カスタムドメイン（例: `particles.nouma.co.jp`）を当てると、独自ドメインで配信できる。

## ローカル確認

ビルド不要なので、任意の静的サーバーで開ける。

```bash
python3 -m http.server 8000
# → http://localhost:8000/nouma-flow.html
```

## 更新フロー

1. ローカルでHTMLを編集
2. `git commit` → `git push origin main`
3. VercelがPushを検知し自動デプロイ（数十秒）
