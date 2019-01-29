---
title: Het technische profiel van een Azure Active Directory definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer het technische profiel van een Azure Active Directory in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b8dac47d1aa91eb8a8ee1ef9515809607b267437
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190698"
---
# <a name="define-an-azure-active-directory-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Het technische profiel van een Azure Active Directory in een aangepast Azure Active Directory B2C-beleid definiëren

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C biedt ondersteuning voor het beheer van de Azure Active Directory-gebruikers. Dit artikel beschrijft de details van een technisch profiel voor interactie met een claimprovider die ondersteuning biedt voor dit gestandaardiseerde protocol.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `Proprietary`. De **handler** kenmerk mag de volledig gekwalificeerde naam van de assembly van de handler protocol `Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Alle technische Azure AD-profielen bevatten de **AAD-gemeenschappelijke** technisch profiel. De volgende technische profielen niet zelf het protocol bepalen omdat het protocol is geconfigureerd de **AAD-gemeenschappelijke** technisch profiel:

- **AAD-UserReadUsingAlternativeSecurityId** en **AAD-UserReadUsingAlternativeSecurityId-NoError** - zoekt u naar boven een sociaal account in de map.
- **AAD-UserWriteUsingAlternativeSecurityId** -een nieuw sociale account maken.
- **AAD-UserReadUsingEmailAddress** - zoekt u naar een lokaal account in de map boven. 
- **AAD-UserWriteUsingLogonEmail** -Maak een nieuwe lokale account.
- **AAD-UserWritePasswordUsingObjectId** -Update van een wachtwoord van een lokaal account.
- **AAD-UserWriteProfileUsingObjectId** -een gebruikersprofiel van een lokale of sociale account bijwerken.
- **AAD-UserReadUsingObjectId** -een gebruikersprofiel van een lokale of sociale account te lezen.
- **AAD-UserWritePhoneNumberUsingObjectId** -het MFA-telefoonnummer van een lokale of sociale account schrijven

Het volgende voorbeeld wordt de **AAD-gemeenschappelijke** technisch profiel:

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />

  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
  </CryptographicKeys>

  <!-- We need this here to suppress the SelfAsserted provider from invoking SSO on validation profiles. -->
  <IncludeInSso>false</IncludeInSso>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Invoerclaims

De volgende technische profielen bevatten **InputClaims** voor sociale en lokale accounts:

- De technische profielen sociaalnetwerkaccount **AAD-UserReadUsingAlternativeSecurityId** en **AAD-UserWriteUsingAlternativeSecurityId** bevat de **AlternativeSecurityId** claim. Deze claim bevat de gebruikers-id van het sociaalnetwerkaccount.
- Het lokale account technische profielen **AAD-UserReadUsingEmailAddress** en **AAD-UserWriteUsingLogonEmail** bevat de **e** claim. Deze claim bevat de naam van het lokale account.
- De geïntegreerde (lokaal en sociale) technische profielen **AAD-UserReadUsingObjectId**, **AAD-UserWritePasswordUsingObjectId**, **AAD-UserWriteProfileUsingObjectId**, en **AAD-UserWritePhoneNumberUsingObjectId** bevat de **objectId** claim. De unieke id van een account.

De **InputClaimsTransformations** element kan bevatten een verzameling van **InputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de invoerclaims of nieuwe labels te genereren.

## <a name="output-claims"></a>Gebruikersclaims

De **OutputClaims** element bevat een lijst met claims die wordt geretourneerd door het technische profiel van de Azure AD. Mogelijk moet u de naam van de claim die is gedefinieerd in uw beleid aan de naam die is gedefinieerd in Azure Active Directory toewijzen. U kunt ook claims die niet zijn geretourneerd door de Azure Active Directory opnemen, zolang u de `DefaultValue` kenmerk.

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.

Bijvoorbeeld, de **AAD-UserWriteUsingLogonEmail** technisch profiel een lokaal account maakt en retourneert de volgende claims:

- **object-id**, dit is de id van het nieuwe account
- **nieuwegebruiker**, waarmee wordt aangegeven of de gebruiker is er nieuw
- **authenticationSource**, welke verificatie wordt ingesteld op `localAccountAuthentication`
- **userPrincipalName**, dit is de user principal name van het nieuwe account
- **signInNames.emailAddress**, dit is de aanmelding accountnaam, vergelijkbaar met de **e** invoer claim

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="objectId" />
  <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
  <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
  <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
</OutputClaims>
```

## <a name="persistedclaims"></a>PersistedClaims

De **PersistedClaims** element bevat alle waarden die moeten worden vastgehouden door Azure AD met mogelijke toewijzingsgegevens tussen een claimtype al gedefinieerd in de sectie ClaimsSchema in het beleid en de Azure AD-kenmerk de naam. 

De **AAD-UserWriteUsingLogonEmail** technisch profiel, die nieuwe lokale account wordt gemaakt, zich blijft voordoen na de claims:

```XML
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
    <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password"/>
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />

    <!-- Optional claims. -->
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
```

De naam van de claim is de naam van de Azure AD-kenmerk, tenzij de **PartnerClaimType** kenmerk is opgegeven, die de naam van het Azure AD-kenmerk bevat.

