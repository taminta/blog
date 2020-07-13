--- 
title: Azure CDN の特徴やよくあるお問い合わせ、トラブル シューティングの紹介
date: 2020-07-14 09:00:00 
tags: 
  - Network 
  - AzureCDN 
  - Troubleshooting 
--- 

こんにちは、Azure テクニカル サポート チーム箕輪です。<br>
<br>
Azure が提供している CDN サービスの Azure CDN において、ご利用いただける機能やよくあるお問合せ、各 SKU の特徴などを踏まえたトラブル シューティングの手順などをご紹介いたします。<br>
Azure CDN は現在 4 つの SKU があり、3 社の CDN プロバイダー (Microsoft / Verizon / Akamai) で CDN サービスを提供しています。<br>
本ブログは Azure CDN における共通的な事項をまとめたブログとなります。<br>
各 SKU の特徴や、各 SKU における情報採取の手順については、以下のブログでまとめていますのでご参照ください。

*  [Azure CDN の各 SKU の特徴、トラブルシューティングの紹介](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-specific-sku.md)
<!---
  URL はリリース後に要修正。
-->

<!-- more --> 

本ブログでは、Azure CDN の共通的な機能として、以下の項目についてご案内しています。<br>

* Azure CDN の特徴
* Azure CDN のよくあるお問合せ
* 構築・設定変更時のエラーのトラブルシューティング
* 接続エラーなどが発生した際のトラブルシューティング

<br>

# Azure CDN の特徴
Azure CDN は 3 社の CDN プロバイダー (Microsoft / Verizon / Akamai)  で CDN サービスを提供しています。<br>
各社の CDN プラットフォームによりご利用いただける機能が異なり、各 SKU の比較は [弊社公開情報](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-features) でご案内しています。<br>
Azure CDN の 4 つの SKU で共通して利用できる機能について、弊社公開情報のリンクとあわせてご紹介します。

* 固定費用がかからない、転送量に応じた課金体系<br>
[https://azure.microsoft.com/ja-jp/pricing/details/cdn/](https://azure.microsoft.com/ja-jp/pricing/details/cdn/)
* 任意のリソース名でプロファイルとエンドポイントが構成でき、既定のエンドポイントは HTTPS に対応<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-create-new-endpoint](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-create-new-endpoint)
* お客様に管理された任意のドメイン (カスタムドメイン) を利用可能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-map-content-to-custom-domain](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-map-content-to-custom-domain)
* Azure CDN で管理された無料の SSL 証明書を用いたカスタムドメインの HTTPS 接続を提供<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-custom-ssl?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-custom-ssl?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate)
* キャッシュされたコンテンツのキャッシュ削除 (Purge)<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint)
* コンテンツのキャッシュ期間の設定<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-caching-rules](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-caching-rules)
* キャッシュされたコンテンツのファイル圧縮機能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-improve-performance](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-improve-performance)
* リクエストやキャッシュ ヒット数などのログ確認機能<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-azure-diagnostic-logs](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-azure-diagnostic-logs)
* 国/地域別のアクセス制御<br>
[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-restrict-access-by-country](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-restrict-access-by-country)

<br>

# Azure CDN のよくあるお問合せ
Azure CDN において、お客様よりよくお問い合わせいただく内容についてご紹介いたします。<br>
<br>

#### Azure CDN のリソースのリージョンを指定したい / Azure CDN のリソースを日本だけに固定したい
Azure CDN では、世界中で構成された CDN の POP からサービスを提供しており、リージョン単位でのサービス提供はしていません。<br>
Azure CDN のプロファイルを構成される場合、既定では global というリージョンでリソースが構成され、特定のリージョンだけにお客様の構成情報を適応するといった機能はありません。<br>
Azure CDN の構成方法によっては Azure CDN のリージョンが global 以外となる場合がありますが、いずれのリージョンであっても動作や機能に違いはありません。<br>

接続制限という観点で特定のリージョンを指定されたい場合は、[国/地域別の Azure CDN コンテンツの制限](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-restrict-access-by-country) の機能をご検討ください。<br>
<br>

#### Azure CDN から更新されたコンテンツが配信されない
Azure CDN はクライアント (ユーザー) からの要求 URL に対して、CDN 上でキャッシュされたコンテンツがあれば CDN から応答し、キャッシュされたコンテンツが無い場合やキャッシュの有効期間が過ぎている場合は、配信元に対象のコンテンツを取得した上で、クライアント (ユーザー) に応答する動作となります。

配信元のコンテンツを更新されたにもかかわらず、Azure CDN から更新される前のコンテンツが応答される場合は、Azure CDN 上のキャッシュを削除 (Purge) いただくことで、Azure CDN はクライアント (ユーザー) からの要求 URL に対して、配信元に更新されたコンテンツを取得し、クライアント (ユーザー)  に応答します。<br>
<br>

