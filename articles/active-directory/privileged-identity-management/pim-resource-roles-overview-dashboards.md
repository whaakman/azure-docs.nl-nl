---
title: Een resource-dashboard gebruiken voor een toegangscontrole uitvoeren in PIM - Azure Active Directory | Microsoft Docs
description: Hierin wordt beschreven hoe u een resource-dashboard met een toegangscontrole uitvoeren in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5404d1b85821458aedef64b72ae635ea49aa1ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65602450"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review-in-pim"></a>Een resource-dashboard gebruiken voor een toegangscontrole uitvoeren in PIM

Een resource-dashboard kunt u een toegangscontrole uitvoeren in Azure Active Directory (Azure AD) Privileged Identity Management (PIM). Het dashboard Beheerdersweergave heeft drie primaire onderdelen:

- Een grafische weergave van rolactiveringen resource.
- Twee grafieken die de distributie van roltoewijzingen door toewijzingstype weergeven.
- Een data-gebied met betrekking tot nieuwe roltoewijzingen.

![Schermopname van het dashboard-Beheerweergave van grafieken en diagrammen](media/pim-resource-roles-overview-dashboards/rbac-overview-top.png)

![Schermafbeelding van het dashboard weergeven van de beheerder een lijst met gegevens](media/pim-resource-roles-overview-dashboards/role-settings.png)

De grafische weergave van rolactiveringen resource bevat informatie over de afgelopen zeven dagen. Deze gegevens is afgestemd op de geselecteerde resource en activeringen voor de meest voorkomende rollen (eigenaar, Inzender, beheerder van gebruikerstoegang), en voor alle rollen gecombineerd weergegeven.

Aan de rechterkant van de grafiek activeringen weergeven twee grafieken de distributie van roltoewijzingen door toewijzingstype, voor zowel gebruikers en groepen. U kunt de waarde in een percentage (of omgekeerd), wijzigen door het selecteren van een segment van de grafiek.

Onder de grafieken ziet u het aantal gebruikers en groepen met nieuwe roltoewijzingen gedurende de afgelopen 30 dagen en een lijst met functies die zijn gesorteerd op basis van totaal aantal toewijzingen (in aflopende volgorde).

## <a name="next-steps"></a>Volgende stappen

- [Een toegangsbeoordeling starten voor mijn Azure AD-resourcerollen in PIM](pim-resource-roles-start-access-review.md) 
