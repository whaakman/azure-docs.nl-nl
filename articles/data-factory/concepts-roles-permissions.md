---
title: Rollen en machtigingen voor Azure Data Factory | Microsoft Docs
description: Beschrijft de rollen en machtigingen die vereist zijn voor de Data Factory's maken en om te werken met de onderliggende resources.
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.openlocfilehash: 10f325f3b7c93b91180b6a170c8b7accb75eb03b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883768"
---
# <a name="roles-and-permissions-for-azure-data-factory"></a>Rollen en machtigingen voor Azure Data Factory

In dit artikel beschrijft de rollen die zijn vereist om te maken en beheren van resources van Azure Data Factory en de machtigingen die door deze rollen worden verleend.

## <a name="roles-and-requirements"></a>Functies en vereisten

Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, lid zijn van de rol *Inzender* of *Eigenaar*, of moet dit een *beheerder* van het Azure-abonnement zijn. Als u wilt weergeven van de machtigingen die u hebt in het abonnement, in de Azure-portal, selecteert u uw gebruikersnaam in de rechterbovenhoek en selecteer vervolgens **machtigingen**. Als u toegang tot meerdere abonnementen hebt, moet u het juiste abonnement selecteren. 

Als u wilt maken en beheren van onderliggende resources voor Data Factory - gegevenssets, inclusief gekoppelde services, pijplijnen, triggers en integratieruntimes - de volgende vereisten zijn van toepassing:
- Als u wilt maken en beheren van onderliggende resources in Azure portal, u moet behoren tot de **Inzender Data Factory** rol op het niveau van de resource of hoger.
- Maken en beheren van onderliggende resources met PowerShell of de SDK de **Inzender** rol op het resourceniveau van de of hoger is voldoende.

Zie voor voorbeelden van instructies over het toevoegen van een gebruiker aan een rol, de [rollen toevoegen](../billing/billing-add-change-azure-subscription-administrator.md) artikel.

## <a name="set-up-permissions"></a>Machtigingen instellen

Nadat u een Data Factory maakt, kunt u de zodat andere gebruikers kunnen werken met de data factory. Dit om toegang te verlenen aan andere gebruikers, die u hebt deze toevoegen aan de ingebouwde **Inzender Data Factory** -rol op de resourcegroep met de data factory.

### <a name="scope-of-the-data-factory-contributor-role"></a>Bereik van de rol Inzender Data Factory

Lidmaatschap van de **Inzender Data Factory** rol kan gebruikers het volgende doen:
- Maken, bewerken en verwijderen van data factory's en onderliggende resources, met inbegrip van gegevenssets, gekoppelde services, pijplijnen, triggers en integratieruntimes.
- Resource Manager-sjablonen implementeren. Resource Manager-implementatie is de implementatiemethode die wordt gebruikt door Data Factory in Azure portal.
- App Insights-waarschuwingen voor een data factory beheren.
- Ondersteuningstickets maken.

Zie voor meer informatie over deze rol [rol van Inzender Data Factory](../role-based-access-control/built-in-roles.md#data-factory-contributor).

### <a name="resource-manager-template-deployment"></a>Sjabloonimplementatie van Resource Manager

De **Inzender Data Factory** rol, op het niveau van de resource of hoger, kan gebruikers Resource Manager-sjablonen implementeren. Als gevolg hiervan kunnen leden van de rol van Resource Manager-sjablonen gebruiken om zowel data factory's en hun onderliggende resources, met inbegrip van gegevenssets, gekoppelde services, pijplijnen, triggers en integratieruntimes te implementeren. Lidmaatschap van deze rol kiest, kunnen niet de gebruiker andere resources, maar kan maken.

> [!IMPORTANT]
> Sjabloonimplementatie van Resource Manager-met de **Inzender Data Factory** rol niet verhogen van uw machtigingen. Bijvoorbeeld, als u een sjabloon die wordt gemaakt van een virtuele Azure-machine implementeert en u bent niet gemachtigd om virtuele machines te maken, mislukt de implementatie met een Autorisatiefout.

### <a name="custom-scenarios-and-custom-roles"></a>Aangepaste scenario's en aangepaste rollen

Soms moet u mogelijk verschillende toegangsniveaus voor verschillende data factory gebruikers verlenen. Bijvoorbeeld:
- Mogelijk moet u een groep waarin gebruikers alleen machtigingen op een specifieke data factory hebben.
- Of mogelijk moet u een groep waarin gebruikers alleen een data factory (of Factory's) kunnen bewaken, maar niet wijzigen.

U kunt deze aangepaste scenario's kunt bereiken met aangepaste rollen maken en toewijzen van gebruikers aan deze rollen. Zie voor meer informatie over aangepaste rollen [aangepaste rollen in Azure](..//role-based-access-control/custom-roles.md).

Hier volgen enkele voorbeelden die laten zien wat u kunt bereiken met aangepaste rollen:

- Laat een gebruiker maken, bewerken of een gegevensfactory in een resourcegroep verwijderen uit de Azure portal.

  Wijs de ingebouwde **Inzender Data Factory** rol op het niveau van de resourcegroep voor de gebruiker. Als u toestaan voor toegang tot alle data factory in een abonnement wilt, moet u de rol op het abonnementsniveau van het toewijzen.

- Laat de weergave van een gebruiker (lezen) en bewaken van een data factory, maar niet bewerken of wijzigen.

  Wijs de ingebouwde **lezer** -rol op de data factory-resource voor de gebruiker.

- Laat een gebruiker bewerken van een enkele data factory in Azure portal.

  Dit scenario vereist twee roltoewijzingen.

  1. Wijs de ingebouwde **Inzender** rol op het niveau van data factory.
  2. Een aangepaste rol maken met de machtiging * Microsoft.Resources/deployments/**. Deze aangepaste rol toewijzen aan de gebruiker op het niveau van de resourcegroep.

- Laat een gebruiker bijwerken van een data factory vanuit PowerShell of de SDK, maar niet in de Azure-portal.

  Wijs de ingebouwde **Inzender** -rol op de data factory-resource voor de gebruiker. Deze rol kan de gebruiker ziet de resources in Azure portal, maar de gebruiker geen toegang tot de **publiceren** en **Alles publiceren** knoppen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over functies in Azure - [roldefinities begrijpen](../role-based-access-control/role-definitions.md)

- Meer informatie over de **Inzender Data Factory** role - [rol van Inzender Data Factory](../role-based-access-control/built-in-roles.md#data-factory-contributor).