---
title: Over claim resolvers in aangepaste beleidsregels voor Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over hoe claims resolvers worden gebruikt in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/25/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 44ac4a5fd14d262fdbd1f6fcd36bb2351d08f754
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692439"
---
# <a name="about-claim-resolvers-in-azure-active-directory-b2c-custom-policies"></a>Over claim resolvers in Azure Active Directory B2C aangepast beleid

Claim resolvers in Azure Active Directory (Azure AD) B2C [aangepast beleid](active-directory-b2c-overview-custom.md) bevatten contextinformatie over een autorisatieaanvraag is, zoals de naam van het beleid, aanvraagcorrelatie-ID en de taal van gebruikersinterface.

Voor het gebruik van een claim resolver in een invoer- of -claim, definieert u een tekenreeks **ClaimType**onder de [ClaimsSchema](claimsschema.md) element, en vervolgens stelt de **DefaultValue** op de claim conflictoplosser in de invoer of uitvoer claim element. Azure AD B2C leest de waarde van de claim-omzetter en de waarde in het technische profiel gebruikt. 

In het volgende voorbeeld wordt een claimtype met de naam `correlationId` is gedefinieerd met een **DataType** van `string`.  

```XML
<ClaimType Id="correlationId">
  <DisplayName>correlationId</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Request correlation Id</UserHelpText>
</ClaimType>
```

In het technische profiel en de conflictoplosser claim worden toegewezen aan het claimtype. Azure AD B2C vult de waarde van de claim-resolver `{Context:CorrelationId}` in de claim `correlationId` en stuurt de claim aan het technische profiel.

```XML
<InputClaim ClaimTypeReferenceId="correlationId" DefaultValue="{Context:CorrelationId}" />
```

## <a name="claim-resolver-types"></a>Conflictoplosser claimtypen

De volgende secties worden de beschikbare claim resolvers.

### <a name="culture"></a>Culture

| Claim | Description | Voorbeeld |
| ----- | ----------- | --------|
| {Culture:LanguageName} | ISO-code voor de taal van de twee letters. | nl |
| {Cultuur: LCID}   | De LCID van taal. | 19 |
| {Cultuur: RegionName} | ISO-code voor de regio van de twee letters. | VS |
| {Culture:RFC5646} | De taalcode RFC5646. | en-US |

### <a name="policy"></a>Beleid

| Claim | Description | Voorbeeld |
| ----- | ----------- | --------|
| {Policy:PolicyId} | De relying party beleidsnaam. | B2C_1A_signup_signin |
| {Policy:RelyingPartyTenantId} | De tenant-ID van het beleid voor relying party. | your-tenant.onmicrosoft.com |
| {Policy:TenantObjectId} | De tenant-object-ID van het beleid voor relying party. | 00000000-0000-0000-0000-000000000000 |
| {Policy:TrustFrameworkTenantId} | De tenant-ID van de vertrouwensrelatie-framework. | your-tenant.onmicrosoft.com |

### <a name="openid-connect"></a>OpenID Connect

| Claim | Description | Voorbeeld |
| ----- | ----------- | --------|
| {OIDC:AuthenticationContextReferences} |De `acr_values` query-tekenreeksparameter. | N/A |
| {OIDC:ClientId} |De `client_id` query-tekenreeksparameter. | 00000000-0000-0000-0000-000000000000 |
| {OIDC:DomainHint} |De `domain_hint` query-tekenreeksparameter. | facebook.com |
| {OIDC:LoginHint} |  De `login_hint` query-tekenreeksparameter. | someone@contoso.com |
| {OIDC:MaxAge} | De `max_age`. | N/A |
| {OIDC:Nonce} |De `Nonce` query-tekenreeksparameter. | defaultNonce |
| {OIDC:Prompt} | De `prompt` query-tekenreeksparameter. | aanmelding |
| {OIDC:Resource} |De `resource` query-tekenreeksparameter. | N/A |
| {OIDC:scope} |De `scope` query-tekenreeksparameter. | openid |

### <a name="context"></a>Context

| Claim | Description | Voorbeeld |
| ----- | ----------- | --------|
| {Context:BuildNumber} | De Identiteitservaring-Framework-versie (build-nummer).  | 1.0.507.0 |
| {Context:CorrelationId} | De correlatie-ID.  | 00000000-0000-0000-0000-000000000000 |
| {Context:DateTimeInUtc} |De datum-tijd in UTC.  | 10-10-2018 12:00:00 PM |
| {Context:DeploymentMode} |De implementatiemodus van beleid.  | Productie |
| {Context:IPAddress} | Het IP-adres van de gebruiker. | 11.111.111.11 |


### <a name="non-protocol-parameters"></a>Parameters voor de niet-protocollen

