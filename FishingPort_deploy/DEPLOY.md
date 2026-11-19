# Render.com での公開手順

このフォルダは、そのまま GitHub にアップロードして Render で公開できる最小構成です。

## 1. GitHub にアップロード

1. GitHub で新しいリポジトリを作成（Public / Private どちらでも可、README 等は追加しない）
2. 「Add file」→「Upload files」
3. **このフォルダの中身をすべて選択して**ドラッグ（フォルダごとではなく、中身を入れ子にしない）
   - ファイル数は 100 未満なので、Web からのアップロードで問題ありません
4. Commit

## 2. Render で公開

1. https://render.com にサインイン（GitHub 連携）
2. **New +** → **Web Service** → 作成したリポジトリを選択
3. 設定は次のとおり（`render.yaml` があるので自動で入る場合もあります）

| 項目 | 値 |
| --- | --- |
| Language / Runtime | Node |
| Build Command | `npm ci && npm run build` |
| Start Command | `npm start` |
| Health Check Path | `/` |
| Plan | Free |

4. 環境変数は**設定不要**（`NODE_VERSION=20` だけ任意で追加）
5. **Create Web Service** → 3〜5分でビルド完了、`https://<name>.onrender.com` が公開URLになります

## 3. 公開後の注意

- 無料プランは 15 分アクセスが無いとスリープし、次のアクセスで 30〜60 秒かかります
- 位置情報（現在地）は HTTPS 必須ですが、Render は自動で HTTPS になります
- Google マップを使う場合は、Render の Environment に `GOOGLE_MAPS_API_KEY` を追加して再デプロイ
- 独自ドメインは Settings → Custom Domains

## ファイル構成

```
app/            画面とAPI（天気・経路）
components/     UI部品
lib/            計算ロジック
public/data/    港データ・魚データ（JSON）
data/           魚データのひな形
scripts/        港データ再生成スクリプト（デプロイには不要）
```

港データ（3,925件）と魚データ（51種）は `public/data/` に同梱済みなので、
デプロイ時に追加のダウンロードは発生しません。
