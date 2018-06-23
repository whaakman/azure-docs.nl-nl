---
title: Richtlijnen voor toepassingen huisstijl | Microsoft Docs
description: Een uitgebreide handleiding voor ontwikkelaars resources voor Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: celested
ms.reviewer: arielgo, skwan
ms.custom: aaddev
ms.openlocfilehash: 97ae16e21e0771c89b1c878c3cabaeab25bcee4b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318599"
---
# <a name="branding-guidelines-for-applications"></a>Huisstijlrichtlijnen voor toepassingen

Dit artikel worden de huisstijlrichtlijnen die moet u bij het ontwikkelen van toepassingen met Azure Active Directory (Azure AD). Deze richtlijnen helpt uw klanten directe wanneer ze hun werk- of schoolaccount gebruikt willen, worden beheerd in Azure AD of persoonlijk account voor aanmelden en aanmelden bij uw toepassing.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Persoonlijke accounts versus werk- of schoolaccounts van Microsoft

Microsoft beheert de twee typen gebruikersaccounts:

* **Persoonlijke accounts** (voorheen Windows Live ID). Deze accounts vertegenwoordigen de relatie tussen *afzonderlijke* gebruikers- en Microsoft- en worden gebruikt voor toegang tot apparaten en services van Microsoft. Deze accounts zijn bedoeld voor persoonlijk gebruik.
* **Werk- of schoolaccount accounts.** Deze accounts worden beheerd door Microsoft namens organisaties die gebruikmaken van Azure Active Directory. Deze accounts worden gebruikt voor het aanmelden bij Office 365 en andere zakelijke services van Microsoft.

Microsoft werk-of schoolaccounts doorgaans voor eindgebruikers (werknemers, studenten, federal werknemers) worden toegewezen door hun organisatie (bedrijf, school, overheidsinstelling). Deze accounts master rechtstreeks in de cloud (in de Azure AD-platform) of vanuit een on-premises adreslijst, zoals Windows Server Active Directory naar Azure AD gesynchroniseerd. Microsoft is de *vallen* van werk of school-accounts, maar de accounts zijn eigendom en wordt beheerd door de organisatie.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Verwijst naar Azure AD-accounts in uw toepassing

Microsoft biedt geen eindgebruikers kunnen de Azure of de namen van Active Directory merk en geen van beide tonen, moet u.

* Zodra gebruikers zijn aangemeld, gebruikt u de naam en het logo zoveel mogelijk van de organisatie. Dit is beter dan het gebruik van algemene voorwaarden zoals "uw organisatie."
* Wanneer gebruikers niet bent aangemeld, verwijst naar hun accounts als ' werk- of schoolaccounts ' en het gebruik van het Microsoft-logo om weer te geven waarop deze accounts wordt beheerd door Microsoft. Gebruik geen voorwaarden zoals 'enterprise-account', 'business-account' of 'zakelijke account', die gebruiker verwarring te maken.

## <a name="user-account-pictogram"></a>Pictogram voor gebruiker-account

In een eerdere versie van deze richtlijnen raden wij met behulp van een pictogram 'blue badge'. Op basis van feedback van gebruikers- en developer, nu het beste het gebruik van het Microsoft-logo in plaats daarvan. Het Microsoft-logo helpt gebruikers begrijpen die ze opnieuw het account dat ze gebruiken met Office 365 of andere Microsoft business services aan te melden bij uw app kunnen gebruiken.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Aanmelden en aanmelden met Azure AD

Uw app kan afzonderlijke paden voor aanmelden en aanmelden aanwezig is en de volgende secties bevatten visuele richtlijnen voor beide scenario's.

**Als uw app (bijvoorbeeld gratis proefversie of freemium model) registratie door eindgebruikers ondersteunt**: U ziet een **aanmelden** knop waarmee gebruikers toegang krijgen tot uw app voor hun werk- of een persoonlijk account. Azure AD wordt een prompt toestemming weer de eerste keer dat ze toegang krijgen uw app tot.

**Als uw app vereist machtigingen die alleen Administrators met instemmen kunnen of dat uw app vereist organisatie licentieverlening**: scheiden van aanschaf van de beheerder van de gebruiker zich aanmelden. De **knop 'ophalen deze app'** omleidt beheerders om te melden en vervolgens vraagt u ze toestemming namens gebruikers in hun organisatie, het voordeel heeft van het onderdrukken van eindgebruikers toestemming wordt gevraagd naar uw app.

## <a name="visual-guidance-for-app-acquisition"></a>Visual richtlijnen voor de aanschaf van app

De koppeling 'ophalen van de app' moet de gebruiker wordt omgeleid naar de Azure AD-toegang verlenen (autoriseren) pagina, zodat de beheerder van een organisatie te autoriseren van uw app toegang hebben tot de gegevens van hun organisatie, die wordt gehost door Microsoft. Meer informatie over het om toegang te vragen worden beschreven de [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md) artikel.

