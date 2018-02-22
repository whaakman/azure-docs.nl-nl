---
title: Gebruikersaanmeldingen een enterprise-App in Azure Active Directory uitschakelen | Microsoft Docs
description: Het uitschakelen van een zakelijke toepassing zodat er geen gebruikers bij deze in Azure Active Directory aanmelden kunnen
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: e044d32406236aacaf7fffa2b4b19dadd96a7d5d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Gebruikersaanmeldingen een enterprise-App in Azure Active Directory uitschakelen
Het is gemakkelijk een zakelijke toepassing uitschakelen zodat er geen gebruikers bij deze in Azure Active Directory (Azure AD aanmelden kunnen). U moet de juiste machtigingen voor het beheren van de app voor de onderneming hebben en u moet een globale beheerder voor de map.

## <a name="how-do-i-disable-user-sign-ins"></a>Hoe schakel ik gebruikersaanmelding
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **alle services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3. Op de **Azure Active Directory** -  ***directoryname*** deelvenster (dat wil zeggen, de Azure AD voor de map die u beheert), selecteer **bedrijfstoepassingen**.

    ![Openen zakelijke apps](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. Op de **bedrijfstoepassingen** deelvenster **alle toepassingen**. U ziet een lijst met de apps die u kunt beheren.
5. Op de **bedrijfstoepassingen - alle toepassingen** deelvenster, selecteert u een app.
6. Op de ***appname*** deelvenster (dat wil zeggen, het deelvenster met de naam van de geselecteerde app in de titel), selecteer **eigenschappen**.

    ![De opdracht Alles toepassingen selecteren](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. Op de ***appname*** - **eigenschappen** deelvenster **Nee** voor **ingeschakeld voor gebruikers om aan te melden?**.
8. Selecteer de **opslaan** opdracht.

## <a name="next-steps"></a>Volgende stappen
* [Mijn groepen bekijken](active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Wijzig de naam of het logo van een enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
