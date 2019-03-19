---
title: De auteur van beleid voor de Matrixeigenschappen van de op Azure-resources
description: Informatie over matrixparameters maken, maken van regels voor de matrix taal expressies, evalueren de alias [*], en elementen toevoegen aan een bestaande matrix met Azure Policy definitieregels.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/06/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 62267a4549355212a18654ff9781b2164ba19fa9
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57864028"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>De auteur van beleid voor de Matrixeigenschappen van de op Azure-resources

Azure Resource Manager-eigenschappen worden gewoonlijk gedefinieerd als tekenreeksen en Booleaanse waarden. Wanneer een een-op-veel-relatie bestaat, worden in plaats daarvan complexe eigenschappen gedefinieerd als matrices. Matrices worden in Azure Policy, op verschillende manieren gebruikt:

- Het type van een [definitie parameter](../concepts/definition-structure.md#parameters), meerdere opties
- Onderdeel van een [beleidsregel](../concepts/definition-structure.md#policy-rule) met behulp van de voorwaarden **in** of **notIn**
- Onderdeel van een beleidsregel die wordt geëvalueerd als de [ \[ \* \] alias](../concepts/definition-structure.md#understanding-the--alias) specifieke scenario's zoals evalueren **geen**, **eventuele**, of  **Alle**
- In de [effect append](../concepts/effects.md#append) wilt vervangen of toevoegen aan een bestaande matrix

Dit artikel wordt ingegaan op elk gebruik door Azure Policy en enkele voorbeeld-definities.

## <a name="parameter-arrays"></a>Parametermatrices

### <a name="define-a-parameter-array"></a>Een parametermatrix definiëren

Een parameter definiëren als een matrix, biedt de flexibiliteit van het beleid als er meer dan één waarde nodig is.
Deze beleidsdefinitie kan een enkele locatie voor de parameter **allowedLocations** en wordt standaard ingesteld op _eastus2_:

```json
"parameters": {
    "allowedLocations": {
        "type": "string",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2"
    }
}
```

Als **type** is _tekenreeks_, alleen een waarde kan worden ingesteld wanneer het toewijzen van beleid. Als dit beleid is toegewezen, worden resources binnen het bereik zijn alleen toegestaan binnen één Azure-regio. De meeste beleid-definities wilt toestaan voor een lijst van goedgekeurde opties, zoals het toestaan van _eastus2_, _eastus_, en _westus2_.

Gebruik voor het maken van de beleidsdefinitie zodat meerdere opties voor de _matrix_ **type**. Hetzelfde beleid kunt als volgt worden herschreven:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2",
        "allowedValues": [
            "eastus2",
            "eastus",
            "westus2"
        ]

    }
}
```

> [!NOTE]
> Wanneer de beleidsdefinitie van een is opgeslagen, de **type** eigenschap voor een parameter kan niet worden gewijzigd.

Deze nieuwe parameterdefinitie vindt meer dan één waarde tijdens de toewijzing van configuratiebeleid. Met de matrixeigenschap **allowedValues** gedefinieerd, de waarden die beschikbaar zijn tijdens de toewijzing worden verder beperkt tot de vooraf gedefinieerde lijst met opties. Het gebruik van **allowedValues** is optioneel.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Waarden doorgeven aan een parametermatrix tijdens de toewijzing

Bij het toewijzen van het beleid via Azure portal, een parameter van het **type** _matrix_ als één tekstvak wordt weergegeven. De hint zegt 'gebruiken. om waarden te scheiden. (bijvoorbeeld Londen; New York) '. De locatie van de toegestane waarden van _eastus2_, _eastus_, en _westus2_ naar de parameter, gebruikt u de volgende tekenreeks:

`eastus2;eastus;westus2`

De indeling voor de parameterwaarde is anders bij het gebruik van Azure CLI, Azure PowerShell of de REST-API. De waarden worden doorgegeven via een JSON-tekenreeks die ook de naam van de parameter.

```json
{
    "allowedLocations": {
        "value": [
            "eastus2",
            "eastus",
            "westus2"
        ]
    }
}
```

Voor het gebruik van deze tekenreeks met de SDK, gebruik de volgende opdrachten:

- Azure CLI: Opdracht [az beleidstoewijzing te maken](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) met parameter **parameters**
- Azure PowerShell: Cmdlet [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) met parameter **PolicyParameter**
- REST-API: In de _plaatsen_ [maken](/rest/api/resources/policyassignments/create) bewerking als onderdeel van de aanvraagtekst als de waarde van de **properties.parameters** eigenschap

## <a name="policy-rules-and-arrays"></a>Beleidsregels en -matrices

### <a name="array-conditions"></a>Matrix-voorwaarden

De beleidsregel [voorwaarden](../concepts/definition-structure.md#conditions) die een _matrix_
**type** van de parameter kan worden gebruikt met is beperkt tot `in` en `notIn`. Uitvoeren van de volgende beleidsdefinitie met voorwaarde `equals` als een voorbeeld:

```json
{
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "equals": "[parameters('allowedLocations')]"
      }
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "description": "The list of allowed locations for resources.",
        "displayName": "Allowed locations",
        "strongType": "location"
      }
    }
  }
}
```

Bij het maken van deze beleidsdefinitie via de Azure portal leidt tot een fout betreft, zoals dit foutbericht wordt weergegeven:

- "Het beleid {GUID} kan niet worden geparametriseerd vanwege validatiefouten. Controleer als beleidsparameters juist zijn gedefinieerd. De binnenste uitzondering 'evaluatie resultaat van de taalexpressie [parameters('allowedLocations')] is type 'Array' verwachte type is 'String','.'

De verwachte **type** van voorwaarde `equals` is _tekenreeks_. Aangezien **allowedLocations** wordt gedefinieerd als **type** _matrix_, de beleidsengine evalueert de taalexpressie en de fout genereert. Met de `in` en `notIn` voorwaarde, de groepsbeleid-engine wordt verwacht dat de **type** _matrix_ in de taalexpressie. Om op te lossen dit foutbericht, wijzigt u `equals` aan `in` of `notIn`.

### <a name="evaluating-the--alias"></a>Evaluatie van de alias [*]

Aliassen waarvoor **[\*]** die is gekoppeld aan hun naam geven de **type** is een _matrix_. In plaats van het evalueren van de waarde van de volledige matrix **[\*]** maakt het mogelijk om te evalueren van elk element van de matrix. Er zijn drie scenario's die dit per rapportitem wordt geëvalueerd is handig in: Geen, een, en alle.

De groepsbeleid-engine triggers de **effect** in **vervolgens** alleen wanneer de **als** regel wordt geëvalueerd als waar.
Dit is belangrijk te begrijpen in de context van de manier waarop **[\*]** evalueert elk afzonderlijk element van de matrix.

De voorbeeld-beleidsregel voor de onderstaande scenario-tabel:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            <-- Condition (see table below) -->
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

De **ipRules** matrix is als volgt voor het scenario van onderstaande tabel:

```json
"ipRules": [
    {
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Vervang bijvoorbeeld elke voorwaarde onderstaande `<field>` met `"field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value"`.

De volgende resultaten zijn het resultaat van de combinatie van de voorwaarde en de voorbeeld-beleidsregel en matrix met bovenstaande van bestaande waarden:

|Voorwaarde |Resultaat |Uitleg |
|-|-|-|
|`{<field>,"notEquals":"127.0.0.1"}` |Niets |Een element van de matrix wordt geëvalueerd als onwaar (127.0.0.1! = 127.0.0.1) en één als waar (127.0.0.1! = 192.168.1.1), waardoor de **notEquals** voorwaarde is _false_ en het effect is niet geactiveerd. |
|`{<field>,"notEquals":"10.0.4.1"}` |Effect van het beleid |Beide matrixelementen evalueren als waar (10.0.4.1! = 127.0.0.1 en 10.0.4.1! = 192.168.1.1), waardoor de **notEquals** voorwaarde is _waar_ en het effect is geactiveerd. |
|`"not":{<field>,"Equals":"127.0.0.1"}` |Effect van het beleid |Een element van de matrix wordt geëvalueerd als waar (127.0.0.1 == 127.0.0.1) en één als onwaar (127.0.0.1 == 192.168.1.1), waardoor de **gelijk is aan** voorwaarde is _false_. De logische operator wordt geëvalueerd als waar (**niet** _false_), zodat het effect is geactiveerd. |
|`"not":{<field>,"Equals":"10.0.4.1"}` |Effect van het beleid |Beide matrixelementen geëvalueerd als onwaar (10.0.4.1 == 127.0.0.1 en 10.0.4.1 == 192.168.1.1), waardoor de **gelijk is aan** voorwaarde is _false_. De logische operator wordt geëvalueerd als waar (**niet** _false_), zodat het effect is geactiveerd. |
|`"not":{<field>,"notEquals":"127.0.0.1" }` |Effect van het beleid |Een element van de matrix wordt geëvalueerd als onwaar (127.0.0.1! = 127.0.0.1) en één als waar (127.0.0.1! = 192.168.1.1), waardoor de **notEquals** voorwaarde is _false_. De logische operator wordt geëvalueerd als waar (**niet** _false_), zodat het effect is geactiveerd. |
|`"not":{<field>,"notEquals":"10.0.4.1"}` |Niets |Beide matrixelementen evalueren als waar (10.0.4.1! = 127.0.0.1 en 10.0.4.1! = 192.168.1.1), waardoor de **notEquals** voorwaarde is _waar_. De logische operator wordt geëvalueerd als onwaar (**niet** _waar_), zodat het effect is niet geactiveerd. |
|`{<field>,"Equals":"127.0.0.1"}` |Niets |Een element van de matrix wordt geëvalueerd als waar (127.0.0.1 == 127.0.0.1) en één als onwaar (127.0.0.1 == 192.168.1.1), waardoor de **gelijk is aan** voorwaarde is _false_ en het effect is niet geactiveerd. |
|`{<field>,"Equals":"10.0.4.1"}` |Niets |Beide matrixelementen geëvalueerd als onwaar (10.0.4.1 == 127.0.0.1 en 10.0.4.1 == 192.168.1.1), waardoor de **gelijk is aan** voorwaarde is _false_ en het effect is niet geactiveerd. |

## <a name="the-append-effect-and-arrays"></a>Het effect toevoegen en matrices

De [effect append](../concepts/effects.md#append) werkt anders afhankelijk van of de **details.field** is een **[\*]** alias of niet.

- Wanneer er geen een **[\*]** toevoegen alias, vervangt u de volledige matrix met de **waarde** eigenschap
- Wanneer een **[\*]** toevoegen alias, voegt de **waarde** eigenschap aan de bestaande matrix of maakt u de nieuwe matrix

Zie voor meer informatie de [append-voorbeelden](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Volgende stappen

- Bekijk voorbeelden op [Azure Policy-voorbeelden](../samples/index.md)
- Controleer de [structuur van beleidsdefinities](../concepts/definition-structure.md)
- Beoordeling [effecten beleid begrijpen](../concepts/effects.md)
- Begrijpen hoe u [programmatisch beleid maken](programmatically-create.md)
- Meer informatie over het [herstellen van niet-compatibele resources](remediate-resources.md)
- Bekijk wat een beheergroep is met [Resources organiseren met Azure-beheergroepen](../../management-groups/overview.md)