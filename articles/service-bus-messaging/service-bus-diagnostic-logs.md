---
title: Diagnostische logboeken van Azure Service Bus | Microsoft Docs
description: Informatie over het instellen van diagnostische logboeken voor Service Bus in Azure.
keywords: 
documentationcenter: .net
services: service-bus-messaging
author: banisadr
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/16/2017
ms.author: sethm
ms.openlocfilehash: 2bf65b7c5b0518da59e767db18fe6f4193e0ab6e
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="service-bus-diagnostic-logs"></a>Diagnostische logboeken van Service Bus

U kunt twee soorten logboeken bekijken voor Azure Service Bus:
* **[Activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Deze logboeken bevatten informatie over de bewerkingen die worden uitgevoerd op een andere taak. De logboeken zijn altijd ingeschakeld.
* **[Diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. U kunt diagnostische logboeken voor uitgebreidere informatie over alles wat er in een job gebeurt kunt configureren. Diagnostische logboeken behandeld activiteiten vanaf het moment dat de taak is gemaakt totdat de taak is verwijderd, inclusief updates en activiteiten die optreden terwijl de taak wordt uitgevoerd.

## <a name="turn-on-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen

Diagnostische logboeken zijn standaard uitgeschakeld. Schakel diagnostische logboeken door de volgende stappen uitvoeren:

1.  In de [Azure-portal](https://portal.azure.com)onder **bewaking + Management**, klikt u op **diagnostische logboeken**.

    ![Blade navigatie naar Logboeken met diagnostische gegevens](./media/service-bus-diagnostic-logs/image1.png)

2. Klik op de bron die u wilt bewaken.  

3.  Klik op **diagnostische gegevens inschakelen**.

    ![Logboeken met diagnostische gegevens inschakelen](./media/service-bus-diagnostic-logs/image2.png)

4.  Voor **Status**, klikt u op **op**.

    ![Diagnostische logboeken status wijzigen](./media/service-bus-diagnostic-logs/image3.png)

5.  Doel voor de archief gewenste; ingesteld bijvoorbeeld, een opslagaccount, een event hub of Azure-logboekanalyse.

6.  De nieuwe instellingen voor diagnostische gegevens opslaan.

Nieuwe instellingen van kracht in ongeveer 10 minuten. Daarna logboeken worden weergegeven in de geconfigureerde archivering doel op de **diagnostische logboeken** blade.

Zie voor meer informatie over het configureren van diagnostische gegevens van de [overzicht van Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-schema"></a>Diagnostische logboeken schema

Alle logboeken worden opgeslagen in de notatie JSON (JavaScript Object)-indeling. Elk item heeft tekenreeksvelden die gebruikmaken van de indeling die wordt beschreven in de volgende sectie.

## <a name="operational-logs-schema"></a>Operationele logboeken schema

Registreert in de **OperationalLogs** categorie leggen wat er gebeurt tijdens de Service Bus-bewerkingen. Deze logboeken vastleggen in het bijzonder het type van de bewerking, waaronder het maken van de wachtrij, resources die worden gebruikt en de status van de bewerking.

Operationeel logboek van JSON tekenreeksen bevatten elementen in de volgende tabel weergegeven:

Naam | Beschrijving
------- | -------
ActivityId | Interne ID, die wordt gebruikt voor bijhouden
EventName | De naam van bewerking           
resourceId | Azure Resource Manager-bron-ID
SubscriptionId | Abonnements-id
EventTimeString | Bewerkingstijd
EventProperties | Eigenschappen van bewerking
Status | De bewerkingsstatus
Caller | De aanroeper van bewerking (Azure portal of management-client)
category | OperationalLogs

Hier volgt een voorbeeld van een operationeel logboek van JSON-tekenreeks:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende koppelingen voor meer informatie over Service Bus:

* [Inleiding tot Servicebus](service-bus-messaging-overview.md)
* [Aan de slag met Service Bus](service-bus-dotnet-get-started-with-queues.md)
