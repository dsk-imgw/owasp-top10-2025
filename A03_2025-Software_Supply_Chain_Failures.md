<link rel="stylesheet" href="./assets/css/RC-stylesheet.css" />

# A03:2025 ソフトウェア サプライチェーンの失敗 <img src="./assets/TOP_10_Icons_Final_Vulnerable_Outdated_Components.png" style="height:80px;width:80px" align="right">


## 背景

これは Top 10 のコミュニティ調査で第 1 位にランクインし、回答者のちょうど 50% が 1 位に挙げました。2013 年の Top 10 に「A9 – 既知の脆弱性を持つコンポーネントの使用」として初めて登場して以来、このリスクは既知の脆弱性に関連するものだけでなく、サプライチェーン全体におけるあらゆる問題を含むように範囲が拡大しました。このように範囲が拡大したにもかかわらず、サプライチェーンの問題は依然として特定が困難であり、関連する CWE を持つ共通脆弱性識別子（CVE）はわずか 11 件しかありません。しかし、提供されたデータに基づいてテストおよび報告された結果では、このカテゴリの平均発生率は 5.19% と最も高くなっています。関連する CWE は、*CWE-477: 非推奨関数の使用、CWE-1104: メンテナンスされていないサードパーティ コンポーネントの使用*、CWE-1329: *更新不可能なコンポーネントへの依存*、および*CWE-1395: 脆弱なサードパーティコンポーネントへの依存*です。

## スコア表


<table>
  <tr>
   <td>対応する CWE 
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
   <td>5
   </td>
   <td>8.81%
   </td>
   <td>5.19%
   </td>
   <td>65.42%
   </td>
   <td>28.93%
   </td>
   <td>8.17
   </td>
   <td>5.23
   </td>
   <td>215,248
   </td>
   <td>11
   </td>
  </tr>
</table>



## 説明

ソフトウェア サプライチェーンの失敗とは、ソフトウェアの構築、配布、または更新プロセスにおける不具合やその他の侵害のことです。これらは多くの場合、システムが依存しているサード パーティ製のコード、ツール、またはその他の依存関係における脆弱性や悪意のある変更によって引き起こされます。

以下のような場合は、脆弱な状態にある可能性が高いです。

* 使用しているすべてのコンポーネント（クライアント側とサーバー側の両方）のバージョンを注意深く追跡していない場合。これには、直接使用しているコンポーネントだけでなく、ネストされた（推移的な）依存関係にあるコンポーネントも含まれます。
* ソフトウェアに脆弱性がある、サポートが終了している、または古いバージョンである場合。これには、OS、Web/アプリケーション サーバー、データベース管理システム（DBMS）、アプリケーション、API、およびすべてのコンポーネント、ランタイム環境、ライブラリが含まれます。
* 脆弱性スキャンを定期的に実行せず、使用しているコンポーネントに関連するセキュリティ情報を購読していない場合。
* 変更管理プロセスがない、またはサプライチェーン内の変更を追跡していない場合。これには、IDE、IDE 拡張機能とアップデート、組織のコード リポジトリ、サンドボックス、イメージおよびライブラリのリポジトリへの変更、アーティファクトの作成および保存方法などが含まれます。サプライチェーンのすべての部分は、特に変更点について文書化する必要があります。
* アクセス制御と最小権限の原則の適用に特に重点を置いて、サプライチェーンのすべての部分を堅牢化していない場合。
* サプライチェーン システムに職務分掌が導入されていない場合。単一の人物が、他の人間の監視なしにコードを記述し、本番環境にデプロイできるべきではありません。
* 開発者、DevOps 担当者、またはインフラストラクチャ担当者が、本番環境で使用するために、信頼できないソースからコンポーネントをダウンロードして使用することを許可している場合。
* 基盤となるプラットフォーム、フレームワーク、および依存関係を、リスク ベースでタイムリーに修正またはアップグレードしていない場合。これは、パッチ適用が変更管理の下で月次または四半期ごとのタスクになっている環境でよく発生し、組織は脆弱性を修正するまで数日から数ヶ月間、不必要なリスクにさらされることになります。
* ソフトウェア開発者が、更新、アップグレード、またはパッチ適用されたライブラリの互換性をテストしていない場合。
* システムのすべての部分の構成をセキュアに設定していない場合（[A02:2025-セキュリティ設定ミス](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) を参照）。
* 多くのコンポーネントを使用する複雑な CI/CD パイプラインがあり、そのセキュリティがアプリケーションの他の部分よりも弱い場合。

## 防止方法

パッチ管理プロセスを導入して、以下を実施する必要があります。

