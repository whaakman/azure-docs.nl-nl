---
title: Een toepassing van derden van de gebruikerservaring in Azure Active Directory verbergen | Microsoft Docs
description: Hoe kan ik een toepassing van derden van de gebruikerservaring in Azure Active Directory verbergen
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 976cbb1341493186b9996d250ebca8f2f3688fdf
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="hide-a-third-party-application-from-users-experience-in-azure-active-directory"></a>Een toepassing van derden van de gebruikerservaring in Azure Active Directory verbergen

Als u een toepassing van derden (een app gepubliceerd door anderen dan Microsoft) hebt dat u niet weergeven op de gebruikers toegang deelvensters of Office 365 voor ruimtevaartuigen wilt, er is een optie voor het verbergen van deze tegel app. Door te verbergen de app gebruikers nog steeds machtigingen voor de app, maar ze worden weergegeven op de app voor ruimtevaartuigen niet zien. U moet de juiste machtigingen voor het beheren van de app voor de onderneming hebben en u moet een globale beheerder voor de map.

## <a name="hiding-a-third-party-app-from-a-users-experience"></a>Een app van derden van een eindgebruiker verbergen
Gebruik de volgende stappen voor het verbergen van een derde partij app van een gebruiker toegangspaneel en Office 365-app voor ruimtevaartuigen

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Hoe kan ik een app van derden uit van de gebruiker toegangspaneel en O365-app voor ruimtevaartuigen verbergen?

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2.  Selecteer **meer services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3.  Op de **Azure Active Directory - *directoryname***  scherm (dat wil zeggen, de Azure AD scherm voor de map die u beheert), selecteer **bedrijfstoepassingen**.
![Zakelijke apps](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  Op de **bedrijfstoepassingen** Schakel in het scherm **alle toepassingen**. U ziet een lijst met de apps die u kunt beheren.
5.  Op de **bedrijfstoepassingen - alle toepassingen** scherm, selecteert u een app.</br>
![Zakelijke apps](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  Op de ***appname*** scherm (dat wil zeggen, het scherm met de naam van de geselecteerde app in de titel), selecteert u eigenschappen.
7.  Op de  ***appname* -eigenschappen** Schakel in het scherm **Ja** voor **zichtbaar voor gebruikers?**.
![Zakelijke apps](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Selecteer de **opslaan** opdracht.

## <a name="next-steps"></a>Volgende stappen
* [Mijn groepen bekijken](active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Wijzig de naam of het logo van een enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
