--- 
title: Azure CDN の各 SKU の特徴、トラブルシューティングの紹介
date: 2020-07-08 16:00:00 
tags: 
  - Network 
  - AzureCDN 
  - Troubleshooting 
--- 

こんにちは、Azure テクニカル サポート チーム箕輪です。<br>
<br>
Azure が提供している CDN サービスの Azure CDN において、ご利用いただける機能やよくあるお問合せ、各 SKU の特徴などを踏まえたトラブル シューティングの手順などをご紹介いたします。<br>
Azure CDN は現在 4 つの SKU があり、3 社の CDN プロバイダー (Microsoft / Verizon / Akamai) で CDN サービスを提供しております。<br>
本ブログは Azure CDN で提供している 4 つの SKU の特徴や、各 SKU における情報採取手順などをまとめたブログとなります。<br>
Azure CDN における共通的な事項については、以下のブログでまとめておりますのでご参照ください。<br>

*  [Azure CDN の特徴やよくあるお問い合わせ、トラブル シューティングの紹介](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-common.md)
<!---
  URL はリリース後に要修正。
-->

<!-- more --> 

本ブログでは、Azure CDN の各 SKU の特徴についてご案内しております。

* [Azure CDN における各 SKU の特徴](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-specific-sku.md#azure-cdn-%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8B%E5%90%84-sku-%E3%81%AE%E7%89%B9%E5%BE%B4)
* [ルール エンジンの設定](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-specific-sku.md#%E3%83%AB%E3%83%BC%E3%83%AB-%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%B3%E3%81%AE%E8%A8%AD%E5%AE%9A)
* [エラー発生時の情報採取手順](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-specific-sku.md#%E3%82%A8%E3%83%A9%E3%83%BC%E7%99%BA%E7%94%9F%E6%99%82%E3%81%AE%E6%83%85%E5%A0%B1%E6%8E%A1%E5%8F%96%E6%89%8B%E9%A0%86)

# Azure CDN における各 SKU の特徴
Azure CDN における各 SKU の機能一覧については、[公開情報](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-features)でご案内しております。<br>
本ブログでは、[公開情報](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-features) でご案内している内容をもとに、各 SKU の特徴について関連するドキュメントと紹介します。
<br>

## Standard Microsoft 
Standard Microsoft SKU は、Microsoft のインフラストラクチャー上で構成された CDN プラットフォームを用いてサービスを提供しています。<br>
Azure CDN としては一番最後に提供された SKU で、新規機能の追加などが随時行われております。<br>
Azure CDN の中で、2020 年 7 月時点で唯一 WAF 機能を利用できる SKU となっており、配信元が Azure サービスであればデータ転送料の観点で他の SKU よりも優れています。<br>
動的コンテンツの最適化については [Azure Front Door](https://docs.microsoft.com/ja-jp/azure/frontdoor/front-door-overview) で機能を提供しています。<br>
<br>

* ルール エンジンによるカスタマイズ設定<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-standard-rules-engine-reference](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-standard-rules-engine-reference)
* WAF 機能の追加 (プレビュー)<br>
[https://docs.microsoft.com/ja-jp/azure/web-application-firewall/cdn/cdn-overview](https://docs.microsoft.com/ja-jp/azure/web-application-firewall/cdn/cdn-overview)
* アクセス ログの取得<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/enable-raw-logs](https://docs.microsoft.com/ja-jp/azure/cdn/enable-raw-logs)
* 独自証明書で指定の認証局 (CA) による制限<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-troubleshoot-allowed-ca](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-troubleshoot-allowed-ca)
* 配信元が Azure サービスの場合、配信元から CDN へのデータ転送が無料<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-billing#which-origin-services-are-eligible-for-free-data-transfer-with-azure-cdn-from-microsoft](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-billing#which-origin-services-are-eligible-for-free-data-transfer-with-azure-cdn-from-microsoft)
<br>

## Standard / Premium Verizon 
Standard Verizon、Premium Verizon SKU は Verizon Degital Media 社の CDN プラットフォームを用いて CDN サービスを提供しています。<br>
Azure ポータルから Verizon のポータルに接続することで、CDN に関わるログをグラフ化して確認することができます。<br>
Azure Media Service において、ストリーミング エンドポイントで CDN を用いた際に既定で選択される SKU であり、メディア ストリーミングの最適化に優れています。<br>
Premium Verizon SKU は他の SKU と価格が異なりますが、多機能のルール エンジンや詳細なレポートなどがご利用いただけます。<br>
<br>

* 任意の認証局 (CA) が発行した証明書を利用可能
* キャッシュの事前読み込み機能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-preload-endpoint](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-preload-endpoint)
* 視覚化されたグラフ<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-analyze-usage-patterns](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-analyze-usage-patterns)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-custom-reports](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-custom-reports)
* メディア ストリーミングの最適化に優れている (Azure Media Service の既定の SKU)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-media-streaming-optimization#media-streaming-optimizations-for-azure-cdn-from-verizon](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-media-streaming-optimization#media-streaming-optimizations-for-azure-cdn-from-verizon)
* ルールエンジンによるカスタマイズ (Premium のみ)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-premium-rules-engine-reference](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-premium-rules-engine-reference)
* 詳細なレポート (Premium)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-advanced-http-reports](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-advanced-http-reports)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-real-time-stats](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-real-time-stats)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-edge-performance](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-edge-performance)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-real-time-alerts](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-real-time-alerts)
* トークン認証 (Premium)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-token-auth](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-token-auth)
<br>

