---
title: RelyingParty - Azure Active Directory B2C | Microsoft Docs
description: Geef het element RelyingParty van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: aaae119ec72a713adc2faa311dbcb6bd204035fd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52835085"
---
# <a name="relyingparty"></a>RelyingParty

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De **RelyingParty** element Hiermee geeft u de gebruikersbeleving om af te dwingen voor de huidige aanvraag voor Azure Active Directory (Azure AD) B2C. U geeft u ook de lijst met claims die de relying party (RP)-toepassing moet als onderdeel van de uitgegeven tokens. Een RP-toepassing, zoals een web-, mobiele of bureaubladtoepassingen-toepassing, roept het beleidsbestand RP. De RP-beleidsbestand voert een specifieke taak, zoals het aanmelden, een wachtwoord in te stellen of bewerken van een profiel. Meerdere toepassingen kunnen gebruikmaken van hetzelfde RP-beleid en één toepassing meerdere beleidsregels kunt gebruiken. Alle RP-toepassingen hetzelfde token met claims ontvangt en de gebruiker de dezelfde gebruikersbeleving doorloopt.

Het volgende voorbeeld wordt een **RelyingParty** -element in de *B2C_1A_signup_signin* beleidsbestand:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="your-tenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://your-tenant.onmicrosoft.com/B2C_1A_signup_signin">

  <BasePolicy>
    <TenantId>your-tenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>

  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
      <SingleSignOn Scope="TrustFramework" />
      <SessionExpiryType>Rolling</SessionExpiryType>
      <SessionExpiryInSeconds>300</SessionExpiryInSeconds>
      <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="your-application-insights-key" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
      </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Description>The policy profile</Description> 
      <Protocol Name="OpenIdConnect" />
      <Metadata>collection of key/value pairs of data</Metadata>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ...
```

De optionele **RelyingParty** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| DefaultUserJourney | 1:1 | De gebruikersbeleving standaard voor de RP-toepassing. |
| UserJourneyBehaviors | 0:1 | Het bereik van de reis gebruikersgedrag. |
| Technische profiel | 1:1 | Een technisch profiel dat wordt ondersteund door de RP-toepassing. Het technische profiel biedt een contract voor de toepassing RP contact opnemen met Azure AD B2C. |

## <a name="defaultuserjourney"></a>DefaultUserJourney

De `DefaultUserJourney` element Hiermee geeft u een verwijzing naar de id van de gebruikersbeleving die gewoonlijk wordt gedefinieerd in het beleid voor basis- of -extensies. De volgende voorbeelden ziet u de registratie- of aanmelden gebruikersbeleving opgegeven in de **RelyingParty** element:

*B2C_1A_signup_signin* beleid:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn">
  ...
```

*B2C_1A_TrustFrameWorkBase* of *B2C_1A_TrustFrameworkExtensionPolicy*:

```XML
<UserJourneys>
  <UserJourney Id="SignUpOrSignIn">
  ...
```

De **DefaultUserJourney** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| referenceId | Ja | Een id van de gebruikersbeleving in het beleid. Zie voor meer informatie, [gebruiker reizen](userjourneys.md) |

## <a name="userjourneybehaviors"></a>UserJourneyBehaviors

De **UserJourneyBehaviors** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| SingleSignOn | 0:1 | Het bereik van de eenmalige aanmelding (SSO) sessiegedrag van een gebruikersbeleving. |
| SessionExpiryType |0:1 | Het verificatiegedrag van de sessie. Mogelijke waarden: `Rolling` of `Absolute`. De `Rolling` waarde (standaard) geeft aan dat de gebruiker aangemeld blijft zolang de gebruiker voortdurend actief zijn in de toepassing is. De `Absolute` waarde geeft aan dat de gebruiker wordt gedwongen om te verifiëren na de periode die is opgegeven door de toepassingssessie levensduur. |
| SessionExpiryInSeconds | 0:1 | De levensduur van Azure AD B2C-sessiecookie die is opgegeven als een geheel getal zijn opgeslagen in de browser van de gebruiker bij een geslaagde verificatie. |
| JourneyInsights | 0:1 | De Azure Application Insights-instrumentatiesleutel moet worden gebruikt. |
| ContentDefinitionParameters | 0:1 | De lijst met sleutel-waardeparen kunnen worden toegevoegd aan de inhoudsdefinitie laden URI. |

### <a name="singlesignon"></a>SingleSignOn

