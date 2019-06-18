---
title: Diagnostische logboeken
titleSuffix: Azure Cognitive Services
description: Deze handleiding bevat stapsgewijze instructies voor het diagnostische logboekregistratie inschakelen voor een Azure Cognitive Service. Deze logboeken bieden uitgebreide, regelmatig gegevens over de werking van een resource die worden gebruikt voor probleem-id en het opsporen van fouten.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.topic: article
ms.date: 06/14/2019
ms.author: erhopf
ms.openlocfilehash: e1a6a44d7ff9d5786388fc47245ef5c79cb9be82
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155729"
---
# <a name="enable-diagnostic-logging-for-azure-cognitive-services"></a>Diagnostische logboekregistratie inschakelen voor Azure Cognitive Services

Deze handleiding bevat stapsgewijze instructies voor het diagnostische logboekregistratie inschakelen voor een Azure Cognitive Service. Deze logboeken bieden uitgebreide, regelmatig gegevens over de werking van een resource die worden gebruikt voor probleem-id en het opsporen van fouten. Voordat u doorgaat, moet u een Azure-account met een abonnement op ten minste één Cognitive Service, zoals hebben [Bing webzoekopdrachten](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/overview), [spraakservices](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview), of [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis).

## <a name="prerequisites"></a>Vereisten

Als u wilt vastleggen van diagnostische gegevens inschakelen, moet u een locatie voor het opslaan van uw logboekgegevens. In deze zelfstudie maakt gebruik van Azure Storage en Log Analytics.

* [Azure-opslag](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) -logboeken met diagnostische gegevens voor controle van beleid, analyse van statische of back-up worden bewaard. Het storage-account heeft geen zich in hetzelfde abonnement als de resource dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC-toegang voor beide abonnementen heeft.
* [Log Analytics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics) -een flexibele log search en analyse hulpprogramma waarmee voor analyse van onbewerkte logboeken die worden gegenereerd door een Azure-resource.

> [!NOTE]
> Extra configuratieopties zijn beschikbaar. Zie voor meer informatie, [verzamelen en gebruiken van logboekgegevens van uw Azure-resources](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-diagnostic-log-collection"></a>Verzamelen van diagnostische logboeken inschakelen  

Laten we beginnen door in te schakelen Diagnostische logboekregistratie met behulp van de Azure portal.

> [!NOTE]
> U schakelt deze functie met behulp van PowerShell of Azure CLI, volg de instructies [verzamelen en gebruiken van logboekgegevens van uw Azure-resources](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

1. Navigeer naar de Azure-portal. Zoek en selecteer vervolgens een Cognitive Services-resource. Uw abonnement naar Bing Web Search.   
2. Zoek vervolgens in het navigatiemenu aan **bewaking** en selecteer **diagnostische instellingen**. Dit scherm bevat alle eerder gemaakte diagnostische instellingen voor deze resource.
3. Als er een eerder gemaakte resource die u wilt gebruiken, kunt u dit nu selecteren. Selecteer anders **+ diagnostische instelling toevoegen**.
4. Voer een naam voor de instelling. Selecteer vervolgens **archiveren naar een opslagaccount** en **verzenden naar log Analytics**.
5. Wanneer u hierom wordt gevraagd om te configureren, selecteert u de storage-account en de OMS-werkruimte die u wilt gebruiken voor het opslaan van logboeken met diagnostische gegevens. **Opmerking**: Als u geen een opslagaccount of de OMS-werkruimte hebt, volgt u de aanwijzingen voor het maken van een.
6. Selecteer **Audit**, **RequestResponse**, en **AllMetrics**. Vervolgens stelt u de bewaarperiode voor uw gegevens diagnostisch logboek. Als een bewaarbeleid is ingesteld op nul, worden gebeurtenissen voor die logboekcategorie voor onbepaalde tijd opgeslagen.
7. Klik op **Opslaan**.

Duurt maximaal twee uur voordat gegevens voor logboekregistratie beschikbaar is voor query's uitvoeren en analyseren. Dus geen zorgen als u iets meteen niet ziet.

## <a name="view-and-export-diagnostic-data-from-azure-storage"></a>Weergeven en diagnostische gegevens exporteren uit Azure Storage

Azure Storage is een oplossing voor robuuste object dat is geoptimaliseerd voor het opslaan van grote hoeveelheden ongestructureerde gegevens. In deze sectie leert u query's uitvoeren voor totaal aantal transacties in uw storage-account gedurende een periode van 30 dagen en de gegevens exporteren naar excel.

1. Ga naar de Azure Storage-resource die u hebt gemaakt in de laatste sectie van de Azure-portal.
2. Zoek in het navigatiemenu aan **bewaking** en selecteer **metrische gegevens**.
3. Gebruik de beschikbare vervolgkeuzelijsten configureren van uw query. In dit voorbeeld gaan we Stel het tijdsbereik voor **afgelopen 30 dagen** en de metriek op **transactie**.
4. Wanneer de query voltooid is, ziet u een visualisatie van transactie in de afgelopen 30 dagen. Om deze gegevens exporteren, gebruikt u de **exporteren naar Excel** knop boven aan de pagina.

Meer informatie over wat u kunt doen met diagnostische gegevens in [Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

## <a name="view-logs-in-log-analytics"></a>Logboeken bekijken in Log Analytics

Volg deze instructies voor het verkennen van log analytics-gegevens voor uw resource.

1. Vanuit de Azure-portal, zoek en selecteer **Log Analytics** in het navigatiemenu aan.
2. Zoek en selecteer de resource die u hebt gemaakt bij het inschakelen van diagnostische gegevens.
3. Onder **algemene**, zoek en selecteer **logboeken**. Op deze pagina kunt u query's uitvoeren op uw Logboeken.

### <a name="sample-queries"></a>Voorbeeldquery's

Hier volgen een paar eenvoudige Kusto-query's die kunt u uw logboekgegevens verkennen.

Voer deze query uit voor alle diagnostische logboeken van Azure Cognitive Services voor een opgegeven periode:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
```

Voer deze query uit om te zien van de 10 meest recente Logboeken:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| take 10
```

Voer deze query uit voor bewerkingen van de groep door **Resource**:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES" |
summarize count() by Resource
```
Deze query om de gemiddelde tijd die nodig is voor het uitvoeren van een bewerking uitvoeren:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize avg(DurationMs)
by OperationName
```

Voer deze query uit om het volume van de bewerkingen na verloop van tijd splitsen op basis van OperationName met aantallen voor elke 10s binned weer te geven.

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize count()
by bin(TimeGenerated, 10s), OperationName
| render areachart kind=unstacked
```

## <a name="next-steps"></a>Volgende stappen

* Lees voor meer informatie over het inschakelen van logboekregistratie en metrische gegevens en logboekbestanden categorieën die worden ondersteund door de verschillende Azure-services, zowel de [overzicht van metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) in Microsoft Azure en [overzicht van Azure diagnostische logboeken ](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview) artikelen.
* Lees deze artikelen voor meer informatie over eventhubs:
  * [Wat is Azure Event Hubs?](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs)
  * [Aan de slag met Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-csharp-ephcs-getstarted)
* Lezen [metrische gegevens en logboeken met diagnostische gegevens downloaden uit Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#download-blobs).
* Lezen [begrijpen zoekopdrachten in Logboeken van Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-new).
