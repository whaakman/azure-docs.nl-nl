---
title: Gebruik en statistieken in een Azure Search-service controleren | Microsoft Docs
description: Bijhouden resourcegrootte verbruik en de index voor Azure Search, een gehoste cloud search-service op Microsoft Azure.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 274513c7e8ad1ca9ed0452f6237eec4ebb38e9cd
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53134859"
---
# <a name="monitoring-an-azure-search-service"></a>Een Azure Search-service controleren

Azure Search biedt verschillende resources voor het bijhouden van gebruik en prestaties van de search-services. Het biedt u toegang tot metrische gegevens, Logboeken, indexstatistieken en uitgebreide mogelijkheden voor bewaking op Power BI. Dit artikel wordt beschreven hoe u om in te schakelen van de verschillende strategieën voor bewaking en over het interpreteren van de resulterende gegevens.

## <a name="azure-search-metrics"></a>Metrische gegevens van Azure Search
Metrische gegevens geven u bijna realtime inzicht in uw search-service en zijn beschikbaar voor elke service, zonder aanvullende instellingen. U kunt de prestaties van uw service gedurende maximaal 30 dagen bijhouden.

Azure Search worden verzameld van drie verschillende metrische gegevens:

* Zoeklatentie: tijd van de search-service die nodig zijn voor het verwerken van zoekquery's bij elkaar opgeteld per minuut.
* Zoekquery's per seconde (QPS): het aantal zoeken ontvangen per seconde, query's bij elkaar opgeteld per minuut.
* Beperkte zoekquery query percentage: Percentage van de zoekquery's die zijn beperkt, bij elkaar opgeteld per minuut.

![Schermafbeelding van QPS activiteit][1]

### <a name="set-up-alerts"></a>Waarschuwingen instellen
U kunt de pagina met metrische gegevens over waarschuwingen voor het activeren van een e-mailbericht of een geautomatiseerde actie wanneer een metriek een drempelwaarde overschrijdt die u hebt gedefinieerd configureren.

Controleer voor meer informatie over metrische gegevens over de volledige documentatie over Azure Monitor.  

## <a name="how-to-track-resource-usage"></a>Het om bij te houden van brongebruik
De groei van indexen en de documentgrootte van het bijhouden, kunt u proactief capaciteit aanpassen voordat u de bovengrens die u hebt ingesteld voor uw service te maken. U kunt dit doen op de portal of programmatisch met behulp van de REST-API.

### <a name="using-the-portal"></a>De portal gebruiken

