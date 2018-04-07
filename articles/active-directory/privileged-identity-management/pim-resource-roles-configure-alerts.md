---
title: Privileged Identity Management voor Azure-Resources - beveiligingswaarschuwingen | Microsoft Docs
description: Beschrijft de PIM-beveiligingswaarschuwingen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 86c9a0f12b2598ffbd02810a11622b13b0363a1f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---alerts"></a>Privileged Identity Management - resourcerollen - waarschuwingen
PIM voor Azure-Resources genereert waarschuwingen wanneer er verdachte of unsafe activiteiten in uw omgeving. Wanneer een waarschuwing wordt geactiveerd, wordt deze weergegeven op de pagina waarschuwingen. 

![](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Waarschuwingen weergeven
Selecteer een waarschuwing voor een overzicht van een rapport met de gebruikers of rollen die de waarschuwing heeft geactiveerd en advies voor herstel.
![](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Waarschuwingen
| Waarschuwing | Ernst | Trigger | Aanbeveling |
| --- | --- | --- | --- |
| **Te veel eigenaren toegewezen aan een resource** |Middelgroot |Te veel gebruikers hebben de rol van eigenaar. |Bekijk de gebruikers in de lijst en sommige aan minder bevoorrechte rollen toewijzen. |
| **Te veel permanente eigenaar worden toegewezen aan een resource** |Middelgroot |Te veel gebruikers zijn permanent toegewezen aan een rol. |Bekijk de gebruikers in de lijst en sommige moeten worden geactiveerd voor rol opnieuw toewijzen. |
| **Dubbele rol gemaakt** |Middelgroot |Meerdere rollen hebben dezelfde criteria. |Gebruik alleen een van deze rollen. |


### <a name="severity"></a>Ernst
* **Hoge**: directe actie is vereist vanwege een schending van het beleid. 
* **Gemiddeld**: geen directe actie is vereist, maar geeft u een potentiële beleidsovertreding.
* **Lage**: geen directe actie is vereist, maar stelt voor een beter dan beleidswijziging.

## <a name="configure-security-alert-settings"></a>Waarschuwing beveiligingsinstellingen configureren
Ga naar instellingen op de pagina waarschuwingen.
![](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Instellingen op de andere waarschuwingen werken met uw omgeving en beveiligingsdoelen aanpassen.
![](media/azure-pim-resource-rbac/rbac-alert-settings.png)
