<link rel="stylesheet" href="./assets/css/RC-stylesheet.css" />

# A06:2025 セキュリティが確保されていない設計 <img src="./assets/TOP_10_Icons_Final_Insecure_Design.png" style="height:80px;width:80px" align="right">


## 背景

セキュリティが確保されていない設計は、**[A02:2025-セキュリティ設定のミス](A02_2025-Security_Misconfiguration.md)** と **[A03:2025-ソフトウェア サプライチェーンの失敗](A03_2025-Software_Supply_Chain_Failures.md)** に追い抜かれたため、ランキングで 4 位から 6 位に 2 つ順位を落としました。このカテゴリは 2021 年に導入され、脅威モデリングとセキュアな設計の重視に関して、業界では目立った改善が見られました。このカテゴリは、設計とアーキテクチャの欠陥に関連するリスクに焦点を当てており、脅威モデリング、セキュアな設計パターン、およびリファレンス アーキテクチャのより多くの使用を求めています。これには、アプリケーションのビジネス ロジックの欠陥、例えば、アプリケーション内の不要または予期しない状態変更の定義の欠如が含まれます。コミュニティとして、コーディング領域における「シフト レフト」を超えて、要件定義やアプリケーション設計といった、セキュア・バイ・デザインの原則に不可欠な事前コーディング活動へと進む必要があります（例: **[現代的なアプリケーション セキュリティ プログラムの確立: 計画・設計フェーズ](0x03_2025-Establishing_a_Modern_Application_Security_Program.md)** を参照）。注目すべき共通脆弱性リスト（CWE）には、*CWE-256: 保護されていない資格情報の保管*、*CWE-269: 不適切な権限管理*、*CWE-434: 危険な種類のファイルの無制限のアップロード*、*CWE-501: 信頼境界違反*、および *CWE-522: 十分に保護されていない資格情報* が含まれます。
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
   <td>39
   </td>
   <td>22.18%
   </td>
   <td>1.86%
   </td>
   <td>88.76%
   </td>
   <td>35.18%
   </td>
   <td>6.96
   </td>
   <td>4.05
   </td>
   <td>729,882
   </td>
   <td>7,647
   </td>
  </tr>
</table>



## 説明


セキュリティが確保されていない設計は、様々な弱点を表す広範なカテゴリーであり、「管理策設計の欠如、または効果のない管理策設計」と表現されます。セキュリティが確保されていない設計は、他の Top 10 リスク カテゴリすべてに当てはまるわけではありません。セキュリティが確保されていない設計とセキュリティが確保されていない実装には違いがあることに注意してください。設計上の欠陥と実装上の欠陥を区別するのには理由があります。それらは根本原因が異なり、開発プロセスの異なる時期に発生し、修正方法も異なるからです。セキュリティが確保された設計であっても、実装上の欠陥が悪用される可能性のある脆弱性につながる可能性があります。セキュリティが確保されていない設計は、特定の攻撃に対する防御のために必要なセキュリティ管理策が策定されていないため、完璧な実装によって修正することはできません。セキュリティが確保されていない設計につながる要因の 1 つは、開発中のソフトウェアまたはシステムに内在するビジネス リスク プロファイリングの欠如であり、その結果、必要なセキュリティ設計レベルを判断できないことです。

セキュリティが確保された設計を実現するための 3 つの重要な要素は以下のとおりです。

* 要件の収集とリソース管理
* セキュリティが確保された設計の作成
* セキュアな開発ライフサイクルの確立


### 要件とリソース管理

アプリケーションのビジネス要件を収集し、ビジネス部門と交渉します。これには、すべてのデータ資産と想定されるビジネス ロジックの機密性、完全性、可用性、真正性に関する保護要件が含まれます。アプリケーションの公開範囲と、（アクセス制御に必要な範囲を超えて）テナントの分離が必要かどうかを考慮します。機能的および非機能的なセキュリティ要件を含む技術要件をまとめます。セキュリティ活動を含む、すべての設計、構築、テスト、運用をカバーする予算を計画し、交渉します。

### セキュリティが確保された設計

