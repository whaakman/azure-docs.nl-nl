---
title: Een technisch profiel definiëren voor een JWT-token certificaatverlener in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer een technisch profiel voor een JWT-token certificaatverlener in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 25352d12e578c289ccb4ab8aab60dc55a444762e
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50413499"
---
# <a name="define-a-technical-profile-for-a-jwt-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Een technisch profiel voor een JWT-token certificaatverlener in een aangepast Azure Active Directory B2C-beleid definiëren

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C verzendt verschillende typen beveiligingstokens tijdens de verwerking van elke verificatiestroom. Een technisch profiel voor een JWT-token verlener verzendt een JWT-token dat wordt geretourneerd naar de relying party-toepassing. In dit technisch profiel is meestal de laatste indelingsstap in de gebruikersbeleving.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `None`. Stel de **OutputTokenFormat** element `JWT`.

Het volgende voorbeeld ziet u een technisch profiel voor `JwtIssuer`:

```XML
<TechnicalProfile Id="JwtIssuer">
  <DisplayName>JWT Issuer</DisplayName>
  <Protocol Name="None" />
  <OutputTokenFormat>JWT</OutputTokenFormat>
  ...
</TechnicalProfile>
```
 
## <a name="input-output-and-persist-claims"></a>De invoer, uitvoer en blijven claims

De **InputClaims**, **OutputClaims**, en **PersistClaims** elementen zijn leeg of niet aanwezig. De **InutputClaimsTransformations** en **OutputClaimsTransformations** elementen zijn ook aanwezig.

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| issuer_refresh_token_user_identity_claim_type | Ja | De claim die moet worden gebruikt als de gebruikers-id claimen binnen de OAuth2-autorisatiecodes en vernieuwingstokens. Standaard moet u dit instellen op `objectId`, tenzij u een andere SubjectNamingInfo claimtype opgeven. | 
| SendTokenResponseBodyWithJsonNumbers | Nee | Altijd ingesteld op `true`. Voor oudere indeling waar numerieke waarden zijn opgegeven als tekenreeksen in plaats van JSON-nummers, die is ingesteld op `false`. Dit kenmerk is vereist voor clients die een afhankelijkheid op een eerdere implementatie dat dergelijke eigenschappen als tekenreeksen geretourneerd hebt ondernomen. | 
| token_lifetime_secs | Nee | Toegang tot levensduur van tokens. De levensduur van het Bearer-token van OAuth 2.0 dat wordt gebruikt om toegang te krijgen tot een beveiligde resource. De standaardwaarde is 3600 seconden (1 uur). De minimumwaarde (inclusief) is 300 seconden (5 minuten). Het maximum (inclusief) is 86.400 seconden (24 uur). | 
| id_token_lifetime_secs | Nee | ID-levensduur van tokens. De standaardwaarde is 3600 seconden (1 uur). De minimumwaarde (inclusief) is 300 seconden (5 minuten). Het maximum (inclusief) is seconden 86,400 (24 uur). | 
| refresh_token_lifetime_secs | Nee | Vernieuw de levensduur van tokens. De maximale periode waarbinnen een vernieuwingstoken kan worden gebruikt om een nieuw toegangstoken verkrijgen als uw toepassing het bereik offline_access is verleend. De standaardwaarde is 120,9600 seconden (14 dagen). De minimumwaarde (inclusief) is 86.400 seconden (24 uur). Het maximum (inclusief) is 7,776,000 seconden (90 dagen). | 
| rolling_refresh_token_lifetime_secs | Nee | Levensduur van sliding window token vernieuwen. Nadat deze periode is verstreken wordt de gebruiker wordt gedwongen om te verifiëren, ongeacht de geldigheidsperiode van de meest recente vernieuwingstoken verkregen door de toepassing. Als u niet wilt om af te dwingen een levensduur van sliding window, stel de waarde van allow_infinite_rolling_refresh_token naar `true`. De standaardwaarde is 7,776,000 seconden (90 dagen). De minimumwaarde (inclusief) is 86.400 seconden (24 uur). Het maximum (inclusief) is 31,536,000 seconden (365 dagen). | 
| allow_infinite_rolling_refresh_token | Nee | Indien ingesteld op `true`, het vernieuwingstoken venster Verschuivend levensduur verloopt nooit. |
| IssuanceClaimPattern | Ja | Hiermee bepaalt u de claim van verlener (iss). Een van de waarden:<ul><li>AuthorityAndTenantGuid - de iss-claim bevat de naam van uw domein, zoals `login.microsoftonline` of `tenant-name.b2clogin.com`, en uw tenant-id https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000/v2.0/</li><li>AuthorityWithTfp - de iss-claim bevat de naam van uw domein, zoals `login.microsoftonline` of `tenant-name.b2clogin.com`, uw tenant-id en de naam van uw relying party-beleid. https://login.microsoftonline.com/tfp/00000000-0000-0000-0000-000000000000/b2c_1a_tp_sign-up-or-sign-in/v2.0/</li></ul> | 
| AuthenticationContextReferenceClaimPattern | Nee | Besturingselementen voor de `acr` claimwaarde.<ul><li>Geen - Azure AD B2C niet de acr-claim uitgeven</li><li>PolicyId - de `acr` claim bevat de naam van het beleid</li></ul>De opties voor het instellen van deze waarde zijn TFP (vertrouwensbeleid framework) en ACR (authentication context verwijzing). Het verdient aanbeveling de waarde instelt op TFP, als u wilt de waarde instelt, zorg ervoor dat de `<Item>` met de `Key="AuthenticationContextReferenceClaimPattern"` bestaat en de waarde is `None`. Voeg in uw relying party-beleid, <OutputClaims> item, voegt u dit element `<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />`. Ook voor zorgen dat uw beleid bevat het claimtype `<ClaimType Id="trustFrameworkPolicy"> <DisplayName>trustFrameworkPolicy</DisplayName>     <DataType>string</DataType> </ClaimType>` | 

## <a name="cryptographic-keys"></a>Cryptografische sleutels

Het element CryptographicKeys bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| issuer_secret | Ja | De X509 certificaat (RSA-sleutelpaar) moet worden gebruikt om te ondertekenen van de JWT-token. Dit is de `B2C_1A_TokenSigningKeyContainer` sleutel u geconfigureerd kan [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md). | 
| issuer_refresh_token_key | Ja | De X509 (RSA-sleutelpaar) te gebruiken voor het versleutelen van het vernieuwingstoken dat certificaat. U hebt geconfigureerd de `B2C_1A_TokenEncryptionKeyContainer` sleutel in [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) |














