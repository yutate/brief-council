---
type: Web Tool
title: Brief Council
description: 荒い入力をAIとの対話で診断・戦略立案・専門家レビューし、一枚のExecutive Briefへ圧縮するマーケティングブリーフ作成ツール。
resource: https://yutate.github.io/brief-council/
tags: [marketing, ai, brief, claude, gemini, github-pages]
timestamp: 2026-07-22T00:00:00Z
---

Brief Council は、会議メモ・依頼メール・思いつきなど整理されていない入力から、AIとの対話を通じて一枚のExecutive Briefへ圧縮するシングルページツール。依存ライブラリを持たない単一HTMLファイルで、GitHub Pagesでホスティングする。

5ステップ（Create → Diagnose → Choose → Council → Final Brief）を通じ、診断・戦略案・5名の専門家視点によるレビュー・最終ブリーフ生成のすべてをAIが担う。

# Steps

1. **Create** — プロジェクトの素材（荒い入力＋任意のコンテキスト：プロジェクト名、商品/サービス、最優先ビジネス成果、実施時期）を入力する。
2. **Diagnose** — AIがブリーフの完成度を0〜100でスコアリングし、目的の混在・不足情報・リスク・強みを指摘、確認すべき3つの問いを提示する。
3. **Choose** — 診断結果と3つの問いへの回答をもとに、AIが互いに異なる3つの戦略的方向性を提案する。
4. **Council** — 選んだ戦略に対し、5名のペルソナ（Growth CMO / Consumer / Creative / Effectiveness / Red Team）がAIでレビューし、合意点を要約する。
5. **Final Brief** — One-page Executive BriefとQuality Score（8指標）をAIが生成する。Markdown保存・クリップボードコピーに対応。

# Usage

1. [https://yutate.github.io/brief-council/](https://yutate.github.io/brief-council/) を開く。
2. 左の「AI設定」で使用するAIを選び、対応するAPIキーを保存する（保存ボタンを押すと確認メッセージが表示される）。
3. Step 1で情報を入力し、「診断する」から順にステップを進める。

「サンプルを読み込む」で、架空の習慣化アプリを題材にしたデモ入力を試すこともできる。

# AI Configuration

APIキーはブラウザのlocalStorageにのみ保存され、選択したAIへ直接送信される。サーバー側での保存・中継は行わない。

| 選択肢 | Provider | デフォルトModel ID |
|---|---|---|
| Claude Sonnet | Anthropic | `claude-sonnet-5` |
| Claude Haiku | Anthropic | `claude-haiku-4-5` |
| Gemini 2.5 Flash | Google | `gemini-2.5-flash` |
| Gemini 2.5 Flash Lite | Google | `gemini-2.5-flash-lite` |

Model IDは設定パネルの「モデルID（変更可）」欄で上書きできる。Google側のモデル非推奨化・前倒し停止に追従するための逃げ道として用意している。

# Architecture

- 単一HTMLファイル（`index.html`）。外部ライブラリへの依存なし。
- Claude呼び出し: `POST https://api.anthropic.com/v1/messages`。`anthropic-dangerous-direct-browser-access: true` ヘッダーを付与し、ブラウザから直接叩く。
- Gemini呼び出し: `POST https://generativelanguage.googleapis.com/v1beta/models/{model}:generateContent`。`responseMimeType: application/json` でJSON出力を強制。
- 各ステップの出力はJSONで受け取り、クライアント側でパースして描画する。
- ホスティング: GitHub Pages（`main` ブランチ / root）。

# Deploy

```bash
cd ~/brief-council
cp /storage/emulated/0/Download/brief_council.html index.html
git add index.html
git commit -m "update"
git pull --rebase
git push
```

# Citations

[1] [Claude's API now supports CORS requests, enabling client-side applications](https://simonwillison.net/2024/Aug/23/anthropic-dangerous-direct-browser-access/)
[2] [Claude Platform Docs — Models overview](https://platform.claude.com/docs/en/about-claude/models/overview)
[3] [Gemini API reference](https://ai.google.dev/api)
[4] [Open Knowledge Format (OKF) — An Annotated Guide](https://okf.md/spec/)
