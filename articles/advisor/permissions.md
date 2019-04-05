---
title: Machtigingen in Azure Advisor
description: Advisor-machtigingen en hoe ze de mogelijkheid om te configureren van abonnementen of uitstellen of negeren aanbevelingen kunnen blokkeren.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 04/03/2019
ms.author: kasparks
ms.openlocfilehash: cbd2e456c96dbf8ca01387f0c7c17a1541dbfe55
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2019
ms.locfileid: "59052790"
---
# <a name="permissions-in-azure-advisor"></a>Machtigingen in Azure Advisor

Azure Advisor biedt aanbevelingen op basis van het gebruik en de configuratie van uw Azure-resources en abonnementen. Advisor maakt gebruik van de [ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) geleverd door [Role-Based Access Control](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC) voor het beheren van uw toegang tot aanbevelingen en Advisor-onderdelen. 

## <a name="roles-and-their-access"></a>Functies en hun toegang

De volgende tabel bevat de rollen en de toegang die ze binnen Advisor hebben:

| **Rol** | **Aanbevelingen bekijken** | **Regels bewerken** | **Abonnementsconfiguratie van bewerken** | **Configuratie van de bewerken**| **Sluiten en aanbevelingen uitstellen**|
|---|:---:|:---:|:---:|:---:|:---:|
|Abonnementhouder|**X**|**X**|**X**|**X**|**X**|
|Bijdrager aan het abonnement|**X**|**X**|**X**|**X**|**X**|
|Abonnementslezer|**X**|--|--|--|--|
|Resourcegroep eigenaar|**X**|--|--|**X**|**X**|
|Resourcegroep Inzender|**X**|--|--|**X**|**X**|
|Resourcegroep lezer|**X**|--|--|--|--|
|Resource-eigenaar|**X**|--|--|--|**X**|
|Resource-Inzender|**X**|--|--|--|**X**|
|Resource-lezer|**X**|--|--|--|--|

> [!NOTE]
> Toegang tot het weergeven van aanbevelingen is afhankelijk van uw toegang tot de betrokken resource van de aanbeveling.

## <a name="permissions-and-unavailable-actions"></a>Machtigingen en acties niet beschikbaar

Gebrek aan de juiste machtigingen kan de mogelijkheid om uit te voeren van acties in Advisor blokkeren. Hier volgen enkele veelvoorkomende problemen.

### <a name="unable-to-configure-subscriptions-or-resource-groups"></a>Kan geen abonnementen of resourcegroepen configureren

Wanneer u probeert te configureren-abonnementen of resourcegroepen in Advisor, ziet u dat de optie wilt opnemen of uitsluiten is uitgeschakeld. Deze status geeft aan dat u geen voldoende machtigingen voor de resourcegroep of abonnement. U lost dit probleem, informatie over hoe u [gebruikers toegang geven](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal).

### <a name="unable-to-postpone-or-dismiss-a-recommendation"></a>Kan geen uitstellen of een aanbeveling negeren

Als u een foutbericht ontvangt bij het uitstellen of te verwijderen van een aanbeveling, mag u niet voldoende machtigingen hebt. Zorg ervoor dat u ten minste Inzender-toegang tot de betrokken resource van de aanbeveling uitstellen of negeren. U lost dit probleem, informatie over hoe u [gebruikers toegang geven](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal).

## <a name="next-steps"></a>Volgende stappen

In dit artikel werd besproken hoe een overzicht van hoe Advisor RBAC gebruikt om de gebruikersmachtigingen voor beheer en het oplossen van veelvoorkomende problemen. Zie voor meer informatie over Advisor:

- [Wat is Azure Advisor?](https://docs.microsoft.com/azure/advisor/advisor-overview)
- [Aan de slag met Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-get-started)
