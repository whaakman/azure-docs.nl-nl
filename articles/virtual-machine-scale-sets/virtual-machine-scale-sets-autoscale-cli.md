---
title: Automatisch schalen virtuele-machineschaalsets met de Azure CLI | Microsoft Docs
description: Het maken van regels voor automatisch schalen voor virtuele-machineschaalset ingesteld met de Azure CLI 2.0
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 8552f6b2723fef2c61d49a34d2d60c2a6c209a32
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Een virtuele-machineschaalset ingesteld met de Azure CLI 2.0 automatisch schalen
Wanneer u een schaalset maakt, definieert u het aantal VM-exemplaren die u wilt uitvoeren. Als uw toepassing vraag verandert, kunt u automatisch vergroten of verkleinen van het aantal VM-exemplaren. De mogelijkheid om te schalen kunt u met de vraag van klanten houden of reageren op wijzigingen in de toepassingsprestaties gedurende de levenscyclus van uw app.

In dit artikel leest u hoe automatisch schalen om regels te maken met de Azure CLI-2.0, die de prestaties van de VM-exemplaren in de schaalset controleren. Deze regels voor automatisch schalen vergroten of verkleinen het aantal VM-exemplaren in reactie op deze maatstaven voor prestaties. U kunt ook deze stappen voltooien met [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) of in de [Azure-portal](virtual-machine-scale-sets-autoscale-portal.md).


## <a name="prerequisites"></a>Vereisten
Voor het maken van regels voor automatisch schalen, moet u een bestaande virtuele machine schaalset. U kunt een instellen met schaal maken de [Azure-portal](virtual-machine-scale-sets-create-portal.md), [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md), of [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md).