Nadat beheerders toestemming aan uw app geven, ze kunnen kiezen toe te voegen aan hun Office 365 app linksboven gebruikerservaring (toegankelijk vanaf de waffle en van [ https://portal.office.com/myapps ](https://portal.office.com/myapps)). Als u adverteren van deze mogelijkheid wilt, kunt u voorwaarden zoals 'Deze app aan uw organisatie toevoegen' en een knop weergeven zoals in het volgende voorbeeld:

![Toepassingstypen en scenario 's](./media/active-directory-branding-guidelines/add-to-my-org.png)

We raden echter aan dat u schrijft verklarende tekst in plaats van knoppen. Bijvoorbeeld:

> *Als u al gebruikt voor Office 365 of andere zakelijke services van Microsoft, u kunt < your_app_name > toegang verlenen tot gegevens van uw organisatie. Zo kunt uw gebruikers toegang krijgen tot < your_app_name > met hun bestaande werkaccounts.*

Voor het downloaden van het officiële Microsoft-logo voor gebruik in uw app met de rechtermuisknop op die u wilt gebruiken en sla deze op uw computer.

| Asset                                | PNG-indeling | SVG-indeling |
| ------------------------------------ | ---------- | ---------- |
| Microsoft-logo  | ![Microsoft-logo PNG](./media/active-directory-branding-guidelines/MS-SymbolLockup_MSSymbol_19.png) | ![SVG-Microsoft-logo](./media/active-directory-branding-guidelines/MS-SymbolLockup_MSSymbol_19.svg) |

## <a name="visual-guidance-for-sign-in"></a>Visuele aanwijzingen voor aanmelden

Uw app moet een knop aanmelden waarmee gebruikers worden omgeleid naar het eindpunt aanmelden die overeenkomt met het protocol dat u gebruiken om te integreren met Azure AD worden weergegeven. De volgende sectie bevat informatie over wat die knop als eruitzien moet.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictogram en 'Aanmelden met Microsoft'

Dit is de koppeling van het Microsoft-logo en de voorwaarden 'Aanmelding in met Microsoft' die een unieke Azure AD onder andere id-providers vertegenwoordigen die uw app kan ondersteunen. Als u geen voldoende ruimte voor 'Aanmelding in met Microsoft', is het ok om door te moet u deze "Aanmelden." U kunt een licht of donker kleurenschema voor de knoppen.

Het volgende diagram toont dat de Microsoft aanbevolen redlines bij het gebruik van de activa met uw app. De redlines van toepassing op 'Aanmelding in met Microsoft' of de kortere versie van 'Aanmelden'.

![Meld u aan met Microsoft redlines](./media/active-directory-branding-guidelines/Sign-in-with-Microsoft-redlines.png)

Voor het downloaden van de officiële afbeeldingen voor gebruik in uw app met de rechtermuisknop op die u wilt gebruiken en sla deze op uw computer.

| Asset                                | PNG-indeling | SVG-indeling |
| ------------------------------------ | ---------- | ---------- |
| Meld u aan met Microsoft (donker thema)  | ![Meld u aan de knop donkere thema PNG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark.png) | ![Meld u aan met Microsoft knop donkere thema SVG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark.svg) |
| Meld u aan met Microsoft (lichte thema) | ![Meld u aan de knop lichte themakleur PNG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light.png) | ![Meld u aan met Microsoft knop lichte themakleur SVG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light.svg) |
| Meld u aan (donker thema)                 | ![Meld u aan in een korte knop donkere thema PNG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark_short.png) | ![Meld u aan in een korte knop donkere thema SVG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_dark_short.svg) |
| Meld u aan (lichte thema)                | ![Meld u aan in een korte knop lichte themakleur PNG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light_short.png) | ![Meld u aan in een korte knop lichte themakleur SVG](./media/active-directory-branding-guidelines/MS-SymbolLockup_SignIn_light_short.svg) |


## <a name="branding-dos-and-donts"></a>De huisstijl van taken en niet moet doen

**Voer** 'werk of school-account' gebruiken in combinatie met de knop 'Aanmelding in met Microsoft' om te bieden aanvullende uitleg zodat eindgebruikers herkennen of software kan worden gebruikt. **Geen** gebruiken van andere voorwaarden zoals 'enterprise-account', 'business-account' of 'zakelijke account'.

**Geen** gebruiken 'Office 365-ID' of 'Azure-id '. Office 365 is ook de naam van een consumer aanbieding van Microsoft Azure AD niet voor verificatie gebruiken.

**Geen** alter van het Microsoft-logo.

**Geen** blootstellen eindgebruikers kunnen de merken Azure of Active Directory. Het is echter wel ok gebruik van deze voorwaarden met ontwikkelaars, IT-professionals en beheerders.

## <a name="navigation-dos-and-donts"></a>Tips voor navigatie

**Voer** bieden een manier voor gebruikers zich afmelden en overschakelen naar een ander gebruikersaccount. Hoewel de meeste mensen één persoonlijke account van Microsoft/Facebook/Google/Twitter, zijn vaak mensen gekoppeld aan meer dan één organisatie. Ondersteuning voor meerdere gebruikers aangemeld is binnenkort beschikbaar.
