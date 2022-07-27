---
title: "トラブルシューティング"
---
# 補完が効かない、検索がうまく動かない等々、IDEの動きが怪しい
https://www.jetbrains.com/help/idea/invalidate-caches.html

IDEをアップデートした際や、強制終了したとき（まれに何もしてなくても）にキャッシュ系が壊れることがあるようです。
IDEの動きが突然怪しくなったときは一度キャッシュ削除をお試しを。

1. メニューの`File | Invalidate Caches..`を選択
2. `Clear file system cache and Local History` `Clear VCS Log caches and indexes`あたりにチェックを入れる
3. `Invalidate and Restart`ボタンをクリック
