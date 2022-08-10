---
title: "トラブルシューティング"
---
# 補完が効かない、検索がうまく動かない等々、IDEの動きが怪しい

## インデックス作成をしている
https://pleiades.io/help/phpstorm/indexing.html
IntelliJのIDEはプロジェクト内のソースコードに対してインデックスを作成しています。
それによって補完や検索を可能にしています。

基本的にそのプロジェクトのインデックス化処理が始まると、補完や検索がうまく動作しなくなります。
また、インデックス化処理はかなり動作が重くなります。
動作が重い・補完が効かないなどの症状の場合、IDE下部のステータスバーを確認します。インデックス化処理が動作している場合、プログレスバーが表示されています。

## キャッシュ関連
https://pleiades.io/help/phpstorm/invalidate-caches.html
IDEをアップデートした際や、強制終了したとき（まれに何もしてなくても）にキャッシュ系が壊れることがあるようです。
IDEの動きが突然怪しくなったときは一度キャッシュ削除をお試しを。

1. メニューの`File | Invalidate Caches..`を選択
2. `Clear file system cache and Local History` `Clear VCS Log caches and indexes`あたりにチェックを入れる
3. `Invalidate and Restart`ボタンをクリック