* ソフトウェア全体のソフトウェア部品表（SBOM）を把握し、SBOM 辞書を一元的に管理します。
* 自身の依存関係だけでなく、それらの（推移的な）依存関係なども追跡します。
* 使用されていない依存関係、不要な機能、コンポーネント、ファイル、ドキュメントを削除して、攻撃対象領域を削減します。
* バージョン管理ツール、OWASP Dependency Check、retire.js などのツールを使用して、クライアント側とサーバー側のコンポーネント（フレームワーク、ライブラリなど）とその依存関係のバージョンを継続的にインベントリ化します。
* 使用しているコンポーネントの脆弱性について、Common Vulnerability and Exposures（CVE）やNational Vulnerability Database（NVD）などの情報源を継続的に監視します。ソフトウェア構成分析、ソフトウェア サプライチェーン、またはセキュリティに特化した SBOM ツールを使用してプロセスを自動化します。使用しているコンポーネントに関連するセキュリティ脆弱性に関するメール アラートを購読します。
* コンポーネントは、セキュリティが確保されたリンクを介して公式の（信頼できる）ソースからのみ入手します。改ざんされた悪意のあるコンポーネントが含まれる可能性を減らすために、署名付きパッケージを優先します（[A08:2025-ソフトウェアとデータの整合性の不具合](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/)を参照）。
* 使用する依存関係のバージョンを意図的に選択し、必要な場合にのみアップグレードします。
* メンテナンスされていないライブラリやコンポーネント、または古いバージョンに対するセキュリティ パッチが提供されていないライブラリやコンポーネントを監視します。パッチ適用が不可能な場合は、検出された問題に対する監視、検出、または保護のために仮想パッチの導入を検討します。
* CI/CD、IDE、およびその他の開発ツールを定期的に更新します。
* CI/CD パイプライン内のコンポーネントをこのプロセスの一部として扱い、セキュリティを強化し、監視し、変更を適切に文書化します。

変更管理プロセスや追跡システムを導入して、以下に対する変更を追跡する必要があります。

* CI/CD の設定（すべてのビルド ツールとパイプライン）
* コード リポジトリ
* サンドボックス環境
* 開発者向け IDE
* SBOM ツールおよび生成されたアーティファクト
* ログ記録システムとログ
* SaaS などのサード パーティ統合
* アーティファクト リポジトリ
* コンテナ レジストリ

以下のシステムを堅牢化します。これには、MFA の有効化と IAM のロック ダウンが含まれます。

* コード リポジトリ（機密情報のコミット禁止、ブランチ保護、バックアップなどを含む）
* 開発者ワーク ステーション（定期的なパッチ適用、MFA、監視など）
* ビルド サーバーおよび CI/CD（職務分掌、アクセス制御、署名付きビルド、環境スコープの機密情報、改ざん防止ログなど）
* アーティファクト（出所、署名、タイム スタンプによる整合性の確保、環境ごとに再ビルドするのではなくアーティファクトを活用、ビルドの不変性の確保）
* コードとしてのインフラストラクチャは、プル リクエストやバージョン管理の使用を含め、すべてのコードと同様に管理

すべての組織は、アプリケーションまたはポートフォリオのライフサイクル全体にわたって、監視、トリアージ、およびアップデートや構成変更の適用に関する継続的な計画を確実にする必要があります。

## 攻撃シナリオの例

**シナリオ #1:** 信頼できるベンダーがマルウェアに感染し、システム アップデート時に自社のコンピュータ システムも感染してしまうケース。おそらく最も有名な例は以下のとおりです。

