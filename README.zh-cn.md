响应式宣言
---------

有许多组织在各自完全不同的领域中工作，他们都正在独立地发现看起来相同的软件构建模式。他们的系统更加稳健，更加有可回复性，更加灵活，并且以更好的定位来满足现代的需求。

上述变化之所以会发生，是因为近几年的应用需求已经发生了戏剧性的变化。仅仅在几年之前，大型应用通常拥有数十台服务器，数秒的响应时间，数小时的离线维护时间以及许多 GB 的数据。而今天，应用被部署在一切场合，从移动设备到基于云的集群，这些集群运行在数以千计的多核心处理器的之上。用户期望毫秒级的响应时间以及 100% 的正常运行时间。数据则以 PB 为单位来衡量。昨天的软件架构已经完全无法地满足今天的需求。

我们相信，一种条理分明的系统架构方法是必要的，而且我们相信关于这种方法的所有必要方面已经逐一地被人们认识到：我们需要的系统是响应式的，具有可回复性的，可伸缩的，以及以消息驱动的。我们将这样的系统之为响应式系统。

以响应式系统方式构建的系统更加灵活，松耦合和[可扩展](/glossary#可扩展性)。这使得它们更容易被开发，而且经得起变化的考验。它们对于系统[失败](/glossary#失败)表现出显著的包容性，并且当失败真的发生时，他们用优雅的方式去应对，而不是放任灾难的发生。响应式系统是高度灵敏的，能够给[用户](/glossary#用户)以有效的交互式的反馈。

**响应式系统是：**

* <a name="灵敏的"></a>**灵敏的**：只要有可能，[系统](/glossary#系统)就会及时响应。灵敏性是可用性和效用的基石，但还不止于此，灵敏性还意味着问题能够被更快地侦测到并得到有效地处理。灵敏的系统着眼于提供迅速和一致的响应时间，建立可靠的服务上限，因而它们可以交付一致的服务质量。这种一致的行为反过来又能简化出错处理，建立最终用户对系统的信心，并且促使他们与系统做进一步交互。

* <a name="有回复性的"></a>**有回复性的**：系统在面临[故障](#故障)时也能保持灵敏度。可回复性不仅适用于高可用的关键任务系统——任何系统都可以具有这种属性，一个不具可回复性的系统一旦出现故障，就会变得不灵敏。可回复性可以通过[复制](/glossary#复制)，围控，[隔离](/glossary#隔离)和[委派](#/glossary#委派)等方式实现。在可回复性的系统中，故障被包含在每个组件中，各组件之间相互隔离，从而允许系统的某些部分出故障并且在不连累整个系统的前提下进行恢复。每个组件的恢复过程都可委派给另一个（外部的）组件来完成，并且在必要的位置可借助复制机制来确保系统的高可用性。这样一来，组件的客户就不必为处理组件自身的故障而承担压力。

* <a name="Elastic"></a>**Elastic**: The system stays responsive under varying workload. Reactive Systems can react to changes in the input rate by increasing or decreasing the [resources](/glossary#Resource) allocated to service these inputs. This implies designs that have no contention points or central bottlenecks, resulting in the ability to shard or replicate components and distribute inputs among them. Reactive Systems support predictive, as well as Reactive, scaling algorithms by providing relevant live performance measures. They achieve [elasticity](/glossary#Elasticity) in a cost-effective way on commodity hardware and software platforms.

* <a name="可伸缩的"></a>**可伸缩的**:

* <a name="Message-Driven"></a>**Message Driven**: Reactive Systems rely on [asynchronous](/glossary#Asynchronous) [message-passing](/glossary#Message-Driven) to establish a boundary between components that ensures loose coupling, isolation, [location transparency](/glossary#Location-Transparency), and provides the means to delegate [errors](/glossary#Failure) as messages. Employing explicit message-passing enables load management, elasticity, and flow control by shaping and monitoring the message queues in the system and applying [back-pressure](/glossary#Back-Pressure) when necessary. Location transparent messaging as a means of communication makes it possible for the management of failure to work with the same constructs and semantics across a cluster or within a single host. [Non-blocking](/glossary#Non-Blocking) communication allows recipients to only consume [resources](/glossary#Resource) while active, leading to less system overhead.

* <a name="消息驱动的"></a>**消息驱动的**:

大系统由较小的系统构成，因此势必依赖于这些构成要素的响应式属性。这意味着响应式系统需要应用一定的设计原则，使这些属性在所有不同等级的构成要素上都适用，进而使它们可组合。世界上最大的系统所依赖的体系结构，都是基于这些属性构建的，它们每天都在服务数十亿人的需求。不要总是在开发中期重新发现这些设计原则了，在开发之初就有意识地运用它们吧！现在正是时候！

[签署这份宣言](http://www.reactivemanifesto.org/#sign-button)
