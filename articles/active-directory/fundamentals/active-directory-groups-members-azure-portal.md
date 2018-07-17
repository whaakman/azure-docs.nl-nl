---
title: De leden van een groep beheren in Azure AD | Microsoft Docs
description: Gebruikers en apparaten toevoegen aan of verwijderen uit een groep in Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 947b0c11aba211530e3ae25d6617079bcaf2995f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860396"
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Groepslidmaatschap beheren voor gebruikers in uw Azure Active Directory-tenant
In dit artikel wordt uitgelegd hoe u de leden van een groep beheert in Azure AD (Azure Active Directory).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Hoe kan ik leden zoeken en ze beheren?
1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
2. Selecteer **Alle services**, typ **Gebruikers en groepen** in het tekstvak en selecteer vervolgens **Enter**.

   ![Gebruikersbeheer openen](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Selecteer **Alle groepen** op de blade **Gebruikers en groepen**.

   ![De blade Groepen openen](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Selecteer een groep op de blade **Gebruikers en groepen - alle groepen**.
5. Selecteer op de blade **Groepen - *groepsnaam*** de optie **Leden**.

   ![De blade Leden openen](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Als u leden wilt toevoegen aan de groep, selecteert u op de blade **Groepen - leden** de optie **Leden toevoegen**.

   ![Opdracht Leden toevoegen](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Selecteer op de blade **Leden** een of meer gebruikers of apparaten die u wilt toevoegen aan de groep, en selecteer vervolgens onder aan de blade de knop **Selecteren** om ze aan de groep toe te voegen. Met het vak **Gebruiker** wordt de weergave gefilterd op basis van overeenkomsten tussen uw invoer en (een deel van) de naam van een gebruiker of apparaat. U kunt geen jokertekens gebruiken in dit vak.
8. Als u leden wilt verwijderen uit de groep, selecteert u een lid op de blade **Groep - leden**.
9. Selecteer op de blade **lidnaam** de opdracht ***Verwijderen*** en bevestig uw keuze wanneer u hierom wordt gevraagd.

   ![opdracht Leden verwijderen](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Wanneer u klaar bent met het wijzigen van leden voor de groep, selecteert u **Opslaan**.

## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Bestaande groepen weergeven](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Lidmaatschappen van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](../users-groups-roles/groups-dynamic-membership.md)
