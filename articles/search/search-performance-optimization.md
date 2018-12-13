---
title: Azure Search prestaties en optimalisatie van overwegingen - Azure Search
description: Meer informatie over technieken en aanbevolen procedures voor het afstemmen van prestaties van Azure Search en optimale schaal configureren.
author: LiamCavanagh
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/01/2017
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 0a98e7f05e766d47a5ea9293409a74a6fafbf837
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310212"
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Azure Search-overwegingen voor prestaties en optimalisatie
Een geweldige zoekervaring is een essentieel voor succes voor veel mobiele en webtoepassingen. Van onroerend goed, auto-marktplaatsen in de aan de online catalogus, gebruikt snel zoeken en relevante resultaten heeft invloed op de ervaring van de klant. Dit document is bedoeld om hulp ontdekt u aanbevolen procedures voor hoe u optimaal gebruikmaken van Azure Search, met name voor geavanceerde scenario's met geavanceerde vereisten voor schaalbaarheid, meertalige ondersteuning of aangepaste positie.  Bovendien, in dit document bevat een overzicht van interne werking van en bevat informatie over de methoden die effectief in echte klantscenario apps werken.

## <a name="performance-and-scale-tuning-for-search-services"></a>Prestaties en schaal afstemmen voor Search-services
We worden gebruikt om te zoeken naar engines, zoals Bing en Google en de hoge prestaties die ze bieden.  Wanneer klanten uw zoekopdrachten ingeschakelde web- of mobiele toepassing, worden ze als gevolg hiervan vergelijkbare prestatiekenmerken verwachten.  Wanneer optimaliseren voor prestaties, wordt een van de beste aanpak is om zich te richten voor latentie, dit de tijd die nodig is een query is te voltooien en de resultaten worden geretourneerd.  Wanneer is het belangrijk om te optimaliseren voor zoeklatentie ervan:

1. Kies met een doellatentie (of maximale hoeveelheid tijd) die een standaardzoekopdrachten aanvragen uitvoeren moet om te voltooien.
2. Maken en testen van een echte werkbelasting op basis van uw search-service met een realistische gegevensset voor het meten van de latentie-tarieven.
3. Beginnen met een klein aantal query's per seconde (QPS) en doorgaan met het nummer in de test wordt uitgevoerd totdat de latentie van query lager is dan de latentie gedefinieerd doel te verhogen.  Dit is een belangrijk benchmark om te plannen voor schaal wanneer uw toepassing in gebruik groeit.
4. Waar mogelijk, opnieuw gebruiken de HTTP-verbindingen.  Als u van de Azure Search .NET SDK gebruikmaakt, betekent dit dat u opnieuw een exemplaar moet worden gebruikt of [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) exemplaar, en als u de REST-API gebruikt, moet u een enkele HttpClient hergebruiken.

Testen tijdens het maken van deze workloads, zijn er enkele kenmerken van Azure Search moet rekening houden met:

1. Het is mogelijk om dit te pushen zo veel zoeken query in één keer dat de beschikbare resources in uw Azure Search-service wordt overbelast raken.  Als dit gebeurt, ziet u HTTP 503-responscodes.  Daarom is het raadzaam te beginnen met verschillende bereiken van de search-aanvragen om te zien van de verschillen in latentie tarieven als u meer zoekaanvragen toevoegt.
2. Uploaden van inhoud naar Azure Search is van invloed op de algehele prestaties en latentie van de Azure Search-service.  Als u verwacht te verzenden van gegevens, terwijl gebruikers zoekopdrachten uitvoeren, is het belangrijk dat u deze workload in aanmerking nemen in uw tests.
3. Niet elke zoekopdracht wordt uitgevoerd op de dezelfde prestatieniveaus.  Bijvoorbeeld, wordt een document lookup- of suggestie wordt doorgaans sneller dan een query met een groot aantal facetten en filters uitvoeren.  Het is raadzaam de verschillende query's die u verwacht te zien in aanmerking bij het bouwen van uw tests uitvoeren.  
4. Variatie van zoekaanvragen is belangrijk omdat als u de dezelfde zoekaanvragen continu worden uitgevoerd, in cache opslaan van gegevens wordt gestart om de prestaties beter dan deze mogelijk met een meer uiteenlopende query ingesteld.

> [!NOTE]
> [Belastingstests van Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) is een goede manier om uit te voeren van uw benchmark test aangezien u HTTP-aanvragen uitvoeren kunt als nodig zou zijn voor het uitvoeren van query's voor Azure Search en kan parallellisering van aanvragen.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Schalen van Azure Search voor hoge query tarieven en aanvragen beperkt
Wanneer u te veel beperkte aanvragen worden ontvangen of groter zijn dan de tarieven van de latentie doel van een hogere querybelasting, kunt u bekijken om te verminderen latentie tarieven op twee manieren:

