---
title: Problemen oplossen met Azure Cache voor Redis | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende problemen met Azure Cache voor Redis.
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: azure-cache-for-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: wesmc
ms.openlocfilehash: fd5e62138d47622417bde658bf0d05308594d64e
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104145"
---
# <a name="how-to-troubleshoot-azure-cache-for-redis"></a>Problemen oplossen met Azure Cache voor Redis
Dit artikel bevat richtlijnen voor het oplossen van de volgende categorieën van Azure Cache voor problemen met Redis.

* [Het oplossen van client-side](#client-side-troubleshooting) : in deze sectie bevat richtlijnen voor het identificeren van en het oplossen van problemen veroorzaakt door de toepassing verbinding maakt met Azure Cache voor Redis.
* [Het oplossen van server-side](#server-side-troubleshooting) : in deze sectie bevat richtlijnen voor het identificeren van en het oplossen van problemen veroorzaakt op de Azure-Cache voor Redis-serverzijde.
* [De time-outuitzonderingen StackExchange.Redis](#stackexchangeredis-timeout-exceptions) -deze sectie bevat informatie over het oplossen van problemen bij het gebruik van de StackExchange.Redis-client.

> [!NOTE]
> Aantal van de stappen in deze handleiding bevat instructies voor het Redis-opdrachten uitvoeren en controleren van verschillende maatstaven voor prestaties. Zie voor meer informatie en instructies, de artikelen in de [aanvullende informatie](#additional-information) sectie.
> 
> 

## <a name="client-side-troubleshooting"></a>Het oplossen van client-side
Deze sectie wordt beschreven voor het oplossen van problemen die vanwege een voorwaarde voor de clienttoepassing optreden.

* [Geheugendruk op de client](#memory-pressure-on-the-client)
* [Enorme verkeerspiek](#burst-of-traffic)
* [Hoge client CPU-gebruik](#high-client-cpu-usage)
* [Client-Side bandbreedte is overschreden](#client-side-bandwidth-exceeded)
* [Grootte van grote aanvraag/antwoord](#large-requestresponse-size)
* [Wat is er gebeurd met mijn gegevens in Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Geheugendruk op de client
#### <a name="problem"></a>Probleem
Geheugendruk op de clientcomputer leidt tot alle soorten prestatieproblemen die verwerking van gegevens die zijn verzonden door de Redis-exemplaar zonder enige vertraging kunnen worden vertraagd. Wanneer geheugendruk raakt, heeft het systeem meestal tot paginagegevens van het fysieke geheugen in het virtuele geheugen, die op schijf. Dit *pagina fout* zorgt ervoor dat het systeem aanzienlijk vertragen.

#### <a name="measurement"></a>Meting
1. Geheugengebruik op de machine om ervoor te zorgen dat het beschikbare geheugen niet overschrijdt. 
2. Monitor de `Page Faults/Sec` prestatiemeteritem. De meeste systemen voor sommige wisselbestandsfouten hebben, zelfs tijdens normale werking, dus bekijken voor pieken in dit prestatiemeteritem paginafouten die met de time-outs overeenkomen.

#### <a name="resolution"></a>Oplossing
Werk de client bij naar een grotere client VM-grootte met meer geheugen of verdiepen in uw gebruikspatronen geheugen geheugen worden verkleind.

### <a name="burst-of-traffic"></a>Enorme verkeerspiek
#### <a name="problem"></a>Probleem
Afhandelen van pieken in combinatie met slecht `ThreadPool` instellingen kunnen leiden tot vertragingen bij het verwerken van gegevens die al door de Redis-Server worden verzonden, maar nog niet op de client verbruikt.

#### <a name="measurement"></a>Meting
Monitor voor hoe uw `ThreadPool` statistieken wijzigen gedurende een periode met behulp van code [zoals deze](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). U kunt ook zoeken op de `TimeoutException` bericht van StackExchange.Redis. Hier volgt een voorbeeld:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Er zijn verschillende problemen die interessant zijn in het bovenstaande bericht:

1. U ziet dat in de `IOCP` sectie en de `WORKER` sectie die u hebt een `Busy` waarde die groter is dan de `Min` waarde. Dit verschil betekent dat uw `ThreadPool` instellingen moeten aanpassen.
2. U kunt ook zien `in: 64221`. Deze waarde geeft aan dat 64,211 bytes zijn ontvangen op het niveau van de kernel socket maar nog niet zijn gelezen door de toepassing (bijvoorbeeld StackExchange.Redis). Dit verschil betekent meestal dat uw toepassing wordt niet van gegevens vanaf het netwerk zo snel lezen als de server is naar u te verzenden.

#### <a name="resolution"></a>Oplossing
Configureer uw [ThreadPool instellingen](https://gist.github.com/JonCole/e65411214030f0d823cb) om ervoor te zorgen dat uw ThreadPool omhoog wordt geschaald uitgaande snel onder burst-scenario's.

### <a name="high-client-cpu-usage"></a>Hoge client CPU-gebruik
#### <a name="problem"></a>Probleem
Hoog CPU-gebruik op de client wordt aangegeven dat het systeem niet kan bijhouden met het werk dat deze is gevraagd om uit te voeren. Deze situatie betekent dat de client voor het verwerken van een reactie van Redis in tijdig Hoewel Redis het antwoord snel verzonden kan mislukken.

#### <a name="measurement"></a>Meting
De Wide systeem-CPU-gebruik via de Azure Portal of via de bijbehorende prestatiemeteritem controleren. Zorg ervoor dat u niet bewaken *proces* CPU omdat een enkel proces lage CPU-gebruik kan hebben op de tijd die het hele systeem CPU kan hoog zijn. Bekijk voor pieken in het CPU-gebruik die met de time-outs overeenkomen. Als gevolg van intensief CPU, ziet u mogelijk ook hoge `in: XXX` waarden in `TimeoutException` foutberichten bij zoals beschreven in de [enorme verkeerspiek](#burst-of-traffic) sectie.

> [!NOTE]
> StackExchange.Redis 1.1.603 en nieuwer, bevat de `local-cpu` metrische in `TimeoutException` foutberichten. Zorg ervoor dat u met behulp van de meest recente versie van de [NuGet-pakket StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die zijn voortdurend wordt verholpen in de volgende code maakt u deze krachtiger voor time-outs wanneer u de meest recente versie is dus belangrijk.
> 
> 

#### <a name="resolution"></a>Oplossing
Een upgrade uitvoert naar een grotere VM-grootte met meer CPU-capaciteit of onderzoeken wat de oorzaak is van CPU, pieken. 

### <a name="client-side-bandwidth-exceeded"></a>Client-side bandbreedte is overschreden
#### <a name="problem"></a>Probleem
Afhankelijk van de architectuur van clientcomputers, kunnen ze hebben beperkingen op de hoeveelheid netwerkbandbreedte ze beschikbaar zijn. Als de client de beschikbare bandbreedte overschrijdt door overbelasting van netwerkcapaciteit, worden klikt u vervolgens gegevens niet verwerkt op de client zo snel als de server het verzendt. Deze situatie kan leiden tot time-outs.

#### <a name="measurement"></a>Meting
Controleren hoe uw bandbreedtegebruik wijzigen gedurende een periode met behulp van code [zoals deze](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Deze code kan niet worden uitgevoerd in sommige omgevingen met beperkte machtigingen (zoals Azure web sites).

#### <a name="resolution"></a>Oplossing
Client VM-grootte vergroten of verkleinen van bandbreedte.

### <a name="large-requestresponse-size"></a>Grootte van grote aanvraag/antwoord
#### <a name="problem"></a>Probleem
Een grote aanvraag/antwoord kan leiden tot time-outs. Stel bijvoorbeeld dat uw time-outwaarde geconfigureerd op de client is 1 seconde. Uw toepassing worden aangevraagd twee sleutels (bijvoorbeeld, "A" en "B") op hetzelfde moment (met behulp van de fysieke netwerkverbinding). De meeste clients ondersteunen "Pipelining" van aanvragen, zodat beide aanvragen "A" en "B" worden verzonden op de kabel met de server een na de andere zonder te wachten op de antwoorden. De server verzendt de antwoorden weer in dezelfde volgorde. Als antwoord "A" groot genoeg is is, kan het deel van de time-out voor volgende aanvragen eten. 

Het volgende voorbeeld ziet u dit scenario. In dit scenario aanvraag "A" en "B" snel worden verzonden, start de server het snel verzenden van reacties "A" en "B", maar vanwege gegevens de overdrachtstijd, "B" zitten achter de andere aanvraag en time-out ook al snel door de server heeft gereageerd.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Meting
Deze aanvraag/antwoord is moeilijk om te meten. U moet in feite instrumenteren van uw clientcode voor het volgen van grote aanvragen en antwoorden. 

#### <a name="resolution"></a>Oplossing
1. Redis is geoptimaliseerd voor een groot aantal kleine waarden, in plaats van enkele grote waarden. De beste oplossing is het opsplitsen van uw gegevens in de gerelateerde lagere waarden. Zie de [wat is de ideale grootte van het waardebereik voor redis? 100 KB te groot is? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) plaatsen voor meer informatie over waarom lagere waarden worden aanbevolen.
2. Verhoog de grootte van uw virtuele machine (voor client en de Azure-Cache voor Redis-Server), om op te halen van de hogere bandbreedte-mogelijkheden, waardoor gegevens de overdrachtstijd voor grotere antwoorden. Ophalen van meer bandbreedte op alleen de server of alleen op de client mogelijk niet voldoende. Uw gebruik van netwerkbandbreedte te meten en vergelijken het met de mogelijkheden van de grootte van virtuele machine die u momenteel hebt.
3. Verhoog het aantal `ConnectionMultiplexer` objecten u gebruik en round robin-aanvragen via verschillende verbindingen.

### <a name="what-happened-to-my-data-in-redis"></a>Wat is er gebeurd met mijn gegevens in Redis?
#### <a name="problem"></a>Probleem
Ik verwacht voor bepaalde gegevens in mijn Azure-Cache voor Redis-exemplaar, maar het is niet lijkt het alsof er.

#### <a name="resolution"></a>Oplossing
Zie [wat is er gebeurd met mijn gegevens in Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) voor mogelijke oorzaken en oplossingen.

## <a name="server-side-troubleshooting"></a>Server side problemen oplossen
Deze sectie wordt beschreven voor het oplossen van problemen die vanwege een voorwaarde op de cacheserver optreden.

* [Geheugendruk op de server](#memory-pressure-on-the-server)
* [Hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load)
* [Server Side bandbreedte is overschreden](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Geheugendruk op de server
#### <a name="problem"></a>Probleem
Geheugendruk op de server leidt tot alle soorten prestatieproblemen die verwerking van aanvragen kunnen vertragen. Wanneer geheugendruk raakt, heeft het systeem meestal tot paginagegevens van het fysieke geheugen in het virtuele geheugen, die op schijf. Dit *pagina fout* zorgt ervoor dat het systeem aanzienlijk vertragen. Er zijn verschillende mogelijke oorzaken van dit geheugendruk: 

1. U kunt de cache van de volledige capaciteit hebt ingevuld met gegevens. 
2. Redis ziet hoge geheugenfragmentatie - meestal veroorzaakt door het opslaan van grote objecten (Redis is geoptimaliseerd voor een kleine objecten - Zie de [wat is de ideale grootte van het waardebereik voor redis? 100 KB te groot is? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) plaatsen voor meer informatie). 

#### <a name="measurement"></a>Meting
Redis wordt aangegeven dat twee metrische gegevens waarmee u dit probleem te identificeren. De eerste is `used_memory` en de andere is `used_memory_rss`. [Deze metrische gegevens](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) zijn beschikbaar in de Azure Portal of via de [Redis INFO](https://redis.io/commands/info) opdracht.

#### <a name="resolution"></a>Oplossing
Er zijn verschillende mogelijke wijzigingen die u maken kunt om te geheugengebruik in orde houden:

1. [Configureer een beleid geheugen](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) en vervaldatum tijden instellen voor uw sleutels. Deze configuratie is mogelijk niet voldoende als er fragmentatie.
2. [Configureren van een waarde maxmemory-gereserveerde](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) die groot genoeg is om te compenseren voor het geheugen gefragmenteerd is.
3. Splitst u uw grote objecten in de cache in kleinere verwante objecten.
4. [Schaal](cache-how-to-scale.md) in een groter formaat in de cache.
5. Als u een [premium-cache met Redis-cluster ingeschakeld](cache-how-to-premium-clustering.md), kunt u [Verhoog het aantal shards](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Hoog CPU-gebruik / Server laden
#### <a name="problem"></a>Probleem
Hoog CPU-verbruik kan betekenen dat de client kan niet worden verwerkt een reactie van Redis in tijdig Hoewel Redis snel de reactie verzonden.

#### <a name="measurement"></a>Meting
De Wide systeem-CPU-gebruik via de Azure Portal of via de bijbehorende prestatiemeteritem controleren. Zorg ervoor dat u niet bewaken *proces* CPU omdat een enkel proces lage CPU-gebruik kan hebben op de tijd die het hele systeem CPU kan hoog zijn. Bekijk voor pieken in het CPU-gebruik die met de time-outs overeenkomen.

#### <a name="resolution"></a>Oplossing
* Bekijk alle advies en waarschuwingen die worden vermeld de [Azure Cache voor Advisor Redis](cache-configure.md#azure-cache-for-redis-advisor).
* Bekijk ook de andere aanbevelingen in dit onderwerp en [aanbevolen procedures voor Azure Redis](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f) om te zien als u alle opties voor het verder optimaliseren van uw cache en de client hebt gebruikt. 
* Controleer de [Azure Cache voor Redis-prestaties](cache-faq.md#azure-cache-for-redis-performance) grafieken en om te zien als u in de buurt van de bovenste drempelwaarden van uw huidige laag. Indien nodig, [schaal](cache-how-to-scale.md) naar een grotere cachelaag met meer CPU-capaciteit. Als u Premium-laag al gebruikt, kunt u [uitschalen met clustering](cache-how-to-premium-clustering.md)


### <a name="server-side-bandwidth-exceeded"></a>Server Side bandbreedte is overschreden
#### <a name="problem"></a>Probleem
Afhankelijk van de grootte van de cache-instanties, kunnen ze hebben beperkingen op de hoeveelheid netwerkbandbreedte ze beschikbaar zijn. Als de server de beschikbare bandbreedte overschrijdt, klikt u vervolgens gegevens niet verzonden naar de client zo snel. Deze situatie kan leiden tot time-outs.

#### <a name="measurement"></a>Meting
U kunt controleren de `Cache Read` metrische gegevens de hoeveelheid gegevens is gelezen uit de cache in MB per seconde (MB/s) tijdens het opgegeven interval voor rapportage. Deze waarde komt overeen met de netwerkbandbreedte die wordt gebruikt door deze cache. Als u instellen van waarschuwingen voor bandbreedtelimieten van server-side '-netwerk wilt, kunt u maken met behulp van dit `Cache Read` teller. Vergelijk uw metingen met de waarden in [deze tabel](cache-faq.md#cache-performance) voor de waargenomen bandbreedtelimieten voor verschillende cache prijzen van lagen en -grootten.

#### <a name="resolution"></a>Oplossing
Als u consistent in de buurt van de geobserveerde maximale bandbreedte voor de grootte van uw prijzen laag en de cache bent, kunt u overwegen [schalen](cache-how-to-scale.md) naar een prijsstelling laag of grootte met meer bandbreedte van het netwerk met behulp van de waarden in [deze tabel](cache-faq.md#cache-performance)als richtlijn.

## <a name="stackexchangeredis-timeout-exceptions"></a>De time-outuitzonderingen StackExchange.Redis
Een configuratie-instelling met de naam maakt gebruik van StackExchange.Redis `synctimeout` voor synchrone bewerkingen waarvoor een standaardwaarde van 1000 ms. Als een synchrone aanroep wordt niet voltooid binnen de tijd sorteert, genereert de StackExchange.Redis-client een time-outfout die vergelijkbaar is met het volgende voorbeeld:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Dit foutbericht bevat metrische gegevens waarmee u verwijzen naar de oorzaak en de mogelijke resolutie van het probleem. De volgende tabel bevat details over de fout bericht metrische gegevens.

| Fout bericht metrische gegevens | Details |
| --- | --- |
| inst |In de afgelopen tijdsperiode: 0-opdrachten zijn uitgegeven |
| Mgr |Het uitvoeren van de manager socket `socket.select`, wat betekent dat deze vraag is het besturingssysteem om aan te geven van een socket dat er iets niet; in feite: de lezer is niet actief lezen vanaf het netwerk omdat het niet denkt er dat te doen |
| wachtrij |Er zijn 73 totaal aantal lopende bewerkingen |
| Qu |6 van de bewerkingen wordt uitgevoerd in de wachtrij staan niet verzonden en nog niet is geschreven met de uitgaande netwerk |
| Qs |67 van de bewerkingen in uitvoering zijn verzonden naar de server, maar een antwoord is nog niet beschikbaar. Het antwoord kan worden `Not yet sent by the server` of `sent by the server but not yet processed by the client.` |
| qc |0 van de bewerkingen in uitvoering antwoorden hebt gezien, maar nog niet is gemarkeerd als voltooid vanwege het wachten op de lus is voltooid |
| wR |Er is een actieve writer (wat betekent dat het 6 unsent aanvragen worden niet genegeerd) bytes/activewriters |
| in |Er zijn geen actieve lezers en nul bytes zijn beschikbaar om te lezen op de NIC-bytes/activereaders |

### <a name="steps-to-investigate"></a>Stappen voor het onderzoeken
1. Als een best practice ervoor te zorgen, u het volgende patroon gebruikt voor het verbinding maken bij het gebruik van de StackExchange.Redis-client.

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

    Zie voor meer informatie, [verbinding maken met de cache met behulp van StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Zorg ervoor dat uw Azure-Cache voor Redis en de clienttoepassing in dezelfde regio in Azure. Bijvoorbeeld, u mogelijk worden ophalen time-outs wanneer uw cache in VS-Oost is, maar de client zich in VS-West en de aanvraag wordt niet voltooid binnen de `synctimeout` interval of u mogelijk worden ophalen time-outs wanneer u fouten in uw lokale ontwikkelcomputer opspoort. 
   
    Het is raadzaam om de cache en in de client in dezelfde Azure-regio. Als u een scenario met een andere regio's aanroepen hebt, moet u instellen de `synctimeout` interval op een waarde die hoger is dan het interval standaard 1000 ms door een `synctimeout` eigenschap in de verbindingsreeks. Het volgende voorbeeld ziet u een StackExchange.Azure Cache voor Redis connection string codefragment met een `synctimeout` van 2000 ms.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. Zorg ervoor dat u met behulp van de meest recente versie van de [NuGet-pakket StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die zijn voortdurend wordt verholpen in de volgende code maakt u deze krachtiger voor time-outs wanneer u de meest recente versie is dus belangrijk.
3. Als er aanvragen voor het ophalen van aan de bandbreedtebeperkingen op de server of client gebonden, duurt het langer om ze te voltooien en waardoor time-outs. Om te zien of de time-out vanwege de netwerkbandbreedte op de server, Zie [bandbreedte van de Server-side overschreden](#server-side-bandwidth-exceeded). Om te zien of de time-out vanwege netwerkbandbreedte client, Zie [Client-side-bandbreedte overschreden](#client-side-bandwidth-exceeded).
4. Bent u aan CPU op de server of op de client gekoppeld?
   
   * Controleer als u aan de CPU op de client, die ervoor zorgen dat de aanvraag niet verwerkt gebonden ophalen kan binnen de `synctimeout` interval, hetgeen een time-out. Verplaatsen naar een groter formaat van de client of distributie van de belasting kan voor het beheren van dit probleem helpen. 
   * Controleer of u wel CPU gebonden op de server door de bewaking van de `CPU` [metrische prestatiegegevens voor weergave in de cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Aanvragen die afkomstig zijn Redis CPU-afhankelijk is, kunnen leiden tot deze aanvragen voor time-out. Om dit probleem op te lossen, kunt u de verdelen over meerdere shards met clusters in een premium-cache of een upgrade uitvoert naar een groter formaat of de prijscategorie. Zie voor meer informatie, [Server Side bandbreedte overschreden](#server-side-bandwidth-exceeded).
5. Zijn er nog opdrachten duurt langer verwerking op de server? De uitvoering lang duurt opdrachten die lange tijd voor het verwerken van op de redis-server kan leiden tot time-outs. Enkele voorbeelden van langlopende opdrachten zijn `mget` met grote aantallen sleutels, `keys *` of slecht geschreven scripts lua. U kunt verbinding maken met uw Azure-Cache voor Redis-exemplaar met behulp van de redis cli-client of de [Redis-Console](cache-configure.md#redis-console) en voer de [SlowLog](https://redis.io/commands/slowlog) opdracht uit om te zien of er aanvragen die langer duren dan verwacht. Redis-Server en StackExchange.Redis zijn geoptimaliseerd voor een groot aantal kleine aanvragen in plaats van aanvragen voor minder grote. Uw gegevens splitsen in kleinere chunks kan dingen die hier worden verbeterd. 
   
    Zie voor meer informatie over verbinding maken met de Azure-Cache voor Redis-SSL-eindpunt met behulp van redis cli en beveiligde tunnel de [aankondiging van ASP.NET-Sessiestatusprovider voor Preview-versie Redis](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbericht. Zie voor meer informatie, [SlowLog](https://redis.io/commands/slowlog).
6. Hoge belasting van de Redis-server kan leiden tot time-outs. U kunt de belasting van de server bewaken door de bewaking van de `Redis Server Load` [metrische prestatiegegevens voor weergave in de cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Een belasting van de server van 100 (maximale waarde) geeft aan dat de redis-server bezet zijn, geen niet-actieve tijd is, verwerken van aanvragen. Als u wilt zien als bepaalde aanvragen van alle van de capaciteit van de server volgen, voert u de opdracht SlowLog, zoals beschreven in de vorige alinea. Zie voor meer informatie, [hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load).
7. Is er een andere gebeurtenis aan de clientzijde die een blip netwerk kan mogelijk veroorzaakt? Controleren op de client (web, werkrol of een Iaas-VM) als er een gebeurtenis is, zoals het aantal clientexemplaren schaal omhoog of omlaag of implementeren van een nieuwe versie van de client of automatisch schalen is ingeschakeld? Bij onze tests, kunt we hebben ontdekt dat automatisch schalen of omhoog/omlaag oorzaak uitgaande netwerkconnectiviteit kan verloren gaan enkele seconden. StackExchange.Redis code bestand is tegen dergelijke gebeurtenissen en opnieuw verbinding maakt. Gedurende deze tijd van de verbinding, kunnen geen aanvragen in de wachtrij een time-out.
8. Is er een grote aanvraag voorafgaand aan verschillende kleine aanvragen met de Azure Cache voor Redis waarvoor een time-out? De parameter `qs` in de volgende fout bericht vertelt u hoeveel aanvragen naar de server van de client zijn verzonden, maar nog niet zijn verwerkt op een reactie. Deze waarde kan blijven toenemen omdat StackExchange.Redis één TCP-verbinding gebruikt en één antwoord kan alleen worden gelezen op een tijdstip. Hoewel er is een time-out opgetreden voor de eerste bewerking, stopt niet de gegevens die worden verzonden naar/van de server en andere aanvragen worden geblokkeerd totdat de grote aanvraag is voltooid, veroorzaakt door een time-out. Eén oplossing is de kans op time-outs minimaliseren door ervoor te zorgen dat uw cache groot genoeg zijn voor uw werkbelasting is en grote waarden te splitsen in kleinere chunks. Een andere mogelijke oplossing is het gebruik van een pool van `ConnectionMultiplexer` objecten in uw client, en kies het minste geladen `ConnectionMultiplexer` bij het verzenden van een nieuwe aanvraag. Hierdoor moet een enkel time-out van andere aanvragen voor time-out van ook veroorzaakt.
9. Als u `RedisSessionStateprovider`, zorg ervoor dat u de time-out voor de nieuwe pogingen correct hebt ingesteld. `retrytimeoutInMilliseconds` moet hoger zijn dan `operationTimeoutinMilliseonds`, anders worden er geen nieuwe pogingen uitgevoerd. In het volgende voorbeeld `retrytimeoutInMilliseconds` is ingesteld op 3000. Zie voor meer informatie, [ASP.NET-Sessiestatusprovider voor Azure Cache voor Redis](cache-aspnet-session-state-provider.md) en [over het gebruik van de parameters voor de configuratie van de Sessiestatusprovider en Cacheprovider voor uitvoer](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

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


1. Geheugengebruik op de Azure-Cache voor Redis-server door te controleren [bewaking](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` en `Used Memory`. Als een beleid van onbeschikbaar ingesteld is, Redis wordt gestart wanneer onbeschikbaar maken van sleutels `Used_Memory` de cachegrootte is bereikt. In het ideale geval `Used Memory RSS` moet worden slechts iets hoger dan `Used memory`. Een groot verschil betekent dat er geheugenfragmentatie (intern of extern). Wanneer `Used Memory RSS` is minder dan `Used Memory`, betekent dit dat onderdeel van het cache-geheugen is door het besturingssysteem zijn gewisseld. Als deze wisselen optreedt, kunt u verwachten dat sommige aanzienlijke latentie. Omdat Redis heeft geen controle over de wijze waarop de toewijzingen zijn toegewezen aan geheugenpagina's, hoge `Used Memory RSS` is vaak het resultaat van een piek in het geheugengebruik. Als Redis wordt vrijgemaakt geheugen, het geheugen wordt weer gegeven aan de toewijzingsfunctie en de toewijzingsfunctie kan of kan niet geven het geheugen terug naar het systeem. Mogelijk zijn er een verschil tussen de `Used Memory` verbruik van waarde en geheugen zoals gemeld door het besturingssysteem. Kan het zijn vanwege het feit geheugen is gebruikt en die zijn uitgebracht door Redis, maar niet de opgegeven terug naar het systeem. Om te beperken geheugenproblemen, kunt u de volgende stappen uitvoeren:
   
   * De cache bijwerken naar een groter formaat, zodat u geen verschil tussen geheugenbeperkingen worden uitgevoerd op het systeem.
   * Vervaldatum tijden voor de sleutels zo instellen dat oudere waarden proactief zijn verwijderd.
   * Monitor de `used_memory_rss` metrische gegevens in de cache. Wanneer deze waarde de grootte van het cachegeheugen nadert, bent u waarschijnlijk beginnen te zien van prestatieproblemen. De gegevens verdelen over meerdere shards, als u met behulp van een premium-cache, of een upgrade uitvoert naar een groter formaat in de cache.
   
   Zie voor meer informatie, [geheugendruk op de server](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Aanvullende informatie
* [Welke Azure Cache voor Redis-aanbieding en de grootte moet ik gebruiken?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Hoe kan ik benchmark-test uitvoeren en testen van de prestaties van mijn cache?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Hoe kan ik de Redis-opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)
* [Azure Cache controleren voor Redis](cache-how-to-monitor.md)

