# Webpackについて

- altjsのコンパイル、画像リソースなどの圧縮、すべてのモジュールのバンドル、lintでの静的解析、単体テスト

## ビルドシステムとモジュールバンドラの違いってなに？

- ビルドシステムは、コンパイルや圧縮、難読化などの必要な処理を順番に記述していくもの
- バンドラは複数のjsモジュールを一つのファイルに統合するもの。require宣言などで依存性を解決する
    - webpackは基本はentryノードから依存性を辿って、最終的に一個のbundle.jsを作るという目的。
    - ただbundleする過程でcssやtxtをjsで読み込めるように変換したり、難読化したり、画像をデータURI化したりする変換をかけられる。
    - jsでのアプリケーション構築に必要な処理は上記の変換でカバーできる
    - gulpなどと組み合わせることで処理責務を分けて、きれいに書くこともできる

## webpackの役割

- 画像の圧縮
- CSSプリプロセッサ周り
- ベンダープレフィックス
- ポリフィル
- 難読化
- ログ送信

## Goal

- jsのアプリケーション構築において、しなくてはならない作業を列挙できるようになる
- しなくてはいけない作業をwebpackで記述し、react-redux+Stylusのアプリケーション初期構築ができるようになる

## WebPack4つのコンセプト

### entry
- 依存グラフの最初のノード。すべてはここから依存性解決する

### Output

- 成果物

### loader

- jsから参照できる形にするため、jsモジュールに変換する
- inputとoutputの流れの中で何かをする

### Plugin

- 難読化したり色々な機能もってる・・・
- loaderでももちろん使えるっぽい
- inputとoutputの流れ関係なく色々できるっぽい

## TODO

- eslintとかpostcssの宣言って？
- react-reduxのボイラープレート読んで何を主にやるべきか、やり方の詳細を詳しく調べる
