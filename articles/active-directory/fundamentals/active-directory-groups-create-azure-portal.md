---
title: Een groep voor gebruikers maken in Azure AD | Microsoft Docs
description: Lees hoe u een groep maakt in Azure Active Directory en leden toevoegt aan de groep
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/04/2017
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 82d475e5adadb4e7670f24a6193348c9e1b37a16
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37767420"
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Een groep maken en leden toevoegen in Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](../users-groups-roles/groups-settings-v2-cmdlets.md)

In dit artikel wordt uitgelegd hoe u een nieuwe groep maakt en vult in Azure Active Directory. U gebruikt een groep om beheertaken uit te voeren zoals het toewijzen van licenties of machtigingen aan een aantal gebruikers of apparaten tegelijk.

## <a name="how-do-i-create-a-group"></a>Hoe maak ik een groep?
1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
2. Selecteer **Alle services**, typ **Gebruikers en groepen** in het tekstvak en druk op **Enter**

   ![Gebruikersbeheer openen](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Selecteer **Alle groepen** op de blade **Gebruikers en groepen**.

   ![De blade Groepen openen](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Selecteer de opdracht **Toevoegen** op de blade **Gebruikers en groepen - alle groepen**.

   ![De opdracht Toevoegen selecteren](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Voeg op de blade **Groep** een naam en beschrijving toe voor de groep.
6. U kunt de leden selecteren die u wilt toevoegen aan de groep door **Toegewezen** te selecteren in de lijst **Type lidmaatschap** en vervolgens **Leden** te selecteren. Zie [Op kenmerken gebaseerde regels voor dynamisch groepslidmaatschap maken in Azure Active Directory](../active-directory-groups-dynamic-membership-azure-portal.md) voor meer informatie over het dynamisch beheren van het lidmaatschap van een groep.

   ![Leden selecteren om toe te voegen](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Selecteer op de blade **Leden** een of meer gebruikers of apparaten die u wilt toevoegen aan de groep en selecteer vervolgens de knop **Selecteren** onderaan de blade om ze aan de groep toe te voegen. De lijst **Gebruiker** wordt gefilterd op basis van de overeenkomst van uw invoer met de volledige of gedeeltelijke naam van een gebruiker of apparaat. U kunt geen jokertekens gebruiken in de lijst.
8. Wanneer u klaar bent met het toevoegen van leden aan de groep, selecteert u **Maken** op de blade **Groep**.    

   ![Bevestiging van maken van groep](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Bestaande groepen weergeven](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschappen van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](../active-directory-groups-dynamic-membership-azure-portal.md)
