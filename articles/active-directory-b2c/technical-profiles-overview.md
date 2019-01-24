---
title: Over technische profielen in het aangepaste beleid van Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over hoe technische profielen worden gebruikt in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 177e44a53f9976879e15e24313c1d8479cb5a6a7
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853218"
---
# <a name="about-technical-profiles-in-azure-active-directory-b2c-custom-policies"></a>Over technische profielen in Azure Active Directory B2C aangepast beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Een technisch profiel biedt een raamwerk met een ingebouwd mechanisme om te communiceren met een ander type met een aangepast beleid in Azure Active Directory (Azure AD) B2C partijen. Technische profielen worden gebruikt om te communiceren met uw Azure AD B2C-tenant, om te maken van een gebruiker of een gebruikersprofiel lezen. Een technisch profiel kan zelf-gecontroleerde om in te schakelen van interactie met de gebruiker zijn. Bijvoorbeeld, verzamelen van de gebruiker referenties aan te melden bij en vervolgens de pagina voor het registreren of de pagina voor het opnieuw instellen van wachtwoorden weer te geven. 

## <a name="type-of-technical-profiles"></a>Type van de technische profielen

Een technisch profiel kunt dit soort scenario's:

- [Azure Active Directory](active-directory-technical-profile.md) -biedt ondersteuning voor het beheer van de Azure Active Directory B2C-gebruikers.
- [Uitgever van het token JWT](jwt-issuer-technical-profile.md) -verzendt een JWT-token dat wordt geretourneerd naar de relying party-toepassing. 
- **Phone factor provider** -multi-factor authentication.
- [OAuth1](oauth1-technical-profile.md) -Federatie met een identiteitsprovider voor OAuth 1.0-protocol.
- [OAuth2](oauth2-technical-profile.md) -Federatie met de id-provider van een OAuth 2.0-protocol.
- [OpenIdConnect](openid-connect-technical-profile.md) -Federatie met een identiteitsprovider OpenId Connect-protocol.
- [Claims transformatie](claims-transformation-technical-profile.md) - aanroep uitvoer claimtransformaties te manipuleren claims waarden, valideren van claims of standaardwaarden instellen voor een set uitvoerclaims.
- [Restful-provider](restful-technical-profile.md) -oproepen naar REST-API-services, zoals het valideren van de invoer van de gebruiker, gebruikersgegevens duidelijker maken of integreren in line-of-business-toepassingen.
- [SAML2](saml-technical-profile.md) -Federatie met de id-provider van een SAML-protocol.
- [Zelf een door de bevestigde](self-asserted-technical-profile.md) -communiceren met de gebruiker. Bijvoorbeeld verzamelen referentie op die van de gebruiker moet zich aanmelden, weergegeven van de pagina voor het registreren of wachtwoord opnieuw instellen.
- **WsFed** -Federatie met eventuele WsFed protocol id-provider. 
- [Sessiebeheer](active-directory-b2c-reference-sso-custom.md) -verschillende soorten sessies worden verwerkt. 
- **Application insights**

## <a name="technical-profile-flow"></a>Technisch profiel stroom

Alle typen technische profielen delen hetzelfde concept. U invoer claims verzenden, claimtransformatie uitvoeren en communiceren met de geconfigureerde partij, zoals een id-provider, REST-API of Azure AD directory-services. Nadat het proces is voltooid, wordt het technische profiel retourneert de uitvoerclaims en uitvoer claimtransformatie kan worden uitgevoerd. Het volgende diagram toont hoe de transformaties en toewijzingen waarnaar wordt verwezen in het technische profiel worden verwerkt. Ongeacht de partij die het technische profiel communiceert met, na claims transformatie wordt uitgevoerd, worden de uitvoerclaims van het technische profiel onmiddellijk opgeslagen in de eigenschappenverzameling claims. 

![Technisch profiel stroom](./media/technical-profiles-overview/technical-profile-idp-saml-flow.png)
 
1. **InputClaimsTransformation** -claims van elke invoer invoer [transformatie claims](claimstransformations.md) zijn opgehaald uit de eigenschappenverzameling vorderingen en na de uitvoering, de uitvoer claims worden teruggeplaatst in de eigenschappenverzameling claims. De uitvoerclaims van een invoer claimtransformatie kunnen invoerclaims van een transformatie van de volgende invoerclaims zijn.
2. **InputClaims** - Claims worden opgehaald uit de eigenschappenverzameling claims en worden gebruikt voor het technische profiel. Bijvoorbeeld, een [door technisch profiel zelf de bevestigde](self-asserted-technical-profile.md) maakt gebruik van de invoer claims om in te vullen van de uitvoerclaims waarmee de gebruiker. Het technische profiel van een REST-API maakt gebruik van de invoerclaims invoerparameters verzenden naar de REST API-eindpunt. Azure Active Directory maakt gebruik van invoer claim als een unieke id te lezen, bijwerken of verwijderen van een account.
3. **De uitvoering van het technische profiel** -het technische profiel uitwisselt de claims met de geconfigureerde partij. Bijvoorbeeld:
    - De gebruiker omgeleid naar de id-provider om de aanmelding in te voeren. Na geslaagde aanmelding, terug door de gebruiker wordt geretourneerd en de uitvoering van het technische profiel wordt voortgezet.
    - Een REST-API aanroepen tijdens het verzenden van parameters als InputClaims en het ophalen van gegevens weer als OutputClaims.
    - Maken of bijwerken van het gebruikersaccount.
    - Verzendt en verifieert u de MFA-SMS-bericht.
