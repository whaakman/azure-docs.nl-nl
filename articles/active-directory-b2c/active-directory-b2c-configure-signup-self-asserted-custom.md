---
title: Aanmelding van wijzigen in het aangepaste beleid en configureren zelf door de provider bevestigde | Microsoft Docs
description: Een overzicht over het toevoegen van claims moeten zich aanmelden en het configureren van de invoer van de gebruiker
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 37492e22b5615ae0b266bc8b2bb6d8f039fdaabe
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336852"
---
# <a name="azure-active-directory-b2c-modify-sign-up-to-add-new-claims-and-configure-user-input"></a>Azure Active Directory B2C: Wijzigen sign up nieuwe claims toevoegen en configureren van de invoer van de gebruiker.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel wordt u een nieuwe vermelding voor de gebruiker opgegeven (een claim) toevoegen aan uw aanmelding bij de gebruikersbeleving.  U wordt de vermelding configureren als een vervolgkeuzelijst en definiëren als dat nodig is.

## <a name="prerequisites"></a>Vereisten

* Voer de stappen in het artikel [aan de slag met beleid voor aangepaste](active-directory-b2c-get-started-custom.md).  De gebruikersbeleving registreren/aanmelden om de registratie een nieuw lokaal account voordat u doorgaat te testen.


Initiële gegevens verzamelen uit uw gebruikers wordt via aanmelding/aanmelding bereikt.  Aanvullende claims kunnen later worden verzameld via profiel bewerken gebruiker reizen. Telkens wanneer de Azure AD B2C verzamelt interactief gegevens rechtstreeks van de gebruiker, de Identiteitservaring-Framework gebruikt de `selfasserted provider`. De onderstaande stappen van toepassing op elk moment deze provider wordt gebruikt.


## <a name="define-the-claim-its-display-name-and-the-user-input-type"></a>De claim, de weergavenaam en het type gebruikersinvoer definiëren
U kunt de gebruiker vragen voor hun plaats.  Voeg het volgende element aan de `<ClaimsSchema>` -element in het beleidsbestand TrustFrameworkBase:

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
Er zijn aanvullende opties die u kunt hier om aan te passen van de claim.  Voor een volledige schema, raadpleegt u de **identiteit ervaring Framework Technical Reference Guide**.  Deze handleiding zal binnenkort worden gepubliceerd in de sectie documentatie.

* `<DisplayName>` is een tekenreeks waarin de gebruiker gerichte *label*

* `<UserHelpText>` de gebruiker weten wat is vereist

* `<UserInputType>` heeft de volgende vier opties hieronder beschreven:
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * `RadioSingleSelectduration` -Hiermee wordt een enkelvoudige selectie.
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

    * `DropdownSingleSelect` -Hiermee kunt de selectie van de enige geldige waarde.