Zodat het eenvoudiger te maken van de regels voor automatisch schalen, het definiëren van sommige variabelen voor uw scale set. Het volgende voorbeeld definieert de variabelen voor de set met de naam scale *myScaleSet* in de resourcegroep met de naam *myResourceGroup* en in de *eastus* regio. Uw abonnement ID wordt verkregen met [az account weergeven](/cli/azure/account#az_account_show). Als u meerdere abonnementen die zijn gekoppeld aan uw account hebt, wordt alleen het eerste abonnement geretourneerd. De namen en de abonnements-ID als volgt aanpassen:

```azurecli
sub=$(az account show --query id -o tsv)
resourcegroup_name="myResourceGroup"
scaleset_name="myScaleSet"
location_name="eastus"
```

## <a name="define-an-autoscale-profile"></a>Definieer een profiel voor automatisch schalen
Regels voor automatisch schalen die zijn geïmplementeerd als JSON (JavaScript Object Notation) met de Azure CLI 2.0. Later in dit artikel vindt u de volledige JSON die definieert en implementeert de regels voor automatisch schalen. 

Definieert de standaardwaarde van het begin van het profiel voor automatisch schalen, minimale en maximale schaal capaciteit instellen. Het volgende voorbeeld wordt de standaard- en ten minste capaciteit van *2* VM-exemplaren en maximaal *10*:

```json
{
  "name": "autoscale rules",
  "capacity": {
    "minimum": "2",
    "maximum": "10",
    "default": "2"
  }
}
```


## <a name="create-a-rule-to-automatically-scale-out"></a>Maak een regel automatisch uit te schalen
Als uw toepassing vraag toeneemt stelt de belasting van de VM-exemplaren in uw scale toeneemt. Als deze toegenomen belasting consistent is, in plaats van slechts een korte vraag, kunt u regels voor automatisch schalen voor een verhoging van het aantal VM-exemplaren in de schaalset configureren. Wanneer deze VM-exemplaren zijn gemaakt en uw toepassingen worden geïmplementeerd, wordt de schaalaanpassingsset begint met het distribueren van verkeer via de load balancer. U bepaalt welke metrische gegevens moeten worden bewaakt, zoals CPU of schijf, hoe lang het laden van toepassingen moet voldoen aan een bepaalde drempelwaarde en instellen hoeveel exemplaren van virtuele machine toevoegen aan de schaal.

We gaan een regel maken waarmee het aantal VM-exemplaren in een schaal ingesteld verhoogt wanneer de gemiddelde CPU-belasting groter dan 70% gedurende een periode 10 minuten is. Wanneer de regel wordt geactiveerd, wordt het aantal VM-instanties verhoogd met 20%. In met een klein aantal exemplaren van de VM-schaalsets, stelt u kan de `type` naar *ChangeCount* en het verhogen van de `value` door *1* of *2* exemplaren. In-schaalsets met een groot aantal VM-instanties, een toename van 10% of % 20 VM-instanties mogelijk meer geschikt is.

De volgende parameters worden gebruikt voor deze regel:

| Parameter         | Uitleg                                                                                                         | Waarde           |
|-------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| *metricName*      | De metriek prestaties bewaken en schalen van toepassing instellen acties op.                                                   | Percentage CPU  |
| *timeGrain*       | Hoe vaak de metrische gegevens worden verzameld voor analyse.                                                                   | 1 minuut        |
| *TimeAggregation van* | Hiermee definieert u hoe de verzamelde metrische gegevens moeten worden samengevoegd voor analyse.                                                | Gemiddeld         |
| *waarde voor timeWindow*      | De hoeveelheid tijd gecontroleerd voordat de metrische gegevens en drempelwaarde waarden worden vergeleken.                                   | 10 minuten      |
| *operator*        | De operator is gebruikt voor het vergelijken van de metrische gegevens tegen de drempelwaarde.                                                     | Groter dan    |
| *drempelwaarde*       | De waarde zorgt ervoor dat de regel voor automatisch schalen voor het activeren van een actie.                                                      | 70%             |
| *richting*       | Als de schaalaanpassingsset omhoog of omlaag wanneer de regel van toepassing moet schalen definieert.                                             | Verhogen        |
| *type*            | Hiermee wordt aangegeven dat het aantal exemplaren van de virtuele machine moet worden gewijzigd door een percentage.                                 | Percentage wijzigen  |
| *waarde*           | Hoeveel exemplaren van de virtuele machine moeten worden geschaald omhoog of omlaag wanneer de regel van toepassing is.                                            | 20              |
| *cooldown*        | De hoeveelheid tijd moet worden gewacht voordat de regel wordt opnieuw toegepast zodat de acties voor automatisch schalen die tijd hebt om te laten treden. | 5 minuten       |

Het volgende voorbeeld definieert de regel moet worden uitgebreid met het aantal VM-exemplaren. De *metricResourceUri* gebruikt de variabelen die eerder zijn gedefinieerd voor de abonnements-ID, de naam van een resourcegroep en de schaal naam instellen:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT10M",
    "timeAggregation": "Average",
    "operator": "GreaterThan",
    "threshold": 70
  },
  "scaleAction": {
    "direction": "Increase",
    "type": "PercentChangeCount",
    "value": "20",
    "cooldown": "PT5M"
  }
}
```


## <a name="create-a-rule-to-automatically-scale-in"></a>Maak een regel automatisch schalen in
Op een 's avonds of het weekend en kan uw aanvraag toepassing afnemen. Als deze minder belasting gedurende een periode consistent is, kunt u regels voor automatisch schalen Verminder het aantal VM-exemplaren in de schaalset configureren. Deze actie schaal in reduceert de kosten voor het uitvoeren van uw scale instellen als u alleen het aantal exemplaren is vereist om te voldoen aan de huidige aanvraag uitvoeren.

Maak een andere regel die het aantal VM-exemplaren in een schaal ingesteld vermindert wanneer de gemiddelde CPU-belasting vervolgens onder 30% gedurende een periode 10 minuten zakt. Het volgende voorbeeld definieert de regel moet worden uitgebreid met het aantal VM-exemplaren. De *metricResourceUri* gebruikt de variabelen die eerder zijn gedefinieerd voor de abonnements-ID, de naam van een resourcegroep en de schaal naam instellen:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT10M",
    "timeAggregation": "Average",
    "operator": "LessThan",
    "threshold": 30
  },
  "scaleAction": {
    "direction": "Decrease",
    "type": "PercentChangeCount",
    "value": "20",
    "cooldown": "PT5M"
  }
}
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Automatisch schalen regels toepassen op een schaalset
De laatste stap is het profiel voor automatisch schalen en regels toepassen op uw schaalset. Schaal kan vervolgens automatisch schalen in- of uitzoomen op basis van de vraag van de toepassing. Toepassen van het profiel voor automatisch schalen met [az monitor automatisch schalen-instellingen maken](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) als volgt. De volledige JSON maakt gebruik van het profiel en de regels in de vorige secties hebt genoteerd.

```azurecli
az monitor autoscale-settings create \
    --resource-group myResourceGroup \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "autoscale by percentage based on CPU usage",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT10M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "PercentChangeCount",
                "value": "20",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT10M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "PercentChangeCount",
                "value": "20",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Aantal exemplaren in een set scale bewaken
