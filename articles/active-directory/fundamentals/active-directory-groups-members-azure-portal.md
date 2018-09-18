---
title: Toevoegen of verwijderen van leden van beveiligingsgroep met Azure Active Directory | Microsoft Docs
description: Informatie over het toevoegen of verwijderen van gebruikers en apparaten uit een groep met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 7c1a83d83dcbf247550c66602a6f53d4ef0d7930
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733361"
---
# <a name="how-to-add-or-remove-group-members-using-azure-active-directory"></a>Hoe: toevoegen of verwijderen van leden van beveiligingsgroep met Azure Active Directory
Met Azure Active Directory, kunt u blijven toevoegen en verwijderen van leden van de beveiligingsgroep.

## <a name="to-add-group-members"></a>Groepsleden toevoegen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een globale beheerdersaccount voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **groepen**.

3. Uit de **groepen - alle groepen** pagina, zoek en selecteer de groep die u wilt toevoegen van het lid. In dit geval gebruikt u de eerder gemaakte groep **MDM-beleid - West**.

    ![Pagina groepen-alle groepen in de naam van de groep is gemarkeerd](media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. Uit de **MDM-beleid - West-overzicht** weergeeft, schakelt **leden** uit de **beheren** gebied.

    ![Beleid voor MDM - West overzichtspagina met leden-optie is gemarkeerd](media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. Selecteer **leden toevoegen**, en vervolgens zoekt en selecteert u elk van de leden die u wilt toevoegen aan de groep en kies vervolgens **Selecteer**.

    U krijgt een bericht weergegeven dat de leden zijn toegevoegd.

    ![Ledenpagina toevoegen met gezocht lid wordt weergegeven](media/active-directory-groups-members-azure-portal/update-members.png)

6. Vernieuwen van het scherm om alle van de namen van leden toegevoegd aan de groep te zien.

## <a name="to-remove-group-members"></a>Groepsleden verwijderen

1. Uit de **groepen - alle groepen** pagina, zoek en selecteer de groep die u wilt voor het lid in verwijderen. Het opnieuw gebruiken we, **MDM-beleid - West**.

2. Selecteer **leden** uit de **beheren** gebied, zoekt en selecteert u de naam van het lid wilt verwijderen en selecteer vervolgens **verwijderen**.

    ![De pagina van het lid info, met de optie verwijderen](media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>Volgende stappen

- [Uw groepen en leden weergeven](active-directory-groups-view-azure-portal.md)

- [Instellingen van uw groep bewerken](active-directory-groups-settings-azure-portal.md)

- [Toegang tot resources met behulp van groepen beheren](active-directory-manage-groups.md)

- [Dynamische regels voor gebruikers in een groep beheren](../users-groups-roles/groups-create-rule.md)

- [Koppelen of een Azure-abonnement toevoegen aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
