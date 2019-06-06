---
title: UTAGE BROADCASTについて
date: 2019-06-06 17:52:36
tags:
---

こんにちは！テクニカルエンジニアのFlowingです。
突然ですが皆さんは[UTAGE BROADCSAT](https://twitch.tv/utage_broadcast/)についてご存知でしょうか？
このサービスはUTAGEの予選試合を自動的に[Twitch.tv](https://twitch.tv/)へ配信するサービスです。
ミラー配信も許可しているため、一か月に行われる宴の試合を簡単に観戦・実況することが出来ます！

今回は、このサービスの構成・仕組みや、技術的な部分について説明して行こうと思います。

### 企画段階
UTAGE BROADCASTは、数あるUTAGEの予選を誰でも簡単に観戦でき、ミラー配信を行えるシステムとして開発しました。
その為手動での操作を可能な限り省き、メイン部分である配信ソフトウェアの操作・CSGOサーバーへの接続は全て全自動で行えるよう構成しました。
そのため、これまで手動で行っていた作業をどこまで自動化出来るかが構築時の焦点となりました。

### 構成
バックエンド部分はTypeScript(NodeJS)で記述してあります。
最初は純正NodeJSで記述していたのですが、実行時エラーが発生すると配信垂れ流しになってしまう危険性を孕んでいた為、事前コンパイルがあるTypeScriptを選びました。
フロントエンドはjQueryで記述してありますが、フロントエンドは後述する"Project Talos"の時しか公開していないので実質サービスの根幹には関わっていないと考えて構いません。

バックエンド側の処理としては、NodeJSに備わっているexpressモジュールを使いHTTPサーバーを立ち上げ、REST API経由で様々な操作を行えます。
CSGO側の状況確認は[GameStateIntegration](https://developer.valvesoftware.com/wiki/Counter-Strike:_Global_Offensive_Game_State_Integration)(以下GSI)とexpressを使い、CSGOへのコマンド送信は[HLAE](https://github.com/advancedfx/advancedfx)のコマンドとwebsocketサーバーをを活用する事で実装しています。
配信ソフトへの操作制御は、[obs-websocket](https://github.com/Palakis/obs-websocket),[obs-websocket-js](https://github.com/haganbmj/obs-websocket-js)を使っています。
因みにvmixの方がREST APIが使える為より直感的に操作を行えそうな印象はあったのですが、コストの関係もありOBSに落ち着きました。

上記三つの技術を組み合わせ、「buytimeになったらエフェクト作動」や、「warmup時はシーンを変える」などの制御を行っています。

構築自体はは約二週間程度で動くレベルまで到達しました(その後Twitchチャンネルの取得・試運転などを入れると本番運用までは一か月程度かかりました)。

### 構築
試合サーバーののホスティングサービスである"Project Hypnos"のコードを少し改造し、サーバー起動時にHTTPリクエストを送れるようにしました。(Hypnosの技術説明記事もいつか公開します。お楽しみに！)
また、REST APIを公開するのはセキュリティ面に不安があった為、上記HypnosとUTAGE BROADCASTを同一PCに共存させ、localhost経由でRESTを叩けるようにしました。

各ソフトウェアが踏む手順としては以下の通りです。
1. ユーザーが".create-get5"と発言し、Hypnosがサーバー作成準備に入る。
2. Hypnosがリーグサーバーが作成すると、UTAGE BROADCASTのREST APIへPOSTでIP,ポート,パスワードが入る(localhost経由)。この時点で配信が開始される。
3. UTAGE BROADCAST側でCSGOを起動、起動オプションに`+connect ip`コマンドを挿入し、起動と同時にサーバーに接続するように。
4. CSGOクライアントとUTAGE BROADCASTをHLAEとGSI経由でリンク。OBSへの操作を行い始める。
5. CSGOクライアントの状況に応じて様々なアクションが実行される。
6. 試合終了を検知すると自動的にCSGOがシャットダウンされ、配信が終了する。

以上がおおまかな流れです。


### リリース
リリースする前に、"Project Talos"という名前で、オンラインでdemoを見れるサービスという体でサービスを仮公開しました。
これはngrok.io 経由でdemファイルをアップロードし、アップロードを検知したらdemファイルを再生するごく簡単なシステムです。
メイン部分(CSGO・配信ソフトの制御)は殆ど書き換える必要なく使いまわせたため、このサービスをUTAGE BROADCASTのベータ版として仮運用することにしました。

予選開始期間一週間前になり、前述した"Project Talos"ではなく、直接GOTVの試合を配信できるサービスをして"UTAGE BROADCAST"の発表・仮運用を開始しました。
これはUTAGE Discordの鯖レンタルチャンネルから起動されたGOTVを自動的に観戦するサービスです(これは告知が前後した関係と、戦術のリークにより不評を招いた為、想定よりも早くテストを終了しました)。

予選開始一日目にはサーバートラブルがありうまく作動しませんでしたが、2日目には通常通り稼働していました。



### まとめ?
以上がUTAGE BROADCASTのおおまかな技術説明です。
技術関連の記事を書くのは初めてなので拙い部分もあると思いますが何か役に立てれば幸いです！
良かったら"[UTAGE BROADCAST](https://twitch.tv/utage_broadcast/)"、使って頂けたら嬉しいです。
以上、[FlowingSPDG](https://twitter.com/FlowingSPDG)によるUTAGE BROADCASTの技術説明でした！


## WE WANT YOU!
UTAGE-Techではエンジニア・デザイナー・ライターを募集中です！
興味のある方は flowing[a]utage-csgo.com までメール下さいませ！