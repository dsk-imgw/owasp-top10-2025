<link rel="stylesheet" href="../../assets/css/RC-stylesheet.css" />

# セキュリティ設定のミス <img src="./assets/TOP_10_Icons_Final_Security_Misconfiguration.png" style="height:80px;width:80px" align="right">


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

Secure installation processes should be implemented, including:



* A repeatable hardening process enabling the fast and easy deployment of another environment that is appropriately locked down. Development, QA, and production environments should all be configured identically, with different credentials used in each environment. This process should be automated to minimize the effort required to set up a new secure environment.
* A minimal platform without any unnecessary features, components, documentation, or samples. Remove or do not install unused features and frameworks.
* A task to review and update the configurations appropriate to all security notes, updates, and patches as part of the patch management process (see [A03:2025-](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)Software Supply Chain Failures). Review cloud storage permissions (e.g., S3 bucket permissions).
* A segmented application architecture provides effective and secure separation between components or tenants, with segmentation, containerization, or cloud security groups (ACLs).
* Sending security directives to clients, e.g., Security Headers.
* An automated process to verify the effectiveness of the configurations and settings in all environments.
* Proactively add a central configuration to intercept excessive error messages as a backup.
* If these varifications are not automated, they should be manually verified annually at a minimum.
 

## 攻撃シナリオの例

**シナリオ #1:** The application server comes with sample applications not removed from the production server. These sample applications have known security flaws that attackers use to compromise the server. Suppose one of these applications is the admin console, and default accounts weren't changed. In that case, the attacker logs in with the default password and takes over.

**シナリオ #2:** Directory listing is not disabled on the server. An attacker discovers they can simply list directories. The attacker finds and downloads the compiled Java classes, which they decompile and reverse engineer to view the code. The attacker then finds a severe access control flaw in the application.

**シナリオ #3:** The application server's configuration allows detailed error messages, such as stack traces to be returned to users. This potentially exposes sensitive information or underlying flaws, such as component versions that are known to be vulnerable.

**シナリオ #4:** A cloud service provider (CSP) defaults to having sharing permissions open to the Internet. This allows sensitive data stored within cloud storage to be accessed.


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