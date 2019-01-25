---
title: Resource gebruiks- en statistieken controleren voor een search-service - Azure Search
description: Query metrische gegevens van activiteiten, resourceverbruik en andere systeemgegevens ophalen uit een Azure Search-service.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 5f8a4e7dcaa1bc2df71246f67d06fc63ae4fcd06
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54883497"
---
# <a name="monitor-an-azure-search-service-in-azure-portal"></a>Azure Search-service in Azure portal controleren

U kunt systeemgegevens over Resourcegebruik, plus query metrische gegevens zoals query's Per seconde (QPS), latentie van query en het percentage aanvragen die zijn beperkt weergeven op de pagina overzicht van uw Azure Search-service. Bovendien kunt u de portal gebruikmaken van een bereik van de bewakingsmogelijkheden in het Azure-platform om meer gedetailleerde gegevens te verzamelen. 

In dit artikel wordt geïdentificeerd en vergelijkt de beschikbare opties voor logboekregistratie van Azure Search-bewerkingen. Deze handleiding bevat instructies voor het inschakelen van logboekregistratie en logboek-opslag en over het uitbreiden van de gegevens die worden verzameld.

Als u een ondersteuningsticket indient, zijn er geen specifieke taken of informatie die u nodig hebt om te bieden. Ondersteuningsmedewerkers hebben de benodigde informatie voor het onderzoeken van specifieke problemen.  

## <a name="metrics-at-a-glance"></a>Metrische gegevens in een oogopslag

