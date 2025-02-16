# What's this

from https://y-ohgi.com/introduction-docker/

調べ物をしていると突然出てくるDocker関連の情報。  
これが出てきた時に、何も知らない状態だと不用意に心理的ハードルを上げてしまうので、これを防ぐ目的の勉強。  

Dockerがどういう要素で構成されているのか、どういう扱い方をするものなのか、あたりがインプットできたら良さそう。なので途中スキップもする。

まとめ

- Dockerはプロセスの仮想化で、ネットワークという仕切りなどを使ってコンテナやリソースの管理を行う
- 本番環境で用いる場合にはdocker-composeのような複数のDockerを管理可能なツールを使う
- コンテナ：ベースImageに対して追加の記述を行い（IaC）、欲しい環境を構築する
- 異なるコンテナ同士の通信：ネットワーク上の異なるコンテナ同士を通信させるための設定を行う。ソケットではなくネットワーク
- 各Dockerの設定を行うファイル（Dockerfile）と、それをラップする設定を行うファイル（docker-compose）は別のものとして考える


# Index

```
1. はじめに
2. コンポーネント
3. プロダクションでの活用
4. Tips
```


# はじめに

## Dcokerとは

- 利点
  - ポータビリティ
  - オーバーヘッド（VMと比較して）
  - スナップショットとIaC (Dockerfile, Image)
    - スナップショット（Gitでも出てきた言葉）

## VMとDocker

![](images/2024-07-09-15-06-53.png)

- VM: マシンの仮想化
- Docker: プロセスの仮想化　→より軽量


## Hello Docker!

起動の確認


# コンポーネント

## image

- Imageは環境のスナップショット
- Docker Hubから公式のImageを入手　→　ベースイメージとしてそれらを拡張していく（IaC, Dockerfileで）
- スナップショットとはそういうことか！

## dockerfile

- コマンドが17種類しかない（しかもよく使うのは7つ程度）ということを知って衝撃を受けた
- なら使えるようになるのも早そう...？


## container

- コンテナ内の各プロセスは、他所とは隔離されている
- フォアグラウンドでコマンドが終了したらコンテナは停止する
  - たとえば`echo "Hello World!"`させた時と`-d nginx`させた時では、コンテナの生存時間が異なるということか
  - でもバックグラウンドで起動した、フォアグラウンドで動かすnginxが停止したら、そのコンテナは停止してしまう（ややこしい）


## network

- Dockerではネットワークの扱いが重要
- プロセス間の通信はソケットではなくネットワークで
  - [ソケット通信とHTTP通信](https://www.miraclejob.com/recommend/detail?cd=2521#:~:text=HTTP%E3%81%AF%E3%82%AF%E3%83%A9%E3%82%A4%E3%82%A2%E3%83%B3%E3%83%88%E3%81%8C%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88,%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E5%8F%AF%E8%83%BD%E3%81%A7%E3%81%99%E3%80%82)
    - コネクションの確立→双方向のやり取り
- 同一ネットワーク（Bridge）内のコンテナ同士だと通信できる
- 異なるネットワーク（Bridge）同士では設定なしに通信はできない
- ネットワークは作れる

## volume

![](images/2024-07-09-15-48-53.png)

- コンテナとは異なる独立したライフサイクル
  - Volumeはコンテナの外側へファイルが保管される
- 異なるコンテナ同士で共有が可能
- ただし、基本的には使用しないことが好ましい（エフェメラル）


# プロダクションでの活用

## 設計

- [12factor app](https://12factor.net/ja/)


## セキュリティ

- Dockerにもセキュリティリスクは存在する
- 特に外部のImageを使う/外部にImageを公開する際は注意


## デバッグ

![](images/2024-07-09-15-57-34.png)

- コンテナは実行のたびに新しい環境が立ち上がる
  - `container id`を使うと再起動が可能
- `docker exec`でコンテナの中に入れる

## イメージの仕組み

- 今回は流し読み、深入りしない


## Dockerfileのベストプラクティス

- 軽量化の話し
- 今回は流し読み、深入りしない


## オーケストレーションツール

- 複数のDockerを扱う技術で、ネットワークの管理などの機能を備える
- 代表的なオーケストレーションツール
  - docker-compose (Docker社)
  - swarm (Docker社)
  - ECS (AWS)
  - Kubernetes (Google社)

## docker-compose

- ポートフォワード
  - `ホストport`:`コンテナport`

## プロダクションへの導入

![](images/2024-07-09-16-10-59.png)



# Tips

## tools

- 今回は流し読み、深入りしない


## docker-compose

- 環境変数を定義＆読み込むには４つほど方法がある
- docker-composeファイルか環境変数ファイルに定義しておくのが一般的？


## 参考書

- 今回は流し読み、深入りしない
