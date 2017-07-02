## はじめに

GCのアルゴリズムとヒープ領域の各領域の役割について言及する。

GCのアルゴリズム自体には色々と種類がある。
最も基本的なのは下記の2つ。

## 基本アルゴリズム

- [参照カウンタ](http://seesaawiki.jp/w/author_nari/d/GC/standard/Reference%20Counter)
- [Mark&Sweep](http://seesaawiki.jp/w/author_nari/d/GC/standard/Mark%26Sweep)

JavaでスタンダードなのはMark&Sweep。
ただこれだけだと下記のような課題があり、実際には拡張されて実装されている。

## Mark&Sweepが抱える課題

1. メモリ領域の断片化
2. GCをしている間のアプリケーションの停止時間 (Stop the world)
3. すべての対象となるメモリ領域にGCをかけると処理負荷が高い(FullGC)

### 1.ヒープ領域断片化への対策 => Copying

不要な領域を解放、というのを繰り返していると、空き領域が凸凹になり、融通がきかなくなる。
つまり新規オブジェクト作成の処理コストがあがる。

そのため、下記のアルゴリズムを組み合わせる。

- [Copying](http://seesaawiki.jp/w/author_nari/d/GC/standard/Copying)

**ヒープ領域にFrom領域とTo領域があるのはこのため。**

### 2.停止時間への対策 => コンカレントマーキング

- Mark&SweepのMarkをつける処理をJavaプログラムを停止せずにスレッドで並行して行う。
    - 問題として、Sweepが走るまえに状態を変更されたりすると漏れやゴミが出る可能性がある。これを解決するため、オブジェクトが変更されたタイミングで印をつけておき(write barrierというらしい)、Markが終わってSweepが始まる前にJavaプログラムを停止して、印をつけたところからマーキングの修正をする。(シリアルマーキング)

コンカレントマーカスイープと呼ばれているのは、このアルゴリズムを足し合わせているから。

### 3.処理負荷への対策

世代別GCアルゴリズムを用いている。
簡単に言えば、新しく出来たばかりのオブジェクトはNew領域に置き、New領域に対してのみちょくちょくGCをかけて、何度かGCをかけた結果生き残ったオブジェクトを今後も生き残り続けるとみなしてOld領域に移動、Oldがたまった時点でFullGCというアルゴリズム。
毎回FullGCをするのではなく、GCの対象範囲を限定的にして、GCの負荷を分散する。

参考: [世代別GC](http://seesaawiki.jp/w/author_nari/d/GC/extend/%c0%a4%c2%e5%ca%ccGC)

**JVMのヒープ領域にNewやOldがあるのはこのため。**

## JVMのメモリ領域はGCのアルゴリズムに基づいて種類がある

今までの内容から、下記のことがわかる。

- New、Old領域の中でもFromとToが別れているのは、ヒープ領域の断片化を防ぐためにCopyingを実施するため。

- ヒープ領域でNew、Oldがあるのは、GCの対象となる領域を限定的にして処理負荷を分散させるためにある。(New内にあるEden領域も、出来たばかりのオブジェクトをGC対象からはずして分離したいから存在している)

### 参考
[Javaのヒープ・メモリ管理の仕組み](http://www.atmarkit.co.jp/ait/articles/0504/02/news005.html)

[JVMのチューニング](http://d.hatena.ne.jp/ogin_s57/20120709/1341836704)


## 余談

Garbage First Garbage Collectionなるアルゴリズムが今後来る・・・？
[Garbage-First Garbage Collection](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.63.6386&rep=rep1&type=pdf)





### write barrier

- 変更されたオブジェクトに印をつけて、ユーザプログラムとGCプログラムを並行してはしらせても整合性を保てるようにする仕組み。

## メモリ管理

- Permanent領域、metaspace、Heap領域の違い

## TODO

- 結局Garbage Firstってなんやねん
