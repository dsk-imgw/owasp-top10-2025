<link rel="stylesheet" href="../../assets/css/RC-stylesheet.css" />

# A02:2025 セキュリティ設定のミス <img src="./assets/TOP_10_Icons_Final_Security_Misconfiguration.png" style="height:80px;width:80px" align="right">


## 背景

前回の 5 位からランク アップし、テスト対象アプリケーションの 100% に何らかの設定ミスが見つかりました。平均発生率は 3.00% で、このリスク カテゴリにおける共通脆弱性リスト（CWE）の出現件数は 71 万 9 千件を超えています。高度に構成可能なソフトウェアへの移行が進む中、このカテゴリのランクが上がるのは当然のことです。注目すべき CWE としては、*CWE-16 設定* と *CWE-611 XML 外部実体参照（XXE）の不適切な制限* が挙げられます。

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
   <td>16
   </td>
   <td>27.70%
   </td>
   <td>3.00%
   </td>
   <td>100.00%
   </td>
   <td>52.35%
   </td>
   <td>7.96
   </td>
   <td>3.97
   </td>
   <td>719,084
   </td>
   <td>1,375
   </td>
  </tr>
</table>



## 説明

セキュリティ設定のミスとは、システム、アプリケーション、またはクラウド サービスがセキュリティの観点から不適切に設定され、脆弱性が生じることです。

アプリケーションが脆弱になる可能性があるのは、以下のような場合です。

* アプリケーション スタックのどこかに適切なセキュリティ強化が施されていません。またはクラウド サービスに対する権限が適切に構成されていません。
* 不要な機能（不要なポート・サービス・ページ・アカウント、テスト用フレームワーク・権限など）が有効化またはインストールされています。
* デフォルトのアカウントとそのパスワードが有効化され、変更されていません。
* 過剰なエラー メッセージを遮断するための集中管理設定が不足しています。エラー処理によって、スタック トレースやその他の過剰な情報を含むエラー メッセージがユーザーに公開されます。
* アップグレードされたシステムで、最新のセキュリティ機能が無効化されているか、セキュアに構成されていません。
* 後方互換性を過度に優先し、セキュアでない構成になっています。
* アプリケーション サーバー、アプリケーション フレームワーク（Struts、Spring、ASP.NETなど）、ライブラリ、データベースなどのセキュリティ設定がセキュアな値に設定されていません。
* サーバーがセキュリティ ヘッダーまたはセキュリティ ディレクティブを送信しないか、それらがセキュアな値に設定されていません。

協調的かつ反復可能なアプリケーション セキュリティ設定の強化プロセスがなければ、システムはより高いリスクにさらされます。

## 防止方法

以下の項目を含む、セキュアなインストール プロセスを実装する必要があります。

* 適切にロック ダウンされた別の環境を迅速かつ容易にデプロイできる繰り返し可能な強化プロセス。開発環境、QA 環境、本番環境はすべて同一構成とし、各環境で異なる認証情報を使用する必要があります。このプロセスは自動化することで、新しいセキュアな環境の構築に必要な労力を最小限に抑えることができます。
* 不要な機能・コンポーネント・文書・サンプルを含まない最小限のプラットフォーム。使用しない機能やフレームワークは削除するか、インストールしてはなりません。
* パッチ管理プロセスの一環として、すべてのセキュリティ ノート、アップデート、パッチに適切な構成をレビューし、更新するタスク（[A03:2025-ソフトウェア サプライチェーンの失敗](A03_2025-Software_Supply_Chain_Failures.md) を参照）。クラウド ストレージの権限（S3 バケットの権限など）をレビューします。
* セグメント化されたアプリケーション アーキテクチャは、セグメンテーション、コンテナ化、またはクラウド セキュリティ グループ（ACL）によって、コンポーネントまたはテナント間の効果的かつセキュアな分離を実現します。
* セキュリティ ヘッダーなどのセキュリティ ディレクティブをクライアントに送信します。
* すべての環境における構成と設定の有効性を検証する自動プロセス。
* 過剰なエラーメッセージをバックアップとして傍受するための中央構成を事前対応的に追加します。
* これらの検証が自動化されていない場合は、少なくとも年に 1 回は手動で検証する必要があります。

