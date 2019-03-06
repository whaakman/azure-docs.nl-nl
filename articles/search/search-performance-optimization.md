---
title: Implementatiestrategieën en aanbevolen procedures voor het optimaliseren van prestaties - Azure Search
description: Meer informatie over technieken en aanbevolen procedures voor het afstemmen van prestaties van Azure Search en optimale schaal configureren.
author: LiamCavanagh
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/02/2019
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 8a07657d04cee57cb69c9f5f7862fed3e7965716
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404464"
---
# <a name="deployment-strategies-and-best-practices-for-optimizing-performance-on-azure-search"></a>Implementatiestrategieën en aanbevolen procedures voor het optimaliseren van prestaties van Azure Search

Dit artikel wordt beschreven aanbevolen procedures voor geavanceerde scenario's met geavanceerde vereisten voor schaalbaarheid en beschikbaarheid. 

## <a name="develop-baseline-numbers"></a>Basislijn getallen ontwikkelen
Bij het optimaliseren voor prestaties, moet u zich richten op het moment dat de uitvoering van query verminderen. Om dit te doen, moet u weten wat een typische querybelasting ziet eruit als. De volgende richtlijnen kunt u basislijn query getallen aankomen.

1. Kies met een doellatentie (of maximale hoeveelheid tijd) die een standaardzoekopdrachten aanvragen uitvoeren moet om te voltooien.
2. Maken en testen van een echte werkbelasting op basis van uw search-service met een realistische gegevensset voor het meten van de latentie-tarieven.
3. Beginnen met een klein aantal query's per seconde (QPS) en het nummer in de test wordt uitgevoerd totdat de latentie van query lager is dan de latentie gedefinieerd doel, geleidelijk te verhogen. Dit is een belangrijk benchmark om te plannen voor schaal wanneer uw toepassing in gebruik groeit.
4. Waar mogelijk, opnieuw gebruiken de HTTP-verbindingen. Als u van de Azure Search .NET SDK gebruikmaakt, betekent dit dat u opnieuw een exemplaar moet worden gebruikt of [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) exemplaar, en als u de REST-API gebruikt, moet u een enkele HttpClient hergebruiken.
5. De inhoud van de query-aanvragen verschillen, zodat die zoekopdracht vindt plaats via verschillende onderdelen van uw index. Verandering is belangrijk omdat als u de dezelfde zoekaanvragen continu worden uitgevoerd, in cache opslaan van gegevens wordt gestart om de prestaties beter dan deze mogelijk met een meer uiteenlopende query ingesteld.
6. De structuur van queryaanvragen variëren zodat u verschillende typen query's krijgen. Niet elke zoekopdracht wordt uitgevoerd op hetzelfde niveau. Bijvoorbeeld, is een document lookup- of suggestie doorgaans sneller dan een query met een groot aantal facetten en filters. Samenstelling van de test moet verschillende query's, opnemen in ongeveer de dezelfde verhoudingen zoals u in de productieomgeving verwachten zou.  

Testen tijdens het maken van deze workloads, zijn er enkele kenmerken van Azure Search moet rekening houden met:

+ Het is mogelijk overbelasting van uw service door te veel zoekquery's in één keer te pushen. Als dit gebeurt, ziet u HTTP 503-responscodes. Om te voorkomen dat een 503 tijdens het testen, start u met verschillende bereiken van de search-aanvragen om te zien van de verschillen in latentie tarieven als u meer zoekaanvragen toevoegt.

+ Azure Search indexeren taken niet op de achtergrond uitgevoerd. Als uw service worden verwerkt voor query's en workloads gelijktijdig indexering, rekening dit door de introductie van indexeringstaken in uw querytests, of door de opties voor het uitvoeren van indexeertaken laten tijdens drukke uren.

> [!NOTE]
> [Belastingstests van Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) is een goede manier om uit te voeren van uw benchmark test aangezien u HTTP-aanvragen uitvoeren kunt als nodig zou zijn voor het uitvoeren van query's voor Azure Search en kan parallellisering van aanvragen.
> 
> 

## <a name="scaling-for-high-query-volume-and-throttled-requests"></a>Schalen voor met hoog queryvolume en beperkte aanvragen
Wanneer u te veel beperkte aanvragen worden ontvangen of groter zijn dan de tarieven van de latentie doel van een hogere querybelasting, kunt u bekijken om te verminderen latentie tarieven op twee manieren:

