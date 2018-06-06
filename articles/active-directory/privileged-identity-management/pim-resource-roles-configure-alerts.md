---
title: Beveiligingswaarschuwingen voor Azure-resources beheren met behulp van Privileged Identity Management | Microsoft Docs
description: Beschrijft de PIM-beveiligingswaarschuwingen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 0ad4cde6a59a8867f9ff3b3cb9d9bded06e34256
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699580"
---
# <a name="manage-security-alerts-for-azure-resources-by-using-privileged-identity-management"></a>Beveiligingswaarschuwingen voor Azure-resources beheren met behulp van Privileged Identity Management
Privileged Identity Management (PIM) voor Azure-Resources genereert waarschuwingen wanneer er verdachte of unsafe activiteiten in uw omgeving. Wanneer een waarschuwing wordt geactiveerd, wordt deze weergegeven op de pagina waarschuwingen. 

![pagina waarschuwingen](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Waarschuwingen weergeven
Selecteer een waarschuwing voor een overzicht van een rapport met de gebruikers of rollen die de waarschuwing, samen met advies voor herstel is geactiveerd.

![Waarschuwingsrapport](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Waarschuwingen
| Waarschuwing | Severity | Trigger | Aanbeveling |
| --- | --- | --- | --- |
| **Te veel eigenaren toegewezen aan een resource** |Middelgroot |Te veel gebruikers hebben de rol van eigenaar. |Bekijk de gebruikers in de lijst en sommige aan minder bevoorrechte rollen toewijzen. |
| **Te veel permanente eigenaar worden toegewezen aan een resource** |Middelgroot |Te veel gebruikers zijn permanent toegewezen aan een rol. |Bekijk de gebruikers in de lijst en sommige moeten worden geactiveerd voor rol opnieuw toewijzen. |
| **Dubbele rol gemaakt** |Middelgroot |Meerdere rollen hebben dezelfde criteria. |Gebruik alleen een van deze rollen. |


### <a name="severity"></a>Severity
* **Hoge**: directe actie is vereist vanwege een schending van het beleid. 
* **Gemiddeld**: geen directe actie is vereist, maar geeft u een potentiële beleidsovertreding.
* **Lage**: geen directe actie is vereist, maar stelt u een beleidswijziging van het gewenste.

## <a name="configure-security-alert-settings"></a>Waarschuwing beveiligingsinstellingen configureren
Ga op de pagina waarschuwingen naar **instellingen**.
![Instellingen](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Instellingen op de andere waarschuwingen werken met uw omgeving en beveiligingsdoelen aanpassen.
![Instellingen aanpassen](media/azure-pim-resource-rbac/rbac-alert-settings.png)
