---
title: Toepassingsregistratie - Azure Active Directory B2C
description: Uw toepassing registreren met Azure Active Directory B2C
services: active-directory-b2c
author: PatAltimore
manager: mtillman
editor: parakhj
ms.custom: seo
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.openlocfilehash: b1d145466382c8fc2ea6c5e4e295940b0f000b97
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: uw toepassing registreren

Met behulp van deze Quickstart kunt u binnen enkele minuten een toepassing registreren in een B2C-tenant van Microsoft Azure Active Directory (Azure AD). Wanneer u klaar bent, is de toepassing geregistreerd voor gebruik in de Azure Active Directory B2C-tenant.

## <a name="prerequisites"></a>Vereisten

Als u een toepassing wilt maken waarin consumenten zich kunnen registreren en aanmelden, moet u de toepassing eerst registreren bij een Azure Active Directory B2C-tenant. Haal uw eigen tenant op aan de hand van de stappen in [Een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md).

Toepassingen die zijn gemaakt in Azure Portal, moeten vanaf dezelfde locatie worden beheerd. Als u de Azure AD B2C-toepassingen bewerkt met PowerShell of een andere portal, worden de toepassingen niet meer ondersteund en werken ze niet met Azure AD B2C. Zie voor meer informatie de sectie [Mislukte toepassingen](#faulted-apps). 

Dit artikel bevat uitleg waarmee u met onze voorbeelden aan de slag kunt gaan. U kunt in de volgende artikelen meer over deze voorbeelden te weten komen.

## <a name="navigate-to-b2c-settings"></a>Navigeren naar de B2C-instellingen

Meld u als globale beheerder van de B2C-tenant aan bij [Azure Portal](https://portal.azure.com/). 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

## <a name="choose-next-steps-based-on-your-application-type"></a>Kies de volgende stappen op basis van het toepassingstype

* [Een web-app registreren](#register-a-web-app)
* [Een web-API registreren](#register-a-web-api)
* [Een mobiele of native toepassing registreren](#register-a-mobile-or-native-app)
 
### <a name="register-a-web-app"></a>Een web-app registreren

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

### <a name="create-a-web-app-client-secret"></a>Een web-app-clientgeheim maken

Als de web-app een web-API aanroept die is beveiligd door Azure AD B2C, voert u deze stappen uit:
   1. Maak een toepassingsgeheim door naar de blade **Sleutels** te gaan en op de knop **Sleutel genereren** te klikken. Noteer de waarde van **App-sleutel**. U gebruikt de waarde als het toepassingsgeheim in de code van uw toepassing.
   2. Klik op **API-toegang**, klik op **Toevoegen** en selecteer uw web-API en bereiken (machtigingen).

> [!NOTE]
> Een **toepassingsgeheim** is een belangrijke beveiligingsreferentie en moet op de juiste wijze worden beveiligd.
> 

[Ga naar **Volgende stappen**](#next-steps)

### <a name="register-a-web-api"></a>Een web-API registreren

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Klik op **Gepubliceerd bereiken** om eventueel meer bereiken toe te voegen. Standaard wordt het bereik 'user_impersonation' gedefinieerd. Dit bereik biedt andere toepassingen de mogelijkheid om namens de aangemelde gebruiker toegang te krijgen tot deze API. Als u wilt, kunt u het bereik user_impersonation verwijderen.

[Ga naar **Volgende stappen**](#next-steps)

### <a name="register-a-mobile-or-native-app"></a>Een mobiele of native toepassing registreren

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Ga naar **Volgende stappen**](#next-steps)

## <a name="limitations"></a>Beperkingen

### <a name="choosing-a-web-app-or-api-reply-url"></a>De antwoord-URL van een web-app of -API kiezen

Toepassingen die zijn geregistreerd bij Azure AD B2C zijn momenteel beperkt tot een vast aantal waarden voor antwoord-URL's. De antwoord-URL voor webtoepassingen en services moet met het schema `https` beginnen. Alle waarden voor antwoord-URL's moeten één DNS-domein delen. U kunt bijvoorbeeld geen webtoepassing met een van deze antwoord-URL's registreren:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Het registratiesysteem vergelijkt de volledige DNS-naam van de bestaande antwoord-URL met de DNS-naam van de antwoord-URL die u wilt toevoegen. De aanvraag voor het toevoegen van de DNS-naam mislukt als aan een van de volgende voorwaarden wordt voldaan:

* De volledige DNS-naam van de nieuwe antwoord-URL komt niet overeen met de DNS-naam van de bestaande antwoord-URL.
* De volledige DNS-naam van de nieuwe antwoord-URL is geen subdomein van de bestaande antwoord-URL.

Als de toepassing bijvoorbeeld deze antwoord-URL heeft:

`https://login.contoso.com`

Dan kunt u er als volgt aan toevoegen:

`https://login.contoso.com/new`

In dit geval komt de DNS-naam precies overeen. Of u kunt dit doen:

`https://new.login.contoso.com`

In dit geval verwijst u naar een DNS-subdomein van login.contoso.com. Als u een toepassing hebt die login-east.contoso.com en login-west.contoso.com als antwoord-URL's heeft, moet u de antwoord-URL's in deze volgorde toevoegen:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste antwoord-URL, contoso.com, zijn.

### <a name="choosing-a-native-app-redirect-uri"></a>Een omleidings-URI voor een native app kiezen

Er zijn twee belangrijke overwegingen bij het kiezen van een omleidings-URI voor mobiele/native toepassingen:

* **Uniek**: het schema van de omleidings-URI moet voor elke toepassing uniek zijn. In het voorbeeld (com.onmicrosoft.contoso.app-naam://redirect/pad) is com.onmicrosoft.contoso.app-naam het schema. We raden aan dit patroon te volgen. Als twee toepassingen hetzelfde schema delen, ziet de gebruiker een dialoogvenster voor het kiezen van een app. Als de gebruiker een foute keuze maakt, mislukt de aanmelding.
* **Volledig**: de omleidings-URI moet een schema en een pad hebben. Het pad moet ten minste één forward slash bevatten na het domein (zo werkt //contoso/ bijvoorbeeld wel en //contoso niet).

Zorg ervoor dat er geen speciale tekens zoals onderstrepingstekens in de omleidings-URI staan.

### <a name="faulted-apps"></a>Mislukte toepassingen

B2C-toepassingen moeten niet worden bewerkt:

* Op andere appbeheerportals zoals de [portal voor appregistratie](https://apps.dev.microsoft.com/).
* Met Graph API of PowerShell

Als u de Azure AD B2C-toepassing bewerkt zoals hierboven wordt beschreven en de toepassing opnieuw probeert te bewerken in Azure AD B2C-functies in Azure Portal, verandert deze in een mislukte toepassing die niet meer kan worden gebruikt met Azure AD B2C. U moet de toepassing verwijderen en deze opnieuw maken.

Als u de toepassing wilt verwijderen, gaat u naar de [portal voor appregistratie](https://apps.dev.microsoft.com/) en verwijdert u daar de toepassing. De toepassing is alleen zichtbaar als u de eigenaar van de toepassing bent (en niet een beheerder van de tenant).

## <a name="next-steps"></a>Volgende stappen

Nu u een toepassing hebt die is geregistreerd bij Azure AD B2C, kunt u snel aan de slag gaan aan de hand van een van [de snelstartzelfstudies](active-directory-b2c-overview.md#get-started).

> [!div class="nextstepaction"]
> [Een ASP.NET-web-app maken met registreren, aanmelden en wachtwoord opnieuw instellen](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