セキュリティが確保された設計とは、脅威を継続的に評価し、既知の攻撃方法を防ぐためにコードが堅牢に設計・テストされていることを保証する文化と方法論です。脅威モデリングは、精緻化セッション（または類似の活動）に統合し、データフロー、アクセス制御、その他のセキュリティ管理策における変更点を探る必要があります。ユーザー ストーリーの開発では、正しいフローと障害状態を特定し、責任者と影響を受ける関係者がそれらを十分に理解し、合意していることを確認します。想定されるフローと失敗フローの前提条件と条件を分析し、それらが正確かつ望ましい状態であることを確実にします。前提条件を検証し、適切な動作に必要な条件を適用する方法を決定します。結果がユーザー ストーリーに文書化されていることを確実にします。失敗から学び、改善を促進するための積極的なインセンティブを提供します。セキュリティが確保された設計は、ソフトウェアに追加できるアドオンでもツールでもありません。

### セキュアな開発ライフサイクル

セキュアなソフトウェアには、セキュアな開発ライフサイクル、セキュアな設計パターン、確立された方法論、セキュアなコンポーネント ライブラリ、適切なツール、脅威モデリング、そしてプロセス改善に活用されるインシデント事後分析が必要です。ソフトウェア プロジェクトの開始時、プロジェクト全体、そして継続的なソフトウェア維持において、セキュリティ専門家に相談します。セキュアなソフトウェア開発の取り組みを構築するには、[OWASP Software Assurance Maturity Model (SAMM)](https://owaspsamm.org/) の活用を検討します。

## 防止方法

* アプリケーション セキュリティ専門家と連携し、セキュリティとプライバシー関連の管理策の評価と設計を支援するセキュアな開発ライフサイクルを確立・運用します。
* セキュアな設計パターンまたは既成コンポーネントのライブラリを確立・運用します。
* 認証、アクセス制御、ビジネス ロジック、主要なフローなど、アプリケーションの重要な部分に脅威モデリングを適用します。
* セキュリティ言語と管理策をユーザー ストーリーに統合します。
* アプリケーションの各層（フロント エンドからバック エンドまで）に妥当性チェックを統合します。
* すべての重要なフローが脅威モデルに耐性があることを検証するための単体テストと統合テストを作成します。アプリケーションの各層の利用事例と誤用ケースをまとめます。
* リスクと保護のニーズに応じて、システム層とネットワーク層を分離します。
* すべての層を通して、テナントを設計によって堅牢に分離します。


## 攻撃シナリオの例

**シナリオ #1:** 資格情報の復旧ワークフローには「質問と回答」が含まれる場合がありますが、これは NIST 800-63b、OWASP ASVS、OWASP Top 10 で禁止されています。質問と回答は、複数の人が回答を知ることができるため、本人確認の証拠として信頼できません。このような機能は削除し、よりセキュアな設計に置き換える必要があります。

**シナリオ #2:** ある映画館チェーンでは、団体予約割引を設けており、最大 15 名まで予約金を要求しています。攻撃者は、このフローを脅威モデル化し、アプリケーションのビジネス ロジックに攻撃ベクトルを見つけられるかどうかをテストできます。例えば、数回のリクエストで 600 席の座席とすべての映画館を一括予約し、莫大な収益損失を引き起こすといった攻撃です。

**シナリオ #3:** ある小売チェーンの e コマース ウェブ サイトには、高級ビデオカードを購入しオークション サイトで転売する転売業者が運営するボットに対する保護対策が施されていません。これは、ビデオカード メーカーと小売チェーンのオーナーにとって悪評を招き、どんなに高くてもこれらのカードを入手できない愛好家との確執を招いてしまいます。慎重なアンチ ボット設計と、入手可能になってから数秒以内に購入を行うといったドメイン ロジック ルールを整備することで、不正な購入を識別し、そのような取引を拒否できる可能性があります。


## 参考情報



* [OWASP Cheat Sheet: Secure Design Principles](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html)
* [OWASP SAMM: Design | Secure Architecture](https://owaspsamm.org/model/design/secure-architecture/)
* [OWASP SAMM: Design | Threat Assessment](https://owaspsamm.org/model/design/threat-assessment/)
* [NIST – Guidelines on Minimum Standards for Developer Verification of Software](https://www.nist.gov/publications/guidelines-minimum-standards-developer-verification-software)
* [The Threat Modeling Manifesto](https://threatmodelingmanifesto.org/)
* [Awesome Threat Modeling](https://github.com/hysnsec/awesome-threat-modelling)


## 対応する CWE の一覧

* [CWE-73 External Control of File Name or Path](https://cwe.mitre.org/data/definitions/73.html)
* [CWE-183 Permissive List of Allowed Inputs](https://cwe.mitre.org/data/definitions/183.html)
* [CWE-256 Unprotected Storage of Credentials](https://cwe.mitre.org/data/definitions/256.html)
* [CWE-266 Incorrect Privilege Assignment](https://cwe.mitre.org/data/definitions/266.html)
* [CWE-269 Improper Privilege Management](https://cwe.mitre.org/data/definitions/269.html)
* [CWE-286 Incorrect User Management](https://cwe.mitre.org/data/definitions/286.html)
* [CWE-311 Missing Encryption of Sensitive Data](https://cwe.mitre.org/data/definitions/311.html)
* [CWE-312 Cleartext Storage of Sensitive Information](https://cwe.mitre.org/data/definitions/312.html)
* [CWE-313 Cleartext Storage in a File or on Disk](https://cwe.mitre.org/data/definitions/313.html)
* [CWE-316 Cleartext Storage of Sensitive Information in Memory](https://cwe.mitre.org/data/definitions/316.html)
* [CWE-362 Concurrent Execution using Shared Resource with Improper Synchronization ('Race Condition')](https://cwe.mitre.org/data/definitions/362.html)
* [CWE-382 J2EE Bad Practices: Use of System.exit()](https://cwe.mitre.org/data/definitions/382.html)
* [CWE-419 Unprotected Primary Channel](https://cwe.mitre.org/data/definitions/419.html)
* [CWE-434 Unrestricted Upload of File with Dangerous Type](https://cwe.mitre.org/data/definitions/434.html)
* [CWE-436 Interpretation Conflict](https://cwe.mitre.org/data/definitions/436.html)
* [CWE-444 Inconsistent Interpretation of HTTP Requests ('HTTP Request Smuggling')](https://cwe.mitre.org/data/definitions/444.html)
* [CWE-451 User Interface (UI) Misrepresentation of Critical Information](https://cwe.mitre.org/data/definitions/451.html)
* [CWE-454 External Initialization of Trusted Variables or Data Stores](https://cwe.mitre.org/data/definitions/454.html)
* [CWE-472 External Control of Assumed-Immutable Web Parameter](https://cwe.mitre.org/data/definitions/472.html)
* [CWE-501 Trust Boundary Violation](https://cwe.mitre.org/data/definitions/501.html)
* [CWE-522 Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)
* [CWE-525 Use of Web Browser Cache Containing Sensitive Information](https://cwe.mitre.org/data/definitions/525.html)
* [CWE-539 Use of Persistent Cookies Containing Sensitive Information](https://cwe.mitre.org/data/definitions/539.html)
* [CWE-598 Use of GET Request Method With Sensitive Query Strings](https://cwe.mitre.org/data/definitions/598.html)
* [CWE-602 Client-Side Enforcement of Server-Side Security](https://cwe.mitre.org/data/definitions/602.html)
* [CWE-628 Function Call with Incorrectly Specified Arguments](https://cwe.mitre.org/data/definitions/628.html)
* [CWE-642 External Control of Critical State Data](https://cwe.mitre.org/data/definitions/642.html)
* [CWE-646 Reliance on File Name or Extension of Externally-Supplied File](https://cwe.mitre.org/data/definitions/646.html)
* [CWE-653 Insufficient Compartmentalization](https://cwe.mitre.org/data/definitions/653.html)
* [CWE-656 Reliance on Security Through Obscurity](https://cwe.mitre.org/data/definitions/656.html)
* [CWE-657 Violation of Secure Design Principles](https://cwe.mitre.org/data/definitions/657.html)
* [CWE-676 Use of Potentially Dangerous Function](https://cwe.mitre.org/data/definitions/676.html)
* [CWE-693 Protection Mechanism Failure](https://cwe.mitre.org/data/definitions/693.html)
* [CWE-799 Improper Control of Interaction Frequency](https://cwe.mitre.org/data/definitions/799.html)
* [CWE-807 Reliance on Untrusted Inputs in a Security Decision](https://cwe.mitre.org/data/definitions/807.html)
* [CWE-841 Improper Enforcement of Behavioral Workflow](https://cwe.mitre.org/data/definitions/841.html)
* [CWE-1021 Improper Restriction of Rendered UI Layers or Frames](https://cwe.mitre.org/data/definitions/1021.html)
* [CWE-1022 Use of Web Link to Untrusted Target with window.opener Access](https://cwe.mitre.org/data/definitions/1022.html)
* [CWE-1125 Excessive Attack Surface](https://cwe.mitre.org/data/definitions/1125.html)