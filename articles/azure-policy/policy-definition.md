---
title: Structuur van Azure-beleidsdefinities
description: Hierin wordt beschreven hoe resourcedefinitie beleid door het beleid voor Azure wordt gebruikt voor het tot stand brengen conventies voor resources in uw organisatie door te beschrijven wanneer het beleid wordt afgedwongen en welk effect te laten worden.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 1937792290d973f3aee7fa3c0714f4667c21e79a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="azure-policy-definition-structure"></a>Structuur van Azure-beleidsdefinities

Beleid resourcedefinitie gebruikt door het beleid van Azure kunt u conventies voor resources in uw organisatie tot stand brengen door te beschrijven wanneer het beleid wordt afgedwongen en welk effect te laten worden. Door het definiëren van conventies u kosten kunt beheren en meer eenvoudig beheren van uw resources. U kunt bijvoorbeeld opgeven dat alleen bepaalde typen virtuele machines zijn toegestaan. Of u kunt vereisen dat alle resources een bepaald label hebben. Beleidsregels worden overgenomen door alle onderliggende resources. Dus als een beleid wordt toegepast op een resourcegroep, is van toepassing op alle resources in die resourcegroep.

Het schema dat wordt gebruikt door het beleid voor Azure vindt u hier: [https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json)

JSON kunt u een beleidsdefinitie maken. De beleidsdefinitie bevat-elementen voor:

- modus
- parameters
- Weergavenaam
- description
- Beleidsregel
  - logische evaluatie
  - effect

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

De **modus** bepaalt welke resourcetypen wordt geëvalueerd voor een beleid. De ondersteunde modi zijn:

- `all`: resourcegroepen en alle brontypen evalueren
- `indexed`: alleen evalueren brontypen die ondersteuning bieden voor labels en locatie

We raden u aan **modus** naar `all` in de meeste gevallen. Alle beleidsdefinities gemaakt via de portal de `all` modus. Als u PowerShell of Azure CLI gebruiken, kunt u de **modus** parameter handmatig. Als de beleidsdefinitie geen bevat een **modus** waarde wordt standaard ingesteld op `all` in Azure PowerShell en zo de `null` in Azure CLI die gelijk is aan `indexed`, voor achterwaartse compatibiliteit.

`indexed` moet worden gebruikt wanneer u beleidsregels maken die wordt afgedwongen tags of locaties. Dit is niet vereist, maar kan resources die geen ondersteuning bieden voor tags en locaties niet worden weergegeven als niet-compatibel in de nalevingsresultaten van de. De enige uitzondering hierop vormt **resourcegroepen**. Beleidsregels die u probeert om af te dwingen locatie of labels van een resourcegroep moeten ingesteld **modus** naar `all` en specifiek doel van de `Microsoft.Resources/subscriptions/resourceGroup` type. Zie voor een voorbeeld [afdwingen resource group-tags](scripts/enforce-tag-rg.md).

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

In de metagegevenseigenschap kunt u **strongType** om te voorzien in een meervoudige selectie lijst van opties in de Azure-portal.  Toegestane waarden voor **strongType** momenteel omvatten:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

In de beleidsregel verwijzen naar parameters met de volgende syntaxis:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Definitielocatie

Tijdens het maken van de definitie van een beleid of het initiatief is het belangrijk dat u de definitie-locatie opgeeft.

De definitie-locatie bepaalt het bereik waarvoor de definitie initiatief of beleid kan worden toegewezen aan. De locatie kan worden opgegeven als een beheergroep of een abonnement.