1. **Verhoging van de replica's:**  Een replica is vergelijkbaar met een kopie van uw gegevens toestaan van aanvragen verdelen over op basis van meerdere exemplaren van Azure Search.  Alle load balancing en replicatie van gegevens tussen de replica's wordt beheerd door Azure Search en kunt u het aantal replica's die zijn toegewezen voor uw service op elk gewenst moment wijzigen.  U kunt maximaal 12 replica's in een Standard-zoekservice en 3 replica's in een Basic-zoekservice toewijzen. Replica's kunnen worden aangepast via de [Azure-portal](search-create-service-portal.md) of [PowerShell](search-manage-powershell.md).
2. **Search Tier verhogen:**  Azure Search is beschikbaar in een [aantal lagen](https://azure.microsoft.com/pricing/details/search/) en elk van deze lagen biedt verschillende prestatieniveaus.  In sommige gevallen mogelijk veel query's dat de laag die u voldoende lage latentie-tarieven, niet kan bieden, zelfs wanneer de replica's volledig worden benut uit.  In dit geval kunt u overwegen gebruik te maken van een van de hogere lagen zoeken, zoals de Azure Search S3-laag die zeer geschikt voor scenario's met grote aantallen documenten en zeer hoge querywerkbelastingen.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Schalen van Azure Search traag afzonderlijke query 's
Er is een andere reden waarom latentie tarieven kunnen traag zijn van één query duurt te lang om te voltooien.  Toevoegen van replica's wordt in dit geval niet verbeterd latentie tarieven.  In dit geval zijn twee opties beschikbaar:

1. **Partities verhogen** een partitie is een mechanisme voor het splitsen van uw gegevens voor extra resources.  Om deze reden dan ook, wanneer u een tweede partitie toevoegen opgehaald uw gegevens splitsen in twee kolommen.  Een derde partitie splitst uw index in drie, enzovoort.  Dit heeft ook het effect dat in sommige gevallen trage query's sneller vanwege de parallelle uitvoering van de berekening uitvoeren zal.  Er zijn enkele voorbeelden van waar we deze parallellisering zeer goed werken met query's met lage selectiviteit query's hebben gezien.  Dit bestaat uit de query's die overeenkomen met veel documenten of wanneer op meerdere niveaus moet bieden voor aantallen grote aantallen documenten.  Omdat er veel van de berekening die nodig zijn om de relevantie van de documenten te beoordelen of de getallen van documenten te tellen, kan extra partities toe te voegen voor extra berekening helpen.  
   
   Er zijn maximaal 12 partities in de Standard-zoekservice en 1 partitie in de basic search-service.  Partities kunnen worden aangepast via de [Azure-portal](search-create-service-portal.md) of [PowerShell](search-manage-powershell.md).
2. **Limiet hoge kardinaliteit velden:** Een hoge kardinaliteit veld bestaat uit een geschikt voor facetten of Filterbaar veld dat is een groot aantal unieke waarden, en als gevolg hiervan wordt een groot aantal bronnen voor het berekenen van resultaten op.   Bijvoorbeeld, zou het instellen van een Product-ID of beschrijving van veld als geschikt voor facetten/Filterbaar maken voor hoge kardinaliteit omdat de meeste van de waarden uit het document naar document uniek zijn. Waar mogelijk, beperkt u het aantal hoge kardinaliteit velden.
3. **Search Tier verhogen:**  Tot het verplaatsen van kan een hogere laag van Azure Search een andere manier om de prestaties van trage query's zijn.  Elke hogere laag biedt snellere CPU's en meer geheugen dat een positieve invloed op prestaties van query's hebben kan.

## <a name="scaling-for-availability"></a>Schaalbaarheid voor beschikbaarheid
Replica's niet alleen te verminderen latentie van query, maar kunnen ook de mogelijkheid om hoge beschikbaarheid.  Met één replica, u kunt verwachten periodieke uitvaltijd vanwege de server opnieuw wordt opgestart nadat de software-updates of voor andere onderhoudsgebeurtenissen die wordt uitgevoerd.  Als gevolg hiervan is het belangrijk om te overwegen als uw toepassing is vereist voor hoge beschikbaarheid van zoekopdrachten (query's) en schrijfbewerkingen (indexering gebeurtenissen).  Azure Search biedt een SLA-opties op de zoeken in betaalde aanbiedingen met de volgende kenmerken:

* 2 replica's voor hoge beschikbaarheid van workloads voor alleen-lezen (query's)
* 3 of meer replica's voor hoge beschikbaarheid van lezen / schrijven-workloads (query's en indexeren)

