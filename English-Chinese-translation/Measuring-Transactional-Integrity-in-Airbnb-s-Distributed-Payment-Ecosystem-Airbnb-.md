# Measuring-Transactional-Integrity-in-Airbnb-s-Distributed-Payment-Ecosystem-Airbnb-

Ninad Khisti
Payments Engineering @ Airbnb
Jan 16

Measuring Transactional Integrity in Airbnb’s Distributed Payment Ecosystem
在Airbnb中分布式支付生态系统测量交易的完整性

By Ninad Khisti & William Betz

In a distributed payment ecosystem, it is critical to accurately measure and track a transaction’s end to end state and contents to ensure consistency throughout the payment cycle. Without robust tracking, data leakage and errors can occur, resulting in either lost revenue or increased costs for all parties in the payment cycle, including the consumers, merchants, gateways, and acquirers.

在分布式生态支付系统中，对于准确测量，跟踪全程交易状态以及在整个支付周期中确保内容的一致性上，备受挑剔。如果没有可靠的跟踪，数据泄漏和出现错误，将会导致交易各方收益减少或增加成本，包括消费者，商家，网关和 收购方。

Transactional integrity is hard to measure precisely in a distributed payment system. With multiple systems and entities involved in any given transaction, tracking the state of a payment can be tedious and hard to obtain in a timely fashion.
在分布式交易系统中，精确测量对于交易完整性来说是不易的。在任何给定的交易中，涉及到多系统和实体的参与，跟踪支付状态可能是繁琐和难以得到即时反馈的。

With big data tools, Airbnb is tracing the contents of every transaction through various payment states to ensure every piece of the payments cycle lands in a consistent state. The reconciliation process not only produces data and insights that enable the team to track and mitigate unexpected transaction behavior, but also enables the system to “self-heal” certain aberrations when detected.

随着大数据工具的出现，Airbnbn跟踪每一个交易通过各种支付状态，确保支付周期的每一个价格能保持一致。帐目核对过程不仅产生数据和能使团队具有跟踪和减轻意外交易行为的洞穿力，而且当检测到异样时能使系统“自我修复”。

Background背景

With rapidly emerging payments technologies, merchants face a continually evolving landscape when it comes to processing payment transactions. The advent of new value-add entities, such as payment gateways, has offered increasingly large benefits to merchants, providing simplification by offering vaulting services and single API integration for payments processing. By integrating with payment gateways, Airbnb has been able to rapidly scale worldwide through various processing entities with minimal changes to our online transaction processing.

随着支付技术的迅速崛起，当处理交易事务时，商人们面临一个不断发生变化的局面，当增值实体出现时，比如支付网关，给商人带来越来越多的利益，为支付处理提供跨越服务和单一API集成来简化支付业务。通过使用支付网关的集成，Airbnb 对我们的在线交易处理用最小的改变，通过各种处理实体在全球范围内迅速扩张。


This being said, integration with any new value-add entity comes at a cost. Each new entity in the payments process adds an additional layer where a transaction’s integrity can be effected. A breakdown in a transaction’s integrity can create headaches for community members, increased workload for customer support, and an operational efficiency overhead.

这样说吧，与其它新的增值实体的集成都需要付出代价。在支付过程中每一个新的实体都额外增加了一个能够影响交易完整的层。交易完整性的崩溃可以为社区成员带来麻烦，增加客服工作量和运营效率的开支。

What Is Transactional Integrity? 交易完整性是什么？

Generally, a transaction represents a unit of work performed in RDMS (Relational Database Management System) with atomic, consistent, isolated and durable properties. When it comes to payments, transactional integrity represents a no-surprise, accurate money movement. Accuracy of a financial transaction can be verified by various attributes, such as amount, currency, status, payment method, and even timeliness.

一般来说，完整性代表在RDMS中(Relational Database Management System 关系型数据库管理系统)执行的工作单元，它具有原子性，持久性，隔防性和持久属性。当涉及到支付时，交易完整性代表了稳定，准确的资金流动。多种属性，比如金额，货币，状态，支付方式甚至时间问题，与金融交易的准确性能都够被验证。

At Airbnb, transactional integrity encompasses both internal and external money movements. This means that we not only expect full consistency of payment attributes between our internal systems, but also across all external partners that the payment touches.

