<link rel="stylesheet" href="./assets/css/RC-stylesheet.css" />

# 現代的なアプリケーション セキュリティ プログラムの確立

OWASP Top 10 のリストは、対象となるトピックにおける最も重要なリスクへの意識向上を目的とした意識啓発のための文書です。これらは完全なリストではなく、あくまで出発点となるものです。以前のバージョンでは、これらのリスクなどを回避する最善の方法として、アプリケーション セキュリティプ ログラムの開始を推奨していました。このセクションでは、現代的なアプリケーション セキュリティ プログラムを開始および構築する方法について説明します。

既にアプリケーション セキュリティ プログラムを持っている場合は、[OWASP SAMM (Software Assurance Maturity Model)](https://owasp.org/www-project-samm/) または DSOMM (DevSecOps Maturity Model) を使用して、成熟度評価を実施することを検討します。これらの成熟度モデルは包括的かつ網羅的であり、プログラムの拡張と成熟化に向けて、どこに重点的に取り組むべきかを判断するのに役立ちます。ただし、OWASP SAMM または DSOMM に記載されているすべての手順を実行する必要はありません。これらのモデルは、ガイドとして、また多くの選択肢を提供することを目的としているためです。これらは、達成不可能な基準を提示したり、予算が足りないプログラムを説明するものではありません。多くのアイデアや選択肢を提供するために、広範囲にわたっています。

プログラムをゼロから始める場合、または OWASP SAMM や DSOMM が現状のチームにとって「過度である」と感じている場合は、以下のアドバイスをレビューします。

### 1. リスク ベースのポートフォリオ アプローチの確立

* ビジネスの観点から、アプリケーション ポートフォリオの保護ニーズを特定します。これは、保護対象のデータ資産に関連するプライバシー法やその他の規制に基づいて決定する必要があります。
* 組織のリスク許容度を反映した、一貫した発生可能性と影響度を持つ[共通のリスク評価モデル](https://owasp.org/www-community/OWASP_Risk_Rating_Methodology)を確立します。
* 上記に基づいて、すべてのアプリケーションと API を測定し、優先順位を付けます。結果を[構成管理データベース (CMDB)](https://de.wikipedia.org/wiki/Configuration_Management_Database) に追加します。
* 必要な範囲と厳密さのレベルを適切に定義するための保証ガイドラインを確立します。

### 2. 強固な基盤によるイネーブルメント

* すべての開発チームが遵守すべきアプリケーション セキュリティのベースラインとなる、焦点を絞ったポリシーと標準を確立します。
* これらのポリシーと標準を補完する、再利用可能なセキュリティ管理策の共通セットを定義し、その使用に関する設計および開発ガイダンスを提供します。
* 開発における様々な役割やトピックを対象とし、必要かつ適切なアプリケーション セキュリティ トレーニングのカリキュラムを確立します。

### 3．既存のプロセスへのセキュリティの統合

* セキュアな実装と検証活動を定義し、既存の開発プロセスおよび運用プロセスに統合します。
* 活動には、脅威モデリング、セキュア設計と設計レビュー、セキュア コーディングとコード レビュー、ペネトレーション テスト、修復が含まれます。
* 開発チームとプロジェクト チームの成功を支援するため、専門家とサポート サービスを提供します。
* 現在のシステム開発ライフ サイクルと、すべてのソフトウェア セキュリティ活動、ツール、ポリシー、プロセスをレビューし、文書化します。
* 新しいソフトウェアについては、システム開発ライフサイクル (SDLC) の各フェーズに 1 つ以上のセキュリティ活動を追加します。以下では、実行できる多くの提案を示します。すべての新規プロジェクトまたはソフトウェア イニシアチブでこれらの新しい活動を実施することで、新しいソフトウェアが組織にとって許容可能なセキュリティ体制で提供されることを確実にします。
* 最終製品が組織にとって許容可能なリスク レベルを満たすように、活動を選択します。
* 既存のソフトウェア (レガシーと呼ばれることもあります) については、正式な維持計画が必要です。セキュアなアプリケーションを維持する方法については、以下の「運用と変更管理」セクションを確認します。

### 4. アプリケーション セキュリティ教育

* 開発者に知っておいてほしいことをすべて教えるために、セキュリティ チャンピオンプ ログラム、または開発者向けの一般的なセキュリティ教育プログラム（セキュリティ推進プログラム（訳注: 原文は advocay program）やセキュリティ意識向上プログラムと呼ばれることもあります）の開始を検討します。これにより、開発者は最新の情報を入手し、セキュアに作業を行う方法を理解できるようになり、現場のセキュリティ文化がより前向きになります。また、チーム間の信頼関係が強化され、円滑な業務関係が築かれることも少なくありません。OWASP は、[OWASP Security Champions Guide](https://securitychampions.owasp.org/) を通じて、この点を支援しており、このガイドは段階的に拡充されています。
* OWASP Education Project は、Web アプリケーション セキュリティに関する開発者の教育に役立つトレーニング資料を提供しています。脆弱性に関する実践的な学習には、[OWASP Juice Shop Project](https://owasp.org/www-project-juice-shop/) または [OWASP WebGoat](https://owasp.org/www-project-webgoat/) で試すことができます。最新情報を入手するには、[OWASP AppSec Conference](https://owasp.org/events/)、[OWASP AppSec Conference](https://owasp.org/events/), [OWASP Conference Training](https://owasp.org/events/)、またはローカルの [OWASP Chapter](https://owasp.org/chapters/) ミーティングに参加します。

### 5. 管理の可視性の提供

* 指標を用いて管理します。指標と収集した分析データに基づいて、改善と予算配分の決定を推進します。指標には、セキュリティ対策および活動の遵守状況、導入された脆弱性、軽減された脆弱性、アプリケーション カバレッジ、種類別の不具合密度、事例数などが含まれる。
* 実装および検証活動から得られたデータを分析し、根本原因と脆弱性のパターンを特定することで、企業全体にわたる戦略的かつ体系的な改善を推進します。失敗から学び改善を促進するための積極的なインセンティブを提供します。

## 反復可能なセキュリティ プロセスと標準セキュリティ管理策の確立と使用

### 要件およびリソース管理フェーズ

* アプリケーションのビジネス要件を収集し、ビジネス部門と交渉します。これには、すべてのデータ資産の機密性、真正性、完全性、可用性に関する保護要件、および想定されるビジネス ロジックが含まれます。
* 機能的および非機能的なセキュリティ要件を含む技術要件をまとめます。OWASP は、アプリケーションのセキュリティ要件を設定するためのガイドとして、[OWASP Application Security Verification Standard (ASVS)(https://owasp.org/www-project-application-security-verification-standard/) の使用を推奨しています。
* セキュリティ活動を含む、設計、構築、テスト、運用のあらゆる側面をカバーする予算を計画し、交渉します。
* プロジェクト スケジュールにセキュリティ活動を追加します。
* プロジェクトのキックオフ時にセキュリティ担当者として参画し、担当者が誰と話をすればよいかを把握できるようにします。

### 提案依頼書 (RFP) と契約

* セキュリティ プログラムに関するガイドラインやセキュリティ要件（SDLC、ベスト プラクティスなど）を含む要件について、内部または外部の開発者と交渉します。
* 計画・設計フェーズを含む、すべての技術要件の達成状況を評価します。
* 設計、セキュリティ、サービス レベル契約 (SLA) を含む、すべての技術要件について交渉します。
* [OWASP Secure Software Contract Annex](https://owasp.org/www-community/OWASP_Secure_Software_Contract_Annex)<sup>(*1)</sup> などのテンプレートとチェック リストを採用します。<br>**\*1: 注** *この附属書は米国の契約法に関するものです。サンプルの附属書を使用する前に、必ず資格のある法律専門家に相談する必要があります。*

### 計画・設計フェーズ

* 開発者や内部関係者（セキュリティ専門家など）と、計画と設計について協議します。
* 保護ニーズと想定される脅威レベルに適したセキュリティ アーキテクチャ、コントロール、対策、設計レビューを定義します。これにはセキュリティ専門家のサポートが必要です。
* アプリケーションや API に後からセキュリティを組み込むよりも、最初からセキュリティを設計に組み込む方がはるかに費用対効果が高いです。OWASP は、[OWASP Cheat Sheet: Threat Modeling](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) と [OWASP Proactive Controls](https://top10proactive.owasp.org/) を、最初からセキュリティを組み込んだ設計方法のガイダンスとして推奨しています。
* 脅威モデリングを実施します。実施においては、[OWASP Cheat Sheet: Threat Modeling](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) を参照します。
* ソフトウェア アーキテクトにセキュアな設計の考え方とパターンを教え、可能な場合は設計に取り入れるよう依頼します。
* 開発者と共にデータ フローを検証します。
* 他のユーザー ストーリーに加えて、セキュリティに関するユーザー ストーリーも追加します。

### セキュアな開発ライフ サイクル

* 組織がアプリケーションや API を構築する際のプロセスを改善するために、OWASP は [OWASP Software Assurance Maturity Model (SAMM)](https://owasp.org/www-project-samm/) を推奨しています。このモデルは、組織が直面する特定のリスクに合わせたソフトウェア セキュリティ戦略を策定・実装するのに役立ちます。
* ソフトウェア開発者に、セキュア コーディングのトレーニング、およびより堅牢でセキュアなアプリケーションの作成に役立つと思われるその他のトレーニングを提供します。
* コード レビューについては、[OWASP Cheat Sheet: Secure Code Review](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Code_Review_Cheat_Sheet.html)　を参照します。

* 開発者にセキュリティ ツールを提供し、その使い方を指導します。特に、静的解析、ソフトウェア構成解析、シークレット情報、および [Infrastructure-as-Code (IaC)](https://cheatsheetseries.owasp.org/cheatsheets/Infrastructure_as_Code_Security_Cheat_Sheet.html) 用のスキャナーについて指導します。
* 可能であれば、開発者向けのガードレール（よりセキュアな選択肢へと導くための技術的安全策）を作成します。
* 強力で使いやすいセキュリティ管理策を構築するのは困難です。可能な限りセキュアなデフォルト設定を提供し、「舗装された道」（何かをするにあたり最も簡単で最もセキュアな方法、つまり明らかに好ましい方法）を可能な限り用意します。[OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/index.html) は開発者にとって良い出発点となり、多くの最新フレームワークには、認証、検証、CSRF 防止などのための標準的で効果的なセキュリティ管理策が組み込まれています。
* 開発者にセキュリティ関連の IDE プラグインを提供し、使用を促します。
* シークレット情報管理ツール、ライセンス、使用方法に関する文書を提供します。
* 開発者が使用できるプライベート AI を提供します。理想的には、有用なセキュリティ文書が満載の RAG サーバー、より良い結果を得るためにチームが作成したプロンプト、そして組織で選択したセキュリティ ツールを呼び出す MCP サーバーでセットアップします。開発者に AI を安全に使用する方法を教えます。好むと好まざるとにかかわらず、開発者は AI を使うことになるからです。

### Establish Continuous Application Security Testing:

*  Test the technical functions and integration with the IT architecture and coordinate business tests.

* Create “use” and “abuse” test cases from technical and business perspectives.

* Manage security tests according to internal processes, the protection needs, and the assumed threat level by the application.

* Provide security testing tools (fuzzers, DAST, etc.), a safe place to test, and training on how to use them, OR do the testing for them OR hire a tester

*  If you require a high level of assurance, consider a formal penetration test, as well as stress testing and performance testing.

*  Work with your developers to help them decide what they need to fix from the bug reports, and ensure their managers give them time to do it.


### Rollout:

* Put the application in operation and migrate from previously used applications if needed.

* Finalize all documentation, including the change management database (CMDB) and security architecture.


### Operations and Change Management:

*  Operations must include guidelines for the security management of the application (e.g. patch management).

*  Raise the security awareness of users and manage conflicts about usability vs. security.

*  Plan and manage changes, e.g. migrate to new versions of the application or other components like OS, middleware, and libraries.

*  Ensure all apps are in your inventory, with all important details documented. Update all documentation, including in the CMDB and the security architecture, controls, and countermeasures, including any runbooks or project documentation.

*  Perform logging, monitoring, and alerting for all apps. Add it if it’s missing.

*  Create processes for effective and efficient updating and patching.

*  Create regular scanning schedules (hopefully dynamic, static, secrets, IaC, and software composition analysis).

*  SLAs for fixing security bugs.

*  Provide a way for employees (and ideally also your customers) to report bugs.

*  Establish a trained incident response team that understands what software attacks look like, observability tooling.

*  Run blocking or shielding tools to stop automated attacks.

*  Annual (or more often) hardening of configurations.

*  At least annual penetration testing (depending upon the level assurance required for your app).

*  Establish processes and tooling for hardening and protecting your software supply chain.

*  Establish and update business continuity and disaster recovery planning that includes your most important applications and the tools you use to maintain them.


### Retiring Systems:

* Any required data should be archived. All other data should be securely wiped.

* Securely retire the application, including deleting unused accounts and roles and permissions.

* Set your application’s state to retired in the CMDB.


## Using the OWASP Top 10 as a standard

The OWASP Top 10 is primarily an awareness document. However, this has not stopped organizations from using it as a de facto industry AppSec standard since its inception in 2003. If you want to use the OWASP Top 10 as a coding or testing standard, know that it is the bare minimum and just a starting point.

One of the difficulties of using the OWASP Top 10 as a standard is that we document AppSec risks, and not necessarily easily testable issues. For example, [A06:2025-Insecure Design](A06_2025-Insecure_Design.md) is beyond the scope of most forms of testing. Another example is testing whether in-place, in-use, and effective logging and monitoring are implemented, which can only be done with interviews and requesting a sampling of effective incident responses. A static code analysis tool can look for the absence of logging, but it might be impossible to determine if business logic or access control is logging critical security breaches. Penetration testers may only be able to determine that they have invoked incident response in a test environment, which is rarely monitored in the same way as production.

Here are our recommendations for when it is appropriate to use the OWASP Top 10:


<table>
  <tr>
   <td><strong>Use Case</strong>
   </td>
   <td><strong>OWASP Top 10 2025</strong>
   </td>
   <td><strong>OWASP Application Security Verification Standard</strong>
   </td>
  </tr>
  <tr>
   <td>Awareness
   </td>
   <td>Yes
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Training
   </td>
   <td>Entry level
   </td>
   <td>Comprehensive
   </td>
  </tr>
  <tr>
   <td>Design and architecture
   </td>
   <td>Occasionally
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Coding standard
   </td>
   <td>Bare minimum
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Secure Code review
   </td>
   <td>Bare minimum
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Peer review checklist
   </td>
   <td>Bare minimum
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Unit testing
   </td>
   <td>Occasionally
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Integration testing
   </td>
   <td>Occasionally
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Penetration testing
   </td>
   <td>Bare minimum
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Tool support
   </td>
   <td>Bare minimum
   </td>
   <td>Yes
   </td>
  </tr>
  <tr>
   <td>Secure Supply Chain
   </td>
   <td>Occasionally
   </td>
   <td>Yes
   </td>
  </tr>
</table>


We would encourage anyone wanting to adopt an application security standard to use the [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/) (ASVS), as it’s designed to be verifiable and tested, and can be used in all parts of a secure development lifecycle.

The ASVS is the only acceptable choice for tool vendors. Tools cannot comprehensively detect, test, or protect against the OWASP Top 10 due to the nature of several of the OWASP Top 10 risks, with reference to [A06:2025-Insecure Design](A06_2025-Insecure_Design.md). OWASP discourages any claims of full coverage of the OWASP Top 10, because it’s simply untrue.