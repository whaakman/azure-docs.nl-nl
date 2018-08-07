---
title: Structuur van Azure-beleidsdefinities
description: Hierin wordt beschreven hoe resourcedefinitie beleid wordt gebruikt door Azure Policy tot stand brengen van conventies voor resources in uw organisatie door te beschrijven wanneer het beleid wordt afgedwongen en welk effect te nemen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/03/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: ced8ebad0122973595cdede4497cd200e3090043
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524104"
---
# <a name="azure-policy-definition-structure"></a>Structuur van Azure-beleidsdefinities

Resource-beleidsdefinitie die is gebruikt door Azure Policy kunt u overeenkomsten voor resources in uw organisatie tot stand brengen door te beschrijven wanneer het beleid wordt afgedwongen en welk effect te nemen. Door te definiëren verdragen, kunt u kosten beheren en meer resources eenvoudig beheren. U kunt bijvoorbeeld opgeven dat alleen bepaalde typen virtuele machines zijn toegestaan. Of u kunt vereisen dat alle resources een bepaald label hebben. Beleidsregels worden overgenomen door alle onderliggende resources. Als een beleid wordt toegepast op een resourcegroep, is het dus van toepassing op alle resources in die resourcegroep.

Het schema dat wordt gebruikt door Azure Policy kunt dit hier vinden: [https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json)

JSON kunt u een beleidsdefinitie maken. De beleidsdefinitie bevat elementen voor:

- modus
- parameters
- Weergavenaam
- description
- beleidsregel
  - logische evaluatie
  - effect

De volgende JSON ziet u bijvoorbeeld een beleid dat beperkt welke resources zijn geïmplementeerd:

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

Alle voorbeelden van Azure Policy lopen [beleid voorbeelden](json-samples.md).

## <a name="mode"></a>Modus

De **modus** bepaalt welke resourcetypen voor een beleid wordt geëvalueerd. De ondersteunde modi zijn:

- `all`: resourcegroepen en alle resourcetypen evalueren
- `indexed`: alleen evalueren resourcetypen die ondersteuning bieden voor labels en de locatie

We raden u aan **modus** naar `all` in de meeste gevallen. Alle beleidsdefinities die zijn gemaakt via de portal gebruiken de `all` modus. Als u PowerShell of Azure CLI gebruikt, kunt u opgeven de **modus** parameter handmatig. Als de beleidsdefinitie bevat een **modus** waarde wordt standaard ingesteld op `all` in Azure PowerShell en in het `null` in de Azure CLI, die gelijk is aan `indexed`, voor achterwaartse compatibiliteit.

`indexed` moet worden gebruikt wanneer het beleid te maken die tags of locaties afdwingt. Dit is niet vereist, maar wordt voorkomen dat resources die geen ondersteuning bieden voor labels en locaties worden weergegeven als niet-compatibel in de nalevingsresultaten van de. De enige uitzondering hierop is **resourcegroepen**. Beleidsregels die u probeert om af te dwingen locatie of de tags voor een resourcegroep moeten ingesteld **modus** naar `all` en een specifiek doel de `Microsoft.Resources/subscriptions/resourceGroup` type. Zie voor een voorbeeld [afdwingen groep resourcetags](scripts/enforce-tag-rg.md).

## <a name="parameters"></a>Parameters

Parameters helpen uw beleidsbeheer te vereenvoudigen door het aantal beleidsdefinities te verminderen. Met parameters, zoals de velden in een formulier – `name`, `address`, `city`, `state`. Deze parameters altijd hetzelfde blijven, maar hun waarden worden gewijzigd op basis van het afzonderlijke invullen van het formulier. Parameters werken op dezelfde manier als het samenstellen van beleid. Door parameters te nemen in de beleidsdefinitie van een, kunt u beleid voor verschillende scenario's met behulp van verschillende waarden gebruiken.

U kunt bijvoorbeeld een beleid voor een resource-eigenschap te beperken van de locaties waar resources kunnen worden geïmplementeerd definiëren. In dit geval zou u de volgende parameters declareren wanneer u uw beleid maakt:

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