![Schermopname van de vervolgkeuzelijst](./media/active-directory-b2c-configure-signup-self-asserted-custom/dropdown-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```


* `CheckboxMultiSelect` Kunt u de selectie van een of meer waarden.

![Schermafbeelding van multiselect-optie](./media/active-directory-b2c-configure-signup-self-asserted-custom/multiselect-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>Receive updates from which cities?</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-the-claim-to-the-sign-upsign-in-user-journey"></a>Toevoegen van de claim naar de aanmeldingspagina omhoog/teken in de gebruikersbeleving

1. Toevoegen van de claim als een `<OutputClaim ClaimTypeReferenceId="city"/>` aan het technische profiel `LocalAccountSignUpWithLogonEmail` (gevonden in het beleidsbestand TrustFrameworkBase).  Houd er rekening mee dat de SelfAssertedAttributeProvider maakt gebruik van deze technische profiel.

  ```xml
  <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
    <DisplayName>Email signup</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
      <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
      <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
      <Item Key="language.button_continue">Create</Item>
    </Metadata>
    <CryptographicKeys>
      <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
    </CryptographicKeys>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
      <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" />
      <OutputClaim ClaimTypeReferenceId="newUser" />
      <!-- Optional claims, to be collected from the user -->
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surName" />
      <OutputClaim ClaimTypeReferenceId="city"/>
    </OutputClaims>
    <ValidationTechnicalProfiles>
      <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
    </ValidationTechnicalProfiles>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

2. Toevoegen van de claim naar de AAD-UserWriteUsingLogonEmail als een `<PersistedClaim ClaimTypeReferenceId="city" />` schrijven van de claim naar de map AAD na het verzamelen van de gebruiker. U kunt deze stap overslaan als u liever niet om vast te leggen van de claim in de map voor toekomstig gebruik.

  ```xml
  <!-- Technical profiles for local accounts -->
  <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
    <Metadata>
      <Item Key="Operation">Write</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" Required="true" />
    </InputClaims>
    <PersistedClaims>
      <!-- Required claims -->
      <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
      <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password" />
      <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
      <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />
      <!-- Optional claims. -->
      <PersistedClaim ClaimTypeReferenceId="givenName" />
      <PersistedClaim ClaimTypeReferenceId="surname" />
      <PersistedClaim ClaimTypeReferenceId="city" />
    </PersistedClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

3. Toevoegen van de claim naar het technische profiel dat wordt gelezen uit de map wanneer een gebruiker zich als aanmeldt een `<OutputClaim ClaimTypeReferenceId="city" />`

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for the provided user ID.</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames" Required="true" />
    </InputClaims>
    <OutputClaims>
      <!-- Required claims -->
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <!-- Optional claims -->
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="otherMails" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  </TechnicalProfile>
  ```

4. Voeg de `<OutputClaim ClaimTypeReferenceId="city" />` bestand aan het beleid RP SignUporSignIn.xml, zodat deze claim wordt verzonden naar de toepassing in het token na een geslaagde gebruikersbeleving.

  ```xml
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
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ```

## <a name="test-the-custom-policy-using-run-now"></a>Het aangepaste beleid met behulp van 'Nu uitvoeren' testen

1. Open de **Azure AD B2C-Blade** en navigeer naar **Identity-Ervaringsframework > aangepast beleid**.
2. Selecteer het aangepaste beleid dat u geüpload en klikt u op de **nu uitvoeren** knop.
3. U zou het mogelijk om u te registreren met behulp van een e-mailadres.

Het scherm registreren in de testmodus moet er ongeveer als volgt uitzien:

![Schermopname van het gewijzigde aanmeldingsoptie](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  Het token naar de toepassing bevat nu de `city` claim zoals hieronder wordt weergegeven
```json
{
  "exp": 1493596822,
  "nbf": 1493593222,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "9c2a3a9e-ac65-4e46-a12d-9557b63033a9",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustf_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1493593222,
  "auth_time": 1493593222,
  "email": "joe@outlook.com",
  "given_name": "Joe",
  "family_name": "Ras",
  "city": "Bellevue",
  "name": "unknown"
}
```

## <a name="optional-remove-email-verification-from-signup-journey"></a>Optioneel: Verwijder e-mailverificatie van aanmelding traject

Als u wilt overslaan e-mailverificatie, de auteur van het beleid kunt verwijderen `PartnerClaimType="Verified.Email"`. Het e-mailadres wordt vereist maar niet geverifieerd, tenzij 'Vereist' = true wordt verwijderd.  U moet zorgvuldig overwegen als deze optie geschikt is voor uw use-cases is!

Geverifieerd met het e-mailbericht is standaard ingeschakeld in de `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in het TrustFrameworkBase beleid-bestand in de beginnerspakket:
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a>Volgende stappen

De nieuwe claim naar de stromen voor sociaal account aanmeldingen toevoegen door het wijzigen van de hieronder vermelde TechnicalProfiles. Deze worden gebruikt door account sociale/federatieve aanmeldingen te schrijven en lezen van de gebruikersgegevens met behulp van de alternativeSecurityId als de locator.
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
