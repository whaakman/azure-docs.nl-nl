---
title: Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory | Microsoft Docs
description: Hoe u een enterprise app selecteren om een gebruiker of groep aan toewijzen in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.openlocfilehash: 8e61044f261033a473241e2de152026bf49c4c70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory
Het is gemakkelijk een gebruiker of een groep toewijzen aan uw zakelijke toepassingen in Azure Active Directory (Azure AD). U moet de juiste machtigingen voor het beheren van de app voor de onderneming hebben en u moet een globale beheerder voor de map.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Hoe wijs ik gebruikerstoegang naar een enterprise-app
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **meer services**, Azure Active Directory invoeren in het tekstvak en selecteer vervolgens **Enter**.
3. Op de **Azure Active Directory - *directoryname***  blade (dat wil zeggen, de Azure AD blade voor de map die u beheert), selecteer **bedrijfstoepassingen**.

    ![Openen zakelijke apps](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Op de **bedrijfstoepassingen** blade Selecteer **alle toepassingen**. Hier ziet u een lijst met de apps die u kunt beheren.
5. Op de **bedrijfstoepassingen - alle toepassingen** blade, selecteert u een app.
6. Op de ***appname*** blade (dat wil zeggen, de blade met de naam van de geselecteerde app in de titel), selecteer **gebruikers en groepen**.

    ![De opdracht Alles toepassingen selecteren](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Op de ***appname*** **-gebruiker & groepstoewijzing** blade, selecteer de **toevoegen** opdracht.
8. Op de **toevoegen toewijzing** blade Selecteer **gebruikers en groepen**.

    ![Een gebruiker of groep toewijzen aan de app.](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Op de **gebruikers en groepen** blade gebruikers of groepen uit de lijst selecteren en selecteer vervolgens de **Selecteer** knop aan de onderkant van de blade.
10. Op de **toevoegen toewijzing** blade Selecteer **rol**. Klik op de **rol selecteren** blade, selecteert u een rol op de geselecteerde gebruikers of groepen wilt toepassen en selecteer vervolgens de **OK** knop aan de onderkant van de blade.
11. Op de **toevoegen toewijzing** blade, selecteer de **toewijzen** knop aan de onderkant van de blade. De toegewezen gebruikers of groepen heeft de machtigingen die zijn gedefinieerd door de geselecteerde rol voor deze app voor de onderneming.

## <a name="next-steps"></a>Volgende stappen
* [Zie al mijn groepen](active-directory-groups-view-azure-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Uitschakelen van de gebruikersaanmeldingen voor een enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
* [Wijzig de naam of het logo van een enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