在Airbnb，交易完整性包含了内部和外部资金的流动。这意味着我们不仅期望在内部系统中支付属性完全一致，而且在支付触及到的所有外部合作伙伴也会有这样的一致性。

Problem Statement 描述问题

Airbnb is an accommodation platform connecting guests and hosts to enable travel worldwide. Payments at Airbnb is a key factor to enable community trust, both on- and off-platform. To maintain this trust, it is crucial that we properly handle payments between our guest and host communities with the utmost accuracy.

Airbnb是一个连接全球客人和房主的住宿平台。无论线上线下，支付在Airbnb是社区信任的一个关键因素，为了保持信任，我们要用最大的准确性妥善处理我们的客人和屋主社区中的支付问题 。
 
Airbnb is a global brand operating in over 190 countries and 40 currencies around the world, including markets that are hard to reach and not commonly supported in the payments ecosystem. A singular processor relationship to move money globally simply does not exist in today’s world. As a result, Airbnb integrates with a handful of gateways and dozens of processors to achieve global coverage and payments redundancy. This system includes processors with varying degrees of maturity in their systems, different modes of integration (API call, batch and URL redirect), and widely differing transaction settlement periods. For example, Airbnb supports the processing of completely synchronous payment methods, such as major credit/debit card networks, as well as payment methods that can take up to several days to settle, such as Brazilian Boletos.

Airbnb是一个在全球运营超过190个国家和40种货币的全球性品牌，包括难以触及到的和在支付生态中通常不受支付的市场。当社会，在全球范围内转移资金依靠单一处理器是不可能的。因此，Airbnb整合了一些网关和数十个处理器以便于处理全球覆盖和支付冗余。这个系统包括了在其系统内有着不同程度完善的处理器，不同模式的整合（API调用，批处理，网址重定向），以及非常大的交易结算周期。举个例子，Airbnb支持完全同步支付方法的处理，比如，主要的信用卡/借记卡网络，以及像巴西的银行转账付款方式 那样，能够用几天时间来解决的支付方式。

Diagram Depicting Various Payment Integration Methods At Airbnb
图表描述了在Airbnb不同的支付整合方法

For a traditional online merchant, money flows into the system as a result of online purchases. More recently however, with the sharing economy on the rise, we have started to witness an increase in two-sided marketplaces. Airbnb is one such example — connecting travelers and hosts worldwide. As a result, Payments at Airbnb handles bi-directional money flow, not only handling payments into our platform, but also all payment outflows to hosts.

对于一个传统的在线商人来说，由于线上采购，资金流进了这个系统。然而最近，随着共享经济的发展，我们开始见证了双边市场的增长。Airbnb是其中一个例子——连接全球的游客和屋主。所以，Airbnb的支付解决了资金的双向流动，不仅解决了由支付资金到我们的平台，而且也解决了所有的支付资金流向屋主。


A large portion of the money flowing out to our hosts occurs with direct integration with banks via batch processing. Batch transaction processing involves two steps. First, we send a collection of transaction requests to a bank in a compliant format. We then process the response file(s) that bank sends us back containing responses to the transaction requests. While batch processes are suitable for larger transaction volume processing, the batching process, by nature, leaves our systems out-of-sync until batch response files are processed. Because this process can take up to several hours to complete, batch processing can prove to be a difficult barrier in maintaining transactional integrity.

流向我们的屋主的最大一部分的资金通过是批处理与银行直接整合。批次交易处理分两步实现。首先，我们以一种兼容的格式发送一些交易请求给银行。接着我们会处理这些响应文件，这些文件是银行向我们发送回包含交易请求的响应。虽然批处理适用于更大量的交易处理，批处理进程，本质上，使得我们的系统不同步直到处理批响应文件。因为这个过程需要几个小时才能完成，在保持交易完整性时，批处理或许是一个障碍。

Even in the most simple example of an Airbnb transaction, a reservation between guest and host, there are at least two financial transactions associated with it. The first financial transaction occurs when the guest pays for the reservation to Airbnb, and the second occurs when Airbnb pays the host within hours of rendering the service. However, travel plans change more often than we think. Additional payment features such as alterations, deposits/installments, group travel, tax withholding, VAT etc. all dramatically increase the number and complexity of financial transactions associated with a reservation. Additionally, the fact that many reservations on Airbnb platform are cross border, involving multiple currencies, further increases the complexity of our money movements.

