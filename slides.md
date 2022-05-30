---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# キャッシュについて

---

# 今日のひとこと

- 『劇場版 名探偵コナン』現状のベスト3
  1. 『名探偵コナン 沈黙の15分（クォーター）』
  2. 『名探偵コナン 水平線上の陰謀（ストラテジー）』
  3. 特になし

---

# キャッシュとは

> キャッシュは、CPUのバスやネットワークなど様々な情報伝達経路において、ある領域から他の領域へ情報を転送する際、その転送遅延を極力隠蔽し転送効率を向上するために考案された記憶階層の実現手段である。(引用: フリー百科事典『ウィキペディア（Wikipedia）』)

---

# リソースリクエストの全体像

Webブラウザがリソースを取得する時の大まかなプロセス

<img src="/cache.png" >

---

# インメモリキャッシュ

- 世の中で利用されているブラウザにはインメモリなキャッシュがある
- RAMに保存されるため、書き込みとアクセスが高速になるが、PCの電源がオフになるか、その他の特定の状況で消去される
- キャッシュは、各ブラウザのRendererプロセスに存在している
- LRU(Least Recently Used)方式でキャッシュが削除される

---

# インメモリキャッシュ

例）ChromiumのBlinkレンダリングエンジン
https://chromium.googlesource.com/chromium/blink/+/refs/heads/main/Source/core/fetch/MemoryCache.cpp

---

# ServiceWorkerとは

- WebブラウザとWebサーバー間のプロキシとして機能する特殊なJavaScript資産のこと
- オフラインでアクセスできるようにすることで信頼性を向上させたり、ページのパフォーマンスを向上させることを目的としている

---

# ServiceWorkerとは

- 主にできること
  - ローカルプロキシ
    - リクエストに加工できる
  - オフラインキャッシュ
    - オフライン時の使用が可能になる
    - PWAにも使われている
  - プッシュ通知
    - アプリを閉じていても通知を受けられる
  - バックグラウンド同期

---

# ServiceWorkerキャッシュ

- ServiceWorkerでHTTPをinterceptして、Cache Storage APIかindexedDBを用いて、リソースのキャッシュを行う
- キャッシュの操作をよりプログラマブルに行うことができる
- リソース単位でのキャッシュ戦略を詳細に立てることができる
- このキャッシュを操作する場合は、直接上記APIを使うことは珍しく、多くはWorkBoxのようなラッパーを利用する

---

# ブラウザキャッシュ

- RFC7234で定められているキャッシュのこと（≒ディスクキャッシュ）
- Browserプロセスに存在している
- ストレージにキャッシュを書き込むため、RAMを利用しているインメモリキャッシュと比較すると、読み書きに時間がかかる
- ページ再訪時には高速なコンテンツ取得が可能になる
- これらの振る舞いや有効期限はHTTPヘッダーを用いて操作する