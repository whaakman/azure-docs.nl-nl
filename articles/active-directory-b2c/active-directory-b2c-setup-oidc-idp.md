---
title: Id-providers OpenID Connect toe te voegen in ingebouwde beleid - Azure AD B2C | Microsoft Docs
description: Overzicht van handleiding voor het toevoegen van OpenID Connect-providers in ingebouwde beleidsregels in Azure AD B2C
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: parakhj
ms.assetid: 357d193a-e33b-469c-8a93-0a8f45a60a9f
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/27/2018
ms.author: davidmu
ms.openlocfilehash: 46223f1ec6b82828983861dae26ec8d777b77b86
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2018
---
# <a name="azure-active-directory-b2c-add-a-custom-openid-connect-identity-provider-in-built-in-policies"></a>Azure Active Directory B2C: Een aangepaste OpenID Connect id-provider in ingebouwde beleid toevoegen

>[!NOTE]
> Deze functie is openbare preview. Gebruik de functie niet in een productieomgeving.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) is een authenticatieprotocol gebouwd op OAuth 2.0, die kan worden gebruikt om gebruikers veilig aanmelden. De meeste id-providers die gebruikmaken van dit protocol, zoals [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md), worden ondersteund in Azure AD B2C. Dit artikel wordt uitgelegd hoe u aangepaste OpenID Connect identiteitsproviders naar uw ingebouwde beleid kunt toevoegen.

## <a name="configuring-a-custom-openid-connect-identity-provider"></a>Een aangepaste OpenID Connect id-provider configureren

Een aangepaste OpenID Connect id-provider toevoegen:

1. Volg deze stappen voor [gaat u naar de Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
1. Klik op **identiteitsproviders**.
1. Klik op **Toevoegen**.
1. Voor de **identiteit providertype**, selecteer **OpenID Connect**.

### <a name="setting-up-the-openid-connect-identity-provider"></a>Instellen van de identiteitsprovider OpenID Connect

#### <a name="metadata-url"></a>Metagegevens-URL

Volgens de specificatie beschrijft elke identiteitsproviders OpenID Connect een document met metagegevens die de meeste van de informatie die nodig zijn om uit te voeren aanmelden bevat. Dit omvat gegevens zoals de URL's te gebruiken en de locatie van de openbare handtekeningsleutels van de service. Het metagegevensdocument OpenID Connect bevindt zich altijd een eindpunt dat eindigt in `.well-known\openid-configuration`.

U zoekt voor de identiteitsprovider OpenID Connect toevoegen, voert u de metagegevens-URL.

#### <a name="client-id-and-secret"></a>Client-ID en geheim

De id-provider nodig ontwikkelaars een toepassing registreren in hun service zodat gebruikers zich aanmelden. Deze toepassing wordt een ID hebben (aangeduid als de **client-ID**) en een **clientgeheim**. Kopieer deze waarden van de id-provider en de bijbehorende velden invoeren.

> [!NOTE]
> Het clientgeheim is optioneel. U moet echter een clientgeheim invoeren als u wilt gebruiken de [autorisatiecodestroom](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), waarbij het geheim wordt gebruikt voor het uitwisselen van de code voor het token.

#### <a name="scope"></a>Bereik

Bereiken definiëren de informatie en machtigingen die u op zoek bent voor het verzamelen van uw aangepaste id-provider. Aanvragen OpenID Connect moeten bevatten de `openid` bereik waarde om te kunnen ontvangen van het token ID van de id-provider. Zonder het token ID gebruikers zich niet aanmelden bij Azure AD B2C met behulp van de aangepaste id-provider.

Andere scopes kunnen worden toegevoegd (gescheiden door een spatie). Raadpleeg de documentatie van de aangepaste identiteitsprovider om te zien welke andere scopes kunnen worden beschikbaar.

#### <a name="response-type"></a>Reactietype

Het antwoord van het type wordt beschreven wat voor soort informatie wordt verzonden terug in de eerste aanroep naar de `authorization_endpoint` van de provider van de aangepaste identiteit. 

* `code`: Conform de [autorisatiecodestroom](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), een code wordt geretourneerd naar Azure AD B2C. Azure AD B2C wordt gaat u verder aan te roepen de `token_endpoint` voor het uitwisselen van de code voor het token.
* `token`: Er is een toegangstoken terug naar Azure AD B2C geretourneerd door de provider van de aangepaste identiteit.
* `id_token`: Er is een token ID terug naar Azure AD B2C geretourneerd door de provider van de aangepaste identiteit.


#### <a name="response-mode"></a>Antwoordmodus

De modus antwoord bepaalt de methode die moet worden gebruikt om de gegevens van de aangepaste identiteitsprovider verzenden naar Azure AD B2C.

* `form_post`: In deze modus antwoord wordt aanbevolen voor de beste beveiliging. Het antwoord wordt verzonden via het HTTP `POST` methode met de code of het token wordt gecodeerd in de hoofdtekst met behulp van de `application/x-www-form-urlencoded` indeling.
* `query`: De code of het token wordt geretourneerd als een queryparameter.


#### <a name="domain-hint"></a>Domeinhint

De hint domein kan worden gebruikt om over te slaan rechtstreeks naar de aanmeldingspagina van de opgegeven id-provider, in plaats van de gebruiker moet een selectie uit de lijst van beschikbare id-providers. Voer een waarde voor de hint domein zodat dit type gedrag.

Toevoegen om door te gaan met de aangepaste identiteit-provider, de parameter `domain_hint=<domain hint value>` aan het einde van uw aanvraag bij het aanroepen van Azure AD B2C voor aanmelding.


### <a name="mapping-the-claims-from-the-openid-connect-identity-provider"></a>Toewijzing van de claims van de identiteitsprovider OpenID Connect

Na de aangepaste identiteit verzendt provider een token ID terug naar Azure AD B2C, Azure AD B2C-behoeften te kunnen toewijzen van de claims van het token ontvangen voor de claims die Azure AD B2C herkent en gebruikt. 

Raadpleeg de documentatie van de aangepaste identiteitsprovider om te begrijpen van de claims die worden weergegeven in de id-provider tokens voor elk van de onderstaande toewijzingen.

* `User ID`: Geef de claim met de unieke id voor de aangemelde gebruiker.
* `Display Name`: Geef de claim met de naam of de volledige naam voor de gebruiker.
* `Given Name`: Geef de claim waarmee de voornaam van de gebruiker.
* `Surname`: Geef de claim waarmee de achternaam van de gebruiker.
* `Email`: Geef de claim waarmee het e-mailadres van de gebruiker.

## <a name="next-steps"></a>Volgende stappen

Toevoegen van de aangepaste OpenID Connect identiteitsprovider naar uw [ingebouwde beleid](active-directory-b2c-reference-policies.md).
