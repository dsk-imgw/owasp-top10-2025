<link rel="stylesheet" href="../../assets/css/RC-stylesheet.css" />

# 次の段階

OWASP Top 10は、設計上、最も重大なリスク 10 件に限定されています。すべての OWASP Top 10 には、掲載に向けて綿密に検討されたものの、最終的に採用されなかった「選ばれる一歩手前の」リスクが含まれています。他のリスクの方がより蔓延しており、影響も大きいからです。

成熟したアプリケーション セキュリティ プログラムの構築を目指す組織、セキュリティ コンサルタント、あるいは製品の適用範囲を拡大したいと考えているツール ベンダーにとって、以下の 2 つの問題は、特定して対策を講じる価値が十分にあります。

## X01:2025 アプリケーションの回復力の欠如

### 背景

これは 2021 年のサービス拒否攻撃を名称変更したものです。これは根本原因ではなく結果を説明していたため、名称が変更されました。このカテゴリは、レジリエンス（回復力）の問題に関連する脆弱性を説明する CWE に焦点を当てています。このカテゴリのスコアは、A10:2025-例外的な状況への不適切な対処 と非常に近いものでした。関連する CWE には、*CWE-400 制御不能なリソース消費、CWE-409 高度に圧縮されたデータの不適切な処理（データ増幅）、CWE-674 制御不能な再帰*、*CWE-835 到達不可能な終了条件を持つループ（「無限ループ」）* などがあります。

### スコア表


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
   <td>20.05%
   </td>
   <td>4.55%
   </td>
   <td>86.01%
   </td>
   <td>41.47%
   </td>
   <td>7.92
   </td>
   <td>3.49
   </td>
   <td>865,066
   </td>
   <td>4,423
   </td>
  </tr>
</table>



### 説明

このカテゴリは、アプリケーションがストレス、障害、そして障害から回復できないエッジ ケースへの対応方法におけるシステム的な弱点を代表しています。アプリケーションが予期せぬ状況、リソース制約、その他の有害事象を適切に処理、耐久、回復できない場合、（最も一般的には）可用性の問題だけでなく、データ破損、機密データの漏洩、連鎖障害、セキュリティ管理策のバイパスなどにも容易につながる可能性があります。

