---
title: Gebruiks- en metrische gegevens voor resources voor een search-service - Azure Search bewaken
description: Logboekregistratie inschakelen, metrische gegevens van de query activiteiten, Resourcegebruik en andere systeemgegevens ophalen uit een Azure Search-service.
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
ms.openlocfilehash: ed084520e092802ffa2a42e8a0c664ec09c4cbb7
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093237"
---
# <a name="monitor-resource-consumption-and-query-activity-in-azure-search"></a>Resource verbruik en query-activiteiten in Azure Search controleren

U kunt systeemgegevens over brongebruik, query metrische gegevens en hoeveel quotum is beschikbaar voor het maken van meer indexen, Indexeerfuncties en gegevensbronnen weergeven op de pagina overzicht van uw Azure Search-service. U kunt de portal ook gebruiken om log analytics of een andere resource die wordt gebruikt voor het verzamelen van permanente gegevens te configureren. 

Instellen van de logboeken is handig voor selfservice-diagnostiek en behoud operationele geschiedenis. Intern, logboeken bevinden zich op de back-end voor een korte tijd, voldoende voor onderzoek en analyse als u een ondersteuningsticket. Als u wilt de controle- en toegangsbeheer om informatie te registreren, moet u instellen van een van de oplossingen die in dit artikel beschreven.

In dit artikel leert u over de controle-opties, over het inschakelen van logboekregistratie en meld u opslag en hoe u logboekinhoud weer te geven.

## <a name="metrics-at-a-glance"></a>Metrische gegevens in een oogopslag

