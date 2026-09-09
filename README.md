# realiq-dev-dist

「リアルIQ測定」の**実機テスト用ビルドを iPhone に入れるためだけ**の置き場。ソースは含まない。

| ファイル | 中身 |
| --- | --- |
| `realiq-preview.ipa` / `manifest-preview.xml` | **単体で動くビルド**。JS を焼き込んであるので PC が要らない |
| `realiq-development.ipa` / `manifest-development.xml` | **開発ビルド(dev client)**。PC の開発サーバーから JS を読む |

**どちらも同じアプリID（`com.robonetcommunications.realiq`）なので、同時には入らない。**
あとから入れたほうが前のものを置き換える。

ファイル名は EAS のビルドプロファイル名に揃えてある（`.github/workflows/ios-dev-build.yml`
の `IPA` / `MANIFEST`）。以前は両方 `realiq-dev.ipa` / `manifest.xml` という名前で、
片方しか置けなかった。

## public だが、他人の端末には入らない

ad hoc 署名の `.ipa` は、**プロビジョニングプロファイルに UDID が入っている端末でしか
インストールできない**。URLを知られても、他の端末では署名検証で弾かれる。

`.ipa` を public に置いているのは、GitHub Pages が private リポジトリでは使えないため。
用が済んだらこのリポジトリは削除してよい。

## なぜ `.plist` ではなく `.xml` なのか

GitHub Pages は `.plist` を `application/octet-stream` で返す。iOS はそれを
マニフェストとして読まないので、拡張子を `.xml` にして `application/xml` で
返させている。**`.plist` を指すURLでは無言で失敗する。**

## manifest の `bundle-version`

`CFBundleVersion` と一致していないと「このAppは、整合性を確認できなかったため
インストールできません」になる。表示バージョン（`CFBundleShortVersionString` = 1.0.0）
ではない。手で書かず、**出来た `.ipa` から読んで生成している**。
