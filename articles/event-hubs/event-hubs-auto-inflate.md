---
title: Automatisch schalen van doorvoereenheden - Azure Event Hubs | Microsoft Docs
description: Schakel automatisch vergroten in een naamruimte doorvoereenheden automatisch geschaald.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: d705993c7cd3816e89da21625dc5b003435b9128
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408152"
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automatisch schalen van Azure Event Hubs-doorvoereenheden
Azure Event Hubs is een uiterst schaalbaar platform voor gegevensstromen. Gebruik van Event Hubs verhoogt daarom vaak na het starten van de service wilt gebruiken. Dergelijk gebruik vereist verhogen van de vooraf ingestelde [doorvoereenheden](event-hubs-features.md#throughput-units) schalen van Event Hubs en grotere standaardtarieven voor gegevensoverdracht worden verwerkt. De **automatisch vergroten** functie van Event Hubs automatisch omhoog wordt geschaald uitgaande door het aantal doorvoereenheden, om te voldoen aan gebruik behoeften. Verhoging van doorvoereenheden wordt voorkomen dat beperkingsscenario's, waarbij:

* Inkomend verkeer gegevenstarieven groter zijn dan set-doorvoereenheden.
* Gegevens uitgaande aanvraag snelheid groter zijn dan de set-doorvoereenheden.

De Event Hubs-service verhoogt de doorvoer wanneer de belasting toeneemt dan de minimale drempelwaarde, zonder dat alle aanvragen waarbij ServerBusy-fouten.

## <a name="how-auto-inflate-works"></a>Automatisch vergroten werking

Verkeer van Event Hubs wordt bepaald door [doorvoereenheden](event-hubs-features.md#throughput-units). Eén doorvoereenheid kunt 1 MB per seconde van inkomend verkeer en twee keer die hoeveelheid uitgaande gegevens. Standard eventhubs kunnen worden geconfigureerd met 1-20 doorvoereenheden. Automatisch vergroten, kunt u met de minimale vereiste doorvoereenheden u klein beginnen. De functie vervolgens automatisch wordt geschaald naar het maximum aantal doorvoereenheden die u nodig, afhankelijk van de toename van uw verkeer hebt. Automatisch vergroten, biedt de volgende voordelen:

- Een efficiënte schaling mechanisme om te klein beginnen en opschalen naarmate u groeit.
- Automatisch schalen aan de opgegeven bovenste limiet zonder beperking van problemen.
- Meer controle over het schalen, omdat u wanneer bepalen en hoe veel op schaal.

## <a name="enable-auto-inflate-on-a-namespace"></a>Automatisch vergroten inschakelen voor een naamruimte

U kunt inschakelen of uitschakelen automatisch vergroten op een Event Hubs-naamruimte met een van de volgende methoden:

- De [Azure-portal](https://portal.azure.com).
- Een [Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate).

### <a name="enable-auto-inflate-through-the-portal"></a>Automatisch vergroten inschakelen via de portal


#### <a name="enable-at-the-time-of-creation"></a>Schakel op het moment van maken 
U kunt de functie voor automatisch vergroten inschakelen **bij het maken van een Event Hubs-naamruimte**:
 
![Inschakelen van automatisch vergroten op tijd event hub maken](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Met deze optie is ingeschakeld, kunt u met uw doorvoereenheden klein beginnen en opschalen toename van de behoeften van uw gebruik. De bovengrens voor inflatie heeft geen direct invloed op prijzen, die afhankelijk is van het aantal throughput units per uur gebruikt.

#### <a name="enable-auto-inflate-for-an-existing-event-hub"></a>Automatisch vergroten inschakelen voor een bestaande event hub
U kunt ook de functie voor automatisch vergroten inschakelen en de instellingen ervan wijzigen met behulp van de volgende instructies: 
 
1. Op de **Event Hubs Namespace** weergeeft, schakelt **uitgeschakelde** onder **doorvoereenheden automatisch vergroten**.  

    ![Throughput units op de pagina van Event Hubs-naamruimte selecteren](./media/event-hubs-auto-inflate/select-throughput-units.png)
2. In de **schaalinstellingen** pagina, selecteert u het selectievakje voor **inschakelen** (als de functie voor automatisch schalen is niet ingeschakeld).

    ![Selecteer inschakelen](./media/event-hubs-auto-inflate/scale-settings.png)
3. Voer de **maximale** aantal doorvoereenheden of gebruik de schuifbalk om in te stellen de waarde. 
4. (optioneel) Update de **minimale** aantal doorvoereenheden boven aan deze pagina. 


> [!NOTE]
> Bij het toepassen van de configuratie om te verhogen doorvoereenheden automatisch vergroten, de Event Hubs-service verzendt diagnostische logboeken waarmee u informatie over waarom en wanneer de doorvoer is verhoogd. Als u Diagnostische logboekregistratie voor een event hub, schakelt **diagnostische instellingen** in het menu links op de Event Hub-pagina in de Azure-portal. Zie voor meer informatie, [instellen van diagnostische logboeken voor een Azure event hub](event-hubs-diagnostic-logs.md). 

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Automatisch vergroten met behulp van een Azure Resource Manager-sjabloon inschakelen

U kunt automatisch vergroten inschakelen tijdens de sjabloonimplementatie van een Azure Resource Manager. Bijvoorbeeld instellen de `isAutoInflateEnabled` eigenschap **waar** en stel `maximumThroughputUnits` tot en met 10. Bijvoorbeeld:

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

Zie voor de volledige sjabloon, de [maken Event Hubs-naamruimte en schakelt u vergroten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) sjabloon op GitHub.


## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)

