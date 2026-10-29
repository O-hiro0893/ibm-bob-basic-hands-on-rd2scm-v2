# 実装計画書: FAQサイト追加改修（ロゴ・リンク・テーマカラー・ダークテーマ）

## 基本情報

* 仕様書: 20261029-faq-site-v2-spec.md
* 作成日時: 2026-10-29T14:46:38+09:00
* 更新日時: 2026-10-29T14:46:38+09:00
* 作成者:
* ステータス: Approved

## 変更ファイル一覧

| ファイル     | 変更種別 | 変更概要                                                   |
| ------------ | -------- | ---------------------------------------------------------- |
| index.html   | 修正     | ヘッダーHTML変更・CSS更新（グラジェント・ダークテーマ）・JS追加（テーマ切り替え） |

## 実装手順

1. **ヘッダーHTML変更**
   - ヘッダーを `position: relative` にして左右レイアウトに対応
   - 左上にロゴ用 `<a>` タグ（`href="https://www.ibm.com/products/bob"`, `target="_blank"`）を追加
   - `<a>` 内に `<img src="bob.svg" alt="Bob" height="48">` を配置
   - 右上にダークテーマトグルボタン `<button id="theme-toggle">` を配置
   - タイトル・サブタイトルは中央配置を維持

2. **CSSのテーマカラー更新**
   - `header` の `background` を `linear-gradient(135deg, #0f62fe 0%, #a56eff 100%)` に変更
   - `.tab-btn.active` の `background` を同グラジェントに、`border-color` を `#6929c4` に変更
   - `.category-title` の `color` を `#6929c4`、`border-left-color` を `#0f62fe` に変更
   - `#search-input:focus` の `border-color` を `#6929c4` に変更

3. **ダークテーマCSS追加**
   - `[data-theme="dark"]` セレクタで以下を上書き:
     - `body`: 背景 `#161616`、テキスト `#f4f4f4`
     - `#search-input`: 背景 `#262626`、テキスト `#f4f4f4`、ボーダー `#393939`
     - `.tab-btn`: 背景 `#262626`、テキスト `#c6c6c6`、ボーダー `#393939`
     - `.faq-list`: 背景 `#262626`、ボーダー `#393939`
     - `.faq-item + .faq-item`: ボーダー `#393939`
     - `.faq-question`: テキスト `#f4f4f4`
     - `.faq-question:hover`: 背景 `#393939`
     - `.faq-answer`: テキスト `#c6c6c6`、ボーダー `#393939`
     - `.faq-answer strong`: テキスト `#f4f4f4`
     - `.category-title`: `color: #a56eff`、`border-left-color: #a56eff`
     - `#no-results`: テキスト `#6f6f6f`

4. **テーマトグルボタンCSS追加**
   - ヘッダー右上に `position: absolute; right` で配置
   - ボタンスタイル: 丸形、背景透過、白アウトライン
   - ホバー時に軽い背景色

5. **テーマ切り替えJavaScript追加**
   - ページ読み込み時に `localStorage.getItem("theme")` を確認し、保存済みテーマを適用
   - トグルボタンクリック時: `<html>` の `data-theme` を `dark`/（なし）で切り替え、ボタンアイコンを `☀️`/`🌙` で切り替え、`localStorage.setItem("theme", ...)` で保存

## GUI実装

* 対象コンポーネント: `index.html` 内のヘッダー・全体CSS・テーマ切り替えJS
* スタイル: インラインCSS（`<style>` タグ）を更新
* 状態管理: `localStorage` でテーマ永続化、`<html data-theme>` 属性で状態管理
* 画面遷移: なし（単一ページ内のテーマ切り替えのみ）
* レスポンシブ対応: ヘッダーのロゴ・ボタンはスマートフォンでも崩れないよう配慮

## テストの追加・更新

自動テストなし。以下を手動で確認する。

- ヘッダー左上にBobロゴが表示される
- ロゴクリックで `https://www.ibm.com/products/bob` が新タブで開く
- ヘッダーにグラジェントが適用されている
- トグルボタンでダーク/ライトが切り替わる
- ページリロード後もテーマが維持される
- ダークテーマ時にすべての文字が読める（コントラスト確認）
- 既存機能（検索・アコーディオン・カテゴリタブ）が引き続き動作する

## 検証計画

1. Chromeで `index.html` を開き全機能を手動確認
2. 開発者ツールでスマートフォン表示を確認

## 移行・ロールバック

`index.html` への変更のみ。Gitで変更前のコミットに戻すことでロールバック可能。

## 未解決事項

なし
