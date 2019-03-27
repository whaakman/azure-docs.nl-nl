---
title: Problemen oplossen met Azure Cache voor Redis | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende problemen met Azure Cache voor Redis.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/15/2019
ms.author: yegu
ms.openlocfilehash: 838fc1da3e167d1df04fbb36a2fea33b8ac248a4
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482603"
---
# <a name="how-to-troubleshoot-azure-cache-for-redis"></a>Problemen oplossen met Azure Cache voor Redis

In dit artikel helpt bij het oplossen van verschillende categorieën van problemen die optreden kunnen bij het verbinden met Azure Cache voor instanties van Redis.

- [Het oplossen van client-side](#client-side-troubleshooting) kunt identificeren en oplossen van problemen in de toepassing verbinding te maken met uw cache.
- [Het oplossen van server-side](#server-side-troubleshooting) kunt identificeren en oplossen van problemen die in de Azure-Cache voor Redis-zijde optreden.
- [Verlies van gegevens oplossen](#data-loss-troubleshooting) helpt bij het identificeren en oplossen van incidenten waar de sleutels zijn verwacht maar niet gevonden in de cache.
- [De time-outuitzonderingen StackExchange.Redis](#stackexchangeredis-timeout-exceptions) bevat specifieke richtlijnen voor het oplossen van problemen met de StackExchange.Redis-bibliotheek.

> [!NOTE]
> Aantal van de stappen in deze handleiding bevat instructies voor het Redis-opdrachten uitvoeren en controleren van verschillende maatstaven voor prestaties. Zie voor meer informatie en instructies, de artikelen in de [aanvullende informatie](#additional-information) sectie.
>
>

## <a name="client-side-troubleshooting"></a>Het oplossen van client-side

Deze sectie wordt beschreven voor het oplossen van problemen die vanwege een voorwaarde voor de clienttoepassing optreden.

- [Geheugendruk op de client](#memory-pressure-on-the-client)
- [Enorme verkeerspiek](#burst-of-traffic)
- [Hoge client CPU-gebruik](#high-client-cpu-usage)
- [Client-Side bandbreedte is overschreden](#client-side-bandwidth-exceeded)
- [Grootte van grote aanvraag/antwoord](#large-requestresponse-size)

### <a name="memory-pressure-on-the-client"></a>Geheugendruk op de client

Geheugendruk op de clientcomputer leidt tot alle soorten prestatieproblemen van antwoorden uit de cache kunnen vertragen. Wanneer de geheugendruk raakt, kan het systeem gegevens naar schijf pagina. Dit _pagina fout_ zorgt ervoor dat het systeem aanzienlijk vertragen.

Voor het detecteren van geheugendruk op de client:

- Geheugengebruik op de machine om ervoor te zorgen dat het beschikbare geheugen niet groter is dan bewaken.
- Controleren van de client `Page Faults/Sec` prestatiemeteritem. Tijdens normale werking hebben de meeste systemen voor sommige wisselbestandsfouten. Geheugendruk kunnen duiden op pieken in paginafouten overeenkomt met de aanvraag time-outs.

Hoge geheugenbelasting op de client kan op verschillende manieren worden verholpen:

- Bestudeer de gebruikspatronen van uw geheugen worden verkleind geheugen op de client.
- Uw client VM bijwerken naar een groter formaat met meer geheugen.

### <a name="burst-of-traffic"></a>Enorme verkeerspiek

Afhandelen van pieken in combinatie met slecht `ThreadPool` instellingen kunnen leiden tot vertragingen bij het verwerken van gegevens die al door de Redis-Server worden verzonden, maar nog niet op de client verbruikt.

Monitor voor hoe uw `ThreadPool` statistieken na verloop van tijd veranderen [een voorbeeld `ThreadPoolLogger` ](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). U kunt `TimeoutException` berichten van StackExchange.Redis, zoals hieronder verder onderzoeken:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0,
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Er zijn verschillende problemen die interessant zijn in de voorgaande uitzondering:

- U ziet dat in de `IOCP` sectie en de `WORKER` sectie die u hebt een `Busy` waarde die groter is dan de `Min` waarde. Dit verschil betekent dat uw `ThreadPool` instellingen moeten aanpassen.
- U kunt ook zien `in: 64221`. Deze waarde geeft aan dat 64,211 bytes op van de client kernel socket-laag zijn ontvangen, maar door de toepassing nog niet hebt gelezen. Dit verschil betekent meestal dat uw toepassing (bijvoorbeeld StackExchange.Redis) is niet van gegevens vanaf het netwerk zo snel lezen als de server is naar u te verzenden.

U kunt [configureren uw `ThreadPool` instellingen](https://gist.github.com/JonCole/e65411214030f0d823cb) om ervoor te zorgen dat uw ThreadPool omhoog wordt geschaald uitgaande snel onder burst-scenario's.

### <a name="high-client-cpu-usage"></a>Hoge client CPU-gebruik

Hoge client CPU-gebruik geeft aan dat het systeem niet kan bijhouden met het werk dat deze is gevraagd om te doen. Zelfs als de cache wordt snel antwoord verzonden, kan de client voor het verwerken van het antwoord op tijdige wijze mislukken.

Bewaken van de client systeembrede CPU-gebruik met behulp van metrische gegevens beschikbaar zijn in Azure portal of via prestatiemeteritems op de machine. Zorg ervoor dat u niet bewaken *proces* CPU omdat een enkel proces lage CPU-gebruik kan hebben, maar het hele systeem-CPU kan hoog zijn. Bekijk voor pieken in het CPU-gebruik die met de time-outs overeenkomen. Hoge CPU kan ook leiden tot hoog `in: XXX` waarden in `TimeoutException` foutberichten bij zoals beschreven in de [enorme verkeerspiek](#burst-of-traffic) sectie.

> [!NOTE]
> StackExchange.Redis 1.1.603 en nieuwer, bevat de `local-cpu` metrische in `TimeoutException` foutberichten. Zorg ervoor dat u met behulp van de meest recente versie van de [NuGet-pakket StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die zijn voortdurend wordt verholpen in de volgende code maakt u deze krachtiger voor time-outs wanneer u de meest recente versie is dus belangrijk.
>
>

Om te beperken van een client hoog CPU-gebruik:

- Onderzoeken wat de oorzaak is van CPU, pieken.
- Werk de client bij naar een grotere VM-grootte met meer CPU-capaciteit.

### <a name="client-side-bandwidth-exceeded"></a>Client-side bandbreedte is overschreden

Afhankelijk van de architectuur van clientcomputers, kunnen ze hebben beperkingen op de hoeveelheid netwerkbandbreedte ze beschikbaar zijn. Als de client de beschikbare bandbreedte overschrijdt door overbelasting van netwerkcapaciteit, wordt niet klikt u vervolgens gegevens verwerkt op de client zo snel als de server het verzendt. Deze situatie kan leiden tot time-outs.

Controleren hoe uw bandbreedtegebruik na verloop van tijd veranderen [een voorbeeld `BandwidthLogger` ](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Deze code kan niet worden uitgevoerd in sommige omgevingen met beperkte machtigingen (zoals Azure web sites).

Als u wilt oplossen, netwerkbandbreedte verlagen of te verhogen van de VM-grootte op een met meer netwerkcapaciteit-client.

### <a name="large-requestresponse-size"></a>Grootte van grote aanvraag/antwoord

Een grote aanvraag/antwoord kan leiden tot time-outs. Stel bijvoorbeeld dat uw time-outwaarde geconfigureerd op de client is 1 seconde. Uw toepassing worden aangevraagd twee sleutels (bijvoorbeeld, "A" en "B") op hetzelfde moment (met behulp van de fysieke netwerkverbinding). De meeste clients ondersteunen aanvraag 'pipelining", waarbij zowel aanvragen"A"en"B"worden verzonden achter elkaar zonder te wachten op hun antwoorden. De server verzendt de antwoorden weer in dezelfde volgorde. Als antwoord "A" groot is, kan het deel van de time-out voor aanvragen eten.

In het volgende voorbeeld worden aanvraag "A" en "B" snel verzonden naar de server. Start de server voor het snel verzenden van reacties "A" en "B". Vanwege de tijden van de overdracht van gegevens, een antwoord, "B" achter het antwoord "A wachten moet" time-out optreedt zelfs als de server heeft gereageerd snel.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)

Deze aanvraag/antwoord is moeilijk om te meten. U kunt uw clientcode voor het volgen van grote aanvragen en antwoorden kan instrumenteren.

Oplossingen voor grote reactieformaat zijn verschillend, maar bevatten:

1. Optimaliseer uw toepassing voor een groot aantal kleine waarden, in plaats van enkele grote waarden.
    - De beste oplossing is het opsplitsen van uw gegevens in de gerelateerde lagere waarden.
    - Zie het bericht [wat is de ideale grootte van het waardebereik voor redis? 100 KB te groot is? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) voor meer informatie over waarom lagere waarden worden aanbevolen.
1. Verhoog de grootte van uw virtuele machine om op te halen van de hogere bandbreedte-mogelijkheden
    - Meer bandbreedte op de client of server-VM kan gegevens de overdrachtstijd voor grotere antwoorden verminderen.
    - Vergelijk uw huidige netwerkgebruik op beide computers met de limieten van uw huidige VM-grootte. Meer bandbreedte op alleen de server of alleen op de client mogelijk niet voldoende.
1. Verhoog het aantal verbindingsobjecten dat maakt gebruik van uw toepassing.
    - Een round robin-benadering gebruiken om aan te vragen over verschillende objecten.

## <a name="server-side-troubleshooting"></a>Het oplossen van server-side

Deze sectie wordt beschreven voor het oplossen van problemen die vanwege een voorwaarde op de cacheserver optreden.

- [Geheugendruk op de server](#memory-pressure-on-the-server)
- [Hoog CPU-gebruik / Server laden](#high-cpu-usage--server-load)
- [Server Side bandbreedte is overschreden](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Geheugendruk op de server

Geheugendruk op de server leidt tot alle soorten prestatieproblemen die verwerking van aanvragen kunnen vertragen. Wanneer de geheugendruk raakt, kan het systeem gegevens naar schijf pagina. Dit _pagina fout_ zorgt ervoor dat het systeem aanzienlijk vertragen. Er zijn verschillende mogelijke oorzaken van dit geheugendruk:

- De cache wordt gevuld met gegevens in de buurt van de maximale capaciteit.
- Redis is hoog geheugenfragmentatie zien. Deze fragmentatie wordt meestal veroorzaakt door het opslaan van grote objecten omdat Redis is geoptimaliseerd voor kleine objecten.

Redis wordt aangegeven dat twee statistieken via de [INFO](https://redis.io/commands/info) opdracht die u kan helpen dit probleem identificeren: "used_memory" en 'used_memory_rss'. U kunt [deze metrische gegevens weergeven](cache-how-to-monitor.md#view-metrics-with-azure-monitor) met behulp van de portal.

Er zijn verschillende mogelijke wijzigingen die kunt u geheugengebruik in orde houden:

- [Configureer een beleid geheugen](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) en vervaldatum tijden instellen voor uw sleutels. Dit beleid is mogelijk niet voldoende als er fragmentatie.
- [Configureren van een waarde maxmemory-gereserveerde](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) die groot genoeg is om te compenseren voor het geheugen gefragmenteerd is. Zie voor meer informatie de extra [overwegingen voor geheugenreserveringen](#considerations-for-memory-reservations) hieronder.
- Splitst u uw grote objecten in de cache in kleinere verwante objecten.
- [Waarschuwingen maken](cache-how-to-monitor.md#alerts) gebruikt op de metrische gegevens, zoals geheugen te vroeg worden gewaarschuwd over mogelijke gevolgen.
- [Schaal](cache-how-to-scale.md) in een groter formaat in de cache met een geheugencapaciteit van meer.

#### <a name="considerations-for-memory-reservations"></a>Overwegingen voor Geheugenreserveringen

Geheugen reservering waarden worden bijgewerkt, zoals maxmemory-gereserveerde, kunnen invloed hebben op prestaties van de cache. Stel dat u hebt een cache van 53 GB die is gevuld met 49 GB aan gegevens. Wijzigen van de waarde van de reservering tot 8 GB komt van het systeem maximaal beschikbare geheugen op 45 GB. Als _used_memory_ of _used_memory_rss_ waarden hoger dan 45 GB zijn, het systeem kan onbeschikbaar maken van gegevens tot beide _used_memory_ en _used_memory_rss_ hieronder 45 GB zijn. Verwijdering kan de fragmentatie van de belasting en het geheugen van de server vergroten.

### <a name="high-cpu-usage--server-load"></a>Hoog CPU-gebruik / Server laden

Een hoge serverbelasting of CPU-gebruik betekent dat de server kan geen aanvragen in tijdig verwerken. De server mogelijk traag reageert en niet kan bijhouden met aanvraag-tarieven.

[Metrische gegevens controleren](cache-how-to-monitor.md#view-metrics-with-azure-monitor) , zoals CPU of server laden. Bekijk voor pieken in het CPU-gebruik die met de time-outs overeenkomen.

Er zijn verschillende wijzigingen die u maken kunt om te hoge serverbelasting verhelpen:

- Onderzoeken wat de oorzaak is van CPU-pieken zoals uitgevoerd [uitgebreide opdrachten](#expensive-commands) of pagina fout vanwege de hoge geheugenbelasting.
- [Waarschuwingen maken](cache-how-to-monitor.md#alerts) op metrische gegevens zoals CPU of server load te vroeg worden gewaarschuwd over mogelijke gevolgen.
- [Schaal](cache-how-to-scale.md) in een groter formaat in de cache met meer CPU-capaciteit.

#### <a name="expensive-commands"></a>Uitgebreide opdrachten

Niet alle Redis-opdrachten worden gelijk gemaakt: sommige zijn duurder om uit te voeren dan andere. De [documentatie voor Redis-opdrachten](https://redis.io/commands) ziet u de complexiteit van de tijd van elke opdracht. Het verdient aanbeveling controleren van de opdrachten die u op uw cache om te begrijpen van de invloed op de prestaties van deze opdrachten uitvoert. Bijvoorbeeld, de [sleutels](https://redis.io/commands/keys) opdracht wordt vaak gebruikt zonder te weten dat het is een O(N)-bewerking. U kunt sleutels vermijden met behulp van [SCANNEN](https://redis.io/commands/scan) pieken te verminderen van de CPU.

Met behulp van de [SLOWLOG](https://redis.io/commands/slowlog) opdracht, kunt u uitgebreide opdrachten worden uitgevoerd op de server meten.

### <a name="server-side-bandwidth-exceeded"></a>Serverzijde bandbreedte is overschreden

Ander cache-grootten hebben andere netwerkbandbreedte capaciteit. Als de server de beschikbare bandbreedte overschrijdt, niet naar de client zo snel gegevens verzonden. Aanvragen van clients kunnen time-out omdat de server kan geen gegevens naar de client snel genoeg pushen.

De metrische gegevens over "Cache lezen" en 'Cache schrijven' kan worden gebruikt om te zien hoeveel bandbreedte serverzijde wordt gebruikt. U kunt [deze metrische gegevens weergeven](cache-how-to-monitor.md#view-metrics-with-azure-monitor) in de portal.

Om te verhelpen situaties waarin gebruik van netwerkbandbreedte bijna de maximale capaciteit is:

- Gedrag van de client-aanroep te verminderen van netwerk-aanvraag wijzigen.
- [Waarschuwingen maken](cache-how-to-monitor.md#alerts) op metrische gegevens zoals de cache lezen of schrijven van de cache te vroeg worden gewaarschuwd over mogelijke gevolgen.
- [Schaal](cache-how-to-scale.md) in een groter formaat in de cache met meer bandbreedtecapaciteit van het netwerk.

## <a name="data-loss-troubleshooting"></a>Verlies van gegevens oplossen

Ik verwacht voor bepaalde gegevens in mijn Azure-Cache voor Redis-exemplaar, maar het is niet lijkt het alsof er.

Zie [wat is er gebeurd met mijn gegevens in Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) voor mogelijke oorzaken en oplossingen.

## <a name="stackexchangeredis-timeout-exceptions"></a>De time-outuitzonderingen StackExchange.Redis

Een configuratie-instelling met de naam maakt gebruik van StackExchange.Redis `synctimeout` voor synchrone bewerkingen met een standaardwaarde van 1000 ms. Als een synchrone aanroep wordt niet voltooid binnen deze tijd, genereert de StackExchange.Redis-client een time-outfout die vergelijkbaar is met het volgende voorbeeld:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)

Dit foutbericht bevat metrische gegevens waarmee u verwijzen naar de oorzaak en de mogelijke resolutie van het probleem. De volgende tabel bevat details over de fout bericht metrische gegevens.

| Fout bericht metrische gegevens | Details |
| --- | --- |
| inst |In de afgelopen tijdsperiode: 0-opdrachten zijn uitgegeven |
| mgr |Het gaat met de manager socket `socket.select`, wat het betekent dat het besturingssysteem om aan te geven van een socket dat er iets wordt gevraagd. De lezer is niet actief lezen van het netwerk, omdat deze niet denkt er dat te doen |
| wachtrij |Er zijn 73 totaal aantal lopende bewerkingen |
| qu |6 van de bewerkingen wordt uitgevoerd in de wachtrij staan niet verzonden en nog niet hebt zijn geschreven voor uitgaand netwerkverkeer |
| qs |67 van de bewerkingen in uitvoering zijn verzonden naar de server, maar een antwoord is nog niet beschikbaar. Het antwoord kan worden `Not yet sent by the server` of `sent by the server but not yet processed by the client.` |
| qc |0 van de bewerkingen in uitvoering antwoorden hebt gezien, maar nog niet hebt zijn gemarkeerd als voltooid omdat ze op de lus is voltooid verwachten |
| wR |Er is een actieve writer (wat betekent dat het 6 unsent aanvragen worden niet genegeerd) bytes/activewriters |
| in |Er zijn geen actieve lezers en nul bytes zijn beschikbaar om te lezen op de NIC-bytes/activereaders |

### <a name="steps-to-investigate"></a>Stappen voor het onderzoeken

1. Als een best practice, zorg ervoor dat u het volgende patroon om bij het gebruik van de StackExchange.Redis-client verbinding te maken.

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
    ```

    Zie voor meer informatie, [verbinding maken met de cache met behulp van StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Zorg ervoor dat uw server en de clienttoepassing in dezelfde regio in Azure. Bijvoorbeeld, u mogelijk worden ophalen time-outs wanneer uw cache in VS-Oost is, maar de client zich in VS-West en de aanvraag wordt niet voltooid binnen de `synctimeout` interval of u mogelijk worden ophalen time-outs wanneer u fouten in uw lokale ontwikkelcomputer opspoort. 

    Het is raadzaam om de cache en in de client in dezelfde Azure-regio. Als u een scenario met een andere regio's aanroepen hebt, moet u instellen de `synctimeout` interval op een waarde die hoger is dan het interval standaard 1000 ms door een `synctimeout` eigenschap in de verbindingsreeks. Het volgende voorbeeld toont een fragment van een verbindingsreeks voor StackExchange.Redis geleverd door Azure Cache voor Redis met een `synctimeout` van 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
1. Zorg ervoor dat u met behulp van de meest recente versie van de [NuGet-pakket StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die zijn voortdurend wordt verholpen in de volgende code maakt u deze krachtiger voor time-outs wanneer u de meest recente versie is dus belangrijk.
1. Als uw aanvragen zijn gebonden aan de bandbreedtebeperkingen op de server of client, duurt het langer om te voltooien en kan leiden tot time-outs. Om te zien of de time-out vanwege de netwerkbandbreedte op de server, Zie [serverzijde bandbreedte overschreden](#server-side-bandwidth-exceeded). Om te zien of de time-out vanwege netwerkbandbreedte client, Zie [Client-side-bandbreedte overschreden](#client-side-bandwidth-exceeded).
1. Bent u aan CPU op de server of op de client gekoppeld?

   - Controleer als u bent ophalen gebonden bent aan de CPU op de client. Hoge CPU kan ervoor zorgen dat de aanvraag niet verwerkt binnen de `synctimeout` interval en de oorzaak van een aanvraag voor het time-out. Verplaatsen naar een groter formaat van de client of distributie van de belasting kan voor het beheren van dit probleem helpen.
   - Controleer of u bij het ophalen van CPU, afhankelijk van op de server door de bewaking van de `CPU` [metrische prestatiegegevens voor weergave in de cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Aanvragen die afkomstig zijn Redis CPU-afhankelijk is, kunnen leiden tot deze aanvragen voor time-out. Om dit probleem op te lossen, kunt u de verdelen over meerdere shards met clusters in een premium-cache of een upgrade uitvoert naar een groter formaat of de prijscategorie. Zie voor meer informatie, [Server Side bandbreedte overschreden](#server-side-bandwidth-exceeded).
1. Zijn er nog opdrachten duurt langer verwerking op de server? Langlopende opdrachten die lange tijd voor het verwerken van op de redis-server kunnen leiden tot time-outs. Zie voor meer informatie over opdrachten langlopende [uitgebreide opdrachten](#expensive-commands). U kunt verbinding maken met uw Azure-Cache voor Redis-exemplaar met behulp van de redis cli-client of de [Redis-Console](cache-configure.md#redis-console). Voer de [SLOWLOG](https://redis.io/commands/slowlog) opdracht uit om te zien of er aanvragen langzamer dan verwacht. Redis-Server en StackExchange.Redis zijn geoptimaliseerd voor een groot aantal kleine aanvragen in plaats van aanvragen voor minder grote. Uw gegevens splitsen in kleinere chunks kan dingen die hier worden verbeterd.

    Voor informatie over verbinding maken met SSL-eindpunt van uw cache met behulp van redis cli en beveiligde tunnel, Zie het blogbericht [aankondiging van ASP.NET-Sessiestatusprovider voor Preview-versie Redis](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).
1. Hoge belasting van de Redis-server kan leiden tot time-outs. U kunt de belasting van de server bewaken door de bewaking van de `Redis Server Load` [metrische prestatiegegevens voor weergave in de cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Een belasting van de server van 100 (maximale waarde) geeft aan dat de redis-server bezet zijn, geen niet-actieve tijd is, verwerken van aanvragen. Als u wilt zien als bepaalde aanvragen van alle van de capaciteit van de server volgen, voert u de opdracht SlowLog, zoals beschreven in de vorige alinea. Zie voor meer informatie, hoog CPU-gebruik / Server laden.
1. Is er een andere gebeurtenis aan de clientzijde die een blip netwerk kan mogelijk veroorzaakt? Algemene gebeurtenissen opgenomen: het aantal clientexemplaren omhoog of omlaag, het implementeren van een nieuwe versie van de client of de functie voor automatisch schalen ingeschakeld. We hebben onze tests gevonden dat automatisch schalen of schaal omhoog/omlaag ervoor zorgen uitgaande netwerkconnectiviteit dat kan verloren gedurende enkele seconden. StackExchange.Redis code bestand is tegen dergelijke gebeurtenissen en opnieuw verbinding maakt. Alle aanvragen in de wachtrij kunnen tijdens het opnieuw verbinding maakt time-out.
1. Is er een grote aanvraag voorafgaand aan verschillende kleine aanvragen tot de cache die is een time-out? De parameter `qs` in de volgende fout bericht vertelt u hoeveel aanvragen naar de server van de client zijn verzonden, maar nog niet een antwoord verwerkt. Deze waarde kan blijven toenemen omdat StackExchange.Redis één TCP-verbinding gebruikt en één antwoord kan alleen worden gelezen op een tijdstip. Hoewel er is een time-out opgetreden voor de eerste bewerking, niet wordt deze niet meer gegevens worden verzonden naar of van de server. Andere aanvragen worden geblokkeerd totdat de grote aanvraag voltooid is en leiden time-outs tot kan. Eén oplossing is de kans op time-outs minimaliseren door ervoor te zorgen dat uw cache groot genoeg zijn voor uw werkbelasting is en grote waarden te splitsen in kleinere chunks. Een andere mogelijke oplossing is het gebruik van een pool van `ConnectionMultiplexer` objecten in uw client, en kies het minste geladen `ConnectionMultiplexer` bij het verzenden van een nieuwe aanvraag. Laden op meerdere verbindingsobjecten moet voorkomen dat een enkele time-out veroorzaakt door andere aanvragen voor ook time-out.
1. Als u `RedisSessionStateProvider`, zorg ervoor dat u de time-out voor de nieuwe pogingen correct hebt ingesteld. `retryTimeoutInMilliseconds` moet hoger zijn dan `operationTimeoutInMilliseconds`, anders worden er geen nieuwe pogingen uitgevoerd. In het volgende voorbeeld `retryTimeoutInMilliseconds` is ingesteld op 3000. Zie voor meer informatie, [ASP.NET-Sessiestatusprovider voor Azure Cache voor Redis](cache-aspnet-session-state-provider.md) en [over het gebruik van de parameters voor de configuratie van de Sessiestatusprovider en Cacheprovider voor uitvoer](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

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

1. Geheugengebruik op de Azure-Cache voor Redis-server door te controleren [bewaking](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` en `Used Memory`. Als een beleid van onbeschikbaar ingesteld is, Redis wordt gestart wanneer onbeschikbaar maken van sleutels `Used_Memory` de cachegrootte is bereikt. In het ideale geval `Used Memory RSS` moet worden slechts iets hoger dan `Used memory`. Een groot verschil betekent dat er geheugenfragmentatie (intern of extern). Wanneer `Used Memory RSS` is minder dan `Used Memory`, betekent dit dat onderdeel van het cache-geheugen is door het besturingssysteem zijn gewisseld. Als deze wisselen optreedt, kunt u verwachten dat sommige aanzienlijke latentie. Omdat Redis geen controle over de wijze waarop de toewijzingen zijn toegewezen aan geheugenpagina's, hoge `Used Memory RSS` is vaak het resultaat van een piek in het geheugengebruik. Als Redis-server maakt vrij geheugen, de toewijzingsfunctie duurt het geheugen, maar kan of kunnen niet geven het geheugen terug naar het systeem. Mogelijk zijn er een verschil tussen de `Used Memory` verbruik van waarde en geheugen zoals gemeld door het besturingssysteem. Geheugen is mogelijk gebruikt en die zijn uitgebracht door Redis, maar niet de opgegeven terug naar het systeem. Om te beperken geheugenproblemen, kunt u de volgende stappen uitvoeren:

   - De cache bijwerken naar een groter formaat, zodat u tegen geheugenbeperkingen worden niet uitgevoerd op het systeem.
   - Vervaldatum tijden voor de sleutels zo instellen dat oudere waarden proactief zijn verwijderd.
   - Monitor de `used_memory_rss` metrische gegevens in de cache. Wanneer deze waarde de grootte van het cachegeheugen nadert, bent u waarschijnlijk beginnen te zien van prestatieproblemen. De gegevens verdelen over meerdere shards, als u met behulp van een premium-cache, of een upgrade uitvoert naar een groter formaat in de cache.

   Zie voor meer informatie, [geheugendruk op de server](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Aanvullende informatie

- [Welke Azure Cache voor Redis-aanbieding en de grootte moet ik gebruiken?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [Hoe kan ik benchmark-test uitvoeren en testen van de prestaties van mijn cache?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [Hoe kan ik de Redis-opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)
- [Azure Cache controleren voor Redis](cache-how-to-monitor.md)
