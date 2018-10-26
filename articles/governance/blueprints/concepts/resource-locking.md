---
title: Resources vergrendelen in Azure blauwdrukken begrijpen
description: Meer informatie over de vergrendeling opties om resources te beschermen bij het toewijzen van een blauwdruk.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: c071c8f422e0cb3a4e0d5e528989dd2987a6796f
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094844"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Resources vergrendelen in Azure blauwdrukken begrijpen

Het maken van consistente testomgevingen op schaal is alleen echt nuttig zijn als er is een mechanisme om dat consistentie te garanderen. In dit artikel wordt uitgelegd hoe in Azure blauwdrukken resource vergrendelen werkt.

## <a name="locking-modes-and-states"></a>Vergrendeling modi en statussen

Vergrendelingsmodus is van toepassing op de blauwdruktoewijzing en heeft slechts twee opties: **geen** of **alle Resources**. De vergrendelingsmodus tijdens de blauwdruktoewijzing is geconfigureerd en kan niet worden gewijzigd nadat de toewijzing is toegepast op het abonnement.

Resources die zijn gemaakt door artefacten in een blauwdruktoewijzing heeft drie statussen: **niet vergrendeld**, **alleen-lezen**, of **kan niet bewerken / verwijderen**. Elke artefact kan zich in de **niet vergrendeld** staat. Niet-resourcegroep artefacten hebben echter **alleen-lezen** en resourcegroepen hebt **kan niet bewerken / verwijderen** Staten. Dit verschil is een belangrijk verschil in hoe deze resources worden beheerd.

De **alleen-lezen** status is precies zoals gedefinieerd: de resource kan niet worden gewijzigd op geen enkele manier--er zijn geen wijzigingen en kunnen niet worden verwijderd. De **kan niet bewerken / verwijderen** is meer genuanceerde vanwege de aard van het 'container' van resourcegroepen. De resource-group-object is alleen-lezen, maar het is mogelijk wijzigingen aanbrengen in niet-vergrendelde resources binnen de resourcegroep.

## <a name="overriding-locking-states"></a>Vergrendeling statussen overschrijven

Het is doorgaans mogelijk voor iemand met de juiste [op rollen gebaseerd toegangsbeheer](../../../role-based-access-control/overview.md) (RBAC) van het abonnement, zoals de 'Eigenaar'-rol, worden toegestaan te wijzigen of verwijderen van een resource. Deze toegang is niet het geval wanneer blauwdrukken van toepassing is als onderdeel van de toewijzing van een geïmplementeerde vergrendelen. Als de toewijzing is ingesteld met de **vergrendeling** optie, het abonnement niet, zelfs de eigenaar van de opgenomen resources kunt wijzigen.

Dit beveiligingsmaatregel beveiligt de consistentie van de gedefinieerde blauwdruk en de omgeving die is ontworpen om u te maken op basis van per ongeluk of programmatische verwijdering of wijziging.

## <a name="removing-locking-states"></a>Vergrendeling statussen verwijderen

Als deze niet meer nodig om de resources die zijn gemaakt door een-toewijzing te verwijderen, is de manier waarop ze verwijderen eerst verwijderen van de toewijzing. Wanneer de toewijzing wordt verwijderd, worden de vergrendelingen die zijn gemaakt door de blauwdrukken verwijderd. Echter, de resource is overgebleven en moet worden verwijderd via een normale manier.

## <a name="how-blueprint-locks-work"></a>Hoe werken in blauwdruk wordt vergrendeld

Een RBAC-rol `denyAssignments` wordt toegepast op artefact resources tijdens de toewijzing van blauwdruk als de toewijzing hebt geselecteerd de **vergrendeling** optie. De rol door de beheerde identiteit van de blauwdruktoewijzing is toegevoegd en kan alleen worden verwijderd uit het artefact resources door de dezelfde beheerde identiteit. Dit beveiligingsmaatregel dwingt de vergrendeling en voorkomt u dat de blauwdruk vergrendeling buiten blauwdrukken verwijderen. Verwijderen van de rol en de vergrendeling is alleen mogelijk door het verwijderen van de blauwdruktoewijzing, die alleen kan worden uitgevoerd door personen die over de juiste rechten heeft.

> [!IMPORTANT]
> Azure Active Directory in de cache opgeslagen rol Toewijzingsdetails gedurende maximaal 30 minuten. Als gevolg hiervan `denyAssignments` blauwdruk resources kunnen niet onmiddellijk ook in het volledige effect. Tijdens deze periode is het mogelijk dat het mogelijk om te verwijderen van een resource die is bedoeld om te worden beveiligd door de blauwdruk wordt vergrendeld.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [levenscyclus van een blauwdruk](lifecycle.md)
- Informatie over hoe u [statische en dynamische parameters](parameters.md) gebruikt
- Meer informatie over hoe u de [blauwdrukvolgorde](sequencing-order.md) aanpast
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md)
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)