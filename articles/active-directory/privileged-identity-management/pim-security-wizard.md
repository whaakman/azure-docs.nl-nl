---
title: De beveiligingswizard in PIM - Azure | Microsoft Docs
description: Beschrijving van de beveiligingswizard die wordt weergegeven de eerste keer dat u Azure AD Privileged Identity Management (PIM) gebruikt.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 178a4c5e978075f2a59b22a1cccf462138527964
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189076"
---
# <a name="security-wizard-in-pim"></a>De beveiligingswizard in PIM
Als u de eerste persoon om uit te voeren van Azure Privileged Identity Management (PIM) voor uw organisatie bent, u krijgt een wizard. De wizard helpt u inzicht in de beveiligingsrisico's van bevoegde identiteiten en hoe u PIM gaat gebruiken om te beperken die risico's. U hoeft te wijzigingen aanbrengt in bestaande roltoewijzingen in de wizard, als u liever later doen.

## <a name="what-to-expect"></a>Wat u kunt verwachten
Voordat uw organisatie wordt gestart met behulp van PIM, alle roltoewijzingen zijn permanent: de gebruikers zich altijd in deze rollen, zelfs als ze geen momenteel hun bevoegdheden nodig.  De eerste stap van de wizard ziet u een lijst met hoge bevoegdheden rollen en hoeveel gebruikers zijn momenteel in deze rollen. U kunt inzoomen op een bepaalde rol voor meer informatie over gebruikers als er een of meer van deze niet bekend bent.

De tweede stap van de wizard hebt u de mogelijkheid om te wijzigen van roltoewijzingen van beheerder.  

> [!WARNING]
> Het is belangrijk dat u ten minste één globale beheerder, en meer dan één beheerder met bevoorrechte rol met een organisatieaccount (niet een Microsoft-account hebt). Als er slechts één beheerder met bevoorrechte rol, zich de organisatie niet kunnen PIM beheren als dit account wordt verwijderd.
> Houd er ook roltoewijzingen permanente als een gebruiker een Microsoft-account (een account dat ze gebruiken om aan te melden heeft bij Microsoft-services zoals Skype en Outlook.com). Als u van plan bent om MFA te vereisen voor activering voor die rol, wordt die gebruiker worden vergrendeld.
> 
> 

Nadat u wijzigingen hebt aangebracht, worden de wizard niet meer weergegeven. De volgende keer dat u of een andere beheerder met bevoorrechte rol met PIM, ziet u de PIM-dashboard.  

* Meer informatie als u wilt toevoegen of verwijderen van gebruikers van rollen of wijzigen van de toewijzingen van permanente in aanmerking komende, [toevoegen of verwijderen van de rol van een gebruiker](pim-how-to-add-role-to-user.md).
* Als u wilt meer gebruikers toegang geven tot PIM beheren, lees meer op [geven toegang tot het beheer in PIM](pim-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Volgende stappen

- [PIM gebruiken](pim-getting-started.md)
- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
- [Toegang verlenen aan andere beheerders voor het beheren van PIM](pim-how-to-give-access-to-pim.md)
