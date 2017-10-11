---
title: Gebruik en de statistieken in een Azure Search-service controleren | Microsoft Docs
description: Bijhouden resourcegrootte verbruik en de index voor Azure Search, een gehoste cloud search-service op Microsoft Azure.
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
tags: azure-portal
ms.assetid: 122948de-d29a-426e-88b4-58cbcee4bc23
ms.service: search
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/01/2017
ms.author: betorres
ms.openlocfilehash: 16cb5a1e16a59200f0e731622398efcf24c3f777
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-an-azure-search-service"></a>Een Azure Search-service controleren

Azure Search biedt verschillende bronnen voor het bijhouden van gebruik en prestaties van de search-services. Deze hebt u toegang tot metrische gegevens, Logboeken, indexstatistieken en uitgebreide bewakingsmogelijkheden Power BI. Dit artikel wordt beschreven voor het inschakelen van de verschillende strategieën voor bewaking en de resulterende gegevens interpreteren.

## <a name="azure-search-metrics"></a>Azure Search metrische gegevens
Metrische gegevens bieden u bijna real-time inzicht in uw zoekservice en beschikbaar zijn voor elke service, zonder aanvullende instellingen. Ze kunnen u de prestaties van uw service tot maximaal 30 dagen bijhouden.

Azure Search worden gegevens verzameld voor drie verschillende metrische gegevens:

* Latentie zoeken: tijd van de zoekservice die nodig zijn voor het verwerken van de zoekquery's per minuut geaggregeerd.
* Zoekquery's per seconde (QPS): het aantal zoekresultaten ontvangen per seconde, query's per minuut geaggregeerd.
* Beperkte zoekopdracht query percentage: Percentage zoekquery's die zijn beperkt, geaggregeerd per minuut.

![Schermopname van QPS activiteit][1]

### <a name="set-up-alerts"></a>waarschuwingen instellen
U kunt de metrische detailpagina waarschuwingen om een e-mailbericht of een geautomatiseerde actie geactiveerd wanneer er een metriek overschrijdt de drempelwaarde die u hebt gedefinieerd configureren.

Controleer de volledige documentatie over Azure-Monitor voor meer informatie over metrische gegevens.  

## <a name="how-to-track-resource-usage"></a>Het bijhouden van Resourcegebruik
Het bijhouden van de groei van indexen en de documentgrootte van het, kunt u proactief capaciteit aanpassen voordat het roept de bovengrens die u hebt ingesteld voor uw service. U kunt dit doen op de portal of programmatisch met behulp van de REST-API.

### <a name="using-the-portal"></a>De portal gebruiken

