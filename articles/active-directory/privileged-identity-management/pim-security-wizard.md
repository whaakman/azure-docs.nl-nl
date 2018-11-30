---
title: Azure AD-rollen beveiligingswizard in PIM | Microsoft Docs
description: Beschrijving van de beveiligingswizard die u kunt gebruiken voor Azure AD-roltoewijzingen voor permanente bevoegde converteren naar in aanmerking komende met behulp van Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 11/09/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: a38f6cac007ec130c860e3a7c5de838cb63b1675
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52497823"
---
# <a name="azure-ad-roles-security-wizard-in-pim"></a>Azure AD-rollen beveiligingswizard in PIM

Als u de eerste persoon om uit te voeren van Azure AD Privileged Identity Management (PIM) voor uw organisatie bent, u krijgt een wizard. De wizard helpt u inzicht in de beveiligingsrisico's van bevoegde identiteiten en hoe u PIM gaat gebruiken om te beperken die risico's. U hoeft te wijzigingen aanbrengt in bestaande roltoewijzingen in de wizard, als u liever later doen.

## <a name="wizard-overview"></a>De wizard-overzicht

Voordat uw organisatie wordt gestart met behulp van PIM, alle roltoewijzingen zijn permanent: de gebruikers zich altijd in deze rollen, zelfs als ze geen momenteel hun bevoegdheden nodig. De eerste stap van de wizard ziet u een lijst met hoge bevoegdheden rollen en hoeveel gebruikers zijn momenteel in deze rollen. U kunt inzoomen op een bepaalde rol voor meer informatie over gebruikers als er een of meer van deze niet bekend bent.

De tweede stap van de wizard hebt u de mogelijkheid om te wijzigen van roltoewijzingen van beheerder.  

> [!WARNING]
> Het is belangrijk dat u ten minste één globale beheerder, en meer dan één beheerder met bevoorrechte rol met een organisatieaccount (niet een Microsoft-account hebt). Als er slechts één beheerder met bevoorrechte rol, zich de organisatie niet kunnen PIM beheren als dit account wordt verwijderd.
> Houd er ook roltoewijzingen permanente als een gebruiker een Microsoft-account (een account dat ze gebruiken om aan te melden heeft bij Microsoft-services zoals Skype en Outlook.com). Als u van plan bent om MFA te vereisen voor activering voor die rol, wordt die gebruiker worden vergrendeld.

## <a name="run-the-wizard"></a>De wizard uitvoeren

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-rollen** en klik vervolgens op **Wizard**.

    ![Azure AD-rollen - Wizard](./media/pim-security-wizard/wizard-start.png)

1. Klik op **1 ontdekken bevoorrechte rollen**.

1. Bekijk de lijst met bevoorrechte rollen om te zien welke gebruikers zijn permanente of die in aanmerking komen.

    ![Gebruikers in bevoorrechte rollen detecteren](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Klik op **volgende** om te selecteren van de leden die u wilt maken die in aanmerking komen.

    ![Status van gebruikers wijzigen naar in aanmerking komend](./media/pim-security-wizard/convert-members-eligible.png)

1. Nadat u de leden hebt geselecteerd, klikt u op **volgende**.

    ![Wijzigingen controleren](./media/pim-security-wizard/review-changes.png)

1. Klik op **OK** de permanente toewijzingen converteren naar in aanmerking komende.

    Wanneer de conversie is voltooid, ziet u een melding.

    ![Meldingen](./media/pim-security-wizard/notification-completion.png)

Als u omzetten in andere toewijzingen van bevoorrechte rollen die in aanmerking komen wilt, kunt u de wizard opnieuw uitvoeren. Als u de PIM-interface gebruiken in plaats van de wizard wilt, Zie [toewijzen van Azure AD-directory-rollen in PIM](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Volgende stappen

- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
- [Toegang verlenen aan andere beheerders voor het beheren van PIM](pim-how-to-give-access-to-pim.md)
