---
title: Azure-resource-rollen in PIM toewijzen | Microsoft Docs
description: Informatie over het toewijzen van rollen voor Azure-resource in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 08/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87c92387c14cd0b82ec26acb411609c794c2601e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171261"
---
# <a name="assign-azure-resource-roles-in-pim"></a>Azure-resource-rollen in PIM toewijzen

Azure AD PIM kunt beheren de ingebouwde Azure-resourcerollen, evenals aangepaste rollen, inclusief (maar niet beperkt tot):

- Eigenaar
- Beheerder van gebruikerstoegang
- Inzender
- Beveiligingsbeheerder
- Beveiligingsbeheer en meer

>[!NOTE]
Gebruikers of leden van een groep die is toegewezen aan de rollen eigenaar of Administrator voor gebruikerstoegang en globale beheerders waarmee beheer van abonnementen in Azure AD zijn Resource-beheerders. Deze beheerders kunnen rollen toewijzen, configureren van instellingen voor de sitesysteemrol en toegang met behulp van PIM voor Azure-resources beoordelen. De lijst weergeven met [ingebouwde rollen voor Azure-resources](../../role-based-access-control/built-in-roles.md).

## <a name="assign-a-role"></a>Een rol toewijzen

Volg deze stappen voor het maken van een gebruiker in aanmerking komen voor de rol van een Azure-resource.

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) met een gebruiker die lid is van de [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

    Zie voor meer informatie over het verlenen van een andere beheerderstoegang voor het beheren van PIM [toegang verlenen aan andere beheerders voor het beheren van PIM](pim-how-to-give-access-to-pim.md).

1. Open **Azure AD Privileged Identity Management**.

    Als u nog PIM in Azure portal nog niet hebt gestart, gaat u naar [beginnen met PIM](pim-getting-started.md).

1. Klik op **Azure-resources**.

1. Gebruik de **resourcefilter** om de lijst van beheerde resources te filteren.

    ![Lijst met Azure-resources te beheren](./media/pim-resource-roles-assign-roles/resources-list.png)

1. Klik op de resource die u beheren wilt, zoals een abonnement of beheergroep-groep.

1. Klik onder beheren, op **rollen** voor een overzicht van de rollen voor Azure-resources.

    ![Azure-resources-rollen](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. Klik op **lid toevoegen** om de nieuwe Toewijzingsdeelvenster te openen.

1. Klik op **Selecteer een rol** Selecteer een rol om deelvenster te openen.

    ![Nieuwe van Toewijzingsdeelvenster](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. Klik op een rol die u wilt toewijzen en klik vervolgens op **Selecteer**.

    Selecteer een lid of een groep deelvenster wordt geopend.

1. Klik op een lid of een groep die u wilt toewijzen aan de rol en klik vervolgens op **Selecteer**.

    ![Een deelvenster lid of groep selecteren](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

    Het deelvenster van de instellingen voor lidmaatschap wordt geopend.

1. In de **toewijzingstype** in de lijst met **in aanmerking komende** of **Active**.

    ![Deelvenster voor lidmaatschappen-instellingen](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    PIM voor Azure-resources biedt twee afzonderlijke toewijzingstypen:

    - **In aanmerking komende** toewijzingen moeten het lid van de rol van een actie voor het gebruik van de rol uit te voeren. Acties kunnen bijvoorbeeld een meervoudige verificatiecontrole, het opgeven van een zakelijke reden of het vragen om toestemming bij aangewezen fiatteurs zijn.

    - **Actieve** toewijzingen vereisen het lid aan een actie voor het gebruik van de rol wordt uitgevoerd. Leden toegewezen als actief hebben de bevoegdheden die zijn toegewezen aan de rol te allen tijde.

1. Als de toewijzing permanente moet (permanent in aanmerking komende of definitief toegewezen), selecteer de **permanent** selectievakje.

    Afhankelijk van de functie-instellingen, het selectievakje in mogelijk niet weergegeven of unmodifiable kan zijn.

1. Geef een duur van de specifieke toewijzing, schakel het selectievakje uit en de begin-en/of end vakken voor datum en tijd wijzigen.

    ![Instellingen voor lidmaatschappen - datum en tijd](./media/pim-resource-roles-assign-roles/resources-membership-settings-date.png)

1. Klik op **Gereed** als u klaar bent.

    ![Nieuwe toewijzing - toevoegen](./media/pim-resource-roles-assign-roles/resources-new-assignment-add.png)

1. Klik op om de nieuwe roltoewijzing **toevoegen**. Een melding van de status wordt weergegeven.

    ![Nieuwe toewijzing - melding](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Bijwerken of verwijderen van een bestaande roltoewijzing

Volg deze stappen als u wilt bijwerken of verwijderen van een bestaande roltoewijzing.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Klik op de resource die u beheren wilt, zoals een abonnement of beheergroep-groep.

1. Klik onder beheren, op **rollen** voor een overzicht van de rollen voor Azure-resources.

    ![Azure-resourcerollen - rol selecteren](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. Klik op de rol die u wilt bijwerken of verwijderen.

1. De roltoewijzing vinden op de **in aanmerking komende rollen** of **actieve rollen** tabbladen.

    ![Bijwerken of verwijderen van roltoewijzing](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. Klik op **bijwerken** of **verwijderen** bijwerken of verwijderen van de roltoewijzing.

    Zie voor meer informatie over het uitbreiden van een roltoewijzing [uitbreiden of vernieuwen van Azure-resource-rollen in PIM](pim-resource-roles-renew-extend.md).

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM verlengen of vernieuwen](pim-resource-roles-renew-extend.md)
- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
