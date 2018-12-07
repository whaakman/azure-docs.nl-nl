---
title: Azure voor veelgestelde vragen over Redis-Cache | Microsoft Docs
description: Meer van de antwoorden op veelgestelde vragen, patronen en best practices voor Azure Cache voor Redis
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: c2c52b7d-b2d1-433a-b635-c20180e5cab2
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: azure-cache-for-redis
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: wesmc
ms.openlocfilehash: 3af59bd3b19744983a44157e108430620fae3532
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53019787"
---
# <a name="azure-cache-for-redis-faq"></a>Azure voor veelgestelde vragen over Redis-Cache
Informatie over de antwoorden op veelgestelde vragen, patronen en best practices voor Azure Cache voor Redis.

## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?
Als uw vraag hier niet wordt weergegeven, laat het ons weten en wij helpen u een antwoord vinden.

* U kunt een vraag stellen in de opmerkingen aan het einde van deze Veelgestelde vragen en neem contact op met de Azure Cache-team en andere communityleden over dit artikel.
* Als u wilt een breder publiek bereiken, kunt u een vraag plaatst op de [MSDN-Forum voor Azure Cache](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) en neem contact op met de Azure Cache-team en andere leden van de community.
* Als u maken van een functie-aanvraag wilt, kunt u uw aanvragen en ideeën indienen [Azure Cache voor Redis User Voice](https://feedback.azure.com/forums/169382-cache).
* U kunt ook een e-mailbericht verzenden naar ons op [externe feedback over Azure Cache](mailto:azurecache@microsoft.com).

## <a name="azure-cache-for-redis-basics"></a>Azure Cache voor Redis-basisbeginselen
De veelgestelde vragen over de in deze sectie betrekking hebben op sommige van de basisprincipes van Azure Cache voor Redis.

* [Wat is Azure voor Redis-Cache?](#what-is-azure-redis-cache)
* [Hoe kan ik aan de slag met Azure Cache voor Redis?](#how-can-i-get-started-with-azure-redis-cache)

De volgende veelgestelde vragen gaan over basisconcepten en vragen over Azure Cache voor Redis en in een van de andere secties van de veelgestelde vragen worden beantwoord.

* [Welke Azure Cache voor Redis-aanbieding en de grootte moet ik gebruiken?](#what-redis-cache-offering-and-size-should-i-use)
* [Welke Azure Cache voor Redis-clients kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
* [Is er een lokale emulator voor Azure Cache voor Redis?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Hoe controleer ik de status en prestaties van mijn cache?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Veelgestelde vragen over plannen
* [Welke Azure Cache voor Redis-aanbieding en de grootte moet ik gebruiken?](#what-redis-cache-offering-and-size-should-i-use)
* [Azure Cache voor Redis-prestaties](#azure-redis-cache-performance)
* [In welke regio moet ik mijn cache vinden?](#in-what-region-should-i-locate-my-cache)
* [Hoe Word ik gefactureerd voor Azure Cache voor Redis?](#how-am-i-billed-for-azure-redis-cache)
* [Kan ik Azure Cache gebruiken voor Redis met Azure Government-Cloud, Azure China-Cloud of Microsoft Azure Duitsland?](#can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Veelgestelde vragen over ontwikkeling
* [Wat doet de StackExchange.Redis-configuratie-opties?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Welke Azure Cache voor Redis-clients kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
* [Is er een lokale emulator voor Azure Cache voor Redis?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Hoe kan ik de Redis-opdrachten uitvoeren?](#how-can-i-run-redis-commands)
* [Waarom niet Azure voor Redis-Cache hebt u een MSDN-klassebibliotheekverwijzing zoals enkele van de andere Azure-services?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Kan ik Azure Cache voor Redis gebruiken als de cache van een PHP-sessie?](#can-i-use-azure-redis-cache-as-a-php-session-cache)
* [Wat zijn Redis-databases?](#what-are-redis-databases)

## <a name="security-faqs"></a>Veelgestelde vragen over Security
* [Wanneer moet ik de niet-SSL-poort voor het verbinden met Redis inschakelen?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Veelgestelde vragen over de productie
* [Wat zijn enkele aanbevolen procedures voor productie?](#what-are-some-production-best-practices)
* [Wat zijn enkele overwegingen bij het gebruik van algemene Redis-opdrachten?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Hoe kan ik benchmark-test uitvoeren en testen van de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Belangrijke informatie over de groei van de ThreadPool](#important-details-about-threadpool-growth)
* [Inschakelen van de server GC meer om doorvoer te krijgen op de client bij het gebruik van StackExchange.Redis](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Prestatie-overwegingen over verbindingen](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>Controle en veelgestelde vragen over probleemoplossing
De veelgestelde vragen over de in deze sectie betrekking op veelvoorkomende bewaking en vragen over probleemoplossing. Zie voor meer informatie over controle en probleemoplossing van uw Azure-Cache voor instanties van Redis [Azure Cache controleren voor Redis](cache-how-to-monitor.md) en [problemen oplossen met Azure Cache voor Redis](cache-how-to-troubleshoot.md).

* [Hoe controleer ik de status en prestaties van mijn cache?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Waarom krijg ik time-outs zien?](#why-am-i-seeing-timeouts)
* [Waarom is mijn klant losgekoppeld van de cache?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Veelgestelde vragen over de voorafgaande Cache-aanbieding
* [Welke Azure Cache-aanbieding is juist voor mij?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-cache-for-redis"></a>Wat is Azure voor Redis-Cache?
Azure Redis-Cache is gebaseerd op de populaire open-source [Azure Cache voor Redis](http://redis.io). Het biedt u toegang hebt tot een beveiligde, toegewezen Cache van Azure voor Redis, beheerd door Microsoft en toegankelijk is vanuit elke toepassing in Azure. Zie voor een gedetailleerder overzicht de [Azure Cache voor Redis](https://azure.microsoft.com/services/cache/) -productpagina op Azure.com.

### <a name="how-can-i-get-started-with-azure-cache-for-redis"></a>Hoe kan ik aan de slag met Azure Cache voor Redis?
Er zijn verschillende manieren waarop die u kunt aan de slag met Azure Cache voor Redis.

* U kunt bekijken een van onze zelfstudies beschikbaar voor [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md), en [Python](cache-python-get-started.md).
* U kunt bekijken [hoe bouw krachtige Apps met behulp van Microsoft Azure Cache voor Redis](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
* U kunt de clientdocumentatie voor de clients die overeenkomen met de programmeertaal van uw project om te zien over het gebruik van Redis bekijken. Er zijn veel Redis-clients die kunnen worden gebruikt met Azure Cache voor Redis. Zie voor een lijst van Redis-clients, [ http://redis.io/clients ](http://redis.io/clients).

Als u nog een Azure-account hebt, kunt u het volgende doen:

* [Gratis een Azure-account openen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). U ontvangt tegoed dat kan worden gebruikt om betaalde Azure-services uit te proberen. Zelfs nadat het tegoed is gebruikt, kunt u het account houden en de gratis Azure-services en -functies gebruiken.
* [Uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Via uw MSDN-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken.

<a name="cache-size"></a>

### <a name="what-azure-cache-for-redis-offering-and-size-should-i-use"></a>Welke Azure Cache voor Redis-aanbieding en de grootte moet ik gebruiken?
Elke Azure-Cache voor Redis-aanbieding biedt verschillende niveaus van **grootte**, **bandbreedte**, **hoge beschikbaarheid**, en **SLA** opties.

Hieronder vindt u overwegingen voor het kiezen van een Cache-aanbieding.

* **Geheugen**: de lagen basis en standaard bieden 250 MB: 53 GB. De Premium-laag biedt tot 530 GB. Zie voor meer informatie, [Azure Cache voor Redis-prijsstelling](https://azure.microsoft.com/pricing/details/cache/).
* **Prestaties van het netwerk**: hebt u een werkbelasting waarvoor hoge doorvoer, de Premium-laag biedt meer bandbreedte in vergelijking met de Standard- of Basic. Binnen elke laag hebben grotere grootte caches ook meer bandbreedte vanwege de onderliggende virtuele machine die als host fungeert voor de cache. Zie de [na de tabel](#cache-performance) voor meer informatie.
* **Doorvoer**: de Premium-laag biedt de maximaal beschikbare doorvoer. Als de cacheserver of de client de bandbreedtelimieten is bereikt, ontvangt u mogelijk time-outs op de client. Zie de volgende tabel voor meer informatie.
* **Hoge beschikbaarheid/SLA**: Azure Redis Cache garandeert dat een Standard of Premium-cache beschikbaar ten minste 99,9% van de tijd is. Zie voor meer informatie over onze SLA [Azure Cache voor Redis-prijsstelling](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). De SLA heeft alleen betrekking op connectiviteit met de Cache-eindpunten. De SLA heeft geen betrekking op beveiliging tegen verlies van gegevens. U wordt aangeraden de functie in de persistentie voor Redis-gegevens in de Premium-laag gebruik te maken om tolerantie tegen verlies van gegevens.
* **Redis-Gegevenspersistentie**: de Premium-laag kunt u het behoud van de cachegegevens in een Azure Storage-account. Alle de gegevens worden opgeslagen in een cache basis/standaard alleen in het geheugen. Als er mag er onderliggende infrastructuurfouten verlies van gegevens. U wordt aangeraden de functie in de persistentie voor Redis-gegevens in de Premium-laag gebruik te maken om tolerantie tegen verlies van gegevens. Azure Redis-Cache biedt RDB en (binnenkort beschikbaar) AOF opties in de Redis-persistentie. Zie voor meer informatie, [persistentie voor een Premium Azure Cache voor Redis configureren](cache-how-to-premium-persistence.md).
* **Redis-Cluster**: om te maken in de cache opslaat groter is dan 53 GB, of gegevens over meerdere Redis-knooppunten, kunt u Redis-clustering, die beschikbaar is in de Premium-laag. Elk knooppunt bestaat uit een primaire/replica-cache paar voor maximale beschikbaarheid. Zie voor meer informatie, [configureren voor een Premium Azure Cache voor Redis clustering](cache-how-to-premium-clustering.md).
* **Verbeterde beveiliging en isolatie**: implementatie van Azure Virtual Network (VNET) biedt verbeterde beveiliging en isolatie voor uw Azure-Cache voor Redis, evenals subnetten, toegangsbeheerbeleid en andere functies voor het verder beperken van toegang. Zie voor meer informatie, [het configureren van Virtual Network-ondersteuning voor een Premium Azure Cache voor Redis](cache-how-to-premium-vnet.md).
* **Redis configureren**: u kunt In de Standard- en Premium-lagen, Redis configureren voor Keyspace-meldingen.
* **Maximum aantal clientverbindingen**: de Premium-laag biedt het maximum aantal clients die verbinding met Redis, met een hoger aantal verbindingen voor grotere grootte caches maken kunnen. Clustering verhoogt niet het aantal verbindingen die beschikbaar zijn voor een geclusterde cache. Zie voor meer informatie, [Azure Cache voor Redis-prijsstelling](https://azure.microsoft.com/pricing/details/cache/).
* **Toegewezen Kerngeheugens voor Redis-Server**: In de Premium-laag, hebben alle cachegrootte een toegewezen kerngeheugens voor Redis. In de lagen Basic/Standard, de grootte van de C1 en hierboven hebt u een speciale core voor Redis-server.
* **Redis is één thread** zodat met meer dan twee cores extra voordeel biedt ten opzichte van dat u hoeft slechts twee kernen, maar grotere VM-grootten meestal meer bandbreedte dan kleinere hebben. Als de cacheserver of de client de bandbreedtelimieten is bereikt, ontvangt u time-outs op de client.
* **Verbeterde prestaties**: Caches in de laag Premium zijn geïmplementeerd op hardware met snellere processors, zodat betere prestaties in vergelijking met de Basic- of Standard-laag. Premium-laag-Caches hebben een hogere doorvoer en een lagere latentie.

<a name="cache-performance"></a>

### <a name="azure-cache-for-redis-performance"></a>Azure Cache voor Redis-prestaties
De volgende tabel ziet u de waarden van maximale bandbreedte tijdens het testen van verschillende grootten van de laag Standard en Premium-caches met behulp van `redis-benchmark.exe` vanuit een Iaas-VM op basis van de Azure-Cache voor Redis-eindpunt. Voor SSL-doorvoer, redis-benchmark gebruikt met beveiligde tunnel om verbinding met de Azure-Cache voor Redis-eindpunt te maken.

>[!NOTE] 
>Deze waarden zijn niet noodzakelijkerwijs en er is geen SLA voor deze getallen, maar moet zijn. U moet laden testen van uw eigen toepassing om te bepalen van de grootte van de juiste cache voor uw toepassing.
>Deze getallen kunnen worden gewijzigd omdat er nieuwere resultaten periodiek plaatsen.
>

We kunnen de volgende conclusies te trekken uit deze tabel:

* Doorvoer voor de caches die even groot is hoger in de Premium-laag in vergelijking met de Standard-laag. Bijvoorbeeld, met een 6 GB-Cache is de doorvoer van P1 180.000 RPS in vergelijking met 100.000 voor C3.
* Met Redis-clustering, verhoogt doorvoer lineair toe als u het aantal shards (knooppunten) in het cluster vergroten. Als u een cluster P4 van 10 shards maken, klikt u vervolgens de beschikbare doorvoer is bijvoorbeeld 400.000 * 10 = 4 miljoen RPS.
* Doorvoer voor grotere sleutelgrootten is hoger in de Premium-laag in vergelijking met de Standard-laag.

| Prijscategorie | Grootte | CPU-kernen | Beschikbare bandbreedte | De grootte van 1 KB | De grootte van 1 KB |
| --- | --- | --- | --- | --- | --- |
| **Standaard cachegrootte** | | |**Megabits per seconde (Mb/s) / MB per seconde (MB/s)** |**Aanvragen per seconde (RPS) niet-SSL** |**Aanvragen per seconde (RPS) SSL** |
| C0 |250 MB |Gedeeld |100 / 12.5 |15.000 |7.500 |
| C1 |1 GB |1 |500 / 62.5 |38.000 |20,720 |
| C2 |2,5 GB |2 |500 / 62.5 |41,000 |37,000 |
| C3 |6 GB |4 |1000 / 125 |100.000 |90,000 |
| C4 |13 GB |2 |500 / 62.5 |60,000 |55,000 |
| C5 |26 GB |4 |1,000 / 125 |102.000 |93,000 |
| C6 |53 GB |8 |2,000 / 250 |126,000 |120,000 |
| **Premium-cachegrootte** | |**CPU-kernen per shard** | **Megabits per seconde (Mb/s) / MB per seconde (MB/s)** |**Aanvragen per seconde (RPS) niet-SSL, per shard** |**Aanvragen per seconde (RPS) SSL, per shard** |
| P1 |6 GB |2 |1,500 / 187.5 |180,000 |172,000 |
| P2 |13 GB |4 |3,000 / 375 |350,000 |341,000 |
| P3 |26 GB |4 |3,000 / 375 |350,000 |341,000 |
| P4 |53 GB |8 |6,000 / 750 |400,000 |373,000 |

Voor instructies over het instellen van een beveiligde tunnel of downloaden van de Redis-hulpprogramma's zoals `redis-benchmark.exe`, Zie de [hoe kan ik Redis-opdrachten uitvoeren?](#cache-commands) sectie.

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>In welke regio moet ik mijn cache vinden?
Zoek uw Azure-Cache voor Redis in dezelfde regio als de clienttoepassing van uw cache voor optimale prestaties en de laagste latentie.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-cache-for-redis"></a>Hoe Word ik gefactureerd voor Azure Cache voor Redis?
Azure voor de prijzen voor Redis-Cache is [hier](https://azure.microsoft.com/pricing/details/cache/). De pagina met prijzen bevat als een uurtarief prijzen. Caches worden in rekening gebracht op basis van per minuut vanaf het moment dat de cache wordt gemaakt tot het moment dat een cache wordt verwijderd. Er bestaat geen optie voor het stoppen of onderbreken van een cache in rekening.

### <a name="can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Kan ik Azure Cache gebruiken voor Redis met Azure Government-Cloud, Azure China-Cloud of Microsoft Azure Duitsland?
Ja, is Azure Redis Cache beschikbaar in Azure Government-Cloud, Azure China-Cloud en Microsoft Azure Duitsland. De URL's voor het raadplegen en beheren van Azure Cache voor Redis verschillen in deze clouds vergeleken met de openbare Azure-Cloud. 

| Cloud   | DNS-achtervoegsel voor Redis            |
|---------|---------------------------------|
| Public  | *.redis.cache.windows.net       |
| Amerikaanse overheid  | *.redis.cache.usgovcloudapi.net |
| Duitsland | *.redis.cache.cloudapi.de       |
| China   | *.redis.cache.chinacloudapi.cn  |

Zie de volgende koppelingen voor meer informatie over overwegingen bij het gebruik van Azure Cache voor Redis met andere clouds.

- [Databases in Azure Government - Azure Redis-Cache](../azure-government/documentation-government-services-database.md#azure-cache-for-redis)
- [Azure China-Cloud - Azure Redis-Cache](https://www.azure.cn/documentation/services/azure-cache-for-redis/)
- [Microsoft Azure Duitsland](https://azure.microsoft.com/overview/clouds/germany/)

Zie voor meer informatie over het gebruik van Azure Cache voor Redis met PowerShell in Azure Government-Cloud, Azure China-Cloud en Microsoft Azure Duitsland [verbinding maken met andere clouds - Azure-Cache voor PowerShell Redis](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Wat doet de StackExchange.Redis-configuratie-opties?
StackExchange.Redis bevat veel opties. In deze sectie vertelt over een aantal van de algemene instellingen. Zie voor meer informatie over de StackExchange.Redis-opties, [StackExchange.Redis configuratie](https://stackexchange.github.io/StackExchange.Redis/Configuration).

| ConfigurationOptions | Description | Aanbeveling |
| --- | --- | --- |
| AbortOnConnectFail |Wanneer is ingesteld op true, wordt de verbinding wordt niet opnieuw verbinding maken na een netwerkstoring op een. |Ingesteld op false en laat StackExchange.Redis automatisch opnieuw verbinding maken. |
| ConnectRetry |Het aantal keren laten herhalen verbindingspogingen tijdens de eerste verbinding. |Zie de volgende opmerkingen voor hulp. |
| ConnectTimeout |Time-out in ms voor verbindingsbewerkingen. |Zie de volgende opmerkingen voor hulp. |

De standaardwaarden van de client zijn meestal voldoende. U kunt de opties op basis van uw werkbelasting kunt afstemmen.

* **Nieuwe pogingen**
  * Voor ConnectRetry en ConnectTimeout is de algemene richtlijnen snel mislukt en probeer het opnieuw. Deze handleiding is gebaseerd op de werkbelasting en de hoeveelheid tijd op gemiddelde het duurt voor de client naar een Redis-opdracht geven en een antwoord kan ontvangen.
  * Laat StackExchange.Redis automatisch opnieuw verbinding maken in plaats van de verbindingsstatus van de controleren en het herstellen van uzelf. **Vermijd het gebruik van de eigenschap ConnectionMultiplexer.IsConnected**.
  * Snowballing - soms u mogelijk ondervindt een probleem waarbij u opnieuw te proberen en de nieuwe pogingen snowball en nooit wordt hersteld. Als er snowballing optreedt, moet u overwegen een algoritme van de nieuwe pogingen exponentieel uitstel zoals beschreven in [algemene richtlijnen voor opnieuw proberen](../best-practices-retry-general.md) gepubliceerd door de Microsoft Patterns & Practices-groep.
* **Time-outwaarden**
  * Houd rekening met uw werkbelasting en stel de waarden dienovereenkomstig. Als u grote waarden opslaat, stelt u de time-out op een hogere waarde.
  * Stel `AbortOnConnectFail` op false en laat StackExchange.Redis opnieuw verbinding maken voor u.
  * Gebruik een enkel ConnectionMultiplexer-exemplaar voor de toepassing. U kunt een LazyConnection gebruiken om te maken van een enkele instantie die wordt geretourneerd door een eigenschap Connection, zoals wordt weergegeven in [verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Stel de `ConnectionMultiplexer.ClientName` eigenschap aan een unieke naam van app-exemplaar voor diagnostische doeleinden.
  * Gebruik van meerdere `ConnectionMultiplexer` exemplaren voor aangepast werkbelastingen.
      * U kunt dit model kunt volgen, hebt u variërende belastingen in uw toepassing. Bijvoorbeeld:
      * U kunt één multiplexer voor het omgaan met grote sleutels hebben.
      * U kunt één multiplexer voor het omgaan met kleine sleutels hebben.
      * U kunt verschillende waarden voor de verbindingstime-outs en logica voor opnieuw proberen voor elke ConnectionMultiplexer die u gebruikt.
      * Stel de `ClientName` eigenschap op elk multiplexer om te helpen met diagnostische gegevens.
      * In deze richtlijnen kan leiden tot meer latentie per gestroomlijnd `ConnectionMultiplexer`.

### <a name="what-azure-cache-for-redis-clients-can-i-use"></a>Welke Azure Cache voor Redis-clients kan ik gebruiken?
Een van de geweldige dingen over Redis is dat er veel clients ondersteunt veel verschillende programmeertalen. Zie voor een huidige lijst van clients, [Redis-clients](http://redis.io/clients). Zie voor een zelfstudie die betrekking hebben op meerdere verschillende talen en -clients, [over het gebruik van Azure Cache voor Redis](cache-dotnet-how-to-use-azure-redis-cache.md) en klik op de gewenste taal van de wisselaar taal aan de bovenkant van het artikel.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-cache-for-redis"></a>Is er een lokale emulator voor Azure Cache voor Redis?
Er is geen lokale emulator voor Azure Redis Cache, maar u kunt de MSOpenTech-versie van redis-server.exe van uitvoeren de [Redis-opdrachtregelprogramma's](https://github.com/MSOpenTech/redis/releases/) op uw lokale machine en maak verbinding met het ophalen van een vergelijkbare ervaring tot een lokale cache -emulator, zoals wordt weergegeven in het volgende voorbeeld:

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


U kunt desgewenst configureren een [redis.conf](http://redis.io/topics/config) bestand dichter bij de [standaardinstellingen van de cache](cache-configure.md#default-redis-server-configuration) voor uw online Azure-Cache voor Redis indien gewenst.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Hoe kan ik de Redis-opdrachten uitvoeren?
U kunt de opdrachten die wordt vermeld op [Redis-opdrachten](http://redis.io/commands#) , met uitzondering van de opdrachten die wordt vermeld op [Redis-opdrachten niet ondersteund in Azure Cache voor Redis](cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis). U hebt verschillende mogelijkheden om uit te voeren van Redis-opdrachten.

* Als u een Standard of Premium-cache hebt, kunt u de Redis-opdrachten met uitvoeren de [Redis-Console](cache-configure.md#redis-console). De Redis-console biedt een veilige manier Redis-opdrachten kunt uitvoeren in Azure portal.
* U kunt ook de Redis-opdrachtregelprogramma's gebruiken. Voor het gebruik ervan, moet u de volgende stappen uitvoeren:
* Download de [Redis-opdrachtregelprogramma's](https://github.com/MSOpenTech/redis/releases/).
* Verbinding maken met behulp van de cache `redis-cli.exe`. Doorgeven in de cache-eindpunt met behulp die de -h schakelen en de sleutel met behulp van - a zoals wordt weergegeven in het volgende voorbeeld:
* `redis-cli -h <Azure Cache for Redis name>.redis.cache.windows.net -a <key>`

> [!NOTE]
> De Redis-opdrachtregelprogramma's werken niet met de SSL-poort, maar u kunt een hulpprogramma zoals `stunnel` veilig verbinden met de hulpprogramma's voor de SSL-poort door de aanwijzingen in de [aankondiging van ASP.NET-Sessiestatusprovider voor Redis-Preview Release](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbericht.
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Waarom niet Azure voor Redis-Cache hebt u een MSDN-klassebibliotheekverwijzing zoals enkele van de andere Azure-services?
Microsoft Azure Cache voor Redis is gebaseerd op de populaire open source Azure Cache voor Redis- en kan worden geopend door een groot aantal verschillende [Redis-clients](http://redis.io/clients) voor veel programmeertalen. Elke client heeft een eigen API waarmee aanroepen naar de Azure-Cache voor het gebruik van Redis-exemplaar [Redis-opdrachten](http://redis.io/commands).

Omdat elke client anders is, er is niet een gecentraliseerde klassenverwijzing op MSDN en elke client een eigen naslagdocumentatie onderhoudt. Naast de naslagdocumentatie zijn er verschillende zelfstudies waarin wordt beschreven hoe u aan de slag met Azure Cache voor Redis met behulp van verschillende talen en clients in de cache. Voor toegang tot deze zelfstudies, Zie [over het gebruik van Azure Cache voor Redis](cache-dotnet-how-to-use-azure-redis-cache.md) en klik op de gewenste taal van de wisselaar taal aan de bovenkant van het artikel.

### <a name="can-i-use-azure-cache-for-redis-as-a-php-session-cache"></a>Kan ik Azure Cache voor Redis gebruiken als de cache van een PHP-sessie?
Ja, voor het gebruik van Azure Cache voor Redis als de cache van een PHP-sessie, geef de verbindingsreeks naar uw Azure-Cache voor Redis-exemplaar in `session.save_path`.

> [!IMPORTANT]
> Wanneer u Azure-Cache voor Redis als de cache van een PHP-sessie, moet u URL coderen van de beveiligingssleutel waarmee verbinding wordt gemaakt met de cache, zoals wordt weergegeven in het volgende voorbeeld:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Als de sleutel geen URL gecodeerd is, ontvangt u mogelijk een uitzondering met een bericht als: `Failed to parse session.save_path`
>
>

Zie voor meer informatie over het gebruik van Azure Cache voor Redis als de cache van een PHP-sessie met de client PhpRedis [sessie PHP-handler](https://github.com/phpredis/phpredis#php-session-handler).

### <a name="what-are-redis-databases"></a>Wat zijn Redis-databases?

Redis-Databases zijn slechts een logische scheiding van gegevens binnen de dezelfde Redis-exemplaar. Het cachegeheugen wordt gedeeld tussen alle databases en de werkelijke hoeveelheid geheugen verbruik van een bepaalde database, is afhankelijk van de sleutels/waarden die zijn opgeslagen in de database. Bijvoorbeeld heeft een cache C6 53 GB geheugen. U kunt ervoor kiezen om alle 53 GB in een database of kunt u het splitsen tussen meerdere databases. 

> [!NOTE]
> Wanneer u een Premium Azure Cache voor Redis Clustering is ingeschakeld, wordt alleen database 0 beschikbaar is. Deze beperking is een ingebouwde Redis-beperking en is niet specifiek voor Azure Cache voor Redis. Zie voor meer informatie, [heb ik wijzigingen aanbrengen in mijn clienttoepassing voor het gebruik van clustering nodig?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Wanneer moet ik de niet-SSL-poort voor het verbinden met Redis inschakelen?
Redis-server biedt standaard geen ondersteuning voor SSL, maar biedt Azure Cache voor Redis. Als u verbinding met Azure-Cache voor Redis maakt en de client SSL, zoals StackExchange.Redis ondersteunt, moet u SSL gebruiken.

>[!NOTE]
>De poort zonder SSL-beveiliging is standaard uitgeschakeld voor nieuwe Azure-Cache voor instanties van Redis. Als de client biedt geen ondersteuning voor SSL, dan u de poort zonder SSL-beveiliging inschakelen moet door de aanwijzingen in de [toegang krijgen tot poorten](cache-configure.md#access-ports) sectie van de [een cache configureren in Azure Cache voor Redis](cache-configure.md) artikel.
>
>

Hulpprogramma's zoals redis `redis-cli` werken niet met de SSL-poort, maar u kunt een hulpprogramma zoals `stunnel` veilig verbinden met de hulpprogramma's voor de SSL-poort door de aanwijzingen in de [aankondiging van ASP.NET-Sessiestatusprovider voor Redis Preview-versie](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbericht.

Zie voor instructies over het downloaden van de Redis-hulpprogramma's, de [hoe kan ik Redis-opdrachten uitvoeren?](#cache-commands) sectie.

### <a name="what-are-some-production-best-practices"></a>Wat zijn enkele aanbevolen procedures voor productie?
* [Aanbevolen procedures van StackExchange.Redis](#stackexchangeredis-best-practices)
* [Configuratie en concepten](#configuration-and-concepts)
* [Prestaties testen](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Aanbevolen procedures van StackExchange.Redis
* Stel `AbortConnect` op false, laat de ConnectionMultiplexer automatisch opnieuw verbinding maken. [Hier ziet voor meer informatie](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* De ConnectionMultiplexer opnieuw gebruiken: Maak een nieuw wachtwoord voor elke aanvraag geen. De `Lazy<ConnectionMultiplexer>` patroon [hieronder](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) wordt aanbevolen.
* Werkt beste redis met lagere waarden, dus houd rekening met hakken grotere gegevens in meerdere sleutels. In [deze Redis-discussie](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb wordt beschouwd als grote. Lezen [in dit artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) voor een voorbeeld-probleem dat kan worden veroorzaakt door grote waarden.
* Configureer uw [ThreadPool instellingen](#important-details-about-threadpool-growth) om te voorkomen dat de time-outs.
* Gebruik ten minste de connectTimeout standaard 5 seconden. Dit interval krijgt StackExchange.Redis voldoende tijd om de verbinding, in het geval van een netwerk blip opnieuw te maken.
* Houd rekening met de prestatiekosten in verband met verschillende bewerkingen die u uitvoert. Bijvoorbeeld, de `KEYS` opdracht is een bewerking O(n) en moeten worden vermeden. De [redis.io site](http://redis.io/commands/) vindt u informatie over de complexiteit van de tijd voor elke bewerking die wordt ondersteund. Klik op elke opdracht om te zien van de complexiteit voor elke bewerking.

#### <a name="configuration-and-concepts"></a>Configuratie en concepten
* Gebruik Standard of Premium-laag voor productiesystemen. De laag Basic is een systeem met één knooppunt met geen replicatie van gegevens en er wordt geen SLA. Ook ten minste een C1-cache gebruiken. C0 caches worden meestal gebruikt voor eenvoudige dev/test-scenario's.
* Houd er rekening mee dat Redis is een **In-Memory** gegevensarchief. Lezen [in dit artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) zodat u zich bewust bent van scenario's waarbij gegevens verloren gaan kunnen.
* Ontwikkelen van uw systeem, zodat ze verbinding blips kan verwerken [vanwege het toepassen van patches en failover](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Prestaties testen
* Start met behulp van `redis-benchmark.exe` gecontroleerd op mogelijke doorvoer een idee krijgen voordat u uw eigen perf schrijft. Omdat `redis-benchmark` biedt geen ondersteuning voor SSL, moet u [de niet-SSL-poort via Azure portal in te schakelen](cache-configure.md#access-ports) voordat u de test uitvoert. Zie voor voorbeelden van [hoe kan ik benchmark-test uitvoeren en testen van de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* De VM die wordt gebruikt voor het testen van de client moet zich in dezelfde regio als uw Azure-Cache voor Redis-exemplaar.
* We adviseren Dv2-serie VM's voor de client gebruiken als ze beschikken over betere hardware en de beste resultaten geeft.
* Zorg ervoor dat de client virtuele machine die u kiest heeft ten minste net zoveel computing en bandbreedte mogelijkheid als de cache die u wilt testen.
* Schakel op de clientcomputer VRSS als u op Windows. [Hier ziet voor meer informatie](https://technet.microsoft.com/library/dn383582.aspx).
* Instanties van Redis voor Premium-laag zijn beter netwerk latentie en doorvoer omdat er op betere hardware voor zowel CPU en het netwerk worden uitgevoerd.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Wat zijn enkele overwegingen bij het gebruik van algemene Redis-opdrachten?
* U moet bepaalde Redis-opdrachten die erg lang duren om uit te voeren, zonder kennis van de impact van deze opdrachten niet uitvoeren.
  * Bijvoorbeeld, niet worden uitgevoerd de [sleutels](http://redis.io/commands/keys) opdracht in de productieomgeving, zoals het lang duren kan om terug te keren afhankelijk van het aantal sleutels. Redis is een server met één thread en opdrachten één bewerking tegelijk worden verwerkt. Als u andere opdrachten na sleutels hebt, wordt deze niet verwerkt totdat Redis de opdracht sleutels verwerkt. De [redis.io site](http://redis.io/commands/) vindt u informatie over de complexiteit van de tijd voor elke bewerking die wordt ondersteund. Klik op elke opdracht om te zien van de complexiteit voor elke bewerking.
* Belangrijkste formaat, moet ik gebruiken kleine sleutelwaarden of grote sleutelwaarden? In het algemeen is het afhankelijk is van het scenario. Als uw scenario grotere sleutels vereist, kunt u de ConnectionTimeout aanpassen en waarden opnieuw en pas uw logica voor opnieuw proberen. Lagere waarden worden vanuit het oogpunt server Redis waargenomen om betere prestaties.
* Deze overwegingen betekenen niet dat u kunt hogere waarden in Redis; opslaan u moet rekening houden met de volgende overwegingen. Latentie zal hoger zijn. Hebt u een set gegevens die groter is en één die kleiner is, kunt u meerdere exemplaren van ConnectionMultiplexer, elk met een andere set waarden voor time-outs en opnieuw hebt geconfigureerd zoals beschreven in de vorige [doen de StackExchange.Redis configuratie-opties doen](#cache-configuration) sectie.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Hoe kan ik benchmark-test uitvoeren en testen van de prestaties van mijn cache?
* [Schakel de diagnostische gegevens van de cache in](cache-how-to-monitor.md#enable-cache-diagnostics), zodat u de status van de cache kunt [bewaken](cache-how-to-monitor.md). U kunt de metrische gegevens weergeven in Azure Portal. U kunt ze ook [downloaden en bekijken](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) met een hulpprogramma van uw keuze.
* U kunt redis-benchmark.exe belastingtest uitvoeren op uw Redis-server.
* Zorg ervoor dat de belasting testen van client en de Azure-Cache voor Redis in dezelfde regio.
* Gebruik van redis-cli.exe en controleer de cache met behulp van de opdracht INFO.
* Als de belasting wordt veroorzaakt door hoge geheugenfragmentatie, moet u omhoog schalen naar een groter formaat in de cache.
* Zie voor instructies over het downloaden van de Redis-hulpprogramma's, de [hoe kan ik Redis-opdrachten uitvoeren?](#cache-commands) sectie.

De volgende opdrachten vindt u een voorbeeld van het gebruik van redis-benchmark.exe. Voor nauwkeurige resultaten, moet u deze opdrachten uitvoeren vanaf een virtuele machine in dezelfde regio als de cache.

* Test Pipelined SET-aanvragen met behulp van een 1 k-nettolading

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Test Pipelined GET-aanvragen met behulp van een 1 k-nettolading.
  Opmerking: De SET test uitvoeren om eerst in te vullen cache hierboven wordt weergegeven

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>Belangrijke informatie over de groei van de ThreadPool
De CLR-ThreadPool heeft twee soorten threads - "Werknemer" en "poort i/o-voltooiing' (ook wel Voltooiingspoort) threads.

* Werkthreads worden gebruikt voor bewerkingen, zoals de verwerking van de `Task.Run(…)`, of `ThreadPool.QueueUserWorkItem(…)` methoden. Deze threads worden ook gebruikt door verschillende onderdelen in de CLR wanneer werk moet worden uitgevoerd op een achtergrond-thread.
* Voltooiingspoort threads worden gebruikt als asynchrone i/o gebeurt (bijvoorbeeld lezen vanaf het netwerk).

De thread-groep biedt nieuwe werkthreads of threads voor i/o-voltooiing op aanvraag (zonder een beperking) totdat de instelling 'Minimale' voor elk type thread is bereikt. Het minimum aantal threads is standaard ingesteld op het aantal processors op een systeem.

Wanneer het aantal bestaande (bezet) threads de 'minimale' aantal threads raakt, wordt de snelheid waarmee deze nieuwe threads op één thread per 500 milliseconden injects vertraging in de ThreadPool. Normaal gesproken als uw systeem een ' burst ' van het werk dat u hoeft een thread Voltooiingspoort wordt, worden verwerkt die zeer snel. Echter, als de omvang van het werk is hoger dan de geconfigureerde instelling van de 'Minimale', zullen er enige vertraging bij het verwerken van deel van het werk als de ThreadPool gewacht op een van twee dingen gebeuren.

1. Een bestaande thread beschikbaar om het werk te verwerken.
2. Er zijn geen bestaande thread wordt gratis voor 500ms, zodat een nieuwe thread is gemaakt.

In feite, betekent dit dat als het aantal actieve threads groter dan de Min-threads is, waarschijnlijk het geval betaalt u een vertraging 500ms voordat het netwerkverkeer wordt verwerkt door de toepassing. Het is ook belangrijk te weten dat als een bestaande thread niet langer zijn dan 15 seconden (op basis van wat ik me herinneren) actief blijft, wordt deze worden opgeschoond en deze cyclus van de groei en inkrimping kunt herhalen.

Als we kijken naar het foutbericht van een voorbeeld van StackExchange.Redis (1.0.450 bouwen of hoger), ziet u deze nu worden afgedrukt ThreadPool statistieken (Zie de Voltooiingspoort- en WERKROLLEN informatie hieronder).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

In het vorige voorbeeld ziet u dat voor Voltooiingspoort thread 6 actieve threads worden en het systeem is geconfigureerd om toe te staan van 4 minimumaantal threads. In dit geval de client zou hebben waarschijnlijk gezien twee 500 ms vertragingen omdat 6 > 4.

Houd er rekening mee dat StackExchange.Redis time-outs bereiken kunnen als groei van de Voltooiingspoort of WORKER threads wordt beperkt.

### <a name="recommendation"></a>Aanbeveling
Deze informatie verstrekt, wordt aangeraden dat klanten de minimale configuratiewaarde voor Voltooiingspoort en WORKER threads ingesteld op iets groter is dan de standaardwaarde. We kan geen niet-gedifferentieerde richtlijnen op deze waarde moet omdat de juiste waarde voor één toepassing te hoog/laag voor een andere toepassing is op te geven. Deze instelling kan ook invloed op de prestaties van andere onderdelen van complexe toepassingen, zodat elke klant nodig heeft voor het afstemmen van deze instelling op hun specifieke behoeften. Een goed startpunt is 200 of 300, en vervolgens testen en indien nodig aanpassen.

Hoe u deze instelling wilt configureren:

* In ASP.NET, gebruikt u de ["minIoThreads" of "minWorkerThreads" configuratie-instelling] [ "minIoThreads" configuration setting] onder de `<processModel>` configuratie-element in web.config. Als u in Azure WebSites uitvoert, wordt deze instelling niet beschikbaar via de configuratieopties. Echter nog steeds moet u kunnen deze instelling via een programma niet te configureren (Zie hieronder) van uw methode Application_Start in global.asax.cs.

  > [!NOTE] 
  > De waarde die is opgegeven in deze configuratie-element is een *per kern* instelling. Bijvoorbeeld, als u wilt dat uw instelling minIOThreads 200 tijdens runtime en een 4-core-machine hebt, gebruikt u `<processModel minIoThreads="50"/>`.
  >

* Buiten ASP.NET en Azure WebSites global.asax, gebruikt u de [ThreadPool.SetMinThreads (...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) DE API.

  > [!NOTE]
  > De waarde gespecificeerd door deze API is een algemene instelling, die betrekking hebben op het hele AppDomain. Als u een 4-core-machine hebt en wilt minWorkerThreads en minIOThreads ingesteld op 50 per CPU tijdens runtime, zou u ThreadPool.SetMinThreads (200, 200).

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Inschakelen van de server GC meer om doorvoer te krijgen op de client bij het gebruik van StackExchange.Redis
Inschakelen van server GC kunt optimaliseren van de client en bieden een betere prestaties en doorvoer bij het gebruik van StackExchange.Redis. Zie de volgende artikelen voor meer informatie over de GC-server en het inschakelen ervan:

* [GC server inschakelen](https://msdn.microsoft.com/library/ms229357.aspx)
* [Grondbeginselen van garbagecollection](https://msdn.microsoft.com/library/ee787088.aspx)
* [Garbagecollection en prestaties](https://msdn.microsoft.com/library/ee851764.aspx)


### <a name="performance-considerations-around-connections"></a>Prestatie-overwegingen over verbindingen

Elke prijscategorie heeft verschillende beperkingen voor clientverbindingen, het geheugen en bandbreedte. Kunt u elke grootte van cache *tot* een bepaald aantal verbindingen, elke verbinding met Redis is overhead die ermee verbonden zijn. Een voorbeeld van dergelijke overhead zou CPU- en geheugengebruik als gevolg van TLS/SSL-codering zijn. De limiet voor maximum aantal verbindingen voor een opgegeven cachegrootte wordt ervan uitgegaan dat een licht geladen cache. Als laden uit verbinding overhead *plus* laden uit clientbewerkingen groter is dan de capaciteit voor het systeem, de cache kan consistentieproblemen capaciteit, zelfs als u hebt de limiet voor verbindingen voor de huidige cachegrootte niet overschreden.

Zie voor meer informatie over de limieten voor verschillende verbindingen voor elke laag [Azure Cache voor Redis-prijsstelling](https://azure.microsoft.com/pricing/details/cache/). Zie voor meer informatie over verbindingen en andere standaardconfiguraties [serverconfiguratie standaard Redis](cache-configure.md#default-redis-server-configuration).

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Hoe controleer ik de status en prestaties van mijn cache?
Microsoft Azure Cache voor instanties van Redis kan worden gecontroleerd in de [Azure-portal](https://portal.azure.com). U kunt metrische gegevens weergeven, grafieken met metrische gegevens aan het Startboard vastmaken, aanpassen van het bereik van datum en tijd van de bewaking van grafieken, toevoegen en metrische gegevens verwijderen uit de grafieken en waarschuwingen instellen wanneer aan bepaalde voorwaarden wordt voldaan. Zie voor meer informatie, [Monitor Azure Cache voor Redis](cache-how-to-monitor.md).

De Azure-Cache voor Redis **resourcemenu** bevat ook verschillende hulpprogramma's voor bewaking en probleemoplossing van uw caches.

* **Problemen vaststellen en oplossen** bevat informatie over veelvoorkomende problemen en strategieën voor het oplossen ervan.
* **Resourcestatus** wordt de resource gecontroleerd en wordt aangegeven of deze wordt uitgevoerd zoals verwacht. Zie voor meer informatie over de Azure Resource health-service, [overzicht van Azure Resource health](../resource-health/resource-health-overview.md).
* **Nieuwe ondersteuningsaanvraag** biedt opties om een ondersteuningsaanvraag voor uw cache te openen.

Deze hulpprogramma's kunnen u de status van uw Azure-Cache voor instanties van Redis en hulp bij het beheren van uw cache in toepassingen worden gecontroleerd. Voor meer informatie, Zie de sectie "Ondersteuning & instellingen voor het oplossen van problemen" van [over het configureren van Azure Cache voor Redis](cache-configure.md).

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Waarom krijg ik time-outs zien?
Time-outs optreden in de client die u gebruiken om te communiceren met Redis. Wanneer een opdracht naar de Redis-server wordt verzonden, wordt de opdracht is in de wachtrij en uiteindelijk neemt het de opdracht en voert dit Redis-server. Maar de client kunt time-out tijdens dit proces en als dit het geval is een uitzondering wordt gegeven aan de aanroepende. Zie voor meer informatie over het oplossen van time-outproblemen [Client-side probleemoplossing](cache-how-to-troubleshoot.md#client-side-troubleshooting) en [StackExchange.Redis time-outuitzonderingen](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Waarom is mijn klant losgekoppeld van de cache?
Hier volgen enkele veelvoorkomende reden voor een cache-verbinding verbreken.

* Client-side oorzaken
  * De clienttoepassing is opnieuw geïmplementeerd.
  * De clienttoepassing een vergroten/verkleinen bewerking uitgevoerd.
    * In het geval van Cloud Services of Web-Apps, kan dit zijn vanwege automatisch schalen.
  * De netwerklaag op de client is gewijzigd.
  * Tijdelijke fouten zijn opgetreden in de client of in de netwerkknooppunten tussen de client en de server.
  * De drempelwaarde voor bandbreedtelimieten zijn bereikt.
  * CPU-gebonden operations duurt te lang om te voltooien.
* Serverzijde oorzaken
  * De Azure-Cache voor Redis-service gestart op de standard-cache-aanbieding, een failover van het primaire knooppunt naar het secundaire knooppunt.
  * Azure is het exemplaar waarop de cache is geïmplementeerd patches
    * Dit kan zijn voor updates voor Redis-server of algemene VM-onderhoud.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Welke Azure Cache-aanbieding is juist voor mij?
> [!IMPORTANT]
> Aan de hand van vorig jaar [aankondiging](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), Azure Managed Cache Service en Azure In-Role Cache service **buiten gebruik gesteld** op 30 November 2016. Onze aanbeveling is het gebruik van [Azure Cache voor Redis](https://azure.microsoft.com/services/cache/). Zie voor meer informatie over het migreren van [migreren vanuit Managed Cache Service met Azure-Cache voor Redis](cache-migrate-to-redis.md).
>
>

### <a name="azure-cache-for-redis"></a>Azure Cache voor Redis
Azure Redis-Cache is algemeen beschikbaar in groottes tot 53 GB en is voor een beschikbaarheids-SLA van 99,9%. De nieuwe [premium-laag](cache-premium-tier-intro.md) biedt groottes tot 530 GB en ondersteuning voor clustering, VNET en persistentie, met een SLA van 99,9%.

Azure Redis-Cache geeft klanten de mogelijkheid om te gebruiken van een beveiligde, toegewezen Cache van Azure voor Redis, beheerd door Microsoft. Met deze aanbieding krijgt u gebruikmaken van de uitgebreide reeks functies en het ecosysteem geleverd door Redis en betrouwbare hosting en controle van Microsoft.

Redis is in tegenstelling tot traditionele caches die omgaan met sleutel / waarde-paren, vaak gebruikt voor de zeer goed presterende gegevenstypen. Redis ook ondersteunt atomische bewerkingen op deze typen, zoals het toevoegen van een tekenreeks; de waarde in een hash; verhogen pushen naar een lijst. set intersection, union en verschil; of voor het verkrijgen van het lid met de hoogste classificatie in een gesorteerde set. Andere functies bieden ondersteuning voor transacties, pub/sub, Lua-scripting, sleutels met beperkte time-to-live, en configuratie-instellingen waardoor Redis zich gedraagt meer, zoals een traditionele cache.

Een ander belangrijk aspect voor Redis succes is het gebaseerd op deze in orde zijn, levendige open-source-ecosysteem. Dit wordt weergegeven in de diverse set met Redis-clients is beschikbaar in meerdere talen. Dit ecosysteem en een breed scala aan clients kunnen Azure-Cache voor Redis moet worden gebruikt bij bijna elke werklast die u in Azure wilt bouwen.

Zie voor meer informatie over aan de slag met Azure Cache voor Redis [hoe u met Azure Cache voor Redis](cache-dotnet-how-to-use-azure-redis-cache.md) en [Azure Cache voor Redis-documentatie](index.md).

### <a name="managed-cache-service"></a>Beheerde cacheservice
[Managed Cache service en met 30 November 2016 buiten gebruik is gesteld.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Als u gearchiveerde documentatie, Zie [gearchiveerde Managed Cache servicedocumentatie](https://msdn.microsoft.com/library/azure/dn386094.aspx).

### <a name="in-role-cache"></a>In-Role Cache
[In-Role Cache en met 30 November 2016 buiten gebruik is gesteld.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Als u gearchiveerde documentatie, Zie [gearchiveerde In-Role Cache documentatie](https://msdn.microsoft.com/library/azure/dn386103.aspx).

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
