反应式宣言
----------------------

各组织独立工作在不同的领域，他们都在为构建相似的软件探索一种模式。这些系统更健壮、更容易恢复、更灵活，可以更好的满足现代需求。

由于近几年应用需求的巨大变化，一些变革正在发生着。仅在几年前，一个大型的应用拥有数十台服务器，秒级的响应时间、数小时的离线维护时间和千兆级的数据。 
今天的应用可以被部署到任何机器上，从移动设备到基于云计算的运行在数以千计的多核处理器上的集群。用户期待毫秒级的响应时间和100%的上线时间。
数据使用单位PB计量。仅使用过去的软件架构是无法满足现在的需求。

我们认为需要一整套系统架构方法，也认为所有必要的方面已经被单独的确认：我们希望系统是可响应的、可恢复的、可伸缩的和消息驱动的。我们称之为反应式系统。

系统被构建为一个反应式系统，将会更加的灵活、松散耦合和[可伸缩](http://www.reactivemanifesto.org/glossary#Scalability)。
这使得开发和定制更加容易。它们对[失败](http://www.reactivemanifesto.org/glossary#Failure)有着明显的容忍，当失败发生时，它们面对的是
优雅而不是灾难。反应式系统具有高反应度，提供给[用户](http://www.reactivemanifesto.org/glossary#User)有效的互动反馈。

*反应式系统是：*

* <a name="Responsive"></a>**可响应**：该系统尽可能的及时做出回应。可响应性是可用和实用的基石，不仅如此，可响应性意味着问题可以快速被监测并获得
有效的处理。反应式系统的重点在于提供迅速和稳定的响应时间，建立可靠的上限，以便于交付质量稳定的服务。这种稳定的表现促使错误的处理的简化，建立了终端用户
的信心并鼓励进一步的互动。

* <a name="Resilient"></a>**可恢复**:该系统在[失败](http://www.reactivemanifesto.org/glossary#Failure)面前依然保持可响应。
这个应用不仅对高可用的，关键任务系统 -任何不能恢复的系统将在失败后无响应。
可恢复的实现是通过[同步复制](http://www.reactivemanifesto.org/glossary#Replication)、容器化、[隔离](http://www.reactivemanifesto.org/glossary#Isolation)和[委派](http://www.reactivemanifesto.org/glossary#Delegation)。
失败可能包含在各个[组件](http://www.reactivemanifesto.org/glossary#Component)，隔离其他的组件从而确保系统的一部分可以失败和恢复，除非
整个系统被损坏。

* <a name="Elastic"></a>**可伸缩**:该系统在负载波动的情况下依然保持可响应。反应式系统对输入率的变化做出反应，通过增加或减少向服务分配的[资源](http://www.reactivemanifesto.org/glossary#Resource)
 以应对这些输入。这样的实现设计没有竞争点或中央瓶颈，产生分片或组件同步复制的能力并分发输入给各组件。反应式系统支持预测，以及反应，伸缩算法通过提供有关可用性能措施。
 他们采用具有成本效益的商品硬件和软件平台方的式实现伸缩。
 
* <a name="Message-Driven"></a>**消息驱动**:反应式系统依赖[异步](http://www.reactivemanifesto.org/glossary#Asynchronous)的[消息传递](http://www.reactivemanifesto.org/glossary#Message-Driven)来界定
 组件之间的边界，确保松散耦合、隔离和[位置透明](http://www.reactivemanifesto.org/glossary#Location-Transparency)。
 这个边界也提供了委派[失败](http://www.reactivemanifesto.org/glossary#Failure)作为消息的方式。
 使用透明的消息传递启用负载管理，伸缩，通过在系统建立和监控消息队列实现流量控制，当需要的时候启用[背压](http://www.reactivemanifesto.org/glossary#Back-Pressure)。
 位置透明的消息传递是一种通信方式，可以在跨集群或单个主机内部使用相同的结构和语义管理失败。
[非阻塞](http://www.reactivemanifesto.org/glossary#Non-Blocking) 通信允许接收请求，只有在活动期间消耗[资源](http://www.reactivemanifesto.org/glossary#Resource)，导致了更少的系统消耗。
 
大型系统是由更小的系统和组成部分所依赖的反应式特性。这意味着反应式系统使用设计原则以便这些特性应用到伸缩的任何水平，使各部分组合在一起。
世界上最大的系统为每天数以万亿的人们的需求提供服务，系统依赖的架构是基于这些特性的。

[签署宣言](http://www.reactivemanifesto.org/#sign-button)