## Standard Akamai
Standard Akamai SKU は Akamai 社の CDN プラットフォームを用いて CDN サービスを提供しております。<br>
Akamai 社は世界最大規模の CDN プラットフォームで CDN サービスを提供しており、動的なサイトの最適化に優れています。<br>
なお、Akamai 社が提供している Akamai Control Center (旧 Luna Control Center) は Azure CDN ではご利用いただけません。<br>
<br>

* [キャッシュ削除 (Purge)](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint) や設定変更の反映が迅速
* Let's Encrypt を用いたカスタムドメインの HTTPS 有効化
* 動的なサイトの最適化機能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-dynamic-site-acceleration](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-dynamic-site-acceleration)
* メディア ストリーミングの最適化機能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-media-streaming-optimization#media-streaming-optimizations-for-azure-cdn-from-akamai](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-media-streaming-optimization#media-streaming-optimizations-for-azure-cdn-from-akamai)
<br>

<br>

# ルール エンジンの設定
Azure CDN では 2020 年 7 月時点で Standard Microsoft SKU と Premium Verizon SKU でルール エンジンの機能を提供しています。<br>
ルール エンジンは以下の項目においてカスタマイズが可能です。<br><br>

* キャッシュ規則のカスタマイズ
* クライアント (ユーザー) からの要求をリダイレクト
* HTTP の要求ヘッダーや応答ヘッダーの変更
* (Premium Verizon SKU) クライアント (ユーザー) からの要求を拒否 (403 で応答) する

<br><br>

### Standard Microsoft SKU　におけるルール エンジンの設定
Stnadard Microsoft SKU のルール エンジンの設定は Azure ポータルから実施いただけます。<br>
ルール エンジンの設定の考え方として、対象の通信を **一致条件** で指定し、対象の通信に対して実施したい **アクション** を設定することで動作します。<br>
Standard Microsoft SKU のルール エンジンの設定は、エンドポイント単位で管理されております。<br>
<br>
設定の一例として、Azure CDN はクライアントから HTTP で接続された場合、配信元 (オリジン) に対して HTTP で接続します。<br>
この通信経路をすべて HTTPS で暗号化されたいシナリオにおいて、[ルール エンジンを利用した HTTPS リダイレクト](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-standard-rules-engine) を設定することで、クライアントから Azure CDN を介した通信経路がすべて HTTPS となります。<br>
この設定においては、Azure CDN のエンドポイントで受け入れプロトコルを HTTP/HTTPS としていただく点と、カスタムドメインにおいては HTTPS 有効化を構成いただく必要がある点はご留意ください。<br>

<br><br>

### Standard Microsoft SKU　におけるルール エンジンの設定のトラブルシューティング
Azure CDN は世界中の POP のリソースに設定したルール エンジンを適応する動作から、反映にはおおよそ 10 分時間が掛かることが想定された動作となります。<br>
そのため、設定直後に動作を確認いただいたとしても、反映が確認できないことがあります。<br>
また、1 度実施いただいた設定の反映の間に何度も設定を変更し保存した場合、設定情報が意図しない状態となる可能性がある点はご留意ください。<br>
<br>
ルール エンジンのご利用に至っては追加の費用などは発生しないため、お客様のご要件にあった設定をお試しいただき、意図した動作にならないなどご確認されたい事項がございましたら、以下の情報をもとに弊社サポート担当までお問い合わせください。<br>
<br>