De **SingleSignOn** element in het volgende kenmerk bevat:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Bereik | Ja | Het bereik van het gedrag van eenmalige aanmelding. Mogelijke waarden: `Suppressed`, `Tenant`, `Application`, of `Policy`. De `Suppressed` waarde geeft aan dat het probleem wordt onderdrukt. Bijvoorbeeld geen sessie wordt bijgehouden voor de gebruiker in het geval van een sessie voor eenmalige aanmelding en de gebruiker altijd een prompt wordt voor de selectie van een id-provider. De `TrustFramework` waarde geeft aan dat het gedrag voor alle beleidsregels in het kader van de vertrouwensrelatie wordt toegepast. Bijvoorbeeld, navigeren door twee trajecten van beleid voor het kader van een vertrouwensrelatie van een gebruiker niet gevraagd voor de selectie van een id-provider. De `Tenant` waarde geeft aan dat het probleem wordt toegepast op alle beleidsregels in de tenant. Bijvoorbeeld, navigeren door twee trajecten van beleid voor een tenant van een gebruiker niet gevraagd voor de selectie van een id-provider. De `Application` waarde geeft aan dat het probleem wordt toegepast op alle beleidsregels voor de toepassing die de aanvraag. Bijvoorbeeld, navigeren door twee beleid trajecten voor een toepassing van een gebruiker niet gevraagd voor de selectie van een id-provider. De `Policy` waarde geeft aan dat het gedrag is alleen van toepassing op een beleid. Bijvoorbeeld, wordt navigeren door twee trajecten van beleid voor het kader van een vertrouwensrelatie van een gebruiker gevraagd een id-provider selecteren bij het schakelen tussen beleid. |

## <a name="journeyinsights"></a>JourneyInsights

De **JourneyInsights** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| TelemetryEngine | Ja | De waarde moet `ApplicationInsights`. | 
| InstrumentationKey | Ja | De tekenreeks is die de instrumentatiesleutel voor de application insights-element bevat. |
| DeveloperMode | Ja | Mogelijke waarden: `true` of `false`. Als `true`, Application Insights versnelt de telemetrie via de verwerkings-pipeline. Deze instelling geldt voor de ontwikkeling van, maar beperkt bij hoge volumes de gedetailleerde activiteitenlogboeken zijn alleen ontworpen om te helpen bij de ontwikkeling van aangepaste beleidsregels. Gebruik geen Ontwikkelingsmodus in productie. Logboeken verzamelen van alle claims verzonden naar en van de id-providers tijdens de ontwikkeling. Als in de productieomgeving gebruikt, verantwoordelijkheid de ontwikkelaar van de krijgt voor PII (privé Identifiable Information) die in het logboek voor App Insights waarvan ze eigenaar zijn verzameld. Deze gedetailleerde logboeken worden alleen verzameld wanneer deze waarde is ingesteld op `true`.|
| ClientEnabled | Ja | Mogelijke waarden: `true` of `false`. Als `true`, verzendt de client-side Application Insights-script voor het bijhouden van pagina bekijken en client-side '-fouten. | 
| ServerEnabled | Ja | Mogelijke waarden: `true` of `false`. Als `true`, wordt de bestaande UserJourneyRecorder JSON als een aangepaste gebeurtenis verzonden naar Application Insights. | 
| TelemetryVersion | Ja | De waarde moet `1.0.0`. | 

Zie voor meer informatie, [verzamelen van Logboeken](active-directory-b2c-troubleshoot-custom.md)

## <a name="contentdefinitionparameters"></a>ContentDefinitionParameters

Met behulp van aangepaste beleidsregels in Azure AD B2C, kunt u een parameter verzenden in een queryreeks. Door de parameter door te geven aan uw HTML-eindpunt, kunt u de pagina-inhoud dynamisch wijzigen. U kunt bijvoorbeeld de achtergrondafbeelding op de registratie- of aanmeldingspagina van Azure AD B2C wijzigen, op basis van een parameter die u doorgeeft vanuit uw web- of mobiele toepassing. Azure AD B2C wordt de query tekenreeksparameters doorgegeven aan uw dynamische HTML-bestand, zoals een ASPX-bestand. 

Het volgende voorbeeld wordt een parameter met de naam doorgegeven `campaignId` met een waarde van `hawaii` in de query-tekenreeks:

`https://login.microsoft.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?pB2C_1A_signup_signin&client_id=a415078a-0402-4ce3-a9c6-ec1947fcfb3f&nonce=defaultNonce&redirect_uri=http%3A%2F%2Fjwt.io%2F&scope=openid&response_type=id_token&prompt=login&campaignId=hawaii`

