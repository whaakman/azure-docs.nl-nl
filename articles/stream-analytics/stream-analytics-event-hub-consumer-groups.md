---
title: Event Hub-ontvangers in Azure Stream Analytics oplossen
description: Dit artikel wordt beschreven hoe u meerdere consumergroepen voor invoer van Event Hubs in Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: aaa8c4e8d273b44f453d3f63f0be1d4baf980649
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="troubleshoot-event-hub-receivers-in-azure-stream-analytics"></a>Event Hub-ontvangers in Azure Stream Analytics oplossen

U kunt Azure Event Hubs in Azure Stream Analytics opnemen of gegevens uit een taak uitvoeren. Er is een best practice voor het gebruik van Event Hubs meerdere consumergroepen gebruiken om te controleren of de taak schaalbaarheid. Een reden hiervoor is dat het aantal lezers in de Stream Analytics-taak voor een specifieke invoer betrekking heeft op het aantal lezers in een enkel consumergroep. Het exacte aantal ontvangers is gebaseerd op interne implementatiedetails voor de scale-out-topologie-logica. Het aantal ontvangers is extern niet toegankelijk. Het aantal lezers kunt wijzigen op de begintijd van taak of tijdens upgrades van de taak.

De fout wordt weergegeven wanneer het aantal ontvangers overschrijdt het maximum is: `The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Wanneer het aantal lezers gewijzigd tijdens de upgrade van een taak, worden naar het controlelogboeken tijdelijke waarschuwingen geschreven. Stream Analytics-taken worden automatisch herstellen van deze problemen van voorbijgaande aard.

## <a name="add-a-consumer-group-in-event-hubs"></a>Toevoegen van een consumergroep in Event Hubs
Een nieuwe consumergroep in uw exemplaar van Event Hubs toevoegen Volg deze stappen:

1. Meld u aan bij Azure Portal.

2. Ga naar de Event Hubs.

3. Selecteer **Event Hubs** onder de **entiteiten** kop.

4. Selecteer de Event Hub met de naam.

5. Op de **Event Hubs exemplaar** pagina onder de **entiteiten** kop, selecteer **consumergroepen**. Een consumergroep met de naam **$Default** wordt vermeld.

6. Selecteer **+ Consumergroep** toevoegen van een nieuwe consumergroep. 

   ![Toevoegen van een consumergroep in Event Hubs](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Wanneer u de invoer in de Stream Analytics-taak om te verwijzen naar de Event Hub hebt gemaakt, kunt u er de consumergroep opgegeven. $Default wordt gebruikt als niets wordt opgegeven. Wanneer u een nieuwe consumergroep maakt, de Event Hub-invoer in de Stream Analytics-taak bewerken en geef de naam van de nieuwe consumergroep.


## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Het aantal lezers per partitie overschrijdt de limiet van vijf Event Hubs

Als uw streaming querysyntaxis verwijst meerdere keren naar de dezelfde invoerresource Event Hub, kan de engine taak meerdere lezers per query van dezelfde consumergroep kunt gebruiken. Wanneer er te veel verwijzingen naar de dezelfde consumergroep, de taak groter zijn dan de limiet van vijf en een fout opgetreden. In deze omstandigheden kunt u verder met behulp van meerdere invoer uit meerdere consumergroepen met behulp van de oplossing wordt beschreven in de volgende sectie verdelen. 

Scenario's waarin het aantal lezers per partitie de Event Hubs-limiet van vijf overschrijdt omvatten het volgende:

* Meerdere SELECT-instructies: als u meerdere SELECT-instructies die naar verwijzen **dezelfde** event hub-invoer, elke SELECT-instructie zorgt ervoor dat een nieuwe ontvanger worden gemaakt.
* UNION: Wanneer u een UNION, het is mogelijk om meerdere invoerwaarden die naar verwijzen de **dezelfde** event hub- en groep.
* SELF-join: Bij gebruik van een SELF JOIN-bewerking is het mogelijk om te verwijzen naar de **dezelfde** event hub meerdere keren.

## <a name="solution"></a>Oplossing

De volgende aanbevolen procedures verminderen scenario's waarin het aantal lezers per partitie de limiet van vijf voor Event Hubs overschrijdt.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Uw query splitsen in meerdere stappen met behulp van een WITH-component

De component WITH Hiermee geeft u een tijdelijk benoemde resultatenset die door een FROM-component in de query kan worden verwezen. De component WITH wordt gedefinieerd in het bereik van de uitvoering van één SELECT-instructie.

Bijvoorbeeld, in plaats van deze query:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Gebruik deze query:

```
WITH data AS (
   SELECT * FROM inputEventHub
)

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Zorg ervoor dat de invoer aan verschillende groepen consumenten binden

Voor query's waarin drie of meer invoerwaarden zijn verbonden met de dezelfde consumergroep van Event Hubs, afzonderlijke consumergroepen te maken. Dit is vereist voor het maken van aanvullende Stream Analytics-invoer.


## <a name="next-steps"></a>Volgende stappen
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor stream Analytics query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
