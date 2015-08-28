The Reactive Manifesto
----------------------

响应式宣言
---------

Organisations working in disparate domains are independently discovering patterns for building software that look the same. These systems are more robust, more resilient, more flexible and better positioned to meet modern demands. 

有许多组织在各自完全不同的领域中工作，他们都正在独立地发现看起来相同的软件构建模式。他们的系统更加稳健，更加具有弹性，更加灵活，并且以更好的定位来满足当代（现代？）的需求。

These changes are happening because application requirements have changed dramatically in recent years. Only a few years ago a large application had tens of servers, seconds of response time, hours of offline maintenance and gigabytes of data. Today applications are deployed on everything from mobile devices to cloud-based clusters running thousands of multi-core processors. Users expect millisecond response times and 100% uptime. Data is measured in Petabytes. Today's demands are simply not met by yesterday’s software architectures.

近几年的应用需求已经发生了戏剧性的变化，因而许多改变正在发生。仅仅在几年之前，大量的应用拥有数十台服务器，数秒的响应时间，数小时的离线维护时间以及许多 GB 的数据。而今天，应用被部署在一切场合，从移动设备到基于云的，运行在数以千计的多核心处理器的集群之上。用户期望毫秒级的响应以及 100% 的正常运行时间。数据以 PB 为单位来衡量。昨天的软件架构已经无法简单地满足今天的需求。

We believe that a coherent approach to systems architecture is needed, and we believe that all necessary aspects are already recognised individually: we want systems that are Responsive, Resilient, Elastic and Message Driven. We call these Reactive Systems.

我们相信，一种条理分明的系统架构方法是必要的，而且我们相信所有这些必要的方面已经得到单独的确认：我们期望系统是响应式的，具有弹性的，可伸缩的，以及以消息驱动。我们将其称之为响应式系统。

Systems built as Reactive Systems are more flexible, loosely-coupled and [scalable](/glossary#Scalability). This makes them easier to develop and amenable to change. They are significantly more tolerant of [failure](/glossary#Failure) and when failure does occur they meet it with elegance rather than disaster. Reactive Systems are highly responsive, giving [users](/glossary#User) effective interactive feedback. 

以响应式方式构建的系统更加灵活，更加松耦合以及更加[可扩展](/glossary#Scalability). 这使得它们更容易被开发以及经得起变化的考验。值得注意的是，他们对于[故障](/glossary#Failure)更加宽容，并且当故障发生时，他们优雅地去面对，而不是出现灾难。响应式系统是高度灵敏的，给予[用户](/glossary#User)有效的互动反馈。

*Reactive Systems are:*

*响应式系统是：*

* <a name="Responsive"></a>**Responsive**: The [system](/glossary#System) responds in a timely manner if at all possible. Responsiveness is the cornerstone of usability and utility, but more than that, responsiveness means that problems may be detected quickly and dealt with effectively. Responsive systems focus on providing rapid and consistent response times, establishing reliable upper bounds so they deliver a consistent quality of service. This consistent behaviour in turn simplifies error handling, builds end user confidence, and encourages further interaction. 

* <a name="Responsive"></a>**灵敏的**:

* <a name="Resilient"></a>**Resilient**: The system stays responsive in the face of [failure](/glossary#Failure). This applies not only to highly-available, mission critical systems — any system that is not resilient will be unresponsive after a failure. Resilience is achieved by [replication](/glossary#Replication), containment, [isolation](/glossary#Isolation) and [delegation](/glossary#Delegation). Failures are contained within each [component](/glossary#Component), isolating components from each other and thereby ensuring that parts of the system can fail and recover without compromising the system as a whole. Recovery of each component is delegated to another (external) component and high-availability is ensured by replication where necessary. The client of a component is not burdened with handling its failures.

* <a name="Resilient"></a>**有弹性的**:

* <a name="Elastic"></a>**Elastic**: The system stays responsive under varying workload. Reactive Systems can react to changes in the input rate by increasing or decreasing the [resources](/glossary#Resource) allocated to service these inputs. This implies designs that have no contention points or central bottlenecks, resulting in the ability to shard or replicate components and distribute inputs among them. Reactive Systems support predictive, as well as Reactive, scaling algorithms by providing relevant live performance measures. They achieve [elasticity](/glossary#Elasticity) in a cost-effective way on commodity hardware and software platforms.

* <a name="Elastic"></a>**可伸缩的**:

* <a name="Message-Driven"></a>**Message Driven**: Reactive Systems rely on [asynchronous](/glossary#Asynchronous) [message-passing](/glossary#Message-Driven) to establish a boundary between components that ensures loose coupling, isolation, [location transparency](/glossary#Location-Transparency), and provides the means to delegate [errors](/glossary#Failure) as messages. Employing explicit message-passing enables load management, elasticity, and flow control by shaping and monitoring the message queues in the system and applying [back-pressure](/glossary#Back-Pressure) when necessary. Location transparent messaging as a means of communication makes it possible for the management of failure to work with the same constructs and semantics across a cluster or within a single host. [Non-blocking](/glossary#Non-Blocking) communication allows recipients to only consume [resources](/glossary#Resource) while active, leading to less system overhead.

* <a name="Message-Driven"></a>**消息驱动的**:

Large systems are composed of smaller ones and therefore depend on the Reactive properties of their constituents. This means that Reactive Systems apply design principles so these properties apply at all levels of scale, making them composable. The largest systems in the world rely upon architectures based on these properties and serve the needs of billions of people daily. It is time to apply these design principles consciously from the start instead of rediscovering them each time.

大系统由较小的部分构成，因此依赖于他们的构成要素中的响应式属性。这意味着响应式系统在所有层级规模上运用着这些属性运用的设计原则，从而使之可以组合。世界上最大的系统依赖着基于这些属性的体系结构，并且应对着数十亿人的日常需求。现在是时候有意识地从头运用这些设计原则而不是每次重新发现他们了。（这句话好没力度）

[Sign the manifesto](http://www.reactivemanifesto.org/#sign-button)
