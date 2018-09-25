---
title: Inzicht in de implementatievolgorde van de in Azure blauwdrukken
description: Meer informatie over de levenscyclus die een blauwdruk doorloopt en details over elke fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: c09fb26d8375e08281241aaed3f6f6e30acc755b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955449"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Inzicht in de implementatievolgorde van de in Azure blauwdrukken

Azure maakt gebruik van blauwdrukken een **volgorde** om te bepalen van de volgorde van de resources worden gemaakt bij het verwerken van de toewijzing van blauwdruk. In dit artikel wordt uitgelegd voor de volgorde van de standaard die wordt gebruikt, over het aanpassen van de volgorde en hoe de aangepaste bestelling is verwerkt.

Er zijn variabelen in de JSON-voorbeelden die u nodig hebt om te vervangen door uw eigen waarden:

- `{YourMG}` -Vervangen door de naam van uw beheergroep

## <a name="default-sequencing-order"></a>Standaard-volgorde

Als de blauwdruk geen richtlijn voor het bevat implementeren van artefacten of de richtlijn null is, wordt de volgende volgorde gebruikt:

- Abonnementsniveau **roltoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Abonnementsniveau **beleidstoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Abonnementsniveau **Azure Resource Manager-sjabloon** artefacten die zijn gesorteerd op naam van het assemblyartefact
- **Resourcegroep** artefacten (inclusief onderliggende artefacten) gesorteerd op naam van de tijdelijke aanduiding

Binnen elk **resourcegroep** artefact dat is verwerkt, wordt de volgende volgorde wordt gebruikt voor artefacten moet worden gemaakt in die resourcegroep:

- Resource group onderliggende **roltoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Resource group onderliggende **beleidstoewijzing** artefacten die zijn gesorteerd op naam van het assemblyartefact
- Resource group onderliggende **Azure Resource Manager-sjabloon** artefacten die zijn gesorteerd op naam van het assemblyartefact

## <a name="customizing-the-sequencing-order"></a>De volgorde aan te passen

Wanneer u grote blauwdrukken, is het mogelijk nodig is voor een resource moet worden gemaakt in een specifieke volgorde in relatie tot een andere resource. De meest voorkomende gebruikspatroon hiervan is wanneer een blauwdruk meerdere Azure Resource Manager-sjablonen bevat. Blauwdrukken doet dit door toe te staan de volgorde te worden gedefinieerd.

Dit wordt bereikt door het definiëren van een `dependsOn` eigenschap in de JSON. Deze eigenschap wordt ondersteund door alleen de blauwdruk (voor resourcegroepen) en artefact-objecten. `dependsOn` is een string-matrix van artefactnamen die het artefact name worden gemaakt moet voordat deze gemaakt.

### <a name="example---blueprint-with-ordered-resource-group"></a>Voorbeeld: blauwdruk met geordende resourcegroep

Dit is een voorbeeld van de blauwdruk met een resourcegroep die een aangepaste volgorde is gedefinieerd door op te geven van een waarde voor `dependsOn`, samen met een standaard-resourcegroep. In dit geval het artefact met de naam **assignPolicyTags** worden verwerkt voor de **besteld-rg** resourcegroep.
**Standard-rg** per de standaard-volgorde worden verwerkt.

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

Dit is een voorbeeld van beleid artefact die afhankelijk zijn van een Azure Resource Manager-sjabloon. Standaard bestellen, zou een artefact beleid worden gemaakt voordat u de Azure Resource Manager-sjabloon. Hiermee wordt het beleid artefact na afloop van de Azure Resource Manager-sjabloon moet worden gemaakt.

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

Tijdens het proces voor het maken, wordt een topologische sorteren gebruikt om te maken van de afhankelijkheidsgrafiek voor de blauwdruk en de artefacten. Dit zorgt ervoor dat meerdere niveaus van afhankelijkheid tussen resourcegroepen en artefacten kunnen worden ondersteund.

Als een afhankelijkheid is gedeclareerd in de blauwdruk of een artefact die de standaardvolgorde wouldn't wijzigen, wordt klikt u vervolgens geen wijziging aangebracht in de volgorde. Voorbeelden hiervan zijn een resourcegroep die afhankelijk zijn van een onderliggende beleidstoewijzing voor abonnement niveau beleid of resource group 'standaard-rg' die afhankelijk zijn van de roltoewijzing voor onderliggende resource group 'standaard-rg'. In beide gevallen wordt de `dependsOn` wouldn't hebt gewijzigd in de standaard volgorde en er zijn geen wijzigingen zouden worden gesteld.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [blauwdruk levenscyclus](lifecycle.md)
- Meer informatie over het gebruik van [statische en dynamische parameters](parameters.md)
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](resource-locking.md)
- Meer informatie over het [bestaande toewijzingen bijwerken](../how-to/update-existing-assignments.md)
- Problemen oplossen bij het toewijzen van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)