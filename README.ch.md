反应式宣言
----------------------

各组织独立工作在不同的领域，他们都在为构建软件探索一种相似的模式。这些系统更健壮、更容易恢复、更灵活，可以更好的满足现代需求。

由于应用需求的变化，近几年发生了很大变化。仅仅几年前，一个大型的应用拥有数十台服务器，秒级的响应时间，小时级离线维护时间和千兆级的数据。 
今天的应用可以被部署到任何机器上，从移动设备到基于云计算的运行在数以千计的多核处理器上的集群。用户期待毫秒级的响应时间和100%的运行时间。
数据单位使用PB测量。使用过去的软件架构是无法满足现在的简单需求。

我们认为需要一整套系统架构方法，我们也认为所有必要的方面已经被分别的确认：我们希望系统是可响应、可恢复、可伸缩和消息驱动。我们称之为反应式系统。

系统构建成一个反应式系统，将更加的灵活、松散耦合和[可扩展](/glossary#Scalability)。这使得开发和定制更加容易。它们对[失败](/glossary#Failure)有明显
的容忍，当失败发生时，它们面对的是优雅而不是灾难。反应式系统具有高反应度，提供给[用户](/glossary#User)有效的互动反馈。

*Reactive Systems are:*

* <a name="Responsive"></a>**Responsive**: The [system](/glossary#System) responds in a timely manner if at all possible. Responsiveness is the cornerstone of usability and utility, but more than that, responsiveness means that problems may be detected quickly and dealt with effectively. Responsive systems focus on providing rapid and consistent response times, establishing reliable upper bounds so they deliver a consistent quality of service. This consistent behaviour in turn simplifies error handling, builds end user confidence, and encourages further interaction. 
* <a name="Resilient"></a>**Resilient**: The system stays responsive in the face of [failure](/glossary#Failure). This applies not only to highly-available, mission critical systems — any system that is not resilient will be unresponsive after a failure. Resilience is achieved by [replication](/glossary#Replication), containment, [isolation](/glossary#Isolation) and [delegation](/glossary#Delegation). Failures are contained within each [component](/glossary#Component), isolating components from each other and thereby ensuring that parts of the system can fail and recover without compromising the system as a whole. Recovery of each component is delegated to another (external) component and high-availability is ensured by replication where necessary. The client of a component is not burdened with handling its failures.
* <a name="Elastic"></a>**Elastic**: The system stays responsive under varying workload. Reactive Systems can react to changes in the input rate by increasing or decreasing the [resources](/glossary#Resource) allocated to service these inputs. This implies designs that have no contention points or central bottlenecks, resulting in the ability to shard or replicate components and distribute inputs among them. Reactive Systems support predictive, as well as Reactive, scaling algorithms by providing relevant live performance measures. They achieve [elasticity](/glossary#Elasticity) in a cost-effective way on commodity hardware and software platforms.
* <a name="Message-Driven"></a>**Message Driven**: Reactive Systems rely on [asynchronous](/glossary#Asynchronous) [message-passing](/glossary#Message-Driven) to establish a boundary between components that ensures loose coupling, isolation, and [location transparency](/glossary#Location-Transparency). This boundary also provides the means to delegate [failures](/glossary#Failure) as messages. Employing explicit message-passing enables load management, elasticity, and flow control by shaping and monitoring the message queues in the system and applying [back-pressure](/glossary#Back-Pressure) when necessary. Location transparent messaging as a means of communication makes it possible for the management of failure to work with the same constructs and semantics across a cluster or within a single host. [Non-blocking](/glossary#Non-Blocking) communication allows recipients to only consume [resources](/glossary#Resource) while active, leading to less system overhead.

Large systems are composed of smaller ones and therefore depend on the Reactive properties of their constituents. This means that Reactive Systems apply design principles so these properties apply at all levels of scale, making them composable. The largest systems in the world rely upon architectures based on these properties and serve the needs of billions of people daily. It is time to apply these design principles consciously from the start instead of rediscovering them each time.

[Sign the manifesto](http://www.reactivemanifesto.org/#sign-button)