**Gebruik** en **bewaking** secties die zijn ingebouwd in het overzicht van het rapport op resourceverbruik pagina en uitvoering van metrische gegevens op te vragen. Deze informatie is beschikbaar zodra u begint met de service, zonder configuratie vereist. Deze pagina wordt vernieuwd om de paar minuten. Als u bij het voltooien van beslissingen over [welke laag moet worden gebruikt voor werkbelastingen voor productie](search-sku-tier.md), of [aanpassen van het aantal actieve replica's en partities](search-capacity-planning.md), deze metrische gegevens kan u helpen bij deze beslissingen komen door die laat zien u hoe snel de resources worden gebruikt en hoe goed de huidige configuratie van de bestaande belasting verwerkt.

De **gebruik** tabblad ziet u de beschikbaarheid van ten opzichte van de huidige [limieten](search-limits-quotas-capacity.md). De volgende afbeelding is voor de gratis service, die beperkt 3-objecten van elk type en 50 MB aan opslagruimte tot wordt. Een Basic- of Standard-service heeft een hogere limieten en als u de aantallen partitie verhoogt, maximumopslag toeneemt proportioneel.

![Status van het ten opzichte van een doeltreffende limieten](./media/search-monitor-usage/usage-tab.png
 "status van het ten opzichte van een doeltreffende limieten")

## <a name="queries-per-second-qps-and-other-metrics"></a>Query's per seconde (QPS) en andere metrische gegevens

De **bewaking** tabblad toont voortschrijdende gemiddelden voor metrische gegevens zoals zoeken *query's Per seconde* (QPS), bij elkaar opgeteld per minuut. 
*Zoeklatentie* is de hoeveelheid tijd de search-service die nodig is voor het verwerken van zoekquery's bij elkaar opgeteld per minuut. *Beperkte zoekquery query percentage* (niet weergegeven), is het percentage van zoekquery's die zijn beperkt, ook bij elkaar opgeteld per minuut.

Deze getallen zijn schattingen en zijn bedoeld om u te bieden u een algemeen beeld van hoe goed uw systeem is onderhoud van aanvragen. Werkelijke QPS is mogelijk hoger of lager is dan het aantal gerapporteerd in de portal.

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
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) | Geregistreerde gebeurtenissen en metrische gegevens voor query's, op basis van de onderstaande, schema's verband houden met de gebruikersgebeurtenissen in uw app. Dit is de enige oplossing die gehouden gebruikersacties of signalen, toewijzing van gebeurtenissen van de gebruiker geïnitieerde zoeken, in plaats van filter aanvragen ingediend door de toepassingscode. Voor het gebruik van deze benadering, kopiëren en plakken instrumentation code in de bronbestanden op aanvraag voor informatie over de route naar Application Insights. Zie voor meer informatie, [Zoekverkeer](search-traffic-analytics.md). |
| [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) | Geregistreerde gebeurtenissen en metrische gegevens voor query's, op basis van de onderstaande schema's. Gebeurtenissen worden geregistreerd in een werkruimte van Log Analytics. U kunt query's uitvoeren op een werkruimte voor gedetailleerde informatie retourneren op het logboek. Zie voor meer informatie, [aan de slag met Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Geregistreerde gebeurtenissen en metrische gegevens voor query's, op basis van de onderstaande schema's. Gebeurtenissen worden vastgelegd in een Blob-container en opgeslagen in de JSON-bestanden. Een JSON-editor gebruiken om de inhoud van bestand weer te geven.|
| [Event Hub](https://docs.microsoft.com/azure/event-hubs/) | Geregistreerde gebeurtenissen en metrische gegevens voor query's, op basis van de schema's beschreven in dit artikel. Kies deze optie als een alternatieve data collection-Services voor zeer grote logboeken. |

Log Analytics- en Blob-opslag zijn beschikbaar als een gratis gedeelde service zodat u dit zonder kosten voor de levensduur van uw Azure-abonnement uitproberen kunt. Application Insights is gratis te registreren en gebruiken, zolang de grootte van toepassing onder bepaalde limieten is (Zie de [pagina met prijzen](https://azure.microsoft.com/ricing/details/monitor/) voor meer informatie).

De volgende sectie helpt u door de stappen voor het inschakelen en gebruiken van Azure Blob-opslag voor het verzamelen en toegang krijgen tot logboekgegevens die zijn gemaakt door Azure Search-bewerkingen.

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Logboekregistratie voor indexeren en query-werkbelastingen is standaard uitgeschakeld en is afhankelijk van aanvullende oplossingen voor infrastructuur voor logboekregistratie en externe langetermijnopslag. Zelfstandig gebruikt zijn de enige persistente gegevens in Azure Search de objecten maakt en beheert, zodat logboeken ergens anders moeten worden opgeslagen.

In deze sectie leert u hoe u Blob storage gebruiken voor het opslaan van gegevens in het logboek gebeurtenissen en metrische gegevens.

1. [Maak een opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) als u er nog geen hebt. U kunt deze plaatsen in dezelfde resourcegroep bevinden als de Azure Search te vereenvoudigen opschonen later als u wilt verwijderen van alle resources die in deze oefening gebruikt.

2. Open de pagina overzicht van uw zoekservice. In het deelvenster navigatie aan de linkerkant, schuif omlaag naar **bewaking** en klikt u op **bewaking inschakelen**.

   ![Schakel de bewaking](./media/search-monitor-usage/enable-monitoring.png "bewaking inschakelen")

3. Kies de gegevens die u wilt exporteren: Logboeken, metrische gegevens of beide. U kunt kopiëren naar een opslagaccount, verzenden naar een event hub of exporteren naar Log Analytics.

   Voor archivering naar Blob-opslag, alleen de storage-account moet bestaan. Containers en blobs wordt gemaakt tijdens het exporteren van gegevens aan het logboek.

   ![Configure blob-opslag archiveren](./media/search-monitor-usage/configure-blob-storage-archive.png "configureren blob-opslag archiveren")

4. Sla het profiel.

5. Test registratie door te maken of verwijderen van objecten (maakt gebeurtenissen vastleggen in logboek) en door het verzenden van query's (genereert metrische gegevens). 

Logboekregistratie is ingeschakeld wanneer u het profiel opslaan. Containers worden alleen gemaakt wanneer er een activiteit of meting. Wanneer de gegevens worden gekopieerd naar een opslagaccount, worden de gegevens zijn opgemaakt als JSON en in twee containers geplaatst:

* Insights-logs-operationlogs: voor zoeken in logboeken over webverkeer
* Insights-metrics-pt1m: voor metrische gegevens

Het duurt een uur voordat de containers wordt weergegeven in de Blob-opslag. Er is een blob, per uur, per container. 

U kunt [Visual Studio Code](#Download-and-open-in-Visual-Studio-Code) of een andere JSON-editor om de bestanden weer te geven. 

### <a name="example-path"></a>Van voorbeeldpad

```
resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2018/m=12/d=25/h=01/m=00/name=PT1H.json
```

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

## <a name="download-and-open-in-visual-studio-code"></a>Downloaden en openen in Visual Studio Code

U kunt elke JSON-editor gebruiken om het logboekbestand weer te geven. Als u niet hebt, raden we [Visual Studio Code](https://code.visualstudio.com/download).

1. Open uw Storage-account in Azure portal. 

2. Klik in het deelvenster navigatie aan de linkerkant **Blobs**. U ziet **insights-logs-operationlogs** en **insights-metrics-pt1m**. Deze containers worden gemaakt door Azure Search tijdens het exporteren van de logboekgegevens naar Blob-opslag.

3. Klik op in de mappenhiërarchie totdat u het .json-bestand.  Het contextmenu gebruiken om het bestand te downloaden.

Zodra het bestand is gedownload, opent u het in een JSON-editor om de inhoud weer te geven.

## <a name="use-system-apis"></a>Systeem-API's gebruiken
Zowel de Azure Search REST-API en de .NET SDK bieden programmatisch toegang tot metrische gegevens, index en indexeerfunctie servicegegevens en document telt.

* [Statistieken over Services ophalen](/rest/api/searchservice/get-service-statistics)
* [Indexstatistieken ophalen](/rest/api/searchservice/get-index-statistics)
* [Aantal documenten](/rest/api/searchservice/count-documents)
* [Status van de indexeerfunctie ophalen](/rest/api/searchservice/get-indexer-status)

Zie de documentatie om in te schakelen met behulp van PowerShell of Azure CLI, [hier](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

## <a name="next-steps"></a>Volgende stappen

[Uw zoekservice op Microsoft Azure beheren](search-manage.md) voor meer informatie over servicebeheer en [prestaties en optimalisatie](search-performance-optimization.md) voor het afstemmen van de richtlijnen.