即使在Airbnb交易最简单的例子中，在客人与屋主之间的预约，至少有两个相关的金融交易。第一个金融交易发生在当客人向Airbnb支付预定时，和第二个则发生在Airbnb在提供服务的几小时内支付给屋主的交易。然而，旅行计划常常会有变化。额外支付功能，比如，变更，存款/分期付款，跟团旅游，预扣税款，增值税，等等。所有这些都大大增加了与预约相关的金融交易的数量和复杂性。此外，事实上许多在Airbnb平台上的预约是跨境的，涉及到多种货币，进一步增加我们资金流动的复杂性。

Airbnb’s “New” Payment Gateway
Airbnb“新“的支付网关

Airbnb has seen an explosive growth in its marketplace in recent years, with payments being a critical underpinning of the expansion. Until recent years, most of Airbnb’s business and financial transaction logic was performed in a monolithic rails application.

近年来，随着支付成为扩张的一个支柱，Airbnb 在其市场上出现了爆炸式的增长。直到近来，大部分的Airbnb商家和金融交易都是在一个整体式轨道应用程序中实现（这个翻译不太确定！望指正！）

To improve scalability, Airbnb is making a significant investment in Service Oriented Architecture. As part of this strategy, we set out to build an internal payment gateway to encapsulate all network communication to/from various processors and handle the “burden” of executing money movements for the application. Our new payment gateway is a Java Service with a dedicated datastore. This datastore hosts various payment methods and serves as a system of records for financial transactions.

为了提高扩展性，Airbnb在面向服务架构中做了重大投资。作为战略的一部分，我们开始建立一个内部支付网关，将所有的网络通信封装到所有双向不同处理器上去，以及，处理为应用程序在执行资金流动时造成的“负担”。我们新的支付网关是一个带有专用数据储存的Java服务。这个数据储存托管各种不同的支付方法和为金融交易的记录系统提供服务。

This new service represents two distinct challenges with transactional integrity. First, an additional internal gateway increases the number of hops made during payment execution and if it behaves in an inconsistent manner or fails to process a gateway or processor response, it will create an “out-of-sync” transaction. Secondly, while we ramp up traffic on the new payment gateway there will be two transaction stores with Airbnb internal system — a legacy transaction store and a new payment gateway transaction store. We cannot afford any drift in consistency within our two data stores for any significant amount of time. These two challenges warranted additional consideration for transactional integrity.

这个新的服务代表了交易完整性的两种不同的挑战。首先，一个额外的内部网关增加了在支付执行中跳转的数量，以及，假如其行为不一致，或者处理网关或处理响应时失败，其将来建立一个“不同步“的交易。第二，当我们在新的支付网关上增加流量时，Airbnb内部系统将会有两个交易储存——遗留的交易储存和一个新的支付网关交易储存。在我们两个数据储存中，我们不能在大量时间内保持漂移的一致性（是否翻译错误？）。对于交易完整性而言，需要额外考虑这两个挑战。

What Is An “out-of-sync” Transaction?
什么是“不同步“交易？

If any system in the payments processing chain fails to respond and/or its subsequent system fails to properly consume the response of money movement it creates an “out-of-sync” transaction. Additionally, incorrect treatment of API responses by any entity in the chain can lead to “out-of-sync” transactions.

假如任何系统在支付处理链中，不能响应或者其后续机制不能正确地读取资金流动的响应，它就会建立一个“不同步”的交易。另外，在处理链中的任何实体对API响应的不正确处理都能导致“不同步”交易的发生。

Diagram Depicting Various Entities Payment Must Be Passed Through During The Payment Process 
描述各种实体交易的图表必须在支付过程中得到验证

Introduction to Solution 从入门到解决方案

Measuring transactional integrity across this maze of distributed systems in a timely fashion, and subsequently detecting and responding to any anomalies is a challenge. Using many systems of varying maturity makes it nearly impossible to instantaneously track a transaction through various states in different systems — platform, payment gateway, payment processor, etc. — at all times.

及时地在错踪复杂的分布式系统中测量交易的完整性，以及其后侦测和响应到异常是一个挑战。在不同的系统中的不同的状态，在任何时候，使用许多不同成熟度的系统几乎不可能跟踪交易，这些系统包括平台，支付网关，支付处理器等。

