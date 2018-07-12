---
title: Rollen voor Azure-resources met behulp van Privileged Identity Management activeren | Microsoft Docs
description: Beschrijft hoe u rollen in PIM activeren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 483f30a6546cf21d93b96b08eede6afae828d9e4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38548301"
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Rollen voor Azure-resources activeren met behulp van Privileged Identity Management
Privileged Identity Management (PIM) introduceert een nieuwe ervaring in het activeren van rollen voor Azure-resources. Leden van een in aanmerking komende rol kunnen activeren voor een toekomstige datum en tijd plannen. Ze kunnen ook een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders) selecteren. Zie voor meer informatie, [activeren of deactiveren van rollen in Azure AD Privileged Identity Management](pim-how-to-activate-role.md).

## <a name="activate-roles"></a>Rollen activeren
Blader naar de **mijn rollen** sectie in het linkerdeelvenster. Selecteer **activeren** voor de rol die u wilt activeren.

!["In aanmerking komende rollen" tabblad het deelvenster 'Mijn functies'.](media/azure-pim-resource-rbac/rbac-roles.png)

Uit de **activeringen** menu, voer de datum en tijd om de rol te activeren. (Optioneel) de duur van de activering te verlagen (de hoeveelheid tijd die de rol actief is) en een reden opgeven indien nodig. Selecteer **activeren**.

Als de datum en tijd zijn niet gewijzigd, wordt de rol geactiveerd in seconden. In de **mijn rollen** in het deelvenster in een bannerbericht aangegeven dat een rol in de wachtrij voor activering is geplaatst. Selecteer de vernieuwknop om te wissen van dit bericht.

![Deelvenster 'Mijn rollen, met een banner weergegeven en een melding over een goedkeuring in behandeling](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Als de activering is gepland voor een toekomstige datum en tijd, de aanvraag in behandeling wordt weergegeven op de **aanvragen in behandeling** tabblad van het linkerdeelvenster. Als de rolactivering niet langer vereist is, kunt u de aanvraag annuleren door het selecteren van de **annuleren** knop.

![Lijst met openstaande aanvragen met de knoppen "Annuleren"](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="apply-just-enough-administration-practices"></a>Just Enough Administration-praktijken toepassen

Met behulp van aanbevolen procedures voor alleen Enough Administration (JEA) met de roltoewijzingen van uw resource is heel eenvoudig met PIM voor Azure-resources. Gebruikers en leden van de groep met toewijzingen in de Azure-abonnementen of resourcegroepen kunnen activeren van hun bestaande roltoewijzing met een lagere bereik. 

Vanuit de zoekpagina kunt u de onderliggende bron die u nodig hebt voor het beheren van vinden.

![Selecteer een resource](media/azure-pim-resource-rbac/azure-resources-02.png)

Selecteer **mijn rollen** in het linkerdeelvenster en kies de juiste rol te activeren. Het toewijzingstype wordt **overgenomen** omdat de rol is toegewezen aan het abonnement, in plaats van op de resourcegroep.

![Lijst met in aanmerking komende roltoewijzingen, met het toewijzingstype is gemarkeerd](media/azure-pim-resource-rbac/my-roles-02.png)
