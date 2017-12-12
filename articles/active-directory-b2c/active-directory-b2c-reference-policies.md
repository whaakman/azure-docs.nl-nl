---
title: 'Azure Active Directory B2C: Ingebouwde beleid | Microsoft Docs'
description: Een onderwerp in de uitbreidbaar beleidsframework van Azure Active Directory B2C en voor het maken van verschillende beleidstypen
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mtillman
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.openlocfilehash: f0aa3d19e15837b75888293f0cd19683b7621a6a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Ingebouwde beleid


De uitbreidbaar beleidsframework van Azure Active Directory (Azure AD) B2C is de belangrijkste sterkte van de service. Volledig beschrijven consumer identiteitservaringen zoals beleidsregels registreren, aanmelden en profiel bewerken. Bijvoorbeeld, kunt een registratiebeleid u bepalen van gedrag door de volgende instellingen configureren:

* Accounttypen (sociale accounts zoals Facebook) of lokale accounts zoals e-mailadressen die consumenten zich aanmelden voor de toepassing kunnen gebruiken
* Kenmerken (bijvoorbeeld de voornaam, postcode en schoen grootte) worden opgehaald van de consument tijdens de registratie
* Gebruik van Azure multi-factor Authentication
* Het uiterlijk van alle aanmeldingspagina 's
* (Die zich voordoet als claims in een token) dat de toepassing wanneer het beleid ontvangt uitgevoerd is voltooid

U kunt meerdere beleidsregels met verschillende typen in uw tenant maken en gebruiken in uw toepassingen indien nodig. Beleid kunnen opnieuw worden gebruikt in toepassingen. Deze flexibiliteit kan ontwikkelaars definiëren en ervaringen van de consument identiteit met minimale of geen wijzigingen aangebracht in de code te wijzigen.

Beleidsregels zijn beschikbaar voor gebruik via een eenvoudige developer-interface. Uw toepassing een beleid wordt geactiveerd met behulp van een standaard HTTP-authenticatie-aanvraag (het doorgeven van een parameter van het beleid in de aanvraag) en een aangepaste token als antwoord ontvangt. Het enige verschil tussen aanvragen die gebruikmaken van een registratiebeleid en aanvragen die gebruikmaken van een beleid voor aanmelden is bijvoorbeeld de naam van het beleid dat wordt gebruikt in de parameter 'p' query-tekenreeks:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Zie voor meer informatie over de beleidsframework [dit blogbericht over Azure AD B2C op de Enterprise Mobility and Security-Blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-or-sign-in-policy"></a>Maak een beleid registreren of aanmelden

Dit beleid verwerkt beide ervaringen consumenten registreren en aanmelden met een configuratie voor één. Consumenten worden omlaag het juiste pad (registreren of aanmelden) zijn afhankelijk van de context geleid. Hierin worden ook de inhoud van de tokens die de toepassing na geslaagde aanmelding-ups of aanmeldingen ontvangen.  Een voorbeeld van code voor het registreren of aanmelden beleid is [beschikbaar hier](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Het wordt aanbevolen dat u dit beleid via een registratiebeleid en beleid voor aanmelden gebruiken.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Een registratiebeleid maken

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Maak een beleid voor aanmelden

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Maken van een profiel te bewerken van beleid

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Maak een beleid voor wachtwoord opnieuw instellen

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Hoe kan ik een beleid registreren of aanmelden met een beleid voor wachtwoordherstel koppelen?
Wanneer u een beleid registreren of aanmelden (met lokale accounts) maakt, ziet u een **wachtwoord vergeten?** koppeling op de eerste pagina van de gebruikerservaring. Op deze koppeling klikt herstellen niet automatisch trigger een wachtwoord beleid. 

In plaats daarvan de foutcode  **`AADB2C90118`**  wordt geretourneerd naar uw app. Uw app moet deze foutcode verwerkt door het aanroepen van een specifiek wachtwoord opnieuw instellen van beleid. Zie voor meer informatie een [voorbeeldbestand dat laat zien van de aanpak van het koppelen van beleidsregels](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Moet ik een beleid registreren of aanmelden of een registratiebeleid en een beleid voor aanmelden gebruiken?
Het is raadzaam dat u een beleid registreren of aanmelden via een registratiebeleid en een beleid voor aanmelden.  

Het registreren of aanmelden beleid heeft meer mogelijkheden dan het beleid voor aanmelden. Ook kunt u gebruikmaken van pagina UI-aanpassing en biedt een betere ondersteuning voor lokalisatie. 

Het beleid voor aanmelden wordt aanbevolen als u niet nodig hebt voor uw beleid lokalisatie, alleen secundaire aanpassingsfuncties voor huisstijl nodig hebt en wilt wachtwoord opnieuw instellen die zijn ingebouwd.

## <a name="next-steps"></a>Volgende stappen
* [Token, sessie en configuratie voor één aanmelding](active-directory-b2c-token-session-sso.md)
* [E-mailverificatie tijdens registratie consumer uitschakelen](active-directory-b2c-reference-disable-ev.md)

