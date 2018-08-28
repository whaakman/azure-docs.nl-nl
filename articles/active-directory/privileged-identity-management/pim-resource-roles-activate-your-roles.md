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
ms.component: pim
ms.date: 08/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2a5c192f231bdc75d04c78cd94838a3f341dc925
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43111055"
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Rollen voor Azure-resources activeren met behulp van Privileged Identity Management
Privileged Identity Management (PIM) introduceert een nieuwe ervaring in het activeren van rollen voor Azure-resources. Leden van een in aanmerking komende rol kunnen activeren voor een toekomstige datum en tijd plannen. Ze kunnen ook een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders) selecteren. Zie voor meer informatie, [activeren of deactiveren van rollen in Azure AD Privileged Identity Management](pim-how-to-activate-role.md).

## <a name="activate-a-role"></a>Een rol activeren
Blader naar de **mijn rollen** sectie in het linkerdeelvenster. Selecteer **activeren** voor de rol die u wilt activeren.

!["In aanmerking komende rollen" tabblad het deelvenster 'Mijn functies'.](media/azure-pim-resource-rbac/rbac-roles.png)

Uit de **activeringen** menu, voer de datum en tijd om de rol te activeren. (Optioneel) de duur van de activering te verlagen (de hoeveelheid tijd die de rol actief is) en een reden opgeven indien nodig. Selecteer **activeren**.

Als de datum en tijd zijn niet gewijzigd, wordt de rol geactiveerd in seconden. In de **mijn rollen** in het deelvenster in een bannerbericht aangegeven dat een rol in de wachtrij voor activering is geplaatst. Selecteer de vernieuwknop om te wissen van dit bericht.

![Deelvenster 'Mijn rollen, met een banner weergegeven en een melding over een goedkeuring in behandeling](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Als de activering is gepland voor een toekomstige datum en tijd, de aanvraag in behandeling wordt weergegeven op de **aanvragen in behandeling** tabblad van het linkerdeelvenster. Als de rolactivering niet langer vereist is, kunt u de aanvraag annuleren door het selecteren van de **annuleren** knop.

![Lijst met openstaande aanvragen met de knoppen "Annuleren"](media/azure-pim-resource-rbac/rbac-activate-pending.png)

## <a name="use-a-role-immediately-after-activation"></a>Gebruik een rol onmiddellijk na de activering

Vanwege de caching, treden niet onmiddellijk activeringen op in Azure portal zonder te vernieuwen. Als u nodig hebt om te beperken van de mogelijkheid van vertragingen na het activeren van een rol, kunt u de **toegang tot toepassingen** pagina in de portal. Toepassingen die via deze pagina meteen controleren of er nieuwe roltoewijzingen.

1. Open Azure AD Privileged Identity Management.

1. Klik op de **toegang tot toepassingen** pagina.

    ![Toegang tot PIM toepassingen - schermafbeelding](./media/pim-resource-roles-activate-your-roles/pim-application-access.png)

1. Klik op **Azure-resources** opnieuw openen van de portal op de **alle resources** pagina.

    Als u deze koppeling klikt, wordt u geforceerd vernieuwen en er is een controle voor nieuwe Azure-resource-roltoewijzingen.

## <a name="apply-just-enough-administration-practices"></a>Just Enough Administration-praktijken toepassen

Met behulp van aanbevolen procedures voor alleen Enough Administration (JEA) met de roltoewijzingen van uw resource is heel eenvoudig met PIM voor Azure-resources. Gebruikers en leden van de groep met toewijzingen in de Azure-abonnementen of resourcegroepen kunnen activeren van hun bestaande roltoewijzing met een lagere bereik. 

Vanuit de zoekpagina kunt u de onderliggende bron die u nodig hebt voor het beheren van vinden.

![Selecteer een resource](media/azure-pim-resource-rbac/azure-resources-02.png)

Selecteer **mijn rollen** in het linkerdeelvenster en kies de juiste rol te activeren. Het toewijzingstype wordt **overgenomen** omdat de rol is toegewezen aan het abonnement, in plaats van op de resourcegroep.

![Lijst met in aanmerking komende roltoewijzingen, met het toewijzingstype is gemarkeerd](media/azure-pim-resource-rbac/my-roles-02.png)
