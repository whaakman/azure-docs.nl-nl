---
title: Een toepassing gebruikerservaring in Azure Active Directory verbergen | Microsoft Docs
description: Hoe u een toepassing gebruikerservaring in de toegangsdeelvensters van de Azure Active Directory of Office 365 voor ruimtevaartuigen verbergen.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: barbkess
ms.reviewer: kasimpso
ms.openlocfilehash: f1203b4381055deca79a70e8d295e072feaf492a
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613565"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>Toepassingen van eindgebruikers in Azure Active Directory verbergen

Instructies voor het verbergen van eindgebruikers MyApps deelvenster of Office 365-startprogramma voor toepassingen. Wanneer een toepassing verborgen is, hebben gebruikers nog steeds de machtigingen voor de toepassing. 

## <a name="prerequisites"></a>Vereisten

Toepassing administrator-bevoegdheden zijn vereist voor het verbergen van een toepassing van de MyApps-Configuratiescherm en startprogramma voor Office 365.

Globale beheerder-bevoegdheden zijn vereist om te verbergen alle Office 365-toepassingen.


## <a name="hide-an-application-from-the-end-user"></a>Een toepassing de eindgebruiker verbergen
Gebruik de volgende stappen uit om te een toepassing van MyApps Configuratiescherm en startprogramma voor toepassingen van Office 365 verbergen.

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder voor uw directory.
2.  Selecteer **Azure Active Directory**.
3.  Selecteer **bedrijfstoepassingen**. De **bedrijfstoepassingen - alle toepassingen** blade wordt geopend.
4.  Onder **toepassingstype**, selecteer **bedrijfstoepassingen**, als deze nog niet is geselecteerd.
5.  Zoeken naar de toepassing die u wilt verbergen, en klik op de toepassing.  Overzicht van de toepassing wordt geopend.
6.  Klik op **Eigenschappen**. 
7.  Voor de **zichtbaar voor gebruikers?** vraag, klikt u op **Nee**.
8.  Klik op **Opslaan**.


## <a name="hide-office-365-applications-from-the-myapps-panel"></a>Office 365-toepassingen in het deelvenster MyApps verbergen

Gebruik de volgende stappen uit om te verbergen alle Office 365-toepassingen in het deelvenster MyApps. De toepassingen zijn nog steeds wordt weergegeven in de Office 365-beheerportal.

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder voor uw directory.
2.  Selecteer **Azure Active Directory**.
3.  Selecteer **gebruikersinstellingen**.
4.  Onder **bedrijfstoepassingen**, klikt u op **beheren hoe eindgebruikers starten en weergeven van hun toepassingen.**
5.  Voor **gebruikers zien alleen Office 365-apps in de Office 365-beheerportal**, klikt u op **Ja**.
6.  Klik op **Opslaan**.


## <a name="next-steps"></a>Volgende stappen
* [Mijn groepen bekijken](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](assign-user-or-group-access-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](remove-user-or-group-access-portal.md)
* [De naam of het logo van een zakelijke app wijzigen](change-name-or-logo-portal.md)

