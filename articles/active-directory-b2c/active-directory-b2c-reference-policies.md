---
title: Ingebouwde beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp op de uitbreidbare beleidsframework van Azure Active Directory B2C en voor het maken van verschillende beleidstypen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f26db8bcb50fa09a8d2829d477f90cac8c52533f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337571"
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Ingebouwd beleid


Het framework uitbreidbaar beleid van Azure Active Directory (Azure AD) B2C, is de belangrijkste sterkte van de service. Volledig beschrijven consument identiteitservaringen zoals beleid voor registreren, aanmelden of profiel bewerken. Zo kunt u bijvoorbeeld een registratiebeleid definiëren door het configureren van de volgende instellingen:

* Accounttypen (sociale accounts zoals Facebook) of lokale accounts, zoals e-mailadressen die consumenten gebruiken kunnen om u te registreren voor de toepassing
* Kenmerken (bijvoorbeeld voornaam, postcode en schoen grootte) moeten worden verzameld van de gebruiker tijdens de registratie
* Gebruik van Azure multi-factor Authentication
* Het uiterlijk van alle aanmeldingspagina 's
* Informatie (die als claims in een token) dat de toepassing ontvangt als het beleid uitgevoerd is voltooid

U kunt meerdere beleidsregels van verschillende typen maken in uw tenant en ze in uw toepassingen indien nodig kunnen gebruiken. Beleidsregels kunnen worden hergebruikt binnen toepassingen. Met deze mogelijkheden kan ontwikkelaars om te definiëren en ervaring van consumenten de identiteit met minimale of geen wijzigingen in hun code wijzigen.

Beleidsregels zijn beschikbaar voor gebruik via een eenvoudige developer-interface. Uw toepassing wordt een beleid wordt geactiveerd met behulp van een standaard HTTP-verificatieaanvraag (een parameter van het beleid in de aanvraag doorgegeven) en een aangepaste token als antwoord ontvangt. Het enige verschil tussen aanvragen waarmee een registratiebeleid kunt aanroepen en aanvragen die aanroepen van een beleid voor aanmelden is bijvoorbeeld de naam van het beleid dat wordt gebruikt in de parameter 'p' query-tekenreeks:

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
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

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

## <a name="create-a-sign-up-or-sign-in-policy"></a>Registratie- of aanmeldingsbeleid maken

Dit beleid worden beide consumentervaringen voor registratie en aanmelden met een configuratie voor één verwerkt. Consumenten worden geleid op het juiste pad (registreren of aanmelden) zijn afhankelijk van de context. Hierin wordt ook de inhoud van tokens die de toepassing van registraties zijn voltooid of aanmeldingen ontvangt.  Voorbeeldcode voor het **registreren of aanmelden** beleid [beschikbaar hier](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Het is raadzaam dat u dit beleid via gebruiken een **aanmelding** beleid of een **aanmelden** beleid.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Een registratiebeleid maken

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Maak een beleid voor aanmelden

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Een beleid voor profielbewerking maken

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Een beleid voor het opnieuw instellen van het wachtwoord maken

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-policies"></a>Voorbeeld van beleid

Als we nieuwe functies beschikbaar zijn, enkele van deze mogelijk niet beschikbaar zijn op bestaande beleidsregels.  We willen oudere versies vervangen door de nieuwste versie van hetzelfde type zodra deze beleidsregels Voer algemene beschikbaarheid.  Om te profiteren van deze nieuwe functies staan u nieuw beleid maken en uw bestaande beleid heeft geen invloed op.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Hoe koppel ik een beleid voor registreren of aanmelden met een beleid voor wachtwoord opnieuw instellen?
Bij het maken van een **registreren of aanmelden** -beleid (met lokale accounts), ziet u een **wachtwoord vergeten?** koppeling op de eerste pagina van de ervaring. Op deze koppeling te klikken niet automatisch trigger een wachtwoord opnieuw instellen van beleid. 

In plaats daarvan de foutcode **`AADB2C90118`** wordt geretourneerd naar uw app. Uw app moet aanroepen van een specifiek wachtwoord opnieuw instellen van beleid voor het verwerken van deze foutcode. Zie voor meer informatie, een [voorbeeld ziet u de aanpak van het koppelen van beleid](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Moet ik een beleid voor registreren of aanmelden of een registratiebeleid en een beleid voor aanmelden gebruiken?
Het is raadzaam dat u een **registreren of aanmelden** beleid via een **Meld u aan** beleid en een **aanmelden** beleid.  

De **registreren of aanmelden** beleid heeft meer mogelijkheden dan het **aanmelden** beleid. Ook kunt u pagina UI-aanpassing en biedt betere ondersteuning voor lokalisatie. 

De **aanmelden** beleid wordt aanbevolen als u niet nodig hebt om te lokaliseren van uw beleid, alleen kleine aanpassingsmogelijkheden nodig hebben voor de huisstijl en wilt dat wachtwoord opnieuw instellen die zijn ingebouwd in het.

## <a name="next-steps"></a>Volgende stappen
* [Token-, sessie en configuratie voor eenmalige aanmelding](active-directory-b2c-token-session-sso.md)
* [Tijdens de consumer Meld u aan een e-mailverificatie uitschakelen](active-directory-b2c-reference-disable-ev.md)

