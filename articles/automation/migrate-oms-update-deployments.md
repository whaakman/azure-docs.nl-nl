---
title: Uw OMS-Update-implementaties te migreren naar Azure
description: In dit artikel wordt beschreven hoe u uw bestaande OMS-Update-implementaties te migreren naar Azure
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 07/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9469a5827765a9b82469ac1eedc66666231d82d6
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117446"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>Uw OMS-Update-implementaties te migreren naar Azure

De portal van Operations Management Suite (OMS) wordt [afgeschaft](../log-analytics/log-analytics-oms-portal-transition.md). Alle functionaliteit die beschikbaar in de OMS-portal voor het beheer van de Update was is beschikbaar in de Azure-portal. Dit artikel bevat informatie die u nodig als u wilt migreren naar Azure portal.

## <a name="key-information"></a>Belangrijke informatie

* Bestaande implementaties blijven werken. Wanneer u de implementatie in Azure hebt gemaakt, kunt u uw oude implementatie verwijderen van OMS.
* Alle bestaande functies die u in OMS hebt zijn beschikbaar in Azure, Zie voor meer informatie over updatebeheer, [overzicht updatebeheer](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Toegang tot de Azure portal

Klik in uw OMS-werkruimte op **openen in Azure**. Dit gaat naar de Log Analytics-werkruimte die OMS gebruikt.

![Open in Azure - OMS-portal](media/migrate-oms-update-deployments/link-to-azure-portal.png)

Klik in de Azure-portal op **Automation-Account**

![Log Analytics](media/migrate-oms-update-deployments/log-analytics.png)

Klik in uw Automation-Account op **updatebeheer** om beheer van updates te openen.

![Updatebeheer](media/migrate-oms-update-deployments/azure-automation.png)

In de toekomst gaat u rechtstreeks naar de Azure-portal onder **alle services**, selecteer **Automation-Accounts** onder **beheerhulpprogramma**, selecteer de juiste Automation Account en klik op **updatebeheer**.

## <a name="recreate-existing-deployments"></a>Bestaande implementaties opnieuw maken

Alle implementaties van updates die zijn gemaakt in de OMS-portal hebben een [opgeslagen zoekopdracht](../log-analytics/log-analytics-computer-groups.md) ook wel bekend als een computergroep, met dezelfde naam als de update-implementatie bestaat. De opgeslagen zoekopdracht bevat de lijst met computers die in de update-implementatie zijn gepland.

![Updatebeheer](media/migrate-oms-update-deployments/oms-deployment.png)

Volg deze stappen voor het gebruik van deze bestaande opgeslagen zoekopdracht:

Maken van een nieuwe update-implementatie, gaat u naar de Azure-portal het Automation-Account dat wordt gebruikt, en klik op selecteren **Update Management**. Klik op **update-implementatie plannen**.

![Implementatie van de update plannen](media/migrate-oms-update-deployments/schedule-update-deployment.png)

De **nieuwe Update-implementatie** deelvenster wordt geopend. Voer waarden in voor de eigenschappen die worden beschreven in de volgende tabel en klik vervolgens op **maken**:

Voor de Machines wilt bijwerken, selecteert u de opgeslagen zoekopdracht die worden gebruikt door de bestaande implementatie van OMS.

| Eigenschap | Beschrijving |
| --- | --- |
|Naam |Unieke naam voor het identificeren van de update-implementatie. |
|Besturingssysteem| Selecteer **Linux** of **Windows**.|
|Bij te werken computers |Voor de Machines wilt bijwerken, selecteert u de opgeslagen zoekopdracht die worden gebruikt door de bestaande implementatie van OMS. |
|Updateclassificaties|Selecteer de updateclassificaties die u nodig hebt. CentOS ondersteunt dit niet buiten het vak.|
|Updates die moeten worden uitgesloten|Voer de updates om uit te sluiten. Voor Windows, voert u het KB-artikel zonder de **KB** voorvoegsel. Voer de naam van het pakket voor Linux of een jokerteken gebruiken.  |
|Planningsinstellingen|Selecteer de tijd om te gaan en selecteer vervolgens **eenmaal** of **periodiek** voor het terugkeerpatroon.|| Onderhoudsvenster |Het aantal minuten instellen voor updates. De waarde mag niet kleiner dan 30 minuten of meer dan 6 uur. |

Klik op **geplande update-implementaties** om de status van de zojuist gemaakte update-implementatie weer te geven.

![nieuwe update-implementatie](media/migrate-oms-update-deployments/new-update-deployment.png)

Zoals eerder is vermeld, nadat uw nieuwe implementaties via Azure portal zijn geconfigureerd, kunnen de bestaande implementaties worden verwijderd uit de OMS-portal.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over updatebeheer in Azure, [updatebeheer](automation-update-management.md)