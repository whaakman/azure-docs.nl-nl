---
title: Instellen van zich kunnen registreren en aanmelden met OpenID verbinding maken met behulp van Azure Active Directory B2C | Microsoft Docs
description: Instellen van zich kunnen registreren en aanmelden met OpenID verbinding maken met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 49f3f80832597b231aec812a4c1613da9897f72a
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722442"
---
# <a name="set-up-sign-up-and-sign-in-with-openid-connect-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met OpenID verbinding maken met behulp van Azure Active Directory B2C

>[!NOTE]
> Deze functie is beschikbaar als openbare preview. Gebruik de functie niet in een productieomgeving.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) is een protocol voor verificatie, gebouwd op OAuth 2.0, die kan worden gebruikt om gebruikers veilig aanmelden. De meeste id-providers die gebruikmaken van dit protocol, zoals [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md), worden ondersteund in Azure AD B2C. In dit artikel wordt uitgelegd hoe u aangepaste OpenID Connect id-providers in uw gebruikersstromen kunt toevoegen.

## <a name="add-the-identity-provider"></a>De id-provider toevoegen

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-Providers**, en klik vervolgens op **toevoegen**.
5. Voor de **type id-provider**, selecteer **OpenID Connect (Preview)**.

## <a name="configure-the-identity-provider"></a>De id-provider configureren

Elke OpenID Connect id-providers beschrijft een document met metagegevens die de meeste van de vereiste om uit te voeren aanmelden informatie bevat. Dit omvat gegevens zoals de URL's te gebruiken en de locatie van de openbare ondersteuningssleutels van de service. Het metagegevensdocument voor OpenID Connect is altijd zich op een eindpunt dat eindigt op `.well-known\openid-configuration`. Voor de OpenID Connect id-provider wilt u toevoegen, voer de metagegevens-URL.

Als u wilt toestaan dat gebruikers zich aanmelden, moet de id-provider ontwikkelaars het registreren van een toepassing in hun service. Deze toepassing heeft een ID die wordt aangeduid als de **client-ID** en een **clientgeheim**. Kopieer deze waarden van de id-provider en de bijbehorende velden aangaan.

> [!NOTE]
> Het clientgeheim is optioneel. U moet echter een clientgeheim invoeren als u wilt gebruiken de [autorisatiecodestroom](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), waarbij de geheime sleutel wordt gebruikt voor het uitwisselen van de code voor het token.

Bereik definieert de informatie en de machtigingen die u wilt verzamelen van uw aangepaste id-provider. OpenID Connect-aanvragen mag de `openid` waarde scope om te kunnen ontvangen van de ID-token van de id-provider. Zonder de ID-token, dat gebruikers zich niet aanmelden bij Azure AD B2C met behulp van de aangepaste id-provider. Andere bereiken kunnen worden toegevoegd aan de gescheiden door een spatie. Raadpleeg de documentatie van de aangepaste id-provider om te zien wat andere scopes mogelijk beschikbaar.

Het reactietype wordt beschreven wat voor soort informatie wordt verzonden in de eerste aanroep naar de `authorization_endpoint` van de aangepaste id-provider. De volgende antwoordtypen kunnen worden gebruikt:

- `code`: Volgens het [autorisatiecodestroom](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), een code naar Azure AD B2C wordt geretourneerd. Azure AD B2C wordt voortgezet om aan te roepen de `token_endpoint` voor het uitwisselen van de code voor het token.
- `token`: Een toegangstoken wordt geretourneerd naar Azure AD B2C vanaf de aangepaste id-provider.
- `id_token`: Een ID-token dat wordt geretourneerd naar Azure AD B2C vanaf de aangepaste id-provider.

De antwoordmodus definieert de methode die moet worden gebruikt voor het verzenden van de gegevens van de aangepaste id-provider naar Azure AD B2C. De volgende modi voor antwoord kunnen worden gebruikt:

- `form_post`: In deze antwoordmodus wordt aanbevolen voor optimale beveiliging. Het antwoord wordt verzonden via het HTTP `POST` methode met de code of het token wordt gecodeerd in de hoofdtekst met de `application/x-www-form-urlencoded` indeling.
- `query`: De code of het token wordt geretourneerd als een queryparameter.

De domeinhint kan worden gebruikt om over te slaan rechtstreeks naar de aanmeldingspagina van de opgegeven id-provider, in plaats van de gebruiker moet een selectie uit de lijst van beschikbare id-providers. Voer een waarde voor de domeinhint zodat dit type gedrag. Als u wilt gaan met de aangepaste id-provider, de parameter toevoegen `domain_hint=<domain hint value>` aan het einde van uw aanvraag bij het aanroepen van Azure AD B2C voor aanmelden.

Nadat de identiteit van de aangepaste verzendt provider een ID-token terug naar Azure AD B2C, Azure AD B2C-behoeften te kunnen toewijzen van de claims van de ontvangen-token naar de claims die Azure AD B2C herkent en gebruikt. Raadpleeg de documentatie van de aangepaste id-provider om te begrijpen van de claims die worden geretourneerd in de id-provider tokens voor elk van de volgende toewijzingen:

- `User ID`: Voer de claim met de unieke id voor de aangemelde gebruiker.
- `Display Name`: Voer de claim waarmee de weergavenaam of de volledige naam voor de gebruiker.
- `Given Name`: Voer de claim waarmee de voornaam van de gebruiker.
- `Surname`: Voer de claim waarmee de achternaam van de gebruiker.
- `Email`: Voer de claim waarmee het e-mailadres van de gebruiker.

