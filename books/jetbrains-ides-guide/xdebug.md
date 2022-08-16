---
title: "Xdebug"
---
# Xdebug
https://pleiades.io/help/phpstorm/debugging-with-phpstorm-ultimate-guide.html

PHP向けのデバッグツールです。

これを設定することで、指定箇所で動作しているプログラムを一時停止し、そのタイミングの変数などの状態を確認したり、別のプログラムを一時的に実行させることができます。

PHP開発においてXdebugはもはや必須ツールと言えます。

PhpStormに限らず他のIDE等でも使用できますので、ぜひ設定しましょう。

## Xdebug3.x設定
ここでは、Xdebug3.xを動作させるための最小限の設定を紹介します。

### 動作可能iniサンプル
まずXdebugが動作したiniを記載しておきます。
開発環境はPhpStormとDockerサーバーです。

```ini
xdebug.client_host=host.docker.internal 
xdebug.client_port=9003
xdebug.mode=debug 
xdebug.start_with_request=yes
xdebug.trigger_value=""
```

### ステップデバッグを行うための設定（mode設定）
https://xdebug.org/docs/all_settings#mode

xdebug.iniに下記設定を追加します。
今回はステップデバッグを使用したいので`debug`を設定します。
```ini
xdebug.mode=debug
```

modeを変更することでいくつかの動作を切り替えることができます。

| modeのvalue | 説明                                   |
|------------|--------------------------------------|
| debug      | ブレークポイントを使用したステップデバッグを行う             |
| develop    | 拡張されたvar_dump等の開発ヘルパーを使用する           |
| profile    | PHPのパフォーマンス等計測するプロファイラー機能を使用する       |
| coverage   | PHPUnit等のテストがどの程度プログラムをカバーしているかを計測する |
| gcstats    | PHPのガベージコレクションの統計情報を収集する             |
| trace      | 関数トレーサー機能が有効になる。関数に関する情報を記録する        |

同時に設定したい場合はvalueをカンマで区切ります。

```ini
xdebug.mode=develop,debug
```

### デバッグポート
Xdebug3.xからデフォルトで待機するポートが9003になりました（v2.xでは9000）。
PhpStormでもデフォルト9003で待機するように設定されています。

変更する場合、下記設定を変更します。
```ini
xdebug.client_port=9003
```

### デバッグクライアントのIPアドレス
Xdebugがリモートで接続するためのIPアドレスもしくはホスト名を設定します。
これはデバッグクライアント（PhpStorm等）のマシンのIPアドレスです。

Docker上でPHPが動作している場合、`host.docker.internal`を設定することでよしなにIPアドレスが設定されます。

```ini
xdebug.client_host=host.docker.internal
```

### 発動タイミング制御
Xdebugはデバッグを動作させるタイミングをある程度設定で決めることが可能です。

下記設定をxdebug.iniへ記載します。

```ini
xdebug.start_with_request=yes
```

| start_with_requestのvalue | 説明                                                                 |
|--------------------------|--------------------------------------------------------------------|
| yes                      | PHPリクエストが開始されるとPHPコードが実行される直前に自動で開始する                              |
| no                       | リクエスト開始時に動作しない                                                     |
| trigger                  | トリガー文字列`XDEBUG_TRIGGER`が`$_GET/$_POST/$_COOKIE`に存在する場合のみ、デバッグを開始する |

※トリガー文字列はv2で使われていた`XDEBUG_PROFILE``XDEBUG_TRACE``XDEBUG_SESSION`も引き続き使用可能

#### trigger_valueの設定

triggerに関しては、trigger_valueという追加の設定があります。
これを設定することによってXDEBUG_TRIGGERに指定した文字列が一致するときのみ、デバッグを開始させることが可能です。
trigger_valueが空の場合、XDEBUG_TRIGGERの文字列に関係なくデバッグが開始します。

```ini
xdebug.trigger_value=""
```

trigger_valueを設定した場合、PHPUniやHTTP Requestを使用したデバッグを行う際、XDEBUG_TRIGGER等の文字列を個別に設定する必要があります。
この場合でPhpStormからPHPUnitでのデバッグを行う場合、`Run/Debug Configurations`の各テストの`Command line | Environment valiables`に`XDEBUG_TRIGGER={values}`を設定すればデバッグが可能になります。
ただ、trigger_valueはローカルでの開発時、どのような文字列であってもデバッグ可能にしていたほうが開発しやすいでしょう。

## PhpStorm設定
PhpStormのXdebugを使用する際の設定を紹介します。

PhpStormでXdebugのステップデバッグを行う際必要なのは、実際のところXdebugが動作するサーバーの設定が主です。
![CLI Interpreter](/images/Xdebug-Cli-Interpreters.png)

CLI Interpreterで必要な設定は通常のDocker Compose設定の他、`Additional`の`Debugger extentions`の項目です。
ここでは`xdebug.so`と記載します。

開発中でも不要であればXdebugを有効にしておく必要はありません。
使用したい場合、コマンドラインからPHPコマンドを実行する際にxdebug.soを読み込むことが可能です。
そうすることで「PHPUnit等でデバッグを実行するときのみXdebugのブレークポイントを使用する」というようなことが可能になります。

ここまでの設定でブレークポイントが動作するか、手元のソースコードで確認してみましょう。