Het type van een parameter kan worden tekenreeks of matrix. De van de metagegevenseigenschap wordt gebruikt voor hulpprogramma's zoals de Azure-portal om beschrijvende informatie weer te geven.

In de metagegevenseigenschap, kunt u **strongType** voor een multi-keuzelijst met opties in Azure portal.  Toegestane waarden voor **strongType** momenteel opnemen:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

In de beleidsregel, verwijst u naar parameters met de volgende syntaxis:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Definitielocatie

Tijdens het maken van een definitie van een beleid of initiatief, is het belangrijk dat u de locatie van de definitie opgeven.

De locatie van de definitie bepaalt het bereik waarvoor de definitie van het beleid of initiatief kan worden toegewezen aan. De locatie kan worden opgegeven als een beheergroep of een abonnement.

> [!NOTE]
> Als u van plan bent om toe te passen deze beleidsdefinitie bij meerdere abonnementen, is de locatie moet een beheergroep met de abonnementen die u het initiatief of het beleid wilt toewijzen.

## <a name="display-name-and-description"></a>Weergavenaam en beschrijving

U kunt **displayName** en **beschrijving** om te bepalen van de beleidsdefinitie en een context bieden voor wanneer deze wordt gebruikt.

## <a name="policy-rule"></a>Beleidsregel

De beleidsregel bestaat uit **als** en **vervolgens** blokken. In de **als** blok, definieert u een of meer voorwaarden die opgeven wanneer het beleid wordt afgedwongen. U kunt logische operators toepassen op deze voorwaarden voor het definiëren van het scenario voor een beleid nauwkeurig.

In de **vervolgens** blok, definieert u het effect dat gebeurt er wanneer de **als** voorwaarden is voldaan.

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

De **niet** syntaxis keert het resultaat van de voorwaarde. De **allOf** syntaxis (vergelijkbaar met de logische **en** bewerking) vereist dat alle voorwaarden wordt voldaan. De **dragen** syntaxis (vergelijkbaar met de logische **of** bewerking) vereist een of meer voorwaarden wordt voldaan.

U kunt logische operators nesten. Het volgende voorbeeld wordt een **niet** bewerking die is genest in een **allOf** bewerking.

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

Wanneer u de **zoals** en **notlike zijn** voorwaarden, kunt u een jokerteken opgeven `*` in de waarde.
De waarde mag niet meer dan één jokerteken `*`.

Wanneer u de **overeenkomen met** en **notMatch** voorwaarden bieden `#` om weer te geven, een cijfer `?` voor een letter en een ander teken om aan te duiden dat werkelijke teken. Zie voor voorbeelden van [toestaan meerdere bestandsnaampatronen](scripts/allow-multiple-name-patterns.md).

### <a name="fields"></a>Velden

Voorwaarden zijn samengesteld met behulp van velden. Een veld geeft eigenschappen in de nettolading van de resource-aanvraag die wordt gebruikt om te beschrijven van de status van de resource.  

De volgende velden worden ondersteund:

- `name`
- `fullName`
  - Retourneert de volledige naam van de resource. De volledige naam van een resource is de naam van de resource voorafgegaan door een bovenliggende resource-namen (bijvoorbeeld ' MijnServer/MijnDatabase').
- `kind`
- `type`
- `location`
- `tags`
- `tags.<tagName>`
  - Waar **\<tagName\>** is de naam van de code voor het valideren van de voorwaarde voor.
  - Voorbeeld: `tags.CostCenter` waar **kostenplaats** is de naam van de tag.
- `tags[<tagName>]`
  - Deze syntaxis haakje ondersteunt tagnamen die punten bevatten.
  - Waar **\<tagName\>** is de naam van de code voor het valideren van de voorwaarde voor.
  - Voorbeeld: `tags.[Acct.CostCenter]` waar **Acct.CostCenter** is de naam van de tag.
