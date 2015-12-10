响应式宣言
---------

有许多组织在各自完全不同的领域中工作，他们都正在独立地发现看起来相同的软件构建模式。他们的系统更加稳健，更加有可回复性，更加灵活，并且以更好的定位来满足现代的需求。

上述变化之所以会发生，是因为近几年的应用需求已经发生了戏剧性的变化。仅仅在几年之前，大型应用通常拥有数十台服务器，数秒的响应时间，数小时的离线维护时间以及许多 GB 的数据。而今天，应用被部署在一切场合，从移动设备到基于云的集群，这些集群运行在数以千计的多核心处理器的之上。用户期望毫秒级的响应时间以及 100% 的正常运行时间。数据则以 PB 为单位来衡量。昨天的软件架构已经完全无法地满足今天的需求。

我们相信，一种条理分明的系统架构方法是必要的，而且我们相信关于这种方法的所有必要方面已经逐一地被人们认识到：我们需要的系统是响应式的，具有可回复性的，可伸缩的，以及以消息驱动的。我们将这样的系统之为响应式系统。

以响应式系统方式构建的系统更加灵活，松耦合和[可扩展](/glossary#可扩展性)。这使得它们更容易被开发，而且经得起变化的考验。它们对于系统[失败](/glossary#失败)表现出显著的包容性，并且当失败真的发生时，他们用优雅的方式去应对，而不是放任灾难的发生。响应式系统是高度灵敏的，能够给[用户](/glossary#用户)以有效的交互式的反馈。

**响应式系统是：**

* <a name="Responsive"></a>**灵敏的**：只要有可能，[系统](/glossary#系统)就会及时响应。灵敏性是可用性和效用的基石，但更进一步，灵敏性还意味着问题能够被更快地侦测到并得到有效地处理。灵敏的系统着眼于提供迅速和一致的响应时间，建立可靠的服务上限，因而它们可以交付一致的服务质量。这种一致的行为反过来又能简化出错处理，建立最终用户对系统的信心，并且促使他们与系统进一步交互。

* <a name="Resilient"></a>**Resilient**: The system stays responsive in the face of [failure](/glossary#Failure). This applies not only to highly-available, mission critical systems — any system that is not resilient will be unresponsive after a failure. Resilience is achieved by [replication](/glossary#Replication), containment, [isolation](/glossary#Isolation) and [delegation](/glossary#Delegation). Failures are contained within each [component](/glossary#Component), isolating components from each other and thereby ensuring that parts of the system can fail and recover without compromising the system as a whole. Recovery of each component is delegated to another (external) component and high-availability is ensured by replication where necessary. The client of a component is not burdened with handling its failures.

* <a name="Resilient"></a>**有弹性的**:

* <a name="Elastic"></a>**Elastic**: The system stays responsive under varying workload. Reactive Systems can react to changes in the input rate by increasing or decreasing the [resources](/glossary#Resource) allocated to service these inputs. This implies designs that have no contention points or central bottlenecks, resulting in the ability to shard or replicate components and distribute inputs among them. Reactive Systems support predictive, as well as Reactive, scaling algorithms by providing relevant live performance measures. They achieve [elasticity](/glossary#Elasticity) in a cost-effective way on commodity hardware and software platforms.

* <a name="Elastic"></a>**可伸缩的**:

* <a name="Message-Driven"></a>**Message Driven**: Reactive Systems rely on [asynchronous](/glossary#Asynchronous) [message-passing](/glossary#Message-Driven) to establish a boundary between components that ensures loose coupling, isolation, [location transparency](/glossary#Location-Transparency), and provides the means to delegate [errors](/glossary#Failure) as messages. Employing explicit message-passing enables load management, elasticity, and flow control by shaping and monitoring the message queues in the system and applying [back-pressure](/glossary#Back-Pressure) when necessary. Location transparent messaging as a means of communication makes it possible for the management of failure to work with the same constructs and semantics across a cluster or within a single host. [Non-blocking](/glossary#Non-Blocking) communication allows recipients to only consume [resources](/glossary#Resource) while active, leading to less system overhead.

* <a name="Message-Driven"></a>**消息驱动的**:

Large systems are composed of smaller ones and therefore depend on the Reactive properties of their constituents. This means that Reactive Systems apply design principles so these properties apply at all levels of scale, making them composable. The largest systems in the world rely upon architectures based on these properties and serve the needs of billions of people daily. It is time to apply these design principles consciously from the start instead of rediscovering them each time.

大系统由较小的部分构成，因此依赖于他们的构成要素中的响应式属性。这意味着响应式系统在所有层级规模上运用着这些属性运用的设计原则，从而使之可以组合。世界上最大的系统依赖着基于这些属性的体系结构，并且应对着数十亿人的日常需求。现在是时候有意识地从头运用这些设计原则而不是每次重新发现他们了。（这句话好没力度）

[签署这份宣言](http://www.reactivemanifesto.org/#sign-button)
