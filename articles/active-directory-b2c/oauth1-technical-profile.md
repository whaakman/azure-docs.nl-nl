---
title: Een technisch profiel OAuth1 definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Een technisch profiel OAuth1 definiëren in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6e93f42540492d138afef3a1d4b826165201b04c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54851348"
---
# <a name="define-a-oauth1-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een technisch profiel OAuth1 definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C biedt ondersteuning voor de [OAuth 1.0-protocol](https://tools.ietf.org/html/rfc5849) id-provider. Dit artikel beschrijft de details van een technisch profiel voor interactie met een claimprovider die ondersteuning biedt voor dit gestandaardiseerde protocol. Technisch profiel dat u met een OAuth1 kan federeren met OAuth1 op basis van id-provider, zoals Twitter, zodat u kunt gebruikers om aan te melden met hun bestaande sociale of ondernemings-id's.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `OAuth1`. Bijvoorbeeld, het protocol voor de **Twitter-OAUTH1** technische profiel is `OAuth1`.

```XML
<TechnicalProfile Id="Twitter-OAUTH1">
  <DisplayName>Twitter</DisplayName>
  <Protocol Name="OAuth1" />
  ...    
```

## <a name="input-claims"></a>Invoerclaims

De **InputClaims** en **InputClaimsTransformations** elementen zijn leeg of niet aanwezig.

## <a name="output-claims"></a>Gebruikersclaims

De **OutputClaims** element bevat een lijst met claims die wordt geretourneerd door de OAuth1-id-provider. Mogelijk moet u de naam van de claim die is gedefinieerd in uw beleid aan de naam die is gedefinieerd in de id-provider toewijzen. U kunt ook bevatten claims die niet zijn geretourneerd door de id-provider, zolang u de **DefaultValue** kenmerk.

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.

Het volgende voorbeeld ziet u de claims die wordt geretourneerd door de Twitter-id-provider:

- De **user_id** claim die is toegewezen aan de **socialIdpUserId** claim.
- De **screen_name** claim die is toegewezen aan de **displayName** claim.
- De **e** claim zonder naam toewijzingen.

Het technische profiel retourneert ook claims die niet zijn geretourneerd door de id-provider: 

- De **identityProvider** claim met de naam van de id-provider.
- De **authenticationSource** claim met een standaardwaarde van `socialIdpAuthentication`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="user_id" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| client_id | Ja | De toepassings-id van de id-provider. |
| ProviderName | Nee | De naam van de id-provider. |
| request_token_endpoint | Ja | De URL van het eindpunt van de aanvraag-token aan de hand van RFC 5849. |
| authorization_endpoint | Ja | De URL van de autorisatie-eindpunt aan de hand van RFC 5849. |
| access_token_endpoint | Ja | De URL van het token-eindpunt aan de hand van RFC 5849. |
| ClaimsEndpoint | Nee | De URL van het eindpunt van de gebruiker informatie. | 
| ClaimsResponseFormat | Nee | De indeling van de claims-antwoord.|

## <a name="cryptographic-keys"></a>Cryptografische sleutels

De **CryptographicKeys** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| client_secret | Ja | Het clientgeheim van de toepassing van id-provider.   | 

## <a name="redirect-uri"></a>Omleidings-URI

Wanneer u de omleidings-URL van uw id-provider configureren, voert u `https://login.microsoftonline.com/te/tenant/policyId/oauth1/authresp`. Vervang **tenant** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) en **policyId** met de id van uw beleid (bijvoorbeeld b2c_1a_policy). De omleidings-URI moet zich in alleen kleine letters. U moet een omleiding UR voor alle beleidsregels die gebruikmaken van aanmelding bij de id-provider toevoegen. 

Als u de **b2clogin.com** domein in plaats van **login.microsoftonline.com** Zorg ervoor dat u b2clogin.com in plaats van login.microsoftonline.com.

Voorbeelden:

- [Twitter als id-provider OAuth1 toevoegen met behulp van aangepaste beleidsregels](active-directory-b2c-custom-setup-twitter-idp.md)













