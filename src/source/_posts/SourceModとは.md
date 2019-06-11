---
title: SourceModとは?
author: Eru
---

こんにちは！EruFFA AdminのEruです。
皆さんはUTAGEの試合を見たことはありますか？
チャット欄に出てくる試合の情報や試合を円滑に進めるために
SourceModというサードパーティ制のサーバーアドオンが使用されています。
今回はこのSourceModというサードパーティ制のアドオンの入れ方からプラグインの作り方まで
順を追って紹介してみたいと思います。

## SourceModとは

SourceModはC++をベースとしたSourceエンジンとSourceSDKを使用して書かれた
MODのためのスクリプト言語です。
管理システムやコマンド、イベント、コンソール変数、ネットワークメッセージなどをSourceModから
拡張できるようにした言語です。

また、SourceModスクリプトはPawnから派生したSourcePawn言語で書かれています。

詳しくは[SourceMod Wiki](https://wiki.alliedmods.net/SourceMod)をご覧ください。


### MetaMod導入について

SourceMod(並びにMetamod)導入について、ここではsteamcmdからのCSGOサーバーのインストール、セットアップは
完了しているもののとし、話を進めていきます。

SourceModはMetamod:Sourceが前提として必要です。
まずはMetamod:Sourceをサーバーにインストールしていきます。
[Metamod:Source](https://www.metamodsource.net)を開き左側のStable Buildsから
自分のOSにあったボタンをクリックします。
ここではWindows版をダウンロードしました。

![I1](https://i.imgur.com/DZC7rpT.png)

さらに、ダウンロードしたファイルの中身のaddonsフォルダを
CSGOサーバーディレクトリ/csgo/
に移動します。
(画像ではすでにaddonsフォルダが存在しますが
最初からは存在しません。)

![I2](https://i.imgur.com/ak5UQoX.png)

下記の画像の通り
CSGOサーバーディレクトリ/csgo/addons
という配置になっていれば成功です。

![I3](https://i.imgur.com/8MxivM2.png)


### SourceMod導入について

次にSourceModをサーバーにインストールしていきます。
まず、[SourceMod](https://www.sourcemod.net)の左側のDownloadsから
Stable Buildsをクリックし、自分のOSにあったボタンをクリックします。
ここではWindows版をダウンロードしました。

![K1](https://i.imgur.com/Y77oydw.png)

次にMetaModと同じ要領で、ファイルの中身のフォルダをすべて移動します。
CSGOサーバーディレクトリ/csgo/addons
CSGOサーバーディレクトリ/csgo/cfg
となるように移動します。
このときに上書きの確認が出た場合は上書きします。

![K2](https://i.imgur.com/ixUpNFj.png)

これでSourceMod並びにMetaMod:Sourceのインストールは完了しました。


### 確認方法について

最後にサーバーを起動して、コンソールに
`sm version`と打ち、しっかりとインストールされているか
確認してください。この際、何も出てこない場合はどこかでインストール工程を飛ばしている、または間違えている可能性があります。
正しくインストールされていない場合はプラグインを有効化できません。
もう一度最初から確認してください。

![L1](https://i.imgur.com/IPiq0Av.png)


### 最後に

SourceModはCSGOでサーバーを稼働させるには便利なアドオンですが、
プラグインを作成できる人が少なく、日本でもSourceMod/SourcePawnの
コミュニティが少ないのが現状です。
これらのことから今回の記事を作成しています。
もし、興味を持ってもらえたら次の記事でもお会いしましょう！
以上、EruによるSourceModの導入方法の説明でした！

