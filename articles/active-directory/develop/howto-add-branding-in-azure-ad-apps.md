---
title: Huisstijlrichtlijnen voor toepassingen | Microsoft Docs
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
ms.reviewer: arielgo
ms.custom: aaddev, signin_art
ms.openlocfilehash: 78a0b9bf7f49cc41c8d78287bcbe54c5c88f0809
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597902"
---
# <a name="branding-guidelines-for-applications"></a>Huisstijlrichtlijnen voor toepassingen

Dit artikel worden de huisstijlrichtlijnen die moet u bij het ontwikkelen van toepassingen met Azure Active Directory (Azure AD). Deze richtlijnen kunnen uw klanten direct wanneer ze hun werk- of schoolaccount gebruikt willen, worden beheerd in Azure AD, of hun eigen account voor registratie en aanmelden bij uw toepassing.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Persoonlijke accounts en werk- of schoolaccounts van Microsoft

Microsoft beheert twee soorten gebruikersaccounts:

* **Persoonlijke accounts** (voorheen bekend als Windows Live ID). Deze accounts geven het verband tussen *afzonderlijke* gebruikers en Microsoft en worden gebruikt voor toegang tot apparaten en services van Microsoft. Deze accounts zijn bedoeld voor persoonlijk gebruik.
* **Accounts voor werk- of schoolaccount.** Deze accounts worden beheerd door Microsoft namens de organisaties die gebruikmaken van Azure Active Directory. Deze accounts worden gebruikt voor het aanmelden bij Office 365 en andere zakelijke services van Microsoft.

Microsoft werk-of schoolaccounts doorgaans aan eindgebruikers (werknemers, studenten, federale werknemers) door hun organisatie (bedrijf, school, overheidsinstelling heeft toegewezen zijn). Deze accounts onder de knie rechtstreeks in de cloud (in de Azure AD-platform) of die uit een on-premises adreslijst, zoals Windows Server Active Directory zijn gesynchroniseerd met Azure AD. Microsoft is de *beheerder* van werk of school-accounts, maar de accounts zijn eigendom van en beheerd door de organisatie.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Verwijzen naar Azure AD-accounts in uw toepassing

Microsoft biedt geen eindgebruikers kunnen de Azure of de Active Directory-merknamen en geen van beide blootstellen, moet u.

* Nadat gebruikers zijn aangemeld, gebruikt u de naam en het logo zo veel mogelijk van de organisatie. Dit is beter dan het gebruik van algemene termen, zoals "uw organisatie."
* Wanneer gebruikers niet bent aangemeld, verwijzen naar hun accounts als ' werk- of schoolaccounts "en het gebruik van het Microsoft-logo om weer te geven dat deze accounts worden beheerd door Microsoft. Gebruik geen termen, zoals 'enterprise-account', 'zakelijke account' of 'zakelijk account', die gebruiker verwarring maken.

## <a name="user-account-pictogram"></a>Pictogram van gebruiker-account

In een eerdere versie van deze richtlijnen raden wij met behulp van een pictogram 'blue logo'. Op basis van feedback van gebruikers- en developer, nu wordt aangeraden het gebruik van het Microsoft-logo in plaats daarvan. Het Microsoft-logo helpt gebruikers begrijpen dat opnieuw kan worden gebruikt de account die ze gebruiken met Office 365 of andere Microsoft business services aan te melden bij uw app.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Zich registreren en aanmelden met Azure AD

Uw app moet u wellicht afzonderlijke paden voor registratie en aanmelding en de volgende secties bevatten visuele richtlijnen voor beide scenario's.

**Als uw app door eindgebruikers zich aanmelden (voor een voorbeeld, gratis proefversie of freemium-model) ondersteunt**: U ziet een **aanmelden** knop waarmee gebruikers toegang krijgen tot uw app met hun werkaccount of persoonlijk account. Azure AD wordt een toestemmingsprompt de eerste keer dat ze toegang krijgen uw app tot weergegeven.

**Als uw app nodig heeft machtigingen die alleen beheerders met instemmen kunnen, of als uw app is een organisatie-licentie vereist**: beheerder aanschaf van aanmelden van gebruikers te scheiden. De **knop 'deze app ophalen'** beheerders om te melden bij en vervolgens vragen om toestemming namens gebruikers in hun organisatie, die het voordeel heeft van het onderdrukken van eindgebruikers toestemming wordt gevraagd naar uw app wordt omgeleid.

## <a name="visual-guidance-for-app-acquisition"></a>Visual richtlijnen voor de aanschaf van app

De koppeling 'ophalen van de app' omleiden van de gebruiker moet de Azure AD om toegang te verlenen (autorisatie) pagina, waarmee een beheerder om uw app toegang hebben tot gegevens van hun organisatie, die wordt gehost door Microsoft te autoriseren. Meer informatie over het aanvragen van toegang worden beschreven de [toepassingen integreren met Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md) artikel.

