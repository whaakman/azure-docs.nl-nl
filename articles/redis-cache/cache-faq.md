---
title: Veelgestelde vragen over Azure Redis-Cache | Microsoft Docs
description: Meer informatie over de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor Azure Redis-Cache
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: c2c52b7d-b2d1-433a-b635-c20180e5cab2
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: wesmc
ms.openlocfilehash: af185725433b0eacc5d57b90fb2e75edd143a59a
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-redis-cache-faq"></a>Veelgestelde vragen over Azure Redis Cache
Informatie over de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor Azure Redis-Cache.

## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?
Als uw vraag hier niet is vermeld, laat ons weten en wij helpen u bij een antwoord vinden.

* U kunt een vraag plaatsen in de opmerkingen aan het einde van deze Veelgestelde vragen en bezighouden met het team van Azure-Cache en andere communityleden over dit artikel.
* Een breder publiek bereiken, kunt u een vraag plaatsen op de [Azure Cache MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) en bezighouden met het team van Azure-Cache en andere leden van de community.
* Als u een functie indienen wilt, kunt u uw aanvragen en ideeën voor het indienen [Azure Redis-Cache User Voice](https://feedback.azure.com/forums/169382-cache).
* U kunt ook een e-mailbericht verzenden naar ons op [externe feedback over Azure Cache](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Basisbeginselen van Azure Redis-Cache
De veelgestelde vragen in deze sectie hebben betrekking op sommige van de basisprincipes van Azure Redis-Cache.

* [Wat is Azure Redis-cache?](#what-is-azure-redis-cache)
* [Hoe kan ik aan de slag met Azure Redis-Cache?](#how-can-i-get-started-with-azure-redis-cache)

De volgende veelgestelde vragen hebben betrekking op basisconcepten en vragen over Azure Redis-Cache en worden beantwoord in een van de andere secties van de veelgestelde vragen over.

* [Welk aanbod voor de Redis-cache en welke cachegrootte moet ik kiezen?](#what-redis-cache-offering-and-size-should-i-use)
* [Welke Redis-cache-clients kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
* [Is er een lokale emulator voor Azure Redis-Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Hoe bewaak ik de status en prestaties van mijn cache?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Veelgestelde vragen over plannen
* [Welk aanbod voor de Redis-cache en welke cachegrootte moet ik kiezen?](#what-redis-cache-offering-and-size-should-i-use)
* [Azure Redis-Cache-prestaties](#azure-redis-cache-performance)
* [In welke regio moet ik mijn cache vinden?](#in-what-region-should-i-locate-my-cache)
* [Hoe ben ik in rekening gebracht voor Azure Redis-Cache?](#how-am-i-billed-for-azure-redis-cache)
* [Kan ik Azure Redis-Cache gebruiken met Azure Government Cloud, Azure China Cloud of Duitse van Microsoft Azure?](#can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Veelgestelde vragen over ontwikkeling
* [Wat moeten de StackExchange.Redis configuratieopties doen?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Welke Redis-cache-clients kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
* [Is er een lokale emulator voor Azure Redis-Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Hoe kan ik de Redis-opdrachten uitvoeren?](#how-can-i-run-redis-commands)
* [Waarom geen Azure Redis-Cache heeft een MSDN-bibliotheek-verwijzing voor klasse zoals enkele van de andere Azure-services?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Kan ik Azure Redis-Cache gebruiken als een PHP-sessie-cache?](#can-i-use-azure-redis-cache-as-a-php-session-cache)
* [Wat zijn de Redis-databases?](#what-are-redis-databases)

## <a name="security-faqs"></a>Veelgestelde vragen over Security
* [Wanneer moet ik de niet-SSL-poort voor het verbinden met Redis inschakelen?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Veelgestelde vragen over productie
* [Wat zijn enkele aanbevolen procedures voor productie?](#what-are-some-production-best-practices)
* [Wat zijn enkele van de overwegingen als u algemene Redis-opdrachten gebruikt?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Hoe kan ik benchmark en test de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Belangrijke informatie over de ThreadPool groei](#important-details-about-threadpool-growth)
* [Inschakelen van de server GC meer doorvoer ophalen op de client bij gebruik van StackExchange.Redis](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Prestatieoverwegingen rond verbindingen](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>Bewaking en probleemoplossing Veelgestelde vragen
De veelgestelde vragen in deze sectie hebben betrekking op algemene bewaking en probleemoplossing van vragen. Zie voor meer informatie over bewaking en probleemoplossing van uw Azure Redis-Cache-exemplaren [Azure Redis-Cache controleren](cache-how-to-monitor.md) en [het oplossen van Azure Redis-Cache](cache-how-to-troubleshoot.md).

* [Hoe bewaak ik de status en prestaties van mijn cache?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Waarom krijg ik time-outs zien?](#why-am-i-seeing-timeouts)
* [Waarom is mijn klant losgekoppeld van de cache?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Veelgestelde vragen over eerdere Cache aanbieding
* [Welke aanbieding voor Azure-Cache is geschikt voor mij?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-redis-cache"></a>Wat is Azure Redis-cache?
Azure Redis-Cache is gebaseerd op de populaire open-source [Redis-cache](http://redis.io). Deze hebt u toegang tot een beveiligde, toegewezen Redis-cache, beheerd door Microsoft en toegankelijk vanuit elke toepassing in Azure. Zie voor een gedetailleerd overzicht de [Azure Redis-Cache](https://azure.microsoft.com/services/cache/) productpagina op Azure.com.

### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Hoe kan ik aan de slag met Azure Redis-Cache?
Er zijn verschillende manieren die u kunt aan de slag met Azure Redis-Cache.

* Raadpleeg een van onze zelfstudies beschikbaar voor [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md), en [Python](cache-python-get-started.md).
* U kunt bekijken [hoe bouwen High-Performance Apps met behulp van Microsoft Azure Redis-Cache](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
* U kunt de clientdocumentatie voor de clients die overeenkomt met de taal van de ontwikkeling van uw project voor informatie over het gebruik van Redis uitchecken. Er zijn veel Redis-clients die kunnen worden gebruikt met Azure Redis-Cache. Zie voor een lijst met Redis-clients, [http://redis.io/clients](http://redis.io/clients).

Als u nog een Azure-account hebt, kunt u:

* [Gratis een Azure-account openen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). U ontvangt tegoed dat kan worden gebruikt om betaalde Azure-services uit te proberen. Zelfs nadat het tegoed is gebruikt, kunt u het account houden en de gratis Azure-services en -functies gebruiken.
* [Uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Via uw MSDN-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken.

<a name="cache-size"></a>

### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Welk aanbod voor de Redis-cache en welke cachegrootte moet ik kiezen?
Elke Azure Redis-Cache biedt verschillende niveaus van **grootte**, **bandbreedte**, **hoge beschikbaarheid**, en **SLA** opties.

Hieronder vindt u overwegingen voor het kiezen van een Cache-aanbieding.

* **Geheugen**: de Basic en Standard lagen bieden 250 MB – 53 GB. De laag Premium biedt tot 530 GB. Zie voor meer informatie [prijzen van Azure Redis-Cache](https://azure.microsoft.com/pricing/details/cache/).
* **Netwerkprestaties**: als u een werkbelasting die hoge doorvoersnelheid nodig hebt, de laag Premium biedt meer bandbreedte vergeleken met de Standard- of Basic. Binnen elke laag hebben grotere grootte caches ook meer bandbreedte vanwege de onderliggende virtuele machine die als host fungeert voor de cache. Zie de [na tabel](#cache-performance) voor meer informatie.
* **Doorvoer**: de Premium-laag biedt de maximaal beschikbare doorvoer. Als de cacheserver of de client de bandbreedtelimieten is bereikt, ontvangt u mogelijk time-outs aan de clientzijde. Zie de volgende tabel voor meer informatie.
* **Hoge beschikbaarheid/SLA**: Azure Redis-Cache wordt gegarandeerd dat een standaard/Premium cache beschikbaar minimaal 99,9% van de tijd is. Zie voor meer informatie over onze SLA, [prijzen van Azure Redis-Cache](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). De SLA bevat alleen informatie over verbinding met de Cache-eindpunten. De SLA heeft geen betrekking op beveiliging tegen verlies van gegevens. U wordt aangeraden de tolerantie tegen verlies van gegevens verhoogd met de functie persistentie van Redis-gegevens in de laag Premium.
* **Redis-Gegevenspersistentie**: de Premium-laag kunt u de cache-gegevens in een Azure Storage-account. Alle gegevens die is opgeslagen in een cache Basic/standaard alleen in het geheugen. Als er zijn onderliggende infrastructuur problemen er mogelijk gegevensverlies. U wordt aangeraden de tolerantie tegen verlies van gegevens verhoogd met de functie persistentie van Redis-gegevens in de laag Premium. Azure Redis-Cache biedt RDB en AOF (binnenkort) opties in Redis-persistentie. Zie voor meer informatie [persistentie voor een Premium Azure Redis-Cache configureren](cache-how-to-premium-persistence.md).
* **Redis-Cluster**: caches groter zijn dan 53 GB te maken of gegevens verdelen over meerdere Redis-knooppunten, kunt u Redis clustering, die beschikbaar is in de laag Premium. Elk knooppunt bestaat uit een combinatie van de cache primair/replica voor hoge beschikbaarheid. Zie [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md) (Clustering voor een Premium Azure Redis Cache configureren) voor meer informatie.
* **Verbeterde beveiliging en isolatie**: implementatie van Azure Virtual Network (VNET) biedt verbeterde beveiliging en isolatie voor uw Azure Redis-Cache, evenals de subnetten, toegangscontrolebeleid, en andere functies nog toegang beperken. Zie [How to configure Virtual Network support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md) (Virtual Network-ondersteuning voor een Premium Azure Redis Cache configureren) voor meer informatie.
* **Configureren van Redis**: In de standaard- en Premium-lagen, configureert u Redis voor Keyspace-kennisgevingen.
* **Maximum aantal clientverbindingen**: de Premium-laag biedt het maximum aantal clients die verbinding met Redis met een hoger aantal verbindingen voor grotere grootte caches maken kunnen. Zie voor meer informatie [prijzen van Azure Redis-Cache](https://azure.microsoft.com/pricing/details/cache/).
* **Speciale kern voor Redis-Server**: In de laag Premium alle cachegrootte hebben een speciale kern voor Redis. In de Basic-/ Standard lagen, de grootte van de C1 en hoger hebben een speciale kern voor Redis-server.
* **Redis is één thread** zodat met meer dan twee kernen geen extra voordeel biedt ten opzichte slechts twee kernen hebben, maar grotere VM doorgaans meer bandbreedte dan kleinere hebben. Als de cacheserver of client bereikt de bandbreedtelimieten, ontvangt u time-outs aan de clientzijde.
* **Verbeterde prestaties**: Caches in de laag Premium zijn geïmplementeerd op hardware met snellere processors geeft betere prestaties in vergelijking met de Basic- of Standard-laag. Premium-laag Caches hebben hogere doorvoer en lagere latenties.

<a name="cache-performance"></a>

### <a name="azure-redis-cache-performance"></a>Azure Redis-Cache-prestaties
De volgende tabel bevat de waarden van de maximale bandbreedte van tijdens het testen van verschillende grootten van standaard en Premium in de cache opgeslagen met behulp van `redis-benchmark.exe` van een Iaas-VM voor het eindpunt van de Azure Redis-Cache. 

>[!NOTE] 
>Deze waarden niet worden gegarandeerd en er is geen SLA voor deze getallen, maar moet typische. U moet laden uw eigen toepassing om te bepalen van de grootte van de juiste cache voor uw toepassing testen.
>
>

We kunnen de volgende conclusie trekt tekenen uit deze tabel:

* Doorvoer voor de caches die dezelfde grootte zijn is hoger in de laag Premium in vergelijking met de Standard-laag. Bijvoorbeeld, met een 6 GB-Cache is doorvoer van P1 180.000 RPS in vergelijking met 49,000 voor C3.
* Met Redis clustering, duurt doorvoer lineair omdat u het aantal shards (knooppunten) in het cluster vergroten. Als u een cluster P4 van 10 shards maakt, vervolgens de beschikbare doorvoer is bijvoorbeeld 400.000 * 10 = 4 miljoen RPS.
* Doorvoer voor grotere sleutel grootten is hoger in de laag Premium in vergelijking met de Standard-laag.

| Prijscategorie | Grootte | CPU-kernen | Beschikbare bandbreedte | De grootte van 1 KB |
| --- | --- | --- | --- | --- |
| **Standaard cachegrootte** | | |**Megabits per seconde (Mb/s) / Megabytes per seconde (MB/s)** |**Aanvragen per seconde (RPS)** |
| C0 |250 MB |Gedeeld |5 / 0.625 |600 |
| C1 |1 GB |1 |100 / 12.5 |12,200 |
| C2 |2,5 GB |2 |200 / 25 |24,000 |
| C3 |6 GB |4 |400 / 50 |49,000 |
| C4 |13 GB |2 |500 / 62.5 |61,000 |
| C5 |26 GB |4 |1,000 / 125 |115,000 |
| C6 |53 GB |8 |2,000 / 250 |150,000 |
| **Premium-cachegrootte** | |**CPU-kernen per shard** | **Megabits per seconde (Mb/s) / Megabytes per seconde (MB/s)** |**Aanvragen per seconde (RPS) per shard** |
| P1 |6 GB |2 |1,500 / 187.5 |180,000 |
| P2 |13 GB |4 |3,000 / 375 |360,000 |
| P3 |26 GB |4 |3,000 / 375 |360,000 |
| P4 |53 GB |8 |6,000 / 750 |400,000 |

Voor instructies over het downloaden van de Redis-hulpprogramma's zoals `redis-benchmark.exe`, Zie de [hoe kan ik Redis-opdrachten uitvoeren?](#cache-commands) sectie.

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>In welke regio moet ik mijn cache vinden?
Zoek uw Azure Redis-Cache in dezelfde regio bevinden als uw toepassing van de client cache voor optimale prestaties en de laagste latentie.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-redis-cache"></a>Hoe ben ik in rekening gebracht voor Azure Redis-Cache?
Prijzen voor Azure Redis-Cache is [hier](https://azure.microsoft.com/pricing/details/cache/). De pagina met prijzen worden prijzen als uur. Caches wordt gefactureerd op basis van per minuut vanaf het moment dat de cache wordt gemaakt tot het moment dat een cache wordt verwijderd. Er is geen optie voor het stoppen of onderbreken van de facturering van een cache.

### <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Kan ik Azure Redis-Cache gebruiken met Azure Government Cloud, Azure China Cloud of Duitse van Microsoft Azure?
Ja, is Azure Redis-Cache beschikbaar in de Cloud van de overheid Azure, Azure China Cloud en Duitse van Microsoft Azure. De URL's voor toegang tot en beheer van Azure Redis-Cache zijn verschillend in deze clouds vergeleken met de openbare Azure-Cloud. 

| Cloud   | DNS-achtervoegsel voor Redis            |
|---------|---------------------------------|
| Openbaar  | *.redis.cache.windows.net       |
| Amerikaanse overheid  | *.redis.cache.usgovcloudapi.net |
| Duitsland | *.redis.cache.cloudapi.de       |
| China   | *.redis.cache.chinacloudapi.cn  |

Zie de volgende koppelingen voor meer informatie over overwegingen bij het gebruik van Azure Redis-Cache met andere clouds.

- [Azure Government Databases - Azure Redis-Cache](../azure-government/documentation-government-services-database.md#azure-redis-cache)
- [Azure China Cloud - Azure Redis-Cache](https://www.azure.cn/documentation/services/redis-cache/)
- [Microsoft Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/)

Zie voor meer informatie over het gebruik van Azure Redis-Cache met PowerShell in Azure Government Cloud, Azure China Cloud en Microsoft Azure Duitsland [verbinding maken met andere clouds - Azure Redis-Cache PowerShell](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Wat moeten de StackExchange.Redis configuratieopties doen?
StackExchange.Redis heeft een groot aantal opties. Deze sectie wordt gesproken over een aantal van de algemene instellingen. Zie voor meer informatie over opties voor StackExchange.Redis gedetailleerde [StackExchange.Redis configuratie](https://stackexchange.github.io/StackExchange.Redis/Configuration).

| ConfigurationOptions | Beschrijving | Aanbeveling |
| --- | --- | --- |
| AbortOnConnectFail |Wanneer is ingesteld op true, wordt de verbinding wordt niet opnieuw verbinding maken na een netwerkstoring op een. |Ingesteld op false en laat StackExchange.Redis automatisch opnieuw verbinding maken. |
| ConnectRetry |Het aantal keren te herhalen verbindingspogingen tijdens de initiële verbinding maken. |Zie de volgende opmerkingen voor hulp. |
| ConnectTimeout |Time-out in ms voor connect-bewerkingen. |Zie de volgende opmerkingen voor hulp. |

De standaardwaarden van de client zijn meestal voldoende. U kunt de opties op basis van uw werkbelasting aanpassen.

* **Nieuwe pogingen**
  * Voor ConnectRetry en ConnectTimeout is de algemene richtlijnen snel mislukt en probeer het opnieuw. In deze richtlijnen wordt op basis van uw werkbelasting en na hoeveel tijd op gemiddelde het duurt voor de client een Redis-opdracht geven en een antwoord ontvangen.
  * Laat StackExchange.Redis automatisch opnieuw verbinden in plaats van de verbindingsstatus controleren en zelf opnieuw verbinding te maken. **Vermijd het gebruik van de eigenschap ConnectionMultiplexer.IsConnected**.
  * Snowballing - soms u kunt uitvoeren in een probleem waarbij u opnieuw uit te proberen en de nieuwe pogingen snowball en nooit wordt hersteld. Als snowballing optreedt, moet u overwegen een nieuwe poging exponentieel uitstel algoritme zoals beschreven in [Probeer algemene richtlijnen](../best-practices-retry-general.md) gepubliceerd door de groep Microsoft Patterns en procedures.
* **Time-outwaarden**
  * Houd rekening met uw werkbelasting en stel de waarden dienovereenkomstig. Als u grote waarden worden opgeslagen, stelt u de time-out voor op een hogere waarde.
  * Stel `AbortOnConnectFail` op false en laat StackExchange.Redis opnieuw verbinding maken voor u.
  * Gebruik één ConnectionMultiplexer exemplaar voor de toepassing. U kunt een LazyConnection gebruiken voor het maken van één exemplaar dat wordt geretourneerd door de verbindingseigenschap van een, zoals wordt weergegeven in [verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Stel de `ConnectionMultiplexer.ClientName` eigenschap in op een unieke naam van app-exemplaar voor diagnostische doeleinden.
  * Meerdere `ConnectionMultiplexer` exemplaren voor aangepaste werkbelastingen.
      * Als u verschillende belasting in uw toepassing hebt, kunt u dit model volgen. Bijvoorbeeld:
      * U kunt een multiplexer voor het omgaan met grote sleutels hebben.
      * U kunt een multiplexer voor het omgaan met kleine sleutels hebben.
      * U kunt verschillende waarden voor time-outs voor verbindingen en Pogingslogica voor elke ConnectionMultiplexer die u gebruikt.
      * Stel de `ClientName` eigenschap op elk multiplexer om te helpen met diagnostische gegevens.
      * In deze richtlijnen kan leiden tot meer gestroomlijnde latentie per `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Welke Redis-cache-clients kan ik gebruiken?
Een van de prettige dingen Redis is dat er veel clients ondersteunen veel verschillende ontwikkelingstalen zijn. Zie voor een huidige lijst van clients [Redis-clients](http://redis.io/clients). Zie voor zelfstudies over diverse verschillende talen en clients, [het gebruik van Azure Redis-Cache](cache-dotnet-how-to-use-azure-redis-cache.md) en klik op de gewenste taal in de taal schakelbaar aan de bovenkant van het artikel.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Is er een lokale emulator voor Azure Redis-Cache?
Er is geen lokale emulator voor Azure Redis-Cache, maar u kunt de versie MSOpenTech van redis-server.exe van uitvoeren de [Redis-opdrachtregelprogramma's](https://github.com/MSOpenTech/redis/releases/) op uw lokale machine en maak verbinding met het ophalen van een vergelijkbare ervaring naar een lokale cache-emulator. zoals u in het volgende voorbeeld:

    private static Lazy<ConnectionMultiplexer>
          lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });

        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


U kunt desgewenst configureren een [redis.conf](http://redis.io/topics/config) bestand dat u wilt meer overeenkomt met de [cache standaardinstellingen](cache-configure.md#default-redis-server-configuration) voor uw online Azure Redis-Cache indien gewenst.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Hoe kan ik de Redis-opdrachten uitvoeren?
U kunt een van de opdrachten die wordt vermeld op [Redis opdrachten](http://redis.io/commands#) , met uitzondering van de opdrachten op [Redis opdrachten niet ondersteund in Azure Redis-Cache](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). U hebt verschillende mogelijkheden Redis-opdrachten uit te voeren.

* Als u een Standard of Premium-cache hebt, u kunt met behulp van Redis-opdrachten uitvoeren de [Redis-Console](cache-configure.md#redis-console). De Redis-console biedt een veilige manier Redis-opdrachten kunt uitvoeren in de Azure-portal.
* U kunt ook de Redis-opdrachtregelprogramma's gebruiken. Als u wilt gebruiken, moet u de volgende stappen uitvoeren:
* Download de [Redis-opdrachtregelprogramma's](https://github.com/MSOpenTech/redis/releases/).
* Verbinding maken met behulp van de cache `redis-cli.exe`. Doorgeven in de cache-eindpunt met behulp die de -h switch en de sleutel met van - a zoals weergegeven in het volgende voorbeeld:
* `redis-cli -h <your cache="" name="">
  .redis.cache.windows.net -a <key>
  `

> [!NOTE]
> De Redis-opdrachtregelprogramma's werken niet met de SSL-poort, maar u kunt een hulpprogramma zoals `stunnel` om veilig met de hulpprogramma's voor de SSL-poort volgens de aanwijzingen in de [aangekondigd ASP.NET Session State-Provider voor Redis-Preview Release](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbericht.
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Waarom geen Azure Redis-Cache heeft een MSDN-bibliotheek-verwijzing voor klasse zoals enkele van de andere Azure-services?
Microsoft Azure Redis-Cache is gebaseerd op de populaire open-source Redis-Cache en toegankelijk zijn voor een groot aantal [Redis-clients](http://redis.io/clients) voor vele programmeertalen. Elke client heeft een eigen API waarmee aanroepen naar de Redis cache exemplaar met behulp van [Redis opdrachten](http://redis.io/commands).

Omdat elke client verschilt, is er niet één centrale klassenverwijzing op MSDN en elke client onderhoudt een eigen naslagdocumentatie. Naast de naslagdocumentatie zijn er verschillende zelfstudies waarin wordt beschreven hoe u aan de slag met Azure Redis-Cache met behulp van verschillende talen en cache-clients. Voor toegang tot deze zelfstudies, Zie [het gebruik van Azure Redis-Cache](cache-dotnet-how-to-use-azure-redis-cache.md) en klik op de gewenste taal in de taal schakelbaar aan de bovenkant van het artikel.

### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Kan ik Azure Redis-Cache gebruiken als een PHP-sessie-cache?
Ja, voor het gebruik van Azure Redis-Cache als de cache van een PHP-sessie, geef de verbindingsreeks naar uw Azure Redis-Cache-exemplaar in `session.save_path`.

> [!IMPORTANT]
> Wanneer u Azure Redis-Cache als een PHP-sessiecache, moet u de URL voor het coderen van de beveiligingssleutel waarmee verbinding met de cache, zoals wordt weergegeven in het volgende voorbeeld:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Als de sleutel geen URL gecodeerd is, ontvangt u mogelijk een uitzondering met een bericht zoals:`Failed to parse session.save_path`
>
>

Zie voor meer informatie over het gebruik van Redis-Cache als de cache van een PHP-sessie met de client PhpRedis [sessie PHP-handler](https://github.com/phpredis/phpredis#php-session-handler).

### <a name="what-are-redis-databases"></a>Wat zijn de Redis-databases?

Redis-Databases zijn alleen een logische scheiding van gegevens binnen hetzelfde Redis-exemplaar. Het cachegeheugen wordt gedeeld tussen alle databases en de werkelijke geheugen verbruik van een bepaalde database, is afhankelijk van de sleutels/waarden in de database opgeslagen. Zo heeft een cache C6 53 GB aan geheugen. U kunt alle 53 GB in één database plaatsen of u kunt het opsplitsen tussen meerdere databases. 

> [!NOTE]
> Als u een Premium Azure Redis-Cache met clusteren is ingeschakeld, wordt alleen database 0 beschikbaar is. Deze beperking is een ingebouwde Redis-beperking en is niet specifiek voor Azure Redis-Cache. Zie voor meer informatie [moet ik mijn clienttoepassing clustering Breng eventuele wijzigingen?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Wanneer moet ik de niet-SSL-poort voor het verbinden met Redis inschakelen?
Redis-server SSL geen systeemeigen ondersteuning, maar biedt Azure Redis-Cache. Als u verbinding met Azure Redis-Cache maakt en de client SSL, zoals StackExchange.Redis ondersteunt, moet u SSL gebruiken.

>[!NOTE]
>De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe exemplaren van Azure Redis-Cache. Als de client geen ondersteuning biedt voor SSL, dan u de poort zonder SSL-beveiliging inschakelen moet door de aanwijzingen in de [poorten openen](cache-configure.md#access-ports) sectie van de [een cache configureren in Azure Redis-Cache](cache-configure.md) artikel.
>
>

Hulpprogramma's zoals redis `redis-cli` werken niet met de SSL-poort, maar u kunt een hulpprogramma zoals `stunnel` om veilig met de hulpprogramma's voor de SSL-poort volgens de aanwijzingen in de [aankondigen van ASP.NET Session State-Provider voor Redis Preview-Release](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbericht.

Zie voor instructies over het downloaden van de Redis-hulpprogramma's, de [hoe kan ik Redis-opdrachten uitvoeren?](#cache-commands) sectie.

### <a name="what-are-some-production-best-practices"></a>Wat zijn enkele aanbevolen procedures voor productie?
* [Aanbevolen procedures van StackExchange.Redis](#stackexchangeredis-best-practices)
* [Configuratie en -concepten](#configuration-and-concepts)
* [Prestaties testen](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Aanbevolen procedures van StackExchange.Redis
* Stel `AbortConnect` op false, laat de ConnectionMultiplexer automatisch opnieuw verbinding maken. [Hier ziet voor meer informatie](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* De ConnectionMultiplexer hergebruiken - Maak een nieuwe voor elke aanvraag geen. De `Lazy<ConnectionMultiplexer>` patroon [hier weergegeven](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) wordt aanbevolen.
* Werkt beste redis met lagere waarden, moet deze hakken van grotere gegevens in meerdere sleutels. In [deze discussie Redis](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb groot wordt geacht. Lees [in dit artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) voor een voorbeeld-probleem dat kan worden veroorzaakt door grote waarden.
* Configureer uw [ThreadPool instellingen](#important-details-about-threadpool-growth) om te voorkomen dat time-outs.
* Gebruik ten minste de standaard connectTimeout van 5 seconden. Dit interval krijgt StackExchange.Redis voldoende tijd om de verbinding, in geval van een netwerk blip te herstellen.
* Let op de prestaties kosten in verband met verschillende bewerkingen die u uitvoert. Bijvoorbeeld, de `KEYS` opdracht is een bewerking O(n) en moeten worden vermeden. De [redis.io site](http://redis.io/commands/) bevat informatie over de complexiteit van de tijd voor elke bewerking dat het ondersteunt. Klik op elke opdracht om te zien van de complexiteit voor elke bewerking.

#### <a name="configuration-and-concepts"></a>Configuratie en -concepten
* Standard of Premium-laag voor productiesystemen gebruiken. De laag Basic is een systeem met één knooppunt met geen gegevensreplicatie en geen SLA. Gebruik ook ten minste een C1-cache. C0 caches worden meestal gebruikt voor scenario's eenvoudig ontwikkelen en testen.
* Houd er rekening mee dat Redis is een **In-Memory** gegevensarchief. Lees [in dit artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) zodat u zich bewust bent van scenario's waarbij gegevens verloren kan gaan.
* Uw systeem ontwikkelen, zodat deze kan omgaan met verbinding blips [vanwege patchen en failover](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Prestaties testen
* Start met `redis-benchmark.exe` te leren hoe voor mogelijke doorvoer voordat u uw eigen perf schrijft wordt getest. Omdat `redis-benchmark` biedt geen ondersteuning voor SSL, moet u [inschakelen via de Azure portal de niet-SSL-poort](cache-configure.md#access-ports) voordat u de test uitvoert. Zie voor voorbeelden [hoe kan ik benchmark en test de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* De virtuele machine die wordt gebruikt voor het testen van de client moet in dezelfde regio bevinden als uw exemplaar van Redis-cache.
* U kunt het beste Dv2 VM-reeks gebruikt voor de client als ze beschikken over betere hardware en de beste resultaten geeft.
* Zorg ervoor dat uw client VM die u kiest is ten minste net zoveel computing en mogelijkheden van de bandbreedte als de cache die u wilt testen.
* Schakel op de clientcomputer VRSS wanneer u in Windows. [Hier ziet voor meer informatie](https://technet.microsoft.com/library/dn383582.aspx).
* Premium-laag Redis exemplaren hebben latentie en doorvoer beter netwerk, omdat er op betere hardware voor zowel CPU en het netwerk worden uitgevoerd.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Wat zijn enkele van de overwegingen als u algemene Redis-opdrachten gebruikt?
* Sommige Redis-opdrachten die erg lang duren om uit te voeren, zonder het effect van deze opdrachten moet u niet uitvoeren.
  * Bijvoorbeeld, Voer niet de [sleutels](http://redis.io/commands/keys) opdracht in productie, zoals het lang duren kan, afhankelijk van het aantal sleutels retourneren. Redis is een server met één thread en opdrachten een tegelijk worden verwerkt. Als u andere opdrachten afgegeven na sleutels hebt, zullen ze niet worden verwerkt totdat Redis de opdracht sleutels verwerkt. De [redis.io site](http://redis.io/commands/) bevat informatie over de complexiteit van de tijd voor elke bewerking dat het ondersteunt. Klik op elke opdracht om te zien van de complexiteit voor elke bewerking.
* Sleutel grootten - moet ik gebruiken kleine sleutelwaarden of grote sleutelwaarden? In het algemeen zijn deze afhankelijk is van het scenario. Als uw scenario grotere sleutels vereist, kunt u de ConnectionTimeout aanpassen en probeer waarden en uw Pogingslogica aanpassen. Vanuit een perspectief Redis-server worden lagere waarden waargenomen voor betere prestaties.
* Deze overwegingen wekken dat u een hogere waarden niet opslaan in Redis; u moet rekening houden met het volgende. Latenties wordt hoger zijn. Als u één set gegevens die groter is en een kleinere hebt, kunt u meerdere exemplaren van ConnectionMultiplexer, elk met een andere set waarden voor time-outs en opnieuw hebt geconfigureerd, zoals beschreven in de vorige [wat de StackExchange.Redis doen configuratieopties doen](#cache-configuration) sectie.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Hoe kan ik benchmark en test de prestaties van mijn cache?
* [Schakel de diagnostische gegevens van de cache in](cache-how-to-monitor.md#enable-cache-diagnostics), zodat u de status van de cache kunt [bewaken](cache-how-to-monitor.md). U kunt de metrische gegevens weergeven in Azure Portal. U kunt ze ook [downloaden en bekijken](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) met een hulpprogramma van uw keuze.
* U kunt redis benchmark.exe laden test Redis-server.
* Zorg ervoor dat de belasting testen van client en de Redis-cache in dezelfde regio.
* Gebruik redis cli.exe en controleren van de cache met behulp van de opdracht INFO.
* Als de belasting wordt veroorzaakt door hoge geheugenfragmentatie, moet u opschalen naar een grotere cachegrootte.
* Zie voor instructies over het downloaden van de Redis-hulpprogramma's, de [hoe kan ik Redis-opdrachten uitvoeren?](#cache-commands) sectie.

De volgende opdrachten vindt u een voorbeeld van het gebruik van redis-benchmark.exe. Voer deze opdrachten van een virtuele machine in dezelfde regio bevinden als uw cache voor nauwkeurige resultaten.

* Test gebruikt in een pijplijn SET-aanvragen met een 1 k-nettolading

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Test gebruikt in een pijplijn ophalen aanvragen met een 1 k-nettolading.
  Opmerking: De SET testen hierboven weergegeven eerst om cache te vullen

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>Belangrijke informatie over de ThreadPool groei
De CLR-ThreadPool heeft twee soorten threads - 'Worker' en 'I/o-voltooiingspoort' (aka Voltooiingspoort) threads.

* Werkthreads worden gebruikt voor de verwerking van de beheerder `Task.Run(…)` of `ThreadPool.QueueUserWorkItem(…)` methoden. Deze threads worden ook gebruikt door diverse onderdelen van de CLR wanneer werk moet gebeuren op een achtergrond-thread.
* Voltooiingspoort threads worden gebruikt als asynchrone i/o gebeurt (bijvoorbeeld lezen van het netwerk).

De thread-groep biedt nieuwe werkthreads of i/o-voltooiing threads op verzoek (zonder een bandbreedtebeperking) totdat de instelling 'Minimale' voor elk type thread is bereikt. Het minimum aantal threads is standaard ingesteld op het aantal processors op een systeem.

Wanneer het aantal bestaande (bezet) threads de 'minimale' aantal threads raakt, wordt de snelheid waarmee het nieuwe threads één thread per 500 milliseconden injects vertraging in de ThreadPool. Normaal gesproken als uw systeem een ' burst ' van het werk dat een thread Voltooiingspoort opgehaald, wordt verwerkt die werken zeer snel. Echter, als de omvang van het werk is meer dan de geconfigureerde instelling voor de 'Minimale', zal er enige vertraging bij het verwerken van een gedeelte van het werk als de ThreadPool wordt gewacht op twee manieren gebeuren.

1. Een bestaande thread beschikbaar voor het verwerken van het werk.
2. Er zijn geen bestaande thread beschikbaar voor 500ms, zodat een nieuwe thread is gemaakt.

In principe betekent dit dat als het aantal threads bezet groter dan de Min-threads is, u waarschijnlijk een vertraging 500ms betaalt voordat netwerkverkeer wordt verwerkt door de toepassing. Het is ook belangrijk te weten dat als een bestaande thread niet-actieve langer dan 15 seconden blijft (gebaseerd op wat ik onthouden), zal worden opgeruimd en wordt deze cyclus van groei en inkrimping kunt herhalen.

Als we een foutbericht voorbeeld van StackExchange.Redis (1.0.450 bouwen of hoger), ziet u deze nu wordt afgedrukt ThreadPool-statistieken (Zie Voltooiingspoort- en WERKROLLEN details hieronder).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

In het vorige voorbeeld ziet u dat voor Voltooiingspoort thread 6 bezet threads zijn en het systeem is geconfigureerd, zodat 4 minimaal threads. In dit geval wordt de client zou hebben waarschijnlijk gezien twee 500 ms vertragingen omdat 6 > 4.

Houd er rekening mee dat time-outs in StackExchange.Redis kan worden bereikt als groei van Voltooiingspoort of WORKER threads wordt beperkt.

### <a name="recommendation"></a>Aanbeveling
Deze informatie gegeven, wordt aangeraden dat klanten de waarde van de minimale configuratievereisten voor Voltooiingspoort en WORKER threads ingesteld op iets groter is dan de standaardwaarde. Er kan geen standaardoplossing leidraad op deze waarde moet omdat de juiste waarde voor een toepassing te hoge en lage voor een andere toepassing. Deze instelling kan ook invloed op de prestaties van andere onderdelen van complexe toepassingen, zodat elke klant moet stemmen deze instelling op hun specifieke behoeften. Een goed uitgangspunt gebruikt is 200 of 300, en vervolgens testen en indien nodig aanpassen.

Het configureren van deze instelling:

* In ASP.NET, gebruikt u de ["minIoThreads" configuratie-instelling] [ "minIoThreads" configuration setting] onder de `<processModel>` configuratie-element in web.config. Als u in Azure WebSites uitvoert, wordt deze instelling niet beschikbaar via de configuratieopties. Echter nog steeds moet u kunnen voor het configureren van deze instelling via programmacode (Zie hieronder) van uw methode Application_Start in global.asax.cs.

  > [!NOTE] 
  > De waarde die is opgegeven in deze configuratie-element is een *core-* instelling. Bijvoorbeeld, als u een machine 4 kernen hebt en wilt dat uw instelling minIOThreads 200 tijdens runtime, gebruikt u `<processModel minIoThreads="50"/>`.
  >

* Buiten de ASP.NET-, gebruiken de [ThreadPool.SetMinThreads(...) ](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API.

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Inschakelen van de server GC meer doorvoer ophalen op de client bij gebruik van StackExchange.Redis
Inschakelen van de server GC kunt optimaliseren van de client en betere prestaties en doorvoer bieden bij het gebruik van StackExchange.Redis. Zie de volgende artikelen voor meer informatie over de GC-server en hoe deze in te schakelen:

* [GC server inschakelen](https://msdn.microsoft.com/library/ms229357.aspx)
* [De grondbeginselen van garbagecollection](https://msdn.microsoft.com/library/ee787088.aspx)
* [Garbagecollection en prestaties](https://msdn.microsoft.com/library/ee851764.aspx)


### <a name="performance-considerations-around-connections"></a>Prestatieoverwegingen rond verbindingen

Elke prijscategorie heeft verschillende beperkingen voor clientverbindingen, geheugen en bandbreedte. Kunt u elke grootte van cache *tot* een bepaald aantal verbindingen, elke verbinding met Redis heeft overhead gekoppeld. Een voorbeeld van een dergelijke overhead zou zijn CPU- en geheugengebruik als gevolg van TLS/SSL-versleuteling. De maximale verbindingslimiet voor een opgegeven cachegrootte wordt ervan uitgegaan dat een licht geladen cache. Als uit verbinding overhead laden *plus* laden vanaf de clientbewerkingen dan er capaciteit voor het systeem, de cache kunt capaciteitsproblemen moeten ondervinden zelfs als u hebt niet de verbindingslimiet voor de huidige cachegrootte overschreden.

Zie voor meer informatie over de verschillende verbindingen beperkingen voor elke laag [prijzen van Azure Redis-Cache](https://azure.microsoft.com/pricing/details/cache/). Zie voor meer informatie over verbindingen en andere standaardconfiguraties [serverconfiguratie standaard Redis](cache-configure.md#default-redis-server-configuration).

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Hoe bewaak ik de status en prestaties van mijn cache?
Exemplaren van Microsoft Azure Redis-Cache kunnen worden bewaakt in de [Azure-portal](https://portal.azure.com). U kunt metrische gegevens weergeven, vastmaken grafieken van de metrische gegevens aan het Startboard, aanpassen van het bereik datum en tijd van de bewaking van grafieken, toevoegen en metrische gegevens verwijderen uit de grafieken en waarschuwingen instellen wanneer aan bepaalde voorwaarden wordt voldaan. Zie voor meer informatie [Monitor Azure Redis-Cache](cache-how-to-monitor.md).

De Redis-Cache **Resource menu** bevat ook verschillende hulpprogramma's voor bewaking en probleemoplossing van uw caches.

* **Diagnosticeren en oplossen van problemen met** bevat informatie over veelvoorkomende problemen en strategieën voor het oplossen ervan.
* **Resourcestatus** controleert uw resource en geeft u aan als deze wordt uitgevoerd zoals verwacht. Zie voor meer informatie over de health-service van Azure Resource [overzicht van Azure Resource health](../resource-health/resource-health-overview.md).
* **Nieuw ondersteuningsverzoek** biedt opties om ondersteuning te vragen voor uw cache.

Deze hulpprogramma's kunnen u de status van uw Azure Redis-Cache-exemplaren en uw cache in toepassingen te beheren. Voor meer informatie, Zie de sectie "Ondersteuning & instellingen voor het oplossen van problemen" van [het configureren van Azure Redis-Cache](cache-configure.md).

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Waarom krijg ik time-outs zien?
Time-outs optreden in de client die u gebruikt voor communicatie met Redis. Wanneer een opdracht naar de Redis-server wordt verzonden, wordt de opdracht is in de wachtrij geplaatst en Redis-server uiteindelijk neemt over de opdracht en deze uitvoert. Maar de client kunt time-out tijdens dit proces en als dit het geval is een uitzondering optreedt op de aanroepende kant. Zie voor meer informatie over het oplossen van problemen met time-out [clientzijde probleemoplossing](cache-how-to-troubleshoot.md#client-side-troubleshooting) en [StackExchange.Redis time-out-uitzonderingen](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Waarom is mijn klant losgekoppeld van de cache?
Hier volgen enkele veelvoorkomende reden voor een cache verbinding verbreken.

* Client-side oorzaken
  * De clienttoepassing is geïmplementeerd.
  * De clienttoepassing een vergroten/verkleinen is uitgevoerd.
    * In het geval van Cloudservices of Web-Apps mogelijk vanwege automatisch schalen.
  * De netwerklaag op de client is gewijzigd.
  * Tijdelijke fouten opgetreden in de client of in de netwerkknooppunten tussen de client en de server.
  * De drempelwaarde bandbreedtelimieten zijn bereikt.
  * CPU-gebonden bewerkingen duurt te lang om te voltooien.
* Serverzijde oorzaken
  * De Azure Redis-Cache-service gestart op de standaard cache aanbieden, een failover van het primaire knooppunt naar het secundaire knooppunt.
  * Azure is het exemplaar waarop de cache is geïmplementeerd patchen
    * Dit kan zijn voor Redis-serverupdates of algemeen onderhoud van de virtuele machine.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Welke Azure Cache-aanbieding is juist voor mij?
> [!IMPORTANT]
> Aan de hand van vorig jaar [aankondiging](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), Azure Managed Cache Service en Azure In-Role Cache service **buiten gebruik gesteld** op 30 November 2016. Onze aanbeveling is het gebruik van [Azure Redis-Cache](https://azure.microsoft.com/services/cache/). Zie voor meer informatie over het migreren van [migreren van Managed Cache Service naar Azure Redis-Cache](cache-migrate-to-redis.md).
>
>

### <a name="azure-redis-cache"></a>Azure Redis-cache
Azure Redis-Cache is algemeen beschikbaar is in groottes tot 53 GB en heeft een beschikbaarheids-SLA van 99,9%. De nieuwe [premium-laag](cache-premium-tier-intro.md) biedt grootten tot 530 GB en ondersteuning voor clustering, VNET en persistentie, met een SLA met 99,9%.

Azure Redis-Cache geeft klanten de mogelijkheid om met een beveiligde, toegewezen Redis-cache, beheerd door Microsoft. Met deze aanbieding krijgt u gebruikmaken van de uitgebreide functieset en geleverd door Redis, en betrouwbare hosting en controle van Microsoft-ecosysteem.

In tegenstelling tot traditionele caches die alleen met sleutel-waardeparen te maken, is de Redis populaire voor de maximaal zodat gegevenstypen. Redis ook ondersteunt uitvoering atomische bewerkingen op deze typen, zoals toe te voegen aan een tekenreeks; de waarde in een hash; en oplopend in stappen pushen naar een lijst; Computing set snijpunt, union en verschil; of voor het verkrijgen van het lid met de hoogste positie in een gesorteerde set. Andere functies bieden ondersteuning voor transacties, pub subitems, scripts, sleutels met een beperkte time-to-live, Lua en configuratie-instellingen aanbrengen Redis gedragen zich meer als een traditionele cache.

Een ander belangrijk aspect op Redis geslaagd is gebaseerd op het ecosysteem van de orde, heldere open-source. Dit wordt weergegeven in de uitgebreide set met Redis-clients in meerdere talen. Dit ecosysteem en een breed scala aan clients kunt Azure Redis-Cache moet worden gebruikt door vrijwel elke werkbelasting die u in Azure maken wilt.

Zie voor meer informatie over aan de slag met Azure Redis-Cache, [hoe gebruikt Azure Redis-Cache](cache-dotnet-how-to-use-azure-redis-cache.md) en [documentatie van Azure Redis-Cache](index.md).

### <a name="managed-cache-service"></a>Beheerde cacheservice
[Managed Cache service 30 November 2016 buiten gebruik werd gesteld.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Gearchiveerde documentatie Zie [gearchiveerde Managed Cache Service documentatie](https://msdn.microsoft.com/library/azure/dn386094.aspx).

### <a name="in-role-cache"></a>In-Role Cache
[In-Role Cache 30 November 2016 buiten gebruik werd gesteld.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Gearchiveerde documentatie Zie [gearchiveerde In-Role Cache documentatie](https://msdn.microsoft.com/library/azure/dn386103.aspx).

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
