---
title: Details van de structuur van beleidsdefinities
description: Hierin wordt beschreven hoe resourcedefinitie beleid wordt gebruikt door Azure Policy tot stand brengen van conventies voor resources in uw organisatie door te beschrijven wanneer het beleid wordt afgedwongen en welk effect te nemen.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/11/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: aa334f88d04bb30ce01fe12fecb3aac3c9cd572d
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237414"
---
# <a name="azure-policy-definition-structure"></a>Structuur van Azure-beleidsdefinities

Resource-beleidsdefinities worden gebruikt door Azure Policy tot stand brengen van conventies voor resources. Elke definitie beschrijft resourcenaleving en wat moet worden uitgevoerd wanneer een resource niet compatibel is zijn doorgevoerd.
Door te definiëren verdragen, kunt u kosten beheren en meer resources eenvoudig beheren. U kunt bijvoorbeeld opgeven dat alleen bepaalde typen virtuele machines zijn toegestaan. Of u kunt vereisen dat alle resources een bepaald label hebben. Beleidsregels worden overgenomen door alle onderliggende resources. Als een beleid wordt toegepast op een resourcegroep, is het van toepassing op alle resources in die resourcegroep.

Het schema dat wordt gebruikt door Azure Policy kunt dit hier vinden: [https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json](https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json)

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
                },
                "defaultValue": "westus2"
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

Alle voorbeelden van Azure Policy lopen [beleid voorbeelden](../samples/index.md).

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="mode"></a>Modus

De **modus** bepaalt welke resourcetypen voor een beleid wordt geëvalueerd. De ondersteunde modi zijn:

- `all`: resourcegroepen en alle resourcetypen evalueren
- `indexed`: alleen evalueren resourcetypen die ondersteuning bieden voor labels en de locatie

We raden u aan **modus** naar `all` in de meeste gevallen. Alle beleidsdefinities die zijn gemaakt via de portal gebruiken de `all` modus. Als u PowerShell of Azure CLI gebruikt, kunt u opgeven de **modus** parameter handmatig. Als de beleidsdefinitie bevat geen een **modus** waarde, wordt standaard `all` in Azure PowerShell en in het `null` in de Azure CLI. Een `null` modus is hetzelfde als wanneer u `indexed` ter ondersteuning van achterwaartse compatibiliteit.

`indexed` moet worden gebruikt bij het maken van beleid dat labels of locaties worden afgedwongen. Hoewel het niet vereist, voorkomt u dat resources die geen ondersteuning bieden voor labels en locaties worden weergegeven als niet-compatibel in de nalevingsresultaten van de. De uitzondering hierop is **resourcegroepen**. Voor die locatie of de tags voor een resourcegroep afdwingen moeten ingesteld **modus** naar `all` en een specifiek doel de `Microsoft.Resources/subscriptions/resourceGroup` type. Zie voor een voorbeeld [afdwingen groep resourcetags](../samples/enforce-tag-rg.md).

## <a name="parameters"></a>Parameters

Parameters helpen uw beleidsbeheer te vereenvoudigen door het aantal beleidsdefinities te verminderen. Met parameters, zoals de velden in een formulier – `name`, `address`, `city`, `state`. Deze parameters altijd hetzelfde blijven, maar hun waarden worden gewijzigd op basis van het afzonderlijke invullen van het formulier.
Parameters werken op dezelfde manier als het samenstellen van beleid. Door parameters te nemen in de beleidsdefinitie van een, kunt u beleid voor verschillende scenario's met behulp van verschillende waarden gebruiken.

> [!NOTE]
> Parameters kunnen worden toegevoegd aan de definitie van een bestaande en toegewezen. De nieuwe parameter moet bevatten de **defaultValue** eigenschap. Dit voorkomt dat bestaande toewijzingen van het beleid of initiatief indirect wordt ongeldig gemaakt.

### <a name="parameter-properties"></a>Eigenschappen van rapportparameter

Een parameter heeft de volgende eigenschappen die worden gebruikt in de beleidsdefinitie:

