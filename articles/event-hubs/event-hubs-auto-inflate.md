---
title: Automatisch opschalen doorvoereenheden Azure Event Hubs | Microsoft Docs
description: Een naamruimte worden automatisch uitgebreid doorvoereenheden inschakelen automatisch vergroten
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 20ee0e6cff2a07cbd62a79799eada5708c7a0f07
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automatisch opschalen doorvoereenheden Azure Event Hubs

Azure Event Hubs is een uiterst schaalbare gegevens streaming-platform. Als zodanig Event Hubs-gebruik vaak neemt toe nadat u de service is gestart. Dergelijk gebruik vereist verhogen van de vooraf ingestelde doorvoereenheden om te verwerken groter overdrachtssnelheid en schalen van Event Hubs. De *automatisch vergroten* functie van Event Hubs schaalt automatisch het aantal doorvoereenheden om te voldoen aan moeten gebruiken. Doorvoereenheden verhogen voorkomt dat scenario's waarin beperking:

* Inkomend gegevenssnelheden overschrijden set doorvoereenheden.
* Gegevenssnelheden uitgaande aanvraag overschrijden set doorvoereenheden.

## <a name="how-auto-inflate-works"></a>Hoe automatisch vergroten werkt

Event Hubs-verkeer wordt geregeld door doorvoereenheden. Eén doorvoereenheid kunt 1 MB per seconde van inkomende en twee keer de omvang van uitgaande gegevens. Standaard event hubs kunnen worden geconfigureerd met 1-20 doorvoereenheden. Automatisch vergroten, kunt u met de minimale vereiste doorvoereenheden klein beginnen. De functie wordt vervolgens automatisch geschaald naar het maximum aantal doorvoereenheden die u nodig hebt, afhankelijk van de toename van het verkeer. Automatisch vergroten biedt de volgende voordelen:

- Een efficiënte vergroten/verkleinen mechanisme voor het begin klein en schaal omhoog wanneer u groeit.
- Automatisch schalen in de opgegeven bovengrens zonder problemen beperking.
- Meer controle over het schalen, als u wanneer bepaalt en hoeveel op schaal.

## <a name="enable-auto-inflate-on-a-namespace"></a>Een naamruimte inschakelen automatisch vergroten

U kunt in- of automatisch vergroten uitschakelen op een naamruimte Event Hubs met behulp van een van de volgende methoden:

1. De [Azure-portal](https://portal.azure.com).
2. Een Azure Resource Manager-sjabloon.

### <a name="enable-auto-inflate-through-the-portal"></a>Automatisch vergroten inschakelen via de portal

Bij het maken van een naamruimte Event Hubs, kunt u de functie automatisch vergroten inschakelen:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Met deze optie is ingeschakeld, kunt u begin klein op uw doorvoereenheden en schaal omhoog wanneer gebruik van uw toename behoeften. De bovengrens voor inflatie niet onmiddellijk voor prijzen die afhankelijk is het aantal doorvoereenheden per uur gebruikt.

U kunt ook inschakelen automatisch vergroten met behulp van de **Scale** optie in het deelvenster instellingen in de portal:
 
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

