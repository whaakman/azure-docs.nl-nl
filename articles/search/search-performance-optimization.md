---
title: Azure Search-overwegingen voor prestaties en optimalisatie | Microsoft Docs
description: Azure Search-prestaties afstemmen en optimale schaal configureren
services: search
documentationcenter: 
author: LiamCavanagh
manager: pablocas
editor: 
ms.assetid: 4d3cd864-29d2-4921-be0d-a3f1a819de46
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: f4e371fc16bc57e6963f1ec51c0ea864fa568f0c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Azure Search-overwegingen voor prestaties en optimalisatie
Een goede zoekfunctie is een sleutel tot een succesvolle voor veel mobiele telefoons en webtoepassingen. Van vastgoed, auto marktplaatsen aan de online catalogus gebruikt snel zoeken en relevante resultaten heeft invloed op de gebruikerservaring. Dit document is bedoeld om kunt u ondersteuning voor aanbevolen procedures voor het ophalen van de meest buiten Azure Search, met name voor geavanceerde scenario's met geavanceerde vereisten voor schaalbaarheid, meertalige ontdekken of aangepaste positie.  Bovendien dit document bevat een overzicht van interne werking en bevat informatie over methoden die effectief in echte klant apps werken.

## <a name="performance-and-scale-tuning-for-search-services"></a>Prestaties en schaal afstemming voor Search-services
Er worden gebruikt voor zoekmachines zoals Google en Bing en de hoge prestaties bieden.  Wanneer klanten uw zoekopdracht ingeschakeld web of mobiele toepassing gebruikt, wordt ze als gevolg hiervan vergelijkbare prestatiekenmerken verwachten.  Als optimaliseren voor zoekprestaties, is een van de aanbevolen methoden ligt de nadruk op latentie is de tijd die een query wordt voltooid en de resultaten worden geretourneerd.  Wanneer voor de zoekopdracht latentie is het belangrijk om te optimaliseren:

1. Kies een doellatentie (of maximale hoeveelheid tijd) die aanvragen van een typische zoekopdracht uitvoeren moet om te voltooien.
2. Maken en testen van een echte werkbelasting op basis van uw search-service met een realistische gegevensset naar deze latentie meten.
3. Beginnen met een klein aantal query's per seconde (QPS) en doorgaan met het nummer in de test wordt uitgevoerd totdat de latentie van de query onder de gedefinieerde doellatentie zakt verhogen.  Dit is een benchmark belangrijk om te plannen voor schaal wanneer uw toepassing in gebruik groeit.
4. Waar mogelijk opnieuw gebruiken de HTTP-verbindingen.  Als u van Azure Search .NET SDK gebruikmaakt, betekent dit dat u opnieuw een exemplaar moet worden gebruikt of [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) exemplaar, en als u de REST-API gebruikt, moet u een enkele HttpClient hergebruiken.

Tijdens het maken van deze test werkbelastingen, er zijn enkele kenmerken van Azure Search rekening moet houden:

1. Het is mogelijk te pushen dus veel zoeken query in één keer dat de bronnen die beschikbaar zijn in uw Azure Search-service zal worden overbelast.  Als dit gebeurt, ziet u HTTP 503-responscodes.  Daarom is het raadzaam te beginnen met verschillende bereiken van de search-aanvragen om te zien van de verschillen in latentie tarieven, zoals het toevoegen van meer search-aanvragen.
2. Uploaden van inhoud naar Azure Search van invloed op de algehele prestaties en de latentie van de Azure Search-service.  Als u van plan bent om gegevens te verzenden terwijl gebruikers zoekopdrachten uitvoert, is het belangrijk dat u deze workload in aanmerking nemen in uw tests uit.
3. Niet elke zoekopdracht wordt uitgevoerd op het niveau van dezelfde prestaties.  Bijvoorbeeld, voert een document lookup- of suggestie doorgaans sneller dan een query met een groot aantal facetten en filters.  Het is raadzaam om op te nemen van de verschillende query's die u verwacht te zien in aanmerking bij het bouwen van uw tests uit.  
4. Variant van de search-aanvragen is belangrijk, omdat het in cache opslaan van gegevens als u de search-aanvragen van dezelfde voortdurend worden uitgevoerd, begint met het maken van de prestaties beter dan deze mogelijk met een meer uiteenlopende query ingesteld.

> [!NOTE]
> [Visual Studio Load testen](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) is een goede manier om de benchmark test zoals u kunt het HTTP-aanvragen worden uitgevoerd als nodig zou zijn voor het uitvoeren van een query uitgevoerd naar Azure Search en Hiermee garandeert van aanvragen.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Azure Search voor hoge query tarieven schalen en beperkt aanvragen
Wanneer u te veel aanvragen voor beperkte ontvangt of groter zijn dan de tarieven van de latentie doel van een toegenomen querybelasting, kunt u raadplegen om te reduceren latentie tarieven op twee manieren:

