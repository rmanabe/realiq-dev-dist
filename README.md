# realiq-dev-dist

「リアルIQ測定」の**開発ビルド(dev client)を実機に入れるためだけ**の置き場。ソースは含まない。

- `realiq-dev.ipa` — ad hoc 署名の開発ビルド
- `manifest.xml` — iOS の OTA インストール用

## public だが、他人の端末には入らない

ad hoc 署名の `.ipa` は、**プロビジョニングプロファイルに UDID が入っている端末でしか
インストールできない**。URLを知られても、他の端末では署名検証で弾かれる。

`.ipa` を public に置いているのは、GitHub Pages が private リポジトリでは使えないため。
用が済んだらこのリポジトリは削除してよい。

## なぜ `.plist` ではなく `.xml` なのか

GitHub Pages は `.plist` を `application/octet-stream` で返す。iOS はそれを
マニフェストとして読まないので、拡張子を `.xml` にして `application/xml` で
返させている。**`.plist` を指すURLでは無言で失敗する。**

## インストール

iPhone の Safari のアドレス欄に貼る（他のブラウザでは動かない）:

    itms-services://?action=download-manifest&url=https%3A%2F%2Frmanabe.github.io%2Frealiq-dev-dist%2Fmanifest.xml

入らないときは、**前に失敗した残骸がホーム画面に残っていないか**を見る。残っていたら先に削除する。

## 開発サーバーに繋ぐ

PC 側で `npx expo start --dev-client` を起動し、iPhone の Safari で:

    http://<PCのIP>:8081/_expo/link?platform=ios&choice=expo-dev-client

**`&choice=expo-dev-client` を省かないこと。** 省くと Expo Go 用のURLへ転送され、
Expo Go は使えないので**エラーも出ずに何も起きない**。

- `exp://` を直打ちしても開かない（不明なスキームは検索語として扱われる）
- ネットワークを変えたら **Metro を再起動する**。起動時のIPを保持するので、
  IPが変わっても古いIPへの転送を返し続ける
- 設定 → プライバシーとセキュリティ → ローカルネットワーク で、このアプリを ON にし、
  **アプリを完全終了して再起動する**（iOS はこの権限変更を再起動まで反映しない）

JS の変更は再ビルド不要。作り直しが要るのはネイティブを変えたときだけ。