**[ご提供いただきたい情報]**
* Azure CDN が構成されている 32 桁の サブスクリプションID
* Azure CDN のプロファイル名
* ルール エンジンを設定したエンドポイント名
* ルール エンジンの設定名
* 設定されたい要件の詳細

<br><br>

### Standard Microsoft SKU のルール エンジンに関するドキュメント

* [Azure CDN の Standard ルール エンジン リファレンス](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-standard-rules-engine-reference)
* [Azure CDN の Standard ルール エンジンの一致条件](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-standard-rules-engine-match-conditions)
* [Azure CDN の Standard ルール エンジンでのアクション](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-standard-rules-engine-actions)
* [Standard ルール エンジンの HTTPS リダイレクト設定](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-standard-rules-engine)

<br><br>

### Premium Verizon SKU におけるルール エンジンの設定
Premium Verizon SKU のルール エンジンの設定は Azure ポータルからログインできる Verizon ポータルで実施いただけます。<br>
概要としては、[こちらの公開情報](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-premium-rules-engine-reference) より確認いただけますが、ルール エンジンの設定の考え方として、対象の通信を **一致条件** で指定し、対象の通信に対して実施したい **機能** を設定することで動作します。<br>
Premium Verizon SKU のルール エンジンの設定は、プロファイル単位で管理されております。<br>
<br>
Premium Verizon SKU のルール エンジンは現在 v4 とよばれるバージョンで動作しており、ルール エンジンは **ポリシー** と呼ばれる単位で管理しています。<br>
**ポリシー** の中でルール エンジンを設定いただくと、**Draft** が構成されますので、**Draft** にある **Locl Draft as Policy** を選択後、**Deploy Request** から **Production** を選択いただき、**Create Deploy Request** と進めていただければ、**ポリシー** で設定いただいたルール エンジンが適応されます。<br>
**Create Deploy Request** 後、ルール エンジンの設定完了は数分で完了いたしますが、実際の環境に適応されるまでは構成によりますが、少なくとも 20 分はかかりますので、設定の反映を確認される場合はお時間を空けて動作をご確認ください。<br>
<br><br>

### Premium Verizon SKU におけるルール エンジンの設定のトラブルシューティング
Premium Verizon SKU のルール エンジンの設定においては、各プロセスにおいて検証がされる動作となりますので、設定ができない状態などの場合エラー メッセージが表示される動作となります。<br>
エラー メッセージが表示されないにも関わらず想定された動作とならない場合、Verizon 社のエンジニアと調査を致しますので、下記の情報をお問い合わせ時にご共有いただきますようお願いいたします。<br>
<br>

**[ご提供いただきたい情報]**
* Azure CDN が構成されている 32 桁の サブスクリプションID
* Azure CDN のプロファイル名
* ルール エンジンを設定されたい CDN のエンドポイント名
* ルール エンジン設定画面の右上に記載れた 5 桁の ID (Verizon Account ID)
* 設定されたルール エンジンの 画面キャプチャ
* 設定されたルール エンジンの XML 情報
* 設定されたい要件の詳細

<br><br>

### Premium Verizon SKU のルール エンジンに関するドキュメント

* [Azure CDN from Verizon Premium ルール エンジンのリファレンス](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-premium-rules-engine-reference)
* [Azure CDN from Verizon Premium ルール エンジンの条件式](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-premium-rules-engine-reference-conditional-expressions)
* [Azure CDN from Verizon Premium ルール エンジンの一致条件](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-premium-rules-engine-reference-match-conditions)
* [Azure CDN from Verizon Premium ルール エンジンの機能](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-verizon-premium-rules-engine-reference-features)
* [Azure CDN from Verizon Premium ルール エンジンの HTTP 変数](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-http-variables)

<br><br>

# WAF の設定
WAF の機能は 2020 年 7 月時点で Standard Microsoft においてプレビュー提供をしています。<br>
WAF では OWASP のルール セットに準拠した Azure で管理されたルール セットの他に、カスタム規則でルールを設定することができます。<br>
Standard Microsoft SKU で構成されたエンドポイントに対して 1 つの WAF が関連付けできます。<br>