さらに、[X02:2025 メモリ管理の失敗](#x022025-memory-management-failures) も、アプリケーションまたはシステム全体の障害につながる可能性があります。

### 防止方法

この種類の脆弱性を防止するには、システムの障害発生と復旧を想定した設計が必要です。

* 制限、クォータ、フェイル オーバー機能を追加し、特にリソースを最も消費する動作に注意します。
* リソースを大量に消費するページを特定し、事前に計画を立てます。特に、不要な「ガジェット」や、大量のリソース（CPU、メモリなど）を必要とする機能を、未知のユーザーや信頼できないユーザーに公開しないようにすることで、攻撃対象領域を削減します。
* 許可リストとサイズ制限を使用して厳格な入力検証を行い、徹底的にテストします。
* レスポンスのサイズを制限し、生のレスポンスをクライアントに返さないようにします（サーバー側で処理します）。
* デフォルトで safe/closed に設定し（決して開かない）、デフォルトで拒否し、エラーが発生した場合はロール バックします。
* リクエスト スレッドで同期呼び出しをブロックしないようにします（非同期/非ブロッキングを使用する、タイムアウトを設定する、同時実行数を制限するなど）。
* エラー処理機能を慎重にテストします。
* サーキット ブレーカー、バルク ヘッド、リトライ ロジック、グレースフル デグラデーションなどの回復力パターンを実装します。
* パフォーマンス テストと負荷テストを実施します。リスク選好がある場合は、カオス エンジニアリングを追加します。
* 合理的かつ費用対効果の高い範囲で、冗長性を実装および設します。
* 監視、可観測性、アラート機能を実装します。
* RFC 2267 に従って、無効な送信元アドレスをフィルタリングします。
* フィンガー プリント、IP アドレス、または振舞いに基づいて、既知のボット ネットを動的にブロックします。
* プルーフ・オブ・ワーク：*攻撃者*側でリソースを消費する操作を開始します。この操作は、通常のユーザーには大きな影響を与えませんが、大量のリクエストを送信しようとするボットには影響を与えます。システムの全体的な負荷が上昇した場合、特に信頼性の低いシステムやボットのように見えるシステムに対しては、プルーフ・オブ・ワークの難易度を上げます。
* 非アクティブ状態と最終タイム アウトに基づいて、サーバー側のセッション時間を制限します。
* セッションにバインドされた情報の保存を制限します。

### 攻撃シナリオの例

**シナリオ #1:** 攻撃者は意図的にアプリケーション リソースを消費し、システム内で障害を引き起こし、サービス拒否を引き起こします。具体的には、メモリ枯渇、ディスク容量の枯渇、CPU の飽和、あるいは無限接続の確立などが挙げられます。

**シナリオ #2:** 入力ファジングにより、細工された応答が生成され、アプリケーションのビジネス ロジックが破壊されます。

**シナリオ #3:** 攻撃者はアプリケーションの依存関係に着目し、API やその他の外部サービスを停止させることで、アプリケーションの動作を継続不能にします。


### 参考情報

* [OWASP Cheat Sheet: Denial of Service](https://cheatsheetseries.owasp.org/cheatsheets/Denial_of_Service_Cheat_Sheet.html)
* [OWASP MASVS‑RESILIENCE](https://mas.owasp.org/MASVS/11-MASVS-RESILIENCE/)
* [ASP.NET Core Best Practices (Microsoft)](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/best-practices?view=aspnetcore-9.0)
* [Resilience in Microservices: Bulkhead vs Circuit Breaker (Parser)](https://medium.com/@parserdigital/resilience-in-microservices-bulkhead-vs-circuit-breaker-54364c1f9d53)
* [Bulkhead Pattern (Geeks for Geeks)](https://www.geeksforgeeks.org/system-design/bulkhead-pattern/)
* [NIST Cybersecurity Framework (CSF)](https://www.nist.gov/cyberframework)
* [Avoid Blocking Calls: Go Async in Java (Devlane)](https://www.devlane.com/blog/avoid-blocking-calls-go-async-in-java)

## 対応する CWE の一覧

* [CWE-73  External Control of File Name or Path](https://cwe.mitre.org/data/definitions/73.html)
* [CWE-183 Permissive List of Allowed Inputs](https://cwe.mitre.org/data/definitions/183.html)
* [CWE-256 Plaintext Storage of a Password](https://cwe.mitre.org/data/definitions/256.html)
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
* [CWE-444 Inconsistent Interpretation of HTTP Requests ('HTTP Request/Response Smuggling')](https://cwe.mitre.org/data/definitions/444.html)
* [CWE-451 User Interface (UI) Misrepresentation of Critical Information](https://cwe.mitre.org/data/definitions/451.html)
* [CWE-454 External Initialization of Trusted Variables or Data Stores](https://cwe.mitre.org/data/definitions/454.html)
* [CWE-472 External Control of Assumed-Immutable Web Parameter](https://cwe.mitre.org/data/definitions/472.html)
* [CWE-501 Trust Boundary Violation](https://cwe.mitre.org/data/definitions/501.html)
* [CWE-522 Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)
* [CWE-525 Use of Web Browser Cache Containing Sensitive Information](https://cwe.mitre.org/data/definitions/20.html)
* [CWE-539 Use of Persistent Cookies Containing Sensitive Information](https://cwe.mitre.org/data/definitions/525.html)
* [CWE-598 Use of GET Request Method With Sensitive Query Strings](https://cwe.mitre.org/data/definitions/598.html)
* [CWE-602 Client-Side Enforcement of Server-Side Security](https://cwe.mitre.org/data/definitions/602.html)
* [CWE-628 Function Call with Incorrectly Specified Arguments](https://cwe.mitre.org/data/definitions/628.html)
* [CWE-642 External Control of Critical State Data](https://cwe.mitre.org/data/definitions/642.html)
* [CWE-646 Reliance on File Name or Extension of Externally-Supplied File](https://cwe.mitre.org/data/definitions/646.html)
* [CWE-653 Improper Isolation or Compartmentalization](https://cwe.mitre.org/data/definitions/653.html)
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


## X02:2025 メモリ管理の失敗

### 背景

Languagess like Java, C#, JavaScript/TypeScript (node.js), Go, and "safe" Rust are memory safe. Memory management problems tend to happen in non-memory safe languages such as C and C++. This category scored the lowest on the community survey and low in the data despite having the third most related CVEs. We believe this is due to the predominance of web applications over more traditional desktop applications. Memory management vulnerabilities frequently have the highest CVSS scores. 


### スコア表


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
   <td>24
   </td>
   <td>2.96%
   </td>
   <td>1.13%
   </td>
   <td>55.62%
   </td>
   <td>28.45%
   </td>
   <td>6.75
   </td>
   <td>4.82
   </td>
   <td>220,414
   </td>
   <td>30,978
   </td>
  </tr>
</table>



### 説明

When an application is forced to manage memory itself, it is very easy to make mistakes. Memory safe languages are being used more often, but there are still many legacy systems in production worldwide, new low-level systems that require the use of non-memory safe languages, and web applications that interact with mainframes, IoT devices, firmware, and other systems that may be forced to manage their own memory. Representative CWEs are *CWE-120 Buffer Copy without Checking Size of Input ('Classic Buffer Overflow')* and *CWE-121 Stack-based Buffer Overflow*.

Memory management failures can happen when:

* You do not allocate enough memory for a variable
* You do not validate input, causing an overflow of the heap, the stack, a buffer
* You store a data value that is larger than the type of the variable can hold 
* You attempt to use unallocated memory or address spaces
* You create off-by-one errors (counting from 1 instead of zero)
* You try to access an object after its been freed
* You use uninitialized variables
* You leak memory or otherwise use up all available memory in error until our application fails

Memory management failures can lead to failure of the application or even the entire system, see also [X01:2025 Lack of Application Resilience](#x012025-lack-of-application-resilience)


### 防止方法

The best way to prevent memory management failures is to use a memory-safe language. Examples include Rust, Java, Go, C#, Python, Swift, Kotlin, JavaScript, etc. When creating new applications, try hard to convince your organization that it is worth the learning curve to switch to a memory-safe language. If performing a full refactor, push for a rewrite in a memory-safe language when it is possible and feasible.

If you are unable to use a memory-safe language, perform the following:

* Enable the following server features that make memory management errors harder to exploit: address space layout randomization (ASLR), Data Execution Protection (DEP), and Structured Exception Handling Overwrite Protection (SEHOP).
* Monitor your application for memory leaks.
* Validate all input to your system very carefully, and reject all input that does not meet expectations.
* Study the language you are using and make a list of unsafe and more-safe functions, then share that list with your entire team. If possible, add it to your secure coding guideline or standard. For example, in C, prefer strncpy() over strcpy() and strncat() over strcat().
* If your language or framework offers memory safety libraries, use them. For example: Safestringlib or SafeStr.
* Use managed buffers and strings rather than raw arrays and pointers whenever possible.
* Take secure coding training that focuses on memory issues and/or your language of choice. Inform your trainer that you are concerned about memory management failures.
* Perform code reviews and/or static analyses.
* Use compiler tools that help with memory management such as StackShield, StackGuard, and Libsafe.
* Perform fuzzing on every input to your system.
* If you have a penetration test performed, inform your tester that you are concerned about memory management failures and that you would like them to pay special attention to this while testing.
*  Fix all compiler errors *and* warnings. Do not ignore warnings because your program compiles.
* Ensure your underlying infrastructure is regularly patched, scanned, and hardened.
* Monitor your underlying infrastructure specifically for potential memory vulnerabilities and other failures.
* Consider using [canaries](https://en.wikipedia.org/wiki/Buffer_overflow_protection#Canaries) to protect your address stack from overflow attacks.

### 攻撃シナリオの例

**Scenario #1:** Buffer overflows are the most famous memory vulnerability, a situation where an attacker submits more information into a field than it can accept, such that it overflows the buffer created for the underlying variable. In a successful attack, the overflow characters overwrite the stack pointer, allowing the attacker to insert malicious instructions into your program.

**Scenario #2:** Use-After-Free (UAF) happens often enough that it’s a semi-common browser bug bounty submission. Imagine a web browser processing JavaScript that manipulates DOM elements. The attacker crafts a JavaScript payload that creates an object (such as a DOM element) and obtains references to it. Through careful manipulation, they trigger the browser to free the object's memory while keeping a dangling pointer to it. Before the browser realizes the memory has been freed, the attacker allocates a new object that occupies the *same* memory space. When the browser tries to use the original pointer, it now points to attacker-controlled data. If this pointer was for a virtual function table, the attacker can redirect code execution to their payload. 

**Scenario #3:** A network service that accepts user input, doesn’t properly validate or sanitize it, then passes it directly to the logging function. The input from the user is passed to the logging function as syslog(user_input) instead of syslog("%s", user_input), which doesn’t specify the format. The attacker sends malicious payloads containing format specifiers such as %x to read stack memory (sensitive data disclosure) or %n to write to memory addresses. By chaining together multiple format specifiers they could map out the stack, locate important addresses, and then overwrite them. This would be a Format string vulnerability (uncontrolled string format). 

Note: modern browsers use many levels of defenses to defend against such attacks, including [browser sandboxing](https://www.geeksforgeeks.org/ethical-hacking/what-is-browser-sandboxing/#types-of-browser-sandboxing) ASLR, DEP/NX, RELRO, and PIE. A memory management failure attack on a browser is not a simple attack to carry out.

### 参考情報

* [OWASP community pages: Memory leak,](https://owasp.org/www-community/vulnerabilities/Memory_leak) [Doubly freeing memory,](https://owasp.org/www-community/vulnerabilities/Doubly_freeing_memory) [& Buffer Overflow](https://owasp.org/www-community/vulnerabilities/Buffer_Overflow)
* [Awesome Fuzzing: a list of fuzzing resources](https://github.com/secfigo/Awesome-Fuzzing) 
* [Project Zero Blog](https://googleprojectzero.blogspot.com)
* [Microsoft MSRC Blog](https://www.microsoft.com/en-us/msrc/blog)

## 対応する CWE の一覧

* [CWE-14 Compiler Removal of Code to Clear Buffers](https://cwe.mitre.org/data/definitions/14.html)
* [CWE-119 Improper Restriction of Operations within the Bounds of a Memory Buffer](https://cwe.mitre.org/data/definitions/119.html)
* [CWE-120 Buffer Copy without Checking Size of Input ('Classic Buffer Overflow')](https://cwe.mitre.org/data/definitions/120.html)
* [CWE-121 Stack-based Buffer Overflow](https://cwe.mitre.org/data/definitions/121.html)
* [CWE-122 Heap-based Buffer Overflow](https://cwe.mitre.org/data/definitions/122.html)
* [CWE-124 Buffer Underwrite ('Buffer Underflow')](https://cwe.mitre.org/data/definitions/124.html)
* [CWE-125 Out-of-bounds Read](https://cwe.mitre.org/data/definitions/125.html)
* [CWE-126 Buffer Over-read](https://cwe.mitre.org/data/definitions/126.html)
* [CWE-190 Integer Overflow or Wraparound](https://cwe.mitre.org/data/definitions/190.html)
* [191 Integer Underflow (Wrap or Wraparound)](https://cwe.mitre.org/data/definitions/191.html)
* [CWE-196 Unsigned to Signed Conversion Error](https://cwe.mitre.org/data/definitions/196.html)
* [CWE-367 Time-of-check Time-of-use (TOCTOU) Race Condition](https://cwe.mitre.org/data/definitions/367.html)
* [CWE-415 Double Free](https://cwe.mitre.org/data/definitions/415.html)
* [CWE-416 Use After Free](https://cwe.mitre.org/data/definitions/416.html)
* [CWE-457 Use of Uninitialized Variable](https://cwe.mitre.org/data/definitions/457.html)
* [CWE-459 Incomplete Cleanup](https://cwe.mitre.org/data/definitions/459.html)
* [CWE-467 Use of sizeof() on a Pointer Type](https://cwe.mitre.org/data/definitions/467.html)
* [CWE-787 Out-of-bounds Write](https://cwe.mitre.org/data/definitions/787.html)
* [CWE-788 Access of Memory Location After End of Buffer](https://cwe.mitre.org/data/definitions/788.html)
* [CWE-824 Access of Uninitialized Pointer](https://cwe.mitre.org/data/definitions/824.html)