4. **ValidationTechnicalProfiles** : voor een [bevestigde technisch profiel](self-asserted-technical-profile.md), kunt u een invoer aanroepen [validatie technisch profiel](validation-technical-profile.md). Het technische validatieprofiel valideert u de gegevens die door de gebruiker geprofileerd en retourneert een foutbericht of klik op Ok, met of zonder uitvoerclaims. Bijvoorbeeld, voordat Azure AD B2C een nieuw account maakt, wordt gecontroleerd of de gebruiker al in de directoryservices bestaat. U kunt een technisch profiel van de REST-API als u wilt toevoegen van uw eigen bedrijfslogica aanroepen.<p>Het bereik van de uitvoerclaims van een technische validatie-profiel is beperkt tot het technische profiel die het technische validatieprofiel aanroept en andere technische validatie-profielen onder hetzelfde technisch profiel. Als u gebruikmaken van de uitvoerclaims in de volgende indelingsstap wilt, die u wilt toevoegen van de uitvoerclaims naar het technische profiel die het technische validatieprofiel aanroept.
5. **OutputClaims** -Claims keert terug naar de eigenschappenverzameling claims. U kunt deze claims in de volgende indelingen stap, of de uitvoer claimtransformaties.
6. **OutputClaimsTransformations** -claims van elke uitvoer invoer [transformatie claims](claimstransformations.md) zijn opgehaald uit de eigenschappenverzameling claims. De uitvoerclaims van het technische profiel van de vorige stappen is invoerclaims van een uitvoer claimtransformatie. Na de uitvoering, worden de uitvoerclaims teruggeplaatst in de eigenschappenverzameling claims. De uitvoerclaims van een uitvoer-claimtransformatie kunnen ook worden invoerclaims van een latere uitvoer claimtransformatie.
7. **Eenmalige aanmelding (SSO) sessiebeheer** - [SSO sessiebeheer](active-directory-b2c-reference-sso-custom.md) interactie met een gebruiker beheert nadat de gebruiker is al geverifieerd. De beheerder kan bijvoorbeeld bepalen of de selectie van de id-providers wordt weergegeven, of dat lokale accountdetails moeten opnieuw worden ingevoerd.

Een technisch profiel kan overnemen van een ander technisch profiel instellingen wijzigen of toevoegen van nieuwe functionaliteit.  De **IncludeTechnicalProfile** element is een verwijzing naar de technische basisprofiel waarvan een technisch profiel is afgeleid.  

Bijvoorbeeld, de **AAD-UserReadUsingAlternativeSecurityId-NoError** technische profiel bevat de **AAD-UserReadUsingAlternativeSecurityId**. Hiermee stelt u in dit technisch profiel de **RaiseErrorIfClaimsPrincipalDoesNotExist** metagegevensitem naar `true`, en wordt een fout gegenereerd als een sociaal account niet in de map bestaat. **AAD-UserReadUsingAlternativeSecurityId-NoError** overschrijft dit gedrag en het foutbericht wordt uitgeschakeld als de gebruiker niet al bestond.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">false</Item>
  </Metadata>
  <IncludeTechnicalProfile ReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
</TechnicalProfile>
``` 

**AAD-UserReadUsingAlternativeSecurityId** bevat de `AAD-Common` technisch profiel.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">User does not exist. Please sign up before you can sign in.</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

Beide **AAD-UserReadUsingAlternativeSecurityId-NoError** en **AAD-UserReadUsingAlternativeSecurityId** geeft u de vereiste **Protocol** element omdat Dit opgegeven in de **AAD-gemeenschappelijke** technisch profiel.

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
</TechnicalProfile>
```

Een technisch profiel mag bevatten of overnemen van een ander technisch profiel, waaronder een andere naam. Er is geen limiet voor het aantal niveaus. Afhankelijk van de zakelijke vereisten, de gebruikersbeleving vragen mogelijk **AAD-UserReadUsingAlternativeSecurityId** die wordt een fout gegenereerd als een sociaal account van gebruiker niet bestaat, of  **AAD-UserReadUsingAlternativeSecurityId-NoError** die een fout niet verhogen.











