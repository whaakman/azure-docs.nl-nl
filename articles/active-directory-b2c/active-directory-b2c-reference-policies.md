---
title: Ingebouwde beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp op de uitbreidbare beleidsframework van Azure Active Directory B2C en voor het maken van verschillende beleidstypen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4a0dfcbba1867d4792a0e4a383ee097df0ea410b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52835808"
---
# <a name="azure-active-directory-b2c-user-flows"></a>Azure Active Directory B2C: Gebruikersstromen


Het framework uitbreidbaar beleid van Azure Active Directory (Azure AD) B2C, is de belangrijkste sterkte van de service. Volledig beschrijven consument identiteitservaringen zoals beleid voor registreren, aanmelden of profiel bewerken. Instellen om u te helpen de meest algemene taken voor identiteit, bevat vooraf gedefinieerde, configureerbare beleidsregels met de naam van de Azure AD B2C-portal **gebruikersstromen**. Bijvoorbeeld, kunt een proefaccount gebruikersstroom u gedrag beheren door het configureren van de volgende instellingen:

* Accounttypen (sociale accounts zoals Facebook) of lokale accounts, zoals e-mailadressen die consumenten gebruiken kunnen om u te registreren voor de toepassing
* Kenmerken (bijvoorbeeld voornaam, postcode en schoen grootte) moeten worden verzameld van de gebruiker tijdens de registratie
* Gebruik van Azure multi-factor Authentication
* Het uiterlijk van alle aanmeldingspagina 's
* Informatie (die als claims in een token) dat de toepassing ontvangt wanneer de gebruiker stroomuitvoering is voltooid

U kunt meerdere gebruikersstromen van verschillende typen in uw tenant maken en deze gebruiken in uw toepassingen, indien nodig. Gebruikersstromen kunnen worden hergebruikt binnen toepassingen. Met deze mogelijkheden kan ontwikkelaars om te definiëren en ervaring van consumenten de identiteit met minimale of geen wijzigingen in hun code wijzigen.

Gebruikersstromen zijn beschikbaar voor gebruik via een eenvoudige developer-interface. Uw toepassing wordt een gebruikersstroom geactiveerd met behulp van een standaard HTTP-verificatieaanvraag (een gebruiker stroom-parameter doorgeven in de aanvraag) en een aangepaste token als antwoord ontvangt. Bijvoorbeeld, is het enige verschil tussen aanvragen die een proefaccount gebruikersstroom aanroepen en aanvragen die een gebruikersstroom aanmelden aanroepen de userjourney-naam die wordt gebruikt in de parameter 'p' query-tekenreeks:

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow

```

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow

```

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>Een gebruikersstroom registreren of aanmelden maken

Deze gebruikersstroom worden beide consumentervaringen voor registratie en aanmelden met een configuratie voor één verwerkt. Consumenten worden geleid op het juiste pad (registreren of aanmelden) zijn afhankelijk van de context. Hierin wordt ook de inhoud van tokens die de toepassing van registraties zijn voltooid of aanmeldingen ontvangt.  Voorbeeldcode voor het **registreren of aanmelden** gebruikersstroom is [beschikbaar hier](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Het is raadzaam dat u deze gebruikersstroom via gebruiken een **aanmelding** gebruikersstroom of een **aanmelden** gebruikersstroom.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-user-flow"></a>Een proefaccount gebruikersstroom maken

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-user-flow"></a>Een gebruikersstroom aanmelden maken

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-user-flow"></a>Een beleid voor profielbewerking maken

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-user-flow"></a>Maken van een wachtwoord opnieuw instellen van beleid

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-user-flows"></a>Preview-gebruikersstromen

Als we nieuwe functies beschikbaar zijn, enkele van deze mogelijk niet beschikbaar zijn op bestaande beleidsregels of gebruikersstromen.  We willen oudere versies vervangen door de nieuwste versie van hetzelfde type zodra deze gebruikersstromen Voer algemene beschikbaarheid.  Om te profiteren van deze nieuwe functies staan u nieuwe gebruikersstromen maken en uw bestaande beleidsregels of gebruikersstromen verandert niet.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-do-i-link-a-sign-up-or-sign-in-user-flow-with-a-password-reset-user-flow"></a>Hoe koppel ik een gebruikersstroom registreren of aanmelden met een wachtwoord opnieuw instellen van beleid
Bij het maken van een **registreren of aanmelden** gebruiker flow (met lokale accounts), ziet u een **wachtwoord vergeten?** koppeling op de eerste pagina van de ervaring. Op deze koppeling te klikken niet automatisch trigger een wachtwoord opnieuw instellen van beleid. 

In plaats daarvan de foutcode **`AADB2C90118`** wordt geretourneerd naar uw app. Uw app moet aanroepen van een specifiek wachtwoord opnieuw instellen van beleid voor het verwerken van deze foutcode. Zie voor meer informatie, een [voorbeeld ziet u de aanpak van het koppelen van gebruikersstromen](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-user-flow-or-a-sign-up-user-flow-and-a-sign-in-user-flow"></a>Moet ik een gebruikersstroom registreren of aanmelden of een aanmelding gebruikersstroom en een gebruikersstroom aanmelden gebruiken?
Het is raadzaam dat u een **registreren of aanmelden** gebruikersstroom via een **Meld u aan** gebruikersstroom en een **aanmelden** gebruikersstroom.  

De **registreren of aanmelden** gebruikersstroom beschikt over meer mogelijkheden dan het **aanmelden** gebruikersstroom. Ook kunt u pagina UI-aanpassing en biedt betere ondersteuning voor lokalisatie. 

De **aanmelden** gebruikersstroom wordt aanbevolen als u niet nodig hebt om te lokaliseren van uw gebruikersstromen, alleen kleine aanpassingsmogelijkheden nodig hebben voor de huisstijl en wachtwoord wilt herstellen die zijn ingebouwd.

## <a name="next-steps"></a>Volgende stappen
* [Token-, sessie en configuratie voor eenmalige aanmelding](active-directory-b2c-token-session-sso.md)
* [Tijdens de consumer Meld u aan een e-mailverificatie uitschakelen](active-directory-b2c-reference-disable-ev.md)

