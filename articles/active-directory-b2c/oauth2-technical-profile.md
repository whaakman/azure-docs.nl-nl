---
title: Een technisch profiel OAuth2 definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Een technisch profiel OAuth2 definiëren in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: cc09998a982f319cfc1d60924d2175a3c17c8465
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836676"
---
# <a name="define-a-oauth2-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een technisch profiel OAuth2 definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C biedt ondersteuning voor de id-provider van de OAuth2-protocol. Dit is de primaire protocol voor de gedelegeerde verificatie en autorisatie. Zie voor meer informatie de [RFC 6749 de OAuth 2.0 machtiging Framework](https://tools.ietf.org/html/rfc6749). U kunt federeren met een OAuth2-technische profiel met OAuth2 op basis van id-provider, zoals Facebook en Live.com, zodat u kunt gebruikers om aan te melden met hun bestaande sociale of ondernemings-id's.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `OAuth2`. Bijvoorbeeld, het protocol voor de **Facebook-OAUTH** technische profiel is `OAuth2`:

```XML
<TechnicalProfile Id="Facebook-OAUTH">
  <DisplayName>Facebook</DisplayName>
  <Protocol Name="OAuth2" />
  ...    
```

## <a name="input-claims"></a>Invoerclaims

De **InputClaims** en **InputClaimsTransformations** elementen zijn niet vereist. Maar mogelijk wilt u extra parameters verzenden naar uw id-provider. Het volgende voorbeeld wordt de **domain_hint** query-tekenreeksparameter met de waarde van `contoso.com` de autorisatie-aanvraag.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Gebruikersclaims

De **OutputClaims** element bevat een lijst met claims die wordt geretourneerd door de OAuth2-id-provider. Mogelijk moet u de naam van de claim die is gedefinieerd in uw beleid aan de naam die is gedefinieerd in de id-provider toewijzen. U kunt ook bevatten claims die niet zijn geretourneerd door de id-provider, zolang u de `DefaultValue` kenmerk.

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.

Het volgende voorbeeld ziet u de claims die wordt geretourneerd door de Facebook-id-provider:

- De **first_name** claim is toegewezen aan de **givenName** claim.
- De **achternaam** claim is toegewezen aan de **achternaam** claim.
- De **displayName** claim zonder Gebruikersnaamtoewijzing...
- De **e** claim zonder naam toewijzingen.

Het technische profiel retourneert ook claims die niet zijn geretourneerd door de id-provider: 

- De **identityProvider** claim met de naam van de id-provider.
- De **authenticationSource** claim met een standaardwaarde van **socialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="facebook.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| client_id | Ja | De toepassings-id van de id-provider. |
| IdTokenAudience | Nee | De doelgroep van het id_token. Als u opgeeft, wordt Azure AD B2C controleert of het token in een claim die wordt geretourneerd door de id-provider is en gelijk aan de versie die is opgegeven is. |
| authorization_endpoint | Ja | De URL van de autorisatie-eindpunt aan de hand van RFC 6749. |
| AccessTokenEndpoint | Ja | De URL van het token-eindpunt aan de hand van RFC 6749. |  
| ClaimsEndpoint | Ja | De URL van het eindpunt van de gebruiker informatie aan de hand van RFC 6749. | 
| AccessTokenResponseFormat | Nee | De indeling van de aanroep van de access-token-eindpunt. Bijvoorbeeld, Facebook is een HTTP GET-methode vereist, maar de reactie van access-token is in JSON-indeling. |
| AdditionalRequestQueryParameters | Nee | Aanvullende queryparameters. U wilt bijvoorbeeld aanvullende parameters verzenden naar uw id-provider. U kunt meerdere parameters met door komma's scheidingsteken opnemen. | 
| ClaimsEndpointAccessTokenName | Nee | De naam van de access token queryreeks-parameter. Sommige id-providers claims eindpunten ondersteuning voor GET HTTP-aanvraag. In dit geval wordt het bearer-token verzonden met behulp van een queryreeks-parameter in plaats van de autorisatie-header. |
| ClaimsEndpointFormatName | Nee | De naam van de indeling queryreeks-parameter. U kunt bijvoorbeeld de naam als instellen `format` in deze LinkedIn claims eindpunt `https://api.linkedin.com/v1/people/~?format=json`. | 
| ClaimsEndpointFormat | Nee | De waarde van de indeling queryreeks-parameter. U kunt bijvoorbeeld de waarde als instellen `json` in deze LinkedIn claims eindpunt `https://api.linkedin.com/v1/people/~?format=json`. | 
| ProviderName | Nee | De naam van de id-provider. |
| response_mode | Nee | De methode die de id-provider gebruikt voor het verzenden van het resultaat terug naar Azure AD B2C. Mogelijke waarden: `query`, `form_post` (standaard), of `fragment`. |
| scope | Nee | Het bereik van de aanvraag voor toegang op basis van de OAuth2-specificatie identity provider gedefinieerd. Zoals `openid`, `profile`, en `email`. |
| HttpBinding | Nee | De verwachte HTTP-binding met de toegang tot tokens en claims token-eindpunten. Mogelijke waarden: `GET` of `POST`.  |
| ResponseErrorCodeParamName | Nee | De naam van de parameter die het foutbericht geretourneerd via een HTTP 200 (Ok bevat). |
| ExtraParamsInAccessTokenEndpointResponse | Nee | Bevat de extra parameters die kunnen worden geretourneerd in het antwoord van **AccessTokenEndpoint** door sommige id-providers. Bijvoorbeeld, het antwoord van **AccessTokenEndpoint** bevat, zoals een extra parameter `openid`, dit is een verplichte parameter naast de access_token in een **ClaimsEndpoint** aanvraag query tekenreeks. Meerdere namen van parameters moeten worden voorafgegaan en door de door komma's gescheiden ',' scheidingsteken. |
| ExtraParamsInClaimsEndpointRequest | Nee | Bevat de extra parameters die kunnen worden geretourneerd in de **ClaimsEndpoint** aanvraag door een id-providers. Meerdere namen van parameters moeten worden voorafgegaan en door de door komma's gescheiden ',' scheidingsteken. |

## <a name="cryptographic-keys"></a>Cryptografische sleutels

De **CryptographicKeys** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| client_secret | Ja | Het clientgeheim van de toepassing van id-provider. De cryptografische sleutel is alleen vereist als de **response_types** metagegevens is ingesteld op `code`. In dit geval wordt Azure AD B2C een aanroep voor het uitwisselen van de autorisatiecode voor een toegangstoken. Als de metagegevens is ingesteld op `id_token` kunt u de cryptografische sleutel weglaten.  |  

## <a name="redirect-uri"></a>Omleidings-URI

Wanneer u de omleidings-URL van uw id-provider configureren, voert u `https://login.microsoftonline.com/te/tenant/policyId/oauth2/authresp`. Vervang **tenant** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) en **policyId** met de id van uw beleid (bijvoorbeeld b2c_1a_policy). De omleidings-URI moet zich in alleen kleine letters.

Als u de **b2clogin.com** domein in plaats van **login.microsoftonline.com** Zorg ervoor dat u b2clogin.com in plaats van login.microsoftonline.com.

Voorbeelden:

- [Google + als een OAuth2-id-provider met behulp van aangepaste beleid toevoegen](active-directory-b2c-custom-setup-goog-idp.md)













