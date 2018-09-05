---
title: Diagnostische logboeken in Azure Service Bus | Microsoft Docs
description: Meer informatie over het instellen van diagnostische logboeken voor Service Bus in Azure.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/05/2018
ms.author: spelluru
ms.openlocfilehash: 3c2528634dea5c75e4a0e35b7e1a6a30de8d96c1
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696153"
---
# <a name="service-bus-diagnostic-logs"></a>Diagnostische logboeken van Service Bus

U kunt twee typen logboeken voor Azure Service Bus weergeven:
* **[Activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Deze logboeken bevatten informatie over de bewerkingen die worden uitgevoerd op een andere taak. De logboeken zijn altijd ingeschakeld.
* **[Diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. U kunt diagnostische logboeken voor uitgebreidere informatie over alles wat er gebeurt in een taak configureren. Diagnostische logboeken voor activiteiten vanaf het moment dat de taak is gemaakt totdat de taak wordt verwijderd, met inbegrip van updates en activiteiten die plaatsvinden terwijl de taak wordt uitgevoerd.

## <a name="turn-on-diagnostic-logs"></a>Logboeken met diagnostische gegevens inschakelen

Diagnostische logboeken zijn standaard uitgeschakeld. Om in te schakelen logboeken met diagnostische gegevens, moet u de volgende stappen uitvoeren:

1.  In de [Azure-portal](https://portal.azure.com)onder **bewaking en beheer**, klikt u op **diagnoselogboeken**.

    ![navigatie in de blade naar Logboeken met diagnostische gegevens](./media/service-bus-diagnostic-logs/image1.png)

2. Klik op de resource die u wilt bewaken.  

3.  Klik op **Diagnostische gegevens inschakelen**.

    ![Logboeken met diagnostische gegevens inschakelen](./media/service-bus-diagnostic-logs/image2.png)

4.  Voor **Status**, klikt u op **op**.

    ![wijzigen van de status van diagnostische logboeken](./media/service-bus-diagnostic-logs/image3.png)

5.  Instellen van het doel van archief dat u wilt. bijvoorbeeld: een storage-account, een event hub of Azure Log Analytics.

6.  De nieuwe instellingen voor diagnostische gegevens opslaan.

Nieuwe instellingen van kracht in ongeveer 10 minuten. Hierna logboeken worden weergegeven in de geconfigureerde archivering doel, op de **diagnoselogboeken** blade.

Zie voor meer informatie over het configureren van diagnostische gegevens over de [overzicht van diagnostische logboeken in Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-schema"></a>Diagnostische logboeken schema

Alle logboeken worden opgeslagen in JavaScript Object Notation (JSON)-indeling. Elk item heeft tekenreeksvelden die gebruikmaken van de indeling die wordt beschreven in de volgende sectie.

## <a name="operational-logs-schema"></a>Operationele logboeken schema

Registreert in de **OperationalLogs** categorie leggen wat er gebeurt tijdens de bewerkingen voor Service Bus. Deze logboeken vastleggen met name het bewerkingstype, met inbegrip van de wachtrij is gemaakt, resources die worden gebruikt en de status van de bewerking.

Operationeel logboek van JSON-tekenreeksen zijn onder andere elementen die worden vermeld in de volgende tabel:

Naam | Beschrijving
------- | -------
ActivityId | Interne ID, die wordt gebruikt voor het bijhouden
EventName | Naam van bewerking           
resourceId | Azure Resource Manager-resource-ID
SubscriptionId | Abonnements-id
EventTimeString | Bewerkingstijd
EventProperties | Eigenschappen van bewerking
Status | Bewerkingsstatus
Caller | De aanroeper van bewerking (Azure portal of de beheer-client)
category | OperationalLogs

Hier volgt een voorbeeld van een operationeel logboek JSON-tekenreeks:

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

Zie de volgende koppelingen voor meer informatie over Service Bus:

* [Inleiding tot Servicebus](service-bus-messaging-overview.md)
* [Aan de slag met Service Bus](service-bus-dotnet-get-started-with-queues.md)
