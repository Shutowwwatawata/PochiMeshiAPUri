
---

# 🛠️ 開発環境セットアップ＆作業手順ガイド（Docker + GitHub）


## 👀 開発ルール

* **必ず作業用ブランチで作業し、mainブランチに直接pushしない**（プルリクエスト経由でマージする）
* **1人1ブランチ**が基本（`feature/機能名` 形式）
* **作業前に`git pull`すること**（コンフリクト防止）
* **作業後は`git push`を忘れずに**
* **作業が終わったらプルリクを出す**
* **レビューしてからmainにマージ**
* **Dockerの状態は他の人と共有する**


---

## ✅ 初回セットアップ（初めてプロジェクトに参加する人向け）

### 1. GitHub からリポジトリをクローン

```bash
git clone https://github.com/Shutowwwatawata/PochiMeshiAPUri.git
cd リポジトリ名
```

### 2. Docker イメージをビルド（初回のみ）

```bash
docker　compose build
```

### 3. コンテナを起動

```bash
docker　compose up -d  # -dはバックグラウンドで起動（ログ非表示）
```

### 4. コンテナが起動しているか確認（オプション）

```bash
docker ps
```

---

## 🚀 作業開始時の手順（毎回行う）

### 1. 最新の main ブランチを取得

```bash
git checkout main
git pull origin main
```

### 2. 作業用ブランチを作成して切り替え

```bash
git checkout -b feature/自分の作業名
# 例: git checkout -b feature/camera

# すでにあるブランチに切り替えるだけなら
git checkout feature/自分の作業名
```

---

## 🧑‍💻 作業中にやること

### 1. ソースコードを編集

（エディタやIDEで作業。Docker上でアプリを動かして確認もOK）

### 2. 動作確認（Docker）

アプリが動作するか確認するには、必要に応じてブラウザで `http://localhost:ポート番号` を開く。

### 3. コンテナに入って操作したい場合（任意）

```bash
docker exec -it コンテナ名 bash
```

---

## 📤 作業が完了したら

### 1. 変更をステージング＆コミット

```bash
git add .
git commit -m "ここに実装した内容を簡潔に記述"
```

### 2. リモートにプッシュ

```bash
git push origin feature/自分の作業名
```

### 3. GitHub 上でプルリクエスト（Pull Request）を作成

* `main`ブランチに対してマージリクエストを作成する
* レビューしてもらい、承認されたらマージ

---

## 🔄 他の人の変更を取り込むとき（競合を避けるために定期的に）

```bash
# mainブランチに移動して更新
git checkout main
git pull origin main

# 作業ブランチに戻ってmainをマージ
git checkout feature/自分の作業名
git merge main
```

---

## 🛑 作業終了時（任意）

```bash
docker　compose down  # コンテナ停止
```

---

## 📌 補足：よく使うコマンドまとめ

| 目的        | コマンド例                             |
| --------- | --------------------------------- |
| ブランチ作成＆切替 | `git checkout -b feature/名前`      |
| コミット      | `git add . && git commit -m "説明"` |
| プッシュ      | `git push origin ブランチ名`           |
| プルリク作成    | GitHub上で操作                        |
| Docker 起動 | `docker-compose up -d`            |
| Docker 停止 | `docker-compose down`             |
| コンテナに入る   | `docker exec -it コンテナ名 bash`      |

---
