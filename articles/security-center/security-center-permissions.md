---
title: Machtigingen in Azure Security Center | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe Azure Security Center maakt gebruik van op rollen gebaseerde toegangsbeheer machtigingen toewijzen aan gebruikers en identificeert de toegestane acties voor elke rol.
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 0aaa99dda44d2020afd3e841e84020eb4ff87a85
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="permissions-in-azure-security-center"></a>Machtigingen in Azure Security Center

Het Azure Beveiligingscentrum gebruikt [op rollen gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md), dat [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md) biedt die kunnen worden toegewezen aan gebruikers, groepen en services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen en beveiligingslekken te identificeren. In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of resourcegroep die deel uitmaakt van een resource te worden toegewezen.

Naast deze rollen zijn er twee specifieke Security Center-rollen:

* **Beveiliging lezer**: een gebruiker die deel uitmaakt van deze rol heeft rechten om Security Center te bekijken. De gebruiker kunt bekijken aanbevelingen, waarschuwingen, een beveiligingsbeleid en beveiliging, maar geen wijzigingen kan aanbrengen.
* **Beveiligingsbeheerder**: een gebruiker die deel uitmaakt van deze rol heeft dezelfde rechten als de lezer van de beveiliging kan ook bijwerken van het beveiligingsbeleid en negeren van waarschuwingen en aanbevelingen.

> [!NOTE]
> De beveiligingsrollen beveiliging lezer en Beveiligingsbeheerder, hebben toegang alleen in Security Center. De beveiligingsrollen geen toegang heeft tot andere servicegebieden van Azure zoals opslag, Web & mobiele of Internet der dingen.
>
>

## <a name="roles-and-allowed-actions"></a>Functies en de toegestane acties

De volgende tabel geeft rollen en toegestane acties in Security Center. Een X geeft aan dat de actie is toegestaan voor die rol.

| Rol | Beveiligingsbeleid bewerken | Aanbevelingen voor beveiliging voor een resource toepassen | Negeren van waarschuwingen en aanbevelingen | Waarschuwingen weergeven en aanbevelingen |
|:--- |:---:|:---:|:---:|:---:|
| De eigenaar van abonnement | X | X | X | X |
| Abonnement Inzender | X | X | X | X |
| De eigenaar van de resourcegroep | -- | X | -- | X |
| Resourcegroep Inzender | -- | X | -- | X |
| Lezer | -- | -- | -- | X |
| Beveiligingsbeheerder | X | -- | X | X |
| Beveiliging lezer | -- | -- | -- | X |

> [!NOTE]
> We raden u aan de rol toe te wijzen die gebruikers minimaal nodig hebben om hun taken uit te voeren. Bijvoorbeeld: de rol Lezer toewijzen aan gebruikers die alleen informatie weergeven over de beveiligingsstatus van een bron, maar geen actie uitvoert, zoals het toepassen van aanbevelingen of het bewerken van beleid nodig hebben.
>
>

## <a name="next-steps"></a>Volgende stappen
Dit artikel wordt uitgelegd hoe Security Center RBAC gebruikt machtigingen toewijzen aan gebruikers en de toegestane acties voor elke rol geïdentificeerd. Nu u bekend met de roltoewijzingen die nodig zijn bent voor het bewaken van de beveiligingsstatus van uw abonnement, beveiligingsbeleid, bewerken en toepassen van aanbevelingen, informatie over hoe:

- [Instellen van beveiligingsbeleid in Security Center](security-center-policies.md)
- [Aanbevelingen voor beveiliging in Security Center beheren](security-center-recommendations.md)
- [De beveiligingsstatus van uw Azure-resources bewaken](security-center-monitoring.md)
- [Beveiligingswaarschuwingen beheren en erop reageren in Security Center](security-center-managing-and-responding-alerts.md)
- [Bewaken van beveiligingsoplossingen van partners](security-center-partner-solutions.md)
