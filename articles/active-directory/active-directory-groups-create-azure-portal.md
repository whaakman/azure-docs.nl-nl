---
title: Maakt een groep voor gebruikers in Azure Active Directory | Microsoft Docs
description: Het maken van een groep in Azure Active Directory en leden toevoegen aan de groep
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d3d37761a9fdf9bd9801396d45f2fcd47efb0be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Een groep maken en leden toevoegen in Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [Klassieke Azure Portal](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

In dit artikel wordt uitgelegd hoe u maakt en vult een nieuwe groep in Azure Active Directory. Een groep gebruiken voor beheertaken uitvoert zoals het toewijzen van licenties of machtigingen voor een aantal gebruikers of apparaten in één keer.

## <a name="how-do-i-create-a-group"></a>Hoe maak ik een groep?
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **meer services**, voer **gebruikers en groepen** in het tekstvak in en selecteer vervolgens **Enter**.

   ![Gebruikersbeheer openen](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Op de **gebruikers en groepen** blade Selecteer **alle groepen**.

   ![De blade groepen openen](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Op de **gebruikers en groepen - alle groepen** blade, selecteer de **toevoegen** opdracht.

   ![De opdracht Add selecteren](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Op de **groep** blade een naam en beschrijving voor de groep toevoegen.
6. Als u wilt leden toevoegen aan de groep selecteert, selecteert u **toegewezen** in de **lidmaatschapstype** vak en selecteer vervolgens **leden**. Zie voor meer informatie over het beheren van het lidmaatschap van een groep dynamisch [kenmerken gebruiken voor het maken van geavanceerde regels voor lidmaatschap](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Leden toevoegen selecteren](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Op de **leden** blade, selecteer een of meer gebruikers of apparaten aan de groep wilt toevoegen en selecteer de **Selecteer** knop aan de onderkant van de blade aan toe te voegen aan de groep. De **gebruiker** vak gefilterd op basis van overeenkomst van uw invoer voor een deel van de naam van een gebruiker of het apparaat weergegeven. Er is geen jokertekens worden in dit vak geaccepteerd.
8. Wanneer u klaar bent met het toevoegen van leden aan de groep, selecteert u **maken** op de **groep** blade.    

   ![Bevestiging van de groep maken](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
