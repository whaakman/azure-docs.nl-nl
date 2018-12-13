---
title: Inzicht in de volgorde van de implementatie
description: Meer informatie over de levenscyclus die een blauwdruk doorloopt en details over elke fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: bd12aabf0ca8f82261e6b3c677d7306ee46c4171
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53308614"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Inzicht in de implementatievolgorde van de in Azure blauwdrukken

Azure maakt gebruik van blauwdrukken een **volgorde** om te bepalen van de volgorde van de resources worden gemaakt bij het verwerken van de toewijzing van blauwdruk. In dit artikel worden de volgende concepten:

- De volgorde van de standaard die wordt gebruikt
- Over het aanpassen van de volgorde
- Hoe de aangepaste bestelling is verwerkt

Er zijn variabelen in de JSON-voorbeelden die u nodig hebt om te vervangen door uw eigen waarden:

- Vervang `{YourMG}` door de naam van uw beheergroep

## <a name="default-sequencing-order"></a>Standaard-volgorde

Als de blauwdruk geen richtlijn voor het bevat implementeren van artefacten of de richtlijn null is, wordt de volgende volgorde gebruikt:

- Abonnementsniveau **roltoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Abonnementsniveau **beleidstoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Abonnementsniveau **Azure Resource Manager-sjabloon** artefacten die zijn gesorteerd op naam van het assemblyartefact
- **Resourcegroep** artefacten (inclusief onderliggende artefacten) gesorteerd op naam van de tijdelijke aanduiding

Binnen elk **resourcegroep** artefact, de volgende volgorde wordt gebruikt voor artefacten moet worden gemaakt in die resourcegroep:

- Resource group onderliggende **roltoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Resource group onderliggende **beleidstoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Resource group onderliggende **Azure Resource Manager-sjabloon** artefacten die zijn gesorteerd op naam van het assemblyartefact

## <a name="customizing-the-sequencing-order"></a>De volgorde aan te passen

Wanneer u grote blauwdrukken, is het mogelijk nodig zijn voor resources in een bepaalde volgorde worden gemaakt. De meest voorkomende gebruikspatroon van dit scenario is wanneer een blauwdruk verschillende Azure Resource Manager-sjablonen bevat. Dit patroon blauwdrukken verwerkt door de volgorde te worden gedefinieerd.

De volgorde wordt bereikt door het definiëren van een `dependsOn` eigenschap in de JSON. Deze eigenschap wordt ondersteund door alleen de blauwdruk (voor resourcegroepen) en artefact-objecten. `dependsOn` is een string-matrix van artefactnamen die het artefact name worden gemaakt moet voordat deze gemaakt.

> [!NOTE]
> **Resourcegroep** artefacten ondersteunt de `dependsOn` eigenschap, maar mag niet het doel van een `dependsOn` door elk artefacttype.

### <a name="example---blueprint-with-ordered-resource-group"></a>Voorbeeld: blauwdruk met geordende resourcegroep

In dit voorbeeld van de blauwdruk heeft een resourcegroep die een aangepaste volgorde is gedefinieerd door op te geven van een waarde voor `dependsOn`, samen met een standaard-resourcegroep. In dit geval het artefact met de naam **assignPolicyTags** worden verwerkt voor de **besteld-rg** resourcegroep. **Standard-rg** per de standaard-volgorde worden verwerkt.

```json
{
    "properties": {
        "description": "Example blueprint with custom sequencing order",
        "resourceGroups": {
            "ordered-rg": {
                "dependsOn": [
                    "assignPolicyTags"
                ],
                "metadata": {
                    "description": "Resource Group that waits for 'assignPolicyTags' creation"
                }
            },
            "standard-rg": {
                "metadata": {
                    "description": "Resource Group that follows the standard sequence ordering"
                }
            }
        },
        "targetScope": "subscription"
    },
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint",
    "type": "Microsoft.Blueprint/blueprints",
    "name": "mySequencedBlueprint"
}
```

### <a name="example---artifact-with-custom-order"></a>Voorbeeld - artefact met aangepaste volgorde

In dit voorbeeld is een beleid artefact die afhankelijk zijn van een Azure Resource Manager-sjabloon. Standaard bestellen, zou een artefact beleid worden gemaakt voordat u de Azure Resource Manager-sjabloon. Deze volgorde, kunt het beleid artefact na afloop van de Azure Resource Manager-sjabloon moet worden gemaakt.

```json
{
    "properties": {
        "displayName": "Assigns an identifying tag",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "resourceGroup": "standard-rg",
        "dependsOn": [
            "customTemplate"
        ]
    },
    "kind": "policyAssignment",
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint/artifacts/assignPolicyTags",
    "type": "Microsoft.Blueprint/artifacts",
    "name": "assignPolicyTags"
}
```

## <a name="processing-the-customized-sequence"></a>Verwerking van de aangepaste takenreeks

Tijdens het proces voor het maken, wordt een topologische sorteren om te maken van de afhankelijkheidsgrafiek voor de artefacten blauwdrukken gebruikt. De controle zorgt ervoor dat elk niveau van afhankelijkheid tussen resourcegroepen en artefacten wordt ondersteund.

Er is geen wijziging wordt aangebracht als een afhankelijkheid artefact wordt aangegeven dat de standaardvolgorde wouldn't alter. Een voorbeeld is een resourcegroep die afhankelijk zijn van een beleid op abonnementsniveau. Een ander voorbeeld is een resource group 'standaard-rg' onderliggende beleidstoewijzing die afhankelijk zijn van de roltoewijzing voor onderliggende resource group 'standaard-rg'. In beide gevallen wordt de `dependsOn` wouldn't hebt gewijzigd in de standaard volgorde en er zijn geen wijzigingen zouden worden gesteld.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [levenscyclus van een blauwdruk](lifecycle.md)
- Informatie over hoe u [statische en dynamische parameters](parameters.md) gebruikt
- Ontdek hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](resource-locking.md)
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md)
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)