**Gebruik** en **bewaking** secties die zijn ingebouwd in overzicht opslagverbruik visualiseren en uitvoering van metrische gegevens op te vragen. Deze informatie is beschikbaar zodra u begint met de service, zonder configuratie vereist. Deze pagina wordt vernieuwd om de paar minuten. Als u bij het voltooien van beslissingen over [welke laag moet worden gebruikt voor werkbelastingen voor productie](search-sku-tier.md), of [aanpassen van het aantal actieve replica's en partities](search-capacity-planning.md), deze metrische gegevens kan u helpen bij deze beslissingen komen door die laat zien u hoe snel de resources worden gebruikt en hoe goed de huidige configuratie van de bestaande belasting verwerkt.

De **gebruik** tabblad ziet u de beschikbaarheid van ten opzichte van de huidige [limieten](search-limits-quotas-capacity.md). De volgende afbeelding is voor de gratis service, die beperkt 3-objecten van elk type en 50 MB aan opslagruimte tot wordt. Een Basic- of Standard-service heeft een hogere limieten en als u de aantallen partitie verhoogt, maximumopslag toeneemt proportioneel.

![Status van het ten opzichte van een doeltreffende limieten](./media/search-monitor-usage/usage-tab.png
 "status van het ten opzichte van een doeltreffende limieten")

## <a name="queries-per-second-qps-and-other-metrics"></a>Query's per seconde (QPS) en andere metrische gegevens

De **bewaking** tabblad toont voortschrijdende gemiddelden voor metrische gegevens zoals zoeken *query's Per seconde* (QPS), bij elkaar opgeteld per minuut. 
*Zoeklatentie* is de hoeveelheid tijd de search-service die nodig is voor het verwerken van zoekquery's bij elkaar opgeteld per minuut. *Beperkte zoekquery query percentage* (niet weergegeven), is het percentage van zoekquery's die zijn beperkt, ook bij elkaar opgeteld per minuut.

![Query's per seconde activiteit](./media/search-monitor-usage/monitoring-tab.png "query's per seconde activiteit")

## <a name="activity-logs"></a>Activiteitenlogboeken

De **activiteitenlogboek** verzamelt gegevens van Azure Resource Manager. Voorbeelden van gegevens die zijn gevonden in het activiteitenlogboek zijn maken of verwijderen van een service bijwerken van een resourcegroep, controleren op beschikbaarheid van de naam of het krijgen van een service-toegangssleutel voor het afhandelen van een aanvraag. 

U hebt toegang tot de **activiteitenlogboek** in het deelvenster navigatie aan de linkerkant of van meldingen in het bovenste vensteropdracht balk of vanuit de **vaststellen en oplossen van problemen** pagina.

Voor het in gebruik zijnde taken, zoals een index te maken of verwijderen van een gegevensbron, ziet u algemene meldingen, zoals 'Admin Key ophalen' voor elke aanvraag, maar niet de specifieke actie zelf. Voor de hand van deze gegevens, moet u een oplossing voor prestatiecontrole van invoegtoepassing inschakelen.

## <a name="add-on-monitoring-solutions"></a>Bewakingsoplossingen van invoegtoepassing

Azure Search slaat geen gegevens buiten de objecten die deze beheert, wat betekent dat logboek gegevens heeft extern worden opgeslagen. U kunt een van de onderstaande bronnen configureren als u wilt behouden van gegevens aan het logboek. 

De volgende tabel vergelijkt de opties voor het opslaan van Logboeken en uitgebreide bewaking van servicebewerkingen en querywerkbelastingen via Application Insights toevoegen.

| Resource | Gebruikt voor |
|----------|----------|
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) | [Zoekverkeer](search-traffic-analytics.md). Dit is de enige oplossing waarmee u meer informatie over aanvragen, die verder gaan dan de waarden die zijn geïdentificeerd in de onderstaande logboekregistratie en metrische gegevens schema wordt vastgelegd. Met deze methode voert u kopiëren en plakken instrumentation code in de bronbestanden voor het doorsturen van gegevens naar Application Insights voor analyse van de invoer in de query-term, query's met nul overeenkomsten en enzovoorts bedekt. Beste de Power BI als de front-end analytics gegevens die zijn opgeslagen in Application Insights.  |
| [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Aanvragen en metrische gegevens, op basis van de onderstaande schema's. Gebeurtenissen worden geregistreerd in een Blob-container. We raden je Excel of Power BI als de analytics-front-end aan opgeslagen gegevens in Azure Blob-opslag.|
| [Event Hub](https://docs.microsoft.com/azure/event-hubs/) | Aanvragen en metrische gegevens, op basis van de schema's beschreven in dit artikel. Kies deze optie als een alternatieve data collection-Services voor zeer grote logboeken. |

Azure search biedt een controle [Power BI Content Pack](https://app.powerbi.com/getdata/services/azure-search) zodat u gegevens kunt analyseren. Het inhoudspakket bestaat uit rapporten die zijn geconfigureerd voor het automatisch verbinding maken met uw gegevens en een visueel overzicht over uw search-service geven. Zie voor meer informatie de [help-pagina](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

De optie voor de Blob-opslag is beschikbaar als gratis gedeelde service zodat u dit zonder kosten voor de levensduur van uw Azure-abonnement uitproberen kunt. De volgende sectie helpt u door de stappen voor het inschakelen en gebruiken van Azure Blob-opslag voor het verzamelen en toegang krijgen tot logboekgegevens die zijn gemaakt door Azure Search-bewerkingen.

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Logboekregistratie voor indexeren en query-werkbelastingen is standaard uitgeschakeld en is afhankelijk van aanvullende oplossingen voor infrastructuur voor logboekregistratie en externe opslag. Zelfstandig gebruikt is de enige persistente gegevens in Azure Search indexen, zodat logboeken ergens anders moeten worden opgeslagen.

In deze sectie leert u hoe u Blob storage gebruiken voor gegevens in het logboek gebeurtenissen en metrische gegevens bevatten.

1. [Maak een opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) als u er nog geen hebt. U kunt deze plaatsen in dezelfde resourcegroep bevinden als de Azure Search te vereenvoudigen opschonen later als u wilt verwijderen van alle resources die in deze oefening gebruikt.

2. Open de pagina overzicht van uw zoekservice. In het deelvenster navigatie aan de linkerkant, schuif omlaag naar **bewaking** en klikt u op **bewaking inschakelen**.

   ![Schakel de bewaking](./media/search-monitor-usage/enable-monitoring.png "bewaking inschakelen")

3. Kies de gegevens die u wilt exporteren: Logboeken, metrische gegevens of beide. U kunt kopiëren naar een opslagaccount, verzenden naar een event hub of exporteren naar Log Analytics.

   Voor archivering naar Blob-opslag, alleen de storage-account moet bestaan. Containers en blobs wordt gemaakt tijdens het exporteren van gegevens aan het logboek.

   ![Configure blob-opslag archiveren](./media/search-monitor-usage/configure-blob-storage-archive.png "configureren blob-opslag archiveren")

4. Sla het profiel.

5. Test registratie door te maken of verwijderen van objecten (genereert een operationeel logboek) en door het verzenden van query's (genereert metrische gegevens). 

Logboekregistratie is ingeschakeld wanneer u het profiel opslaan, containers worden alleen gemaakt wanneer er een gebeurtenis in het logboek of de meting. Het kan enige tijd duren voor de containers worden weergegeven. U kunt [de gegevens visualiseren in Power BI](#analyze-with-power-bi) zodra deze beschikbaar.

Wanneer de gegevens worden gekopieerd naar een opslagaccount, worden de gegevens zijn opgemaakt als JSON en in twee containers geplaatst:

* Insights-logs-operationlogs: voor zoeken in logboeken over webverkeer
* Insights-metrics-pt1m: voor metrische gegevens

Er is een blob, per uur, per container.

Voorbeeldpad: `resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2018/m=12/d=25/h=01/m=00/name=PT1H.json`

## <a name="log-schema"></a>Logboek-schema
BLOBs met de verkeerslogboeken van uw search-service zijn gestructureerd zoals beschreven in deze sectie. Elke blob heeft een basis-object met de naam **records** die een matrix van logboek-objecten. Elke blob bevat records voor alle bewerkingen die plaatsgevonden tijdens hetzelfde uur hebben.

| Name | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| time |datum/tijd |"2018-12-07T00:00:43.6872559Z" |Tijdstempel van de bewerking |
| resourceId |string |' / SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>STANDAARD-RESOURCEGROUPS-PROVIDERS /<br/> MICROSOFT. ZOEKEN/SEARCHSERVICES/SEARCHSERVICE" |Uw ResourceId |
| operationName |string |"Query.Search" |De naam van de bewerking |
| operationVersion |string |"2017-11-11" |De api-versie die wordt gebruikt |
| category |string |"OperationLogs" |constante |
| resultType |string |"Geslaagd" |Mogelijke waarden: Slagen of mislukken |
| resultSignature |int |200 |HTTP-resultaatcode |
| durationMS |int |50 |Duur van de bewerking in milliseconden |
| properties |object |Zie de volgende tabel |Object met de bewerking-specifieke gegevens |

**Eigenschappen van schema**

| Name | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| Description |string |'/Indexes('content')/docs ophalen' |Eindpunt van de bewerking |
| Query’s uitvoeren |string |"?search=AzureSearch&$count=true&api-version=2017-11-11" |De queryparameters |
| Documenten |int |42 |Aantal verwerkte documenten |
| Index |string |"testindex" |Naam van de index die is gekoppeld aan de bewerking |

## <a name="metrics-schema"></a>Schema van de metrische gegevens

Metrische gegevens vastgelegd voor queryaanvragen.

| Name | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| resourceId |string |' / SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>STANDAARD-RESOURCEGROUPS-PROVIDERS /<br/>MICROSOFT. ZOEKEN/SEARCHSERVICES/SEARCHSERVICE" |de resource-id |
| MetricName |string |"Latentie" |de naam van de metrische gegevens |
| time |datum/tijd |"2018-12-07T00:00:43.6872559Z" |tijdstempel van de bewerking |
| gemiddeld |int |64 |De gemiddelde waarde van de onbewerkte voorbeelden in de metrische tijdsinterval |
| minimum |int |37 |De minimumwaarde van de onbewerkte voorbeelden in de metrische tijdsinterval |
| maximum |int |78 |De maximale waarde van de onbewerkte voorbeelden in de metrische tijdsinterval |
| totaal |int |258 |De totale waarde van de onbewerkte voorbeelden in de metrische tijdsinterval |
| count |int |4 |Het aantal onbewerkte voorbeelden die worden gebruikt voor het genereren van de metrische gegevens |
| timegrain |string |"PT1M" |Het tijdsinterval van de metrische gegevens in de ISO 8601 |

Alle metrische gegevens worden gerapporteerd in één minuut intervallen. Elke metriek wordt de minimale, maximale en gemiddelde waarden per minuut.

Voor de metriek SearchQueriesPerSecond is minimaal de laagste waarde voor zoekquery's per seconde dat is geregistreerd gedurende die minuut. Dit geldt ook voor de maximale waarde. Gemiddelde, wordt de statistische functie voor de hele minuut.
Denk aan dit scenario gedurende één minuut: één seconde van hoog laden dat wil zeggen het maximum voor SearchQueriesPerSecond, gevolgd door 58 seconden gemiddelde belasting, en tot slot een tweede met slechts één query die is de minimale.

ThrottledSearchQueriesPercentage, minimum, maximum, gemiddelde en totale, een hebben dezelfde waarde: het percentage van zoekquery's die zijn beperkt, van het totale aantal zoekquery's gedurende één minuut.

## <a name="analyze-with-power-bi"></a>Analyseren met Power BI

Wordt u aangeraden [Power BI](https://powerbi.microsoft.com) om te verkennen en visualiseren van uw gegevens, met name als u ingeschakeld [zoekverkeer](search-traffic-analytics.md). Zie voor meer informatie de [help-pagina](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

Verbindingen vereisen naam en toegangssleutel van het opslagaccount, die u kunt ophalen uit Azure portal-pagina's op de **toegangssleutels** pagina van uw storage-account-dashboard.

1. Installeer de [Power BI-inhoudspakket](https://app.powerbi.com/getdata/services/azure-search). Het inhoudspakket wordt toegevoegd voor vooraf gedefinieerde grafieken en tabellen die handig is voor het analyseren van de aanvullende gegevens die zijn vastgelegd voor search traffic analytics. 

   Als u van Blob-opslag of een ander opslagmechanisme gebruikmaakt en u geen instrumentatie kunt aan uw code toevoegen hebt, kunt u het inhoudspakket overslaan en ingebouwde Power BI-visualisaties.

2. Open **Power BI**, klikt u op **gegevens ophalen** > **Services** > **Azure Search**.

3. Voer de naam van het opslagaccount, selecteer **sleutel** voor verificatie en plak in een toegangssleutel.

4. De gegevens te importeren en klik vervolgens op **inzien**.

De volgende schermafbeelding ziet u de ingebouwde rapporten en grafieken voor het analyseren van zoekverkeer.

![Power BI-dashboard voor Azure Search](./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png "Power BI-dashboard voor Azure Search")

## <a name="get-sys-info-apis"></a>Sys-info-API's ophalen
Zowel de Azure Search REST-API en de .NET SDK bieden programmatisch toegang tot metrische gegevens, index en indexeerfunctie servicegegevens en document telt.

* [Statistieken over Services ophalen](/rest/api/searchservice/get-service-statistics)
* [Indexstatistieken ophalen](/rest/api/searchservice/get-index-statistics)
* [Aantal documenten](/rest/api/searchservice/count-documents)
* [Status van de indexeerfunctie ophalen](/rest/api/searchservice/get-indexer-status)

Zie de documentatie om in te schakelen met behulp van PowerShell of Azure CLI, [hier](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

## <a name="next-steps"></a>Volgende stappen

[Uw zoekservice op Microsoft Azure beheren](search-manage.md) voor meer informatie over servicebeheer en [prestaties en optimalisatie](search-performance-optimization.md) voor het afstemmen van de richtlijnen.

Meer informatie over het maken van geweldige rapporten. Zie [aan de slag met Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) voor meer informatie.