1. **Verhoging van de replica's:**  Een replica is vergelijkbaar met een kopie van uw gegevens toestaan van aanvragen verdelen over op basis van meerdere exemplaren van Azure Search.  Alle load balancing en replicatie van gegevens tussen de replica's wordt beheerd door Azure Search en kunt u het aantal replica's die zijn toegewezen voor uw service op elk gewenst moment wijzigen.  U kunt maximaal 12 replica's in een Standard-zoekservice en 3 replica's in een Basic-zoekservice toewijzen. Replica's kunnen worden aangepast via de [Azure-portal](search-create-service-portal.md) of [PowerShell](search-manage-powershell.md).
2. **Search Tier verhogen:**  Azure Search is beschikbaar in een [aantal lagen](https://azure.microsoft.com/pricing/details/search/) en elk van deze lagen biedt verschillende prestatieniveaus.  In sommige gevallen mogelijk veel query's dat de laag die u voldoende lage latentie-tarieven, niet kan bieden, zelfs wanneer de replica's volledig worden benut uit. In dit geval kunt u overwegen gebruik te maken van een van de hogere lagen zoeken, zoals de Azure Search S3-laag is geschikt voor scenario's met grote aantallen documenten en zeer hoge querywerkbelastingen.

## <a name="scaling-for-slow-individual-queries"></a>Schaalbaarheid voor trage afzonderlijke query 's
Een andere reden voor de hoge latentie is één query duurt te lang om te voltooien. In dit geval kunnen toevoegen van replica's niet. Twee opties mogelijke opties die kunnen helpen bij de volgende:

1. **Partities verhogen** een partitie is een mechanisme voor het splitsen van uw gegevens voor extra resources. Gegevens toevoegen van een tweede partitie worden gesplitst in twee, een derde partitie wordt dit gesplitst in drie, enzovoort. Een positief neveneffect is dat langzamer query's soms sneller vanwege parallelle berekeningen voeren. We hebben parallellisering op query's met lage selectiviteit, zoals query's die overeenkomen met veel documenten of facetten telt het aantal leveren via een groot aantal documenten die u hebt genoteerd. Sinds de aanzienlijke berekening is vereist om de relevantie van de documenten te beoordelen of te tellen van het aantal documenten, het toevoegen van extra partities kan query's sneller zijn voltooid.  
   
   Er zijn maximaal 12 partities in de Standard-zoekservice en 1 partitie in de basic search-service.  Partities kunnen worden aangepast via de [Azure-portal](search-create-service-portal.md) of [PowerShell](search-manage-powershell.md).

2. **Limiet hoge kardinaliteit velden:** Een hoge kardinaliteit veld bestaat uit een geschikt voor facetten of Filterbaar veld dat heeft een groot aantal unieke waarden, en als gevolg hiervan aanzienlijke resources verbruikt bij het berekenen van resultaten. Bijvoorbeeld, wordt instellen van een Product-ID of beschrijving van veld als geschikt voor facetten/Filterbaar gerekend als een hoge kardinaliteit omdat de meeste van de waarden uit het document naar document uniek zijn. Waar mogelijk, beperkt u het aantal hoge kardinaliteit velden.

3. **Search Tier verhogen:**  Tot het verplaatsen van kan een hogere laag van Azure Search een andere manier om de prestaties van trage query's zijn. Elke hogere laag biedt snellere CPU's en meer geheugen, die beide een positieve invloed op prestaties van query's hebben.

## <a name="scaling-for-availability"></a>Schaalbaarheid voor beschikbaarheid
Replica's niet alleen te verminderen latentie van query, maar kunnen ook de mogelijkheid om hoge beschikbaarheid. Met één replica, u kunt verwachten periodieke uitvaltijd vanwege de server opnieuw wordt opgestart nadat de software-updates of voor andere onderhoudsgebeurtenissen die wordt uitgevoerd.  Als gevolg hiervan is het belangrijk om te overwegen als uw toepassing is vereist voor hoge beschikbaarheid van zoekopdrachten (query's) en schrijfbewerkingen (indexering gebeurtenissen). Azure Search biedt een SLA-opties op de zoeken in betaalde aanbiedingen met de volgende kenmerken:

* 2 replica's voor hoge beschikbaarheid van workloads voor alleen-lezen (query's)
* 3 of meer replica's voor hoge beschikbaarheid van lezen / schrijven-workloads (query's en indexeren)

