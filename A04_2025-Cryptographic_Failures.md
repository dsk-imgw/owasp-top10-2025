<link rel="stylesheet" href="../../assets/css/RC-stylesheet.css" />

# A04:2025 暗号化の失敗 <img src="./assets/TOP_10_Icons_Final_Crypto_Failures.png" style="height:80px;width:80px" align="right">


## 背景


2 つ順位を下げて 4 位となったこの脆弱性は、暗号化の欠如、暗号化の強度不足、暗号鍵の漏洩、および関連するエラーに関連する不具合に焦点を当てています。このリスクにおける最も一般的な共通脆弱性リスト（CWE）のうち、脆弱な疑似乱数生成器の使用に関連するものは 3 つあります。*CWE-327 破損または危険な暗号化アルゴリズムの使用*、*CWE-331: 不十分なエントロピー*、*CWE-1241: 乱数生成器における予測可能なアルゴリズムの使用*、および *CWE-338 暗号的に脆弱な疑似乱数生成器（PRNG）の使用*です。

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
   <td>32
   </td>
   <td>13.77%
   </td>
   <td>3.80%
   </td>
   <td>100.00%
   </td>
   <td>47.74%
   </td>
   <td>7.23
   </td>
   <td>3.90
   </td>
   <td>1,665,348
   </td>
   <td>2,185
   </td>
  </tr>
</table>



## 説明