> [!NOTE]
> Als u van plan bent de beleidsdefinitie voor dit toepassen op meerdere abonnementen, is de locatie moet een beheergroep met de abonnementen die u, het initiatief of het beleid toewijzen wilt.

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
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
    }
}
```

### <a name="logical-operators"></a>Logische operators

Ondersteunde logische operators zijn:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

De **niet** syntaxis keert het resultaat van de voorwaarde. De **zet** syntaxis (vergelijkbaar met de logische **en** bewerking) moet u alle voorwaarden wordt voldaan. De **dragen** syntaxis (vergelijkbaar met de logische **of** bewerking) vereist een of meer voorwaarden wordt voldaan.

U kunt logische operators nesten. Het volgende voorbeeld wordt een **niet** bewerking die is genest binnen een **zet** bewerking.

```json
"if": {
    "allOf": [{
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

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"notMatch": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"exists": "bool"`

Wanneer u de **zoals** en **notLike** voorwaarden, kunt u een jokerteken (*) in de waarde opgeven.

Wanneer u de **overeen met** en **notMatch** voorwaarden bieden `#` vertegenwoordigt een cijfer `?` voor een letter en een ander teken dat werkelijke teken vertegenwoordigt. Zie voor voorbeelden [toestaan meerdere bestandsnaampatronen](scripts/allow-multiple-name-patterns.md).

### <a name="fields"></a>Velden

Voorwaarden zijn samengesteld op basis van velden. Een veld geeft eigenschappen in de nettolading van de resource-aanvraag die wordt gebruikt om de status van de resource te beschrijven.  

De volgende velden worden ondersteund:

- `name`
- `fullName`
  - Retourneert de volledige naam van de bron, met inbegrip van eventuele bovenliggende items (bijvoorbeeld ' MijnServer/MijnDatabase')
- `kind`
- `type`
- `location`
- `tags`
- `tags.tagName`
- `tags[tagName]`
  - Deze syntaxis accolade ondersteunt labelnamen die punten bevatten
- eigenschap aliassen - Zie voor een lijst [aliassen](#aliases).

### <a name="alternative-accessors"></a>Alternatieve Accessors

**Veld** is de primaire accessor in beleidsregels gebruikt. Deze inspecteert rechtstreeks de resource die wordt geëvalueerd. Beleid ondersteunt echter een andere accessor **bron**.

```json
"source": "action",
"equals": "Microsoft.Compute/virtualMachines/write"
```

**Bron** biedt alleen ondersteuning voor één waarde **actie**. Actie retourneert de autorisatie-actie van de aanvraag die wordt geëvalueerd. Autorisatie acties beschikbaar worden gesteld in de sectie autorisatie van de [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

Als u beleid evalueert bestaande resources op de achtergrond, wordt **actie** naar een `/write` autorisatie-actie op het type van de resource.

### <a name="effect"></a>Effect

Beleid ondersteunt de volgende typen van kracht:

- **Weigeren**: genereert u een gebeurtenis in het controlelogboek en de aanvraag is mislukt
- **Audit**: genereert een waarschuwingsgebeurtenis in controlelogboek maar mislukt de aanvraag niet
- **Toevoeg-**: voegt de gedefinieerde set velden toe aan de aanvraag
- **AuditIfNotExists**: kunt controleren als een resource niet bestaat
- **DeployIfNotExists**: implementeert een resource als deze niet al bestaat. Hiertoe wordt momenteel alleen ondersteund via het ingebouwde beleid.

Voor **toevoegen**, moet u de volgende details opgeven:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

De waarde kan niet een tekenreeks of een object van JSON-indeling.

Met **AuditIfNotExists** en **DeployIfNotExists** kunt u het bestaan van gerelateerde bron evalueren en toepassen van een regel en een bijbehorende effect wanneer die resource niet bestaat. U kunt bijvoorbeeld vereisen dat een netwerk-watcher wordt geïmplementeerd voor alle virtuele netwerken.
Zie voor een voorbeeld van het controle-wanneer de extensie van een virtuele machine niet is geïmplementeerd, [controleren als de extensie is niet opgenomen](scripts/audit-ext-not-exist.md).

## <a name="aliases"></a>Aliassen

U eigenschap aliassen gebruiken voor toegang tot specifieke eigenschappen voor een resourcetype. Aliassen kunnen u beperken welke waarden of de voorwaarden zijn toegestaan voor een eigenschap van een resource. Elke alias wordt toegewezen aan paden in verschillende API-versies voor een bepaald brontype. Tijdens de evaluatie van het beleid haalt de beleidsengine het eigenschapspad voor die API-versie.

De lijst met aliassen groeit altijd. Om te detecteren welke aliassen worden momenteel ondersteund door Azure-beleid, moet u een van de volgende methoden gebruiken:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  $azContext = Get-AzureRmContext
  $azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
  $profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
  $token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
  $authHeader = @{
      'Content-Type'='application/json'
      'Authorization'='Bearer ' + $token.AccessToken
  }

  # Invoke the REST API
  $response = Invoke-RestMethod -Uri 'https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases' -Method Get -Headers $authHeader

  # Create an Array List to hold discovered aliases
  $aliases = New-Object System.Collections.ArrayList

  foreach ($ns in $response.value) {
      foreach ($rT in $ns.resourceTypes) {
          if ($rT.aliases) {
              foreach ($obj in $rT.aliases) {
                  $alias = [PSCustomObject]@{
                      Namespace       = $ns.namespace
                      resourceType    = $rT.resourceType
                      alias           = $obj.name
                  }
                  $aliases.Add($alias) | Out-Null
              }
          }
      }
  }

  # Output the list, sort, and format. You can customize with Where-Object to limit as desired.
  $aliases | Sort-Object -Property Namespace, resourceType, alias | Format-Table
  ```

- Azure-CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # Get Azure Policy aliases for a specific Namespace
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST-API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

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
        "policyDefinitions": [{
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