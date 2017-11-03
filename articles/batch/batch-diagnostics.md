---
title: Diagnostische logboekregistratie voor gebeurtenissen van de Batch - Azure inschakelen | Microsoft Docs
description: Registreren en analyseren van diagnostische gebeurtenissen voor Azure Batch-account resources zoals pools en taken.
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7bc6fd9921ab0f2374ace33ea5c1ab93a78f860
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a>Gebeurtenissen voor diagnostische evaluatie en bewaking van Batch-oplossingen

Net als bij veel Azure-services, verzendt de Batch-service logboekgebeurtenissen voor bepaalde bronnen gedurende de levensduur van de resource. U kunt diagnostische logboeken van de Azure Batch record gebeurtenissen voor resources zoals pools en taken wilt inschakelen en vervolgens gebruikt u de logboeken voor diagnostische evaluatie en bewaking. Gebeurtenissen, zoals toepassingen maken, verwijderen van de groep van toepassingen, taak start, taak voltooid en anderen zijn opgenomen in Batch diagnostische logboeken.

> [!NOTE]
> Dit artikel wordt beschreven logboekgebeurtenissen voor resources van de Batch-account zelf, geen taken en uitvoergegevens van de taak. Zie voor meer informatie over het opslaan van de uitvoergegevens van uw jobs en taken [behouden Azure Batch-taak en uitvoer](batch-task-output.md).
> 
> 

## <a name="prerequisites"></a>Vereisten
* [Azure Batch-account](batch-account-create-portal.md)
* [Azure Storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  Om te blijven behouden diagnostische logboeken van Batch, moet u een Azure Storage-account waarin de logboeken worden opgeslagen in Azure maken. U dit opslagaccount opgeven wanneer u [Diagnostische logboekregistratie inschakelen](#enable-diagnostic-logging) voor uw Batch-account. Het opslagaccount dat u opgeeft wanneer het inschakelen van logboekverzameling is niet hetzelfde zijn als een gekoppeld opslagaccount verwezen in de [toepassingspakketten](batch-application-packages.md) en [taak uitvoer persistentie](batch-task-output.md) artikelen.
  
  > [!WARNING]
  > U bent **in rekening gebracht** voor gegevens die zijn opgeslagen in uw Azure Storage-account. Dit omvat de logboeken met diagnostische gegevens die in dit artikel wordt besproken. Houd er rekening mee houden bij het ontwerpen van uw [Meld bewaarbeleid](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).
  > 
  > 

## <a name="enable-diagnostic-logging"></a>Bijhouden van diagnostische gegevens inschakelen
Diagnostische logboekregistratie is niet standaard ingeschakeld voor uw Batch-account. Diagnostische logboekregistratie voor elke Batch-account dat u wilt bewaken, moet u expliciet inschakelen:

[Het inschakelen van verzamelen van diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

Het is raadzaam dat u de volledige leest [overzicht van Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel om beter inzicht niet alleen het inschakelen van logboekregistratie, maar de logboek-categorieën ondersteund door de verschillende Azure-services. Azure Batch ondersteunt bijvoorbeeld momenteel één logboek categorie: **servicelogboeken**.

## <a name="service-logs"></a>Service-Logboeken
Azure Batch-Service-logboeken bevatten gebeurtenissen die door de Azure Batch-service tijdens de levensduur van een Batch-bron, zoals een groep of de taak. Elke gebeurtenis verzonden door de Batch wordt opgeslagen in het opgegeven opslagaccount in JSON-indeling. Dit is bijvoorbeeld de hoofdtekst van een steekproef **groep maken gebeurtenis**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Hoofdtekst van de gebeurtenis bevindt zich in een .json-bestand in het opgegeven Azure Storage-account. Als u rechtstreeks toegang hebben tot de logboeken wilt, kunt u desgewenst om te controleren de [schema van diagnostische logboeken in het opslagaccount](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Service-logboekgebeurtenissen
De Batch-service verzendt momenteel de volgende Service gebeurtenissen. Deze lijst zijn mogelijk niet volledig, omdat er aanvullende gebeurtenissen zijn toegevoegd sinds dit artikel voor het laatst is bijgewerkt.

| **Service-logboekgebeurtenissen** |
| --- |
| [Groep maken][pool_create] |
| [Toepassingen verwijderen starten][pool_delete_start] |
| [Groep verwijderen is voltooid][pool_delete_complete] |
| [Groep formaat starten][pool_resize_start] |
| [Groep formaat voltooid][pool_resize_complete] |
| [Taak starten][task_start] |
| [De taak is voltooid][task_complete] |
| [Taak is mislukt][task_fail] |

## <a name="next-steps"></a>Volgende stappen
Naast de diagnostische gebeurtenissen in een Azure Storage-account op te slaan, kunt u ook Batch-Service logboekgebeurtenissen streamen een [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), en ze verzenden [Azure Log Analytics](../log-analytics/log-analytics-overview.md).

* [Stream Azure logboeken met diagnostische gegevens naar Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  Stream Batch diagnostische gebeurtenissen naar de service voor inkomende gegevens zeer schaalbaar, Event Hubs. Event Hubs kunnen miljoenen gebeurtenissen per seconde die u kunt vervolgens transformeren en opslaan met behulp van een realtime-analyseprovider opnemen.
* [Azure diagnostische logboeken met logboekanalyse analyseren](../log-analytics/log-analytics-azure-storage.md)
  
  De diagnostische logboeken verzenden met Log Analytics kunt u ze in de portal Operations Management Suite (OMS) te analyseren, of ze te exporteren voor analyse in Power BI of Excel.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
