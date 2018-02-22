---
title: Wijzig de naam of het logo van een enterprise-app in Azure Active Directory | Microsoft Docs
description: Het wijzigen van de naam of het logo voor een aangepaste enterprise-app in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: fe03d2a8dcb39cb06679386959ac17354a543089
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>De naam of het logo van een enterprise-app in Azure Active Directory wijzigen
Het is gemakkelijk om te wijzigen van de naam of het logo voor een aangepaste toepassing in Azure Active Directory (Azure AD). U moet de juiste machtigingen voor u deze wijzigingen hebben en u moet de maker van de aangepaste app.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Hoe wijzig ik de naam of het logo een enterprise-app?
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **alle services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3. Op de **Azure Active Directory - *directoryname***  deelvenster (dat wil zeggen, de Azure AD voor de map die u beheert), selecteer **bedrijfstoepassingen**.

    ![Openen zakelijke apps](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. Op de **bedrijfstoepassingen** deelvenster **alle toepassingen**. U ziet een lijst met de apps die u kunt beheren.
5. Op de **bedrijfstoepassingen - alle toepassingen** deelvenster, selecteert u een app.
6. Op de ***appname*** deelvenster (dat wil zeggen, het deelvenster met de naam van de geselecteerde app in de titel), selecteer **eigenschappen**.

    ![De opdracht Eigenschappen selecteren](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. Op de ***appname*** **-eigenschappen** deelvenster, blader naar een bestand om te gebruiken als een nieuwe logo of bewerken van de app-naam of beide.

    ![Het wijzigen van de app-logo of nameproperties-opdracht](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Selecteer de **opslaan** opdracht.

## <a name="next-steps"></a>Volgende stappen
* [Zie al mijn groepen](active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Uitschakelen van de gebruikersaanmeldingen voor een enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
