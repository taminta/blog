--- 
title: Azure CDN の各 SKU の特徴、トラブルシューティング手順
date: 2020-07-08 16:00:00 
tags: 
  - Network 
  - AzureCDN 
  - Troubleshooting 
--- 

こんにちは、Azure テクニカル サポート チーム箕輪です。<br>
<br>
Azure で提供している CDN サービスのAzure CDN において、ご利用いただける機能やよくあるお問合せ、各 SKU の特徴などを踏まえたトラブル シューティングの手順などをご紹介いたします。<br>
Azure CDN には現在 4 つの SKU があり、3 つのプロバイダー (Microsoft / Verizon / Akamai) で CDN サービスを提供しております。<br>
本ブログは Azure CDN で提供している 4 つの SKU の特徴や、各 SKU における情報採取手順などをまとめたブログとなります。<br>
<br>
Azure CDN における共通的な事項については、以下のブログでまとめておりますのでご参照ください。<br>

*  [Azure CDN の特徴、トラブルシューティング手順](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-common.md)

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

_執筆中_

# WAF の設定
WAF の機能は 2020 年 7 月時点で Standard Microsoft においてプレビュー提供をしています。<br>
WAF では OWASP のルール セットに準拠した Azure で管理されたルール セットの他に、カスタム規則でルールを設定することができます。<br>
Standard Microsoft SKU で構成されたエンドポイントに対して 1 つの WAF が関連付けできます。<br>

[https://docs.microsoft.com/ja-jp/azure/web-application-firewall/cdn/cdn-overview](https://docs.microsoft.com/ja-jp/azure/web-application-firewall/cdn/cdn-overview)<br>

2020 年 7月時点の動作として、エンドポイントに関連付けられた WAF におけるカスタム規則の変更は一部制限がかかるため、意図した設定が追加できない場合は、一度エンドポイントとの関連付けを解除した上でカスタム規則の変更をお試しください。<br>
<br>

# エラー発生時の情報採取手順
Azure CDN では Microsoft 以外に Verizon 社と Akamai 社の CDN プラットフォームで CDN サービスを提供していることから、事象に応じて各社と連携して調査を実施する必要があります。<br>
SKU によって情報採取の手順が一部異なりますので、弊社サポート担当にお問い合わせいただく際に、以下の手順で情報採取にご協力いただければ幸いです。<br>
<br>

## **Standard Microsoft** においてエラー発生時にご提供いただきたい情報
Standard Microsoft SKU では、CDN を介した通信における HTTP ヘッダの x-azure-ref が、サポート担当が Azure 基盤のログを確認するために必要な情報となります。<br>
そのため、エラー発生時にご提供いただきたい情報として、以下のいずれかの方法で HTTP ヘッダの x-azure-ref を取得いただき、お問合せ時にご共有いただけますと幸いです。<br>

* [Web プラウザにおけるネットワーク トレース (F12)](https://docs.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace) で確認する。
* curl コマンドで確認する。
'''bash
curl -I  https://contoso-cdn.azureedge.net/xxx/xxx.png
'''
(上記の contoso-cdn を対象の FQDN に置き換えてください)
<br>

## **Standard Verizon / Premium Verizon** においてエラー発生時にご提供いただきたい情報

_執筆中_

エラー発生時にご提供いただきたい情報

F12 と
・ルール エンジン

XML や右上の 5 桁の ID を教えて
Pilice ID を共有して


## **Standard Akamai** においてエラー発生時にご提供いただきたい情報
Akamai Standard SKU では、CDN からエラーメッセージを応答された場合に、画面上に **Reference** と呼ばれるエラー コードが記載されます。<br>
弊サポート担当にお問い合わせいただいた際、Akamai 社と共同で調査を実施いたしますが、この「Reference」の情報があると調査が比較的早く実施できる可能性があるため、お問合せ時にご共有いただければ幸いです。<br>

_画像準備_

<br>
curl コマンドを利用したトラブルシューティングでは、CDN からのヘッダ情報を取得することで状況を確認いただけますが、Akamai CDN では以下のパラメーターを追加で付与いただかない限り、Akamai のヘッダが参照できません。<br>
そのため、事象が発生した際に以下の curl コマンドの結果をお問合せ時に合わせてご共有いただけると幸いです。
<br>

```bash
$ curl -H \
 "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" \
 -IXGET https://www.contoso.com/
```
(www.contoso.com の箇所は CDN にアクセスする際の URL に変えて実施ください)

<br>

*※ 本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。*
