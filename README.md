# multi-agent-guild

<div align="center">

**Claude Code 冒険者ギルド運営システム**

*依頼書1つで、8体のAIエージェントが並列稼働*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude-Code-blueviolet)](https://claude.ai)
[![tmux](https://img.shields.io/badge/tmux-required-green)](https://github.com/tmux/tmux)

[English](readme_en.md) | [日本語](README.md)

</div>

---

## これは何？

**multi-agent-guild** は、複数の Claude Code インスタンスを同時に実行し、冒険者ギルドの運営構造として統率するシステムです。
本リポジトリはフォーク元から Unity 向けに特化した内容へ調整しています。

**なぜ使うのか？**
- 1つの依頼で、8体の冒険者が並列で実行
- 待ち時間なし - クエストがバックグラウンドで進行中も次の依頼を出せる
- AIがセッションを跨いであなたの好みを記憶（Memory MCP）
- クエスト掲示板（dashboard.md）でリアルタイム進行確認

**ギルドの流れ:** 依頼 → クエスト → 冒険 → 報告

```
      あなた（依頼者）
           │
           ▼ 依頼書を提出
    ┌─────────────┐
    │   GUILDMASTER    │  ← 依頼を受け取り、即座に委譲
    └──────┬──────┘
           │ YAMLファイル + tmux
    ┌──────▼──────┐
    │    RECEPTIONIST     │  ← クエストを分解し配分
    └──────┬──────┘
           │
  ┌─┬─┬─┬─┴─┬─┬─┬─┐
  │1│2│3│4│5│6│7│8│  ← 8体の冒険者が並列実行
  └─┴─┴─┴─┴─┴─┴─┴─┘
      ADVENTURERS
```

---

## 🚀 クイックスタート

### 🪟 Windowsユーザー（最も一般的）

<table>
<tr>
<td width="60">

**Step 1**

</td>
<td>

📥 **リポジトリをダウンロード**

[ZIPダウンロード](https://github.com/yohey-w/multi-agent-guild/archive/refs/heads/main.zip) して `C:\tools\multi-agent-guild` に展開

*または git を使用:* `git clone https://github.com/yohey-w/multi-agent-guild.git C:\tools\multi-agent-guild`

</td>
</tr>
<tr>
<td>

**Step 2**

</td>
<td>

🖱️ **`install.bat` を実行**

右クリック→「管理者として実行」（WSL2が未インストールの場合）。WSL2 + Ubuntu をセットアップします。

</td>
</tr>
<tr>
<td>

**Step 3**

</td>
<td>

🐧 **Ubuntu を開いて以下を実行**（初回のみ）

```bash
cd /mnt/c/tools/multi-agent-guild
./first_setup.sh
```

</td>
</tr>
<tr>
<td>

**Step 4**

</td>
<td>

✅ **出発！**

```bash
./shutsujin_departure.sh
```

</td>
</tr>
</table>

#### 📅 毎日の起動（初回セットアップ後）

**Ubuntuターミナル**（WSL）を開いて実行：

```bash
cd /mnt/c/tools/multi-agent-guild
./shutsujin_departure.sh
```

---

<details>
<summary>🐧 <b>Linux / Mac ユーザー</b>（クリックで展開）</summary>

### 初回セットアップ

```bash
# 1. リポジトリをクローン
git clone https://github.com/yohey-w/multi-agent-guild.git ~/multi-agent-guild
cd ~/multi-agent-guild

# 2. スクリプトに実行権限を付与
chmod +x *.sh

# 3. 初回セットアップを実行
./first_setup.sh
```

### 毎日の起動

```bash
cd ~/multi-agent-guild
./shutsujin_departure.sh
```

</details>

---

<details>
<summary>❓ <b>WSL2とは？なぜ必要？</b>（クリックで展開）</summary>

### WSL2について

**WSL2（Windows Subsystem for Linux）** は、Windows内でLinuxを実行できる機能です。このシステムは `tmux`（Linuxツール）を使って複数のAIエージェントを管理するため、WindowsではWSL2が必要です。

### WSL2がまだない場合

問題ありません！`install.bat` を実行すると：
1. WSL2がインストールされているかチェック（なければ自動インストール）
2. Ubuntuがインストールされているかチェック（なければ自動インストール）
3. 次のステップ（`first_setup.sh` の実行方法）を案内

**クイックインストールコマンド**（PowerShellを管理者として実行）：
```powershell
wsl --install
```

その後、コンピュータを再起動して `install.bat` を再実行してください。

</details>

---

<details>
<summary>📋 <b>スクリプトリファレンス</b>（クリックで展開）</summary>

| スクリプト | 用途 | 実行タイミング |
|-----------|------|---------------|
| `install.bat` | Windows: WSL2 + Ubuntu のセットアップ | 初回のみ |
| `first_setup.sh` | tmux、Node.js、Claude Code CLI のインストール + Memory MCP設定 | 初回のみ |
| `shutsujin_departure.sh` | tmuxセッション作成 + Claude Code起動 + 指示書読み込み | 毎日 |

### `install.bat` が自動で行うこと：
- ✅ WSL2がインストールされているかチェック（未インストールなら案内）
- ✅ Ubuntuがインストールされているかチェック（未インストールなら案内）
- ✅ 次のステップ（`first_setup.sh` の実行方法）を案内

### `shutsujin_departure.sh` が行うこと：
- ✅ tmuxセッションを作成（guildmaster + multiagent）
- ✅ 全エージェントでClaude Codeを起動
- ✅ 各エージェントに指示書を自動読み込み
- ✅ キューファイルをリセットして新しい状態に

**実行後、全エージェントが即座に依頼を受け付ける準備完了！**

</details>

---

<details>
<summary>🔧 <b>必要環境（手動セットアップの場合）</b>（クリックで展開）</summary>

依存関係を手動でインストールする場合：

| 要件 | インストール方法 | 備考 |
|------|-----------------|------|
| WSL2 + Ubuntu | PowerShellで `wsl --install` | Windowsのみ |
| Ubuntuをデフォルトに設定 | `wsl --set-default Ubuntu` | スクリプトの動作に必要 |
| tmux | `sudo apt install tmux` | ターミナルマルチプレクサ |
| Node.js v20+ | `nvm install 20` | Claude Code CLIに必要 |
| Claude Code CLI | `npm install -g @anthropic-ai/claude-code` | Anthropic公式CLI |

</details>

---

### ✅ セットアップ後の状態

どちらのオプションでも、**10体のAIエージェント**が自動起動します：

| エージェント | 役割 | 数 |
|-------------|------|-----|
| 🛡️ ギルドマスター（Guildmaster） | 依頼統括 - 依頼を受け取る | 1 |
| 📋 受付官（Receptionist） | 上級冒険者 - クエストを分解しミッションを割当 | 1 |
| ⚔️ 冒険者（Adventurer） | 冒険者 - 並列でクエスト実行 | 8 |

tmuxセッションが作成されます：
- `guildmaster` - ここに接続して依頼を出す
- `multiagent` - 冒険者がバックグラウンドで稼働

---

## 📖 基本的な使い方

### Step 1: ギルドマスターに接続

`shutsujin_departure.sh` 実行後、全エージェントが自動的に指示書を読み込み、作業準備完了となります。

新しいターミナルを開いてギルドマスターに接続：

```bash
tmux attach-session -t guildmaster
```

### Step 2: 最初の依頼を出す

ギルドマスターは既に初期化済み！そのまま依頼を出せます：

```
JavaScriptフレームワーク上位5つを調査して比較表を作成せよ
```

ギルドマスターは：
1. クエストをYAMLファイルに書き込む
2. 受付官（受付担当）に通知
3. 即座にあなたに制御を返す（待つ必要なし！）

その間、受付官はクエストを冒険者に分配し、並列実行します。

### Step 3: 進捗を確認

エディタでクエスト掲示板（`dashboard.md`）を開いてリアルタイム状況を確認：

```markdown
## 進行中
| 冒険者 | クエスト | 進行状況 |
|----------|--------|------|
| 冒険者 1 | React調査 | 実行中 |
| 冒険者 2 | Vue調査 | 実行中 |
| 冒険者 3 | Angular調査 | 完了 |
```

---

## ✨ 主な特徴

### ⚡ 1. 並列実行

1つの依頼で最大8つの並列クエストを生成：

```
あなた: 「5つのMCPサーバを調査せよ」
→ 5体の冒険者が同時に調査開始
→ 数時間ではなく数分で報告が届く
```

### 🔄 2. ノンブロッキングワークフロー

ギルドマスターは即座に委譲して、あなたに制御を返します：

```
あなた: 依頼 → ギルドマスター: 委譲 → あなた: 次の依頼をすぐ出せる
                                    ↓
                    冒険者: バックグラウンドで実行
                                    ↓
                    掲示板: 報告を表示
```

長いクエストの完了を待つ必要はありません。

---

## 🎮 Unityギルド特化

本ギルドは Unity ゲーム開発に特化しています。標準 Skill は以下に定義済み：

- `skills/unity-adventurer-skills/`

冒険者は Unity の専門クラス（エンジニア / AI / UI / 最適化 / QA）としてクエストに参加します。Skill 定義には目的・入出力・モデル適性が含まれます。

### モデル切替（Unity）

- **設計/アーキテクチャ/原因究明** → 推論重視モデル
- **C#実装/Unity API作業** → コード特化モデル
- **要約/チェックリスト** → 高速モデル

### 🧠 3. セッション間記憶（Memory MCP）

AIがあなたの好みを記憶します：

```
セッション1: 「シンプルな方法が好き」と伝える
            → Memory MCPに保存

セッション2: 起動時にAIがメモリを読み込む
            → 複雑な方法を提案しなくなる
```

### 📡 4. イベント駆動（ポーリングなし）

エージェントはYAMLファイルで通信し、tmux send-keysで互いを起こします。
**ポーリングループでAPIコールを浪費しません。**

### 📸 5. スクリーンショット連携

VSCode拡張のClaude Codeはスクショを貼り付けて事象を説明できます。このCLIシステムでも同等の機能を実現：

```
# config/settings.yaml でスクショフォルダを設定
screenshot:
  path: "/mnt/c/Users/あなたの名前/Pictures/Screenshots"

# ギルドマスターに伝えるだけ:
あなた: 「最新のスクショを見ろ」
あなた: 「スクショ2枚見ろ」
→ AIが即座にスクリーンショットを読み取って分析
```

**💡 Windowsのコツ:** `Win + Shift + S` でスクショが撮れます。保存先を `settings.yaml` のパスに合わせると、シームレスに連携できます。

こんな時に便利：
- UIのバグを視覚的に説明
- エラーメッセージを見せる
- 変更前後の状態を比較

### 📁 6. コンテキスト管理

効率的な知識共有のため、3層構造のコンテキストを採用：

| レイヤー | 場所 | 用途 |
|---------|------|------|
| Memory MCP | `memory/guildmaster_memory.jsonl` | セッションを跨ぐ長期記憶 |
| グローバル | `memory/global_context.md` | システム全体の設定、依頼者の好み |
| プロジェクト | `context/{project}.md` | プロジェクト固有の知見 |

この設計により：
- どの冒険者でも任意のプロジェクトを担当可能
- エージェント切り替え時もコンテキスト継続
- 関心の分離が明確
- セッション間の知識永続化

### 汎用コンテキストテンプレート

すべてのプロジェクトで同じ7セクション構成のテンプレートを使用：

| セクション | 目的 |
|-----------|------|
| What | プロジェクトの概要説明 |
| Why | 目的と成功の定義 |
| Who | 関係者と責任者 |
| Constraints | 期限、予算、制約 |
| Current State | 進捗、次のアクション、ブロッカー |
| Decisions | 決定事項と理由の記録 |
| Notes | 自由記述のメモ・気づき |

この統一フォーマットにより：
- どのエージェントでも素早くオンボーディング可能
- すべてのプロジェクトで一貫した情報管理
- 冒険者間の作業引き継ぎが容易

---

### 🧠 モデル設定

| エージェント | モデル | 思考モード | 理由 |
|-------------|--------|----------|------|
| ギルドマスター | Opus | 無効 | 委譲と掲示板更新に深い推論は不要 |
| 受付官 | デフォルト | 有効 | クエスト分配には慎重な判断が必要 |
| 冒険者 | デフォルト | 有効 | 実装作業にはフル機能が必要 |

ギルドマスターは `MAX_THINKING_TOKENS=0` で拡張思考を無効化し、高レベルな判断にはOpusの能力を維持しつつ、レイテンシとコストを削減。

---

## 🎯 設計思想

### なぜ階層構造（ギルドマスター→受付官→冒険者）なのか

1. **即座の応答**: ギルドマスターは即座に委譲し、あなたに制御を返す
2. **並列実行**: 受付官が複数の冒険者に同時分配
3. **単一責任**: 各役割が明確に分離され、混乱しない
4. **スケーラビリティ**: 冒険者を増やしても構造が崩れない
5. **障害分離**: 1体の冒険者が失敗しても他に影響しない
6. **依頼者への報告一元化**: ギルドマスターだけが依頼者とやり取りするため、情報が整理される

### なぜ YAML + send-keys なのか

1. **進行状況の永続化**: YAMLファイルで構造化通信し、エージェント再起動にも耐える
2. **ポーリング不要**: イベント駆動でAPIコストを削減
3. **割り込み防止**: エージェント同士やあなたの入力への割り込みを防止
4. **デバッグ容易**: 依頼者がYAMLを直接読んで状況把握できる
5. **競合回避**: 各冒険者に専用ファイルを割り当て

### なぜクエスト掲示板（dashboard.md）は受付官のみが更新するのか

1. **単一更新者**: 競合を防ぐため、更新責任者を1人に限定
2. **情報集約**: 受付官は全冒険者の報告を受ける立場なので全体像を把握
3. **一貫性**: すべての更新が1つの品質ゲートを通過
4. **割り込み防止**: ギルドマスターが更新すると、依頼者の入力中に割り込む恐れあり

---

## 🛠️ スキル

初期状態ではスキルはありません。
運用中はクエスト掲示板（dashboard.md）の「スキル化候補」から承認して増やしていきます。

スキルは `/スキル名` で呼び出し可能。ギルドマスターに「/スキル名 を実行」と伝えるだけ。

### スキルの思想

**1. スキルはコミット対象外**

`.claude/commands/` 配下のスキルはリポジトリにコミットしない設計。理由：
- 各ユーザの業務・ワークフローは異なる
- 汎用的なスキルを押し付けるのではなく、ユーザが自分に必要なスキルを育てていく

**2. スキル取得の手順**

```
冒険者が作業中にパターンを発見
    ↓
クエスト掲示板（dashboard.md）の「スキル化候補」に上がる
    ↓
依頼者（あなた）が内容を確認
    ↓
承認すれば受付官に指示してスキルを作成
```

スキルはユーザ主導で増やすもの。自動で増えると管理不能になるため、「これは便利」と判断したものだけを残す。

---

## 🔌 MCPセットアップガイド

MCP（Model Context Protocol）サーバはClaudeの機能を拡張します。セットアップ方法：

### MCPとは？

MCPサーバはClaudeに外部ツールへのアクセスを提供します：
- **Notion MCP** → Notionページの読み書き
- **GitHub MCP** → PR作成、Issue管理
- **Memory MCP** → セッション間で記憶を保持

### MCPサーバのインストール

以下のコマンドでMCPサーバを追加：

```bash
# 1. Notion - Notionワークスペースに接続
claude mcp add notion -e NOTION_TOKEN=your_token_here -- npx -y @notionhq/notion-mcp-server

# 2. Playwright - ブラウザ自動化
claude mcp add playwright -- npx @playwright/mcp@latest
# 注意: 先に `npx playwright install chromium` を実行してください

# 3. GitHub - リポジトリ操作
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=your_pat_here -- npx -y @modelcontextprotocol/server-github

# 4. Sequential Thinking - 複雑な問題を段階的に思考
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking

# 5. Memory - セッション間の長期記憶（推奨！）
# ✅ first_setup.sh で自動設定済み
# 手動で再設定する場合:
claude mcp add memory -e MEMORY_FILE_PATH="$PWD/memory/guildmaster_memory.jsonl" -- npx -y @modelcontextprotocol/server-memory
```

### インストール確認

```bash
claude mcp list
```

全サーバが「Connected」と表示されるはずです。

---

## 🌍 実用例

### 例1: 調査クエスト

```
あなた: 「AIコーディングアシスタント上位5つを調査して比較せよ」

実行される処理:
1. ギルドマスターが受付官に委譲
2. 受付官が割り当て:
   - 冒険者1: GitHub Copilotを調査
   - 冒険者2: Cursorを調査
   - 冒険者3: Claude Codeを調査
   - 冒険者4: Codeiumを調査
   - 冒険者5: Amazon CodeWhispererを調査
3. 5体が同時に調査
4. 報告がクエスト掲示板（dashboard.md）に集約
```

### 例2: PoC準備

```
あなた: 「このNotionページのプロジェクトでPoC準備: [URL]」

実行される処理:
1. 受付官がMCP経由でNotionコンテンツを取得
2. 冒険者2: 確認すべき項目をリスト化
3. 冒険者3: 技術的な実現可能性を調査
4. 冒険者4: PoC計画書を作成
5. 全報告がクエスト掲示板（dashboard.md）に集約、会議の準備完了
```

---

## ⚙️ 設定

### 言語設定

`config/settings.yaml` を編集：

```yaml
language: ja   # 日本語のみ
language: en   # 日本語 + 英訳併記
```

---

## 🛠️ 上級者向け

<details>
<summary><b>スクリプトアーキテクチャ</b>（クリックで展開）</summary>

```
┌─────────────────────────────────────────────────────────────────────┐
│                      初回セットアップ（1回だけ実行）                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  install.bat (Windows)                                              │
│      │                                                              │
│      ├── WSL2のチェック/インストール案内                              │
│      └── Ubuntuのチェック/インストール案内                            │
│                                                                     │
│  first_setup.sh (Ubuntu/WSLで手動実行)                               │
│      │                                                              │
│      ├── tmuxのチェック/インストール                                  │
│      ├── Node.js v20+のチェック/インストール (nvm経由)                │
│      ├── Claude Code CLIのチェック/インストール                      │
│      └── Memory MCPサーバー設定                                      │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│                      毎日の起動（毎日実行）                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  shutsujin_departure.sh                                             │
│      │                                                              │
│      ├──▶ tmuxセッションを作成                                       │
│      │         • "guildmaster"セッション（1ペイン）                        │
│      │         • "multiagent"セッション（9ペイン、3x3グリッド）        │
│      │                                                              │
│      ├──▶ キューファイルと掲示板をリセット                             │
│      │                                                              │
│      └──▶ 全エージェントでClaude Codeを起動                          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

</details>

<details>
<summary><b>shutsujin_departure.sh オプション</b>（クリックで展開）</summary>

```bash
# デフォルト: フル起動（tmuxセッション + Claude Code起動）
./shutsujin_departure.sh

# セッションセットアップのみ（Claude Code起動なし）
./shutsujin_departure.sh -s
./shutsujin_departure.sh --setup-only

# フル起動 + Windows Terminalタブを開く
./shutsujin_departure.sh -t
./shutsujin_departure.sh --terminal

# ヘルプを表示
./shutsujin_departure.sh -h
./shutsujin_departure.sh --help
```

</details>

<details>
<summary><b>よく使うワークフロー</b>（クリックで展開）</summary>

**通常の毎日の使用：**
```bash
./shutsujin_departure.sh          # 全て起動
tmux attach-session -t guildmaster     # 接続して依頼を出す
```

**デバッグモード（手動制御）：**
```bash
./shutsujin_departure.sh -s       # セッションのみ作成

# 特定のエージェントでClaude Codeを手動起動
tmux send-keys -t guildmaster:0 'claude --dangerously-skip-permissions' Enter
tmux send-keys -t multiagent:0.0 'claude --dangerously-skip-permissions' Enter
```

**クラッシュ後の再起動：**
```bash
# 既存セッションを終了
tmux kill-session -t guildmaster
tmux kill-session -t multiagent

# 新しく起動
./shutsujin_departure.sh
```

</details>

<details>
<summary><b>便利なエイリアス</b>（クリックで展開）</summary>

`first_setup.sh` を実行すると、以下のエイリアスが `~/.bashrc` に自動追加されます：

```bash
alias css='tmux attach-session -t guildmaster'      # ギルドマスターウィンドウの起動
alias csm='tmux attach-session -t multiagent'  # 受付官・冒険者ウィンドウの起動
```

※ エイリアスを反映するには `source ~/.bashrc` を実行するか、PowerShellで `wsl --shutdown` してからターミナルを開き直してください。

</details>

---

## 📁 ファイル構成

<details>
<summary><b>クリックでファイル構成を展開</b></summary>

```
multi-agent-guild/
│
│  ┌─────────────────── セットアップスクリプト ───────────────────┐
├── install.bat               # Windows: 初回セットアップ
├── first_setup.sh            # Ubuntu/Mac: 初回セットアップ
├── shutsujin_departure.sh    # 毎日の起動（指示書自動読み込み）
│  └────────────────────────────────────────────────────────────┘
│
├── instructions/             # エージェント指示書
│   ├── guildmaster.md             # ギルドマスターの指示書
│   ├── receptionist.md               # 受付官の指示書
│   └── adventurer.md           # 冒険者の指示書
│
├── config/
│   └── settings.yaml         # 言語その他の設定
│
├── projects/                # プロジェクト詳細（git対象外、機密情報含む）
│   └── <project_id>.yaml   # 各プロジェクトの全情報（クライアント、クエスト、Notion連携等）
│
├── queue/                    # 通信ファイル
│   ├── guildmaster_to_receptionist.yaml   # ギルドマスターから受付官への依頼
│   ├── tasks/                # 各クエストファイル
│   └── reports/              # 冒険者レポート
│
├── memory/                   # Memory MCP保存場所
├── dashboard.md              # クエスト掲示板（リアルタイム進行状況）
└── CLAUDE.md                 # Claude用プロジェクトコンテキスト
```

</details>

---

## 📂 プロジェクト管理

このシステムは自身の開発だけでなく、**全てのホワイトカラー業務**を管理・実行する。プロジェクトのフォルダはこのリポジトリの外にあってもよい。

### 仕組み

```
config/projects.yaml          # プロジェクト一覧（ID・名前・パス・進行状況のみ）
projects/<project_id>.yaml    # 各プロジェクトの詳細情報
```

- **`config/projects.yaml`**: どのプロジェクトがあるかの一覧（サマリのみ）
- **`projects/<id>.yaml`**: そのプロジェクトの全詳細（クライアント情報、契約、クエスト、関連ファイル、Notionページ等）
- **プロジェクトの実ファイル**（ソースコード、設計書等）は `path` で指定した外部フォルダに配置
- **`projects/` はGit追跡対象外**（クライアントの機密情報を含むため）

### 例

```yaml
# config/projects.yaml
projects:
  - id: my_client
    name: "クライアントXコンサルティング"
    path: "/mnt/c/Consulting/client_x"
    progress: active

# projects/my_client.yaml
id: my_client
client:
  name: "クライアントX"
  company: "X株式会社"
contract:
  fee: "月額"
current_quests:
  - id: quest_001
    name: "システムアーキテクチャレビュー"
    progress: in_progress
```

この分離設計により、ギルドマスターシステムは複数の外部プロジェクトを横断的に統率しつつ、プロジェクトの詳細情報はバージョン管理の対象外に保つことができる。

---

## 🔧 トラブルシューティング

<details>
<summary><b>MCPツールが動作しない？</b></summary>

MCPツールは「遅延ロード」方式で、最初にロードが必要です：

```
# 間違い - ツールがロードされていない
mcp__memory__read_graph()  ← エラー！

# 正しい - 先にロード
ToolSearch("select:mcp__memory__read_graph")
mcp__memory__read_graph()  ← 動作！
```

</details>

<details>
<summary><b>エージェントが権限を求めてくる？</b></summary>

`--dangerously-skip-permissions` 付きで起動していることを確認：

```bash
claude --dangerously-skip-permissions --system-prompt "..."
```

</details>

<details>
<summary><b>冒険者が停止している？</b></summary>

冒険者のペインを確認：
```bash
tmux attach-session -t multiagent
# Ctrl+B の後に数字でペインを切り替え
```

</details>

---

## 📚 tmux クイックリファレンス

| コマンド | 説明 |
|----------|------|
| `tmux attach -t guildmaster` | ギルドマスターに接続 |
| `tmux attach -t multiagent` | 冒険者に接続 |
| `Ctrl+B` の後 `0-8` | ペイン間を切り替え |
| `Ctrl+B` の後 `d` | デタッチ（実行継続） |
| `tmux kill-session -t guildmaster` | ギルドマスターセッションを停止 |
| `tmux kill-session -t multiagent` | 冒険者セッションを停止 |

### 🖱️ マウス操作

`first_setup.sh` が `~/.tmux.conf` に `set -g mouse on` を自動設定するため、マウスによる直感的な操作が可能です：

| 操作 | 説明 |
|------|------|
| マウスホイール | ペイン内のスクロール（出力履歴の確認） |
| ペインをクリック | ペイン間のフォーカス切替 |
| ペイン境界をドラッグ | ペインのリサイズ |

キーボード操作に不慣れな場合でも、マウスだけでペインの切替・スクロール・リサイズが行えます。

---

## 🙏 クレジット

[Claude-Code-Communication](https://github.com/Akira-Papa/Claude-Code-Communication) by Akira-Papa をベースに開発。

---

## 📄 ライセンス

MIT License - 詳細は [LICENSE](LICENSE) を参照。

---

<div align="center">

**AIの軍勢を統率せよ。より速く構築せよ。**

</div>
