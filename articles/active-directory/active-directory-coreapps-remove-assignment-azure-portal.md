---
title: De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory | Microsoft Docs
description: De toewijzing van de toegang van een gebruiker of groep van een enterprise-app in Azure Active Directory verwijderen
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 8459f9a890f6f57e8c93da9b1d703449b09ba666
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory
Het is gemakkelijk om te verwijderen van een gebruiker of groep toegang wordt toegewezen aan een van uw zakelijke toepassingen in Azure Active Directory (Azure AD). U moet de juiste machtigingen voor het beheren van de app voor de onderneming hebben en u moet een globale beheerder voor de map.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Hoe kan ik een gebruiker of groepstoewijzing verwijderen?
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **meer services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3. Op de **Azure Active Directory - *directoryname***  blade (dat wil zeggen, de Azure AD blade voor de map die u beheert), selecteer **bedrijfstoepassingen**.

    ![Openen zakelijke apps](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. Op de **bedrijfstoepassingen** blade Selecteer **alle toepassingen**. Hier ziet u een lijst met de apps die u kunt beheren.
5. Op de **bedrijfstoepassingen - alle toepassingen** blade, selecteert u een app.
6. Op de ***appname*** blade (dat wil zeggen, de blade met de naam van de geselecteerde app in de titel), selecteer **gebruikers en groepen**.

    ![Gebruikers of groepen selecteren](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. Op de ***appname*** **-gebruiker & groepstoewijzing** blade een van meer gebruikers of groepen selecteren en selecteer vervolgens de **verwijderen** opdracht. Bevestig uw beslissing bij de opdrachtprompt.

    ![De opdracht Remove selecteren](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Volgende stappen
* [Zie al mijn groepen](active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
* [Uitschakelen van de gebruikersaanmeldingen voor een enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
* [Wijzig de naam of het logo van een enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
