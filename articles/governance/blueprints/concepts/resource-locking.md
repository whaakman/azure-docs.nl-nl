---
title: Resources vergrendelen in Azure blauwdrukken begrijpen
description: Meer informatie over de vergrendeling opties om resources te beschermen bij het toewijzen van een blauwdruk.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 718c23b806da5058c042b961077e51ba0d4b6245
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973249"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Resources vergrendelen in Azure blauwdrukken begrijpen

Het maken van consistente testomgevingen op schaal is alleen echt nuttig zijn als er een mechanisme om ervoor te zorgen is dat consistentie worden bewaard. In dit artikel wordt uitgelegd hoe in Azure blauwdrukken resource vergrendelen werkt.

## <a name="locking-modes-and-states"></a>Vergrendeling modi en statussen

Vergrendelingsmodus is van toepassing op de blauwdruktoewijzing en heeft slechts twee opties: **geen** of **alle Resources**. Dit is geconfigureerd tijdens de blauwdruktoewijzing en kan niet worden gewijzigd nadat de toewijzing is toegepast op het abonnement.

Resources die zijn gemaakt door de definities van de artefacten binnen de blauwdruk die is toegewezen aan het abonnement heeft drie statussen: **niet vergrendeld**, **alleen-lezen**, of **kan niet bewerken / verwijderen**. Elk soort artefact kan hebben de **niet vergrendeld** staat. Niet-resourcegroep artefacten zijn echter als **alleen-lezen** en resourcegroepen zijn als **kan niet bewerken / verwijderen**. Dit is een belangrijk verschil in hoe deze resources worden beheerd.

De **alleen-lezen** status is precies zoals gedefinieerd: de resource kan niet worden gewijzigd op geen enkele manier--er zijn geen wijzigingen en kunnen niet worden verwijderd. De **kan niet bewerken / verwijderen** is meer genuanceerde vanwege de aard van het 'container' van resourcegroepen. De resource-group-object is alleen-lezen, maar het is mogelijk te maken, update en delete-resources binnen de resource groep--, zolang ze geen deel uitmaken van een blauwdruktoewijzing met de **alleen-lezen** status vergrendelen.

## <a name="overriding-locking-states"></a>Vergrendeling statussen overschrijven

Het is doorgaans mogelijk voor iemand met de juiste [op rollen gebaseerd toegangsbeheer](../../../role-based-access-control/overview.md) (RBAC) van het abonnement, zoals de 'Eigenaar'-rol, om te kunnen wijzigen of verwijderen van een resource, dat niet het geval wanneer blauwdrukken is van toepassing als onderdeel van de toewijzing van een geïmplementeerde vergrendelen. Als de toewijzing is ingesteld met de **vergrendeling** optie, het abonnement niet, zelfs de eigenaar van de opgenomen resources kunt wijzigen.

Dit beveiligt de consistentie van de gedefinieerde blauwdruk en de omgeving die is ontworpen om u te maken op basis van per ongeluk of programmatische verwijdering of wijziging.

## <a name="removing-locking-states"></a>Vergrendeling statussen verwijderen

Als deze niet meer nodig om de resources die zijn gemaakt door een-toewijzing te verwijderen, klikt u vervolgens is de enige manier om ze te verwijderen eerst verwijderen van de toewijzing. Wanneer de toewijzing wordt verwijderd, worden de vergrendelingen die zijn gemaakt door de blauwdrukken verwijderd. De resource is overgebleven echter en moet worden verwijderd via een normale manier door iemand met de juiste machtigingen.

## <a name="how-blueprint-locks-work"></a>Hoe werken in blauwdruk wordt vergrendeld

Een RBAC-rol `denyAssignments` wordt toegepast op artefact resources tijdens de toewijzing van blauwdruk als de toewijzing hebt geselecteerd de **vergrendeling** optie. De rol door de beheerde identiteit van de blauwdruktoewijzing is toegevoegd en kan alleen worden verwijderd uit het artefact resources door de dezelfde beheerde identiteit. Dit dwingt de vergrendeling en wordt voorkomen dat de blauwdruk vergrendeling buiten blauwdrukken verwijderen. Verwijderen van de rol en de vergrendeling is alleen mogelijk door het verwijderen van de blauwdruktoewijzing, die alleen kan worden uitgevoerd door personen die over de juiste rechten heeft.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [blauwdruk levenscyclus](lifecycle.md)
- Meer informatie over het gebruik van [statische en dynamische parameters](parameters.md)
- Meer informatie over het aanpassen van de [volgorde blauwdruk](sequencing-order.md)
- Meer informatie over het [bestaande toewijzingen bijwerken](../how-to/update-existing-assignments.md)
- Problemen oplossen bij het toewijzen van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)