## 攻撃シナリオの例

**シナリオ #1:** アプリケーション サーバーには、本番サーバーから削除されていないサンプル アプリケーションが付属しています。これらのサンプル アプリケーションには、攻撃者がサーバーへの侵入に利用する既知のセキュリティ上の欠陥があります。これらのアプリケーションの 1 つが管理コンソールで、デフォルトのアカウントが変更されていないと仮定します。この場合、攻撃者はデフォルトのパスワードでログインし、サーバーを乗っ取ることができます。

**シナリオ #2:** サーバー上でディレクトリ一覧表示が無効化されていません。攻撃者は、ディレクトリ一覧表示が簡単にできることを発見します。攻撃者は、コンパイル済みの Java クラスを見つけてダウンロードし、逆コンパイルしてリバース エンジニアリングを行い、コードを確認します。そして、アプリケーションに重大なアクセス制御の欠陥があることを発見します。

**シナリオ #3:** アプリケーション サーバーの設定により、スタック トレースなどの詳細なエラー メッセージをユーザーに返すことが可能になっています。これにより、脆弱性が知られているコンポーネントのバージョンなど、機密情報や潜在的な欠陥が明らかになる可能性があります。

**シナリオ #4:** クラウド サービス プロバイダー (CSP) は、デフォルトで共有権限をインターネットに公開しています。これにより、クラウド ストレージに保存されている機密データにアクセスできるようになります。


## 参考情報

* OWASP Testing Guide: Configuration Management
* OWASP Testing Guide: Testing for Error Codes
* Application Security Verification Standard 5.0.0
* NIST Guide to General Server Hardening
* CIS Security Configuration Guides/Benchmarks
* Amazon S3 Bucket Discovery and Enumeration
* ScienceDirect: Security Misconfiguration


## 対応する CWE の一覧

* [CWE-5 J2EE Misconfiguration: Data Transmission Without Encryption](https://cwe.mitre.org/data/definitions/5.html)
* [CWE-11 ASP.NET Misconfiguration: Creating Debug Binary](https://cwe.mitre.org/data/definitions/11.html)
* [CWE-13 ASP.NET Misconfiguration: Password in Configuration File](https://cwe.mitre.org/data/definitions/13.html)
* [CWE-15 External Control of System or Configuration Setting](https://cwe.mitre.org/data/definitions/15.html)
* [CWE-16 Configuration](https://cwe.mitre.org/data/definitions/16.html)
* [CWE-260 Password in Configuration File](https://cwe.mitre.org/data/definitions/260.html)
* [CWE-315 Cleartext Storage of Sensitive Information in a Cookie](https://cwe.mitre.org/data/definitions/315.html)
* [CWE-489 Active Debug Code](https://cwe.mitre.org/data/definitions/489.html)
* [CWE-526 Exposure of Sensitive Information Through Environmental Variables](https://cwe.mitre.org/data/definitions/526.html)
* [CWE-547 Use of Hard-coded, Security-relevant Constants](https://cwe.mitre.org/data/definitions/547.html)
* [CWE-611 Improper Restriction of XML External Entity Reference](https://cwe.mitre.org/data/definitions/611.html)
* [CWE-614 Sensitive Cookie in HTTPS Session Without 'Secure' Attribute](https://cwe.mitre.org/data/definitions/614.html)
* [CWE-776 Improper Restriction of Recursive Entity References in DTDs ('XML Entity Expansion')](https://cwe.mitre.org/data/definitions/776.html)
* [CWE-942 Permissive Cross-domain Policy with Untrusted Domains](https://cwe.mitre.org/data/definitions/942.html)
* [CWE-1004 Sensitive Cookie Without 'HttpOnly' Flag](https://cwe.mitre.org/data/definitions/1004.html)

* [CWE-1174 ASP.NET Misconfiguration: Improper Model Validation](https://cwe.mitre.org/data/definitions/1174.html)
