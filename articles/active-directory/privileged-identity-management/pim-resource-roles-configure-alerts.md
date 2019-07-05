---
title: Beveiligingswaarschuwingen voor Azure resource-rollen in PIM - Azure Active Directory configureren | Microsoft Docs
description: Informatie over het configureren van beveiligingswaarschuwingen voor Azure resource-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c7ce8b79644a9ffc9481ba825ec5623a9268983
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476335"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren
Azure Active Directory (Azure AD) Privileged Identity Management (PIM) genereert waarschuwingen wanneer er verdachte of unsafe activiteit in uw omgeving. Wanneer een waarschuwing wordt geactiveerd, wordt deze weergegeven op de pagina met waarschuwingen. 

![Azure-resources - pagina met waarschuwingen aanbieding waarschuwing, risiconiveau en aantal](media/pim-resource-roles-configure-alerts/rbac-alerts-page.png)

## <a name="review-alerts"></a>Waarschuwingen weergeven
Selecteer een waarschuwing om te zien van een rapport met de gebruikers of rollen die de waarschuwing, samen met herstel advies is geactiveerd.

![Waarschuwing rapport weergeven met de laatste scan tijd, beschrijving, stappen voor risicobeperking, type, ernst, gevolgen voor de beveiliging en om te voorkomen dat de volgende keer](media/pim-resource-roles-configure-alerts/rbac-alert-info.png)

## <a name="alerts"></a>Waarschuwingen
| Waarschuwing | Severity | Trigger | Aanbeveling |
| --- | --- | --- | --- |
| **Te veel eigenaren toegewezen aan een resource** |Gemiddeld |Te veel gebruikers hebben de rol van eigenaar. |Bekijk de gebruikers in de lijst en opnieuw toewijzen van enkele tot minder bevoorrechte rollen. |
| **Te veel permanente eigenaren toegewezen aan een resource** |Gemiddeld |Te veel gebruikers zijn definitief toegewezen aan een rol. |Bekijk de gebruikers in de lijst en sommige activering vereist voor gebruik van de rol opnieuw toewijzen. |
| **Dubbele rol gemaakt** |Gemiddeld |Meerdere rollen hebben dezelfde criteria. |Gebruik slechts één van deze rollen. |


### <a name="severity"></a>Severity
* **Hoge**: Onmiddellijke actie vereist vanwege een schending van het beleid. 
* **Gemiddeld**: Geen directe actie is vereist, maar geeft een mogelijke schending van het beleid.
* **Lage**: Geen directe actie is vereist, maar stelt een beleidswijziging van het gewenste.

## <a name="configure-security-alert-settings"></a>Instellingen voor beveiligingswaarschuwingen configureren
Op de pagina met waarschuwingen, gaat u naar **instellingen**.

![Pagina met instellingen die zijn gemarkeerd met waarschuwingen](media/pim-resource-roles-configure-alerts/rbac-navigate-settings.png)

Pas de instellingen op de verschillende waarschuwingen om te werken met uw omgeving en beveiligingsdoelen.

![Pagina voor een waarschuwing inschakelen en configureren van instellingen](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
