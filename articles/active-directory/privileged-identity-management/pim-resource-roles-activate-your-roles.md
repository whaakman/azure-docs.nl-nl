---
title: Rollen voor Azure-resources met behulp van Privileged Identity Management activeren | Microsoft Docs
description: Beschrijft hoe u de rollen in PIM activeren.
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
ms.openlocfilehash: a985e67cc566cc45b3ee6b8dc98e91a8f34abd1b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Rollen voor Azure-resources met behulp van Privileged Identity Management activeren
Privileged Identity Management (PIM) introduceert een nieuwe ervaring in rollen voor Azure-resources te activeren. Leden van een in aanmerking komende rol kunnen activeren voor een toekomstige datum en tijd plannen. Ze kunnen ook een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders) selecteren. Zie voor meer informatie [activeren of deactiveren van rollen in Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Rollen activeren
Blader naar de **mijn rollen** sectie in het linkerdeelvenster. Selecteer **activeren** voor de rol die u wilt activeren.

![Tabblad 'In aanmerking komende functies' het deelvenster 'Mijn functies'.](media/azure-pim-resource-rbac/rbac-roles.png)

Van de **activeringen** menu, voer de datum en tijd om de rol te activeren. Eventueel verlagen de activeringsduur (de lengte van de tijd dat de functie actief is) en een reden opgeven indien nodig. Selecteer **activeren**.

Als de begindatum en -tijd niet worden gewijzigd, wordt de rol wordt geactiveerd (in seconden). In de **mijn rollen** deelvenster een bannerbericht geeft aan dat een rol in de wachtrij voor activering staat. Selecteer de knop Vernieuwen om te wissen van dit bericht.

![Deelvenster van de 'Mijn functies' met een bannerbericht en een melding over een in afwachting van goedkeuring](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Als de activering is gepland voor een toekomstige datum en tijd, de aanvraag in behandeling wordt weergegeven op de **aanvragen in behandeling** tabblad van het linkerdeelvenster. Als de rolactivering van de niet langer vereist is, kunt u de aanvraag annuleren door het selecteren van de **annuleren** knop.

![Lijst met openstaande aanvragen met de knoppen 'Annuleren'](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="apply-just-enough-administration-practices"></a>Net genoeg beheerprocedures toepassen

Met behulp van aanbevolen procedures net genoeg Administration (JEA) met de roltoewijzingen resource is heel eenvoudig met PIM voor Azure-resources. Gebruikers en leden van de beveiligingsgroep met toewijzingen in de Azure-abonnementen of resourcegroepen kunnen hun bestaande roltoewijzing op een verminderde scope activeren. 

Door de zoekpagina vinden de onderliggende resource die u wilt beheren.

![Een resourcegroep selecteren](media/azure-pim-resource-rbac/azure-resources-02.png)

Selecteer **mijn rollen** in het linkerdeelvenster en kies de juiste rol te activeren. Het toewijzingstype is **overgenomen** omdat de rol is toegewezen aan het abonnement in plaats van op de resourcegroep.

![Lijst van in aanmerking komende roltoewijzingen met het toewijzingstype gemarkeerd](media/azure-pim-resource-rbac/my-roles-02.png)
