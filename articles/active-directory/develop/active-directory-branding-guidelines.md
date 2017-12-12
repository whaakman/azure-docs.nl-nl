---
title: Huisstijl richtlijnen voor toepassingen | Microsoft Docs
description: Een uitgebreide handleiding voor ontwikkelaars resources voor Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mtillman
editor: 
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: 51adb13e15312130841c065f5678209508457559
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="branding-guidelines-for-applications"></a>Huisstijlrichtlijnen voor toepassingen
In dit onderwerp wordt de huisstijlrichtlijnen die moet u bij het ontwikkelen van toepassingen met Azure Active Directory (Azure AD) beschreven. Deze richtlijnen helpt uw klanten directe wanneer ze hun werk- of schoolaccount gebruikt willen, worden beheerd in Azure AD of persoonlijk account voor aanmelden en aanmelden bij uw toepassing.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Persoonlijke accounts versus werk- of schoolaccounts van Microsoft
Microsoft beheert de twee typen gebruikersaccounts:

* **Persoonlijke accounts** (voorheen Windows Live ID). Deze accounts vertegenwoordigen de relatie tussen *afzonderlijke* gebruikers- en Microsoft- en worden gebruikt voor toegang tot apparaten en services van Microsoft. Deze accounts zijn bedoeld voor persoonlijk gebruik.
* **Werk- of schoolaccount accounts.** Deze accounts worden beheerd door Microsoft namens organisaties die gebruikmaken van Azure Active Directory. Deze accounts worden gebruikt voor het aanmelden bij Office 365 en andere zakelijke services van Microsoft.

Microsoft werk-of schoolaccounts doorgaans voor eindgebruikers (werknemers, studenten, federal werknemers) worden toegewezen door hun organisatie (bedrijf, school, overheidsinstelling). Deze accounts zijn onder de knie rechtstreeks in de cloud, in de Azure AD-platform of vanuit een on-premises adreslijst, zoals Windows Server Active Directory naar Azure AD gesynchroniseerd. Microsoft is de *vallen* van werk of school-accounts, maar de accounts zijn eigendom en wordt beheerd door de organisatie.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Verwijst naar Azure AD-accounts in uw toepassing
Microsoft tonen niet eindgebruikers tot de Azure of de namen van Active Directory merk en geen van beide, moet u.

* Nadat gebruikers zijn aangemeld, moet u de naam en het logo zoveel mogelijk van de organisatie. Dit is beter dan het gebruik van algemene voorwaarden zoals 'uw organisatie'.
* Wanneer gebruikers niet bent aangemeld, moet u verwijzen naar hun accounts als ' werk- of schoolaccounts ' en het gebruik van het Microsoft-logo om weer te geven dat deze accounts worden beheerd door Microsoft. Gebruik geen voorwaarden zoals 'enterprise-account', 'business-account' of 'zakelijke account' die gebruiker verwarring.

## <a name="user-account-pictogram"></a>Pictogram voor gebruiker-account
In een eerdere versie van deze richtlijnen raden wij met behulp van een pictogram 'blue badge'. Op basis van feedback van gebruikers- en developer, nu het beste het gebruik van het Microsoft-logo in plaats daarvan. Dit helpt gebruikers begrijpen die ze opnieuw het account dat ze gebruiken met Office 365 of andere Microsoft business services aan te melden bij uw app kunnen gebruiken.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Aanmelden en aanmelden met Azure AD
Uw app kan afzonderlijke paden voor aanmelden en aanmelden aanwezig is en de volgende secties bevatten visuele richtlijnen voor beide scenario's.

**Als uw app eindgebruiker sign ondersteunt up (bijvoorbeeld gratis proefversie of freemium model)**: U kunt weergeven een **aanmelden** knop waarmee gebruikers toegang krijgen tot uw app voor hun werk- of een persoonlijk account. Azure AD wordt een prompt toestemming weer de eerste keer dat ze toegang krijgen uw app tot.