* 2019 年に発生した SolarWinds 社のシステム侵害事件。これにより、約 18,000 もの組織が被害を受けました。[https://www.npr.org/2021/04/16/985439655/a-worst-nightmare-cyberattack-the-untold-story-of-the-solarwinds-hack](https://www.npr.org/2021/04/16/985439655/a-worst-nightmare-cyberattack-the-untold-story-of-the-solarwinds-hack)

**シナリオ #2:** 信頼できるベンダーが侵害され、特定の条件下でのみ悪意のある動作をするようになる。

* 2025 年に発生した Bybit の 15 億ドル相当の盗難事件は、標的のウォレットが使用されている場合にのみ実行されるウォレット ソフトウェアへのサプライチェーン攻撃によって引き起こされました。[https://thehackernews.com/2025/02/bybit-hack-traced-to-safewallet-supply.html](https://thehackernews.com/2025/02/bybit-hack-traced-to-safewallet-supply.html)

**シナリオ #3:** 2025年 に発生した GlassWorm サプライチェーン攻撃では、攻撃者が VS Code マーケットプレイスの正規拡張機能および OpenVSX マーケットプレイスの複数の拡張機能に、目に見えない自己複製型の悪意のあるコードを仕込み、それが開発者のマシンに自動的にアップデートされるという事態が発生しました。このマルウェアは、開発者のマシンからローカルの機密情報を即座に収集し、コマンド＆コントロール サーバーへの接続を試みるとともに、可能であれば開発者の暗号資産ウォレットから資金を盗み出そうとしました。このサプライチェーン攻撃は非常に高度で、急速に拡散し、甚大な被害をもたらしました。開発者のマシンを標的としたこの攻撃は、開発者自身が今やサプライチェーン攻撃の主要な標的となっていることを明確に示しています。

**シナリオ #4:** コンポーネントは通常、アプリケーション自体と同じ権限で実行されるため、いずれかのコンポーネントに欠陥があると深刻な影響が生じる可能性があります。このような欠陥は、偶発的なもの（例：コーディング エラー）である場合もあれば、意図的なもの（例：コンポーネントに仕込まれたバックドア）である場合もあります。発見された悪用可能なコンポーネントの脆弱性の例を、以下にいくつか挙げます。

* Struts 2のリモート コード実行の脆弱性である CVE-2017-5638 は、サーバー上で任意のコードを実行できるため、重大な情報漏洩の原因となりました。
* モノのインターネット（IoT）機器は、パッチ適用が困難または不可能な場合が多いですが、パッチを適用することは非常に重要です（例：医療機器）。

攻撃者がパッチ未適用または設定ミスのあるシステムを見つけるのに役立つ自動化ツールが存在します。例えば、[Shodan IoT](https://www.shodan.io) 検索エンジンを使えば、2014 年 4 月にパッチが適用された Heartbleed 脆弱性の影響を受けるデバイスを今でも見つけることができます。

## 参考情報

* [OWASP Application Security Verification Standard: V15 Secure Coding and Architecture](https://owasp.org/www-project-application-security-verification-standard/)
* [OWASP Cheat Sheet Series: Dependency Graph SBOM](https://cheatsheetseries.owasp.org/cheatsheets/Dependency_Graph_SBOM_Cheat_Sheet.html)
* [OWASP Cheat Sheet Series: Vulnerable Dependency Management](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerable_Dependency_Management_Cheat_Sheet.html)
* [OWASP Dependency-Track](https://owasp.org/www-project-dependency-track/)
* [OWASP CycloneDX](https://owasp.org/www-project-cyclonedx/)
* [OWASP Application Security Verification Standard: V1 Architecture, design and threat modelling](https://owasp-aasvs.readthedocs.io/en/latest/v1.html)
* [OWASP Dependency Check (for Java and .NET libraries)](https://owasp.org/www-project-dependency-check/)
* OWASP Testing Guide - Map Application Architecture (OTG-INFO-010)
* [OWASP Virtual Patching Best Practices](https://owasp.org/www-community/Virtual_Patching_Best_Practices)
* [The Unfortunate Reality of Insecure Libraries](https://www.scribd.com/document/105692739/JeffWilliamsPreso-Sm)
* [MITRE Common Vulnerabilities and Exposures (CVE) search](https://www.cve.org)
* [National Vulnerability Database (NVD)](https://nvd.nist.gov)
* [Retire.js for detecting known vulnerable JavaScript libraries](https://retirejs.github.io/retire.js/)
* [GitHub Advisory Database](https://github.com/advisories)
* Ruby Libraries Security Advisory Database and Tools
* [SAFECode Software Integrity Controls (PDF)](https://safecode.org/publication/SAFECode_Software_Integrity_Controls0610.pdf)
* [Glassworm supply chain attack](https://thehackernews.com/2025/10/self-spreading-glassworm-infects-vs.html)
* [PhantomRaven supply chain attack campaign](https://thehackernews.com/2025/10/phantomraven-malware-found-in-126-npm.html)


## 対応する CWE の一覧

* [CWE-447 Use of Obsolete Function](https://cwe.mitre.org/data/definitions/447.html)
* [CWE-1035 2017 Top 10 A9: Using Components with Known Vulnerabilities](https://cwe.mitre.org/data/definitions/1035.html)
* [CWE-1104 Use of Unmaintained Third Party Components](https://cwe.mitre.org/data/definitions/1104.html)
* [CWE-1329 Reliance on Component That is Not Updateable](https://cwe.mitre.org/data/definitions/1329.html)
* [CWE-1395 Dependency on Vulnerable Third-Party Component](https://cwe.mitre.org/data/definitions/1395.html)
