# やめドラ Documents (Legal Site)

「やめドラ」のプライバシーポリシーと利用規約の公開用静的サイト。

## 構成

- `_config.yml` — Jekyll 設定（GitHub Pages が自動でビルド）
- `index.md` — ランディング
- `privacy-policy.md` — プライバシーポリシー本文
- `terms-of-service.md` — 利用規約本文

## GitHub Pages へのデプロイ手順

### 前提
- GitHub アカウントを持っている
- `Melt-dev` 開発者名でリポジトリを作りたい場合は、Melt-dev という GitHub 組織または個人アカウントを用意（既存アカウントを Melt-dev 名で使い分けても可）

### Step 1: 公開リポジトリ作成
1. https://github.com/new にアクセス
2. **Repository name**: `yamedora-docs`（任意の名前で OK）
3. **Public** を選択（GitHub Pages 無料利用は public 必須）
4. README は付けない（このフォルダで上書きするため）
5. 「Create repository」

### Step 2: ローカル → GitHub にプッシュ
PowerShell でこの `legal-site/` フォルダの中で：

```powershell
cd C:\Users\pikac\Desktop\private-claudecode\gamble-quit-app\legal-site

# Git リポジトリ初期化
git init
git add .
git commit -m "Initial: privacy policy and terms"

# GitHub のリモート追加（URL は自分のリポジトリのものに置き換え）
git branch -M main
git remote add origin https://github.com/<your-username>/yamedora-docs.git
git push -u origin main
```

### Step 3: GitHub Pages 有効化
1. GitHub のリポジトリページに行く
2. **Settings** タブ
3. 左メニュー「**Pages**」
4. **Source**: `Deploy from a branch`
5. **Branch**: `main` / `/ (root)` を選択
6. **Save**

数分で `https://<your-username>.github.io/yamedora-docs/` でアクセス可能に。

### Step 4: アプリ内 URL の差し替え
`lib/presentation/screens/settings_screen.dart` の以下を実 URL に書き換え：

```dart
const _privacyPolicyUrl =
    'https://example.com/yamedora/privacy-policy';
```

↓

```dart
const _privacyPolicyUrl =
    'https://<your-username>.github.io/yamedora-docs/privacy-policy/';
```

### Step 5: Google Play Console にも登録
Play Console → アプリの内容 → プライバシーポリシー → URL 入力欄に：
```
https://<your-username>.github.io/yamedora-docs/privacy-policy/
```

## ローカルプレビュー（任意）

Jekyll をローカルで動かしたい場合（Ruby 環境が必要）：

```bash
gem install bundler jekyll
cd legal-site
jekyll serve
# → http://localhost:4000 で確認
```

通常は不要。GitHub Pages にプッシュして実際の表示を確認すれば OK。

## 更新方法

`privacy-policy.md` や `terms-of-service.md` を編集 → コミット → プッシュ。
GitHub Pages が自動で再ビルド（数分）。

## トラブル

- **404**: Pages 設定の Branch / 公開ステータスを再確認。Pages タブに公開 URL が表示されるまで数分待つ
- **theme が反映されない**: `_config.yml` の `theme` 設定を確認、`jekyll-theme-cayman` がデフォルトで利用可能
- **日本語が文字化け**: 通常起きないが、もし起きたら `lang: ja` を `_config.yml` に確認

---

公開後、Play Console とアプリ内 URL の差し替えを忘れずに。
