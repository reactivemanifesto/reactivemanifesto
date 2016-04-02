反应式宣言
----------------------

各组织独立工作在不同的领域，他们都在为构建相似的软件探索一种模式。这些系统更健壮、更容易恢复、更灵活，可以更好的满足现代需求。

由于应用需求的变化，近几年发生了很大变化。仅仅几年前，一个大型的应用拥有数十台服务器，秒级的响应时间，小时级离线维护时间和千兆级的数据。 
今天的应用可以被部署到任何机器上，从移动设备到基于云计算的运行在数以千计的多核处理器上的集群。用户期待毫秒级的响应时间和100%的运行时间。
数据使用单位PB计量。使用过去的软件架构是无法满足现在的简单需求。

我们认为需要一整套系统架构方法，我们也认为所有必要的方面已经被分别的确认：我们希望系统是可响应、可恢复、可伸缩和消息驱动。我们称之为反应式系统。

系统构建成一个反应式系统，将更加的灵活、松散耦合和[可扩展](/glossary#Scalability)。这使得开发和定制更加容易。它们对[失败](/glossary#Failure)有明显
的容忍，当失败发生时，它们面对的是优雅而不是灾难。反应式系统具有高反应度，提供给[用户](/glossary#User)有效的互动反馈。

*反应式系统是：*

* <a name="Responsive"></a>**即时响应**：该系统尽可能的及时做出回应。灵敏度是可用和实用的基石，不仅如此，灵敏度意味着问题可以被快速检测并获得有效的处理。
反应式系统的重点在于提供迅速和一致的响应时间，建立可靠的上限，因此他们交付一致的质量的服务。这种一致的行为反过来简化了错误的处理，建立了终端用户的信心和鼓励了进一步的互动。

* <a name="Resilient"></a>**可恢复**:该系统在[失败](/glossary#Failure)面前依然保持可响应。
这个应用不仅对高可用的，关键任务系统 -任何不能恢复的系统将在失败后无响应。 
可恢复是通过[同步复制](/glossary#Replication)、包含、[隔离](/glossary#Isolation)和[委派](/glossary#Delegation)。
失败可能包含在各个[组件](/glossary#Component)，隔离其他不同的组件从而确系统的一部分可以失败和恢复，除了整个系统被损坏。

 * <a name="Elastic"></a>**弹性**:该系统在变化的负载情况下依然保持可响应。反应式系统对输入率的变化做出反应，通过增加或减少向服务
  分配的[资源](/glossary#Resource)应对这些输入。这样的实现设计没有竞争点或中央瓶颈，导致分片能力或组件同步复制和分发输入给它们。反应
  式系统支持预测，以及反应，伸缩算法通过提供有关可用性能措施。他们实现伸缩在具有成本效益的商品在硬件和软件平台。
 
 
* <a name="Message-Driven"></a>**Message Driven**: 
 Reactive Systems rely on [asynchronous](/glossary#Asynchronous) [message-passing](/glossary#Message-Driven) to establish
 a boundary between components that ensures loose coupling, isolation, 
 and [location transparency](/glossary#Location-Transparency). This boundary also provides the means to 
 delegate [failures](/glossary#Failure) as messages. Employing explicit message-passing enables load management, 
 elasticity, and flow control by shaping and monitoring the message queues in the system and 
 applying [back-pressure](/glossary#Back-Pressure) when necessary. Location transparent messaging as a means of 
 communication makes it possible for the management of failure to work with the same constructs and semantics 
 across a cluster or within a single host. [Non-blocking](/glossary#Non-Blocking) communication allows recipients 
 to only consume [resources](/glossary#Resource) while active, leading to less system overhead.

Large systems are composed of smaller ones and therefore depend on the Reactive properties of their constituents. 
This means that Reactive Systems apply design principles so these properties apply at all levels of scale, 
making them composable. The largest systems in the world rely upon architectures based on these properties and serve 
the needs of billions of people daily. It is time to apply these design principles consciously from the start 
instead of rediscovering them each time.


[Sign the manifesto](http://www.reactivemanifesto.org/#sign-button)
[签署宣言](http://www.reactivemanifesto.org/#sign-button)