## <a name="requirements-of-an-operation"></a>Vereisten van een bewerking

- Er moet exact één **InputClaim** -element in de claims in eigenschappenverzameling voor technische profielen voor alle Azure AD. 
- Als de bewerking is `Write` of `DeleteClaims`, dan moet deze worden weergegeven een **PersistedClaims** element.
- De waarde van de **userPrincipalName** claim moet zich in de indeling van `user@tenant.onmicrosoft.com`.
- De **displayName** claim is vereist en mag geen lege tekenreeks.

## <a name="azure-ad-technical-provider-operations"></a>Azure AD technische providerbewerkingen

### <a name="read"></a>Lezen

De **lezen** bewerking leest de gegevens over één gebruikersaccount. Als u wilt lezen gebruikersgegevens, moet u een sleutel opgeven als een invoer claim, zoals **objectId**, **userPrincipalName**, **signInNames** (elk type, de gebruikersnaam en de account op basis van een e-mailadres) of **alternativeSecurityId**.  

De volgende technische profiel leest de gegevens over een gebruikersaccount met behulp van de gebruiker object-id:

```XML
<TechnicalProfile Id="AAD-UserReadUsingObjectId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims>

    <!-- Required claims -->
    <OutputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />

    <!-- Optional claims -->
    <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="write"></a>Schrijven

De **schrijven** bewerking gemaakt of bijgewerkt één gebruikersaccount. Voor het schrijven van een gebruikersaccount, moet u een sleutel opgeven als een invoer claim zoals **objectId**, **userPrincipalName**, **signInNames.emailAddress**, of  **alternativeSecurityId**.  

De volgende technische profiel maakt nieuwe sociaal account:

```XML
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Write</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalAlreadyExists">You are already registered, please press the back button and sign in instead.</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CreateOtherMailsFromEmail" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <PersistedClaim ClaimTypeReferenceId="userPrincipalName" />
    <PersistedClaim ClaimTypeReferenceId="mailNickName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />

    <!-- Optional claims -->
    <PersistedClaim ClaimTypeReferenceId="otherMails" />
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>
```

### <a name="deleteclaims"></a>DeleteClaims

De **DeleteClaims** bewerking wist de gegevens uit een lijst van claims. Als gegevens wilt verwijderen van claims, moet u een sleutel opgeven als een invoer claim zoals **objectId**, **userPrincipalName**, **signInNames.emailAddress** of **alternativeSecurityId**.  

De volgende technische profiel wordt verwijderd van claims:

```XML
<TechnicalProfile Id="AAD-DeleteClaimsUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaims</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" PartnerClaimType="strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims />
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="deleteclaimsprincipal"></a>DeleteClaimsPrincipal

De **DeleteClaimsPrincipal** bewerking wordt een gebruikersaccount verwijderd uit de map. Als u wilt verwijderen van een gebruikersaccount, moet u een sleutel opgeven als een invoer claim zoals **objectId**, **userPrincipalName**, **signInNames.emailAddress** of  **alternativeSecurityId**.  

De volgende technische profiel wordt een gebruikersaccount dat is verwijderd uit de directory met behulp van de user principal name:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

De volgende technische profiel Hiermee verwijdert u een sociale gebruikersaccount met **alternativeSecurityId**:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```
## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Bewerking | Ja | De bewerking die moet worden uitgevoerd. Mogelijke waarden: `Read`, `Write`, `DeleteClaims`, of `DeleteClaimsPrincipal`. | 
| RaiseErrorIfClaimsPrincipalDoesNotExist | Nee | Als het gebruikersobject niet in de map bestaat, moet u een foutbericht. Mogelijke waarden: `true` of `false`. | 
| UserMessageIfClaimsPrincipalDoesNotExist | Nee | Als een fout is gegenereerd (Zie de beschrijving van het kenmerk RaiseErrorIfClaimsPrincipalDoesNotExist), geef het bericht om weer te geven aan de gebruiker als gebruikersobject niet bestaat. De waarde kan zijn [gelokaliseerde](localization.md).| 
| RaiseErrorIfClaimsPrincipalAlreadyExists | Nee | Als het gebruikersobject al bestaat, moet u een foutbericht. Mogelijke waarden: `true` of `false`.| 
| UserMessageIfClaimsPrincipalAlreadyExists | Nee | Als een fout is gegenereerd (Zie de beschrijving van het kenmerk RaiseErrorIfClaimsPrincipalAlreadyExists), geef het bericht om weer te geven aan de gebruiker als gebruikersobject al bestaat. De waarde kan zijn [gelokaliseerde](localization.md).| 
| ApplicationObjectId | Nee | De toepassings-object-id voor extensiekenmerken. Waarde: Object-id van een toepassing. Zie voor meer informatie, [gebruik aangepaste kenmerken in een aangepast profiel bewerken beleid](active-directory-b2c-create-custom-attributes-profile-edit-custom.md). | 
| ClientId | Nee | De client-id voor toegang tot de tenant als een derde partij. Zie voor meer informatie, [beleid voor aangepaste kenmerken gebruiken in een aangepast profiel bewerken](active-directory-b2c-create-custom-attributes-profile-edit-custom.md) | 














