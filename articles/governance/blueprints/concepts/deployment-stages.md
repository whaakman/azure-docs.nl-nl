---
title: Fasen van een implementatie van de blauwdruk
description: Meer informatie over de stappen die de blauwdruk services doorloopt tijdens een implementatie.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: a3ab5589cd327b73f2e66540da5c49343c4449cd
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/15/2019
ms.locfileid: "57999169"
---
# <a name="stages-of-a-blueprint-deployment"></a>Fasen van een implementatie van de blauwdruk

Wanneer een blauwdruk wordt geïmplementeerd, wordt een reeks acties die door de blauwdrukken voor Azure-service om de resources die zijn gedefinieerd in de blauwdruk te implementeren. In dit artikel bevat informatie over wat elke stap bestaat uit.

Implementatie van de blauwdruk wordt geactiveerd door een blauwdruk toewijzen aan een abonnement of [bijwerken van een bestaande toewijzing](../how-to/update-existing-assignments.md). Blauwdrukken wordt tijdens de implementatie van de volgende hoofdstappen:

> [!div class="checklist"]
> - Blauwdrukken eigendomsrechten verleend
> - De toewijzing van blauwdruk object wordt gemaakt
> - Hiermee maakt u optioneel - blauwdrukken **systeem toegewezen** beheerde identiteit
> - De beheerde identiteit wordt geïmplementeerd blauwdrukartefacten
> - Blauwdruk service en **systeem toegewezen** beheerde identiteit rechten zijn ingetrokken

## <a name="blueprints-granted-owner-rights"></a>Blauwdrukken eigendomsrechten verleend

De service-principal voor Azure blauwdrukken krijgt eigendomsrechten voor het toegewezen abonnement of abonnementen. De toegekende rol blauwdrukken voor het maken en later intrekt, kan de [systeem toegewezen identiteit beheerd](../../../active-directory/managed-identities-azure-resources/overview.md).

De rechten worden automatisch verleend als de toewijzing vindt plaats via de portal. Echter, als de toewijzing wordt gedaan via de REST-API, verleent de rechten moeten worden met een afzonderlijke API aanroepen. De blauwdruk Azure AppId is `f71766dc-90d9-4b7d-bd9d-4499c4331c3f`, maar de service-principal is afhankelijk van de tenant. Gebruik [Azure Active Directory Graph API](../../../active-directory/develop/active-directory-graph-api.md) en REST-eindpunt [servicePrincipals](/graph/api/resources/serviceprincipal) om op te halen van de service-principal. Vervolgens, verleent de blauwdrukken Azure de _eigenaar_ rol via de [Portal](../../../role-based-access-control/role-assignments-portal.md), [Azure CLI](../../../role-based-access-control/role-assignments-cli.md), [Azure PowerShell](../../../role-based-access-control/role-assignments-powershell.md), [REST API](../../../role-based-access-control/role-assignments-rest.md), of een [Resource Manager-sjabloon](../../../role-based-access-control/role-assignments-template.md).

De service blauwdrukken implementeren niet rechtstreeks van de resources.

## <a name="the-blueprint-assignment-object-is-created"></a>De toewijzing van blauwdruk object wordt gemaakt

Een gebruiker, groep of service-principal wijst u een blauwdruk toe aan een abonnement. Het toewijzingsobject bestaat op het abonnementsniveau van het waaraan de blauwdruk is toegewezen. Resources die zijn gemaakt door de implementatie niet worden uitgevoerd in de context van de entiteit implementeren.

Tijdens het maken van de blauwdruktoewijzing, het type [beheerde identiteit](../../../active-directory/managed-identities-azure-resources/overview.md) is geselecteerd. De standaardwaarde is een **systeem toegewezen** beheerde identiteit. Een **gebruiker toegewezen** beheerde identiteit kan worden gekozen. Wanneer u een **gebruiker toegewezen** beheerde identiteit, moet worden gedefinieerd en machtigingen worden verleend voordat de blauwdruktoewijzing is gemaakt.

## <a name="optional---blueprints-creates-system-assigned-managed-identity"></a>Optioneel - blauwdrukken wordt gemaakt door het systeem toegewezen beheerde identiteit

Wanneer [systeem toegewezen identiteit beheerd](../../../active-directory/managed-identities-azure-resources/overview.md) is geselecteerd tijdens de toewijzing, blauwdrukken wordt gemaakt van de identiteit en de beheerde identiteit verleent de [eigenaar](../../../role-based-access-control/built-in-roles.md#owner) rol. Als een [bestaande toewijzing wordt bijgewerkt](../how-to/update-existing-assignments.md), blauwdrukken maakt gebruik van de eerder gemaakte beheerde identiteit.

De beheerde identiteit met betrekking tot de blauwdruktoewijzing is gebruikt om te implementeren of de resources die zijn gedefinieerd in de blauwdruk opnieuw implementeren. Dit ontwerp voorkomt toewijzingen per ongeluk interactie met elkaar.
Dit ontwerp biedt ook ondersteuning voor de [resource vergrendelen](./resource-locking.md) functie door de beveiliging van elke geïmplementeerde resource in de blauwdruk te beheren.

## <a name="the-managed-identity-deploys-blueprint-artifacts"></a>De beheerde identiteit wordt geïmplementeerd blauwdrukartefacten

De beheerde identiteit wordt vervolgens geactiveerd voor de Resource Manager-implementaties van de artefacten binnen de blauwdruk in de gedefinieerde [volgorde](./sequencing-order.md). De volgorde kan worden aangepast om ervoor te zorgen artefacten die afhankelijk zijn van andere artefacten in de juiste volgorde worden geïmplementeerd.

Een mislukte toegang door een implementatie is vaak het resultaat van het niveau van toegang verleend aan de beheerde identiteit. De blauwdrukken-service beheert de levenscyclus van de beveiliging van de **systeem toegewezen** beheerde identiteit. De gebruiker is echter verantwoordelijk voor het beheren van de rechten en de levenscyclus van een **gebruiker toegewezen** beheerde identiteit.

## <a name="blueprint-service-and-system-assigned-managed-identity-rights-are-revoked"></a>Blauwdruk service en beheerde identiteit systeem toegewezen rechten worden ingetrokken.

Zodra de implementaties zijn voltooid, blauwdrukken trekt u de rechten van de **systeem toegewezen** beheerde identiteit van het abonnement. De service blauwdrukken trekt u vervolgens de rechten van het abonnement. Verwijderen van rechten wordt voorkomen dat blauwdrukken tegen om een permanente eigenaar van een abonnement.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [statische en dynamische parameters](parameters.md).
- Meer informatie over het aanpassen van de [blauwdruk volgorde](sequencing-order.md).
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](resource-locking.md).
- Meer informatie over het [bijwerken, bestaande toewijzingen](../how-to/update-existing-assignments.md).
- Problemen oplossen bij het toewijzen van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md).