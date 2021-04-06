---
title: "Xdebug3.0.0がリリースされたので、ver2からの雑な設定コンバート"
emoji: "🐿"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["PHP","xdebug"]
published: true
---
### Xdebug 3.0.0 is out!
11/25に[Xdebug 3.0.0 is out!](https://xdebug.org/announcements/2020-11-25)されたわけですが、`pecl install xdebug`と記載していて、まんまと勝手にver3がインストールされてしまい、まんまと以前の設定で動かず焦っている今日この頃ですが皆さんいかがお過ごしでしょうか？

そんなわけで、私が使用している設定をコンバートし、ひとまず変更を加えて動くところまで雑にやって、雑にまとめました。

### mode設定
ver2.x系でとりあえずステップデバッグを使うためには、下記の設定を行っていたと思います。いや、むしろこれがほぼすべてと言ってもよいくらい。


```ini
xdebug.remote_enable=1
xdebug.default_enable=0
xdebug.profiler_enable=0
xdebug.auto_trace=0
xdebug.coverage_enable=0
```

ver3.x系では

```ini
xdebug.mode=debug
```

となります。

modeにいくつかの設定が集約されたようで、そのvalueの記載でフラグを管理していく感じです。

以前の設定とmodeのvalueのコンバートはこう。

|以前の設定|modeのvalue|
|:---:|:---:|
|default_enable|develop|
|profiler_enable|profile|
|remote_enable|debug|

同時に設定したい場合はvalueをカンマ区切り。

```ini
xdebug.mode=develop,debug
```

### その他コンバートが必要だったもの
xdebug.remote_autostartを設定する場合、下記の2つを設定するようです。

- xdebug.mode=debug
- xdebug.start_with_request=yes.

だいたいの人が必要そうな設定はこちらです。

|以前の設定|新しい設定|
|:---:|:---:|
|profiler_output_dir|output_dir|
|remote_host|client_host|
|remote_port|client_port|

output_dir系はoutput_dirにまとめられたっぽいです。

ここまでの設定でブレークポイントまでは動かすことができました。

### ポート番号
私はポートを指定していたので引っかからなかったのですが、デフォルトのポート番号が`9003`へ変更になっています。

### 参考
参考にしたのは公式ドキュメント。
結局、必要な設定を検索しながらポチポチ地道に書き換えていくしかなさそうです。

https://xdebug.org/docs/upgrade_guide#changed-xdebug.coverage_enable
https://xdebug.org/docs/all_settings
https://xdebug.org/

### 動いたini
最期にXdebugが動作したiniを記載しておきます。
開発環境はPhpStormで、Dockerサーバーです。

```ini
xdebug.client_host=host.docker.internal 
xdebug.client_port=9010 
xdebug.idekey=PHPSTORM 
xdebug.mode=debug,develop 
```

## Profilerの設定
https://zenn.dev/naoyukik/articles/2d10858178535c38f0c4

こちらも公開しましたので合わせてご覧ください。
