<link rel="stylesheet" href="./assets/css/RC-stylesheet.css" />

# A08:2025 ソフトウェアまたはデータの整合性の不具合 <img src="./assets/TOP_10_Icons_Final_Software_and_Data_Integrity_Failures.png" style="height:80px;width:80px" align="right">

## 背景

ソフトウェアまたはデータの整合性の不具合は、引き続き 8 位にランクインしています。「ソフトウェア *および* データの整合性の不具合」という名称から、やや分かりやすく名称が変更されています。このカテゴリは、[A03:2025-ソフトウェア サプライチェーン](A03_2025-Software_Supply_Chain_Failures.md) の失敗よりも低いレベルで、信頼境界の維持とソフトウェア、コード、およびデータ アーティファクトの整合性検証の失敗に焦点を当てています。このカテゴリは、整合性を検証することなく、ソフトウェアの更新や重要なデータに関する憶測を行うことに焦点を当てています。注目すべき共通脆弱性リスト（CWE）には、*CWE-829: 信頼されていない制御範囲からの機能の取り込み*、*CWE-915: 動的に決定されたオブジェクト属性の不適切な制御による変更*、*CWE-502: 信頼されていないデータの逆シリアル化* などがあります。

## スコア表


<table>
  <tr>
   <td>対応する CWE 数
   </td>
   <td>インシデント割合（最大）
   </td>
   <td>インシデント割合（平均）
   </td>
   <td>カバレッジ（最大）
   </td>
   <td>カバレッジ（平均）
   </td>
   <td>重み付き悪用可能性（最大）
   </td>
   <td>重み付き悪用可能性（平均）
   </td>
   <td>総発生件数
   </td>
   <td>総 CVE 数
   </td>
  </tr>
  <tr>
   <td>14
   </td>
   <td>8.98%
   </td>
   <td>2.75%
   </td>
   <td>78.52%
   </td>
   <td>45.49%
   </td>
   <td>7.11
   </td>
   <td>4.79
   </td>
   <td>501,327
   </td>
   <td>3,331
   </td>
  </tr>
</table>



## 説明

ソフトウェアまたはデータの整合性の不具合は、無効または信頼できないコードやデータが信頼できる有効なものとして扱われることに対する保護対策を講じていないコードやインフラストラクチャに起因します。例えば、アプリケーションが、信頼できないソース、リポジトリ、コンテンツ配信ネットワーク（CDN）からのプラグイン、ライブラリ、またはモジュールに依存している場合などが挙げられます。ソフトウェアの整合性チェックを利用・提供しないセキュアでない CI/CDパイプラインは、不正アクセス、セキュアでないコードや悪意のあるコード、あるいはシステム侵害の危険性をはらんでいます。もう 1 つの例として、信頼できない場所からコードやアーティファクトをプルしたり、使用前に検証（署名の確認など）を行わない CI/CD が挙げられます。さらに、多くのアプリケーションには自動更新機能が搭載されており、十分な整合性検証を行わずに更新がダウンロードされ、以前は信頼されていたアプリケーションに適用されます。攻撃者は、独自の更新をアップロードし、それをすべてのインストール環境で配布・実行させる可能性があります。また、オブジェクトやデータが攻撃者が参照・変更できる構造にエンコードまたはシリアル化されている場合セキュアでない逆シリアル化に対して脆弱です。

## 防止方法

* デジタル署名または同様の仕組みを使用して、ソフトウェアまたはデータが期待されるソースからのものであり、変更されていないことを検証します。
* npm や Maven などのライブラリと依存関係が、信頼できるリポジトリのみを使用していることを確実にします。リスク プロファイルが高い場合は、内部で検証済みで信頼性のあるリポジトリをホストすることを検討します。
* 悪意のあるコードや構成がソフトウェア パイプラインに持ち込まれる可能性を最小限に抑えるため、コードと構成の変更に対するレビュー プロセスが確立されていることを確実にします。
* CI/CD パイプラインに適切な分離、構成、アクセス制御が備わっていることを確実にし、ビルドおよびデプロイ プロセスを流れるコードの整合性を確保します。
* 署名なしまたは暗号化されていないシリアル化データを信頼できないクライアントから受信していないことを確実にし、シリアル化データの改ざんやリプレイを検出するための何らかの整合性チェックやデジタル署名が行われずに使用されないようにします。

## 攻撃シナリオの例

