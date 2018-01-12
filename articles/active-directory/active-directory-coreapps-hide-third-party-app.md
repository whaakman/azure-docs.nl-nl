---
title: Een toepassing van de gebruikerservaring in Azure Active Directory verbergen | Microsoft Docs
description: Hoe een toepassing van de gebruikerservaring in Azure Active Directory toegang panelen of Office 365 voor ruimtevaartuigen verbergen.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 00307311ca2bc1432f2de3755575adf1804f9c77
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Een toepassing van de gebruikerservaring in Azure Active Directory verbergen

Als u een toepassing die u niet wilt weergeven op de toegang panelen of Office 365 voor ruimtevaartuigen gebruikers hebt, zijn er opties voor het verbergen van deze tegel app.  De volgende twee opties zijn beschikbaar voor toepassingen van de gebruiker de app voor ruimtevaartuigen verbergen.

- Een toepassing van derden van gebruikers toegang panelen en Office 365-app voor ruimtevaartuigen verbergen
- Alle Office 365-toepassingen van gebruikers toegang panelen verbergen

Door te verbergen de app gebruikers nog steeds machtigingen voor de app, maar ze worden weergegeven op de app voor ruimtevaartuigen niet zien. U moet de juiste machtigingen voor het beheren van de app voor de onderneming hebben en u moet een globale beheerder voor de map.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Een toepassing van de eindgebruiker van gebruikerservaringen verbergen
U kunt de stappen hieronder en afhankelijk van uw situatie gebruiken voor het verbergen van toepassingen van het toegangsvenster.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Hoe kan ik een app van derden uit van de gebruiker toegangspaneel en O365-app voor ruimtevaartuigen verbergen?
Gebruik de volgende stappen voor het verbergen van de toepassing in het deelvenster toegang en de Office 365-app voor ruimtevaartuigen van een gebruiker.

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

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Hoe kan ik Office 365-toepassingen uit van de gebruiker toegang deelvenster verbergen?

Gebruik de volgende stappen voor het verbergen van alle Office 365-toepassingen in het deelvenster toegang. Deze apps nog steeds zichtbaar in de Office 365-portal.

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2.  Selecteer **meer services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3.  Op de **Azure Active Directory - *directoryname***  scherm (dat wil zeggen, de Azure AD scherm voor de map die u beheert), selecteer **gebruikersinstellingen**.
4.  Op de **gebruikersinstellingen** scherm onder **bedrijfstoepassingen** Selecteer **Ja** voor **kunnen gebruikers alleen Office 365-apps in de Office 365-beheerportalzien**.

![Enterprise-apps](media/active-directory-coreapps-hide-third-party-app/apps4.png)

## <a name="next-steps"></a>Volgende stappen
* [Mijn groepen bekijken](active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Wijzig de naam of het logo van een enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)