[https://docs.microsoft.com/ja-jp/azure/web-application-firewall/cdn/cdn-overview](https://docs.microsoft.com/ja-jp/azure/web-application-firewall/cdn/cdn-overview)<br>

2020 年 7月時点の動作として、エンドポイントに関連付けられた WAF におけるカスタム規則の変更は一部制限がかかるため、意図した設定が追加できない場合は、一度エンドポイントとの関連付けを解除した上でカスタム規則の変更をお試しください。<br>
<br>

# エラー発生時の情報採取手順
Azure CDN では Microsoft 以外に Verizon 社と Akamai 社の CDN プラットフォームで CDN サービスを提供していることから、事象に応じて各社と連携して調査を実施する必要があります。<br>
SKU によって情報採取の手順が一部異なりますので、弊社サポート担当にお問い合わせいただく際に、以下をご確認いただき、情報採取にご協力いただければ幸いです。<br>
<br><br>

## **Standard Microsoft** においてエラー発生時にご提供いただきたい情報
Standard Microsoft SKU では、CDN を介した通信における HTTP ヘッダの x-azure-ref が、サポート担当が Azure 基盤のログを確認するために必要な情報となります。<br>
そのため、調査においては以下の情報に加えて、HTTP ヘッダの x-azure-ref を取得いただき、お問合せ時にご共有いただけますと幸いです。<br>
<br>

**[ご提供いただきたい情報]**
* Azure CDN が構成されている 32 桁の サブスクリプションID
* Azure CDN のプロファイル名
* Azure CDN のエンドポイント名
* 事象が発生している URL (FQDN) 
* 事象の概要
* HTTP ヘッダの x-azure-ref の結果 (画像ではなくテキスト ベースでご共有願います)

<br>

1. [Web プラウザにおけるネットワーク トレース (F12)](https://docs.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace) を取得して確認する
2. 以下の curl コマンドで確認する。

```bash
curl -I  https://contoso-cdn.azureedge.net/xxx/xxx.png
```
(上記の contoso-cdn を対象の FQDN に置き換えてください)
<br><br>

## **Standard Verizon / Premium Verizon** においてエラー発生時にご提供いただきたい情報
Standard Verizon、Premium Verizon SKU は Verizon 社の CDN プラットフォームでサービスを提供しているため、調査は Verizon 社のエンジニアと共同で実施いたします。<br>
調査においては、以下の情報をご共有いただけますと幸いです。<br>
<br>

**[ご提供いただきたい情報]**
* Azure CDN が構成されている 32 桁の サブスクリプションID
* Azure CDN のプロファイル名
* Azure CDN のエンドポイント名
* 事象が発生している URL (FQDN) 
* 事象の概要
* [Web プラウザにおけるネットワーク トレース (F12) の結果](https://docs.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace)
<br><br>

## **Standard Akamai** においてエラー発生時にご提供いただきたい情報
Standard Akamai SKU は Akamai 社の CDN プラットフォームでサービスを提供しているため、調査は Akamai 社のエンジニアと共同で実施いたします。<br>
Standard Akamai SKU では、CDN からエラーメッセージを応答された場合に、画面上に **Reference** と呼ばれるエラー コードが記載されます。<br>
この  **Reference**  のエラー コードをご共有いただくことで、ご状況について推察することも可能となるため、お問合せ時にご共有いただけますと幸いです。<br>
<br>
また、curl コマンドを利用したトラブルシューティングでは、CDN からの HTTP ヘッダ情報を取得することで状況を確認いただけますが、Akamai CDN では指定のパラメーターを追加で付与いただかない限り、Akamai の HTTP ヘッダが参照できません。<br>
そのため、事象が発生した状況で、下記でご案内する curl コマンドの実行結果をお問合せ時に合わせてご共有いただけると、Akamai 社のエンジニアとスムーズに連携して調査が実施できますので、ご協力いただけますと幸いです。<br>
<br>

![参考 : Reference のエラー コード](./cdn-specific-sku/reference-error-code.png)  
<br>
**[ご提供いただきたい情報]**
* Azure CDN が構成されている 32 桁の サブスクリプションID
* Azure CDN のプロファイル名
* Azure CDN のエンドポイント名
* 事象が発生している URL (FQDN) 
* 事象の概要
* Reference のエラー コード(画像ではなくテキスト ベースでご共有願います)
* 以下の curl コマンドの実行結果

```bash
curl -H \
 "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" \
 -IXGET https://www.contoso.com/
```
(www.contoso.com の箇所は CDN にアクセスする際の URL に変えて実施ください)
<br><br>

<br>

*※ 本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。*
