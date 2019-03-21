---
title: Privileged Identity Management voor Azure AD-beheerdersrollen - toewijzen | Microsoft Docs
description: Informatie over het toewijzen van Azure Active Directory-beheerdersrollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/30/2018
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3c8a7fb7a94dbca7b0e63ddaf756a536fbd0600
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58000624"
---
# <a name="assign-azure-ad-administrator-roles-in-pim"></a>Azure AD-beheerdersrollen in PIM toewijzen

Met Azure Active Directory (Azure AD), een globale beheerder kan er **permanente** roltoewijzingen van Azure AD-beheerder. Deze roltoewijzingen kunnen worden gemaakt met de [Azure-portal](../users-groups-roles/directory-assign-admin-roles.md) of met behulp van [PowerShell-opdrachten](/powershell/module/azuread#directory_roles).

De service Azure AD Privileged Identity Management (PIM) kunnen ook Privileged Role Administrators op de roltoewijzingen van permanent beheerder maken. Bovendien Privileged Role Administrators kunnen moet u gebruikers **in aanmerking komende** voor Azure AD-beheerdersrollen. Een in aanmerking komende beheerder kan de rol kunt activeren wanneer ze deze nodig hebben, en vervolgens hun machtigingen verlopen wanneer ze klaar bent.

## <a name="make-a-user-eligible-for-a-role"></a>Een gebruiker in aanmerking voor een rol maken

Volg deze stappen voor het maken van een gebruiker in aanmerking voor een Azure AD-beheerdersrol.

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) met een gebruiker die lid is van de [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

    Zie voor meer informatie over het verlenen van een andere beheerderstoegang voor het beheren van PIM [toegang verlenen aan andere beheerders voor het beheren van PIM](pim-how-to-give-access-to-pim.md).

1. Open **Azure AD Privileged Identity Management**.

    Als u nog PIM in Azure portal nog niet hebt gestart, gaat u naar [beginnen met PIM](pim-getting-started.md).

1. Klik op **Azure AD-maprollen**.

1. Klik op **rollen** of **leden**.

    ![Rollen voor Azure AD-directory](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Klik op **lid toevoegen** beheerde leden toevoegen te openen.

1. Klik op **Selecteer een rol**, klikt u op een rol die u wilt beheren, en klik vervolgens op **Selecteer**.

    ![Rol selecteren](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Klik op **leden selecteren**, selecteer de gebruikers die u wilt toewijzen aan de rol en klik vervolgens op **Selecteer**.

    ![Rol selecteren](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Klik in toevoegen beheerde leden op **OK** om toe te voegen van de gebruiker aan de rol.

1. Klik op de rol die u zojuist hebt toegewezen om te zien van de lijst met leden in de lijst met rollen.

     Wanneer de rol wordt toegewezen, de geselecteerde gebruiker wordt weergegeven in de lijst met leden als **in aanmerking komende** voor de rol.

    ![Gebruiker komt in aanmerking voor een rol](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. De gebruiker is die in aanmerking komen voor de rol, laat deze weten dat ze deze willen activeren op basis van de instructies in [mijn Azure AD-directory-rollen in PIM activeren](pim-how-to-activate-role.md).

    In aanmerking komende beheerders wordt gevraagd om u te registreren voor Azure multi-factor Authentication (MFA) tijdens de activering. Als een gebruiker niet voor MFA registreren kan of met behulp van een Microsoft-account (meestal @outlook.com), moet u deze permanent in alle hun rollen te maken.

## <a name="make-a-role-assignment-permanent"></a>Een roltoewijzing permanent maken

Standaard worden alleen nieuwe gebruikers in aanmerking voor een Azure AD-beheerdersrol. Volg deze stappen als u wilt dat een roltoewijzing om permanent te maken.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-maprollen**.

1. Klik op **leden**.

    ![Lijst met leden](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Klik op een **in aanmerking komende** rol die u wilt om permanent te maken.

1. Klik op **meer** en klik vervolgens op **maken permanent**.

    ![Roltoewijzing permanent maken](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    De functie wordt nu weergegeven als **permanente**.

    ![Lijst met leden met permanente wijziging](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Een gebruiker verwijderen van een rol

U kunt gebruikers van roltoewijzingen verwijderen, maar zorg ervoor dat er altijd ten minste één gebruiker die een globale beheerder is permanent. Als u niet zeker weet welke gebruikers moeten nog steeds hun roltoewijzingen, kunt u [een toegangscontrole voor de rol starten](pim-how-to-start-security-review.md).

Volg deze stappen voor een specifieke gebruiker verwijderen uit een Azure AD-beheerdersrol.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-maprollen**.

1. Klik op **leden**.

    ![Lijst met leden](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Klik op een roltoewijzing die u wilt verwijderen.

1. Klik op **meer** en klik vervolgens op **verwijderen**.

    ![Een rol verwijderen](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. In het bericht dat u vraagt te bevestigen, klikt u op **Ja**.

    ![Een rol verwijderen](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    De roltoewijzing is verwijderd.

## <a name="authorization-error-when-assigning-roles"></a>Autorisatiefout bij het toewijzen van rollen

Als u onlangs PIM ingeschakeld voor een abonnement en u een Autorisatiefout krijgt wanneer u probeert te maken van een gebruiker in aanmerking voor een Azure AD-beheerdersrol, komt dit mogelijk doordat de MS-PIM-service-principal heeft nog geen de juiste machtigingen. De MS-PIM-service-principal moet de [Administrator voor gebruikerstoegang](../../role-based-access-control/built-in-roles.md#user-access-administrator) rol het toewijzen van rollen voor anderen. In plaats van er wordt gewacht tot MS-PIM de rol Administrator voor gebruikerstoegang is toegewezen, kunt u deze handmatig toewijzen.

Volg deze stappen voor de rol Administrator voor gebruikerstoegang toewijzen aan de MS-PIM service-principal voor een abonnement.

1. Meld u aan bij de Azure-portal als globale beheerder.

1. Kies **alle services** en vervolgens **abonnementen**.

1. Kies uw abonnement.

1. Kies **Toegangsbeheer (IAM)**.

1. Kies **roltoewijzingen** om te zien van de huidige lijst van roltoewijzingen weergegeven bij het abonnementsbereik.

   ![Blade toegangsbeheer (IAM) voor een abonnement](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. Controleer of de **MS-PIM** service-principal is toegewezen de **Administrator voor gebruikerstoegang** rol.

1. Als dit niet het geval is, kiest u **roltoewijzing toevoegen** openen de **roltoewijzing toevoegen** deelvenster.

1. In de **rol** vervolgkeuzelijst, selecteer de **Administrator voor gebruikerstoegang** rol.

1. In de **Selecteer** lijst, zoek en selecteer de **MS-PIM** service-principal.

   ![Machtigingen voor MS-PIM toevoegen](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. Kies **opslaan** aan de rol toe te wijzen.

   Na enkele ogenblikken wordt de MS-PIM service-principal de rol Administrator voor gebruikerstoegang op het abonnementsbereik toegewezen.

   ![De rol Administrator voor gebruikerstoegang voor MS-PIM](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)


## <a name="next-steps"></a>Volgende stappen

- [Azure AD-beheerder rolinstellingen in PIM configureren](pim-how-to-change-default-settings.md)
- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
