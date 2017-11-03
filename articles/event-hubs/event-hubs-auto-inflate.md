---
title: Automatisch opschalen doorvoereenheden Azure Event Hubs | Microsoft Docs
description: Een naamruimte worden automatisch uitgebreid doorvoereenheden inschakelen automatisch vergroten
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 1cd31e0866ee6088483f88e8f80d01f75764c771
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automatisch opschalen doorvoereenheden Azure Event Hubs

Azure Event Hubs is een uiterst schaalbare gegevens streaming-platform. Event Hubs klanten verhogen als zodanig vaak hun gebruik na het voorbereiden van de service. Dergelijke toeneemt, moet u de vooraf ingestelde doorvoereenheden om te verwerken groter overdrachtssnelheid en schalen van Event Hubs meer. De *automatisch vergroten* functie van Event Hubs schaalt automatisch het aantal doorvoereenheden om te voldoen aan moeten gebruiken. Doorvoereenheden verhogen voorkomt dat scenario's waarin beperking:

* Inkomend gegevenssnelheden overschrijden set doorvoereenheden.
* Gegevenssnelheden uitgaande aanvraag overschrijden set doorvoereenheden.

## <a name="how-auto-inflate-works"></a>Hoe automatisch vergroten werkt

Event Hubs-verkeer wordt geregeld door doorvoereenheden. Eén doorvoereenheid kunt 1 MB per seconde van inkomende en twee keer de omvang van uitgaande gegevens. Standaard Event Hubs kunnen worden geconfigureerd met 1-20 doorvoereenheden. Automatisch vergroten, kunt u met de minimale vereiste doorvoereenheden klein beginnen. De functie wordt vervolgens automatisch geschaald naar het maximum aantal doorvoereenheden die u nodig hebt, afhankelijk van de toename van het verkeer. Automatisch vergroten biedt de volgende voordelen:

- Een efficiënte vergroten/verkleinen mechanisme voor het begin klein en schaal omhoog wanneer u groeit.
- Automatisch schalen in de opgegeven bovengrens zonder problemen beperking.
- Meer controle over het schalen, als u wanneer bepaalt en hoeveel op schaal.

## <a name="enable-auto-inflate-on-a-namespace"></a>Een naamruimte inschakelen automatisch vergroten

U kunt in- of uitschakelen van automatische vergroten voor een naamruimte met een van de volgende methoden:

1. De [Azure-portal](https://portal.azure.com).
2. Een Azure Resource Manager-sjabloon.

### <a name="enable-auto-inflate-through-the-portal"></a>Automatisch vergroten inschakelen via de portal

U kunt de functie automatisch vergroten op een naamruimte inschakelen bij het maken van een Event Hubs-naamruimte:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Met deze optie is ingeschakeld, kunt u begin klein op uw doorvoereenheden en schaal omhoog wanneer gebruik van uw toename behoeften. De bovengrens voor inflatie heeft geen invloed op prijzen die afhankelijk is het aantal doorvoereenheden per uur gebruikt.

U kunt ook inschakelen automatisch vergroten met behulp van de **Scale** optie op de instellingenblade in de portal:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Schakel automatisch vergroten met een Azure Resource Manager-sjabloon

U kunt automatisch vergroten inschakelen tijdens de sjabloonimplementatie van een Azure Resource Manager. Bijvoorbeeld, stelt de `isAutoInflateEnabled` eigenschap **true** en stel `maximumThroughputUnits` tot en met 10.

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Zie voor de volledige sjabloon, het [maken Event Hubs-naamruimte en schakel vergroten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) sjabloon op GitHub.

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
