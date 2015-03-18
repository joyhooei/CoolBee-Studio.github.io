﻿MSDK Android 紹介
=======

モジュール紹介
---

| モジュール名称 | モジュール機能 | 接続条件 |
| ------------- |:-------------:|
|データ分析モジュール|データ報告、異常報告を提供	||
|モバイルQQ	 |ログイン及びモバイルQQへの共有の能力を提供	|モバイルQQ appIdとappKeyを必要|
|ウィーチャット |	ウィーチャットのログインと共有能力を提供	|ウィーチャット appId と appKeyを必要|
|QQゲームホール	|ゲームホールによるゲーム実行能力を提供	|
|内蔵ブラウザ	|アプリの内蔵ブラウザ能力を提供	|
|公告	|ポップ公告のスクロール能力を提供	||
|LBS	| LBS能力を提供	||
|応用宝登録|	応用宝登録の能力を提供	|接続プロセスはSDKパッケージの『応用宝登録機能接続ガイド』を参照|
|応用宝のトラヒック節約の更新|	応用宝のトラヒック節約の更新能力を提供|
注意事項:
- 支払(Midas) はMSDKと独立しており、関連の支払ドキュメントを参照してください。
- appId、appKey、offerIdは関連モジュールに接続するための証拠であり、申請方法は「MSDK製品紹介」に記載されています。

用語解釈
---

| 名称 | 用語概説 |
| ------------- |:-------------:|
| プラットフォーム| ウィーチャットとモバイルQQをプラットフォームという|
|openId|ユーザーの授権後、プラットフォームから戻る唯一標識|
|accessToken|ユーザー授権のトークンです。このトークンを取得した後、ユーザーが授権したことを認めます。共有/支払などの機能はこのトークンを必要とします。モバイルQQのaccessTokenの有効期間は90日です。ウィーチャットのaccessTokenの有効期間は2時間です。|
|payToken|支払のトークンであり、このトークンはモバイルQQ支払に利用されます。モバイルQQの授権でこのトークンが戻ります。ウィーチャットの授権ではこのトークンが戻りません。有効期間は6日です。|
|offerId|支払時に使用し、AndroidのofferidはモバイルQqappidです|
|refreshToken|ウィーチャット・プラットフォーム独自のトークンであり、有効期間は30日です。ウィーチャットaccessTokenの期間切れ後にaccessTokenを更新します。|
|別アカウント|ゲームで授権したアカウントとモバイルQQ/ウィーチャットで授権したアカウントは同じではありません。このシーンを別アカウントと言います。|
|構造化メッセージ|共有メッセージの一種です。このメッセージを共有した後、表示形式としては左にサムネイル、右上にメッセージタイトル、右下にメッセージ概要です。|
|大画像メッセージ|共有メッセージの一種です。このメッセージは画像1枚だけで、表示も画像となります。大画像共有、純粋画像共有とも呼ばれています。|
|共遊び友達|モバイルQQ又はウィーチャットの友達で、同じゲームを遊んだ友達を共遊び友達と言います|
|ゲームセンター|モバイルQQクライアント又はウィーチャットクライアントのゲームセンターを統一にゲームセンターと言います。|
|ゲームホール|特にQQゲームホールを言います|
|プラットフォームによる実行|プラットフォーム又はチャンネル（モバイルQQ/ウィーチャット/ゲームホール/応用宝など）を通じてゲームを実行します|
|関係チェーン|ユーザーのプラットフォームでの友達関係|
|会話|モバイルQQ又はウィーチャットのチャット情報|
|インストール・チャンネル|ゲーム運営の前にパッケージングする時、チャンネル別に(例えば応用宝、豌豆莢,91など)チャンネル番号の異なるapkパッケージを生成します。インストールパッケージのチャンネル番号をインストールチャンネルと言います。|
|登録チャンネル|ユーザーは始めてログインする時、ゲームのインストールチャンネルが, ユーザーの登録チャンネルとして、 MSDKバックグラウンドで記録されます|
|Pf|支払に必要なフィールドであり、データ分析に使用します。Pfの構成としては、実行プラットフォーム_アカウント体系-登録チャンネル-OS-インストールチャンネル-自己定義フィールド.|
|pfKey| 支払に使用します|

バージョンの履歴
---
* [クリックしてMSDKバージョンの変更履歴を確認します](version.md)
