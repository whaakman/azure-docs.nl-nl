---
title: Privileged Identity Management voor Azure-Resources - rollen activeren | Microsoft Docs
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
ms.openlocfilehash: 3e5456e7a632639cb82d7ba2b2e073938b1798ef
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---activate"></a>Privileged Identity Management - resourcerollen - activeren
Rollen activeren voor Azure-Resources introduceert een nieuwe ervaring waarmee de leden van een in aanmerking komende rol activeren voor een toekomstige datum/tijd plannen en selecteert u een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders). Meer informatie over [hier Azure AD-rollen activeren](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Rollen activeren
Navigeer naar de mijn rollen sectie op de linkernavigatiebalk. Klik op 'Activeren' voor de rol die u activeren wilt in.
![](media/azure-pim-resource-rbac/rbac-roles.png)

Voer in het menu activeringen, de gewenste begindatum en de tijd om de rol te activeren. Eventueel de activeringsduur van de te verlagen (hoe lang de functie actief is) en voer een reden indien nodig; klikt u op activeren.

Als de begindatum en -tijd niet worden gewijzigd, wordt de rol binnen enkele seconden worden geactiveerd. U ziet dat een rol voor activering bannerbericht aangegeven op de pagina Mijn rollen in de wachtrij. Klik op de knop Vernieuwen om te wissen van dit bericht.

![](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Als de activering is gepland voor een tijdstip in de toekomst, wordt de aanvraag in behandeling op het tabblad aanvragen in behandeling van het navigatiemenu links weergegeven. In het geval van de rolactivering van de is niet langer vereist, kan de gebruiker de aanvraag annuleren door te klikken op de knop Annuleren aan de rechterkant van de pagina.

![](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="just-enough-administration"></a>Net voldoende beheer

Met behulp van de aanbevolen procedures van net voldoende administration (JEA) met uw roltoewijzingen resource is heel eenvoudig met PIM voor Azure-Resources. Gebruikers en leden van de beveiligingsgroep met toewijzingen in de Azure-abonnementen of resourcegroepen kunnen hun bestaande roltoewijzing op een verminderde scope activeren. 

Zoeken naar de onderliggende resource die u wilt beheren door de zoekpagina.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Mijn rollen selecteren in het menu linkernavigatiegedeelte en kies de juiste rol te activeren. U ziet het toewijzingstype wordt overgenomen, omdat de rol is toegewezen aan het abonnement, in plaats van de resourcegroep, zoals hieronder wordt weergegeven.

![](media/azure-pim-resource-rbac/my-roles-02.png)