De **ContentDefinitionParameters** element bevat het volgende element:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| ContentDefinitionParameter | 0: n | Een tekenreeks zijn met de sleutel-waardepaar dat wordt toegevoegd aan de querytekenreeks van een inhoudsdefinitie laden URI. |

De **ContentDefinitionParameter** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Naam | Ja | De naam van de sleutel-waardepaar. |

Zie voor meer informatie, [configureren van de gebruikersinterface met dynamische inhoud met behulp van aangepaste beleidsregels](active-directory-b2c-ui-customization-custom-dynamic.md)

## <a name="technicalprofile"></a>Technische profiel

De **TechnicalProfile** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- | 
| Id | Ja | De waarde moet `PolicyProfile`. |

De **TechnicalProfile** bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| Weergavenaam | 0:1 | De tekenreeks zijn met de naam van het technische profiel dat wordt weergegeven aan gebruikers. |
| Beschrijving | 0:1 | De tekenreeks zijn met de beschrijving van het technische profiel dat wordt weergegeven aan gebruikers. |
| Protocol | 1:1 | Het protocol dat wordt gebruikt voor de Federatie. |
| Metagegevens | 0:1 | De verzameling van *Item* van sleutel/waarde-paren die door het protocol voor communicatie met het eindpunt in de loop van een transactie worden gebruikt voor het configureren van interactie tussen de relying party's en andere deelnemers aan de community. |
| OutputClaims | 0:1 | Een lijst met claimtypen die worden uitgevoerd als uitvoer in het technische profiel. Elk van deze elementen bevat een verwijzing naar een **ClaimType** al gedefinieerd in de **ClaimsSchema** sectie of in een beleid dat dit beleidsbestand overneemt. |
| SubjectNamingInfo | 0:1 | De onderwerpnaam in tokens gebruikt. |

De **Protocol** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Naam | Ja | De naam van een geldig protocol dat wordt ondersteund door Azure AD B2C die wordt gebruikt als onderdeel van het technische profiel. Mogelijke waarden: `OpenIdConnect` of `SAML2`. De `OpenIdConnect` waarde vertegenwoordigt de OpenID Connect 1.0-protocol standaard aan de hand van OpenID foundation-specificatie. De `SAML2` vertegenwoordigt de standaard SAML 2.0-protocol aan de hand van OASIS-specificatie. Gebruik een SAML-token niet in de productieomgeving. |

## <a name="outputclaims"></a>OutputClaims

De **OutputClaims** element bevat het volgende element:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| outputClaim | 0: n | De naam van een verwachte claimtype in de lijst met ondersteunde voor het beleid waarnaar de relying party is geabonneerd. Deze claim fungeert als een uitvoer op voor het technische profiel. |

De **OutputClaim** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Ja | Een verwijzing naar een **ClaimType** al gedefinieerd in de **ClaimsSchema** sectie in het beleid-bestand. |
| Standaardwaarde | Nee | Een standaardwaarde die kan worden gebruikt als de claimwaarde leeg is. |
| PartnerClaimType | Nee | Verzendt de claim in een andere naam zoals geconfigureerd in het definitie ClaimType. |

### <a name="subjectnaminginfo"></a>SubjectNamingInfo

Met de **SubjectNameingInfo** -element, u de waarde van het onderwerp van de token beheren:
- **JTW token** : de `sub` claim. Dit is een principal waarover het token worden bevestigd met gegevens, zoals de gebruiker van een toepassing. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Het kan worden gebruikt om uit te voeren van veilige autorisatie controles, zoals wanneer het token wordt gebruikt voor toegang tot een resource. Standaard wordt de claim onderwerp gevuld met de object-ID van de gebruiker in de map. Zie voor meer informatie, [Token-, sessie en configuratie voor eenmalige aanmelding](active-directory-b2c-token-session-sso.md).
- **SAML-token** : de `<Subject><NameID>` element waarmee het onderwerp element uniek wordt geïdentificeerd.

De **SubjectNamingInfo** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| ClaimType | Ja | Een verwijzing naar een uitvoerclaim **PartnerClaimType**. De uitvoer claims moeten worden gedefinieerd in het beleid voor relying party **OutputClaims** verzameling. |

Het volgende voorbeeld ziet hoe u definieert een OpenId Connect relying party. De gegevens van de naam van onderwerp is geconfigureerd als de `objectId`:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```
De JWT-token bevat de `sub` claim met de object-id voor de gebruiker:

```JSON
{
  ...
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  ...
}
```