Voor het bewaken van Resourcegebruik, geven de aantallen en de statistieken voor de service in de [portal](https://portal.azure.com).

1. Aanmelden bij de [portal](https://portal.azure.com).
2. Open het servicedashboard van uw Azure Search-service. Tegels voor de service kunnen worden gevonden op de startpagina of kunt u bladeren naar de service van bladeren in de Snelbalk.

De Usage-sectie bevat een meter die aangeeft welk gedeelte van de beschikbare resources zijn momenteel in gebruik. Zie voor informatie over de beperkingen van de per-service voor indexen, documenten en opslag, [Servicelimieten](search-limits-quotas-capacity.md).

  ![Tegel gebruik][2]

> [!NOTE]
> In de bovenstaande schermafbeelding is voor de gratis service, die is maximaal één replica en elke partitie en kan alleen host 3 indexen, 10.000 documenten of 50 MB aan gegevens, afhankelijk van wat zich het eerste voordoet. Services die zijn gemaakt op een Basic- of Standard-laag een veel grotere Servicelimieten. Zie voor meer informatie over het kiezen van een laag [kiest u een laag of SKU](search-sku-tier.md).
>
>

### <a name="using-the-rest-api"></a>Met behulp van de REST-API
Zowel de Azure Search REST-API en de .NET SDK programmatisch toegang biedt tot service metrische gegevens.  Als u [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx) als u wilt een index van Azure SQL Database- of Azure Cosmos DB, een extra API is beschikbaar voor het ophalen van de getallen die u nodig hebt.

* [Indexstatistieken opvragen](/rest/api/searchservice/get-index-statistics)
* [Aantal documenten](/rest/api/searchservice/count-documents)
* [Status van de indexeerfunctie ophalen](/rest/api/searchservice/get-indexer-status)

## <a name="how-to-export-logs-and-metrics"></a>Het exporteren van Logboeken en metrische gegevens

U kunt de bewerkingslogboeken van de voor de service en de onbewerkte gegevens van de metrische gegevens die worden beschreven in de vorige sectie exporteren. Bewerkingslogboeken zodat die u weet hoe de service wordt gebruikt en kan worden gebruikt vanuit Power BI wanneer gegevens worden gekopieerd naar een opslagaccount. Azure search biedt een controle Power BI-inhoudspakket voor dit doel.


### <a name="enabling-monitoring"></a>Controle inschakelen
Open uw Azure Search-service in de [Azure-portal](http://portal.azure.com) onder de optie bewaking inschakelen.

Kies de gegevens die u wilt exporteren: Logboeken, metrische gegevens of beide. U kunt kopiëren naar een opslagaccount, verzenden naar een event hub of exporteren naar logboekanalyse.

![Het inschakelen van bewaking in de portal][3]

Zie de documentatie inschakelen met PowerShell of Azure CLI [hier](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Schema's voor logboeken en metrische gegevens
Wanneer de gegevens worden gekopieerd naar een opslagaccount, worden de gegevens zijn opgemaakt als JSON en de plaats in twee containers:

* Insights-logboeken-operationlogs: voor verkeerslogboeken zoeken
* Insights-metrics-pt1m: van metrische gegevens

Er is een blob, per uur, per container.

Voorbeeldpad:`resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Logboek schema
De logboeken blobs bevatten uw verkeerslogboeken search-service.
Elke blob heeft een basis-object aangeroepen **records** die een matrix van logboek-objecten bevat.
Elke blob heeft records van de bewerking die plaatsgevonden tijdens hetzelfde uur hebben.

| Naam | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| tijd |Datum/tijd |' 2015-12-07T00:00:43.6872559Z ' |Tijdstempel van de bewerking |
| resourceId |Tekenreeks |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>STANDAARD-RESOURCEGROUPS-PROVIDERS /<br/> MICROSOFT CORPORATION. SEARCHSERVICES-ZOEKOPDRACHT/SEARCHSERVICE' |Uw ResourceId |
| operationName |Tekenreeks |'Query.Search' |De naam van de bewerking |
| operationVersion |Tekenreeks |"2015-02-28" |De api-versie die wordt gebruikt |
| category |Tekenreeks |'OperationLogs' |constante |
| resultType |Tekenreeks |'Geslaagd' |Mogelijke waarden: geslaagd of mislukt |
| resultSignature |int |200 |HTTP-resultaatcode |
| durationMS |int |50 |Duur van de bewerking in milliseconden |
| properties |object |Zie de volgende tabel |Object met de bewerking-specifieke gegevens |

**Eigenschappen schema**
| Naam | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| Beschrijving |Tekenreeks |'/Indexes('content')/docs ophalen' |De bewerking eindpunt |
| Query’s uitvoeren |Tekenreeks |'? zoeken = AzureSearch & in $count = true & api-version = 2015-02-28 ' |De queryparameters |
| Documenten |int |42 |Aantal verwerkte documenten |
| NaamCommunity |Tekenreeks |'testindex' |Naam van de index die is gekoppeld aan de bewerking |

#### <a name="metrics-schema"></a>Schema van de metrische gegevens
| Naam | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| resourceId |Tekenreeks |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>STANDAARD-RESOURCEGROUPS-PROVIDERS /<br/>MICROSOFT CORPORATION. SEARCHSERVICES-ZOEKOPDRACHT/SEARCHSERVICE' |de resource-id |
| metricName |Tekenreeks |'Latentie' |de naam van de metrische gegevens |
| tijd |Datum/tijd |' 2015-12-07T00:00:43.6872559Z ' |de tijdstempel van de bewerking |
| Gemiddelde |int |64 |De gemiddelde waarde van de onbewerkte voorbeelden in het metrisch tijdsinterval |
| minimale |int |37 |De minimumwaarde van de onbewerkte voorbeelden in het metrisch tijdsinterval |
| Maximum |int |78 |De maximale waarde van de onbewerkte voorbeelden in het metrisch tijdsinterval |
| Totaal |int |258 |De totale waarde van de onbewerkte voorbeelden in het metrisch tijdsinterval |
| Aantal |int |4 |Het aantal onbewerkte voorbeelden die worden gebruikt voor het genereren van de metrische gegevens |
| timegrain |Tekenreeks |'PT1M' |De tijdseenheid van de metrische gegevens in de ISO 8601 |

Alle metrische gegevens worden gerapporteerd in intervallen van één minuut. Elke metriek wordt de minimum, maximum en gemiddelde waarden per minuut.

Voor de metriek SearchQueriesPerSecond is minimaal de laagste waarde voor zoekquery's per seconde dat is geregistreerd in die minuut. Dit geldt ook voor de maximumwaarde. Gemiddelde, is de statistische functie voor de hele minuut.
Denk aan dit scenario in één minuut: één seconde van hoog niveau is het maximum voor laden SearchQueriesPerSecond, gevolgd door 58 seconden gemiddelde belasting, en ten slotte één seconde met slechts één query die het minimum.

ThrottledSearchQueriesPercentage, minimum, maximum, gemiddelde en totale, een hebben dezelfde waarde: het percentage van de zoekquery's die zijn beperkt van het totale aantal zoekquery's in een minuut.

## <a name="analyzing-your-data-with-power-bi"></a>Analyseren van uw gegevens met Power BI

Wordt u aangeraden [Power BI](https://powerbi.microsoft.com) om te verkennen en uw gegevens te visualiseren. U kunt gemakkelijk verbinding te maken met uw Azure Storage-Account en snel starten analyseren van uw gegevens.

Azure Search biedt een [Power BI-inhoudspakket](https://app.powerbi.com/getdata/services/azure-search) waarmee u om te controleren en inzicht in uw search-verkeer met vooraf gedefinieerde grafieken en tabellen. Het bevat een set van Power BI-rapporten die automatisch verbinding maken met uw gegevens en visuele inzicht bieden over uw search-service. Zie voor meer informatie de [inhoudspakket help-pagina](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Power BI-dashboard voor Azure Search][4]

## <a name="next-steps"></a>Volgende stappen
Bekijk [schalen van replica's en partities](search-limits-quotas-capacity.md) voor instructies voor het verdelen van de toewijzing van partities en replica's voor een bestaande service.

Ga naar [beheren van uw zoekservice in Microsoft Azure](search-manage.md) voor meer informatie over servicebeheer of [prestaties en optimalisatie](search-performance-optimization.md) voor afstemming richtlijnen.

Meer informatie over het maken van fantastische rapporten. Zie [aan de slag met Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) voor meer informatie

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
