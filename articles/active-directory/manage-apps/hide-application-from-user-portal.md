---
title: Een toepassing gebruikerservaring in Azure Active Directory verbergen | Microsoft Docs
description: Hoe u een toepassing gebruikerservaring in de toegangsdeelvensters van de Azure Active Directory of Office 365 voor ruimtevaartuigen verbergen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 55f80396df4cbfe7d0a16a6a5066b68aadc0bdd3
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369343"
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Een toepassing gebruikerservaring in Azure Active Directory verbergen

Als u een toepassing die u niet wilt weergeven op de toegangsdeelvensters of Office 365 voor ruimtevaartuigen gebruikers hebt, zijn er opties voor het verbergen van deze app-tegel.  De volgende twee opties zijn beschikbaar voor toepassingen van de gebruiker de app voor ruimtevaartuigen verbergen.

- Een toepassing van derden van toegangsdeelvensters van de gebruikers- en Office 365-app voor ruimtevaartuigen verbergen
- Alle Office 365-toepassingen uit de toegangsdeelvensters van gebruikers te verbergen

Door te verbergen gebruikers van de app nog steeds machtigingen hebben voor de app, maar ze worden weergegeven op de app voor ruimtevaartuigen niet zien. U moet de juiste machtigingen voor het beheren van de enterprise-app hebt en u moet een globale beheerder voor de map.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Een toepassing van de eindgebruiker van gebruikerservaringen verbergen
U kunt de stappen hieronder, afhankelijk van uw situatie toepassingen vanuit het toegangsvenster verbergen.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Hoe kan ik een app van derden vanuit het toegangsvenster en O365-app voor ruimtevaartuigen van gebruiker verbergen?
Gebruik de volgende stappen uit om te een toepassing van het toegangsvenster en de Office 365-app voor ruimtevaartuigen van een gebruiker verbergen.

1.  Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
2.  Selecteer **alle services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3.  Op de **Azure Active Directory - *directoryname***  scherm (dat wil zeggen, de Azure AD het scherm voor de map die u beheert), selecteer **bedrijfstoepassingen**.
![Enterprise-apps](./media/hide-application-from-user-portal/app1.png)
4.  Op de **bedrijfstoepassingen** scherm, selecteer **alle toepassingen**. U ziet een lijst van de apps die u kunt beheren.
5.  Op de **bedrijfstoepassingen - alle toepassingen** scherm, selecteert u een app.</br>
![Enterprise-apps](./media/hide-application-from-user-portal/app2.png)
6.  Op de ***appname*** scherm (dat wil zeggen, het scherm met de naam van de geselecteerde app in de titel), selecteer Eigenschappen.
7.  Op de  ***appname* -eigenschappen** scherm, selecteer **Ja** voor **zichtbaar voor gebruikers?**.
![Enterprise-apps](./media/hide-application-from-user-portal/app3.png)
8.  Selecteer de **opslaan** opdracht.

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Hoe kan ik Office 365-toepassingen vanuit het toegangsvenster van de gebruiker verbergen?

Gebruik de volgende stappen uit om te verbergen alle Office 365-toepassingen vanuit het toegangsvenster. Deze apps nog steeds worden weergegeven in de Office 365-portal.

1.  Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
2.  Selecteer **alle services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
3.  Op de **Azure Active Directory - *directoryname***  scherm (dat wil zeggen, de Azure AD het scherm voor de map die u beheert), selecteer **gebruikersinstellingen**.
4.  Op de **gebruikersinstellingen** scherm onder **bedrijfstoepassingen** Selecteer **Ja** voor **kunnen gebruikers alleen Office 365-apps in de Office 365-beheerportalzien**.

![Enterprise-apps](./media/hide-application-from-user-portal/apps4.png)

## <a name="next-steps"></a>Volgende stappen
* [Mijn groepen bekijken](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](assign-user-or-group-access-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](remove-user-or-group-access-portal.md)
* [De naam of het logo van een zakelijke app wijzigen](change-name-or-logo-portal.md)

