---
title: Een technisch OpenId Connect-profiel te definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer een technische OpenId Connect-profiel in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: a13ca362bf08b86297641061992f0820f0b624c5
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916764"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een technisch OpenId Connect-profiel te definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C biedt ondersteuning voor de [OpenId Connect](https://openid.net/2015/04/17/openid-connect-certification-program/) protocol-id-provider. OpenID Connect 1.0 definieert een identiteitslaag bovenop OAuth 2.0 en vertegenwoordigt de status van de techniek in moderne-verificatieprotocollen.  Technisch profiel dat u met een OpenId Connect kan federeren met OpenId Connect op basis van id-provider, zoals Azure AD, zodat u kunt gebruikers om aan te melden met hun bestaande sociale of ondernemings-id's.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `OpenIdConnect`. Bijvoorbeeld, het protocol voor de **MSA-OIDC** technische profiel is `OpenIdConnect`:

```XML
<TechnicalProfile Id="MSA-OIDC">
  <DisplayName>Microsoft Account</DisplayName>
  <Protocol Name="OpenIdConnect" />
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

De **OutputClaims** element bevat een lijst met claims die zijn geretourneerd door de identiteitsprovider OpenId Connect. Mogelijk moet u de naam van de claim die is gedefinieerd in uw beleid aan de naam die is gedefinieerd in de id-provider toewijzen. U kunt ook claims die niet zijn geretourneerd door de id-provider opnemen, zolang u de `DefaultValue` kenmerk.

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.

Het volgende voorbeeld ziet u de claims die wordt geretourneerd door de Microsoft-Account-id-provider:

- De **sub** claim die is toegewezen aan de **socialIdpUserId** claim.
- De **naam** claim die is toegewezen aan de **displayName** claim.
- De **e** zonder naam toewijzingen.

Het technische profiel retourneert ook claims die niet zijn geretourneerd door de id-provider:

- De **identityProvider** claim met de naam van de id-provider.
- De **authenticationSource** claim met een standaardwaarde van **socialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
  <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| client_id | Ja | De toepassings-id van de id-provider. |
| IdTokenAudience | Nee | De doelgroep van het id_token. Als u opgeeft, wordt Azure AD B2C controleert of het token in een claim die wordt geretourneerd door de id-provider is en gelijk aan de versie die is opgegeven is. |
| METAGEGEVENS | Ja | Een URL die verwijst naar een JSON-document-configuratie die is ingedeeld volgens de specificatie OpenID Connect Discovery, wat ook wel bekend als een bekende openid-configuratie-eindpunt. |
| ProviderName | Nee | De naam van de id-provider. |
| response_types | Nee | Het reactietype op basis van de OpenID Connect Core 1.0-specificatie. Mogelijke waarden: `id_token`, `code`, of `token`. |
| response_mode | Nee | De methode die de id-provider gebruikt voor het verzenden van het resultaat terug naar Azure AD B2C. Mogelijke waarden: `query`, `form_post` (standaard), of `fragment`. |
| scope | Nee | Het bereik van de aanvraag voor toegang tot gedefinieerd op basis van de OpenID Connect Core 1.0-specificatie. Zoals `openid`, `profile`, en `email`. |
| HttpBinding | Nee | De verwachte HTTP-binding met de toegang tot tokens en claims token-eindpunten. Mogelijke waarden: `GET` of `POST`.  |
| ValidTokenIssuerPrefixes | Nee | Een sleutel die kan worden gebruikt voor het aanmelden bij elk van de tenants bij het gebruik van een multitenant-id-provider zoals Azure Active Directory. |
| UsePolicyInRedirectUri | Nee | Hiermee wordt aangegeven of een beleidsregel gebruiken bij het maken van de omleidings-URI. Wanneer u uw toepassing in de id-provider configureert, moet u de omleidings-URI opgeven. De omleidings-URI verwijst naar Azure AD B2C, `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` (login.microsoftonline.com kan worden gewijzigd met de tenant-name.b2clogin.com).  Als u opgeeft `false`, moet u een omleidings-URI voor elk beleid dat u toevoegen. Bijvoorbeeld: `https://login.microsoftonline.com/te/{tenant}/{policy}/oauth2/authresp`. |
| MarkAsFailureOnStatusCode5xx | Nee | Geeft aan of een aanvraag naar een externe service moet worden gemarkeerd als mislukt als de Http-statuscode in het bereik 5xx is. De standaardwaarde is `false`. |
| DiscoverMetadataByTokenIssuer | Nee | Geeft aan of de OIDC-metagegevens moet worden gedetecteerd met behulp van de verlener in de JWT-token. |

## <a name="cryptographic-keys"></a>Cryptografische sleutels

De **CryptographicKeys** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| client_secret | Ja | Het clientgeheim van de toepassing van id-provider. De cryptografische sleutel is alleen vereist als de **response_types** metagegevens is ingesteld op `code`. In dit geval wordt Azure AD B2C een aanroep voor het uitwisselen van de autorisatiecode voor een toegangstoken. Als de metagegevens is ingesteld op `id_token` kunt u de cryptografische sleutel weglaten.  |  

## <a name="redirect-uri"></a>Redirect Uri
 
Wanneer u de omleidings-URI van uw id-provider configureren, voert u `https://login.microsoftonline.com/te/tenant/oauth2/authresp`. Vervang **tenant** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) of van de tenant-id. De omleidings-URI moet zich in alleen kleine letters.

Als u de **b2clogin.com** domein in plaats van **login.microsoftonline.com** Zorg ervoor dat u b2clogin.com in plaats van login.microsoftonline.com.

Voorbeelden:

- [Microsoft-Account (MSA) toevoegen als een id-provider met behulp van aangepaste beleidsregels](active-directory-b2c-custom-setup-msa-idp.md)
- [Meld u aan met behulp van Azure AD-accounts](active-directory-b2c-setup-aad-custom.md)
- [Toestaan dat gebruikers zich aanmelden met een id-provider van het Azure AD met meerdere tenants met behulp van aangepaste beleidsregels](active-directory-b2c-setup-commonaad-custom.md)

 