**Als uw app vereist machtigingen die alleen Administrators met instemmen kunnen of dat uw app vereist organisatie licentieverlening**: scheidt u overname van de beheerder van de gebruiker zich aanmelden. De **knop 'ophalen deze app'** omleidt beheerders om te melden en vervolgens vraagt u ze toestemming namens gebruikers in hun organisatie. Dit heeft het voordeel van het onderdrukken van eindgebruikers toestemming wordt gevraagd naar uw app.

## <a name="visual-guidance-for-app-acquisition"></a>Visual richtlijnen voor de aanschaf van app
De koppeling 'ophalen van de app' moet de gebruiker wordt omgeleid naar de Azure AD-toegang verlenen (autoriseren) pagina, zodat de beheerder van een organisatie te autoriseren van uw app toegang hebben tot gegevens van hun organisatie die wordt gehost door Microsoft. Meer informatie over het om toegang te vragen worden beschreven de [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md) artikel.

Nadat beheerders toestemming aan uw app geven, ze kunnen kiezen toe te voegen aan hun Office 365 app linksboven gebruikerservaring (toegankelijk vanaf de waffle en van [https://portal.office.com/myapps](https://portal.office.com/myapps)). Als u adverteren van deze mogelijkheid wilt, kunt u voorwaarden zoals 'Deze app aan uw organisatie toevoegen' en een knop als volgt weergegeven:

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/add-to-my-org.png)

We raden echter aan dat u schrijft verklarende tekst in plaats van knoppen. Bijvoorbeeld:

> *Als u al gebruikt voor Office 365 of andere zakelijke services van Microsoft, u kunt gewoon < your_app_name > toegang verlenen tot gegevens van uw organisatie. Zo kunt uw gebruikers toegang krijgen tot < your_app_name > met hun bestaande werkaccounts.*
> 
> 

## <a name="visual-guidance-for-sign-in"></a>Visuele aanwijzingen voor aanmelden
Uw app een aanmelding moet worden weergegeven op de knop waarmee gebruikers worden omgeleid naar het eindpunt aanmelden die overeenkomt met het protocol dat u gebruiken om te integreren met Azure AD. De volgende sectie bevat informatie over wat die knop als eruitzien moet.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictogram en 'Aanmelden met Microsoft'
Dit is de koppeling van het Microsoft-logo en de voorwaarden 'Aanmelding in met Microsoft' met een unieke Azure AD onder andere id-providers die uw app kan ondersteunen. Als u geen voldoende ruimte voor 'Aanmelding in met Microsoft', is het ok om door te moet u deze 'Aanmelden'.

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-light.png)

U kunt ook een donker kleurenschema voor de knoppen.

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>De huisstijl van taken en niet moet doen
**Voer** 'werk of school-account' gebruiken in combinatie met de knop 'Aanmelding in met Microsoft' om te bieden aanvullende uitleg zodat eindgebruikers herkennen of software kan worden gebruikt. **Geen** gebruiken van andere voorwaarden zoals 'enterprise-account', 'business-account' of 'zakelijke account'.

**Geen** 'Office 365-ID' of 'Azure-ID' gebruiken. Office 365 is ook de naam van een consumer aanbieding van Microsoft Azure AD niet voor verificatie gebruiken.

**Geen** alter van het Microsoft-logo.

**Geen** blootstellen eindgebruikers de merken Azure of Active Directory. Het is echter wel ok gebruik van deze voorwaarden met ontwikkelaars, IT-professionals en beheerders.

## <a name="navigation-dos-and-donts"></a>Tips voor navigatie
**Voer** bieden een manier voor gebruikers zich afmelden en overschakelen naar een ander gebruikersaccount. Hoewel de meeste mensen één persoonlijke account van Microsoft/Facebook/Google/Twitter, zijn vaak mensen gekoppeld aan meer dan één organisatie. Ondersteuning voor meerdere gebruikers aangemeld is binnenkort beschikbaar.