Het aantal en de status van de VM-exemplaren Geef weer voor een lijst met instanties in uw instellen met schaal [az vmss lijstexemplaren](/cli/azure/vmss#az_vmss_list_instances). De status geeft aan of de VM-instantie is inrichting als de schaal automatisch uitgeschaald of is opheffen van inrichting tijdens de schaal automatisch schalen. De status van het VM-exemplaar voor de schaal set met de naam van het volgende voorbeeld-weergaven *myScaleSet* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```


## <a name="autoscale-based-on-a-schedule"></a>Op basis van een schema voor automatisch schalen
De eerdere voorbeelden automatisch geschaald op een schaal in- of instellen met basic host metrische gegevens zoals CPU-gebruik. U kunt ook regels voor automatisch schalen op basis van schema's maken. Deze regels op basis van een planning kunnen u automatisch schalen het aantal VM-instanties van tevoren een verwachte toename van de vraag van de toepassing, zoals core werkuren, en vervolgens automatisch schalen in het aantal exemplaren op een tijdstip dat u minder verwacht vraag, zoals het weekend.

Maak een JSON-profiel dat het aantal exemplaren van de virtuele machine definieert worden uitgevoerd voor een vaste begin en einde tijdvenster wilt gebruiken op basis van het schema voor automatisch schalen regels. Het volgende voorbeeld definieert u een regel moet worden uitgebreid naar *10* exemplaren op *9* AM elke dag (maandag tot vrijdag).

```json
{
  "name": "Scale out during each work day",
  "capacity": {
      "minimum": "10",
      "maximum": "10",
      "default": "10"
  },
  "rules": [],
  "recurrence": {
      "frequency": "Week",
      "schedule": {
          "timeZone": "Pacific Standard Time",
          "days": [
              "Monday",
              "Tuesday",
              "Wednesday",
              "Thursday",
              "Friday"
          ],
          "hours": [
              9
          ],
          "minutes": [
              0
          ]
      }
  }
}
```

Als u wilt schalen tijdens de avond, een andere regel waarmee een kleiner aantal VM-instanties en een juiste begintijd te maken.

De volgende volledige voorbeeld definieert de regels voor scale-out en klik vervolgens op schaal in en klik vervolgens van toepassing is het profiel voor automatisch schalen met [az monitor automatisch schalen-instellingen maken](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create). In dit voorbeeld worden de regels op basis van meetwaarde voor automatisch schalen die zijn gemaakt in de voorgaande voorbeelden overschreven. De *metricResourceUri* gebruikt de variabelen die eerder zijn gedefinieerd voor de abonnements-ID, de naam van een resourcegroep en de schaal naam instellen:

```azurecli
az monitor autoscale-settings create \
    --resource-group myResourceGroup \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "Scale out during each work day",
          "capacity": {
            "minimum": "10",
            "maximum": "10",
            "default": "10"
          },
          "rules": [],
          "recurrence": {
            "frequency": "Week",
            "schedule": {
              "timeZone": "Pacific Standard Time",
              "days": [
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday"
              ],
              "hours": [
                9
              ],
              "minutes": [
                0
              ]
            }
          }
        },
        {
          "name": "Scale in during the evening",
          "capacity": {
            "minimum": "3",
            "maximum": "3",
            "default": "3"
          },
          "rules": [],
          "recurrence": {
            "frequency": "Week",
            "schedule": {
              "timeZone": "Pacific Standard Time",
              "days": [
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday"
              ],
              "hours": [
                18
              ],
              "minutes": [
                0
              ]
            }
          }
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u regels voor automatisch schalen gebruiken om horizontaal schalen en vergroten of verkleinen de *getal* van VM-exemplaren in uw scale ingesteld. Kunt u ook de schaal verticaal vergroten of verkleinen van de VM-instantie *grootte*. Zie voor meer informatie [verticale automatisch geschaald met virtuele-machineschaalsets](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Zie voor meer informatie over het beheren van uw VM-exemplaren [beheren virtuele-machineschaalsets met Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).

Zie voor meer informatie over hoe waarschuwingen worden gegenereerd wanneer uw automatisch schalen regels trigger, [acties automatisch schalen gebruiken voor het verzenden van e-mail en -webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). U kunt ook [controlelogboeken voor gebruik met het verzenden van e-mail en webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