1. **Replica's vergroten:** een replica is vergelijkbaar met een kopie van uw Azure Search, op basis van meerdere exemplaren van aanvragen verdelen over zodat gegevens.  Alle load balancing en replicatie van gegevens over de replica's wordt beheerd door de Azure Search en kunt u het aantal replica's die zijn toegewezen voor uw service op elk gewenst moment wijzigen.  U kunt maximaal 12 replica's in een standaard-zoekservice en 3 replica's in een Basic-zoekservice toewijzen. Replica's kunnen worden aangepast vanaf de [Azure-portal](search-create-service-portal.md) of [PowerShell](search-manage-powershell.md).
2. **Toename zoeken laag:** Azure Search wordt geleverd een [aantal lagen](https://azure.microsoft.com/pricing/details/search/) en elk van deze lagen biedt verschillende niveaus van prestaties.  In sommige gevallen mogelijk hebt u zo veel query's dat de laag die u voldoende lage latentie tarieven, kan niet bieden zelfs wanneer de replica's volledig worden benut uit.  In dit geval wilt u overwegen gebruik te maken van een van de hogere lagen zoeken zoals de Azure Search S3-laag is geschikt voor scenario's met een groot aantal documenten en zeer hoge query werkbelastingen.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Schalen van Azure Search voor langzame afzonderlijke query 's
Er is een andere reden waarom latentie tarieven kunnen traag zijn van één query duurt te lang om te voltooien.  In dit geval wordt verbeterd het toevoegen van replica's niet latentie tarieven.  Voor deze aanvraag zijn twee opties beschikbaar:

1. **Partities verhogen** een partitie is een mechanisme voor het splitsen van uw gegevens over extra bronnen.  Om deze reden dat wanneer u een tweede partitie toevoegen uw gegevens opgehaald opgesplitst in twee.  Een derde partitie splitst uw index in drie, enzovoort.  Dit heeft ook het effect dat in sommige gevallen trage query's wordt sneller vanwege de garandeert van de berekening.  Er zijn enkele voorbeelden van wanneer we dit garandeert extreem met query's met lage selectiviteit query's werken hebben gezien.  Dit proces bestaat van query's die overeenkomen met veel documenten of wanneer facetten moet aantallen bieden via een groot aantal documenten.  Omdat er veel van de berekening die nodig zijn voor de beoordeling van de relevantie van de documenten of de getallen van documenten te tellen, kunt toe te voegen extra partities u extra berekening bieden.  
   
   Er zijn mag maximaal 12 partities in de standaard-zoekservice en 1 partitie in de basic search-service.  Partities kunnen worden aangepast vanaf de [Azure-portal](search-create-service-portal.md) of [PowerShell](search-manage-powershell.md).
2. **Limiet hoge kardinaliteit velden:** een veld van hoge kardinaliteit bestaat uit een geschikt voor facetten of Filterbaar veld dat een groot aantal unieke waarden is, en als gevolg hiervan wordt een groot aantal bronnen voor het berekenen van resultaten op.   Bijvoorbeeld, zouden instellen van een Product-ID of beschrijving veld als geschikt voor facetten/Filterbaar voor hoge kardinaliteit omdat de meeste van de waarden uit het document naar document uniek zijn. Beperk het aantal velden van hoge kardinaliteit waar mogelijk.
3. **Toename zoeken laag:** verplaatsen tot een hogere laag van Azure Search is een andere manier om de prestaties van trage query's te verbeteren.  Elke laag hoger biedt ook snellere processor en meer geheugen die een positieve invloed op prestaties van query's hebben kan.

## <a name="scaling-for-availability"></a>Schaal van beschikbaarheid
Replica's niet alleen te reduceren latentie van query maar kunnen ook de mogelijkheid voor hoge beschikbaarheid.  Met een enkele replica verwachte periodieke downtime als gevolg van de server opnieuw wordt opgestart nadat de software-updates of voor andere onderhoud gebeurtenissen die zich voordoen.  Als gevolg hiervan is het belangrijk in als uw toepassing is vereist voor hoge beschikbaarheid van zoekopdrachten (query's) en schrijfbewerkingen (indexering gebeurtenissen).  Azure Search biedt SLA-opties op de betaald zoeken aanbiedingen met de volgende kenmerken:

* 2 replica's voor hoge beschikbaarheid van alleen-lezen werkbelastingen (query's)
* 3 of meer replica's voor hoge beschikbaarheid van alleen-lezen-werkbelastingen (query's en indexeren)