Nadat beheerders toestemming aan uw app geven, ze kunnen kiezen toe te voegen aan hun Office 365-app startprogramma voor de gebruikerservaring (toegankelijk is vanaf de wafel en vanaf [ https://portal.office.com/myapps ](https://portal.office.com/myapps)). Als u adverteren van deze mogelijkheid wilt, kunt u voorwaarden, zoals 'Deze app aan uw organisatie toevoegen' en een knop weergeven, zoals in het volgende voorbeeld:

![Soorten toepassingen en scenario 's](./media/howto-add-branding-in-azure-ad-apps/add-to-my-org.png)

We raden echter aan dat u verklarende tekst in plaats van knoppen schrijven. Bijvoorbeeld:

> *Als u al Office 365 of andere zakelijke services van Microsoft hebt gebruikt, kunt u < your_app_name > toegang verlenen tot gegevens van uw organisatie. Hierdoor kunnen uw gebruikers toegang tot < your_app_name > met hun bestaande werkaccounts.*

Voor het downloaden van het officiële Microsoft-logo voor gebruik in uw app, met de rechtermuisknop op het account dat u wilt gebruiken en sla deze op uw computer.

| Asset                                | PNG-indeling. | SVG-indeling |
| ------------------------------------ | ---------- | ---------- |
| Microsoft-logo  | ![Microsoft-logo PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_MSSymbol_19.png) | ![SVG-Microsoft-logo](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_MSSymbol_19.svg) |

## <a name="visual-guidance-for-sign-in"></a>Visual richtlijnen voor het aanmelden

Uw app moet een knop aanmelden waarmee gebruikers worden omgeleid naar het eindpunt aanmelden die overeenkomt met het protocol dat u gebruiken om te integreren met Azure AD worden weergegeven. De volgende sectie biedt informatie over hoe die knop eruit moet zien.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictogram en 'Meld u aan met Microsoft'

Dit is de koppeling van het Microsoft-logo en de voorwaarden voor 'Sign in met Microsoft' die staan voor een unieke Azure AD met andere id-providers uw app kan ondersteunen. Als u niet genoeg ruimte heeft voor 'Sign in met Microsoft', is het om moet u deze 'Aanmelden.' U kunt een lichte of donkere kleurenschema voor de knoppen.

Het volgende diagram toont dat de Microsoft aanbevolen redlines bij het gebruik van de activa met uw app. De redlines van toepassing op 'Sign in met Microsoft' of de kortere 'Aanmelden' versie.

![Aanmelden bij Microsoft redlines](./media/howto-add-branding-in-azure-ad-apps/Sign-in-with-Microsoft-redlines.png)

Voor het downloaden van de officiële afbeeldingen voor gebruik in uw app, met de rechtermuisknop op het account dat u wilt gebruiken en sla deze op uw computer.

| Asset                                | PNG-indeling. | SVG-indeling |
| ------------------------------------ | ---------- | ---------- |
| Aanmelden bij Microsoft (donker thema)  | ![Meld u aan knop donker thema PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark.png) | ![Meld u aan met Microsoft knop donker thema SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark.svg) |
| Aanmelden bij Microsoft (licht thema) | ![Meld u aan knop lichte thema PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light.png) | ![Meld u aan met Microsoft knop lichte thema SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light.svg) |
| Meld u aan (donker thema)                 | ![Meld u aan in een korte knop donker thema PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark_short.png) | ![Meld u aan in een korte knop donker thema SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark_short.svg) |
| Meld u aan (licht thema)                | ![Meld u aan in een korte knop lichte thema PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light_short.png) | ![Meld u aan in een korte knop lichte thema SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light_short.svg) |


## <a name="branding-dos-and-donts"></a>Huisstijl van uw taken en niet moet doen

**Voer** 'account voor werk of school' gebruiken in combinatie met de knop 'Sign in met Microsoft' voor aanvullende uitleg zodat eindgebruikers herkennen of kan worden gebruikt. **Geen** gebruiken andere termen, zoals 'enterprise-account', 'zakelijke account' of 'zakelijk account'.

**Geen** gebruiken 'Office 365-ID' of 'Azure-id '. Office 365 is ook de naam van een gebruiker van Microsoft, die geen Azure AD voor verificatie gebruiken.

**Geen** wijzigen van het Microsoft-logo.

**Geen** blootstellen eindgebruikers kunnen de merken die Azure of Active Directory. Het is geen probleem echter het gebruik van deze voorwaarden met ontwikkelaars, IT-professionals en beheerders.

## <a name="navigation-dos-and-donts"></a>Navigatie-taken en niet moet doen

**Voer** bieden een manier voor gebruikers zich afmelden en schakel over naar een ander gebruikersaccount. Hoewel de meeste mensen één persoonlijke account van Microsoft/Facebook/Google/Twitter hebben, zijn mensen vaak gekoppeld aan meer dan één organisatie. Ondersteuning voor meerdere aangemelde gebruikers wordt binnenkort beschikbaar.
