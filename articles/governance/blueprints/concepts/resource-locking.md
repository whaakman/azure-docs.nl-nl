---
title: Informatie over resource vergrendelen
description: Meer informatie over de vergrendeling opties om resources te beschermen bij het toewijzen van een blauwdruk.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/28/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 5409de8aabb52a531551abbc28ae9e873b262eba
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762426"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Resources vergrendelen in Azure blauwdrukken begrijpen

Het maken van consistente testomgevingen op schaal is alleen echt nuttig zijn als er is een mechanisme om dat consistentie te garanderen. In dit artikel wordt uitgelegd hoe in Azure blauwdrukken resource vergrendelen werkt. Voor een voorbeeld van de resource vergrendelen en de toepassing van _weigeren toewijzingen_, Zie de [beveiligen van nieuwe resources](../tutorials/protect-new-resources.md) zelfstudie.

## <a name="locking-modes-and-states"></a>Vergrendeling modi en statussen

Vergrendelingsmodus is van toepassing op de blauwdruktoewijzing en bestaat uit drie opties: **Niet vergrendelen**, **alleen-lezen**, of **niet verwijderen**. De vergrendelingsmodus is tijdens de implementatie van het artefact tijdens een blauwdruktoewijzing geconfigureerd. Een andere vergrendelingsmodus kan worden ingesteld door het bijwerken van de blauwdruktoewijzing.
Vergrendeling modi, maar kan niet worden gewijzigd buiten blauwdrukken.

Resources die zijn gemaakt door artefacten in een blauwdruktoewijzing heeft vier statussen: **Niet vergrendeld**, **alleen-lezen**, **kan niet bewerken / verwijderen**, of **kan niet worden verwijderd**. Elk artefacttype kan zich in de **niet vergrendeld** staat. De volgende tabel kan worden gebruikt om de status van een resource te bepalen:

|Modus|Artefacttype-Resource|Status|Beschrijving|
|-|-|-|-|
|Niet vergrendelen|*|Niet vergrendeld|Resources niet zijn beveiligd door de blauwdrukken. Deze status wordt ook gebruikt voor resources die zijn toegevoegd aan een **alleen-lezen** of **niet verwijderen** resource group-artefact van buiten een blauwdruktoewijzing.|
|Alleen-lezen|Resourcegroep|Kan geen bewerken / verwijderen|De resourcegroep is alleen-lezen en tags voor de resourcegroep kunnen niet worden gewijzigd. **Niet vergrendeld** resources kunnen worden toegevoegd, verplaatst, gewijzigd of verwijderd uit deze resourcegroep.|
|Alleen-lezen|Niet-resourcegroep|Alleen-lezen|De resource kan niet worden gewijzigd op geen enkele manier--er zijn geen wijzigingen en kunnen niet worden verwijderd.|
|Niet verwijderen|*|Kan niet worden verwijderd|De resources kunnen worden gewijzigd, maar kunnen niet worden verwijderd. **Niet vergrendeld** resources kunnen worden toegevoegd, verplaatst, gewijzigd of verwijderd uit deze resourcegroep.|

## <a name="overriding-locking-states"></a>Vergrendeling statussen overschrijven

Het is doorgaans mogelijk voor iemand met de juiste [op rollen gebaseerd toegangsbeheer](../../../role-based-access-control/overview.md) (RBAC) van het abonnement, zoals de 'Eigenaar'-rol, worden toegestaan te wijzigen of verwijderen van een resource. Deze toegang is niet het geval wanneer blauwdrukken van toepassing is als onderdeel van de toewijzing van een geïmplementeerde vergrendelen. Als de toewijzing is ingesteld met de **alleen-lezen** of **niet verwijderen** optie, het abonnement niet, zelfs de eigenaar van de geblokkeerde actie kunt uitvoeren op de beveiligde bron.

Dit beveiligingsmaatregel beveiligt de consistentie van de gedefinieerde blauwdruk en de omgeving die is ontworpen om u te maken op basis van per ongeluk of programmatische verwijdering of wijziging.

