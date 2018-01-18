---
title: Definitie van Azure beleidsstructuur | Microsoft Docs
description: Hierin wordt beschreven hoe resourcedefinitie beleid door het beleid voor Azure wordt gebruikt voor het tot stand brengen conventies voor resources in uw organisatie door te beschrijven wanneer het beleid wordt afgedwongen en welke actie moet worden uitgevoerd.
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/17/2018
ms.topic: article
ms.service: azure-policy
ms.custom: 
ms.openlocfilehash: af373e2770ad020b3a3eb669424c001670ec9204
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-policy-definition-structure"></a>Structuur van Azure-beleidsdefinities

Beleid resourcedefinitie gebruikt door het beleid van Azure kunt u conventies voor resources in uw organisatie tot stand brengen door te beschrijven wanneer het beleid wordt afgedwongen en welke actie moet worden uitgevoerd. Door het definiëren van conventies u kosten kunt beheren en meer eenvoudig beheren van uw resources. U kunt bijvoorbeeld opgeven dat alleen bepaalde typen virtuele machines zijn toegestaan. Of u kunt vereisen dat alle resources een bepaald label hebben. Beleidsregels worden overgenomen door alle onderliggende resources. Dus als een beleid wordt toegepast op een resourcegroep, is van toepassing op alle resources in die resourcegroep.

JSON kunt u een beleidsdefinitie maken. De beleidsdefinitie bevat-elementen voor:

* modus
* parameters
* Weergavenaam
* description
* Beleidsregel
  * logische evaluatie
  * effect

De volgende JSON ziet u bijvoorbeeld een beleid dat wordt beperkt welke resources zijn geïmplementeerd:

```json
{
  "properties": {
    "mode": "all",
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

Alle Azure-beleid sjabloon voorbeelden zijn op [sjablonen voor Azure beleid](json-samples.md).

## <a name="mode"></a>Modus

We raden u aan `mode` naar `all` om een beleid evalueren toewijzing alle resourcegroepen en typen. U ziet een voorbeeld van een beleidsdefinitie die wordt afgedwongen labels voor een resourcegroep op [toestaan aangepaste VM-installatiekopie van een resourcegroep](scripts/allow-custom-vm-image.md).

Als u deze instelt op **alle**, resourcegroepen en alle brontypen worden geëvalueerd voor het beleid. De portal maakt gebruik van **alle** voor elk beleid. Als u PowerShell of Azure CLI gebruiken, moet u opgeven de `mode` parameter en wordt ingesteld op **alle**.

Alle beleidsregels die zijn gemaakt met behulp van de portal gebruiken een `all` modus, maar als u gebruiken van PowerShell of Azure CLI wilt, moet u opgeven de `mode` parameter en wordt ingesteld op `all`.

Als u de modus instelt op `indexed`, de toewijzing van beleid wordt geëvalueerd alleen brontypen die ondersteuning bieden voor labels en locatie.


## <a name="parameters"></a>Parameters

Parameters uw voor het beleidsbeheer te vereenvoudigen doordat het aantal beleidsdefinities. Bedenk parameters als de velden op een formulier – `name`, `address`, `city`, `state`. Deze parameters blijven altijd hetzelfde, maar de waarden ervan wijzigen op basis van het afzonderlijke invullen van het formulier. Parameters op dezelfde manier werken tijdens het bouwen van beleid. Door parameters in de beleidsdefinitie van een, kunt u dit beleid voor verschillende scenario's met behulp van verschillende waarden hergebruiken.

U kunt bijvoorbeeld een beleid voor een broneigenschap te beperken van de locaties waar resources kunnen worden geïmplementeerd definiëren. In dit geval zou u de volgende parameters declareren bij het maken van uw beleid:


```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations",
      "strongType": "location"
    }
  }
}
```

Het type van een parameter kan tekenreeks- of matrixtype zijn. De metagegevenseigenschap wordt gebruikt voor hulpprogramma's zoals de Azure-portal om gebruiksvriendelijke informatie weer te geven.

U kunt gebruiken in de van metagegevenseigenschap **strongType** om te voorzien in een meervoudige selectie lijst van opties in de Azure-portal.  Toegestane waarden voor **strongType** momenteel omvatten:

* `"location"`
* `"resourceTypes"`
* `"storageSkus"`
* `"vmSKUs"`

In de beleidsregel verwijzen naar parameters met de volgende syntaxis:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Weergavenaam en beschrijving

U kunt **displayName** en **beschrijving** om te bepalen van de beleidsdefinitie en voorzien in context wanneer deze wordt gebruikt.

## <a name="policy-rule"></a>Beleidsregel

De beleidsregel bestaat uit **als** en **vervolgens** blokken. In de **als** blok definiëren van een of meer voorwaarden die opgeven wanneer het beleid wordt afgedwongen. U kunt logische operators toepassen op deze voorwaarden precies bepalen het scenario voor een beleid.

In de **vervolgens** blok, definieert u het effect dat gebeurt wanneer de **als** voorwaarden is voldaan.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Logische operators

Ondersteunde logische operators zijn:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

De **niet** syntaxis keert het resultaat van de voorwaarde. De **zet** syntaxis (vergelijkbaar met de logische **en** bewerking) moet u alle voorwaarden wordt voldaan. De **dragen** syntaxis (vergelijkbaar met de logische **of** bewerking) vereist een of meer voorwaarden wordt voldaan.

U kunt logische operators nesten. Het volgende voorbeeld wordt een **niet** bewerking die is genest binnen een **zet** bewerking.

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Voorwaarden

Een voorwaarde wordt geëvalueerd of een **veld** aan bepaalde criteria voldoet. De ondersteunde voorwaarden zijn:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Wanneer u de **zoals** voorwaarde, kunt u een jokerteken (*) in de waarde opgeven.

Wanneer u de **overeen met** voorwaarde, bieden `#` vertegenwoordigt een cijfer `?` voor een letter en een ander teken dat werkelijke teken vertegenwoordigt. Zie voor voorbeelden [goedgekeurd VM-installatiekopieën](scripts/allowed-custom-images.md).

