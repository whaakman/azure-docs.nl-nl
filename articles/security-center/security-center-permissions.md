---
title: Machtigingen in Azure Security Center | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe Azure Security Center maakt gebruik van op rollen gebaseerd toegangsbeheer machtigingen toewijzen aan gebruikers en identificeert de toegestane acties voor elke rol.
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: b93b57d50ccf5d5dfb092bdb71820da77f345878
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44295459"
---
# <a name="permissions-in-azure-security-center"></a>Machtigingen in Azure Security Center

Azure Security Center maakt gebruik van [Role-Based Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md), waarmee u [ingebouwde rollen](../role-based-access-control/built-in-roles.md) die kunnen worden toegewezen aan gebruikers, groepen en services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen met zich mee en beveiligingsproblemen te identificeren. In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of de resourcegroep die deel uitmaakt van een resource om te worden toegewezen.

Naast deze rollen zijn er twee specifieke Security Center-rollen:

* **Beveiligingslezer**: een gebruiker die deel uitmaakt van deze rol heeft rechten om Security Center te bekijken. De gebruiker kunt weergeven, aanbevelingen, waarschuwingen, een beveiligingsbeleid en beveiliging, maar kan geen wijzigingen aanbrengen.
* **Beveiligingsbeheerder**: een gebruiker die deel uitmaakt van deze rol heeft dezelfde toegangsrechten als de Beveiligingslezer kan ook het beveiligingsbeleid bijwerken en negeren van waarschuwingen en aanbevelingen.

> [!NOTE]
> De beveiligingsrollen Beveiligingslezer en Beveiligingsbeheerder, hebben toegang alleen in Security Center. De beveiligingsrollen hebt geen toegang tot andere servicegebieden van Azure, zoals Storage, Web & Mobile of Internet of Things.
>
>

## <a name="roles-and-allowed-actions"></a>Functies en toegestane acties

De volgende tabel geeft de rollen en toegestane acties in Security Center. Een X geeft aan dat de actie is toegestaan voor die rol.

| Functie | Beveiligingsbeleid bewerken | Beveiligingsaanbevelingen voor een resource toepassen | Negeren van waarschuwingen en aanbevelingen | Waarschuwingen weergeven en aanbevelingen |
|:--- |:---:|:---:|:---:|:---:|
| Abonnementhouder | X | X | X | X |
| Inzender abonnementen | X | X | X | X |
| De eigenaar van de resourcegroep | -- | X | -- | X |
| Inzender voor resourcegroep | -- | X | -- | X |
| Lezer | -- | -- | -- | X |
| Beveiligingsbeheerder | X | -- | X | X |
| Beveiligingslezer | -- | -- | -- | X |

> [!NOTE]
> We raden u aan de rol toe te wijzen die gebruikers minimaal nodig hebben om hun taken uit te voeren. Bijvoorbeeld, de rol van lezer toewijzen aan gebruikers die alleen willen informatie weergeven over de beveiligingsstatus van de resource van een resource, maar geen maatregelen, zoals het toepassen van aanbevelingen of bewerken van beleid.
>
>

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt uitgelegd hoe Security Center maakt gebruik van RBAC machtigingen toewijzen aan gebruikers en de toegestane acties voor elke rol geïdentificeerd. Nu dat u bekend met de roltoewijzingen die nodig zijn bent voor het bewaken van de beveiligingsstatus van uw abonnement, beveiligingsbeleid, bewerken en toepassen van aanbevelingen, informatie over hoe u:

- [Beveiligingsbeleid instellen in Security Center](security-center-policies.md)
- [Aanbevelingen voor beveiliging in Security Center beheren](security-center-recommendations.md)
- [De beveiligingsstatus van uw Azure-resources controleren](security-center-monitoring.md)
- [Beveiligingswaarschuwingen beheren en erop reageren in Security Center](security-center-managing-and-responding-alerts.md)
- [Bewaken van beveiligingsoplossingen van partners](security-center-partner-solutions.md)
