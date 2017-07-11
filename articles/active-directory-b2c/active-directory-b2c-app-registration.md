---
title: 'Azure Active Directory B2C: toepassingsregistratie | Microsoft Docs'
description: Uw toepassing registreren met Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 3499ff57e650c70679dfa018eec5dbe1a6173a33
ms.contentlocale: nl-nl
ms.lasthandoff: 07/04/2017



---
<a id="azure-active-directory-b2c-register-your-application" class="xliff"></a>

# Azure Active Directory B2C: uw toepassing registreren

> [!IMPORTANT]
> Toepassingen die zijn gemaakt vanaf de blade Azure AD B2C in Azure Portal, moeten vanaf dezelfde locatie worden beheerd. Als u de B2C-toepassingen bewerkt met PowerShell of een andere portal, worden de toepassingen niet meer ondersteund en werken ze niet met Azure AD B2C. U vindt [hieronder](#faulted-apps) meer informatie.
>

<a id="prerequisite" class="xliff"></a>

## Vereiste

Als u een toepassing wilt maken waarin consumenten zich kunnen registreren en aanmelden, moet u de toepassing eerst registreren bij een Azure Active Directory B2C-tenant. Haal uw eigen tenant op aan de hand van de stappen in [Een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md). Als u alle stappen in dit artikel hebt uitgevoerd, is de blade B2C-functies vastgemaakt aan uw Startboard.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

<a id="navigate-to-the-b2c-features-blade" class="xliff"></a>

## Ga naar de blade B2C-functies

Als u de blade B2C-functies hebt vastgemaakt aan uw Startboard, ziet u de blade zodra u zich aanmeldt bij de [Azure Portal](https://portal.azure.com/) als globale beheerder van de B2C-tenant.

U kunt de blade ook openen door te klikken op **Meer services** en vervolgens te zoeken op **Azure AD B2C** in het linkernavigatiedeelvenster in [Azure Portal](https://portal.azure.com/).

> [!IMPORTANT]
> U moet een globale beheerder van de B2C-tenant zijn om de blade B2C-functies te kunnen openen. Een globale beheerder van andere tenant of een gebruiker van een tenant heeft hiertoe geen toegang.  U kunt overschakelen naar uw B2C-tenant met behulp van de tenantwisselaar in de rechterbovenhoek van Azure Portal.
>
>

<a id="register-a-web-application" class="xliff"></a>

## Een web-app registreren

1. Klik op de blade B2C-functies in de Azure Portal op **Toepassingen**.
1. Klik op **+Toevoegen** boven aan de blade.
1. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld Contoso B2C-app opgeven.
1. Zet de schakeloptie **Inclusief web-app/web-API** op **Ja**.
1. Voer [een juiste](#limitations) waarde in voor de **Antwoord-URL's**, die eindpunten zijn waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. Geef bijvoorbeeld `https://localhost:44316/` op.
1. Klik op **Maken** om uw toepassing te registreren.
1. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.
1. Als uw webtoepassing ook een web-API aanroept die is beveiligd door Azure AD B2C, moet u het volgende doen:
    1. Maak een **toepassingsgeheim** door naar de blade **Sleutels** te gaan en te klikken op de knop **Sleutel genereren**.
    1. Klik op **API-toegang**, klik op **Toevoegen** en selecteer vervolgens uw web-API en scopes (machtigingen).

> [!NOTE]
> Een **toepassingsgeheim** is een belangrijke beveiligingsreferentie en moet op de juiste wijze worden beveiligd.
>

<a id="register-a-web-api" class="xliff"></a>

## Een web-API registreren

1. Klik op de blade B2C-functies in de Azure Portal op **Toepassingen**.
1. Klik op **+Toevoegen** boven aan de blade.
1. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld 'Contoso B2C-API' opgeven.
1. Zet de schakeloptie **Inclusief web-app/web-API** op **Ja**.
1. Voer [een juiste](#choosing-a-web-app/api-reply-url) waarde in voor de **Antwoord-URL's**, die eindpunten zijn waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. Geef bijvoorbeeld `https://localhost:44316/` op.
1. Voer een waarde in voor **URI voor de app-id**. Dit is de id die wordt gebruikt voor uw web-API. Voer bijvoorbeeld 'notities' in. De volledige URI-id wordt eronder gegenereerd.
1. Klik op **Maken** om uw toepassing te registreren.
1. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.
1. Klik op **Gepubliceerd bereiken**. Dit is waar u de machtigingen (bereiken) definieert die kunnen worden verleend aan andere toepassingen.
1. Voeg indien nodig meer bereiken toe. Standaard wordt het bereik 'user_impersonation' gedefinieerd. Dit bereik biedt andere toepassingen de mogelijkheid om namens de aangemelde gebruiker toegang te krijgen tot deze API. Dit bereik kan desgewenst ook weer worden ingetrokken.
1. Klik op **Opslaan**.

<a id="register-a-mobilenative-application" class="xliff"></a>

## Een mobiele/native toepassing registreren

1. Klik op de blade B2C-functies in de Azure Portal op **Toepassingen**.
1. Klik op **+Toevoegen** boven aan de blade.
1. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld Contoso B2C-app opgeven.
1. Zet de schakeloptie **Inclusief native client** op **Ja**.
1. Voer bij **Omleidings-URI** een URI met een aangepast schema in. Typ bijvoorbeeld com.onmicrosoft.contoso.app-naam://redirect/pad. Zorg ervoor dat u een [goede omleidings-URI](#choosing-a-native-application-redirect-uri) kiest en geen speciale tekens zoals onderstrepingstekens gebruikt.
1. Klik op **Opslaan** om de toepassing te registreren.
1. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.
1. Als uw systeemeigen toepassing ook een web-API aanroept die is beveiligd door Azure AD B2C, moet u het volgende doen:
    1. Maak een **toepassingsgeheim** door naar de blade **Sleutels** te gaan en te klikken op de knop **Sleutel genereren**.
    1. Klik op **API-toegang**, klik op **Toevoegen** en selecteer vervolgens uw web-API en scopes (machtigingen).

> [!NOTE]
> Een **toepassingsgeheim** is een belangrijke beveiligingsreferentie en moet op de juiste wijze worden beveiligd.
>

<a id="limitations" class="xliff"></a>

## Beperkingen

<a id="choosing-a-web-appapi-reply-url" class="xliff"></a>

### De antwoord-URL van een webtoepassing/-API kiezen

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

<a id="choosing-a-native-application-redirect-uri" class="xliff"></a>

### Een omleidings-URI voor een systeemeigen toepassing kiezen

Er zijn twee belangrijke overwegingen bij het kiezen van een omleidings-URI voor mobiele/native toepassingen:

* **Uniek**: het schema van de omleidings-URI moet voor elke toepassing uniek zijn. In ons voorbeeld (com.onmicrosoft.contoso.app-naam://redirect/pad) gebruiken we com.onmicrosoft.contoso.app-naam als het schema. We raden aan dit patroon te volgen. Als twee toepassingen hetzelfde schema delen, ziet de gebruiker een dialoogvenster voor het kiezen van een app. Als de gebruiker een onjuiste keuze maakt, mislukt de aanmelding.
* **Volledig**: de omleidings-URI moet een schema en een pad hebben. Het pad moet ten minste één forward slash bevatten na het domein (zo werkt //contoso/ bijvoorbeeld wel en //contoso niet).

Zorg ervoor dat er geen speciale tekens zoals onderstrepingstekens in de omleidings-URI staan.

<a id="faulted-apps" class="xliff"></a>

### Mislukte toepassingen

B2C-toepassingen moeten niet worden bewerkt:

* Op andere appbeheerportals zoals de [klassieke Azure-portal](https://manage.windowsazure.com/) en de [portal voor appregistratie](https://apps.dev.microsoft.com/).
* Met Graph API of PowerShell

Als u de B2C-toepassing bewerkt zoals hierboven wordt beschreven en de toepassing opnieuw probeert te bewerken in de blade Azure AD B2C-functies op Azure Portal, verandert deze in een mislukte toepassing die niet meer kan worden gebruikt met Azure AD B2C. U moet de toepassing verwijderen en deze opnieuw maken.

Als u de toepassing wilt verwijderen, gaat u naar de [portal voor appregistratie](https://apps.dev.microsoft.com/) en verwijdert u daar de toepassing. De toepassing is alleen zichtbaar als u de eigenaar van de toepassing bent (en niet een beheerder van de tenant).

<a id="next-steps" class="xliff"></a>

## Volgende stappen

Nu u een toepassing hebt die is geregistreerd bij Azure AD B2C, kunt u snel aan de slag gaan aan de hand van een van [onze snelstartzelfstudies](active-directory-b2c-overview.md#get-started).