One potential solution that comes to mind is to use payment gateway APIs for transaction record comparison. The downside to this approach is that it’s harder to scale the comparison between internal transaction stores and the external transaction stores using APIs. In fact, some external entities do not even offer this information via API. Furthermore, a dedicated comparison system may be needed to execute API calls to the entity for transactional analysis to avoid any potential impact on live traffic, a scenario which most web-services cannot tolerate.

一个潜在的解决方案就是，对交易记录比较来说，使用支付网关APIs.这种方法的缺点是比较难以在使用APIs时，在内部交易储存和外部交易储存中进行比较。事实上，一些外部实体不能通过API提供信息。此外，也许需要一个专用的比较系统来执行API调用，对实体进行交易分析，以避免对实时流量产生潜在的影响。

Airbnb’s Processor Transaction Reporting System
Airbnb的处理器交易报告系统


Almost all processors and gateways offers transaction reports and details to the merchant via secure file transfer protocol (SFTP). These transaction reports are offered as part of settlement to the merchant within an agreed upon SLA. Typically, merchants reconcile all the money movement on the platform via 3-way reconciliation between platform transaction records, processor settlement files, and bank statements. Airbnb has a dedicated service that imports, extracts and exports every processor file. In detail, the service:

几乎所有的处理器和网关都提供交易报告和通过安全文件传输协议（SFTP）的商家细节。这些交易报告是在约定SLA中，提供给商家作为结算的一部分。通常，在平台上商家会通过三种方式来协调所有的资金流动，平台交易记录，处理器结算文件和银行对帐单。Airbnb有专用的可以导入，提取和导出每个处理器文件的服务器，详细的服务：

•	Imports daily transaction reports from our payment partners to S3,
•	从我们的支付合作伙伴到S3调入每日交易报告，
•	
•	Extracts reports into report-specific staging tables,
•	把报告提取在特定报告数据库中，

•	Exports these reports in a consistent format.
•	在一致的格式中导出这些报导。

Additionally, it has logic to detect duplicate files and avoid repeated processing of the same file. Many processors/gateways offer these reports in CSV format and every entity uses their own vocabulary in the reports. These transaction details are then stored in a datastore and as part of the export they are sent for reconciliation.

此外，它具有检测重复文件的逻辑和避免重复处理相同的文件。许多处理器/网关用CSV格式提供这些报告，以及每个实体都在报告中使用自己的词汇。这些交易细节在数据库中存储起来，其中一些被导出用于对账。

Diagram Depicting Overall TI Pipeline
描述所有TI管道的图表

Airbnb’s system uses a set of scheduled jobs to execute these activities on a dedicated server. While this system does not have access to Airbnb’s platform transactions, we are able to use additional tooling to combine these data sources to trace transactions end-to-end.

Airbnb的系统使用一组预定工作来执行这些在专用服务器上的活动。虽然这个系统没有访问Airbnb的平台交易，我们可以使用额外的工具联合这些数据来源以跟踪交易全程。

Snapshots of every database are available in HDFS at regular intervals. This makes it possible to trace a transaction throughout the ecosystem without leaving your network boundary. It additionally means this is an IO bound problem. Big data technologies are well suited for large scale data comparison problems via map-reduce.

Snapshots的每个数据都可以定期在HDFS(分布式文件系统)中使用。这使得在整个生态系统中，不需要离开你的网络边界，使跟踪交易成为可能。另外这也意味着这是IO约束问题。大数据技术通过多核处理，非常适合与大规模数据比较问题匹配。

Comprehensive Solution 全面解决方案

As a result, we decided to approach this problem in a segmented format, dividing all payment activity into four broad categories — payments, refunds, payouts, and returns. Each category has a dedicated hive pipeline to generate transaction-level reports for all transactions, from all payment entities, both internal and external, within the category. This gives the ability to understand trends within and across the categories. By computing a moving average of these combined categories we’re able to produce a single number representing transactional integrity within Airbnb’s payment eco-system. This number allows us to measure improvements, detect issues, and set an overall goal.

