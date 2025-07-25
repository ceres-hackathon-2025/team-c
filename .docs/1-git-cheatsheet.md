# Git基本操作チートシート

## 🤔 そもそもGitって何？

**Git = コードの変更履歴を管理するツール**

想像してみてください：
- 📄 Word文書で「最終版.docx」「最終版_修正.docx」「本当の最終版.docx」みたいなファイルを作った経験ありませんか？
- 🎮 ゲームのセーブデータのように、コードの「セーブポイント」を作れるのがGit
- 📚 過去のバージョンに戻ったり、誰がいつ何を変更したかが分かる

## 🌳 ブランチって何？

**ブランチ = 並行世界でコード開発**

```
main（メインブランチ）: 動く完成品
    ↓
    ├── login-feature（ログイン機能開発中）
    ├── design-update（デザイン変更中）
    └── bug-fix（バグ修正中）
```

**なぜブランチを使うの？**
- 🛡️ **安全性**: メインのコードを壊さない
- 🔄 **並行作業**: 複数の機能を同時開発
- 🧪 **実験**: 失敗しても元に戻せる

**例：ログイン機能を作る場合**
1. mainブランチから「login-feature」ブランチを作成
2. login-featureで安全にコード開発
3. 完成したらmainブランチに統合（マージ）

## 🎯 これだけ覚えれば開発できる！

### 🌿 ブランチを切る（新機能開発用）
```bash
git checkout -b memo-feature
```
→ `memo-feature`という名前の新しいブランチを作成して移動

**なぜブランチを切るの？**
- メインのコードを壊さずに新機能を開発できる
- 複数人での開発時に衝突を避けられる

### 📁 変更をステージに追加
```bash
git add .
```
→ 全ての変更ファイルをコミット対象に追加（よく使う）

特定ファイルだけ追加したい場合：
```bash
git add app/Http/Controllers/MemoController.php
```

### 💾 変更をコミット
```bash
git commit -m "メモ機能を追加"
```
→ 変更内容を記録（コミットメッセージは分かりやすく）

**良いコミットメッセージの例：**
- ✅ `メモ作成機能を追加`
- ✅ `バリデーションエラーを修正`
- ❌ `修正` `update` `fix`（何を修正したか分からない）

### ☁️ リモートにプッシュ
```bash
git push origin memo-feature
```
→ 作業したブランチをGitHub等にアップロード

### 🔄 メインブランチにマージ
```bash
git checkout main
git merge memo-feature
```
→ メインブランチに戻って、作業ブランチの変更を統合

## 🚀 基本的な開発フロー

### 1日の作業パターン
```bash
# 1. 新機能用のブランチを作成
git checkout -b user-login

# 2. コードを編集（VSCodeなどで）

# 3. 変更をステージに追加
git add .

# 4. コミット
git commit -m "ユーザーログイン機能を追加"

# 5. リモートにプッシュ
git push origin user-login

# 6. 完成したらメインブランチにマージ
git checkout main
git merge user-login
```

## 🆘 困ったときのコマンド

### 現在の状況を確認
```bash
git status
```
→ どのファイルが変更されているか確認

### どのブランチにいるか確認
```bash
git branch
```
→ 現在のブランチに`*`がつく

### メインブランチに戻る
```bash
git checkout main
```

## 💡 よくある間違いと対処法

### 間違ったブランチで作業してしまった
```bash
# 変更をコミットしてから移動
git add .
git commit -m "作業内容"
git checkout 正しいブランチ名
git merge 間違ったブランチ名
```

### コミットメッセージを間違えた
```bash
git commit --amend -m "正しいメッセージ"
```

## 🎯 覚える順番

1. **まずはこれだけ**: `git add .` → `git commit -m "メッセージ"` → `git push`
2. **慣れてきたら**: ブランチを使った開発フロー
3. **余裕があれば**: エラー対処法

## 🚨 絶対にやってはいけないこと

- ❌ メインブランチで直接作業（必ずブランチを切る）
- ❌ 意味のないコミットメッセージ
- ❌ 動かないコードをコミット

## 📋 GitHub PR（プルリクエスト）について

### 🤔 PRって何？
**PR = 「このコード変更をメインブランチに取り込んでください」というお願い**

### 📅 PRを作るタイミング
```bash
# 1. ブランチで作業完了
git checkout -b memo-feature
# コード編集
git add .
git commit -m "メモ機能を追加"
git push origin memo-feature

# 2. この時点でPRを作成！
```

### 🌐 PR作成方法（GitHub）

**方法1: ブラウザで作成**
1. GitHubのリポジトリページを開く
2. 「Compare & pull request」ボタンをクリック
3. タイトルと説明を記入
4. 「Create pull request」をクリック

**方法2: コマンドラインで作成（GitHub CLI）**
```bash
# GitHub CLIをインストール済みの場合
gh pr create --title "メモ機能を追加" --body "メモの作成・一覧機能を実装しました"
```

### ✅ 良いPRの書き方

**タイトル例:**
- ✅ `メモ機能を追加`
- ✅ `ユーザーログインのバグを修正`
- ❌ `update` `fix`

**説明欄に書くこと:**
```markdown
## 変更内容
- メモ作成機能を追加
- メモ一覧表示機能を追加
- バリデーション機能を追加

## 確認方法
1. `/memos/create` にアクセス
2. タイトルと内容を入力
3. 保存ボタンをクリック
4. 一覧ページで確認

## スクリーンショット
（必要に応じて）
```

### 🔄 PRマージ後の流れ
```bash
# 1. ローカルのmainブランチを最新に
git checkout main
git pull origin main

# 2. 作業ブランチを削除（任意）
git branch -d memo-feature
```

## 🎉 これで安全に開発できます！
ブランチを切って → 作業して → コミットして → プッシュ → **PR作成** → マージ
この流れを覚えれば完璧です！