- **name**: De naam van de parameter. Gebruikt door de `parameters` implementatie-functie binnen de beleidsregel. Zie voor meer informatie, [met behulp van een parameterwaarde](#using-a-parameter-value).
- `type`: Bepaalt of de parameter een **tekenreeks** of een **matrix**.
- `metadata`: Definieert subeigenschappen hoofdzakelijk wordt gebruikt door de Azure portal om beschrijvende informatie weer te geven:
  - `description`: De uitleg van waarvoor de parameter wordt gebruikt. Kan worden gebruikt voor voorbeelden van acceptabele waarden.
  - `displayName`: De beschrijvende naam die wordt weergegeven in de portal voor de parameter.
  - `strongType`: (Optioneel) Gebruikt bij het toewijzen van de beleidsdefinitie via de portal. Geeft een lijst van context-op de hoogte. Zie voor meer informatie, [strongType](#strongtype).
- `defaultValue`: (Optioneel) Hiermee stelt u de waarde van de parameter in een toewijzing, als er geen waarde is opgegeven. Vereist bij het bijwerken van een bestaande beleidsdefinitie die is toegewezen.
- `allowedValues`: (Optioneel) Geeft de lijst met waarden die de parameter tijdens de toewijzing accepteert.

U kunt bijvoorbeeld een beleidsdefinitie voor het beperken van de locaties waar resources kunnen worden geïmplementeerd definiëren. Een parameter voor de beleidsdefinitie van dit kan worden **allowedLocations**. Deze parameter kan worden gebruikt door elke toewijzing van de beleidsdefinitie om te beperken van de geaccepteerde waarden. Het gebruik van **strongType** biedt een verbeterde ervaring bij het voltooien van de toewijzing via de portal:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "westus2",
        "allowedValues": [
            "eastus2",
            "westus2",
            "westus"
        ]
    }
}
```

### <a name="using-a-parameter-value"></a>Met behulp van een parameterwaarde

In de beleidsregel u verwijzen naar parameters met de volgende `parameters` implementatie waarde functiesyntaxis:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

In dit voorbeeld verwijst naar de **allowedLocations** parameter die hebt u gezien hoe in [parametereigenschappen](#parameter-properties).

### <a name="strongtype"></a>strongType

Binnen de `metadata` eigenschap, kunt u **strongType** voor een multi-keuzelijst met opties in Azure portal. Toegestane waarden voor **strongType** momenteel opnemen:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

## <a name="definition-location"></a>Definitielocatie

Tijdens het maken van een initiatief of het beleid, is het nodig zijn om de definitielocatie te geven. De locatie van de definitie moet op een beheergroep of een abonnement. Deze locatie bepaalt het bereik waarvoor de initiatief of het beleid kan worden toegewezen. Resources moet directe leden van of kinderen binnen de hiërarchie van de locatie van de definitie om de doelgroep voor het toewijzen van te zijn.

Als de locatie van de definitie a:

- **Abonnement** - alleen de resources in dat abonnement aan het beleid kunnen worden toegewezen.
- **Beheergroep** - alleen resources in de onderliggende beheergroepen en onderliggende abonnementen kunnen worden toegewezen op het beleid. Als u van plan bent om toe te passen van de beleidsdefinitie voor verschillende abonnementen, is de locatie moet een beheergroep met deze abonnementen.

## <a name="display-name-and-description"></a>Weergavenaam en beschrijving

U gebruikt **displayName** en **beschrijving** om te bepalen van de beleidsdefinitie en een context bieden voor wanneer deze wordt gebruikt. **displayName** heeft een maximale lengte van _128_ tekens en **beschrijving** een maximale lengte van _512_ tekens.

## <a name="policy-rule"></a>Beleidsregel

De beleidsregel bestaat uit **als** en **vervolgens** blokken. In de **als** blok, definieert u een of meer voorwaarden die opgeven wanneer het beleid wordt afgedwongen. U kunt logische operators toepassen op deze voorwaarden voor het definiëren van het scenario voor een beleid nauwkeurig.

In de **vervolgens** blok, definieert u het effect dat gebeurt er wanneer de **als** voorwaarden is voldaan.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists | disabled"
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

Een voorwaarde wordt geëvalueerd of een **veld** of de **waarde** accessor aan bepaalde criteria voldoet. De ondersteunde voorwaarden zijn:

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

Wanneer u de **zoals** en **notlike zijn** voorwaarden bieden u een jokerteken `*` in de waarde.
De waarde mag niet meer dan één jokerteken hebben `*`.

Wanneer u de **overeenkomen met** en **notMatch** voorwaarden bieden `#` zodat deze overeenkomt met een cijfer `?` voor een letter, `.` zodat deze overeenkomt met alle tekens en een ander teken zodat deze overeenkomt met werkelijke teken. Zie voor voorbeelden van [toestaan verschillende bestandsnaampatronen](../samples/allow-multiple-name-patterns.md).

### <a name="fields"></a>Velden

Voorwaarden zijn samengesteld met behulp van velden. Een veld overeenkomt met eigenschappen in de nettolading van de resource-aanvraag en een beschrijving van de status van de resource.

De volgende velden worden ondersteund:

- `name`
- `fullName`
  - Retourneert de volledige naam van de resource. De volledige naam van een resource is de naam van de resource voorafgegaan door een bovenliggende resource-namen (bijvoorbeeld ' MijnServer/MijnDatabase').
