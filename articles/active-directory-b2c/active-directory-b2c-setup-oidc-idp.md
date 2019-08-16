---
title: Registratie instellen en aanmelden met OpenID Connect Connect-Azure Active Directory B2C
description: Stel registratie in en meld u aan met OpenID Connect Connect met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 720deb28ce124af23035337ac88cfb1d37fc7c53
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69509692"
---
# <a name="set-up-sign-up-and-sign-in-with-openid-connect-using-azure-active-directory-b2c"></a>Registratie instellen en aanmelden met OpenID Connect Connect met behulp van Azure Active Directory B2C

[OpenID Connect Connect](active-directory-b2c-reference-oidc.md) is een verificatie protocol dat is gebaseerd op OAuth 2,0 en dat kan worden gebruikt voor het aanmelden van beveiligde gebruikers. De meeste id-providers die gebruikmaken van dit protocol worden ondersteund in Azure AD B2C. In dit artikel wordt uitgelegd hoe u aangepaste OpenID Connect Connect-id-providers kunt toevoegen aan uw gebruikers stromen.

## <a name="add-the-identity-provider"></a>De ID-provider toevoegen

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**en klik vervolgens op **toevoegen**.
5. Selecteer **OpenID Connect Connect**voor het **type ID-provider**.

## <a name="configure-the-identity-provider"></a>De ID-provider configureren

Elke OpenID Connect Connect-id-providers beschrijft een meta gegevens document dat de meeste informatie bevat die nodig is voor het uitvoeren van de aanmelding. Dit omvat informatie zoals de Url's die moeten worden gebruikt en de locatie van de open bare handtekeningen sleutels van de service. Het OpenID Connect Connect meta data-document bevindt zich altijd op een `.well-known\openid-configuration`eind punt dat eindigt op. Voer de meta gegevens-URL in voor de OpenID Connect Connect-ID-provider die u wilt toevoegen.

Om gebruikers toe te staan zich aan te melden, vereist de ID-provider ontwikkel aars om een toepassing in hun service te registreren. Deze toepassing heeft een ID die de **client-id** en een **client geheim**wordt genoemd. Kopieer deze waarden van de ID-provider en geef ze op in de bijbehorende velden.

> [!NOTE]
> Het client geheim is optioneel. U moet echter een client geheim invoeren als u de [autorisatie code stroom](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)wilt gebruiken, die het geheim gebruikt voor het uitwisselen van de code voor het token.

Met het bereik worden de informatie en machtigingen gedefinieerd die u wilt verzamelen uit uw aangepaste ID-provider. OpenID Connect Connect-aanvragen moeten de `openid` bereik waarde bevatten om het id-token van de ID-provider te ontvangen. Zonder het ID-token kunnen gebruikers zich niet aanmelden bij Azure AD B2C met behulp van de aangepaste ID-provider. Andere bereiken kunnen worden toegevoegd, gescheiden door een spatie. Raadpleeg de documentatie van de aangepaste ID-provider om te zien welke andere bereiken mogelijk beschikbaar zijn.

Het antwoord type beschrijft wat voor soort informatie wordt weer gegeven in de eerste aanroep `authorization_endpoint` van de aangepaste ID-provider. De volgende antwoord typen kunnen worden gebruikt:

- `code`: Volgens de [autorisatie code stroom](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)wordt een code geretourneerd naar Azure AD B2C. Azure AD B2C opbrengsten om de code `token_endpoint` aan te roepen voor het token.
- `token`: Er wordt een toegangs token geretourneerd naar Azure AD B2C van de aangepaste ID-provider.
- `id_token`: Er wordt een ID-token geretourneerd naar Azure AD B2C van de aangepaste ID-provider.

De antwoord modus definieert de methode die moet worden gebruikt om de gegevens terug van de aangepaste ID-provider naar Azure AD B2C te verzenden. De volgende antwoord modi kunnen worden gebruikt:

- `form_post`: Deze antwoord modus wordt aanbevolen voor de beste beveiliging. Het antwoord wordt verzonden via de http `POST` -methode, waarbij de code of het token wordt gecodeerd in de hoofd `application/x-www-form-urlencoded` tekst met de-indeling.
- `query`: De code of het token wordt geretourneerd als een query parameter.

De domein hint kan worden gebruikt om rechtstreeks over te slaan op de aanmeldings pagina van de opgegeven id-provider, in plaats van dat de gebruiker een selectie maakt tussen de lijst met beschik bare id-providers. Als u dit gedrag wilt toestaan, voert u een waarde in voor de domein hint. Als u naar de aangepaste ID-provider wilt gaan, `domain_hint=<domain hint value>` voegt u de para meter toe aan het einde van uw aanvraag wanneer u Azure AD B2C aanroept om u aan te melden.

Wanneer de aangepaste ID-provider een ID-token terugstuurt naar Azure AD B2C, moet Azure AD B2C de claims van het ontvangen token kunnen toewijzen aan de claims die Azure AD B2C herkent en gebruikt. Raadpleeg de documentatie van de Custom ID-provider voor elk van de volgende toewijzingen om inzicht te krijgen in de claims die terug worden geretourneerd in de tokens van de identiteits provider:

- `User ID`: Voer de claim in die de unieke id voor de aangemelde gebruiker levert.
- `Display Name`: Voer de claim in die de weergave naam of volledige naam voor de gebruiker levert.
- `Given Name`: Voer de claim in die de voor naam van de gebruiker levert.
- `Surname`: Voer de claim in die de achternaam van de gebruiker levert.
- `Email`: Voer de claim in die het e-mail adres van de gebruiker levert.

