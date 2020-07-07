--- 
title: Azure CDN の特徴、トラブルシューティング手順
date: 2020-05-11 12:30:00 
tags: 
  - Network 
  - AzureCDN 
  - Troubleshooting 
--- 

こんにちは、Azure テクニカル サポート チーム箕輪です。
Azure で提供している CDN サービスのAzure CDN において、ご利用いただける機能やよくあるお問合せ、各 SKU の特徴などを踏まえたトラブル シューティングの手順などをご紹介いたします。
Azure CDN には現在 4 つの SKU があり、3 つのプロバイダー (Microsoft / Verizon / Akamai) で CDN サービスを提供しております。
本ブログは Azure CDN における共通的な事項をまとめたブログとなります。
各 SKU に応じた特徴などは以下のブログでまとめておりますのでご参照ください。

・ [Azure CDN の SKU 毎の特徴](https://xxx)

<!-- more --> 

本ブログでは、Azure CDN の共通的な機能として、以下の項目についてご案内しております。
(各 SKU における特徴については [こちら](https://xxx) をご覧ください。)

![Azure CDN の特徴]()
![よくあるお問合せ]()
![エラー発生時の切り分け手順]()

## Azure CDN の特徴
Azure CDN の特徴として、4 つの SKU でご利用いただける機能について以下の通り関連するドキュメントを記載します。

* ランニングコストがかからない、転送量に応じた課金体系<br>
[https://azure.microsoft.com/ja-jp/pricing/details/cdn/](https://azure.microsoft.com/ja-jp/pricing/details/cdn/)
* 任意のリソース名でプロファイルとエンドポイントが構成でき、エンドポイントは既定で HTTPS に対応<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-create-new-endpoint](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-create-new-endpoint)
* お客様に管理された任意のドメイン (カスタムドメイン) を利用可能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-map-content-to-custom-domain](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-map-content-to-custom-domain)
* Azure CDN で管理された無料の SSL 証明書を用いてカスタムドメインの HTTPS 接続を利用可能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-custom-ssl?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-custom-ssl?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate)
* キャッシュされたコンテンツのキャッシュ削除 (Purge)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint)
* コンテンツのキャッシュ期間の設定<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-caching-rules](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-caching-rules)
* キャッシュされたコンテンツのファイル圧縮機能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-improve-performance](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-improve-performance)
* リクエストの数などのログ提供<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-azure-diagnostic-logs](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-azure-diagnostic-logs)
* 国/地域別のアクセス制御<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-restrict-access-by-country](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-restrict-access-by-country)

## Azure CDN のよくあるお問合せ
Azure CDN においてよくお問い合わせいただく内容についてご紹介いたします。

### Azure CDN のリソースのリージョンを指定したい / 日本だけに固定したい
Azure CDN では、世界中で構成された CDN の POP からサービスを提供しており、リージョン単位でのサービス提供はしておりません。
Azure CDN のプロファイルを構成される場合、既定では global というリージョンでリソースが構成され、特定のリージョンだけにお客様の構成情報を適応するといった機能はありません。
Azure CDN の構成方法によっては Azure CDN のリージョンが global 以外になる場合がございますが、いずれのリージョンであっても動作や機能に違いはありません。

接続制限という観点でリージョンを指定されたい場合は、[国/地域別の Azure CDN コンテンツの制限](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-restrict-access-by-country) の機能をご利用ください。

### Azure CDN から更新されたコンテンツが配信されない
Azure CDN はクライアント (ユーザー) からの要求 URL に対して、CDN 上でキャッシュされたコンテンツがあれば CDN から応答し、キャッシュされたコンテンツが無い場合やキャッシュの有効期間が過ぎている場合は、配信元に対象のコンテンツを取得した上で、クライアント (ユーザー) に応答する動作となります。

配信元のコンテンツを更新されたにもかかわらず、Azure CDN から更新される前のコンテンツが応答される場合は、Azure CDN 上のキャッシュを削除 (Purge) いただくことで、Azure CDN はクライアント (ユーザー) からの要求 URL に対して、配信元に更新されたコンテンツを取得し、クライアント (ユーザー)  に応答します。

### Azure CDN を利用した際に発生する費用の考え方について
Azure CDN における一般的な費用の考え方については、以下の弊社公開情報にてご案内しておりますのでご参照ください。
なお、お客様の固有のご環境における費用のご質問について、課金観点でお問合せいただきますようお願い申し上げます。

[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-billing](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-billing)