**シナリオ #1 信頼できないソースの Web 機能のの取り込み:** ある企業は、サポート機能を提供するために外部サービス プロバイダーを利用しています。便宜上、`myCompany.SupportProvider.com` から `support.myCompany.com` への DNS マッピングが設定されています。つまり、`myCompany.com` ドメインに設定されたすべての Cookie（認証 Cookie を含む）がサポート プロバイダーに送信されることになります。サポート プロバイダーのインフラストラクチャにアクセスできるユーザーは誰でも、`support.myCompany.com` にアクセスしたすべてのユーザーの Cookie を盗み、セッション ハイジャック攻撃を実行することができます。

**シナリオ #2-1: 署名なしでの更新:** 多くの家庭用ルーター、セットトップ ボックス、デバイスのファームウェアなどは、署名されたファームウェアによるアップデートの検証を行っていません。署名のないファームウェアは攻撃者の標的として増加しており、今後さらに悪化すると予想されています。多くの場合、将来のバージョンで修正され以前のバージョンが期限切れになるまで待つ以外に対策手段がないため、これは大きな懸念事項です。

**シナリオ #2-2:** 開発者は、探しているパッケージの更新バージョンを見つけるのに苦労し、通常の信頼できるパッケージ マネージャーではなく、オンラインのウェブサイトからダウンロードします。パッケージは署名されていないため、整合性を確認する機会がありません。パッケージには悪意のあるコードが含まれています。

**シナリオ #3 セキュアでない逆シリアル化:** React アプリケーションは、Spring Boot マイクロサービス群を呼び出します。関数型プログラマーである開発者は、コードの不変性を確保しようと試みました。そこで開発者が思いついた解決策は、ユーザー状態をシリアル化し、リクエストごとにそれをやり取りすることでした。攻撃者は「rO0」という Java オブジェクト シグネチャ（base64形式）に気づき、[Java Deserialization Scanner](https://github.com/federicodotta/Java-Deserialization-Scanner) を使用して、アプリケーション サーバー上でリモート コード実行権限を取得します。

## 参考情報

* [OWASP Cheat Sheet: Software Supply Chain Security](https://cheatsheetseries.owasp.org/cheatsheets/Software_Supply_Chain_Security_Cheat_Sheet.html)
* [OWASP Cheat Sheet: Infrastructure as Code](https://cheatsheetseries.owasp.org/cheatsheets/Infrastructure_as_Code_Security_Cheat_Sheet.html)
* [OWASP Cheat Sheet: Deserialization](https://wiki.owasp.org/index.php/Deserialization_Cheat_Sheet)
* [SAFECode Software Integrity Controls](https://safecode.org/publication/SAFECode_Software_Integrity_Controls0610.pdf)
* [A 'Worst Nightmare' Cyberattack: The Untold Story Of The SolarWinds Hack](https://www.npr.org/2021/04/16/985439655/a-worst-nightmare-cyberattack-the-untold-story-of-the-solarwinds-hack)
* [CodeCov Bash Uploader Compromise](https://about.codecov.io/security-update)
* [Securing DevOps by Julien Vehent](https://www.manning.com/books/securing-devops)
* [Insecure Deserialization by Tenendo](https://tenendo.com/insecure-deserialization/)


## 対応する CWE の一覧

* [CWE-345 Insufficient Verification of Data Authenticity](https://cwe.mitre.org/data/definitions/345.html)
* [CWE-353 Missing Support for Integrity Check](https://cwe.mitre.org/data/definitions/353.html)
* [CWE-426 Untrusted Search Path](https://cwe.mitre.org/data/definitions/426.html)
* [CWE-427 Uncontrolled Search Path Element](https://cwe.mitre.org/data/definitions/427.html)
* [CWE-494 Download of Code Without Integrity Check](https://cwe.mitre.org/data/definitions/494.html)
* [CWE-502 Deserialization of Untrusted Data](https://cwe.mitre.org/data/definitions/502.html)
* [CWE-506 Embedded Malicious Code](https://cwe.mitre.org/data/definitions/506.html)
* [CWE-509 Replicating Malicious Code (Virus or Worm)](https://cwe.mitre.org/data/definitions/509.html)
* [CWE-565 Reliance on Cookies without Validation and Integrity Checking](https://cwe.mitre.org/data/definitions/565.html)
* [CWE-784 Reliance on Cookies without Validation and Integrity Checking in a Security Decision](https://cwe.mitre.org/data/definitions/784.html)
* [CWE-829 Inclusion of Functionality from Untrusted Control Sphere](https://cwe.mitre.org/data/definitions/829.html)
* [CWE-830 Inclusion of Web Functionality from an Untrusted Source](https://cwe.mitre.org/data/definitions/830.html)
* [CWE-915 Improperly Controlled Modification of Dynamically-Determined Object Attributes](https://cwe.mitre.org/data/definitions/915.html)
* [CWE-926 Improper Export of Android Application Components](https://cwe.mitre.org/data/definitions/926.html)