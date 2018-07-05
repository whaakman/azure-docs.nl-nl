---
title: Id-providers OpenID Connect toe te voegen in de ingebouwde beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Overzicht van de handleiding over het toevoegen van OpenID Connect-provider in de ingebouwde beleidsregels in Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e5baf95cd2426c9753620cba7c5a67b4c1672788
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444222"
---
# <a name="azure-active-directory-b2c-add-a-custom-openid-connect-identity-provider-in-built-in-policies"></a>Azure Active Directory B2C: Een aangepaste OpenID Connect id-provider in het ingebouwde beleid toevoegen

>[!NOTE]
> Deze functie is beschikbaar als openbare preview. Gebruik de functie niet in een productieomgeving.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) is een protocol voor verificatie, gebouwd op OAuth 2.0, die kan worden gebruikt om gebruikers veilig aanmelden. De meeste id-providers die gebruikmaken van dit protocol, zoals [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md), worden ondersteund in Azure AD B2C. In dit artikel wordt uitgelegd hoe u aangepaste OpenID Connect id-providers in de ingebouwde beleidsregels kunt toevoegen.

## <a name="configuring-a-custom-openid-connect-identity-provider"></a>Een aangepaste OpenID Connect id-provider configureren

Een aangepaste OpenID Connect id-provider toevoegen:

1. Volg deze stappen om [gaat u naar de Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
1. Klik op **id-Providers**.
1. Klik op **Toevoegen**.
1. Voor de **type id-provider**, selecteer **OpenID Connect**.

### <a name="setting-up-the-openid-connect-identity-provider"></a>Instellen van de id-provider met OpenID Connect

#### <a name="metadata-url"></a>Metagegevens-URL

Volgens de specificatie beschrijft elke identiteitsproviders OpenID verbinding maken met een document met metagegevens die de meeste van de vereiste om uit te voeren aanmelden informatie bevat. Dit omvat gegevens zoals de URL's te gebruiken en de locatie van de openbare ondersteuningssleutels van de service. Het metagegevensdocument voor OpenID Connect is altijd zich op een eindpunt dat eindigt op `.well-known\openid-configuration`.

Voor de OpenID Connect id-provider wilt u toevoegen, voer de metagegevens-URL.

#### <a name="client-id-and-secret"></a>Client-ID en -geheim

Als u wilt toestaan dat gebruikers zich aanmelden, moet de id-provider ontwikkelaars het registreren van een toepassing in hun service. Deze toepassing heeft een ID (aangeduid als de **client-ID**) en een **clientgeheim**. Kopieer deze waarden van de id-provider en de bijbehorende velden aangaan.

> [!NOTE]
> Het clientgeheim is optioneel. U moet echter een clientgeheim invoeren als u wilt gebruiken de [autorisatiecodestroom](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), waarbij de geheime sleutel wordt gebruikt voor het uitwisselen van de code voor het token.

#### <a name="scope"></a>Bereik

Bereiken definiëren de informatie en de machtigingen die u wilt verzamelen van uw aangepaste id-provider. OpenID Connect-aanvragen mag de `openid` waarde scope om te kunnen ontvangen van de ID-token van de id-provider. Zonder de ID-token, dat gebruikers zich niet aanmelden bij Azure AD B2C met behulp van de aangepaste id-provider.

Andere bereiken kunnen worden toegevoegd (gescheiden door een spatie). Raadpleeg de documentatie van de aangepaste id-provider om te zien wat andere scopes mogelijk beschikbaar.

#### <a name="response-type"></a>Reactietype

Het reactietype wordt beschreven wat voor soort informatie wordt verzonden in de eerste aanroep naar de `authorization_endpoint` van de aangepaste id-provider. 

* `code`: Volgens het [autorisatiecodestroom](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), een code naar Azure AD B2C wordt geretourneerd. Azure AD B2C wordt gaat u verder aan te roepen de `token_endpoint` voor het uitwisselen van de code voor het token.
* `token`: Een toegangstoken geretourneerd naar Azure AD B2C van de aangepaste id-provider.
* `id_token`: Een ID-token terug naar Azure AD B2C geretourneerd van de aangepaste id-provider.


#### <a name="response-mode"></a>Antwoordmodus

De antwoordmodus definieert de methode die moet worden gebruikt voor het verzenden van de gegevens van de aangepaste id-provider naar Azure AD B2C.

* `form_post`: In deze antwoordmodus wordt aanbevolen voor optimale beveiliging. Het antwoord wordt verzonden via het HTTP `POST` methode met de code of het token wordt gecodeerd in de hoofdtekst met de `application/x-www-form-urlencoded` indeling.
* `query`: De code of het token wordt geretourneerd als een queryparameter.


#### <a name="domain-hint"></a>Domeinhint

De domeinhint kan worden gebruikt om over te slaan rechtstreeks naar de aanmeldingspagina van de opgegeven id-provider, in plaats van de gebruiker moet een selectie uit de lijst van beschikbare id-providers. Voer een waarde voor de domeinhint zodat dit type gedrag.

Als u wilt gaan met de aangepaste id-provider, de parameter toevoegen `domain_hint=<domain hint value>` aan het einde van uw aanvraag bij het aanroepen van Azure AD B2C voor aanmelden.


### <a name="mapping-the-claims-from-the-openid-connect-identity-provider"></a>Toewijzing van de claims van de identiteitsprovider OpenID Connect

Nadat de identiteit van de aangepaste verzendt provider een ID-token terug naar Azure AD B2C, Azure AD B2C-behoeften te kunnen toewijzen van de claims van de ontvangen-token naar de claims die Azure AD B2C herkent en gebruikt. 

Raadpleeg de documentatie van de aangepaste id-provider om te begrijpen van de claims die worden geretourneerd in de id-provider tokens voor elk van de onderstaande toewijzingen.

* `User ID`: Voer de claim met de unieke id voor de aangemelde gebruiker.
* `Display Name`: Voer de claim waarmee de weergavenaam of de volledige naam voor de gebruiker.
* `Given Name`: Voer de claim waarmee de voornaam van de gebruiker.
* `Surname`: Voer de claim waarmee de achternaam van de gebruiker.
* `Email`: Voer de claim waarmee het e-mailadres van de gebruiker.

## <a name="next-steps"></a>Volgende stappen

De aangepaste OpenID Connect id-provider toevoegen aan uw [ingebouwd beleid](active-directory-b2c-reference-policies.md).