Ga voor meer informatie over dit naar de [Azure Search Service Level Agreement](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Omdat de replica's zijn kopieën van uw gegevens, kan met meerdere replica's Azure Search te van de computer opnieuw wordt opgestart en onderhoud op basis van een van de replica op een tijdstip terwijl query's om door te gaan om te worden uitgevoerd op de andere replica's.  Om die reden moet u ook rekening houden met hoe deze downtime mogelijk van invloed op de query's die moeten worden uitgevoerd op een minder kopie van de gegevens.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Geografisch gedistribueerde werkbelastingen schalen en geo-redundantie bieden
Voor geografisch gedistribueerde werkbelastingen vindt u dat gebruikers die zich ver van het datacenter waar uw Azure Search-service wordt gehost ligt hogere latentie-tarieven hebben.  Daarom is het vaak belangrijk dat u hebt meerdere search-services in regio's die zich in dichter bij de aan deze gebruikers.  Azure Search biedt momenteel geen een geautomatiseerde methode om Azure Search-index geo-replicatie tussen regio's, maar er zijn bepaalde technieken die kunnen worden gebruikt om dit proces eenvoudig te implementeren en beheren. Deze worden beschreven in de volgende gedeelten.

Het doel van een geografisch gedistribueerde set search-services is dat twee of meer indexen beschikbaar in twee of meer regio's waar een gebruiker worden doorgestuurd naar de Azure Search-service met de laagste latentie zoals te zien is in dit voorbeeld:

   ![Cross-tabblad van services per regio][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Gegevens synchroon houden over meerdere Azure Search-services
Er zijn twee opties voor het beveiligen van uw gedistribueerde zoekservices gesynchroniseerd die bestaan uit het mogelijk maken via de [Azure Search-indexeerfunctie](search-indexer-overview.md) of de Push-API (ook wel de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Azure Search-indexeerfuncties
Als u van de Azure Search-indexeerfunctie gebruikmaakt, importeert u al gegevens worden gewijzigd vanuit een centrale gegevensopslag zoals Azure SQL DB of Azure Cosmos DB. Als u een nieuwe zoekopdracht-Service, maakt u gewoon ook maken een nieuwe Azure Search-indexeerfunctie voor die service die naar deze dezelfde gegevensopslag verwijst. Op die manier kunnen wanneer nieuwe wijzigingen in het gegevensarchief komen worden ze vervolgens geïndexeerd door de verschillende indexeerfuncties.  

Hier volgt een voorbeeld van hoe die architectuur eruit zou zien.

   ![Één gegevensbron met gedistribueerde indexeerfunctie en combinaties van service][2]

### <a name="push-api"></a>Push-API
Als u de Azure Search Push API naar [inhoud in uw Azure Search-index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index), kunt u uw verschillende zoekservices gesynchroniseerd door wijzigingen aan alle zoekservices pushen wanneer een update vereist is.  Wanneer dit is het belangrijk om ervoor te zorgen voor het afhandelen van gevallen waarin een update aan voor een search-service mislukt en een of meer updates is mislukt.

## <a name="leveraging-azure-traffic-manager"></a>Gebruik te maken van Azure Traffic Manager
[Met Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) kunt u routeren van aanvragen naar meerdere geo zich websites die vervolgens worden ondersteund door meerdere Azure Search-Services.  Eén voordeel van het Traffic Manager is dat het Azure Search om ervoor te zorgen dat deze beschikbaar is en gebruikers te routeren naar alternatieve search-services in het geval van downtime kunt testen.  Bovendien, als u zoekaanvragen via Azure Web Sites zijn routering, Azure Traffic Manager kunt u saldo gevallen waarin de Website is geladen, maar niet Azure Search.  Hier volgt een voorbeeld van wat de architectuur die gebruikmaakt van Traffic Manager.

   ![Cross-tabblad van services per regio, met centrale Traffic Manager][3]

## <a name="monitoring-performance"></a>Prestaties bewaken
Azure Search biedt de mogelijkheid om te analyseren en controleren van de prestaties van uw service via [Search Traffic Analytics (STA)](search-traffic-analytics.md). Via STA, kunt u eventueel de afzonderlijke bewerkingen, evenals de samengevoegde metrische gegevens zich op een Azure Storage-account dat vervolgens kan worden verwerkt voor analyse of gevisualiseerd in Power BI.  STA metrische gegevens gebruikt, kunt u statistieken over de prestaties, zoals het gemiddelde aantal query's of query reactietijden bekijken.  Bovendien kunt de logboekregistratie van bewerkingen u inzoomen op gegevens van specifieke zoekopdrachten.

STA is een waardevol hulpmiddel om te begrijpen latentie tarief met ingang van die Azure Search-perspectief.  Omdat de prestatiegegevens van de query geregistreerd zijn gebaseerd op de tijd voor een query volledig in Azure Search worden verwerkt (vanaf het moment dat deze wordt aangevraagd wanneer deze wordt verzonden), zijn u kunt dit gebruiken om te bepalen of latentieproblemen met van de kant van Azure Search-service of time-outs IDE van de service, zoals netwerklatentie.  

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
