# わたしのクエスト（PWA / GitHub Pages 用）

## 中身
- index.html … アプリ本体
- manifest.webmanifest … アプリ情報（名前・アイコン・全画面表示）
- sw.js … オフライン動作用（サービスワーカー）
- icon-192.png / icon-512.png / icon-maskable-512.png … アプリアイコン

## GitHub Pages で公開する手順
1. GitHub にログイン →「New repository」→ 名前を `questboard`、**Public** で作成。
2. リポジトリの「Add file → Upload files」で、この4種（index.html, sw.js, manifest.webmanifest, icon-*.png）を
   **すべてリポジトリ直下**にアップロード → Commit。（index.html が一番上の階層にあること）
3. 「Settings → Pages」→ Source を **Deploy from a branch**、Branch を **main / (root)** → Save。
4. 数分待つと `https://<ユーザー名>.github.io/questboard/` で公開されます。

## 父 → 娘 への渡し方
1. 父端末の Chrome で上記URLを開く。
2. 日付を**長押し** → 暗証番号（初期 **5290**）→ 保護者メニュー。
3. クエスト候補・ごほうび・ガチャ景品などを設定 → 「保存する」。
4. 「娘の端末へ引き継ぎ」の**コードをコピー**。
5. 娘の Android Chrome で同じURLを開く → メニュー「アプリをインストール／ホーム画面に追加」。
6. 娘端末で保護者メニュー（長押し＋5290）→ コードを貼り付け →「読み込む」。
7. 以後はアイコンから起動。オフラインでも動きます。

## メモ
- 暗証番号は保護者メニューで変更できます。
- アプリを更新したい時は、新しい index.html 等を同じリポジトリに上書きアップロードし、
  sw.js の `CACHE = 'questboard-v3-1'` の数字を上げると確実に反映されます。

## （任意）二台リアルタイム同期 — Firebase 設定手順
父端末で設定を変えると娘端末に即反映。引き継ぎコードが不要になります。

1. https://console.firebase.google.com/ にGoogleアカウントでログイン →「プロジェクトを追加」（例: quest-family）。GoogleアナリティクスはオフでOK。
2. 左メニュー「構築 → Realtime Database」→「データベースを作成」→ ロケーションを選択 →「テストモードで開始」。
3. できたDBの「ルール」タブを開き、次に置き換えて「公開」：
   {
     "rules": {
       "rooms": { "$room": { ".read": true, ".write": true } }
     }
   }
   ※家族向けの簡易設定です。ルームIDは推測されにくい長めの文字列にしてください。
4. プロジェクト設定（歯車 → プロジェクトの設定）→「マイアプリ」→ Web（</>）でアプリ登録 → 表示される
   `const firebaseConfig = { ... }` を丸ごとコピー。
5. アプリの保護者メニュー →「二台同期」→ Firebase設定に貼り付け → ルームIDを「生成」→「同期を開始」。
6. 娘端末でも同じURLを開き、同じ手順で **同じFirebase設定・同じルームID** を入れて「同期を開始」。
   → 以後リアルタイムで共有されます。
