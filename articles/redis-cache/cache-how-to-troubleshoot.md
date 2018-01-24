---
title: Het oplossen van Azure Redis-Cache | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende problemen met Azure Redis-Cache.
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: wesmc
ms.openlocfilehash: a88adc300e52c74f2a1fcd2e546ab879000d877e
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="how-to-troubleshoot-azure-redis-cache"></a>Het oplossen van Azure Redis-Cache
Dit artikel bevat richtlijnen voor het oplossen van de volgende categorieën van Azure Redis-Cache-problemen.

* [Client-side probleemoplossing](#client-side-troubleshooting) - in deze sectie bevat richtlijnen te identificeren en het oplossen van problemen veroorzaakt door de toepassing verbinding maken met Azure Redis-Cache.
* [Server side probleemoplossing](#server-side-troubleshooting) - in deze sectie bevat richtlijnen te identificeren en het oplossen van problemen veroorzaakt aan de serverzijde van Azure Redis-Cache.
* [StackExchange.Redis time-out-uitzonderingen](#stackexchangeredis-timeout-exceptions) -in deze sectie bevat informatie over het oplossen van problemen bij het gebruik van de client StackExchange.Redis.

> [!NOTE]
> Aantal van de stappen in deze handleiding bevatten instructies om Redis-opdrachten uitvoeren en controleren van verschillende maatstaven voor prestaties. Zie voor meer informatie en instructies in de artikelen in de [aanvullende informatie](#additional-information) sectie.
> 
> 

## <a name="client-side-troubleshooting"></a>Client-side probleemoplossing
Deze sectie wordt beschreven voor het oplossen van problemen die vanwege een voorwaarde op de clienttoepassing optreden.

* [Geheugendruk op de client](#memory-pressure-on-the-client)
* [Burst van verkeer](#burst-of-traffic)
* [Hoge client CPU-gebruik](#high-client-cpu-usage)
* [Client-Side bandbreedte overschreden](#client-side-bandwidth-exceeded)
* [De grootte van veel aanvragen/reacties](#large-requestresponse-size)
* [Waar vind ik mijn gegevens in Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Geheugendruk op de client
#### <a name="problem"></a>Probleem
Geheugendruk op de clientcomputer leidt tot allerlei soorten prestatieproblemen die de verwerking van gegevens die zijn verzonden door het Redis-exemplaar zonder enige vertraging kunnen worden vertraagd. Wanneer geheugendruk raakt, heeft het systeem meestal op de paginagegevens van het fysieke geheugen in het virtuele geheugen op de schijf. Dit *pagina met fout* zorgt ervoor dat het systeem aanzienlijk vertragen.

#### <a name="measurement"></a>Meting
1. Geheugengebruik op de machine om ervoor te zorgen dat het beschikbare geheugen niet overschrijdt. 
2. Monitor voor de `Page Faults/Sec` prestatiemeteritem. De meeste systemen wordt hebben sommige wisselbestandsfouten zelfs tijdens normale werking, dus pieken in dit prestatiemeteritem voor pagina-fouten die met time-outs overeenkomen gecontroleerd.

#### <a name="resolution"></a>Oplossing
Upgrade voor de client naar een grotere client VM-grootte met meer geheugen of verdiepen in uw geheugen gebruikspatronen te verminderen geheugen consuption.

### <a name="burst-of-traffic"></a>Burst van verkeer
#### <a name="problem"></a>Probleem
Bursts van verkeer, gecombineerd met slecht `ThreadPool` instellingen kunnen leiden tot vertragingen bij het verwerken van gegevens die al zijn verzonden door de Redis-Server, maar nog niet op de client verbruikt.

#### <a name="measurement"></a>Meting
Monitor hoe uw `ThreadPool` statistieken wijzigen gedurende een periode met code [zoals deze](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). U kunt ook zoeken op de `TimeoutException` bericht van StackExchange.Redis. Hier volgt een voorbeeld:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

In het bovenstaande bericht zijn er verschillende problemen die interessante:

1. Merk op dat in de `IOCP` sectie en de `WORKER` sectie die u hebt een `Busy` waarde die groter is dan de `Min` waarde. Dit betekent dat uw `ThreadPool` instellingen moeten aan te passen.
2. U ziet ook `in: 64221`. Dit geeft aan dat 64211 bytes op de laag van de kernel-socket is ontvangen, maar nog niet zijn gelezen door de toepassing (bijvoorbeeld StackExchange.Redis). Dit betekent doorgaans dat uw toepassing wordt niet lezen van gegevens vanaf het netwerk snel de server is naar u te verzenden.

#### <a name="resolution"></a>Oplossing
Configureer uw [ThreadPool instellingen](https://gist.github.com/JonCole/e65411214030f0d823cb) burst scenario's om ervoor te zorgen dat uw thread-groep snel onder worden geschaald.

### <a name="high-client-cpu-usage"></a>Hoge client CPU-gebruik
#### <a name="problem"></a>Probleem
Hoog CPU-gebruik op de client is een indicatie dat het systeem niet kan bijhouden met het werk dat deze is gevraagd om uit te voeren. Dit betekent dat de client een antwoord van Redis op tijdige wijze verwerkt Hoewel Redis antwoord verzonden door het zeer snel kan mislukken.

#### <a name="measurement"></a>Meting
Het systeem Wide CPU-gebruik via de Azure Portal of via het bijbehorende prestatiemeteritem bewaken. Zorg ervoor dat u niet bewaken *proces* CPU omdat een enkel proces lage CPU-gebruik kan hebben op de tijd die het hele systeem CPU kan hoog zijn. Pieken in CPU-gebruik die met time-outs overeenkomen gecontroleerd. Als gevolg van een hoog CPU, u ziet misschien ook hoog `in: XXX` in waarden `TimeoutException` foutberichten zoals beschreven in de [Burst van verkeer](#burst-of-traffic) sectie.

> [!NOTE]
> StackExchange.Redis 1.1.603 en nieuwer, bevat de `local-cpu` metrische in `TimeoutException` foutberichten. Zorg ervoor dat u de nieuwste versie van de [NuGet-pakket StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die voortdurend wordt verholpen in de volgende code maakt u deze krachtiger voor time-outs dus belangrijk dat de meest recente versie is.
> 
> 

#### <a name="resolution"></a>Oplossing
Upgrade naar een grotere VM-grootte met meer CPU-capaciteit of onderzoeken wat de oorzaak van CPU, pieken. 

### <a name="client-side-bandwidth-exceeded"></a>Client-side bandbreedte overschreden
#### <a name="problem"></a>Probleem
Clientcomputers van verschillende grootte hebben hun beperkingen op de hoeveelheid netwerkbandbreedte ze beschikbaar zijn. Als de client de beschikbare bandbreedte overschrijdt, wordt klikt u vervolgens gegevens niet verwerkt op de client snel de server is verzonden. Dit kan leiden tot time-outs.

#### <a name="measurement"></a>Meting
Controleren hoe uw bandbreedtegebruik wijzigen gedurende een periode met code [zoals deze](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Houd er rekening mee dat deze code kan niet worden uitgevoerd in sommige omgevingen met beperkte machtigingen (zoals Azure websites).

#### <a name="resolution"></a>Oplossing
Client VM-grootte vergroten of verkleinen van netwerkbandbreedte.

### <a name="large-requestresponse-size"></a>De grootte van veel aanvragen/reacties
#### <a name="problem"></a>Probleem
Een veel aanvragen/reacties kan leiden tot time-outs. Stel bijvoorbeeld dat uw time-outwaarde die is geconfigureerd op de client is 1 seconde. Uw toepassing aanvragen twee sleutels (bijvoorbeeld) "A" en "B") op hetzelfde moment (met behulp van de fysieke netwerkverbinding). De meeste clients ondersteunen 'Pipelining' van aanvragen, zodat zowel aanvragen "A" en "B" worden verzonden op de kabel met de server een achter elkaar zonder te wachten op voor de reacties. De server stuurt de antwoorden terug in dezelfde volgorde. Als antwoord "A" groot kunt is het meeste van de time-out voor volgende aanvragen eat. 

Het volgende voorbeeld ziet dit scenario. In dit scenario aanvraag "A" en "B" snel worden verzonden, de server begint met het verzenden van antwoorden "A" en "B" snel, maar vanwege overdrachtstijden van gegevens, "B" hangen achter de andere aanvraag en time-out ondanks dat de server snel heeft gereageerd.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Meting
Dit is een moeilijk te meten. U hebt in feite softwareontwikkelaars de clientcode om grote aanvragen en antwoorden te houden. 

#### <a name="resolution"></a>Oplossing
1. Redis is geoptimaliseerd voor een groot aantal kleine waarden in plaats van enkele grote waarden. De beste oplossing is om uw gegevens in de gerelateerde lagere waarden. Zie de [wat is het ideaal grootte van het waardebereik voor redis? 100KB te groot is? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) post een bericht voor meer informatie over waarom de lagere waarden worden aanbevolen.
2. Verhoog de grootte van uw virtuele machine (voor client en Server voor Redis-Cache), als u de mogelijkheden voor hogere bandbreedte, overdrachtstijden van gegevens voor grotere antwoorden verminderen. Houd er rekening mee dat het ophalen van meer bandbreedte op alleen de server of alleen op de client mogelijk niet voldoende. Meet het bandbreedteverbruik en vergelijk dit met de mogelijkheden van de grootte van virtuele machine die u momenteel hebt.
3. Verhoog het aantal `ConnectionMultiplexer` objecten u gebruik en round robin-aanvragen via andere verbindingen.

### <a name="what-happened-to-my-data-in-redis"></a>Waar vind ik mijn gegevens in Redis?
#### <a name="problem"></a>Probleem
Verwacht voor bepaalde gegevens worden in mijn Azure Redis-Cache-exemplaar, maar het niet lijkt te zijn er.

#### <a name="resolution"></a>Oplossing
Zie [waar vind ik mijn gegevens in Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) voor mogelijke oorzaken en oplossingen.

## <a name="server-side-troubleshooting"></a>Server side problemen oplossen
Deze sectie wordt beschreven voor het oplossen van problemen die vanwege een voorwaarde op de server van de cache optreden.

* [Geheugendruk op de server](#memory-pressure-on-the-server)
* [Hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load)
* [Server Side bandbreedte is overschreden](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Geheugendruk op de server
#### <a name="problem"></a>Probleem
Geheugendruk aan de serverzijde leidt tot allerlei soorten prestatieproblemen die de verwerking van aanvragen kunnen vertragen. Wanneer geheugendruk raakt, heeft het systeem meestal op de paginagegevens van het fysieke geheugen in het virtuele geheugen op de schijf. Dit *pagina met fout* zorgt ervoor dat het systeem aanzienlijk vertragen. Er zijn verschillende mogelijke oorzaken van dit geheugendruk: 

1. U hebt de cache van de volledige capaciteit ingevuld met gegevens. 
2. Redis ziet hoge geheugenfragmentatie - meestal veroorzaakt door het opslaan van grote objecten (Redis is geoptimaliseerd voor een kleine objecten - Zie de [wat is het ideaal grootte van het waardebereik voor redis? 100KB te groot is? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) post een bericht voor meer informatie). 

#### <a name="measurement"></a>Meting
Redis beschrijft de twee metrische gegevens kunt u dit probleem identificeren. De eerste is `used_memory` en de andere `used_memory_rss`. [Deze metrische gegevens](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) zijn beschikbaar in de Azure Portal of via de [Redis INFO](http://redis.io/commands/info) opdracht.

#### <a name="resolution"></a>Oplossing
Er zijn verschillende mogelijke wijzigingen die u kunt om te voorkomen dat het geheugengebruik in orde:

1. [Configureer een beleid geheugen](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) en verlopen tijdstippen instellen op uw sleutels. Houd er rekening mee dat dit mogelijk niet voldoende als er fragmentatie.
2. [Een waarde maxmemory gereserveerd configureert](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) die groot genoeg is om te compenseren voor geheugenfragmentatie is.
3. Verdeel uw grote objecten in de cache in kleinere verwante objecten.
4. [Schaal](cache-how-to-scale.md) in een groter formaat in de cache.
5. Als u een [premium cache met Redis-cluster ingeschakeld](cache-how-to-premium-clustering.md) kunt u [Verhoog het aantal shards](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Hoog CPU-gebruik / Server laden
#### <a name="problem"></a>Probleem
Hoog CPU-gebruik kan betekenen dat de client een antwoord van Redis op tijdige wijze verwerkt Hoewel Redis antwoord verzonden door het zeer snel kan mislukken.

#### <a name="measurement"></a>Meting
Het systeem Wide CPU-gebruik via de Azure Portal of via het bijbehorende prestatiemeteritem bewaken. Zorg ervoor dat u niet bewaken *proces* CPU omdat een enkel proces lage CPU-gebruik kan hebben op de tijd die het hele systeem CPU kan hoog zijn. Pieken in CPU-gebruik die met time-outs overeenkomen gecontroleerd.

#### <a name="resolution"></a>Oplossing
[Schaal](cache-how-to-scale.md) naar een grotere cache servicetier met meer CPU-capaciteit of onderzoeken wat de oorzaak van CPU, pieken. 

### <a name="server-side-bandwidth-exceeded"></a>Server Side bandbreedte is overschreden
#### <a name="problem"></a>Probleem
Exemplaren van verschillende grootte cache hebben hun beperkingen op de hoeveelheid netwerkbandbreedte ze beschikbaar zijn. Als de server de beschikbare bandbreedte overschrijdt, wordt er geen gegevens naar de client zo snel worden verzonden. Dit kan leiden tot time-outs.

#### <a name="measurement"></a>Meting
U kunt controleren de `Cache Read` metrische gegevens de hoeveelheid gegevens is gelezen uit de cache in MB per seconde (MB/s) tijdens het opgegeven interval voor rapportage. Deze waarde komt overeen met de netwerkbandbreedte gebruikt door deze cache. Als u waarschuwingen instellen voor server side netwerk bandbreedtelimieten wilt, u kunt ze maken gebruik van deze `Cache Read` teller. Vergelijk uw metingen met de waarden in [deze tabel](cache-faq.md#cache-performance) voor de waargenomen-bandbreedtelimieten voor verschillende cache lagen en grootten prijzen.

#### <a name="resolution"></a>Oplossing
Als u consistent in de buurt van de geobserveerde maximale bandbreedte voor de grootte van uw prijscategorie laag en de cache bent, kunt u overwegen [schalen](cache-how-to-scale.md) met behulp van de waarden in een prijscategorie laag, of grootte die een groter netwerkbandbreedte heeft [deze tabel](cache-faq.md#cache-performance)als richtlijn.

## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis time-out-uitzonderingen
Configuratie-instelling met de naam maakt gebruik van StackExchange.Redis `synctimeout` voor synchrone bewerkingen waarvoor een standaardwaarde van 1000 ms. Als een synchrone aanroep komt niet in de aangegeven tijd hebt voltooid, genereert de client StackExchange.Redis een time-outfout vergelijkbaar met het volgende voorbeeld.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Dit foutbericht bevat metrische gegevens die kunnen helpen bij het wijst u de oorzaak en de mogelijke oplossing van het probleem. De volgende tabel bevat details over de fout bericht metrische gegevens.

| Fout bericht metrische gegevens | Details |
| --- | --- |
| inst |In de afgelopen tijdsperiode: 0-opdrachten zijn uitgegeven |
| Mgr |Het uitvoeren van de socket-manager `socket.select` wat het betekent dat het besturingssysteem om aan te geven van een socket met iets te maken; vraagt in feite: de lezer is niet actief lezen vanaf het netwerk omdat deze niet denkt er is niets dat te maken |
| Wachtrij |Er zijn 73 totale voortgang-bewerkingen |
| Qu |6 van de bewerkingen in uitvoering zijn in de wachtrij voor niet-verzonden en nog niet zijn geschreven met het uitgaande netwerk |
| Qs |67 van he lopende bewerkingen zijn verzonden naar de server, maar een antwoord is nog niet beschikbaar. Kan het antwoord worden `Not yet sent by the server` of`sent by the server but not yet processed by the client.` |
| qc |0 van de bewerkingen in uitvoering antwoorden hebt gezien, maar nog niet is gemarkeerd als voltooid vanwege het wachten op de voltooiing lus |
| wR |Er is een actieve writer (wat betekent dat de 6 unsent aanvragen worden niet genegeerd) bytes/activewriters |
| in |Er zijn geen actieve lezers en nul bytes beschikbaar zijn voor worden gelezen op de NIC-bytes/activereaders |

### <a name="steps-to-investigate"></a>Stappen voor het onderzoeken van
1. Als best practice Controleer of u het volgende patroon gebruikt voor het verbinding maken wanneer u de client StackExchange.Redis.

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ````

    Zie voor meer informatie [verbinding maken met de cache met behulp van StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Zorg ervoor dat uw Azure Redis-Cache en de clienttoepassing in dezelfde regio in Azure. Bijvoorbeeld, u kunt zich voordoen time-outs wanneer uw cache in VS-Oost, maar de client zich in VS-West en de aanvraag niet voltooid binnen de `synctimeout` interval of u kunt zich voordoen time-outs wanneer u foutopsporing van uw lokale ontwikkelcomputer. 
   
    Het is raadzaam om de cache en in de client in dezelfde Azure-regio. Als u een scenario met aanroepen tussen regio hebt, moet u instellen de `synctimeout` interval op een waarde die hoger is dan het interval standaard 1000 ms door een `synctimeout` eigenschap in de verbindingstekenreeks. Het volgende voorbeeld ziet u een fragment StackExchange.Redis cache verbinding tekenreeks met een `synctimeout` van 2000 ms.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. Zorg ervoor dat u de nieuwste versie van de [NuGet-pakket StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die voortdurend wordt verholpen in de volgende code maakt u deze krachtiger voor time-outs dus belangrijk dat de meest recente versie is.
3. Als er aanvragen die ophalen door bandbreedtebeperkingen op de server of client gebonden zijn, duurt het langer om te voltooien en waardoor time-outs. Zie voor als de time-out als gevolg van de netwerkbandbreedte op de server is [bandbreedte van de Server-side overschreden](#server-side-bandwidth-exceeded). Zie voor als de time-out als gevolg van de netwerkbandbreedte client is [Client side bandbreedte overschreden](#client-side-bandwidth-exceeded).
4. U CPU ophalen gebonden zijn op de server of op de client?
   
   * Controleer als u aan de CPU op de client ervoor zorgen dat de aanvraag niet verwerkt gebonden ophalen kan binnen de `synctimeout` interval, hetgeen een time-out. Verplaatsen naar een groter formaat van de client of distributie van de belasting kunt u dit instellen. 
   * Controleer of uw CPU-gebonden op de server door de bewaking van de `CPU` [prestaties metrische gegevens in de cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Voor de configuratieaanvragen terwijl Redis CPU-gebonden kunnen leiden tot deze aanvragen voor time-out. Daarom kunt u de verdelen over meerdere shards in een premium-cache of bijwerken naar een groter formaat of prijscategorie. Zie voor meer informatie [Server Side bandbreedte overschreden](#server-side-bandwidth-exceeded).
5. Zijn er opdrachten duurt lang voordat op de server kan worden verwerkt? Langlopende opdrachten die het duurt lang worden verwerkt op de redis-server kan leiden tot time-outs. Enkele voorbeelden van langdurige opdrachten zijn `mget` met een groot aantal sleutels, `keys *` of slecht geschreven scripts lua. U kunt verbinding maken met uw Azure Redis-Cache-exemplaar met behulp van de client redis cli of de [Redis-Console](cache-configure.md#redis-console) en voer de [SlowLog](http://redis.io/commands/slowlog) opdracht uit om te zien of er aanvragen duurt langer dan verwacht. Redis-Server en StackExchange.Redis zijn geoptimaliseerd voor veel kleine aanvragen in plaats van minder grote aanvragen. Uw gegevens splitsen in kleinere reeksen kan dingen hier verbeteren. 
   
    Zie voor informatie over verbinding maken met de Azure Redis-Cache SSL-eindpunt met redis cli en stunnel, de [aangekondigd ASP.NET Session State-Provider voor de Preview-versie Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbericht. Zie voor meer informatie [SlowLog](http://redis.io/commands/slowlog).
6. Hoge belasting van de Redis-server kan leiden tot time-outs. U kunt de belasting van de server controleren door de bewaking van de `Redis Server Load` [prestaties metrische gegevens in de cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Belasting van de server van 100 (maximumwaarde) geeft aan dat de redis-server is bezet geen niet-actieve tijd verwerken van aanvragen. Als u wilt zien als bepaalde verzoeken zijn inneemt alle van de mogelijkheid van de server, voert u de opdracht SlowLog zoals beschreven in de vorige alinea. Zie voor meer informatie [hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load).
7. Is er een andere gebeurtenis aan de clientzijde die kan worden gelegd een blip netwerk? Controleer op de client (web, werkrol of een Iaas VM) als er een gebeurtenis is, zoals het aantal exemplaren van de client schaal omhoog of omlaag of implementeren van een nieuwe versie van de client of automatisch schalen is ingeschakeld? Bij onze tests die hebben we gevonden voor automatisch schalen of omhoog/omlaag schalen kan veroorzaken kan uitgaande netwerkverbinding verloren zijn enkele seconden. StackExchange.Redis code is tegen dergelijke gebeurtenissen en verbinding wordt hersteld. Alle aanvragen in de wachtrij kunnen gedurende deze tijd nieuwe verbinding een time-out.
8. Is er een grote aanvraag diverse kleine aanvragen voorafgaand aan de Redis-Cache waarvoor een time-out? De parameter `qs` in de volgende fout bericht vertelt u hoeveel aanvragen naar de server van de client zijn verzonden, maar nog niet zijn verwerkt op een antwoord. Deze waarde kan blijven groeien omdat StackExchange.Redis één TCP-verbinding gebruikt en alleen van een reactie op een tijdstip lezen kan. Hoewel er is een time-out opgetreden voor de eerste bewerking, stopt niet de gegevens worden verzonden vanaf de server en andere aanvragen worden geblokkeerd totdat dit proces is voltooid, waardoor een time-out. Eén oplossing is de kans op time-outs minimaliseren door ervoor te zorgen dat uw cache groot genoeg is voor uw workload is en grote waarden splitsen in kleinere reeksen. Een andere mogelijke oorzaak is het gebruik van een pool van `ConnectionMultiplexer` in uw client-objecten en kies de minste geladen `ConnectionMultiplexer` bij het verzenden van een nieuwe aanvraag. Hierdoor moet een enkel time-out veroorzaakt door andere aanvragen voor ook time-out.
9. Als u `RedisSessionStateprovider`, controleert u de time-out opnieuw correct hebt ingesteld. `retrytimeoutInMilliseconds`moet hoger zijn dan `operationTimeoutinMilliseonds`, anders geen nieuwe pogingen wordt uitgevoerd. In het volgende voorbeeld `retrytimeoutInMilliseconds` is ingesteld op 3000. Zie voor meer informatie [ASP.NET Session State-Provider voor Azure Redis-Cache](cache-aspnet-session-state-provider.md) en [het gebruik van de parameters voor de configuratie van de sessiestatus-Provider en de Provider voor de uitvoercache](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


1. Controleer geheugengebruik op de server van Azure Redis-Cache door [bewaking](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` en `Used Memory`. Als een beleid voor verwijdering gemaakt is, Redis wordt gestart wanneer onbeschikbaar maken van sleutels `Used_Memory` de cachegrootte is bereikt. In het ideale geval `Used Memory RSS` moet slechts iets hoger dan `Used memory`. Een grote verschil betekent dat er geheugenfragmentatie (intern of extern. Wanneer `Used Memory RSS` is minder dan `Used Memory`, betekent dit deel van het cachegeheugen is door het besturingssysteem zijn gewisseld. In dat geval kunt u een aantal belangrijke latenties verwachten. Omdat Redis geen controle heeft over hoe de toewijzingen zijn toegewezen aan geheugenpagina's, hoge `Used Memory RSS` is vaak het resultaat van een piek in geheugengebruik. Redis maakt vrij geheugen, het geheugen terug naar de toewijzingsfunctie wordt gegeven als de toewijzingsfunctie kan of kan geen geven het geheugen terug op het systeem. Mogelijk zijn er een discrepantie is tussen de `Used Memory` verbruik waarde en geheugen zoals gemeld door het besturingssysteem. Kan zijn vanwege het feit geheugen is gebruikt en vrijgegeven door Redis, maar niet gegeven weer aan het systeem. U kunt de volgende stappen uitvoeren om te voorkomen dat geheugenproblemen.
   
   * De cache bijwerken naar een groter, zodat u niet mogelijkheden geheugenbeperkingen worden uitgevoerd op het systeem.
   * Geldigheidsduur van de sleutels zo instellen dat oudere waarden proactief zijn verwijderd.
   * Monitor voor de de `used_memory_rss` metrische gegevens in de cache. Wanneer deze waarde de grootte van het cachegeheugen nadert, bent u waarschijnlijk beginnen te zien van prestatieproblemen. De gegevens verdelen over meerdere shards, als u met behulp van een premium-cache, of een upgrade uitvoert naar een grotere cachegrootte.
   
   Zie voor meer informatie [geheugendruk op de server](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Aanvullende informatie
* [Welk aanbod voor de Redis-cache en welke cachegrootte moet ik kiezen?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
* [Hoe kan ik benchmark en test de prestaties van mijn cache?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Hoe kan ik de Redis-opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)
* [Azure Redis-Cache controleren](cache-how-to-monitor.md)