#### Azure CDN を利用した際に発生する費用の考え方について
Azure CDN おいて発生する費用の考え方については、以下の弊社公開情報にてご案内しています。<br>
実際に発生する費用や、お客様の固有のご環境における費用のご質問についてご確認されたい場合は、課金の観点で弊社サポート担当までお問い合わせください。

[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-billing](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-billing)<br>
<br>

#### 新規でエンドポイントを設定したがエラーになる
Azure CDN は世界中で構成された CDN の POP から CDN サービスを提供しているため、世界中で一意のリソース名で構成される必要があります。<br>
設定されたいエンドポイント名が既に利用されている場合はエラーとなり設定ができないため、別のエンドポイント名をご利用ください。<br>
(Azure CDN のプロファイル名は任意のリソース名を設定できます。)

設定されたいエンドポイント名が利用可能かは、[こちらの REST API](https://docs.microsoft.com/en-us/rest/api/cdn/checknameavailability) によりご確認いただけます。<br>
(以下は Body のサンプルとなりますが、contoso-cdn をお客様のご利用されたいリソース名に置き換えて REST API を実行してください。)

```json
{
  "name": "contoso-cdn",
  "type": "Microsoft.Cdn/Profiles/Endpoints"
}
```

#### カスタムドメインを別のエンドポイントに移行したい
Azure CDN のエンドポイントに設定されたカスタムドメインの設定を維持したままエンドポイントの変更はできません。<br>
カスタムドメインを別のエンドポイントに移行する場合、既存のカスタムドメインの設定を削除いただいた上で移行する必要があります。<br>
<br>

#### CORS に対応する方法を知りたい
HTTP ヘッダの CORS (クロス オリジン リソース共有) の利用方法については、以下の弊社公開上でご案内しています。<br>
CORS の設定後、意図した動作とならない場合は、一度キャッシュ削除 (Purge) を実行いただき、動作をご確認ください。

[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-cors](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-cors) <br>
<br>

#### 独自の証明書を利用したカスタムドメインの HTTPS 有効化について
カスタムドメインの HTTPS 有効化において、Azure CDN で管理された証明書 (DigiCert 社の証明書) 以外に、独自の証明書 (お客様に準備いただいた証明書) がご利用できます。<br>
独自の証明書がご利用いただけるのは Standard Microsoft と Standard Verizon、Premium Verizon となります。<br>
Standard Akamai では独自の証明書はご利用いただけず、Let's Encrypt の証明書で HTTPS 接続が利用できます。<br>
独自の証明書をご利用いただく際は、Azure CDN のリソースが構成された同じサブスクリプションに Azure Key Vault リソースを構成いただき、証明書をアップロードする必要があります。

なお、Standard Microsoft では [特定の認証局 (CA) で発行された証明書](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-troubleshoot-allowed-ca) をご利用いただく必要があります。<br>
Standard Verizon、Premium Verizon においては、任意の認証局 (CA) で発行された証明書が利用できますが、自己証明書は利用できず、また Azure Key Vault にアップロードいただく証明書には認証局 (CA) の中間証明書を含めるように構成する必要があります。
以下の openssl コマンドで、中間 CA 証明書を含んだ PFX 形式の証明書を構成できますのでご参照ください。

```bash
openssl pkcs12 -export -in PublicSSLcertificate.cer -inkey PrivateKey.key -certfile IntermediateCAcertificate.cer -out PFXcertificate.pfx
 
* PublicSSLcertificate.cer : 認証局 (CA) より発行された SSL 証明書
* PrivateKey.key : 認証局 (CA) に送付した CSR を構成された際の秘密鍵
* IntermediateCAcertificate.cer : 認証局 (CA) の中間 CA 証明書
* PFXcertificate.pfx : openssl コマンドにより出力される PFX 形式のファイル
```
<br>

#### Azure CDN で利用されている IP リストが知りたい
Azure CDN で利用されている IP リストは、以下の弊社公開情報にて公開しています。<br>
Standard Akamai においては、公開された IP リストはありません。<br>
配信元 (オリジン) で Azure CDN からの接続を IP リストで制限されたい場合は、Standard Microsoft、Standard Verizon、Premium Verizon をご利用ください。

[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-pop-list-api](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-pop-list-api)<br>
<br>

#### 配信元 (オリジン) に HTTPS だけ接続するようにしたい
Azure CDN の動作として、クライアント (ユーザー) からの要求プロトコルを用いて配信元 (オリジン) に接続する動作となります。<br>
クライアント (ユーザー) が HTTP で要求 URL を送信した場合、Azure CDN は配信元 (オリジン) に HTTP でコンテンツを取得します。

クライアント (ユーザー) からの HTTP の要求 URL を HTTPS にリダイレクトしたい場合、Standard Microsoft と Premium Verizon で提供しているルール エンジンを利用いただく必要があります。<br>
Standard Akamai や Standard Verizon はルール エンジンが利用できないため、HTTPS リダイレクトは構成できません。<br>
HTTPS リダイレクトを構成する方法は、以下の弊社公開情報でご案内しておりますのでご参照ください。<br>
なお、Azure CDN のエンドポイントの設定で、HTTP を受け入れプロトコルとして許可していない場合、HTTPS へのリダイレクトは動作しない点はご注意ください。

[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-storage-custom-domain-https#http-to-https-redirection](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-storage-custom-domain-https#http-to-https-redirection)<br>
<br>

#### Azure CDN で認証サービスを利用したい。
Premium Verizon では [トークン認証](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-token-auth) の機能を提供しておりますが、それ以外の認証機能 (基本認証、クライアント認証など) は Azure CDN では提供していません。<br>
配信元 (オリジン) として Azure Storage をご利用されている環境では、Azure Storage の SAS (Shared Access Signature) の機能を利用し、以下の公開情報でご案内しておりますのでご参照ください。

[https://docs.microsoft.com/ja-jp/azure/cdn/cdn-sas-storage-support](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-sas-storage-support) <br>
<br>

#### Azure CDN で対象の URL (コンテンツ) がキャッシュされているかを確認したい。
Azure CDN でキャッシュがされているかを確認する方法として、Azure ポータルなどで特定のコンテンツが Azure CDN 上でキャッシュされているかどうかを確認する機能はございませんが、HTTP ヘッダの x-cache の結果で確認することができます。<br>
x-cache が HIT や TCP_HIT となっている場合、Azure CDN 上でキャッシュされたコンテンツが応答されている状態となります。<br>

HTTP ヘッダの x-cache の確認方法としては、[Web プラウザにおけるネットワーク トレース (F12) ](https://docs.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace) を利用いただくか、以下の curl コマンドでご確認いただけます。<br>

```bash
 curl -H https://contoso-cdn.azureedge.net/xxx/xxx
```
(contoso-cdn.azureedge.net の箇所は CDN にアクセスする際の URL に変えて実施ください)<br>

Standard Akamai SKU では以下の curl コマンドで確認することができます。

```bash
 curl -H "Pragma: akamai-x-cache-on" \
 -IXGET https://contoso-cdn.azureedge.net/xxx/xxx
```
(contoso-cdn.azureedge.net の箇所は CDN にアクセスする際の URL に変えて実施ください)
<br><br>

# 構築・設定変更時のエラーのトラブルシューティング
Azure CDN を新規で構築されている場合、または設定変更後に Azure CDN を介してコンテンツが取得できなくなった場合は以下をお試しください。
<br>

#### Azure CDN を新規で構築した場合
Azure CDN を新規で構築された場合、世界中にある CDN のリソースに構成情報を展開する動作から、新規で構成いただいた後に反映までしばらくお時間を要します。<br>
構成いただいた SKU 毎に設定の反映の時間が異なりますが、特に Verizon SKU をご利用いただいているご環境においては、設定の反映に 90 分はお時間を要し、90 分以内にご確認いただいても想定された動作とならない可能性がある点はご留意願います。

　 Microsoft Standard SKU：10 分
 　Verizon Standard / Verizon Premium SKU： 90 分
 　Akamai Standard SKU：1 分
<br><br>
#### DNS 関連の変更をした場合
DNS 関連の変更などをした場合、DNS の設定の反映にしばらく時間が掛かかることで意図した結果とならない可能性があります。<br>
例えば DNS の参照する先を変更した場合などは、クライアント環境においてキャッシュ DNS をクリアなどしてから状況が変わるかをお試しください。<br>
また、[オンライン ツール](https://digwebinterface.com/) をご利用いただき、DNS の最新の状況についてご確認ください。
<br><br>
#### Azure CDN の設定反映
Azure CDN は世界中にある CDN のリソースに構成情報を展開する動作から、Azure ポータルなどで変更された内容が反映されるまで時間を要します。<br>
設定変更後にしばらく時間を空けずに再設定されると、設定の反映に齟齬が発生し、設定が適応されない事象などが発生する可能性があるため、設定変更後は少なくとも 10 分ほどはお時間を空けてからお試しください。<br>
各設定の反映を確認するために、[キャッシュの削除 (Purge)](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint) を一度実施いただくことをお勧めいたします。<br>
また、カスタムドメインの再設定などにおいては、短時間で削除、再生成するとエラーとなる可能性があるため、削除後しばらく時間を空けてから再生成ください。
<br><br>
Microsoft Standard や Premium Verizon で利用いただけるルール エンジンや Microsoft Standard でご利用いただける WAF については、以下をご参照ください。

*  [ルール エンジンについて]()
*  [WAF について]()
<br><br>

# 接続エラーなどが発生した際のトラブルシューティング
設定変更などを実施されていないご状況において、Azure CDN を介してコンテンツが取得できなくなった場合は以下をお試しください。<br><br>

0. エラー状況の確認
Azure CDN を介してエラーが発生した場合、Web プラウザのネットワーク トレース (F12) の機能を利用した応答結果の確認をすることで、エラーの状況を確認することができます。<br>
Web プラウザにおけるネットワーク トレース (F12) の取得方法については、[弊社公開情報](https://docs.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace)をご参照ください。

  *  HTTP レスポンスが応答されない<br>
     DNS レイヤーや TCP レイヤーで異常が発生し接続ができていない可能性があります。<br>
   Azure CDN はパブリック ネットワークから接続ができるため、異なるクライアント環境やネットワーク環境から接続が可能かをご確認ください。

  *  いずれかの HTTP レスポンスが応答される<br>
     Azure CDNかバックエンドとはネットワーク的に接続がされている状況を確認できます。<br>
     以下の手順で被疑箇所と思われる箇所を切り分けます。

<br>

1. 配信元の状況確認<br>
Azure CDN を介してコンテンツが取得できない場合は、配信元 (オリジン) に直接接続し、想定されるコンテンツが取得できるかをご確認ください。<br>
配信元からコンテンツが取得できない場合、配信元の影響により Azure CDN を介して取得できていない可能性があります。<br><br>

2. 配信元の証明書の状態を確認する<br>
Azure CDN はクライアントからの HTTPS 接続をそのまま配信元に転送するため、クライアントから配信元までの間で HTTPS 接続のための SSL 証明書の有効期限切れなどが発生した場合、エラーが応答される場合があります。<br>
証明書の状態については、以下のopenssl コマンドを用いて確認することが可能です。

```bash
[Azure CDN へのTLS 接続確認]
openssl s_client -connect <Azure CDN のエンドポイント (xxx..azureedge.net)>:443 -servername <FQDN> -showcerts

[バックエンドへの TLS 接続確認]
openssl s_client -connect <バックエンドのパブリック IP アドレスや FQDN>:443 -servername <FQDN> -showcerts
```
<br>

3. キャッシュ クリア (Purge)<br>
Azure CDN 上でキャッシュされたコンテンツが意図しない応答となりエラーとなっている可能性があります。<br>
一度、[キャッシュ削除 (Purge)](https://docs.microsoft.com/ja-jp/azure/cdn/cdn-purge-endpoint) を実施いただき、Azure CDN から配信元に再度接続するように試行いただき、エラー状況をご確認ください。

<br>
上記を実施いただき、事象が改善されない場合は、ご利用いただいている SKU に応じて情報取得をいただき、弊社サポート担当までお問い合わせください。
<br>

* [Standard Microsoft SKU をご利用いただいている場合](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-specific-sku.md#standard-microsoft-%E3%81%AB%E3%81%8A%E3%81%84%E3%81%A6%E3%82%A8%E3%83%A9%E3%83%BC%E7%99%BA%E7%94%9F%E6%99%82%E3%81%AB%E3%81%94%E6%8F%90%E4%BE%9B%E3%81%84%E3%81%9F%E3%81%A0%E3%81%8D%E3%81%9F%E3%81%84%E6%83%85%E5%A0%B1)
* [Standarad / Premium Verizon をご利用いただいている場合](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-specific-sku.md#standard-verizon--premium-verizon-%E3%81%AB%E3%81%8A%E3%81%84%E3%81%A6%E3%82%A8%E3%83%A9%E3%83%BC%E7%99%BA%E7%94%9F%E6%99%82%E3%81%AB%E3%81%94%E6%8F%90%E4%BE%9B%E3%81%84%E3%81%9F%E3%81%A0%E3%81%8D%E3%81%9F%E3%81%84%E6%83%85%E5%A0%B1)
* [Standard Akamai をご利用いただいている場合](https://github.com/taminta/blog/blob/patch-2/articles/network/cdn-specific-sku.md#standard-akamai-%E3%81%AB%E3%81%8A%E3%81%84%E3%81%A6%E3%82%A8%E3%83%A9%E3%83%BC%E7%99%BA%E7%94%9F%E6%99%82%E3%81%AB%E3%81%94%E6%8F%90%E4%BE%9B%E3%81%84%E3%81%9F%E3%81%A0%E3%81%8D%E3%81%9F%E3%81%84%E6%83%85%E5%A0%B1)

<br>

*※ 本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。*