Ga voor meer informatie over dit naar de [Azure Search Service Level Agreement](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Omdat de replica's zijn kopieën van uw gegevens, kan met meerdere replica's Azure Search opnieuw wordt opgestart en onderhoud op basis van een van de replica uitvoeren voor machine terwijl de uitvoering van de query om door te gaan op andere replica's. Daarentegen als u replica's opgeslagen, u zult in rekening worden gebracht query prestatievermindering, ervan uitgaande dat deze replica's zijn een resource benut.

## <a name="scaling-for-geo-distributed-workloads-and-geo-redundancy"></a>Schalen voor geografisch gedistribueerde werkbelastingen en geografische redundantie
Gebruikers die zich ver van het datacenter die als host fungeert voor Azure Search ligt hebben hogere latentie tarieven voor geografisch gedistribueerde werkbelastingen. Een beperking is voor het inrichten van meerdere search-services in de regio's met dichter bij de aan deze gebruikers. Azure Search biedt momenteel geen een geautomatiseerde methode om Azure Search-index geo-replicatie tussen regio's, maar er zijn bepaalde technieken die kunnen worden gebruikt om dit proces eenvoudig te implementeren en beheren. Deze worden beschreven in de volgende gedeelten.

Het doel van een geografisch gedistribueerde set search-services is dat twee of meer indexen beschikbaar in twee of meer regio's waar een gebruiker wordt doorgestuurd naar de Azure Search-service met de laagste latentie zoals te zien is in dit voorbeeld:

   ![Cross-tabblad van services per regio][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Gegevens synchroon houden over meerdere Azure Search-services
Er zijn twee opties voor het beveiligen van uw gedistribueerde zoekservices gesynchroniseerd die bestaan uit het mogelijk maken via de [Azure Search-indexeerfunctie](search-indexer-overview.md) of de Push-API (ook wel de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="use-indexers-for-updating-content-on-multiple-services"></a>Indexeerfuncties gebruiken voor het bijwerken van inhoud op meerdere services

Als u al indexeerfunctie op één service gebruikt, kunt u een tweede indexeerfunctie configureren op een tweede service voor het gebruik van dezelfde gegevensbronobject binnenhalen van gegevens vanaf dezelfde locatie is. Elke service in elke regio heeft een eigen indexeerfunctie en een doelindex (uw zoekverzameling wordt niet gedeeld, wat betekent dat gegevens worden gedupliceerd), maar elke indexeerfunctie verwijst naar dezelfde gegevensbron.

Hier volgt een visual op hoog niveau van wat die architectuur eruit zou zien.

   ![Één gegevensbron met gedistribueerde indexeerfunctie en combinaties van service][2]

### <a name="use-rest-apis-for-pushing-content-updates-on-multiple-services"></a>REST-API's gebruiken voor het pushen van updates van inhoud op meerdere services
Als u de Azure Search REST-API voor [inhoud in uw Azure Search-index pushen](https://docs.microsoft.com/rest/api/searchservice/update-index), kunt u uw verschillende zoekservices gesynchroniseerd door wijzigingen aan alle zoekservices pushen wanneer een update vereist is. In uw code ervoor zorgen om af te handelen wanneer een update aan voor een search-service is mislukt, maar andere zoekservices is mislukt.

## <a name="leverage-azure-traffic-manager"></a>Maak gebruik van Azure Traffic Manager
[Met Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) kunt u routeren van aanvragen naar meerdere geo zich websites die vervolgens worden ondersteund door meerdere Azure Search-Services. Eén voordeel van het Traffic Manager is dat het Azure Search om ervoor te zorgen dat deze beschikbaar is en gebruikers te routeren naar alternatieve search-services in het geval van downtime kunt testen. Bovendien, als u zoekaanvragen via Azure Web Sites zijn routering, Azure Traffic Manager kunt u saldo gevallen waarin de Website is geladen, maar niet Azure Search. Hier volgt een voorbeeld van wat de architectuur die gebruikmaakt van Traffic Manager.

   ![Cross-tabblad van services per regio, met centrale Traffic Manager][3]

## <a name="monitor-performance"></a>Prestaties bewaken
Azure Search biedt de mogelijkheid om te analyseren en controleren van de prestaties van uw service via [zoekverkeer](search-traffic-analytics.md). Wanneer u deze functie inschakelen en instrumentatie kunt aan uw client-app toevoegen, kunt u eventueel de afzonderlijke bewerkingen, evenals de samengevoegde metrische gegevens zich op een Azure Storage-account dat vervolgens kan worden verwerkt voor analyse of gevisualiseerd in Power BI. Metrische gegevens van schermopnamen worden op deze manier bieden prestatiestatistieken, zoals het gemiddelde aantal query's of de reactietijden van de query. Bovendien kunt de logboekregistratie van bewerkingen u inzoomen op gegevens van specifieke zoekopdrachten.

Traffic analytics is handig om te begrijpen latentie tarief met ingang van die Azure Search-perspectief. Omdat de prestatiegegevens van de query geregistreerd zijn gebaseerd op de tijd voor een query volledig in Azure Search worden verwerkt (vanaf het moment dat deze wordt aangevraagd wanneer deze wordt verzonden), zijn u kunt dit gebruiken om te bepalen of latentieproblemen met van de kant van Azure Search-service of time-outs IDE van de service, zoals netwerklatentie.  

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de prijzen-lagen en services beperkingen voor elk adres, [Servicelimieten in Azure Search](search-limits-quotas-capacity.md).

Ga naar [capaciteitsplanning](search-capacity-planning.md) voor meer informatie over combinaties van partitie en de replica.

Bekijk de volgende video voor meer Inzoomen op de prestaties en ziet u enkele demonstraties van het implementeren van de optimalisaties die in dit artikel worden besproken:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