- de eigenschap aliassen - Zie voor een lijst [aliassen](#aliases).

### <a name="effect"></a>Effect

Beleid ondersteunt de volgende typen effect:

- **Weigeren**: genereert een gebeurtenis in het auditlogboek en de aanvraag is mislukt
- **Audit**: een waarschuwingsgebeurtenis in logboek dat wordt gegenereerd, maar mislukt de aanvraag niet
- **Toevoeg-**: voegt de gedefinieerde set velden toe aan de aanvraag
- **AuditIfNotExists**: kunt controleren als een resource niet bestaat
- **DeployIfNotExists**: implementeert een resource als deze niet al bestaat. Dit effect is op dit moment alleen ondersteund via het ingebouwde beleid.

Voor **append**, moet u de volgende gegevens opgeven:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

De waarde kan een tekenreeks of een JSON-indeling-object zijn.

Met **AuditIfNotExists** en **DeployIfNotExists** kunt u het bestaan van een resource gerelateerde evalueren en toepassen van een regel en een bijbehorende effect wanneer deze resource niet bestaat. U kunt bijvoorbeeld vereisen dat een network watcher wordt geïmplementeerd voor alle virtuele netwerken.
Zie voor een voorbeeld van de controle wanneer een VM-extensie niet is geïmplementeerd, [Audit als extensie is niet opgenomen](scripts/audit-ext-not-exist.md).

Zie voor meer informatie over elk effect, de volgorde van de evaluatie, eigenschappen en voorbeelden, [Understanding beleid effecten](policy-effects.md).

## <a name="aliases"></a>Aliassen

U de eigenschap aliassen gebruiken voor toegang tot specifieke eigenschappen voor een resourcetype. Aliassen kunnen u beperken welke waarden of voorwaarden zijn toegestaan voor een eigenschap van een resource. Elke alias wordt toegewezen aan paden in verschillende API-versies voor een gegeven resourcetype. Tijdens de evaluatie van het beleid haalt de beleidsengine het eigenschapspad voor die API-versie.

De lijst met aliassen groeien blijft. Om te detecteren welke aliassen worden momenteel ondersteund door Azure Policy, moet u een van de volgende methoden gebruiken:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  $azContext = Get-AzureRmContext
  $azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
  $profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
  $token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
  $authHeader = @{
    'Authorization'='Bearer ' + $token.AccessToken
  }

  # Create a splatting variable for Invoke-RestMethod
  $invokeRest = @{
    Uri = 'https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases'
    Method = 'Get'
    ContentType = 'application/json'
    Headers = $authHeader
  }

  # Invoke the REST API
  $response = Invoke-RestMethod @invokeRest

  # Create an List to hold discovered aliases
  $aliases = [System.Collections.Generic.List[pscustomobject]]::new()

  foreach ($ns in $response.value)
  {
      foreach ($rT in $ns.resourceTypes)
      {
          if ($rT.aliases)
          {
              foreach ($obj in $rT.aliases)
              {
                  $alias = [PSCustomObject]@{
                      Namespace    = $ns.namespace
                      resourceType = $rT.resourceType
                      alias        = $obj.name
                  }
                  $aliases.Add($alias)
              }
          }
      }
  }

  # Output the list and sort it by Namespace, resourceType and alias. You can customize with Where-Object to limit as desired.
  $aliases | Sort-Object -Property Namespace, resourceType, alias
  ```

- Azure-CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Automation -- Microsoft.Automation)
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST-API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

## <a name="initiatives"></a>Initiatieven

Initiatieven kunnen u verschillende beleidsdefinities toewijzingen en het beheer vereenvoudigen omdat u met een groep als één item werkt verwante groeperen. U kunt bijvoorbeeld alle gerelateerde tagging beleidsdefinities in een enkele initiatief groeperen. In plaats van elk beleid afzonderlijk toewijst, moet u het initiatief toepassen.

Het volgende voorbeeld wordt het maken van een initiatief voor het verwerken van twee labels: `costCenter` en `productName`. Het maakt gebruik van twee ingebouwde beleidsregels om toe te passen van de standaardwaarde van de tag.

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

- Bekijk meer voorbeelden op [Voorbeelden van Azure Policy](json-samples.md).