因此，我们决定用分段格式解决这个问题。将支付活动分成四大类——支付，退款，付款和收益。每一个分类有专业的储备管道，为所有的交易生成交易级报告。这样就能理解在分类里和跨分类中的趋势。通过计算这些组合分类的移动平均数，我们能够在Airbnb的支付生态系统中生成代表交易完整性的单个数字。这个数字能让我们测量得到改进，检测到问题以及设置一个总体目标。

With this approach, we are able to trace every transaction at every stage since day zero (the day when Airbnb’s Payment Gateway started receiving significant traffic) at regular intervals. Once a normalized view of all the transactions within a category is created, transactions with anomalies can be further grouped based on the type of aberration found. 
The anomaly attribution can then be directly tied to a business use case such as delayed payouts or mismatched transaction attributes. And because transactions with similar anomalies often require similar remediation of data and/or code fixes, these error groupings help us prioritize our efforts for fixes.

用这个方法，我们能每隔一段时间，跟踪自第0天（这一天，Airbnb的支付网关开始接受大量流量）以来每个阶段每个的交易。一旦一个分类内的所有交易的规范化视图被创建，基于发现的异常类型可以进一步对异常交易进行分组。这个异常属性可以直接绑定到业务用例，比如延迟支付或者不配对的交易属性。因为类似的异常交易经常要求相似的数据修复或代码修复，这些错误的分组将帮助我们对修复工作进行优先组排序。

Chart Depicting Categorical Transactional Integrity Status描述分类交易完整性状态的图表

Payment Reconciliation Using Traditional Tools Is Not Sufficient
使用传统工具进行核对款项是不够的


Transactional integrity initiatives differ from the payment reconciliation process in many aspects. Transactional integrity measurements require comparing each and every transaction from day zero (the hypothetically defined start date for a system), while reconciliation often leaves out already matched transactions. Integrity measurement techniques can also be expanded to any number of systems holding transaction records — this can include much more than one internal/external system.

 在很多方面，交易完整性计划不同于核对款项进程。交易的完整性测量要求比较从0天（系统假设的开始日期）开始的单一个或者每一个交易，而对帐往往遗漏了已经核对的交易。完整性测量技术也能够扩展到任何数量的持有交易记录的系统——这可以包括多个内部/外部的系统。

Often payment reconciliation is done by matching platform and processor transactions alone. This precludes crucial links where transactional integrity could break down, since it does not offer comparison results for each stage of the process, but only provides an overall picture. Payment reconciliation is done by matching a set of identifying attributes as opposed to executing a deep comparison between two models. Many third party reconciliation technologies we analyzed focused on accounting aspects and provided process management toolkits, but these features don’t contribute to transactional integrity directly.

 通常核对款项是通过用匹配平台和服务器单独交易来完成的。这排除了交易完整性可能崩溃的关键环节，因为它没有提供过程的每一个阶段的比较结果，而提供了一个整体的情况。核对款项是通过匹配一组识别属性来完成的，而不是在两个模型中进行深度比较 。
我们分析的许多第三方的对帐技术，集中在会计方面和提供流程管理工具包，但这些特色并不能直接对交易的完整性有促进作用。

In addition, payment reconciliation is only done when money exchanges hands. However, there are a few transaction types — such as voiding an authorization — where money never moves, but that still may have an impact on our customers and/or system performance. To maintain a healthy payment system, it’s important to compare all types of transactions as opposed to a subset.

此外，核对款项是当资金交易时才能进行。然而，也有少量的交易类型——比如无效授权——资金并没有转移，但是对我们的客户和/或者系统性能依然产生影响。为了维护健康的支付系统，与子集相反，比较所有交易类型是很重要的。

Big Data Toolkit — Hive, Hadoop, HDFS, Airflow And S3
大数据工具包——数据仓库，分布式系统基础架构，分布式文件系统，工作流分配管理系统和简单存储服务


Hive has the ability to compare different transaction models represented by different schemas directly with minimal interpretation and without needing any additional integration. This gives us the ability to quickly iterate on the solution.

数据仓库可以比较不同代表的交易模型，通过不同的模式直接用最小的解释以及不需要任何额外的集成。这给了我们快速迭代解决方案的能力。

