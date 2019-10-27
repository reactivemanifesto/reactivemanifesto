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
* [Řízený zprávami (na rozdíl od řízený událostmi)](#Message-Driven)
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
Současné počítače jsou optimalizované pro opakovaní stejných výpočtů: predikce a cachování instrukcí zvyšuje počet instrukcí za sekundu při neměnném taktu procesoru. Pokud dostane stejný procesor jinou úlohu ke zpracování, nedokáže dosáhnout plného výpočetního výkonu: proto by měly být programy strukturovány tak, aby docházelo k co nejmenším změnám instrukcí. Buď je třeba zpracovávat sady dat v dávkách, nebo provádět rozdílné úlohy v různých vláknech.

Ke stejnému závěru docházíme i při užití externích [zdrojů](#Resource), které potřebují synchronizaci a koordinaci. Vstupní-výstupní propustnost datových médií může být výrazně zvýšena, pokud je přístup prováděn z jediného dedikovaného vlákna. Použití jednoho vstupního bodu umožňuje reorganizaci operací optimálně pro dané zařízení (současná média pracují lépe při lineárním nežli přímém přístupu).

Navíc umožňuje dávkové zpracování sdílení režií drahých operací jako čtení a psaní. Příkladem je zabalení více datových položek do stejného síťového packetu nebo psaní do jednoho diskového bloku pro zvýšení prostupnosti a efektivity.

## <a name="Component"></a>Komponenta
Modulární architektura, o které je řeč, je velmi stará myšlenka, viz např. [Parnas (1972)](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf) [[ACM](https://dl.acm.org/citation.cfm?id=361623)]. V reakčním manifestu je termín “komponenta” použitý pro svůj strukturální význam vyjadřující samostatnost, zapouzdření a [izolaci](#Isolation) vůči ostatním komponentám. Tento pohled se váže především k běhu systému, typicky je však zrcadlen i ve struktuře zdrojového kódu. Zatímco mohou různé komponenty pro běžné úlohy používat stejné moduly, kód programu definující chování každé z komponent je sám modulární. Hranice komponent jsou často úzce spojeny s [hraničními kontexty](http://martinfowler.com/bliki/BoundedContext.html) v dané doméně. Takto navržený systém tíhne k soudržnosti s danou doménou, čímž udržuje izolaci a usnadňuje tak vývoj. [Protokoly](#Protocol) založené na zasílání zpráv poskytují přirozenou komunikační vrstvu (mapování) mezi hraničními kontexty komponent.

## <a name="Delegation"></a>Delegace
[Asynchronním](#Asynchronous) delegováním úlohy z jedné [komponenty](#Component) do druhé dojde k provedení dané úlohy v kontextu druhé komponenty. Tato delegace může mít za následek odlišné zpracování chyb, provedení v jiném vlákně, na jiném procesoru či v jiné síti, atd. Účel delegování je předání odpovědnosti za zpracování jiné komponentě nebo například umožnění monitorování průběhu delegované úlohy a případné reportování, ošetření chybových stavů, atd.

## <a name="Elasticity"></a>Pružnost (na rozdíl od škálovatelnosti)
Pružný systém se umí automaticky přizpůsobovat měnícím se požadavkům pomocí přiměřeného přidávání a odebírání zdrojů. Podmínkou pro využití přidělených zdrojů za běhu je [škálovatelnost](#Scalability) systému. Pružnost je tedy založena na škálovatelnosti, již rozšiřuje ve smyslu automatického řízení zdrojů.

## <a name="Failure"></a>Výpadek (na rozdíl od chyby)
Výpadek je neočekávaná událost uvnitř služby bránící od pokračování v normálním provozu. Výpadek obecně nedovoluje odeslání odpovědi k danému (a pravděpodobně všem následujícím) klientskému požadavku. Na rozdíl od výpadku, chybou rozumíme programem předvídanou podmínku, která je detekována během validace vstupu a komunikovaná směrem ke klientovi jako běžné zpracování, bez vlivu na pozdější požadavky. Výpadek je neočekávaný a vyžaduje zásah před tím, než je [systém](#System) znovu uveden do provozu. To ovšem neznamená, že mají všechny výpadky fatální dopad na systém - po výpadku může například dojít jen k redukci kapacity systému, omezení některé funkcionality atd. Chyby jsou naopak součástí běžného provozu služby, jsou zpracovány okamžitě a bez negativního vlivu na kapacitu či funkci systému.

Příkladem výpadku je hardwarová závada, neočekávané ukončení procesu kvůli nedostatku prostředků, vady programu vedoucí k nekorektnímu vnitřnímu stavu systému, atd.

## <a name="Isolation"></a>Izolace (a zamezení šíření výpadku)
Izolace znamená oddělení (decoupling) v čase a prostoru. Oddělení v čase znamená nezávislý životní cyklus odesílatele a příjemce, kteří v rámci komunikace dokonce nemusí být aktivní ve stejném časovém okamžiku. Oddělení v čase je umožněno skrze vymezení [asynchronních](#Asynchronous) hranic mezi [komponentami](#Component) a komunikací přes [zasílání zpráv](#Message-Driven). Oddělení v prostoru ([transparentnost polohy](#Location-Transparency)) znamená, že odesílatel a příjemce nemusí běžet v rámci stejného procesu, ale kdekoliv dle operační efektivity, proměnlivě v průběhu životního cyklu aplikace. 

Pravá izolace jde za hranice objektového zapouzdření, přidává kompartmentalizaci a zamezení šíření:
* Stavu a chování: umožňuje návrh nulového sdílení a minimalizuje režijní náklady spojené se soudržností (jak definováno v [Universal Scalability Law](http://www.perfdynamics.com/Manifesto/USLscalability.html); 
* Výpadku: umožňuje [výpadky](#Failure) zachytit, signalizovat a spravovat na odpovídající úrovni namísto kaskádovitého šíření do ostatních komponent.

Silná izolace mezi komponentami je postavena na komunikaci skrze zcela definované [protokoly](#Protocol) a umožňují volnou provázanost vedoucí ke snadnějšímu pochopení, vývoji a testování.

## <a name="Location-Transparency"></a>Transparentnost polohy
[Pružný](#Elasticity) musí být adaptivní, musí umět reagovat na nepřetržité změny v požadavcích a být schopen efektivního škálování. Klíčové je pochopení, že se jedná o distribuovaný výpočet. A to i v případě, že systém běží na jediném stroji (s mnoha nezávislými CPU komunikující skrze QPI), nebo na clusteru (s mnoha nezávislými stroji komunikujícími skrze síť). Tento fakt znamená, že neexistuje žádný koncepční rozdíl mezi vertikálním škálování na multi-jádrovém procesoru a horizontálním škálovaní na clusteru.

Pokud všechny [komponenty](#Component) systému podporují mobilitu a lokální komunikace je pouhý optimalizační detail, potom není třeba definovat topologii systému staticky, ale přenechat její stavbu operacím za běhu a umožnit tak její dynamickou optimalizaci na základě aktuálních požadavků.

Toto oddělení v prostoru (viz definice [izolace](#Isolation)), umožněné skrze [asynchronní](#Asynchronous) [zasílání zpráv](#Message-Driven), a oddělení běžících instancí od svých referencí nazýváme transparentnost polohy. Transparentnost polohy je často zaměňována s “transparentním distribuovaným výpočtem”, i když ve skutečnosti znamená opak: všechny aspekty a omezení (výpadky, podsítě, ztracené zprávy) komunikace přes síť jsou brány v potaz a explicitně modelovány namísto snahy o simulaci lokální komunikace přes síť (RPC, XA atd.). Náš pohled na transparentnost polohy je v plném souladu s [A Note On Distributed Computing](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.7628) od Waldo et al.

## <a name="Message-Driven"></a>Řízený zprávami (na rozdíl od řízený událostmi)
Zpráva je datová jednotka zaslaná do konkrétní destinace. Událost je signál vyslaný [komponentou](#Component) na základě dosažení jistého stavu. V systému řízeném zprávami očekává adresát příjem zprávy, na kterou poté reaguje nebo ji nechá bez odezvy. V systému řízeném událostmi jsou posluchači spjati se zdrojem událostí tak, aby mohli reagovat, když k události dojde. Systém řízený událostmi je tedy zaměřen na adresovatelné zdroje událostí, zatímco systém řízený zprávami je zaměřen na adresovatelné příjemce. Zpráva může obsahovat zakódovanou událost.

Dosáhnout odolnosti je mnohem náročnější v systému řízeném událostmi kvůli krátkodobé a nenávratné povaze řetězu událostí: Když je proces v běhu, služba obvykle řeší [výpadky](#Failure) přímo, ve smyslu odezvy zpět ke klientovi. Řešení výpadku komponenty ve smyslu obnovy jejího normálního provozu však vyžaduje ošetření, které není svázano s krátkodobým klientským požadavkem, ale je schopno reagovat na celkový stav komponenty.

## <a name="Non-Blocking"></a>Neblokující
Programování konkurenčních algoritmů je neblokující, pokud vlákna soupeřící o sdílený zdroj nemusí na neurčito pozastavit činnost kvůli exkluzivnímu přístupu k danému zdroji. Tato praxe se většinou manifestuje skrze API, která umožní přístup ke [zdroji](#Resource), pokud je dostupný, v opačném případě okamžitě odmítne přístup a informuje o tom klienta, namísto blokování klienta po dobu nedostupnosti zdroje. Neblokující API umožňuje klientovi zabývat se jinou činností namísto pasivního čekání na zdroj. Klient se může dodatečně zaregistrovat k danému zdroji a být informován o jeho dostupnosti či dokončení požadované operace.

## <a name="Protocol"></a>Protokol
Protokol definuje způsob a chování přenosu a výměny zpráv mezi [komponentami](#Component). Protokol shrnuje relaci mezi účastníky do množiny stavů a druhů zpráv. Protokol popisuje jaké zprávy mohou být v daném čase mezi účastníky zaslány. Protokoly se klasifikují dle typu výměny; mezi nejznámější patří požadavek–odpověď (request–reply), opakovaný požadavek–odpověď (repeated request–reply) - jako v HTTP, publikovat-odebírat (publish–subscribe), a streamování (push i pull).

V porovnání s lokálními rozhraními je protokol obecnější, neboť může popisovat výměnu mezi více účastníky a zohledňuje proměnu stavů zpráv, zatímco programové rozhraní specifikuje pouze jedinou interakci mezi volajícím a volaným v daném čase.

Je třeba poznamenat, že protokol pouze definuje, jaké zprávy mohou být zaslány, nehovoří však o tom, jakým způsobem má být přenos proveden, jak má být zpráva zakódována, atd. - tyto implementační detaily jsou zcela transparentní pro všechny uživatele daného protokolu.

## <a name="Replication"></a>Replikace
Výpočet [komponenty](#Component) souběžně na více různých místech se nazývá replikace. Může se jednat o výpočet v rozdílných vláknech, procesech, síťových uzlech nebo výpočetních centrech. Replikace přináší [škálovatelnost](#Scalability), při čemž je zátěž rozdělena mezi více instancí dané komponenty, a nebo odolnost na základě vícenásobného provedení požadavku na více instancích paralelně. Tyto přístupy je možné kombinovat například tak, že jsou všechny transakce příslušící danému uživateli provedeny na nejméně dvou instancích, zatímco celkový počet instancí se přizpůsobuje měnící se zátěži (viz [pružnost](#Elasticity)).

Při replikaci stavové komponenty je nutné zajistit synchronizaci dat mezi replikami; v opačném případě by musel klient vědět o daném replikačním mechanismu, což by vedlo k porušení principu zapouzdření. Synchronizace vede obecně k nutnosti kompromisu mezi konzistencí a dostupností, protože stoprocentní dostupnosti lze dosáhnout pouze v případě připuštění krátkodobého nekonzistentního stavu replik (eventuální konzistence), a naopak plná konzistence vyžaduje zamykání stavu replik, při čemž mohou tyto být krátkodobě nedostupné. Mezi těmito extrémy však leží celé spektrum možností, více či méně vyhovujících pro každou komponentu.

## <a name="Resource"></a>Zdroj
Zdroj je vše, na čem [komponenta](#Component) závisí. Zdroj musí být na základě potřeb komponenty zpřístupněn a udržován. Mezi zdroje patří například čas CPU, operační paměť, místo na disku, nebo propustnost sítě, plánovač služeb, a další vstupní a výstupní služby jako databáze, systém souborů atd. Pokud není [pružnost](#Elasticity) a odolnost těchto zdrojů zajištěna, může dojít k nesprávnému fungování požadované komponenty.

## <a name="Scalability"></a>Škálovatelnost
Škálovatelnost, tedy schopnost [systému](#System) využít více [zdrojů](#Resource) pro zvýšení výpočetního výkonu, je dána poměrem získané výkonnosti ku zvýšení zdrojů. V dokonale škálovatelném systému jsou tyto hodnoty proporcionální: dvojnásobek přidělených zdrojů znamená také dvojnásobný výkon. Škálovatelnost je často omezena kvůli úzkému hrdlu v systému nebo nutnosti synchronizace; viz [Amdahl’s Law and Gunther’s Universal Scalability Model](http://blogs.msdn.com/b/ddperf/archive/2009/04/29/parallel-scalability-isn-t-child-s-play-part-2-amdahl-s-law-vs-gunther-s-law.aspx).

## <a name="System"></a>Systém
Systém poskytuje službu [uživatelům](#User) či klientům. Systém může být rozsáhlý nebo jednoduchý, což je dáno obsahem velkého či malého počtu [komponent](#Component), ze kterých se systém skládá. Komponenty systému poskytují dané služby na základě vzájemnné kolaborace. Komponenty jsou v rámci systému často ve vztahu klient-server (například front-end komponenty závisí na back-end komponentách). Systém sdílí společný model odolnosti, což znamená, že [výpadek](#Failure) komponent je řešen v rámci systému [delegováním](#Delegation) z jedné komponenty na druhou. Sub-systém je tvořen skupinami komponent [izolovanými](#Isolation) uvnitř systému ve smyslu poskytované funkce, [zdrojů](#Resource) nebo způsobu řešení výpadku.

## <a name="User"></a>Uživatel
Uživatelem může být myšlen jakýkoliv konzument služby, ať už člověk nebo jiná služba.