一般的に、転送中のすべてのデータは[トランスポート層](https://en.wikipedia.org/wiki/Transport_layer)（[OSI 層](https://en.wikipedia.org/wiki/OSI_model) 4）で暗号化する必要があります。CPU パフォーマンスや秘密鍵／証明書管理といった従来の課題は、暗号化を高速化するように設計された命令（例：[AES サポート](https://en.wikipedia.org/wiki/AES_instruction_set)）を備えた CPU によって解決され、秘密鍵と証明書の管理は [LetsEncrypt.org](https://LetsEncrypt.org) などのサービスによって簡素化されています。大手クラウド ベンダーは、それぞれのプラットフォーム向けに、より緊密に統合された証明書管理サービスを提供しています。

トランスポート層のセキュリティ確保に加えて、転送中（[アプリケーション層](https://en.wikipedia.org/wiki/Application_layer)、OSI 層 7）に追加の暗号化が必要なデータは何か？だけでなく、保存時に暗号化が必要なデータは何か？を判断することが重要です。例えば、パスワード、クレジット カード番号、健康記録、個人情報、企業秘密などは、EU の一般データ保護規則（GDPR）などのプライバシー法や PCI データ セキュリティ基準（PCI DSS）などの規制の対象となる場合は、特に、特別な保護が必要です。これらのデータはすべて、以下の点に注意する必要があります。

* デフォルトまたは古いコードで、古いあるいは脆弱な暗号アルゴリズムやプロトコルが使用されていますか？
* デフォルトの暗号鍵が使用されていますか？弱い暗号鍵が生成されていますか？鍵が再利用されていますか？あるいは、適切な鍵管理と変更が行われていませんか？
* 暗号鍵はソース コード リポジトリにチェックインされていますか？
* 暗号化が強制されていませんか？例えば、HTTP ヘッダー（ブラウザ）のセキュリティ ディレクティブやセキュリティ ヘッダーが欠落していませんか？
* 受信したサーバー証明書と信頼チェーンは適切に検証されていますか？
* 初期化ベクトルを無視したり再利用していますか？あるいは、暗号動作モードに対して十分にセキュアな方法で生成されていませんか？ECB などの安全でない動作モードが使用されていますか？認証付き暗号化の方が適切な場合に暗号化が使用されていますか？
* パスワード ベースの鍵導出関数がないにもかかわらず、パスワードが暗号鍵として使用されていますか？
* 暗号要件を満たすように設計されていないランダム性が暗号目的で使用されていますか？正しい関数が選択されている場合でも、開発者がシードを設定する必要がありますか？必要でない場合、開発者は、組み込みの強力なシード機能を、十分なエントロピー/予測不可能性に欠けるシードで上書きしていませんか？
* MD5 や SHA1 などの非推奨のハッシュ関数が使用されていますか？あるいは、暗号ハッシュ関数が必要なのに非暗号ハッシュ関数が使用されていますか？
* PKCS #1 v1.5 などの非推奨の暗号パディング方式が使用されていますか？
* 暗号エラー メッセージやサイド チャネル情報は、例えばパディング オラクル攻撃などの形で悪用される可能性がありますか？
* 暗号アルゴリズムはダウングレードまたはバイパスされる可能性がありますか？

参考情報「ASVS: 暗号化 (V11)、安全な通信 (V12)、およびデータ保護 (V14)」を参照してください。


## 防止方法

少なくとも以下を実施し、参考情報を参照します。

* アプリケーションによって処理、保存、または伝送されるデータを分類し、ラベル付けします。プライバシー法、規制要件、またはビジネス ニーズに応じて、機密性の高いデータを特定します。
* 最も機密性の高い鍵は、ハードウェアまたはクラウド ベースの HSM に保存します。
* 可能な限り、信頼性の高い暗号化アルゴリズムの実装を使用します。
* 機密データを不必要に保存してはなりません。できるだけ早く破棄するか、PCI DSS に準拠したトークン化、あるいはトランケーションを使用してください。保持されていないデータは盗難されることはありません。
* 保存中のすべての機密データは必ず暗号化します。
* 最新かつ強力な標準アルゴリズム、プロトコル、および鍵が確実に導入され、適切な鍵管理が採用されていることを確実にします。
* 伝送中のすべてのデータは、前方秘匿性 (FS) 暗号、サーバーによる暗号の優先順位付け、およびセキュアなパラメータを使用した TLS などのセキュアなプロトコルを使用して暗号化します。HTTP Strict Transport Security (HSTS) などのディレクティブを使用して暗号化を強制します。
* 機密データを含む応答のキャッシュを無効にします。これには、CDN、Web サーバー、およびあらゆるアプリケーション キャッシュ (例: Redis) におけるキャッシュが含まれます。
* データ分類に応じて必要なセキュリテ管理策を適用します。
* FTP や SMTP などの暗号化されていないプロトコルは使用してはなりません。
* パスワードは、Argon2、scrypt、bcrypt (レガシー システム)、PBKDF2-HMAC-SHA-256 など、ワーク ファクター (遅延係数) を持つ強力な適応型ソルト付きハッシュ関数を使用して保存します。bcrypt を使用しているレガシー システムについては、[OWASP OWASP Cheat Sheet: Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) で詳細なアドバイスを参照してます。
* 初期化ベクトルは、動作モードに適したものを選択する必要があります。これには、CSPRNG (暗号的にセキュアな擬似乱数生成器) の使用が含まれます。ナンスを必要とするモードでは、初期化ベクトル (IV) に CSPRNG は必要ありません。いかなる場合でも、固定キーに対して IV を 2 回使用してはなりません。
* 単なる暗号化ではなく、常に認証付き暗号化を使用します。
* 鍵は暗号学的にランダムに生成し、バイト配列としてメモリに保存する必要があります。パスワードを使用する場合は、適切なパスワード ベースの鍵導出関数を使用して鍵に変換する必要があります。
* 暗号学的ランダム性が適切な場合に使用され、予測可能な方法や低いエントロピーでシードされていないことを確実にします。ほとんどの最新の API では、開発者が CSPRNG のシードをセキュアに使用する必要がありません。
* MD5、SHA1、暗号ブロック連鎖モード（CBC）、PKCS #1 v1.5などといった非推奨の暗号化関数、ブロック構築方法、パディング スキームは使用してはなりません。
* 設定と構成がセキュリティ要件を満たしていることを確実にするために、セキュリティ専門家、この目的のために設計されたツール、またはその両方によるレビューを受けます。
* 耐量子暗号（PQC）への対応を検討します。詳細については、参考資料（ENISA、NIST）を参照します。



## 攻撃シナリオの例


**シナリオ #1**: サイトがすべてのページで TLS を使用または強制していないか、弱い暗号化をサポートしています。攻撃者はネットワーク トラフィック（例: セキュアでない無線ネットワーク）を監視し、接続を HTTPS から HTTP にダウングレードし、リクエストを傍受してユーザーのセッション Cookie を盗みます。その後、攻撃者はこの Cookie をリプレイし、ユーザーの（認証済み）セッションを乗っ取り、ユーザーの個人データにアクセスまたは変更します。上記の他に、送金の受取者など、伝送されるすべてのデータを改ざんすることも可能です。

**シナリオ #2**: パスワード データベースでは、ソルトなしまたは単純なハッシュを使用してすべてのパスワードが保存されています。ファイル アップロードの脆弱性により、攻撃者はパスワード データベースを取得できます。ソルトなしのハッシュはすべて、事前に計算されたハッシュのレインボー テーブルによって意図せず公開される可能性があります。単純または高速なハッシュ関数によって生成されたハッシュは、ソルト付きであっても GPU によって解読される可能性があります。

## 参考情報



* [OWASP Proactive Controls: C2: Use Cryptography to Protect Data ](https://top10proactive.owasp.org/archive/2024/the-top-10/c2-crypto/)
* [OWASP Application Security Verification Standard (ASVS): ](https://owasp.org/www-project-application-security-verification-standard) [V11,](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x20-V11-Cryptography.md) [12, ](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x21-V12-Secure-Communication.md) [14](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x23-V14-Data-Protection.md)
* [OWASP Cheat Sheet: Transport Layer Protection](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html)
* [OWASP Cheat Sheet: User Privacy Protection](https://cheatsheetseries.owasp.org/cheatsheets/User_Privacy_Protection_Cheat_Sheet.html)
* [OWASP Cheat Sheet: Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
* [OWASP Cheat Sheet: Cryptographic Storage](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
* [OWASP Cheat Sheet: HSTS](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)
* [OWASP Testing Guide: Testing for weak cryptography](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/README)
* [ENISA: A Coordinated Implementation Roadmap for the Transition to Post-Quantum Cryptography](https://digital-strategy.ec.europa.eu/en/library/coordinated-implementation-roadmap-transition-post-quantum-cryptography)
* [NIST Releases First 3 Finalized Post-Quantum Encryption Standards](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards)


## 対応する CWE の一覧

* [CWE-261 Weak Encoding for Password](https://cwe.mitre.org/data/definitions/261.html)
* [CWE-296 Improper Following of a Certificate's Chain of Trust](https://cwe.mitre.org/data/definitions/296.html)
* [CWE-319 Cleartext Transmission of Sensitive Information](https://cwe.mitre.org/data/definitions/319.html)
* [CWE-320 Key Management Errors (Prohibited)](https://cwe.mitre.org/data/definitions/320.html)
* [CWE-321 Use of Hard-coded Cryptographic Key](https://cwe.mitre.org/data/definitions/321.html)
* [CWE-322 Key Exchange without Entity Authentication](https://cwe.mitre.org/data/definitions/322.html)
* [CWE-323 Reusing a Nonce, Key Pair in Encryption](https://cwe.mitre.org/data/definitions/323.html)
* [CWE-324 Use of a Key Past its Expiration Date](https://cwe.mitre.org/data/definitions/324.html)
* [CWE-325 Missing Required Cryptographic Step](https://cwe.mitre.org/data/definitions/325.html)
* [CWE-326 Inadequate Encryption Strength](https://cwe.mitre.org/data/definitions/326.html)
* [CWE-327 Use of a Broken or Risky Cryptographic Algorithm](https://cwe.mitre.org/data/definitions/327.html)
* [CWE-328 Reversible One-Way Hash](https://cwe.mitre.org/data/definitions/328.html)
* [CWE-329 Not Using a Random IV with CBC Mode](https://cwe.mitre.org/data/definitions/329.html)
* [CWE-330 Use of Insufficiently Random Values](https://cwe.mitre.org/data/definitions/330.html)
* [CWE-331 Insufficient Entropy](https://cwe.mitre.org/data/definitions/331.html)
* [CWE-332 Insufficient Entropy in PRNG](https://cwe.mitre.org/data/definitions/332.html)
* [CWE-334 Small Space of Random Values](https://cwe.mitre.org/data/definitions/334.html)
* [CWE-335 Incorrect Usage of Seeds in Pseudo-Random Number Generator(PRNG)](https://cwe.mitre.org/data/definitions/335.html)
* [CWE-336 Same Seed in Pseudo-Random Number Generator (PRNG)](https://cwe.mitre.org/data/definitions/336.html)
* [CWE-337 Predictable Seed in Pseudo-Random Number Generator (PRNG)](https://cwe.mitre.org/data/definitions/337.html)
* [CWE-338 Use of Cryptographically Weak Pseudo-Random Number Generator(PRNG)](https://cwe.mitre.org/data/definitions/338.html)
* [CWE-340 Generation of Predictable Numbers or Identifiers](https://cwe.mitre.org/data/definitions/340.html)
* [CWE-342 Predictable Exact Value from Previous Values](https://cwe.mitre.org/data/definitions/342.html)
* [CWE-347 Improper Verification of Cryptographic Signature](https://cwe.mitre.org/data/definitions/347.html)
* [CWE-523 Unprotected Transport of Credentials](https://cwe.mitre.org/data/definitions/523.html)
* [CWE-757 Selection of Less-Secure Algorithm During Negotiation('Algorithm Downgrade')](https://cwe.mitre.org/data/definitions/757.html)
* [CWE-759 Use of a One-Way Hash without a Salt](https://cwe.mitre.org/data/definitions/759.html)
* [CWE-760 Use of a One-Way Hash with a Predictable Salt](https://cwe.mitre.org/data/definitions/760.html)
* [CWE-780 Use of RSA Algorithm without OAEP](https://cwe.mitre.org/data/definitions/780.html)
* [CWE-916 Use of Password Hash With Insufficient Computational Effort](https://cwe.mitre.org/data/definitions/916.html)
* [CWE-1240 Use of a Cryptographic Primitive with a Risky Implementation](https://cwe.mitre.org/data/definitions/1240.html)
* [CWE-1241 Use of Predictable Algorithm in Random Number Generator](https://cwe.mitre.org/data/definitions/1241.html)