### 新規でエンドポイントを設定したがエラーになる
Azure CDN は世界中で構成された CDN の POP からサービスを提供しているため、世界中で一意の名前で構成される必要があります。
設定されたいエンドポイント名が既に利用されている場合はエラーとなり設定ができないため、別のエンドポイント名をご利用ください。
(Azure CDN のプロファイル名にはこの動作は適応されません。)

設定されたいエンドポイント名が利用可能かは、[REST API](https://docs.microsoft.com/en-us/rest/api/cdn/checknameavailability) によりご確認いただけます。

```
{
  "name": "contoso-cdn",
  "type": "Microsoft.Cdn/Profiles/Endpoints"
}
【上記の contoso-cdn をお客様のご利用されたいリソース名に置き換えてください】
```

### カスタムドメインを別のエンドポイントに移行したい
Azure CDN のエンドポイントに設定いただいたカスタムドメインを、別のエンドポイントに移行する場合は、一度既存のカスタムドメインの設定を削除いただいた上で移行していただく必要があります。

### CORS に対応する方法を知りたい
HTTP の 1 つの機能である CORS (クロス オリジン リソース共有) の利用方法については、[公開情報](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-cors) がございますのでご参照ください。
設定後、意図した動作とならない場合は、一度 [キャッシュ削除 (Purge)](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint) を実行いただき、動作をご確認ください。

### 独自の証明書 (準備いただいた証明書) の利用方法
Azure CDN で提供している 4 つの SKU のうち、独自の証明書がご利用いただけるのは Standard Microsoft と Standard Verizon、Premium Verizon となります。Standard Akamai では独自の証明書はご利用いただけません。
独自の証明書をご利用いただく際は、Azure CDN と同じサブスクリプション上に Azure Key Vault リソースを構成いただき、証明書をアップロードいただく必要があります。
Standard Microsoft では [特定の認証局 (CA) で発行された証明書](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-troubleshoot-allowed-ca) である必要があります。
Standard Verizon、Premium Verizon においては、任意の認証局 (CA) で発行された証明書が利用できますが、自己証明書は利用できず、また Azure Key Vault にアップロードいただく証明書には、認証局 (CA) の中間証明書を含めるように構成いただく必要があります。

以下の openssl コマンドで、中間 CA 証明書を含んだ PFX 形式の証明書を構成いただけるのでご参照ください。
```
openssl pkcs12 -export -in PublicSSLcertificate.cer -inkey PrivateKey.key -certfile IntermediateCAcertificate.cer -out PFXcertificate.pfx
 
* PublicSSLcertificate.cer : 認証局 (CA) より発行された SSL 証明書
* PrivateKey.key : 認証局 (CA) に送付した CSR を構成された際の秘密鍵
* IntermediateCAcertificate.cer : 認証局 (CA) の中間 CA 証明書
* PFXcertificate.pfx : openssl コマンドにより出力される PFX 形式のファイル
```

### Azure CDN で利用されている IP リストが知りたい
Azure CDN で利用されている IP リストは[公開情報](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-pop-list-api) にて公開されております。
Standard Akamai においては、公開された IP リストはありません。

### 配信元 (オリジン) に HTTPS だけ接続するようにしたい
Standard Microsoft と Premium Verizon で提供しているルール エンジンを利用いただくことで CDN 上で HTTPS にリダイレクトするように構成いただけます。
なお、Azure CDN のエンドポイントで HTTP を許可していない場合、HTTPS へのリダイレクトは動作しない点はご注意ください。

HTTPS へのリダイレクト
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-storage-custom-domain-https#http-to-https-redirection](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-storage-custom-domain-https#http-to-https-redirection)

### Azure CDN で認証サービスを利用したい。
Premium Verizon では トークン認証 の機能を提供しておりますが、それ以外の認証機能 (基本認証、クライアント認証など) は Azure CDN では提供しておりません。
配信元として Azure Storage で提供している SAS (Shared Access Signature) を用いた認証については、[公開情報](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-sas-storage-support) をご参照ください。


## 構築・設定変更時のエラーにおける確認手順
Azure CDN を新規で構築されている場合、または設定変更後に Azure CDN を介してコンテンツが取得できなくなった場合は以下をお試しください。

### Azure CDN を新規で構築した場合
Azure CDN を新規で構築された場合、世界中にある CDN のリソースに構成情報を展開する動作から、新規で構成いただいた後に反映までしばらくお時間を要します。
構成いただいた SKU 毎に設定の反映の時間が異なりますが、特に Verizon SKU をご利用いただいているご環境においては、設定の反映に 90 分はお時間を要し、90 分以内にご確認いただいても想定された動作とならない可能性がある点はご留意願います。

　 Microsoft Standard SKU：10 分
 　Verizon Standard / Verizon Premium SKU： 90 分
 　Akamai Standard SKU：1 分

### DNS 関連の変更をした場合
DNS 関連の変更などをした場合、DNS の設定の反映にしばらく時間が掛かかることで意図した結果とならない可能性があります。
例えば DNS の参照する先を変更した場合などは、クライアント環境においてキャッシュ DNS をクリアなどしてから状況が変わるかをお試しください。
また、[オンライン ツール](https://digwebinterface.com/) をご利用いただき、DNS の最新の状況についてご確認ください。


### Azure CDN の設定反映
Azure CDN は世界中にある CDN のリソースに構成情報を展開する動作から、Azure ポータルなどで変更された内容が反映されるまで時間を要します。
設定変更後にしばらく時間を空けずに再設定されると、設定の反映に齟齬が発生し、設定が適応されない事象などが発生する可能性があるため、設定変更後は少なくとも 10 分ほどはお時間を空けてからお試しください。
各設定の反映を確認するために、[キャッシュの削除 (Purge)](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint) を一度実施いただくことをお勧めいたします。
また、カスタムドメインの再設定などにおいては、短時間で削除、再生成するとエラーとなる可能性があるため、削除後しばらく時間を空けてから再生成ください。

Microsoft Standard や Premium Verizon で利用いただけるルール エンジンや Microsoft Standard でご利用いただける WAF については、以下をご参照ください。

![ルール エンジンについて]()
![WAF について]()

## 接続エラーなどが発生した際の切り分けの手順
設定変更などを実施されていないご状況において、Azure CDN を介してコンテンツが取得できなくなった場合は以下をお試しください。

0. エラー状況の確認
Azure CDN を介してエラーが発生した場合、Web プラウザのネットワーク トレース (F12) の機能を利用した応答結果の確認をすることで、エラーの状況を確認することができます。
Web プラウザにおけるネットワーク トレース (F12) の取得方法については、[弊社公開情報](https://docs.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace)をご参照ください。

!HTTP レスポンスが応答されない 
   DNS レイヤーや TCP レイヤーで異常が発生し接続ができていない可能性があります。
   Azure CDN はパブリック ネットワークから接続ができるため、異なるクライアント環境やネットワーク環境から接続が可能かをご確認ください。

! いずれかの HTTP レスポンスが応答される
   Azure CDNかバックエンドとはネットワーク的に接続がされている状況を確認できます。
   以下の手順で被疑箇所と思われる箇所を切り分けます。

1. 配信元の状況確認
Azure CDN を介してコンテンツが取得できない場合は、配信元 (オリジン) に直接接続し、想定されるコンテンツが取得できるかをご確認ください。
配信元からコンテンツが取得できない場合、配信元の影響により Azure CDN を介して取得できていない可能性があります。

2. 配信元の証明書の状態を確認する
Azure CDN はクライアントからの HTTPS 接続をそのまま配信元に転送するため、クライアントから配信元までの間で HTTPS 接続のための SSL 証明書の有効期限切れなどが発生した場合、エラーが応答される場合があります。
証明書の状態については、以下のopenssl コマンドを用いて確認することが可能です。

```
[Azure CDN へのTLS 接続確認]
openssl s_client -connect <Azure CDN のエンドポイント (xxx..azureedge.net)>:443 -servername <FQDN> -showcerts

[バックエンドへの TLS 接続確認]
openssl s_client -connect <バックエンドのパブリック IP アドレスや FQDN>:443 -servername <FQDN> -showcerts
```

3. キャッシュ クリア (Purge)
Azure CDN 上でキャッシュされたコンテンツが意図しない応答となりエラーとなっている可能性があります。
一度、[キャッシュ削除 (Purge)](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint) を実施いただき、Azure CDN から配信元に再度接続するように試行いただき、エラー状況をご確認ください。

上記を実施いただき、事象が改善されない場合は、ご利用いただいている SKU に応じて情報取得をいただき、弊社サポート担当までお問い合わせください。

* [Standard Microsoft SKU をご利用いただいている場合]()
* [Standarad / Premium Verizon をご利用いただいている場合]()
* [Standard Akamai をご利用いただいている場合]()

_※ 本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。_
