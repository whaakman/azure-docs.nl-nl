---
title: Reageren op gebeurtenissen voor Azure SignalR Service
description: Gebruik Azure Event Grid om u te abonneren op gebeurtenissen voor Azure SignalR Service.
services: azure-signalr,event-grid
author: chenyl
ms.author: chenyl
ms.reviewer: zhshang
ms.date: 06/12/2019
ms.topic: conceptual
ms.service: azure-signalr
ms.openlocfilehash: 02f88c5953d499b30f2ea3408318f70a72f42b0f
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789187"
---
# <a name="reacting-to-azure-signalr-service-events"></a>Reageren op gebeurtenissen voor Azure SignalR Service

Azure SignalR Service-gebeurtenissen kunnen toepassingen om te reageren op clientverbindingen verbonden of verbroken met behulp van moderne architecturen zonder servers. Dit gebeurt zonder de noodzaak voor complexe code of kostbaar en inefficiënt polling-services.  In plaats daarvan de gebeurtenissen worden gepusht via [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) voor abonnees zoals [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), of zelfs naar uw eigen aangepaste http-listener en u alleen Betaal voor wat u gebruikt.

Azure SignalR Service-gebeurtenissen worden op betrouwbare wijze verzonden naar de Event Grid-service waarmee u betrouwbare van leveringsservices aan uw toepassingen door middel van uitgebreide opnieuw beleid en de dead-letter uitvoeren voor levering. Zie voor meer informatie, [bezorging van berichten van Event Grid en probeer het opnieuw](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

![Event Grid-Model](https://docs.microsoft.com/azure/event-grid/media/overview/functional-model.png)

## <a name="serverless-state"></a>Serverloze status
Azure SignalR Service gebeurtenissen zijn alleen beschikbaar wanneer clientverbindingen serverloze status hebben. In het algemeen als een client niet naar een hubserver doorsturen, krijgt deze de status van de serverloze. Klassieke modus werken alleen als de hub die verbinding maken-clientverbindingen, beschikt niet over een hubserver. Serverloze modus wordt echter aanbevolen om te voorkomen dat een probleem. Zie voor meer informatie over de servicemodus van de meer [kiezen-servicemodus](https://github.com/Azure/azure-signalr/blob/dev/docs/faq.md#what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose).

## <a name="available-azure-signalr-service-events"></a>Beschikbare Azure SignalR Service-gebeurtenissen
Maakt gebruik van Event grid [gebeurtenisabonnementen](../event-grid/concepts.md#event-subscriptions) gebeurtenis om berichten te routeren voor abonnees. Azure SignalR Service-event-abonnementen bieden ondersteuning voor twee soorten gebeurtenissen:  

|De naam van gebeurtenis|Description|
|----------|-----------|
|`Microsoft.SignalRService.ClientConnectionConnected`|Treedt op wanneer een client is verbonden.|
|`Microsoft.SignalRService.ClientConnectionDisconnected`|Treedt op wanneer een clientverbinding wordt verbroken.|

## <a name="event-schema"></a>Gebeurtenisschema
Azure SignalR Service-gebeurtenissen bevatten alle informatie die u nodig hebt om te reageren op de wijzigingen in uw gegevens. U kunt een Azure SignalR Service gebeurtenis met de eigenschap type gebeurtenis wordt gestart met 'Microsoft.SignalRService' identificeren. Meer informatie over het gebruik van de eigenschappen van Event Grid-gebeurtenis is beschreven op [Event Grid-gebeurtenisschema](../event-grid/event-schema.md).  

Hier volgt een voorbeeld van een client verbinding verbonden gebeurtenis:
```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Zie voor meer informatie, [SignalR-Service het gebeurtenissenschema](../event-grid/event-schema-azure-signalr.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Event Grid en probeer Azure SignalR Service-gebeurtenissen:

> [!div class="nextstepaction"]
> [Probeer een voorbeeld van Event Grid-integratie met Azure SignalR Service](./signalr-howto-event-grid-integration.md)
> [over Event Grid](../event-grid/overview.md)
