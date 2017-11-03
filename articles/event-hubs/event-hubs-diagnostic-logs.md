---
title: Diagnostische logboeken van Azure Event Hubs | Microsoft Docs
description: Informatie over het instellen van diagnostische logboeken voor event hubs in Azure.
keywords: 
documentationcenter: 
services: event-hubs
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: bcc8427d57a001f73d321fbf35c5226a047b68d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="event-hubs-diagnostic-logs"></a>Diagnostische logboeken van Event Hubs

U kunt twee soorten logboeken voor Azure Event Hubs bekijken:
* **[Activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Deze logboeken bevat gegevens over de bewerkingen die worden uitgevoerd op een andere taak. De logboeken zijn altijd ingeschakeld.
* **[Diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. U kunt diagnostische logboeken voor een uitgebreidere weergave van alles wat er gebeurt met een taak configureren. Diagnostische logboeken behandeld activiteiten vanaf het moment dat de taak is gemaakt totdat de taak is verwijderd, inclusief updates en activiteiten die optreden terwijl de taak wordt uitgevoerd.

## <a name="turn-on-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen

Diagnostische logboeken zijn standaard uitgeschakeld. Logboeken met diagnostische gegevens inschakelen:

1.  In de [Azure-portal](https://portal.azure.com)onder **bewaking + Management**, klikt u op **diagnostische logboeken**.

    ![Blade navigatie naar Logboeken met diagnostische gegevens](./media/event-hubs-diagnostic-logs/image1.png)

2.  Klik op de bron die u wilt bewaken.

3.  Klik op **diagnostische gegevens inschakelen**.

    ![Logboeken met diagnostische gegevens inschakelen](./media/event-hubs-diagnostic-logs/image2.png)

4.  Voor **Status**, klikt u op **op**.

    ![Wijzig de status van diagnostische logboeken](./media/event-hubs-diagnostic-logs/image3.png)

5.  Doel voor de archief gewenste; ingesteld bijvoorbeeld, een opslagaccount, een event hub of Azure-logboekanalyse.

6.  De nieuwe instellingen voor diagnostische gegevens opslaan.

Nieuwe instellingen van kracht in ongeveer 10 minuten. Daarna logboeken worden weergegeven in de geconfigureerde archivering doel op de **diagnostische logboeken** blade.

Zie voor meer informatie over het configureren van diagnostische gegevens van de [overzicht van Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-categories"></a>Diagnostische logboeken categorieën
Event Hubs worden vastgelegd diagnostische logboeken voor twee categorieën:

* **ArchiveLogs**: Logboeken specifiek betrekking hebben op Event Hubs-archieven, logboeken die betrekking hebben op fouten te archiveren.
* **OperationalLogs**: informatie over wat er gebeurt tijdens Event Hubs-bewerkingen in het bijzonder de bewerking hebt getypt, waaronder event hub is gemaakt, resources die worden gebruikt en de status van de bewerking.

## <a name="diagnostic-logs-schema"></a>Diagnostische logboeken schema
Alle logboeken worden opgeslagen in de notatie JSON (JavaScript Object)-indeling. Elk item heeft tekenreeksvelden die gebruikmaken van de indeling die wordt beschreven in de volgende secties.

### <a name="archive-logs-schema"></a>Archief logboeken schema

Archief logboek JSON tekenreeksen bevatten elementen in de volgende tabel weergegeven:

Naam | Beschrijving
------- | -------
Taaknaam | Beschrijving van de taak die is mislukt.
ActivityId | Interne ID gebruikt om te worden bijgehouden.
trackingId | Interne ID gebruikt om te worden bijgehouden.
resourceId | Azure Resource Manager resource-ID.
EventHub | Event hub volledige naam (inclusief naamruimtenaam).
partitionId | Event Hub-partitie wordt geschreven.
archiveStep | ArchiveFlushWriter
startTime | De begintijd is mislukt.
fouten | Aantal keren is een fout opgetreden.
durationInSeconds | De duur van de fout.
Bericht | Foutbericht.
category | ArchiveLogs

De volgende code is een voorbeeld van een logboek archiveren JSON-tekenreeks:

```json
{
     "TaskName": "EventHubArchiveUserError",
     "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
     "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
     "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
     "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
     "partitionId": "1",
     "archiveStep": "ArchiveFlushWriter",
     "startTime": "9/22/2016 5:11:21 AM",
     "failures": 3,
     "durationInSeconds": 360,
     "message": "Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (404) Not Found. ---> System.Net.WebException: The remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a>Operationele logboeken schema

Operationeel logboek van JSON tekenreeksen bevatten elementen in de volgende tabel weergegeven:

Naam | Beschrijving
------- | -------
ActivityId | Interne ID gebruikt voor het bijhouden van doel.
EventName | Naam van de bewerking.  
resourceId | Azure Resource Manager resource-ID.
SubscriptionId | Abonnements-ID.
EventTimeString | Bewerkingstijd.
EventProperties | Eigenschappen van de bewerking.
Status | Status van de bewerking.
Caller | De aanroeper van bewerking (Azure portal of management-client).
category | OperationalLogs

De volgende code is een voorbeeld van een operationeel logboek van JSON-tekenreeks:

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create EventHub",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Event Hubs](event-hubs-what-is-event-hubs.md)
* [Event Hubs-API-overzicht](event-hubs-api-overview.md)
* [Aan de slag met Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