Weergeven voor het bewaken van Resourcegebruik, het aantal en de statistieken voor uw service in de [portal](https://portal.azure.com).

1. Meld u aan bij de [portal](https://portal.azure.com).
2. Open het servicedashboard van uw Azure Search-service. Tegels voor de service kunnen worden gevonden op de startpagina of kunt u bladeren naar de service door middel van bladeren in de Snelbalk.

De sectie gebruik bevat een meter die aangeeft welk gedeelte van de beschikbare resources zijn momenteel in gebruik. Zie voor meer informatie over limieten voor indexen, documenten en opslag per service [Servicelimieten](search-limits-quotas-capacity.md).

  ![Gebruikstegel][2]

> [!NOTE]
> De bovenstaande schermafbeelding is voor de gratis service, die geldt een maximum van een van de replica en elke partitie en kan alleen host 3 indexen, 10.000 documenten of 50 MB aan gegevens, afhankelijk van wat het eerste komt. Services die zijn gemaakt in een Basic of Standard-laag hebben veel grotere Servicelimieten. Zie voor meer informatie over het kiezen van een laag [een laag of SKU kiezen](search-sku-tier.md).
>
>

### <a name="using-the-rest-api"></a>Met behulp van de REST API
Zowel de Azure Search REST-API en de .NET SDK bieden programmatische toegang tot metrische gegevens van services.  Als u [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx) voor het laden een index uit Azure SQL Database of Azure Cosmos DB, een extra API is beschikbaar voor het ophalen van de getallen die u nodig hebt.

* [Indexstatistieken ophalen](/rest/api/searchservice/get-index-statistics)
* [Aantal documenten](/rest/api/searchservice/count-documents)
* [Status van de indexeerfunctie ophalen](/rest/api/searchservice/get-indexer-status)

## <a name="how-to-export-logs-and-metrics"></a>Het exporteren van Logboeken en metrische gegevens

U kunt de logboeken voor bewerkingen voor de service en de onbewerkte gegevens voor de metrische gegevens die worden beschreven in de voorgaande sectie exporteren. Bewerkingslogboeken zodat die u weet hoe de service wordt gebruikt en kan worden gebruikt vanuit Power BI wanneer gegevens worden gekopieerd naar een opslagaccount. Azure search biedt een controle Power BI-inhoudspakket voor dit doel.


### <a name="enabling-monitoring"></a>-Bewaking inschakelen
Open uw Azure Search-service in de [Azure-portal](http://portal.azure.com) onder de optie bewaking inschakelen.

Kies de gegevens die u wilt exporteren: Logboeken, metrische gegevens of beide. U kunt kopiëren naar een opslagaccount, verzenden naar een event hub of exporteren naar Log Analytics.

![Het inschakelen van bewaking in de portal][3]

Zie de documentatie om in te schakelen met behulp van PowerShell of Azure CLI, [hier](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Schema's voor logboeken en metrische gegevens
Wanneer de gegevens worden gekopieerd naar een opslagaccount, worden de gegevens zijn opgemaakt als JSON en de plaats in twee containers:

* Insights-logs-operationlogs: voor zoeken in logboeken over webverkeer
* Insights-metrics-pt1m: voor metrische gegevens

Er is een blob, per uur, per container.

Voorbeeldpad: `resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Logboek-schema
De logboeken blobs bevatten de verkeerslogboeken van uw search-service.
Elke blob heeft een basis-object met de naam **records** die een matrix met objecten logboek bevat.
Elke blob heeft records voor de bewerking die plaatsgevonden tijdens hetzelfde uur hebben.

| Name | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| time |datum/tijd |"2015-12-07T00:00:43.6872559Z" |Tijdstempel van de bewerking |
| resourceId |string |' / SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>STANDAARD-RESOURCEGROUPS-PROVIDERS /<br/> MICROSOFT. ZOEKEN/SEARCHSERVICES/SEARCHSERVICE" |Uw ResourceId |
| operationName |string |"Query.Search" |De naam van de bewerking |
| operationVersion |string |"2015-02-28" |De api-versie die wordt gebruikt |
| category |string |"OperationLogs" |constante |
| resultType |string |"Geslaagd" |Mogelijke waarden: slagen of mislukken |
| resultSignature |int |200 |HTTP-resultaatcode |
| durationMS |int |50 |Duur van de bewerking in milliseconden |
| properties |object |Zie de volgende tabel |Object met de bewerking-specifieke gegevens |

**Eigenschappen van schema**

| Name | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| Description |string |'/Indexes('content')/docs ophalen' |Eindpunt van de bewerking |
| Query’s uitvoeren |string |'? search = AzureSearch & $count = true & api-version = 2015-02-28 ' |De queryparameters |
| Documenten |int |42 |Aantal verwerkte documenten |
| Index |string |"testindex" |Naam van de index die is gekoppeld aan de bewerking |

#### <a name="metrics-schema"></a>Schema van de metrische gegevens

| Name | Type | Voorbeeld | Opmerkingen |
| --- | --- | --- | --- |
| resourceId |string |' / SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>STANDAARD-RESOURCEGROUPS-PROVIDERS /<br/>MICROSOFT. ZOEKEN/SEARCHSERVICES/SEARCHSERVICE" |de resource-id |
| MetricName |string |"Latentie" |de naam van de metrische gegevens |
| time |datum/tijd |"2015-12-07T00:00:43.6872559Z" |tijdstempel van de bewerking |
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

## <a name="analyzing-your-data-with-power-bi"></a>Analyseren van uw gegevens met Power BI

Wordt u aangeraden [Power BI](https://powerbi.microsoft.com) om te verkennen en visualiseren van uw gegevens. U kunt eenvoudig verbinding te maken met uw Azure Storage-Account en snel starten voor het analyseren van uw gegevens.

Azure Search biedt een [Power BI Content Pack](https://app.powerbi.com/getdata/services/azure-search) waarmee u om te controleren en begrijpen van uw verkeer zoeken met vooraf gedefinieerde grafieken en tabellen. Het bevat een set met Power BI-rapporten die automatisch verbinding maken met uw gegevens en een visueel overzicht over uw search-service geven. Zie voor meer informatie de [help-pagina](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Power BI-dashboard voor Azure Search][4]

## <a name="next-steps"></a>Volgende stappen
Beoordeling [de schaal van replica's en partities](search-limits-quotas-capacity.md) voor instructies over het saldo van de toewijzing van partities en replica's voor een bestaande service.

Ga naar [beheren van uw zoekservice op Microsoft Azure](search-manage.md) voor meer informatie over servicebeheer of [prestaties en optimalisatie](search-performance-optimization.md) voor het afstemmen van de richtlijnen.

Meer informatie over het maken van geweldige rapporten. Zie [aan de slag met Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) voor meer informatie

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