Elke parameternaam opgenomen als onderdeel van een OIDC of OAuth2-aanvraag kan worden toegewezen aan een claim in de gebruikersbeleving. De aanvraag van de toepassing kan bijvoorbeeld een queryreeks-parameter met de naam `app_session`, `loyalty_number`, of een aangepaste query-tekenreeks.

| Claim | Description | Voorbeeld |
| ----- | ----------------------- | --------|
| {OAUTH-KV:campaignId} | Een queryreeks-parameter. | Hawaï |
| {OAUTH-KV:app_session} | Een queryreeks-parameter. | A3C5R |
| {OAUTH-KV:loyalty_number} | Een queryreeks-parameter. | 1234 |
| {OAUTH-KV: een aangepaste query-tekenreeks} | Een queryreeks-parameter. | N/A |

### <a name="oauth2"></a>OAuth2

| Claim | Description | Voorbeeld |
| ----- | ----------------------- | --------|
| {oauth2:access_token} | Het toegangstoken. | N/A |

## <a name="how-to-use-claim-resolvers"></a>Het gebruik van de claim resolvers

### <a name="restful-technical-profile"></a>RESTful technisch profiel

In een [RESTful](restful-technical-profile.md) technisch profiel, kunt u voor het verzenden van de gebruikerstaal, de naam van beleid, bereik en client-ID. Op basis van deze claims de REST-API kunt uitvoeren van aangepaste zakelijke logica, en indien nodig een gelokaliseerde foutbericht verhogen. 

Het volgende voorbeeld ziet u een RESTful technisch profiel:

```XML
<TechnicalProfile Id="REST">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app.azurewebsites.net/api/identity</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userLanguage" DefaultValue="{Culture:LCID}" />
    <InputClaim ClaimTypeReferenceId="policyName" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="scope" DefaultValue="{OIDC:scope}" />
    <InputClaim ClaimTypeReferenceId="clientId" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

### <a name="direct-sign-in"></a>Direct aanmelden

Claim resolvers gebruikt, kunt u de naam of direct aanmelden met een specifieke sociale id-provider, zoals Facebook of LinkedIn, een Microsoft-account vooraf invullen. Zie voor meer informatie, [instellen direct aanmelden met behulp van Azure Active Directory B2C](direct-signin.md).

### <a name="dynamic-ui-customization"></a>Dynamische gebruikersinterface aanpassen

Azure AD B2C kunt u queryreeksparameters doorgeven aan uw HTML-inhoud definition-eindpunten, zodat u kunt de pagina-inhoud dynamisch renderen. Bijvoorbeeld, kunt u de afbeelding op de Azure AD B2C registreren of aanmelden pagina op basis van een aangepaste parameter die u vanuit uw web- of mobiele toepassing doorgeven. Zie voor meer informatie, [dynamische configuratie van de gebruikersinterface met behulp van aangepaste beleidsregels in Azure Active Directory B2C](active-directory-b2c-ui-customization-custom-dynamic.md). U kunt ook uw HTML-pagina op basis van een parameter taal lokaliseren, of kunt u de inhoud op basis van de client-ID.

Het volgende voorbeeld wordt een parameter met de naam doorgegeven in de querytekenreeks **campaignId** met een waarde van `hawaii`, een **taal** gedragscode `en-US`, en **app** vertegenwoordigt de client-ID:

```XML
<UserJourneyBehaviors>
  <ContentDefinitionParameters>
    <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
    <Parameter Name="language">{Culture:RFC5646}</Parameter>
    <Parameter Name="app">{OIDC:ClientId}</Parameter>
  </ContentDefinitionParameters>
</UserJourneyBehaviors>
```

Azure AD B2C wordt als gevolg hiervan de bovenstaande parameters verzendt naar de pagina van de HTML-inhoud:

```
/selfAsserted.aspx?campaignId=hawaii&language=en-US&app=0239a9cc-309c-4d41-87f1-31288feb2e82
```

### <a name="application-insights-technical-profile"></a>Technische Application Insights-profiel

Met Azure Application Insights en claim resolvers krijgt u inzicht in gebruikersgedrag. In het technische profiel van de Application Insights stuurt u invoerclaims die worden doorgevoerd naar Azure Application Insights. Zie voor meer informatie, [gebruikersgedrag bijhouden in Azure AD B2C reizen met behulp van Application Insights](active-directory-b2c-custom-guide-eventlogger-appins.md). Het volgende voorbeeld verzendt de beleids-ID, correlatie-ID, taal en de client-ID naar Azure Application Insights.

```XML
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Alternate Email</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
    <InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
    <InputClaim ClaimTypeReferenceId="AppId" PartnerClaimType="{property:App}" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
</TechnicalProfile>
```
