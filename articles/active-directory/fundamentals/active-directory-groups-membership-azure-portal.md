---
title: De groepen beheren waartoe u groep behoort in Azure AD | Microsoft Docs
description: Groepen kunnen andere groepen bevatten in Azure Active Directory. Hier ziet u hoe u deze lidmaatschappen beheert.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 8a71677ae3ceb5617f0a817a8eff438d5e3f2774
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860329"
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Beheren bij welke groepen een groep hoort in uw Azure Active Directory-tenant
Groepen kunnen andere groepen bevatten in Azure Active Directory. Hier ziet u hoe u deze lidmaatschappen beheert.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Hoe vind ik de groepen waarvan mijn groep lid is?
1. Meld u aan bij het [beheercentrum van Azure AD](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **Gebruikers en groepen**.

   ![Afbeelding: Gebruikers en groepen openen](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. Selecteer **Alle groepen**.

   ![Afbeelding: Groepen selecteren](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Selecteer een groep.
2. Selecteer **Groepslidmaatschappen**.

   ![Afbeelding: Groepslidmaatschappen openen](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. Als u uw groep wilt toevoegen als lid van een andere groep, selecteert u op de blade **Groep - groepslidmaatschappen** de opdracht **Toevoegen**.
2. Selecteer een groep op de blade **Groep selecteren** en selecteer vervolgens onder aan de blade de knop **Selecteren**. U kunt uw groep slechts aan één groep tegelijk toevoegen. Met het vak **Gebruiker** wordt de weergave gefilterd op basis van overeenkomsten tussen uw invoer en (een deel van) de naam van een gebruiker of apparaat. U kunt geen jokertekens gebruiken in dit vak.

   ![Een groepslidmaatschap toevoegen](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Als u uw groep wilt verwijderen als lid van een andere groep, selecteert u een groep op de blade **Groep - groepslidmaatschappen**.
9. Selecteer de opdracht **Verwijderen** en bevestig uw keuze wanneer u hierom wordt gevraagd.

   ![opdracht lidmaatschap verwijderen](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Wanneer u klaar bent met het wijzigen van groepslidmaatschappen voor uw groep, selecteert u **Opslaan**.

## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Bestaande groepen weergeven](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](../users-groups-roles/groups-dynamic-membership.md)
