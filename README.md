# talk-to-structure

> 話すだけで、情報の"構造"が見えてくる

AIエージェントとの会話を通じて、頭の中にある業務情報やアイデアを整理し、
ズーム・ドラッグ操作可能なインタラクティブな関係図として出力する Agent Skill です。

**Claude Code と OpenAI Codex の両方で動きます。** スキルの実体は1つ（`.claude/skills/`）で、
Codex 用の発見パス（`.agents/skills/`）にはシンボリックリンクを張ってあるだけなので、
二重管理・ドリフトの心配なく同じ中身を両ツールから使えます（[Agent Skills 標準](https://agentskills.io)準拠）。

エンジニアでなくても使えます。専門用語は一切出てきません。

---

## デモ

```
あなた：「ライブハウスの予約管理について整理したい」

AI：「何について整理したいですか？業務の話でもアイデアでも...」
         ↓（5フェーズのインタビュー）
AI：「output/index.html をブラウザで開いてください」
```

→ ブラウザで操作できる関係図が生成されます

---

## インストール

```bash
git clone https://github.com/Cyberdog-AI-Lab/talk-to-structure
cd talk-to-structure
```

このフォルダを **Claude Code** または **Codex** で開くと、スキルが自動的に認識されます
（Claude Code は `.claude/skills/`、Codex は `.agents/skills/` からスキルを読み込みます）。

---

## 使い方

Claude Code / Codex で以下のように話しかけるだけです：

```
「〇〇の業務について情報を整理したい」
「〇〇システムの構造を図にしたい」
「頭の中のアイデアを整理したい」
```

AIが5フェーズのインタビューを開始します。

### インタビューの流れ

| フェーズ | 内容 |
|--------|------|
| 0. 全体像 | 何について整理したいかを確認 |
| 1. もの出し | 業務に登場する「もの・人・出来事」を洗い出す |
| 2. 中身の確認 | 各ものが持つ情報項目を確認 |
| 3. つながりの確認 | ものとものの関係を確認 |
| 4. ルールの確認 | 選択肢や条件を確認（省略可） |

完了すると `output/index.html` と `output/data.json` が生成されます。

---

## 出力ファイル

```
output/
├── index.html   ← ブラウザで開く関係図（ズーム・ドラッグ対応）
└── data.json    ← 構造化されたJSONデータ
```

### index.html の操作方法

- **ズーム：** マウスホイール
- **移動：** ドラッグ
- **詳細確認：** ノード（四角）にホバー → 項目一覧・説明・ルールが表示
- **関係の確認：** エッジ（矢印）にホバー → 関係の説明が表示

---

## 生成されるJSONの構造

```json
{
  "meta": { "title": "...", "description": "..." },
  "entities": [
    {
      "id": "customer", "label": "顧客", "type": "master",
      "description": "...",
      "attributes": [
        { "name": "顧客ID", "pk": true, "description": "..." },
        { "name": "会社名", "required": true, "description": "..." }
      ]
    }
  ],
  "relations": [
    { "id": "r1", "from": "customer", "to": "order",
      "label": "発注する", "cardinality": "1:N", "description": "..." }
  ],
  "constraints": [
    { "entity": "order", "attribute": "ステータス",
      "type": "enum", "values": ["受付中", "確定", "キャンセル"] }
  ]
}
```

---

## ディレクトリ構成

```
talk-to-structure/
├── README.md
├── .claude/
│   └── skills/
│       └── talk-to-structure/
│           ├── SKILL.md              ← スキル本体（実体はここ1つだけ）
│           └── assets/
│               └── graph-template.html  ← 関係図のHTMLテンプレート
├── .agents/
│   └── skills/
│       └── talk-to-structure → ../../.claude/skills/talk-to-structure  ← Codex用シンボリックリンク（中身は共通）
├── examples/
│   └── order-management.json        ← サンプル出力
└── output/                          ← 生成ファイルの出力先
```

> `.agents/skills/talk-to-structure` は `.claude/skills/talk-to-structure` へのシンボリックリンクです。
> Claude Code と Codex で発見パスが違うだけで、参照する SKILL.md は同じ1つ。
> **シンボリックリンクを実ファイルに置き換えたり、中身を分岐させたりしないでください。**

---

## 開発・研究

[Cyberdog AI Lab](https://github.com/Cyberdog-AI-Lab) が研究・開発しています。

> AIパラダイムシフトを自ら体現し、次世代エンジニアの新しい働き方を創出する

---

## ライセンス

MIT
