---
title: De groepen die uw groep bij Azure Active Directory behoort beheren | Microsoft Docs
description: Groepen kunnen andere Azure Active Directory-groepen bevatten. Hier volgt deze lidmaatschappen beheren.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 251289d2a70f824714789fdf2fb484949745d6d7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Beheren welke groepen door een groep behoort in uw Azure Active Directory-tenant
Groepen kunnen andere Azure Active Directory-groepen bevatten. Hier volgt deze lidmaatschappen beheren.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Hoe kan ik de groepen waarvan mijn groep lid vinden?
1. Meld u aan bij het [beheercentrum van Azure AD](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **gebruikers en groepen**.

   ![Openen gebruikers en groepen installatiekopie](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. Selecteer **alle groepen**.

   ![Installatiekopie van de groepen te selecteren](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Selecteer een groep.
2. Selecteer **groepslidmaatschappen**.

   ![Afbeelding van lidmaatschap van de groep openen](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. Uw groep toevoegen als lid van een andere groep, op de **Group - groepslidmaatschappen** blade, selecteer de **toevoegen** opdracht.
2. Selecteer een groep in de **groep selecteren** blade en selecteer vervolgens de **Selecteer** knop aan de onderkant van de blade. U kunt uw groep toevoegen aan slechts één groep tegelijk. De **gebruiker** vak gefilterd op basis van overeenkomst van uw invoer voor een deel van de naam van een gebruiker of het apparaat weergegeven. Er is geen jokertekens worden in dit vak geaccepteerd.

   ![Geen groepslidmaatschap toevoegen](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Verwijderen van uw groep als lid van een andere groep voor de **Group - groepslidmaatschappen** blade, selecteert u een groep.
9. Selecteer de **verwijderen** opdracht in en Bevestig uw keuze bij de opdrachtprompt.

   ![lidmaatschap opdracht verwijderen](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Wanneer u klaar bent met het wijzigen van groepslidmaatschappen voor uw groep, selecteert u **opslaan**.

## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toe te voegen](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
