---
title: Azure functions voor blauwdrukken
description: Beschrijft de functies voor gebruik met Azure blauwdrukken definities en toewijzingen.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/15/2019
ms.topic: reference
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: dc72113a8f5ed978d64d35c43e94dc9e19e4cdb1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209408"
---
# <a name="functions-for-use-with-azure-blueprints"></a>Functies voor gebruik met Azure blauwdrukken

Blauwdrukken voor Azure biedt een blauwdrukdefinitie maken van meer dynamische functies. Deze functies zijn voor gebruik met blauwdrukdefinities en blauwdruk artefacten. De volledige gebruik van Resource Manager-functies naast het ophalen van een dynamische waarde via een blauwdrukparameter biedt ondersteuning voor de artefacten van een Resource Manager-sjabloon.

De volgende functies worden ondersteund:

- [Artefacten](#artifacts)
- [concat](#concat)
- [parameters](#parameters)
- [resourceGroup](#resourcegroup)
- [resourceGroups](#resourcegroups)
- [abonnement](#subscription)

## <a name="artifacts"></a>Artefacten

`artifacts(artifactName)`

Retourneert dat een object van de eigenschappen van gevuld met die blauwdrukartefacten uitvoer.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| artifactName |Ja |string |De naam van een blauwdruk artefact. |

### <a name="return-value"></a>Retourwaarde

Een object van output-eigenschappen. De **levert** eigenschappen zijn afhankelijk van het type van de blauwdruk artefact waarnaar wordt verwezen. Alle typen als volgt de indeling:

```json
{
  "outputs": {collectionOfOutputProperties}
}
```

#### <a name="policy-assignment-artifact"></a>Beleid voor toewijzing artefact

```json
{
    "outputs": {
        "policyAssignmentId": "{resourceId-of-policy-assignment}",
        "policyAssignmentName": "{name-of-policy-assignment}",
        "policyDefinitionId": "{resourceId-of-policy-definition}",
    }
}
```

#### <a name="resource-manager-template-artifact"></a>Artefact van Resource Manager-sjabloon

De **levert** eigenschappen van het geretourneerde object wordt gedefinieerd in het Resource Manager-sjabloon en geretourneerd door de implementatie.

#### <a name="role-assignment-artifact"></a>Roltoewijzingsartefact

```json
{
    "outputs": {
        "roleAssignmentId": "{resourceId-of-role-assignment}",
        "roleDefinitionId": "{resourceId-of-role-definition}",
        "principalId": "{principalId-role-is-being-assigned-to}",
    }
}
```

### <a name="example"></a>Voorbeeld

Een Resource Manager-sjabloon-artefact met de ID _myTemplateArtifact_ die het volgende voorbeeld bevat de eigenschap uitvoer:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    ...
    "outputs": {
        "myArray": {
            "type": "array",
            "value": ["first", "second"]
        },
        "myString": {
            "type": "string",
            "value": "my string value"
        },
        "myObject": {
            "type": "object",
            "value": {
                "myProperty": "my value",
                "anotherProperty": true
            }
        }
    }
}
```

Enkele voorbeelden van het ophalen van gegevens uit de _myTemplateArtifact_ voorbeeld zijn:

| expressie | Type | Value |
|:---|:---|:---|
|`[artifacts("myTemplateArtifact").outputs.myArray]` | Matrix | \["first", "second"\] |
|`[artifacts("myTemplateArtifact").outputs.myArray[0]]` | String | "first" |
|`[artifacts("myTemplateArtifact").outputs.myString]` | String | "de tekenreekswaarde" |
|`[artifacts("myTemplateArtifact").outputs.myObject]` | Object | {"myproperty": "Mijn value", "anotherProperty": true} |
|`[artifacts("myTemplateArtifact").outputs.myObject.myProperty]` | String | "de waarde" |
|`[artifacts("myTemplateArtifact").outputs.myObject.anotherProperty]` | Bool | True |

## <a name="concat"></a>concat

`concat(string1, string2, string3, ...)`

Combineert meerdere tekenreekswaarden en retourneert de samengevoegde tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| tekenreeks1 |Ja |string |De eerste waarde voor samenvoeging. |
| aanvullende argumenten |Nee |string |Aanvullende waarden in opeenvolgende volgorde voor samenvoeging |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met samengevoegde waarden.

### <a name="remarks"></a>Opmerkingen

De blauwdruk voor Azure-functie verschilt van de Azure Resource Manager sjabloonfunctie werkt alleen met tekenreeksen.

### <a name="example"></a>Voorbeeld

`concat(parameters('organizationName'), '-vm')`

## <a name="parameters"></a>parameters

`parameters(parameterName)`

Retourneert een waarde van de parameter blauwdruk. De opgegeven parameternaam moet worden gedefinieerd in de blauwdrukdefinitie van de of blauwdrukartefacten.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| parameterName |Ja |string |De naam van de parameter om terug te keren. |

### <a name="return-value"></a>Retourwaarde

De waarde van de opgegeven blauwdruk of blauwdrukparameter artefact.

### <a name="remarks"></a>Opmerkingen

De blauwdruk voor Azure-functie verschilt van de Azure Resource Manager sjabloonfunctie werkt alleen met blauwdrukparameters.

### <a name="example"></a>Voorbeeld

Parameter definiëren _principalIds_ in het blauwdrukdefinitie van de:

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "parameters": {
            "principalIds": {
                "type": "array",
                "metadata": {
                    "displayName": "Principal IDs",
                    "description": "This is a blueprint parameter that any artifact can reference. We'll display these descriptions for you in the info bubble. Supply principal IDs for the users,groups, or service principals for the RBAC assignment.",
                    "strongType": "PrincipalId"
                }
            }
        },
        ...
    }
}
```

Gebruik vervolgens _principalIds_ als het argument voor `parameters()` in een blauwdruk artefact:

```json
{
    "type": "Microsoft.Blueprint/blueprints/artifacts",
    "kind": "roleAssignment",
    ...
    "properties": {
        "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
        "principalIds": "[parameters('principalIds')]",
        ...
    }
}
```

## <a name="resourcegroup"></a>ResourceGroup

`resourceGroup()`

Retourneert een object dat de huidige resourcegroep vertegenwoordigt.

### <a name="return-value"></a>Retourwaarde

Het geretourneerde object is in de volgende indeling:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="remarks"></a>Opmerkingen

De blauwdruk voor Azure-functie verschilt van de sjabloonfunctie Azure Resource Manager. De `resourceGroup()` functie kan niet worden gebruikt in een abonnement op artefact of de blauwdrukdefinitie van de. Het kan alleen worden gebruikt in blauwdrukartefacten die deel van een resource-group-artefact uitmaken.

Een algemene gebruik van de `resourceGroup()` functie bestaat uit het maken van resources in dezelfde locatie als de resource-group-artefact.

### <a name="example"></a>Voorbeeld

Voor het gebruik van de locatie van de resourcegroep, ingesteld op een van de blauwdrukdefinitie of tijdens de toewijzing, als de locatie waar een ander artefact declareert u een resource group-tijdelijke aanduiding-object in de blauwdrukdefinitie van de. In dit voorbeeld _NetworkingPlaceholder_ is de naam van de tijdelijke aanduiding voor de resource-groep.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

Gebruik vervolgens de `resourceGroup()` functie in de context van een artefact blauwdruk die is gemaakt voor een resource group tijdelijke aanduiding-object. In dit voorbeeld wordt de sjabloon-artefact wordt geïmplementeerd in de _NetworkingPlaceholder_ resourcegroep en biedt parameter _resourceLocation_ dynamisch worden ingevuld met de  _NetworkingPlaceholder_ resourcegroeplocatie aan de sjabloon. De locatie van de _NetworkingPlaceholder_ resourcegroep is kan statisch zijn gedefinieerd in de blauwdrukdefinitie van de of dynamisch gedefinieerd tijdens de toewijzing. In beide gevallen moet het artefact sjabloon die informatie als een parameter is opgegeven en gebruikt voor het implementeren van de resources naar de juiste locatie.

```json
{
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "kind": "template",
  "properties": {
      "template": {
        ...
      },
      "resourceGroup": "NetworkingPlaceholder",
      ...
      "parameters": {
        "resourceLocation": {
          "value": "[resourceGroup().location]"
        }
      }
  }
}
```

## <a name="resourcegroups"></a>resourceGroups

`resourceGroups(placeholderName)`

Retourneert een object dat staat voor de opgegeven resource group-artefact. In tegenstelling tot `resourceGroup()`, die context van het artefact vereist, wordt deze functie wordt gebruikt om op te halen van de eigenschappen van een specifieke resource group-tijdelijke aanduiding als deze niet in de context van die resourcegroep.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| placeholderName |Ja |string |De naam van de tijdelijke aanduiding van de resource group-artefact om terug te keren. |

### <a name="return-value"></a>Retourwaarde

Het geretourneerde object is in de volgende indeling:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="example"></a>Voorbeeld

Voor het gebruik van de locatie van de resourcegroep, ingesteld op een van de blauwdrukdefinitie of tijdens de toewijzing, als de locatie waar een ander artefact declareert u een resource group-tijdelijke aanduiding-object in de blauwdrukdefinitie van de. In dit voorbeeld _NetworkingPlaceholder_ is de naam van de tijdelijke aanduiding voor de resource-groep.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

Gebruik vervolgens de `resourceGroups()` functie uit de context van een blauwdruk artefact dat moet een verwijzing naar de resource-group-tijdelijke aanduiding-object opgehaald. In dit voorbeeld wordt de sjabloon-artefact wordt geïmplementeerd buiten de _NetworkingPlaceholder_ resourcegroep en biedt parameter _artifactLocation_ dynamisch worden ingevuld met de  _NetworkingPlaceholder_ resourcegroeplocatie aan de sjabloon. De locatie van de _NetworkingPlaceholder_ resourcegroep is kan statisch zijn gedefinieerd in de blauwdrukdefinitie van de of dynamisch gedefinieerd tijdens de toewijzing. In beide gevallen moet het artefact sjabloon die informatie als een parameter is opgegeven en gebruikt voor het implementeren van de resources naar de juiste locatie.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "artifactLocation": {
          "value": "[resourceGroups('NetworkingPlaceholder').location]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="subscription"></a>abonnement

`subscription()`

Meer informatie over het abonnement voor de huidige blauwdruktoewijzing retourneert.

### <a name="return-value"></a>Retourwaarde

Het geretourneerde object is in de volgende indeling:

```json
{
    "id": "/subscriptions/{subscriptionId}",
    "subscriptionId": "{subscriptionId}",
    "tenantId": "{tenantId}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Voorbeeld

De weergavenaam van het abonnement en de `concat()` functie voor het maken van een naamgevingsconventie die is doorgegeven als parameter _resourceName_ tot de sjabloon-artefact.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "resourceName": {
          "value": "[concat(subscription().displayName, '-vm')]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [levenscyclus van een blauwdruk](../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md).
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md).