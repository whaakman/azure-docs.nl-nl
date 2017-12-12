---
title: 'Azure Active Directory B2C: Sign up wijzigen in het aangepaste beleid en zelf-provider die wordt beweerd configureren'
description: Een stapsgewijze uitleg over het toevoegen van claims moeten registreren en invoer van de gebruiker configureren
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: tbd
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/29/2017
ms.author: joroja
ms.openlocfilehash: e9eb9fa941569c508c4dddc6b85786537a5a0fac
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-modify-sign-up-to-add-new-claims-and-configure-user-input"></a>Azure Active Directory B2C: Wijzigen aanmelding om nieuwe claims toe te voegen en invoer van gebruiker configureren.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel hebt toevoegt u een nieuwe vermelding voor gebruiker is opgegeven (een claim) aan uw aanmelding gebruiker reis.  U wordt de vermelding configureren als een vervolgkeuzelijst en definiëren als dat nodig is.

## <a name="prerequisites"></a>Vereisten

* Voer de stappen in het artikel [aan de slag met beleid voor aangepaste](active-directory-b2c-get-started-custom.md).  Test het traject aanmelding/aanmelding gebruiker voor aanmelding bij een nieuwe lokale account voordat u doorgaat.


Initiële gegevens verzamelen uit uw gebruikers wordt via aanmelding/aanmelding bereikt.  Aanvullende claims kunnen later via profiel bewerken gebruiker trajecten worden verzameld. Telkens wanneer Azure AD B2C informatie interactief rechtstreeks van de gebruiker verzamelt, het identiteit ervaring Framework gebruikt de `selfasserted provider`. De onderstaande stappen gelden telkens wanneer deze provider wordt gebruikt.


## <a name="define-the-claim-its-display-name-and-the-user-input-type"></a>Definieer de claim, de weergavenaam en het invoertype van gebruiker
Hiermee kunt de gebruiker vragen voor hun plaats.  Voeg het volgende element aan de `<ClaimsSchema>` element in het beleidsbestand TrustFrameworkBase:

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
Er zijn aanvullende opties die u kunt hier de claim aanpassen.  Raadpleeg voor een volledige schema, de **identiteit ervaring Framework technische handleiding**.  Deze handleiding zal binnenkort worden gepubliceerd in de sectie verwijzingen.

* `<DisplayName>`is een tekenreeks waarin de gebruiker gerichte *label*

* `<UserHelpText>`kan de gebruiker begrijpen wat is vereist

* `<UserInputType>`heeft de volgende vier opties hieronder gemarkeerd:
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * `RadioSingleSelectduration`-Een enkelvoudige selectie wordt afgedwongen.
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

    * `DropdownSingleSelect`-Hiermee kunt de selectie van de enige geldige waarde.

![Schermopname van dropdown-optie](./media/active-directory-b2c-configure-signup-self-asserted-custom/dropdown-menu-example.png)


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


* `CheckboxMultiSelect`Kunt u de selectie van een of meer waarden.

![Schermopname van meervoudige selectie optie](./media/active-directory-b2c-configure-signup-self-asserted-custom/multiselect-menu-example.png)


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

## <a name="add-the-claim-to-the-sign-upsign-in-user-journey"></a>Toevoegen van de claim op het teken omhoog/reis gebruiker aanmelden

1. Toevoegen van de claim als een `<OutputClaim ClaimTypeReferenceId="city"/>` naar de TechnicalProfile `LocalAccountSignUpWithLogonEmail` (te vinden in het beleidsbestand TrustFrameworkBase).  Houd er rekening mee dat deze TechnicalProfile maakt gebruik van de SelfAssertedAttributeProvider.

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

2. Toevoegen van de claim de AAD-UserWriteUsingLogonEmail als een `<PersistedClaim ClaimTypeReferenceId="city" />` schrijven van de claim naar de AAD-directory na het verzamelen van de gebruiker. Als u liever niet persistent maken van de claim in de map voor toekomstig gebruik, kunt u deze stap overslaan.

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

3. Toevoegen van de claim de TechnicalProfile die uit de map lezen wanneer een gebruiker zich als aanmeldt een`<OutputClaim ClaimTypeReferenceId="city" />`

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

4. Voeg de `<OutputClaim ClaimTypeReferenceId="city" />` bestand aan het beleid RP SignUporSignIn.xml zodat deze claim wordt verzonden naar de toepassing in het token na een geslaagde-transport.

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

## <a name="test-the-custom-policy-using-run-now"></a>Het aangepaste beleid met 'Nu uitvoeren' testen

1. Open de **Azure AD B2C-Blade** en navigeer naar **identiteit ervaring Framework > aangepast beleid**.
2. Selecteer het aangepaste beleid die u hebt geüpload en klik op de **nu uitvoeren** knop.
3. U moet mogelijk aan te melden met behulp van een e-mailadres.

Het scherm aanmelding in de testmodus moet er ongeveer als volgt uitzien:

![Schermopname van gewijzigde aanmeldingsoptie](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  Het token terug naar de toepassing omvatten nu de `city` claim, zoals hieronder wordt weergegeven
```json
{
  "exp": 1493596822,
  "nbf": 1493593222,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
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

## <a name="optional-remove-email-verification-from-signup-journey"></a>Optioneel: Verwijderen e-mailverificatie van aanmelding reis

Als u wilt overslaan e-mailverificatie, de auteur van het beleid kunt verwijderen `PartnerClaimType="Verified.Email"`. Het e-mailadres vereist maar niet geverifieerd, tenzij 'Vereist' = true wordt verwijderd.  Overweeg zorgvuldig als deze optie geschikt voor uw gebruiksvoorbeelden is!

Geverifieerd e-mailbericht is standaard ingeschakeld in de `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in het beleidsbestand TrustFrameworkBase in het starter pack:
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a>Volgende stappen

De nieuwe claim op de stromen voor sociale account aanmeldingen toevoegen door het wijzigen van de hieronder vermelde TechnicalProfiles. Deze worden gebruikt door aanmeldingen sociale/federatieve account om te schrijven en de gebruikersgegevens lezen met de alternativeSecurityId als de locator.
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