## <a name="removing-locking-states"></a>Vergrendeling statussen verwijderen

Als deze niet meer nodig zijn om te wijzigen of verwijderen van een resource die is beveiligd door een toewijzing, zijn er twee manieren om dit te doen.

- De blauwdruktoewijzing bijwerken naar een vergrendelingsmodus van **niet vergrendelen**
- De blauwdruktoewijzing verwijderen

Wanneer de toewijzing wordt verwijderd, worden de vergrendelingen die zijn gemaakt door de blauwdrukken verwijderd. Echter, de resource is overgebleven en moet worden verwijderd via een normale manier.

## <a name="how-blueprint-locks-work"></a>Hoe werken in blauwdruk wordt vergrendeld

Een RBAC [weigeren toewijzingen](../../../role-based-access-control/deny-assignments.md) weigeren actie wordt toegepast op artefact resources tijdens de toewijzing van blauwdruk als de toewijzing hebt geselecteerd de **alleen-lezen** of **niet verwijderen** optie. De actie voor weigeren van de beheerde identiteit van de blauwdruktoewijzing is toegevoegd en kan alleen worden verwijderd uit het artefact resources door de dezelfde beheerde identiteit. Dit beveiligingsmaatregel dwingt de vergrendeling en voorkomt u dat de blauwdruk vergrendeling buiten blauwdrukken verwijderen.

![Blauwdruk weigeren toewijzing op resourcegroep](../media/resource-locking/blueprint-deny-assignment.png)

> [!IMPORTANT]
> Azure Resource Manager in de cache opgeslagen rol Toewijzingsdetails gedurende maximaal 30 minuten. Als gevolg hiervan weigeren toewijzingen weigeren van de actie voor blauwdruk resources mogelijk onmiddellijk niet in het volledige effect. Tijdens deze periode is het mogelijk dat het mogelijk om te verwijderen van een resource die is bedoeld om te worden beveiligd door de blauwdruk wordt vergrendeld.

## <a name="exclude-a-principal-from-a-deny-assignment"></a>Een principal uitsluiten van een toewijzing weigeren

In bepaalde situaties ontwerp of de beveiliging, kan het nodig zijn om uit te sluiten van een principal van zijn de [toewijzing weigeren](../../../role-based-access-control/deny-assignments.md) de blauwdruktoewijzing wordt gemaakt. Dit doet u in de REST-API door toe te voegen van maximaal vijf waarden naar de **excludedPrincipals** matrix in de **vergrendelingen** eigenschap wanneer [het maken van de toewijzing](/rest/api/blueprints/assignments/createorupdate).
Dit is een voorbeeld van een aanvraagtekst met **excludedPrincipals**:

```json
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "eastus",
  "properties": {
    "description": "enforce pre-defined simpleBlueprint to this XXXXXXXX subscription.",
    "blueprintId": "/providers/Microsoft.Management/managementGroups/{mgId}/providers/Microsoft.Blueprint/blueprints/simpleBlueprint",
    "locks": {
        "mode": "AllResourcesDoNotDelete",
        "excludedPrincipals": [
            "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
            "38833b56-194d-420b-90ce-cff578296714"
        ]
    },
    "parameters": {
      "storageAccountType": {
        "value": "Standard_LRS"
      },
      "costCenter": {
        "value": "Contoso/Online/Shopping/Production"
      },
      "owners": {
        "value": [
          "johnDoe@contoso.com",
          "johnsteam@contoso.com"
        ]
      }
    },
    "resourceGroups": {
      "storageRG": {
        "name": "defaultRG",
        "location": "eastus"
      }
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

- Ga als volgt de [nieuwe resources beveiligen](../tutorials/protect-new-resources.md) zelfstudie.
- Meer informatie over de [levenscyclus van een blauwdruk](lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](sequencing-order.md) aanpast.
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md).
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md).