---
title: De leden voor een groep in Azure Active Directory beheren | Microsoft Docs
description: Toevoegen of verwijderen van gebruikers en apparaten uit een groep in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 49c975aa34f28c6c00591435726e538bb79a78cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Groepslidmaatschap voor gebruikers in uw Azure Active Directory-tenant beheren
In dit artikel wordt uitgelegd hoe u voor het beheren van de leden voor een groep in Azure Active Directory (Azure AD).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Hoe ik de leden vinden en ze beheren?
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **meer services**, voer **gebruikers en groepen** in het tekstvak in en selecteer vervolgens **Enter**.

   ![Gebruikersbeheer openen](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Op de **gebruikers en groepen** blade Selecteer **alle groepen**.

   ![De blade groepen openen](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Op de **gebruikers en groepen - alle groepen** blade, selecteert u een groep.
5. Op de **groep - *groupname***  blade Selecteer **leden**.

   ![De blade leden openen](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Leden toevoegen aan de groep op de **groep - leden** blade Selecteer **leden toevoegen**.

   ![De opdracht leden toevoegen](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Op de **leden** blade, selecteer een of meer gebruikers of apparaten aan de groep wilt toevoegen en selecteer de **Selecteer** knop aan de onderkant van de blade aan toe te voegen aan de groep. De **gebruiker** vak gefilterd op basis van overeenkomst van uw invoer voor een deel van de naam van een gebruiker of het apparaat weergegeven. Er is geen jokertekens worden in dit vak geaccepteerd.
8. Leden verwijderen uit de groep op de **groep - leden** blade, selecteert u een lid.
9. Op de ***membername*** blade, selecteer de **verwijderen** opdracht in en Bevestig uw keuze bij de opdrachtprompt.

   ![Verwijder leden opdracht](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Wanneer u klaar bent met het wijzigen van de leden van de groep, selecteert u **opslaan**.

## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toe te voegen](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
