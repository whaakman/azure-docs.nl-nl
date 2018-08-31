---
title: Azure AD-directory-rollen in PIM toewijzen | Microsoft Docs
description: Informatie over het toewijzen van rollen voor Azure AD-directory in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 07/23/2018
ms.author: rolyon
ms.openlocfilehash: 33bfe28bf612c47c9f42345dabccc017337c3d45
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190153"
---
# <a name="assign-azure-ad-directory-roles-in-pim"></a>Azure AD-directory-rollen in PIM toewijzen

Met Azure Active Directory (Azure AD), een globale beheerder kunnen er **permanente** directory-roltoewijzingen. Deze roltoewijzingen kunnen worden gemaakt met de [Azure-portal](../users-groups-roles/directory-assign-admin-roles.md) of met behulp van [PowerShell-opdrachten](/powershell/module/azuread#directory_roles).

De service Azure AD Privileged Identity Management (PIM) kunnen ook beheerders met bevoegdheid kunnen roltoewijzingen permanente directory. Beheerders met bevoegdheid kunnen bovendien zodat gebruikers **in aanmerking komende** voor directory-rollen. Een in aanmerking komende beheerder kan de rol kunt activeren wanneer ze deze nodig hebben, en vervolgens hun machtigingen verlopen wanneer ze klaar bent. Zie voor meer informatie over de functies die u kunt beheren met PIM [Azure AD-directory-rollen in PIM kunt u beheren](pim-roles.md).

## <a name="make-a-user-eligible-for-a-role"></a>Een gebruiker in aanmerking voor een rol maken

Volg deze stappen voor het maken van een gebruiker in aanmerking voor een Azure AD-directory-rol.

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) met een gebruiker die lid is van de [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

    Zie voor meer informatie over het verlenen van een andere beheerderstoegang voor het beheren van PIM [toegang verlenen aan andere beheerders voor het beheren van PIM](pim-how-to-give-access-to-pim.md).

1. Open **Azure AD Privileged Identity Management**.

    Als u nog PIM in Azure portal nog niet hebt gestart, gaat u naar [beginnen met PIM](pim-getting-started.md).

1. Klik op **Azure AD-maprollen**.

1. Klik op **rollen** of **leden**.

    ![Rollen voor Azure AD-adreslijst](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

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

Standaard worden alleen nieuwe gebruikers in aanmerking voor een directory-rol. Volg deze stappen als u wilt dat een roltoewijzing om permanent te maken.

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

Volg deze stappen voor een specifieke gebruiker verwijderen van een directory-rol.

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

## <a name="next-steps"></a>Volgende stappen

- [Azure AD directory rolinstellingen in PIM configureren](pim-how-to-change-default-settings.md)
- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