Hive offers a SQL like interface to query the data, but its biggest strength is to effectively execute map-and-reduce jobs at scale. With this, it’s possible to compare each and every transaction throughout the ecosystem on various transaction attributes of interest — amount, currency, instrument used, transaction code, status, etc.

 数据仓库提供一个类似SQL的接口来查询数据，但它最大的优点就是在范围内有效执行映射—归约工作。有了它，就可以比较在整个生态系统中某一个或者每一个交易，在各种交易属性上——金额，货币，使用的工具，交易代码，状态等等。

Hadoop also offers scalability, with its MapReduce mechanism that is suited well for comparison between large and growing datasets. HDFS gives us the ability to snapshot transactional integrity at regular intervals to produce a meaningful trend over time. Amazon S3 offers a cost effective datastore for archive purposes and works effectively with our big data toolkit.

分布式系统基础架构也能提供可扩展性,它的映射归约机制适合在大型和增长的数据集中进行比较。分布式文件系统使我们在每隔一段时间内捕捉到交易完整性，随着时间的流逝，从而产生一种有意义的趋势。简单存储服务为归档提供了一个有效成本数据储存，以及用我们的大数据工具包有效地工作。

Airflow, an Airbnb developed service, offers a scheduling tool to orchestrate data operations, allowing us to execute various steps of intermediate computation and produce a transaction level report.

工作流分配管理系统——Airbnb开发服务——提供一个调度工具以便协调数据操作，允许执行中间计算的各个步骤和生成交易级别报告。

Monitoring Transactional Integrity (Druid, Superset, And Automated Reporting)
监测交易完整性（开源数据存储系统，超集和自动报告）


Dashboards and automated reporting are critical to improve organizational awareness of system issues and to provide tools to measure performance over time. Without clear reporting, it is often difficult and time consuming to identify customer and business impacting events in a timely fashion.

指示板和自动报告对于提供系统问题的组织意识和在超时时提供工具测量性能是重要的。没有明确的报告，识别客户和业务对事件的影响是困难和耗时的。

At Airbnb, we are able to take advantage of an OLAP system built on top of Druid, a low latency, distributed data store, to ingest our transaction pipelines and interactively explore big data in a scalable fashion. Utilizing Superset as a dashboard tool, we are able to display constantly up-to-date measures of health for the entire payment system. With robust payment categorizations built into the our reporting tools, we are able to see the progress of various system issues over time, and in certain cases, these trends help us manage a long tail of historical issues by dealing with them in a scalable way.

在Airbnb，我们能够利用OLAP（,On-LineAnalysis Processing在线分析处理)系统建立数据库连接池（Druid），一个低延时，分布式数据储存，以获取我们的交易管道以及以可扩展的方式交互式地搜索大数据。利用超集作为一个指示板工具，我们可以对整个支付系统随时显时最新的有效测量。用稳健的支付分类建立了我们的报告工具，我们可以观察到各种系统问题在超时下的进展，以及在某些特定情况下，这些趋势能够帮助我们以一种扩展的方式去管理历史问题的长尾效应。

Chart Depicting Anomaly Detection Example
描述异常检测例子的图表

Anomaly detection is another critical outcome of our transactional integrity projects. Our OLAP system allows us to easily configure anomaly detection algorithms across various dimensional cuts, enabling automatic email/slack notification of any anomalies shortly after our data pipelines land.

异常检测是另一种我们的交易完整性项目重要的结果。我们的在线分析处理（DLAP）系统能使我们通过削减不同的维度，轻易地配置异常检测算法，在我们的数据管道着陆后，开启电子邮件/任何异常的及时通知。

Key Wins Of Transactional Integrity Analysis
交易完整性分析的关键胜利

Transactional integrity analysis has helped us identify and size issues ranging from simple integration bugs to more nuanced edge-cases around eventual consistency. It has also helped us fine tune various system parameters such as socket timeouts, error handling, and retry mechanisms

交易完整性分析能帮助我们识别和处理问题，范围从单一集成漏洞到最终一致性的更微妙的边际情况。这也能帮助我们微调各种系统参数，比如，套接字超时（socket timeouts），错误处理和重试机制。

With the help of transactional integrity data and auxiliary tools, it is easier to proactively synchronize out-of-sync transactions before they impact our community. This has not only dramatically cut down high-volume, low-complexity support tickets, but it has led to large improvements in payment reconciliation, leading to more accurate financial reporting and streamlined operations.