Ga voor meer informatie over dit naar de [Azure Search Service Level Agreement](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Omdat replica's kopieën van uw gegevens zijn, kunt met van meerdere replica's van de computer opnieuw wordt opgestart en het onderhoud op basis van een replica op een tijdstip terwijl u query's om te worden uitgevoerd met de andere replica's in Azure Search.  Daarom moet u ook rekening houden met deze uitvaltijd mogelijk invloed van de query's die moeten worden uitgevoerd met één exemplaar van de gegevens minder.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Geografisch verspreide werkbelastingen schalen en geografische redundantie bieden
Voor geografisch verspreide werkbelastingen, zult u merken dat gebruikers zich ver van het datacenter waar uw Azure Search-service wordt gehost hogere latentie tarieven hebben.  Daarom is het vaak belangrijk dat u meerdere search-services in regio's die in dichter bij deze gebruikers hebben.  Azure Search biedt momenteel geen een automatische methode van Azure Search-index geo-replicatie tussen regio's, maar er zijn bepaalde technieken die kunnen worden gebruikt waardoor dit proces eenvoudig te implementeren en beheren. Deze worden beschreven in de volgende secties.

Het doel van een set geografisch verspreide zoekservices is om twee of meer indexen beschikbaar in twee of meer regio's waar een gebruiker worden doorgestuurd naar de Azure Search-service waarmee de laagste latentie, zoals in dit voorbeeld:

   ![Cross-tabblad van services per regio][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Gegevens synchroon te houden op meerdere Azure Search-services
Er zijn twee opties voor het bewaren van uw gedistribueerde zoekservices synchroon die bestaan uit een met behulp van de [Azure Search-indexeerfunctie](search-indexer-overview.md) of de Push-API (ook wel de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Indexeerfuncties in Azure Search
Als u de Azure Search-indexeerfunctie gebruikt, importeert u al gegevenswijzigingen van een centrale datastore zoals Azure SQL DB- of Azure Cosmos DB. Wanneer u een nieuwe zoekopdracht Service, maakt u gewoon ook een nieuwe Azure Search-indexeerfunctie voor de service die naar deze hetzelfde gegevensarchief verwijst te maken. Wanneer u nieuwe wijzigingen in het gegevensarchief komen worden ze vervolgens geïndexeerd door de verschillende indexeerfuncties.  

Hier volgt een voorbeeld van hoe die architectuur eruit zou.

   ![Één gegevensbron met gedistribueerde indexeerfunctie en combinaties van service][2]

### <a name="push-api"></a>Push-API
Als u de Azure Search Push API naar [bijwerken van inhoud in uw Azure Search-index](https://docs.microsoft.com/rest/api/searchservice/update-index), kunt u de verschillende zoekservices synchroon wijzigingen door naar te pushen alle zoekservices telkens wanneer een update vereist is.  Wanneer dit is het belangrijk om ervoor te zorgen voor het afhandelen van gevallen waarbij een update aan voor een search-service mislukt en een of meer updates is mislukt.

## <a name="leveraging-azure-traffic-manager"></a>Gebruik van Azure Traffic Manager
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) kunt u de route-aanvragen voor meerdere websites geo bevinden die vervolgens worden ondersteund door meerdere Azure Search-Services.  Een voordeel van het Traffic Manager is dat het Azure Search om te zorgen dat deze beschikbaar is en gebruikers omgeleid naar alternatieve search-services in het geval van uitvaltijd kunt zoeken.  Bovendien als u de search-aanvragen via de Azure-websites worden routering, Azure Traffic Manager kunt u saldo gevallen waarin de Website van laden, maar geen Azure Search.  Hier volgt een voorbeeld van wat de architectuur die gebruikmaakt van Traffic Manager.

   ![Cross-tabblad van services per regio, met centrale Traffic Manager][3]

## <a name="monitoring-performance"></a>Prestaties bewaken
Azure Search biedt de mogelijkheid om te analyseren en controleren van de prestaties van uw service via [Search Traffic Analytics (STA)](search-traffic-analytics.md). Via STA zijn, kunt u eventueel de afzonderlijke zoekopdrachten evenals samengevoegde metrische gegevens aan een Azure Storage-account die vervolgens kan worden verwerkt voor analyse of die in Power BI worden gevisualiseerd registreren.  STA metrische gegevens kunt u prestatiestatistieken zoals gemiddelde aantal query's of reactietijden van de query bekijken.  De bewerking logboekregistratie Bovendien kunt u inzoomen op gegevens van specifieke zoekopdrachten.

STA is een waardevol hulpmiddel om te begrijpen latentie tarieven vanuit het perspectief van die Azure Search.  Omdat de query prestatiewaarden geregistreerd zijn gebaseerd op het duurt voordat een query volledig moeten worden verwerkt in Azure Search (vanaf het moment dat deze wordt aangevraagd wanneer deze wordt verzonden), bent u kunt gebruiken dit om te bepalen of latentieproblemen vanaf de Azure Search-service of buiten de service, zoals van netwerklatentie zijn.  

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de prijzen lagen en services beperkingen voor elk adres, [Servicelimieten in Azure Search](search-limits-quotas-capacity.md).

Ga naar [capaciteitsplanning](search-capacity-planning.md) voor meer informatie over de combinaties van partitie en replica.

Bekijk de volgende video voor meer drilldown op de prestaties en sommige demonstraties van het implementeren van de optimalisaties in dit artikel wordt beschreven:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
