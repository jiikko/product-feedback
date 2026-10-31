# プロダクトが増えた時にやること

新しいプロダクトをこのフィードバック窓口リポジトリの対象に加えるとき、以下の **すべて** のファイルを更新する。1 つでも漏らすと「README には載っているが Issue フォームから選べない」「Issue フォームには出るが README に説明がない」などの不整合が発生する。

## 更新対象ファイル

| ファイル | 追加箇所 | 並び順 |
|---|---|---|
| `README.md` | 「対象プロダクト / Supported Products」リスト | LP URL 付きで末尾に追加 (例: `- [Foo](https://jiikko.com/foo/)`) |
| `.github/ISSUE_TEMPLATE/bug_report.yml` | `id: product` の `options:` | アルファベット順 |
| `.github/ISSUE_TEMPLATE/feature_request.yml` | `id: product` の `options:` | アルファベット順 |
| `.github/ISSUE_TEMPLATE/support_question.yml` | `id: product` の `options:` | アルファベット順 |
| `.github/labels.yml` | `product: <名前>` のラベル (名前はフォームの選択肢と完全一致) | 選択肢と同じ順 |

### 更新不要なファイル

- `.github/ISSUE_TEMPLATE/config.yml` — `contact_links` (セキュリティ連絡先など) のみで製品リストは持たない。新プロダクト追加時に触る必要なし

## 確認手順

追加後、以下のコマンドで 5 か所の一覧が揃っているかを検証する (CI の `check-products` も同じものを走らせる):

```bash
ruby bin/check-products
```

`OK: プロダクト N 件を…照合した` の N が追加後の件数なら OK。NG なら、出力にどのファイルの何が足りない / 余分かが出る。

## ラベルの付き方

- `.github/labels.yml` を master へ push すると `labels-sync` ワークフローが GitHub 側のラベルを作る / 更新する
  (skip-delete。ファイルから消してもラベル自体は消えないので、消すときは Web か `gh label delete` で)
- Issue が立つ / 本文が編集されると `product-label` ワークフローが Product の選択に合わせて `product: <名前>` を付け替える。
  `labels.yml` に無い名前は付けず、ワークフローのログに warning を出す

## やらないこと

- ✗ 一部のファイルだけ更新して「あとで揃える」運用
- ✗ Issue テンプレートの `options:` 並び順を崩す (新プロダクトを途中に挿入したい時もアルファベット順を維持)
- ✗ README に App Store URL を直接書く (LP URL `https://jiikko.com/<slug>/` を使う。commit `bc75246` で LP URL に統一済み)

## 関連

- LP URL に統一した経緯: commit `bc75246` (`fix: use LP URLs instead of App Store URLs for product links`)