### <a name="fields"></a>Velden
Voorwaarden zijn samengesteld op basis van velden. Een veld geeft eigenschappen in de nettolading van de resource-aanvraag die wordt gebruikt om de status van de resource te beschrijven.  

De volgende velden worden ondersteund:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*`
* eigenschap aliassen - Zie voor een lijst [aliassen](#aliases).

### <a name="effect"></a>Effect
Beleid ondersteunt de volgende typen van kracht:

* **Weigeren**: genereert u een gebeurtenis in het controlelogboek en de aanvraag is mislukt
* **Audit**: genereert een waarschuwingsgebeurtenis in controlelogboek maar mislukt de aanvraag niet
* **Toevoeg-**: voegt de gedefinieerde set velden toe aan de aanvraag
* **AuditIfNotExists**: kunt controleren als een resource niet bestaat
* **DeployIfNotExists**: implementeert een resource als deze niet al bestaat. Hiertoe wordt momenteel alleen ondersteund via het ingebouwde beleid.

Voor **toevoegen**, moet u de volgende details opgeven:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

De waarde kan niet een tekenreeks of een object van JSON-indeling.

Met **AuditIfNotExists** en **DeployIfNotExists** kunt u het bestaan van een onderliggende resource evalueren en toepassen van een regel en een bijbehorende effect wanneer die resource niet bestaat. U kunt bijvoorbeeld vereisen dat een netwerk-watcher wordt geïmplementeerd voor alle virtuele netwerken.
Zie voor een voorbeeld van het controle-wanneer de extensie van een virtuele machine niet is geïmplementeerd, [controleren als de extensie is niet opgenomen](scripts/audit-ext-not-exist.md).


## <a name="aliases"></a>Aliassen

U eigenschap aliassen gebruiken voor toegang tot specifieke eigenschappen voor een resourcetype. Aliassen kunnen u beperken welke waarden of de voorwaarden zijn toegestaan voor een eigenschap van een resource. Elke alias wordt toegewezen aan paden in verschillende API-versies voor een bepaald brontype. Tijdens de evaluatie van het beleid haalt de beleidsengine het eigenschapspad voor die API-versie.

**Microsoft.Cache/Redis**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Instellen is of de Redis-server niet-ssl-poort (6379) ingeschakeld. |
| Microsoft.Cache/Redis/shardCount | Stel het aantal shards op een Cluster Premium Cache moeten worden gemaakt.  |
| Microsoft.Cache/Redis/sku.capacity | Stel de grootte van de Redis-cache te implementeren.  |
| Microsoft.Cache/Redis/sku.family | Stel de SKU-serie te gebruiken. |
| Microsoft.Cache/Redis/sku.name | Instellen van het type van Redis-Cache te implementeren. |

**Microsoft.Cdn/profiles**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | De naam van de prijscategorie instellen. |

**Microsoft.Compute/disks**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Stel de aanbieding van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imagePublisher | Stel de uitgever van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imageSku | Stel de SKU van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imageVersion | Stel de versie van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |


**Microsoft.Compute/virtualMachines**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Compute/imageId | De id van de afbeelding die wordt gebruikt voor het maken van de virtuele machine instellen. |
| Microsoft.Compute/imageOffer | Stel de aanbieding van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imagePublisher | Stel de uitgever van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imageSku | Stel de SKU van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imageVersion | Stel de versie van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/licenseType | Instellen dat de installatiekopie of schijf gelicentieerde lokaal is. Deze waarde wordt alleen gebruikt voor afbeeldingen die het besturingssysteem Windows Server bevatten.  |
| Microsoft.Compute/virtualMachines/imageOffer | Stel de aanbieding van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/virtualMachines/imagePublisher | Stel de uitgever van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/virtualMachines/imageSku | Stel de SKU van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/virtualMachines/imageVersion | Stel de versie van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Stel de vhd-URI. |
| Microsoft.Compute/virtualMachines/sku.name | Stel de grootte van de virtuele machine. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | De naam van de uitgever de extensie instellen. |
| Microsoft.Compute/virtualMachines/extensions/type | Het type van extensie instellen. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Stel de versie van de uitbreiding. |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Compute/imageId | De id van de afbeelding die wordt gebruikt voor het maken van de virtuele machine instellen. |
| Microsoft.Compute/imageOffer | Stel de aanbieding van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imagePublisher | Stel de uitgever van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imageSku | Stel de SKU van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/imageVersion | Stel de versie van de platforminstallatiekopie van het of de marketplace-installatiekopie gebruikt voor het maken van de virtuele machine. |
| Microsoft.Compute/licenseType | Instellen dat de installatiekopie of schijf gelicentieerde lokaal is. Deze waarde wordt alleen gebruikt voor afbeeldingen die het besturingssysteem Windows Server bevatten. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Het voorvoegsel van de computer voor de virtuele machines in de schaalset instellen. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Stel de blob-URI voor de gebruikersinstallatiekopie van de. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | De container-URL's die worden gebruikt voor het opslaan van besturingssysteem schijven voor de schaalaanpassingsset ingesteld. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | De grootte van virtuele machines in een scale-set instellen. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | De laag van virtuele machines in een scale-set instellen. |

**Microsoft.Network/applicationGateways**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | De grootte van de gateway instellen. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Het type van deze virtuele netwerkgateway instellen. |
| Microsoft.Network/virtualNetworkGateways/sku.name | De naam van de gateway-SKU ingesteld. |

**Microsoft.Sql/servers**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Stel de versie van de server. |

**Microsoft.Sql/databases**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Stel de versie van de database. |
| Microsoft.Sql/servers/databases/elasticPoolName | De naam van de elastische groep die de database is ingesteld. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Stel de geconfigureerde service level objective-ID van de database. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | De naam van de geconfigureerde serviceniveaudoelstelling van de database instellen.  |

**Microsoft.Sql/elasticpools**

| Alias | Beschrijving |
| ----- | ----------- |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Stel de totale gedeelde DTU voor een elastische pool in de database. |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Stel de versie van de elastische groep. |

**Microsoft.Storage/storageAccounts**

| Alias | Beschrijving |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Stel de toegangslaag gebruikt voor facturering. |
| Microsoft.Storage/storageAccounts/accountType | De SKU-naam instellen. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Instellen of de service de gegevens versleutelt die wordt opgeslagen in de blob storage-service. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Instellen of de service de gegevens versleutelt die in de storage-service van het bestand wordt opgeslagen. |
| Microsoft.Storage/storageAccounts/sku.name | De SKU-naam instellen. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Stel in dat alleen https-verkeer met storage-service. |

## <a name="initiatives"></a>Initiatieven

Initiatieven inschakelen u diverse groeperen gerelateerd beleidsdefinities toewijzingen en het beheer vereenvoudigen omdat u met een groep als één item werkt. U kunt bijvoorbeeld alle gerelateerde tagging beleidsdefinities in een enkel initiatief groeperen. In plaats van elk beleid afzonderlijk toewijst, moet u het initiatief toepassen.

Het volgende voorbeeld wordt het maken van een initiatief voor het verwerken van twee tags: `costCenter` en `productName`. Deze twee ingebouwde beleidsregels toe te passen, de standaardwaarde van de tag gebruikt.


```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Volgende stappen

- Bekijk de voorbeelden van de sjabloon Azure beleid op [sjablonen voor Azure beleid](json-samples.md).
