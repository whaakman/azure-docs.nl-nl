---
title: Toegang verlenen aan andere beheerders voor het beheren van Azure Active Directory voor PIM - | Microsoft Docs
description: Leer hoe u toegang wilt verlenen aan andere beheerders uitgebreide mogelijkheden voor het beheren van Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb22e3cc93baebac023c0148812c6a4c6c95be60
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60288618"
---
# <a name="grant-access-to-other-administrators-to-manage-pim"></a>Toegang verlenen aan andere beheerders voor het beheren van PIM

De hoofdbeheerder die Azure Active Directory (Azure AD) Privileged Identity Management (PIM) voor een organisatie automatisch kunnen ophalen roltoewijzingen en toegang tot PIM. Niemand anders wordt schrijftoegang standaard, met inbegrip van andere globale beheerders. Andere globale beheerders, Beveiligingsbeheerders en Beveiligingslezers hebben alleen-lezen toegang tot PIM. Om toegang te verlenen tot PIM, de eerste gebruiker, anderen kunt toewijzen de **beheerder met bevoorrechte rol** rol.

> [!NOTE]
> PIM beheren, moet Azure MFA. Omdat Microsoft-accounts niet voor Azure MFA registreren kunnen, geen PIM toegang tot een gebruiker die zich met een Microsoft-account aanmeldt.

Zorg ervoor dat er altijd ten minste twee gebruikers in een rol beheerder met bevoorrechte rol in het geval één gebruiker is vergrendeld of hun account wordt verwijderd.

## <a name="grant-access-to-manage-pim"></a>Toegang verlenen tot PIM beheren

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-rollen**.

1. Klik op **rollen**.

    ![Rollen in PIM-Azure AD - rollen](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. Klik op de **beheerder met bevoorrechte rol** rol om de ledenpagina te openen.

    ![Beheerder met bevoorrechte rol - leden](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. Klik op **lid toevoegen** om het deelvenster van de beheerde leden toevoegen te openen.

1. Klik op **leden selecteren** om het deelvenster selecteren leden te openen.

    ![Beheerder met bevoorrechte rol - leden selecteren](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. Selecteer een lid en klik vervolgens op **Selecteer**.

1. Klik op **OK** om het lid in aanmerking komen voor de **beheerder met bevoorrechte rol** rol.

    Wanneer u een nieuwe rol aan iemand in PIM toewijzen, ze automatisch worden geconfigureerd als **in aanmerking komende** om de rol te activeren.

1. Het lid als permanent wilt maken, klikt u op de gebruiker in de lijst met leden van de beheerder met bevoorrechte rol.

1. Klik op **meer** en vervolgens **permanent maken** de toewijzing om permanent te maken.

    ![Beheerder met bevoorrechte rol - permanent maken](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. Verzenden naar de gebruiker een koppeling naar [beginnen met PIM](pim-getting-started.md).

## <a name="remove-access-to-manage-pim"></a>Toegang tot het beheer van PIM verwijderen

Voordat u iemand uit de rol beheerder met bevoorrechte rol verwijdert, zorg er altijd zullen nog steeds er ten minste twee gebruikers die zijn toegewezen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-rollen**.

1. Klik op **rollen**.

1. Klik op de **beheerder met bevoorrechte rol** rol om de ledenpagina te openen.

1. Schakel het selectievakje naast de gebruiker die u wilt verwijderen en klik vervolgens op **lid verwijderen**.

    ![Beheerder met bevoorrechte rol - lid verwijderen](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. Klik in het bericht dat gevraagd wordt of u wilt het lid verwijderen uit de rol, **Ja**.

## <a name="next-steps"></a>Volgende stappen

- [Beginnen met PIM](pim-getting-started.md)