在交易完整性数据和辅助仪表的帮助下，在它们影响我们社区前，主动同步那些不同步交易较为容易。这不仅极大地减少大容量，低复杂度的支付票（是否翻译正确？），而且对核对款项有了巨大的改善，导致更准确的金融报告和简化操作。

Deep data analysis has also helped us easily monitor and understand processor issues and misbehavior. And building robust alerting on top of our analytical frameworks has enabled our system to proactively notify us about processor outages in online transaction processing or missing/out-of-SLA transaction reports.

深度数据分析也能帮助我们较容易地监视和理解处理问题和不当行为。以及在我们的分析框架上能建立强有力的警报，能使我们的系统能够主动通知我们关于处理在线上交易处理或者丢失/中断服务水平协议的交易报告。

Future Looking 未来的展望


While we have made much progress monitoring and improving our transactional integrity figures to date, there is still much work to be completed in the future to achieve unblemished transactional integrity.

到目前为止，尽管我们在监测和改进我们的交易完整性取得了较大进展的时候，在未来实现完美的交易完整性方面，我们依然还有很多工作要做。

Today at Airbnb, our system is designed to achieve “eventual consistency” through automatic retries of failed transactions. Retries address cases where our system does not hear back from downstream processing entities within allowed timeframe due to timeouts, transient system issues, loss of network connectivity, outages, etc. This gives our system the ability to get back “in-sync” with our processing partners.

在今天的Airbnb，我们的系统通过失败交易的自动重试功能来实现“最终的一致性”。重试解决方案是由于超时，瞬间系统问题，失去网络连接，运行中断，等等，我们的系统在允许的范围内不能从下游加工实体中得到反馈。这使我们的系统能够与我们的合作伙伴恢复“同步”。

However, safely retrying a payment request requires a strong idempotence guarantee to execute one and only one money movement, which is hard to achieve for every possible use case. With transactional integrity analysis, we have gained insight on how to deal with various edge-case scenarios that break our idempotency guarantee, and we are in the middle of redesigning our framework to achieve near perfect idempotency.

然而，安全地重试付款请求要求一个强有力的冥等性保证（是否翻译错误？）来执行一个或者仅一个的资金流动，这对每个可能可用例子都非常困难。随着交易完整性的分析，我们知道如何解决各种边际问题的情景，从而打破我们的幂等性保证，以及我们在重新设计我们的框架中达到近乎完美的幂等性。

Furthermore, to trace transactions effectively throughout our distributed system, a normalized transaction taxonomy across processors is required. Computation of transactional integrity requires all data to be available in Hive. An event-driven approach can help us address both these problems elegantly, by designing a normalized settlement event schema that each processor record system can use to share its activity. 

此外，通过我们的分布系统有效的跟踪交易，需要跨处理器的交易分类规范化。交易完整性的计算要求在数据仓库内提供所有的数据。事件驱动方法能帮助我们优雅地解决这两个问题，通过设计一个规范化的处理事件模式，这个模式是每一个处理器记录系统都能分享它的活动的。

These settlement vents can optionally be decorated with gateway information, and can be captured in our data warehouse to allow transactional integrity measurement and analysis. They can also be consumed by our online transaction system, or be streamed into tools that create tickets, offer real-time analysis, and automatically repair underlying transactions to achieve consistency.

这些处理事件能选择性地用网关信息进行布置，以及能够被我们的数据库存捕获，允许进行交易完整性的测试和分析。它们也通过我们的在线交易系统被消耗，或者被导入到创建门票的工具中，提供实时分析，自动修复底层交易以完成交易的一致性。

If you’re interested in working on the intricacies of a distributed payment system, or adding an additional “9” to our transactional integrity numbers, which can even require rebuilding certain parts of our payments system, Airbnb is hiring!

假如你有兴趣为纷繁复杂的分布式支付系统工作，或者为我们的交易完整性数字增加一个“9”，甚至需要重建我们的支付系统的某些部分，Airbnb欢迎你！

Big shout out to Lou Kosak for his thought leadership and prototype. Many thanks to Sam Wyman, Alice Liang, Khaled Hussein, Brian Wey, and Cynthia Adams for their generous contributions.

非常感谢Lou Kosak的领导力和标准。也非常感谢Sam Wyman, Alice Liang, Khaled Hussein, Brian Wey, and Cynthia Adams慷慨的帮助。

