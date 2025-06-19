# 🍽️ PochiMeshiAPUri - 学食の混雑を減らすための電子食券システム

## ✅ 開発の背景・目的

大学の学食では、現在「現金による食券購入 → 窓口で食券を渡す → 注文後に調理される → 受け取り」という手順が踏まれているが、以下のような問題点がある：

- **現金払いのみ**で不便、学生のキャッシュレス化に対応できていない。
- 混雑時、**調理前に列ができることで滞留が発生**し、効率が悪い。
- 学生が食堂に来たタイミングで初めて調理が始まるため、**受け取りまでに時間がかかる**。

本アプリ「PochiMeshiAPUri」は、この課題を解決するために開発する。

---

## 🎯 開発のゴール

1. **PayPayを使ったキャッシュレス食券購入**
2. **Bluetoothを利用した食堂内滞在の検知**
3. **Webサイト上で「調理依頼」ボタンを有効にする**
4. **カメラで窓口上の料理配置を検出して、受け取り可能通知などに活用**

---

## 🧩 主な構成要素（マイクロサービス構成）

| モジュール | 説明 | 技術候補 |
|------------|------|----------|
| Frontend   | 購入・調理依頼用のWeb UI（Bluetooth機能含む） | React + TypeScript + Web Bluetooth API |
| Backend    | API処理、トリガー制御、注文管理 | FastAPI（Python） |
| Camera     | 料理配置検知カメラ処理 | Python + OpenCV |
| 決済連携（将来） | PayPay API連携（※初期段階ではモックで対応） | REST API連携（外部サービス） |

---

## 📡 初期に知っておくべき技術事項

### 1. Web Bluetooth API
- ブラウザ上のJavaScriptでBluetoothデバイスに接続できるAPI。
- Chrome系ブラウザでしか動作しない（iOS/Safari非対応）。
- サーバーではなく「クライアントが食堂内のビーコンなどに接続できるか」で位置を推定。

### 2. Docker開発環境
- 各サービス（フロント/バック/カメラ処理）を個別コンテナで構築。
- `docker-compose` により一括起動・ネットワーク共有。
- `volumes`を使ってコード変更を即反映。

### 3. Python + OpenCV
- カメラ画像から、窓口上に料理が「ある」状態を簡単に検出。
- 最初は画像の明度変化や色差分でOK。将来的には物体検出モデル（YOLOなど）に置き換え可能。

---

## 🛠️ 最初に作るもの（MVP）

- [ ] **決済完了後に表示されるQRコード or 調理依頼画面**
- [ ] **Bluetooth接続可否による調理ボタンの活性/非活性**
- [ ] **Webボタンからバックエンドへ「調理開始」POST送信**
- [ ] **カメラ画像を周期的に判定 → フロントに表示 or 通知**

---

## 🧪 開発の進め方（推奨フロー）

1. Docker環境構築（`docker-compose.yml` 完成済み）
2. Web Bluetooth API の実験用UI構築（React）
3. BackendにFastAPIでトリガー受信API作成
4. 簡単な食堂「滞在中」判定 → トリガーボタン表示
5. カメラ画像 → 明度 or 差分判定による料理検出

---

## 🏃‍♂️ セットアップ手順

1. リポジトリをクローン
   ```sh
   git clone <このリポジトリのURL>
   cd pochi-meshi-apuri
   ```
2. Docker環境を起動
   ```sh
   docker-compose up --build
   ```
3. 各サービスの開発は `frontend/`, `backend/`, `camera_module/` ディレクトリで行う

---

## 📁 開発フォルダ構成

```
pochi-meshi-apuri/
├── docker-compose.yml
├── frontend/         # React + Web Bluetooth
├── backend/          # FastAPI
├── camera_module/    # Python + OpenCV
└── shared/           # 共通設定
```

---

### 起動コマンド例（ローカル開発時）

```sh
cd backend
pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

---
