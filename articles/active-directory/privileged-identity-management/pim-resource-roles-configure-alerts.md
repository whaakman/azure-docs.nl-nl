---
title: Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren | Microsoft Docs
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
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 33bd13421b0c1574475516da942abe4e8c71ed52
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188829"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren
Privileged Identity Management (PIM) voor Azure-Resources genereert waarschuwingen wanneer er verdachte of unsafe activiteit in uw omgeving. Wanneer een waarschuwing wordt geactiveerd, wordt deze weergegeven op de pagina met waarschuwingen. 

![Pagina met waarschuwingen](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Waarschuwingen weergeven
Selecteer een waarschuwing om te zien van een rapport met de gebruikers of rollen die de waarschuwing, samen met herstel advies is geactiveerd.

![Rapport waarschuwingen](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Waarschuwingen
| Waarschuwing | Severity | Trigger | Aanbeveling |
| --- | --- | --- | --- |
| **Te veel eigenaren toegewezen aan een resource** |Middelgroot |Te veel gebruikers hebben de rol van eigenaar. |Bekijk de gebruikers in de lijst en opnieuw toewijzen van enkele tot minder bevoorrechte rollen. |
| **Te veel permanente eigenaren toegewezen aan een resource** |Middelgroot |Te veel gebruikers zijn definitief toegewezen aan een rol. |Bekijk de gebruikers in de lijst en sommige activering vereist voor gebruik van de rol opnieuw toewijzen. |
| **Dubbele rol gemaakt** |Middelgroot |Meerdere rollen hebben dezelfde criteria. |Gebruik slechts één van deze rollen. |


### <a name="severity"></a>Severity
* **Hoge**: direct actie vereist vanwege een schending van het beleid. 
* **Gemiddeld**: geen directe actie is vereist, maar geeft een mogelijke schending van het beleid.
* **Lage**: geen directe actie is vereist, maar stelt u een beleidswijziging van het gewenste.

## <a name="configure-security-alert-settings"></a>Instellingen voor beveiligingswaarschuwingen configureren
Op de pagina met waarschuwingen, gaat u naar **instellingen**.
![Instellingen](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Pas de instellingen op de verschillende waarschuwingen om te werken met uw omgeving en beveiligingsdoelen.
![Instellingen aanpassen](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Volgende stappen

- [Meervoudige verificatie vereisen voor Azure-resource-rollen in PIM](pim-resource-roles-require-mfa.md)
- [Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren](pim-resource-roles-configure-alerts.md)
