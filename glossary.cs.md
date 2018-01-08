# Glosář reaktivního manifestu

* [Asynchronní](#Asynchronous)
* [Back-Pressure](#Back-Pressure)
* [Dávkové zpracování](#Batching)
* [Komponenta](#Component)
* [Delegace](#Delegation)
* [Pružnost (na rozdíl od škálovatelnosti)](#Elasticity)
* [Výpadek (na rozdíl od chyby)](#Failure)
* [Izolace (a zamezení šíření výpadku)](#Isolation)
* [Transparentnost polohy](#Location-Transparency)
* [Zprávami-řízený (na rozdíl od událostmi-řízený)](#Message-Driven)
* [Neblokující](#Non-Blocking)
* [Protokol](#Protocol)
* [Replikace](#Replication)
* [Zdroj](#Resource)
* [Škálovatelnost](#Scalability)
* [Systém](#System)
* [Uživatel](#User)

## <a name="Asynchronous"></a>Asynchronní
Doslovně přeloženo jako _“nesoudobý, nesoučasný či nesynchronizovaný v čase”_. V kontextu tohoto manifestu je myšleno, že k zpracování požadavku dochází v libovolném časovém bodu poté, co byl doručen od klienta ke službě. Klient není schopen jakkoliv sledovat či sám sebe synchronizovat vzhledem ke zpracování požadavku službou. Přímý protiklad je pak synchronní zpracování, při čemž je klient o výsledku zpracování informován hned, jakmile je službou provedeno.

## <a name="Back-Pressure"></a>Back-Pressure
Pokud se [komponenta](#Component) nachází pod přílišnou zátěží, musí [systém](#System) jako celek reagovat přiměřeným způsobem. Komponenta pod takovýmto tlakem nesmí způsobit katastrofální výpadek celého systému, ani odmítnout či ztratit příchozí zprávy. Pokud není komponenta schopna nápor korektně zpracovat, ale zároveň nedojde ani k jejímu výpadku, musí být tato skutečnost komunikována směrem k nadřazeným komponentám, které se musí postarat o vyrovnání zátěže. Back-pressure je důležitý mechanismus zpětné vazby umožnující systému vyrovnat se s nadměrnou zátěží namísto celkového kolapsu. Back-pressure může být kaskádovitě aplikován na celé cestě požadavku od klienta k zajištění odolnosti systému vůči nadměrnému zatížení. Systém využije této informace k poskytnutí více zdrojů pro rozložení zátěže, viz [pružnost](#Elasticity).

## <a name="Batching"></a>Dávkové zpracování
Současné počítače jsou optimalizované pro opakovaní stejných výpočtů: predikce a cachování instrukcí zvyšuje počet instrukcí za sekundu při neměnném taktu processoru. Pokud dostane stejný procesor jinou úlohu ke zpracování, nedokáže dosáhnout plného výpočetního výkonu: proto by měly být programy strukturovány tak, aby docházelo k co nejmenším změnám instrukcí. Buď je třeba zpracovávat sady dat v dávkách, nebo provádět rozdílné úlohy v různých vláknech.

Ke stejnému závěru docházíme i při užití externích [zdrojů](#Resource), které potřebují synchronizaci a koordinaci. Vstupní-výstupní propustnost datových médií může být výrazně zvýšena, pokud je přístup prováděn z jediného dedikovaného vlákna. Použití jednoho vstupního bodu umožňuje reorganizaci operací optimálně pro dané zařízení (současná média pracují lépe při lineárním nežli přímém přístupu).

Navíc umožňuje dávkové zpracování sdílení řežijí drahých operací jako čtení a psaní. Příkladem je zabalení více datových položek do stejného síťového packetu nebo psaní do jednoho diskového bloku pro zvýšení prostupnosti a efektivity.

## <a name="Component"></a>Komponenta
Modulární architektura, o které je řeč, je velmi stará myšlenka, viz např. [Parnas (1972)](https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf). V reakčním manifestu je termín “komponenta” požitý pro svůj strukturální význam vyjadřující samostatnost, zapouzdření a [izolaci](#Isolation) vůči ostatním komponentám. Tento pohled se váže především k běhu systému, typicky je však zrcadlen i ve struktuře zdrojového kódu. Zatímco mohou různé komponenty pro běžné úlohy používat stejné moduly, kód programu definující chování každé z komponent je sám modulární. Hranice komponent jsou často úzce spojeny s [hraničními kontexty](http://martinfowler.com/bliki/BoundedContext.html) v dané doméně. Takto navržený systém tíhne k soudržnosti s danou doménou, čímž udržuje izolaci a usnadňuje tak vývoj. [Protokoly](#Protocol) založené na zasílání zpráv poskytují přirozenou komunikační vrstvu (mapování) mezi hraničními kontexty komponent.

## <a name="Delegation"></a>Delegace
[Asynchronním](#Asynchronous) delegováním úlohy z jedné [komponenty](#Component) do druhé dojde k provedení dané úlohy v kontextu druhé komponenty. Tato delegace může mít za následek odlišné zpracování chyb, provedení v jiném vlákně, na jiném procesoru či v jiné síti, atd. Účel delegování je předání odpovědnosti za zpracování jiné komponentě nebo například umožnění monitorování průběhu delegované úlohy a případné reportování, ošetření chybových stavů, atd.

## <a name="Elasticity"></a>Pružnost (na rozdíl od škálovatelnosti)
Pružný systém se umí automaticky přizpůsobovat měnícím se požadavkům pomocí přiměřeného přidávání a odebírání zdrojů. Podmínkou pro využítí přidělených zdrojů za běhu je [škálovatelnost](#Scalability) systému. Pružnost je tedy založena na škálovatelnosti, již rozšiřuje ve smyslu automatického řízení zdrojů.

## <a name="Failure"></a>Výpadek (na rozdíl od chyby)
Výpadek je neočekávaná událost uvnitř služby bránící od pokračování v normálním provozu. Výpadek obecně nedovoluje odeslání odpovědi k danému (a pravděpodobně všem následujícím) klientskému požadavku. Narozdíl od výpadku, chybou rozumíme programem předvídanou podmínku, která je detekována během validace vstupu a komunikovaná směrem ke klientovi jako běžné zpracování, bez vlivu na pozdější požadavky. Výpadek je neočekávaný a vyžaduje zásah před tím, než je [systém](#System) znovu uveden do provozu. To ovšem neznamená, že mají všechny výpadky fatální popad na systém - po výpadku může například dojít jen k redukci kapacity systému, omezení některé funkcionality atd. Chyby jsou naopak součástí běžného provozu služby, jsou zpracovány okamžitě a bez negativního vlivu na kapacitu či funkci systému.

Příkladem výpadku je hardwarová závada, neočekáváné ukončení procesu kvůli nedostatku prostředků, vady programu vedoucí k nekorektnímu vnitřnímu stavu systému, atd.

## <a name="Isolation"></a>Izolace (a zamezení šíření výpadku)
Izolace znamená oddělení (decoupling) v čase a prostoru. Oddělení v čase znamená nezávislý životní cyklus odesílatele a příjemce, kteří v rámci komunikace dokonce nemusí být aktivní ve stejném časovém okamžiku. Oddělení v čase je umožněno skrze vložení [asynchronních](#Asynchronous) hranic mezi [komponenty](#Component) a komunikací přes [zasílání zpráv](#Message-Driven). Oddělení v prostoru ([transparentnost polohy](#Location-Transparency)) znamená, že odesílatel a příjemce nemusí běžet v rámci stejného procesu, ale kdekoliv dle operační efektivity, proměnlivě v průběhu životního cyklu aplikace. 

Pravá izolace jde za hranice objektového zapouzdření a přidává kompartmentalizaci a zamezení šíření:
* Stavu a chování: umožňuje návrh nulového sdílení a minimalizuje režijní náklady spojené se soudržností (jak definováno v [Universal Scalability Law](http://www.perfdynamics.com/Manifesto/USLscalability.html); 
* Výpadku: umožňuje [výpadky](#Failure) zachytit, signalizovat a spravovat na odpovídající úrovni namísto kaskádovitého šíření do ostatních komponent.

Silná izolace mezi komponentami je postavena na komunikaci skrze kompletně definované [protokoly](#Protocol) a umožňují volnou provázanost vedoucí ke snadnějšímu pochopení, vývoji a testování.

## <a name="Location-Transparency"></a>Transparentnost polohy
[Pružný](#Elasticity) musí být adaptivní, musí umět reagovat na nepřetržité změny v požadavcích a musí být schopen efektivního škálování. Klíčové je pochopení, že se jedná o distribuovaný výpočet. A to i v případě, že systém běží na jediném stroji (s mnoha nezávislými CPU komunikující skrze QPI), nebo na clusteru (s mnoha nezávislými stroji komunikujícími skrze síť). Tento fakt znamená, že neexistuje žádný koncepční rozdíl mezi vertikálním škálování na multi-jádrovém procesoru a horizontálním škálovaní na clusteru.

Pokud všechny [komponenty](#Component) systému podporují mobilitu a lokální komunikace je pouhý optimalizační detail, potom není třeba definovat topologii systému staticky, ale přenechat její stavbu operacím za běhu a umožnit tak její dynamickou optimimalizaci na základě aktuálních požadavků.

This decoupling in space (see the definition for [Isolation](#Isolation)), enabled through [asynchronous](#Asynchronous) [message-passing](#Message-Driven), and decoupling of the runtime instances from their references is what we call Location Transparency. Location Transparency is often mistaken for 'transparent distributed computing', while it is actually the opposite: we embrace the network and all its constraints—like partial failure, network splits, dropped messages, and its asynchronous and message-based nature—by making them first class in the programming model, instead of trying to emulate in-process method dispatch on the network (ala RPC, XA etc.). Our view of Location Transparency is in perfect agreement with [A Note On Distributed Computing](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.7628) by Waldo et al.

## <a name="Message-Driven"></a>Message-Driven (in contrast to Event-Driven)
A message is an item of data that is sent to a specific destination. An event is a signal emitted by a [component](#Component) upon reaching a given state. In a message-driven system addressable recipients await the arrival of messages and react to them, otherwise lying dormant. In an event-driven system notification listeners are attached to the sources of events such that they are invoked when the event is emitted. This means that an event-driven system focuses on addressable event sources while a message-driven system concentrates on addressable recipients. A message can contain an encoded event as its payload.

Resilience is more difficult to achieve in an event-driven system due to the short-lived nature of event consumption chains: when processing is set in motion and listeners are attached in order to react to and transform the result, these listeners typically handle success or [failure](#Failure) directly and in the sense of reporting back to the original client. Responding to the failure of a component in order to restore its proper function, on the other hand, requires a treatment of these failures that is not tied to ephemeral client requests, but that responds to the overall component health state.

## <a name="Non-Blocking"></a>Non-Blocking
In concurrent programming an algorithm is considered non-blocking if threads competing for a resource do not have their execution indefinitely postponed by mutual exclusion protecting that resource. In practice this usually manifests as an API that allows access to the [resource](#Resource) if it is available otherwise it immediately returns informing the caller that the resource is not currently available or that the operation has been initiated and not yet completed. A non-blocking API to a resource allows the caller the option to do other work rather than be blocked waiting on the resource to become available. This may be complemented by allowing the client of the resource to register for getting notified when the resource is available or the operation has completed.

## <a name="Protocol"></a>Protocol
A protocol defines the treatment and etiquette for the exchange or transmission of messages between [components](#Component). Protocols are formulated as relations between the participants to the exchange, the accumulated state of the protocol and the allowed set of messages to be sent. This means that a protocol describes which messages a participant may send to another participant at any given point in time. Protocols can be classified by the shape of the exchange, some common classes are request–reply, repeated request–reply (as in HTTP), publish–subscribe, and stream (both push and pull).

In comparison to local programming interfaces a protocol is more generic since it can include more than two participants and it foresees a progression of the state of the message exchange; an interface only specifies one interaction at a time between the caller and the receiver.

It should be noted that a protocol as defined here just specifies which messages may be sent, but not how they are sent: encoding, decoding (i.e. codecs), and transport mechanisms are implementation details that are transparent to the components’ use of the protocol.

## <a name="Replication"></a>Replication
Executing a [component](#Component) simultaneously in different places is referred to as replication. This can mean executing on different threads or thread pools, processes, network nodes, or computing centers. Replication offers [scalability](#Scalability), where the incoming workload is distributed across multiple instances of a component, or resilience, where the incoming workload is replicated to multiple instances which process the same requests in parallel. These approaches can be mixed, for example by ensuring that all transactions pertaining to a certain user of the component will be executed by two instances while the total number of instances varies with the incoming load, (see [Elasticity](#Elasticity)).

Where a stateful component is replicated, care must be taken to synchronize the state data between replicas, otherwise clients of this component need to be aware of the replication scheme and encapsulation is violated. The choice of synchronization scheme in general presents a trade-off between consistency and availability, where perfect availability can only be achieved if replicas are allowed to diverge for limited time periods (eventual consistency) and perfect consistency requires all replicas to advance their state effectively in lock-step. Between these extremes lies a spectrum of possible solutions, from which each component should pick the one that is most appropriate for its needs.

## <a name="Resource"></a>Resource
Everything that a [component](#Component) relies upon to perform its function is a resource that must be provisioned according to the component’s needs. This includes CPU allocation, main memory and persistent storage as well as network bandwidth, main memory bandwidth, CPU caches, inter-socket CPU links, reliable timer and task scheduling services, other input and output devices, external services like databases or network file systems etc. The [elasticity](#Elasticity) and resilience of all these resources must be considered, since the lack of a required resource will prevent the component from functioning when required.

## <a name="Scalability"></a>Scalability
The ability of a [system](#System) to make use of more computing [resources](#Resource) in order to increase its performance is measured by the ratio of throughput gain to resource increase. A perfectly scalable system is characterized by both numbers being proportional: a twofold allocation of resources will double the throughput. Scalability is typically limited by the introduction of bottlenecks or synchronization points within the system, leading to constrained scalability, see [Amdahl’s Law and Gunther’s Universal Scalability Model](http://blogs.msdn.com/b/ddperf/archive/2009/04/29/parallel-scalability-isn-t-child-s-play-part-2-amdahl-s-law-vs-gunther-s-law.aspx).

## <a name="System"></a>System
A system provides services to its [users](#User) or clients. Systems can be large or small, in which case they comprise many or just a few [components](#Component). All components of a system collaborate to provide these services. In many cases the components are in a client–server relationship within the same system (consider for example front-end components relying upon back-end components). A system shares a common resilience model, by which we mean that [failure](#Failure) of a component is handled within the system, [delegated](#Delegation) from one component to the other. It is useful to view groups of components within a system as subsystems if they are [isolated](#Isolation) from the rest of the system in their function, [resources](#Resource) or failure modes.

## <a name="User"></a>User
We use this term informally to refer to any consumer of a service, be that a human or another service.
