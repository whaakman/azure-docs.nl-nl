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
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 564517052796ee5dbc022ff92afcaa0216bdf8ea
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196834"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren
Privileged Identity Management (PIM) voor Azure-resources genereert waarschuwingen wanneer er verdachte of unsafe activiteit in uw omgeving. Wanneer een waarschuwing wordt geactiveerd, wordt deze weergegeven op de pagina met waarschuwingen. 

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
* **Hoge**: Onmiddellijke actie vereist vanwege een schending van het beleid. 
* **Gemiddeld**: Geen directe actie is vereist, maar geeft een mogelijke schending van het beleid.
* **Lage**: Geen directe actie is vereist, maar stelt een beleidswijziging van het gewenste.

## <a name="configure-security-alert-settings"></a>Instellingen voor beveiligingswaarschuwingen configureren
Op de pagina met waarschuwingen, gaat u naar **instellingen**.
![Instellingen](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Pas de instellingen op de verschillende waarschuwingen om te werken met uw omgeving en beveiligingsdoelen.
![Instellingen aanpassen](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Volgende stappen

- [Beveiligingswaarschuwingen voor Azure resource-rollen in PIM configureren](pim-resource-roles-configure-alerts.md)