- `kind`
- `type`
- `location`
  - Gebruik **globale** voor resources die locatie-onafhankelijke zijn. Zie voor een voorbeeld [voorbeelden - locaties toegestaan](../samples/allowed-locations.md).
- `identity.type`
  - Retourneert het type van [beheerde identiteit](../../../active-directory/managed-identities-azure-resources/overview.md) ingeschakeld op de bron.
- `tags`
- `tags.<tagName>`
  - Waar **\<tagName\>** is de naam van de code voor het valideren van de voorwaarde voor.
  - Voorbeeld: `tags.CostCenter` waar **kostenplaats** is de naam van de tag.
- `tags[<tagName>]`
  - Deze syntaxis haakje ondersteunt tagnamen waarvoor een periode.
  - Waar **\<tagName\>** is de naam van de code voor het valideren van de voorwaarde voor.
  - Voorbeeld: `tags[Acct.CostCenter]` waar **Acct.CostCenter** is de naam van de tag.

- de eigenschap aliassen - Zie voor een lijst [aliassen](#aliases).

### <a name="value"></a>Value

Voorwaarden kunnen ook worden samengesteld met behulp van **waarde**. **waarde** controleert of voorwaarden op basis van [parameters](#parameters), [ondersteund sjabloonfuncties](#policy-functions), of letterlijke waarden.
**waarde** is gekoppeld met een ondersteund [voorwaarde](#conditions).

#### <a name="value-examples"></a>Waarde-voorbeelden

In dit voorbeeld van beleid voor regel maakt gebruik van **waarde** om te vergelijken van het resultaat van de `resourceGroup()` functie en de geretourneerde **naam** eigenschap in op een **zoals** voorwaarde van `*netrg`. De regel weigert een resource niet van de `Microsoft.Network/*` **type** in elke willekeurige resourcegroep waarvan de naam eindigt in `*netrg`.

```json
{
    "if": {
        "allOf": [{
                "value": "[resourceGroup().name]",
                "like": "*netrg"
            },
            {
                "field": "type",
                "notLike": "Microsoft.Network/*"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

In dit voorbeeld van beleid voor regel maakt gebruik van **waarde** om te controleren als het resultaat van meerdere geneste functies **gelijk is aan** `true`. De regel wordt een resource die niet ten minste drie labels hebt geweigerd.

```json
{
    "mode": "indexed",
    "policyRule": {
        "if": {
            "value": "[less(length(field('tags')), 3)]",
            "equals": true
        },
        "then": {
            "effect": "deny"
        }
    }
}
```

### <a name="effect"></a>Effect

Beleid ondersteunt de volgende typen effect:

- **Weigeren**: genereert een gebeurtenis in het activiteitenlogboek en de aanvraag is mislukt
- **Audit**: genereert een waarschuwingsgebeurtenis in het activiteitenlogboek, maar niet de aanvraag mislukt
- **Toevoeg-**: voegt de gedefinieerde set velden toe aan de aanvraag
- **AuditIfNotExists**: kunt controleren als een resource bestaat niet
- **DeployIfNotExists**: implementeert een resource als deze nog niet bestaat
- **Uitgeschakelde**: resources voor naleving van de beleidsregel niet evalueren

Voor **append**, moet u de volgende gegevens opgeven:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

De waarde kan een tekenreeks of een JSON-indeling-object zijn.

**AuditIfNotExists** en **DeployIfNotExists** sprake is van een resource gerelateerde evalueren en een regel toepast. Als de resource komt niet overeen met de regel, wordt het effect is geïmplementeerd. U kunt bijvoorbeeld vereisen dat een network watcher wordt geïmplementeerd voor alle virtuele netwerken. Zie voor meer informatie de [controleren als extensie bestaat](../samples/audit-ext-not-exist.md) voorbeeld.

De **DeployIfNotExists** effect vereist de **roleDefinitionId** eigenschap in de **details** gedeelte van de beleidsregel. Zie voor meer informatie, [herstel - beleidsdefinitie configureren](../how-to/remediate-resources.md#configure-policy-definition).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

Zie voor meer informatie over elk effect, de volgorde van de evaluatie, eigenschappen en voorbeelden, [Understanding beleid effecten](effects.md).

### <a name="policy-functions"></a>Beleidsfuncties

Met uitzondering van de volgende implementatie en het resourcefuncties, alle [Resource Manager-sjabloonfuncties](../../../azure-resource-manager/resource-group-template-functions.md) zijn beschikbaar voor gebruik binnen een regel voor:

- copyIndex()
- Deployment()
- lijst met *
- providers()
- reference()
- resourceId()
- variables()

Bovendien de `field` functie is beschikbaar voor de regels. `field` wordt voornamelijk gebruikt met **AuditIfNotExists** en **DeployIfNotExists** verwijzing velden op de resource die worden geëvalueerd. Een voorbeeld van het gebruik kan worden weergegeven de [DeployIfNotExists voorbeeld](effects.md#deployifnotexists-example).

#### <a name="policy-function-examples"></a>Beleid voor voorbeelden

In dit voorbeeld van beleid voor regel maakt gebruik van de `resourceGroup` resource-functie om op te halen de **naam** eigenschap in combinatie met de `concat` matrix- en -functie voor het bouwen een `like` voorwaarde dat de naam van de resource te starten met de naam van de resourcegroep.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

In dit voorbeeld van beleid voor regel maakt gebruik van de `resourceGroup` resource-functie om op te halen de **tags** matrix eigenschapswaarde van de **CostCenter** tag op de resourcegroep en toevoegen aan de **kostenplaats**  tag op de nieuwe resource.

```json
{
    "if": {
        "field": "tags.CostCenter",
        "exists": "false"
    },
    "then": {
        "effect": "append",
        "details": [{
            "field": "tags.CostCenter",
            "value": "[resourceGroup().tags.CostCenter]"
        }]
    }
}
```

## <a name="aliases"></a>Aliassen

U de eigenschap aliassen gebruiken voor toegang tot specifieke eigenschappen voor een resourcetype. Aliassen kunnen u beperken welke waarden of voorwaarden zijn toegestaan voor een eigenschap van een resource. Elke alias wordt toegewezen aan paden in verschillende API-versies voor een gegeven resourcetype. Tijdens de evaluatie van het beleid haalt de beleidsengine het eigenschapspad voor die API-versie.

De lijst met aliassen groeien blijft. Als wilt weten welke aliassen worden momenteel ondersteund door Azure Policy, moet u een van de volgende methoden gebruiken:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzAccount if not using Cloud Shell

  # Use Get-AzPolicyAlias to list available providers
  Get-AzPolicyAlias -ListAvailable

  # Use Get-AzPolicyAlias to list aliases for a Namespace (such as Azure Automation -- Microsoft.Automation)
  Get-AzPolicyAlias -NamespaceMatch 'automation'
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

### <a name="understanding-the--alias"></a>Inzicht krijgen in de alias [*]

Aantal van de aliassen die beschikbaar zijn hebben een versie die wordt weergegeven als een 'normale' naam en een andere waarvoor **[\*]** gekoppeld. Bijvoorbeeld:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

Het eerste voorbeeld wordt gebruikt om te evalueren van de volledige matrix, waar de **[\*]** alias evalueert elk element van de matrix.

We bekijken een beleidsregel als voorbeeld. Dit beleid wordt **weigeren** een opslagaccount waarvoor ipRules geconfigureerd en als **geen** van de ipRules heeft een waarde van '127.0.0.1'.

```json
"policyRule": {
    "if": {
        "allOf": [{
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "127.0.0.1"
            }
        ]
    },
    "then": {
        "effect": "deny",
    }
}
```

De **ipRules** matrix is als volgt voor het voorbeeld:

```json
"ipRules": [{
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Hier ziet u hoe in het volgende voorbeeld wordt verwerkt:

- `networkAcls.ipRules` -Controleer of de matrix niet-null is. Dit resulteert in waar, evaluatie wordt dus voortgezet.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
    "exists": "true"
  }
  ```

- `networkAcls.ipRules[*].value` -Controleert alle _waarde_ eigenschap in de **ipRules** matrix.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
    "notEquals": "127.0.0.1"
  }
  ```

  - Als een matrix, wordt elk element verwerkt.

    - '127.0.0.1'! = '127.0.0.1' wordt geëvalueerd als onwaar.
    - '127.0.0.1'! = '192.168.1.1' wordt geëvalueerd als waar.
    - Ten minste één _waarde_ eigenschap in de **ipRules** matrix geëvalueerd als onwaar, zodat de evaluatie wordt gestopt.

Als een voorwaarde is geëvalueerd op false, de **weigeren** effect wordt niet geactiveerd.

## <a name="initiatives"></a>Initiatieven

Initiatieven kunnen u verschillende beleidsdefinities toewijzingen en het beheer vereenvoudigen omdat u met een groep als één item werkt verwante groeperen. Bijvoorbeeld, kunt u gerelateerde tagging beleidsdefinities in een enkele initiatief groeperen. In plaats van elk beleid afzonderlijk toewijst, moet u het initiatief toepassen.

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

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Beoordeling [effecten beleid begrijpen](effects.md)
- Begrijpen hoe u [programmatisch beleid maken](../how-to/programmatically-create.md)
- Meer informatie over het [Nalevingsgegevens ophalen](../how-to/getting-compliance-data.md)
- Meer informatie over het [herstellen van niet-compatibele resources](../how-to/remediate-resources.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/overview.md)