---
title: Een zelf-gecontroleerde technisch profiel definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer een zelf-gecontroleerde technisch profiel in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: dcc94daeb19174b85fface05222f8842e9544adf
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188861"
---
# <a name="define-a-self-asserted-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een zelf-gecontroleerde technisch profiel definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Alle interacties in Azure Active Directory (Azure AD) B2C waar de gebruiker wordt verwacht voor invoer zijn zelf door bevestigde technische profielen. Bijvoorbeeld, een pagina voor het registreren, de aanmeldingspagina opgeven of het wachtwoord opnieuw instellen pagina.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `Proprietary`. De **handler** kenmerk mag de volledig gekwalificeerde naam van de handler-assembly voor protocol dat wordt gebruikt door Azure AD B2C, voor zelf een door de bevestigde: `Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Het volgende voorbeeld ziet een zelf-gecontroleerde technisch profiel voor e-mailbericht Meld u aan:

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
```
 
## <a name="input-claims"></a>Invoerclaims

In een zelf-gecontroleerde technisch profiel, kunt u de **InputClaims** en **InputClaimsTransformations** elementen om in te vullen van de waarde van de claims die worden weergegeven op de zelf-gecontroleerde pagina (uitvoer claims). Bijvoorbeeld, in het beleid van het profiel bewerken de gebruikersbeleving eerst het gebruikersprofiel van de Azure AD B2C-directory-service leest vervolgens de zelf-gecontroleerde technisch profiel de invoerclaims ingesteld met de gegevens die zijn opgeslagen in het gebruikersprofiel. Deze claims worden verzameld van het gebruikersprofiel en vervolgens gepresenteerd aan de gebruiker die vervolgens de bestaande gegevens kunt bewerken.

```XML
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <InputClaim ClaimTypeReferenceId="userPrincipalName" />
    <InputClaim ClaimTypeReferenceId="givenName" />
    <InputClaim ClaimTypeReferenceId="surname" />
  </InputClaims>
```


## <a name="output-claims"></a>Gebruikersclaims

De **OutputClaims** element bevat een lijst met claims kan worden weergegeven voor het verzamelen van gegevens van de gebruiker. Voor het vullen van de uitvoerclaims met bepaalde waarden, gebruikt u de invoerclaims die eerder zijn beschreven. Het element kan ook een standaardwaarde bevatten. De volgorde van de claims in **OutputClaims** bepaalt de volgorde die Azure AD B2C de claims in het scherm wordt weergegeven. De **DefaultValue** kenmerk wordt alleen toegepast als de claim is nooit is ingesteld. Maar als deze is ingesteld voordat in de vorige stap orchestration, zelfs als de gebruiker de waarde leeg laat, de standaardwaarde is pas van kracht. Om af te dwingen het gebruik van een standaardwaarde, stel de **AlwaysUseDefaultValue** kenmerk `true`. Als u wilt dat de gebruiker een waarde opgeven voor een specifieke uitvoerclaim, stel de **vereist** kenmerk van de **OutputClaims** element op de `true`.

De **ClaimType** -element in de **OutputClaims** verzameling nodig heeft om in te stellen de **UserInputType** element aan een gebruiker invoer dat wordt ondersteund door Azure AD B2C, zoals `TextBox`of `DropdownSingleSelect`. Of de **OutputClaim** element moet instellen een **DefaultValue**.  

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.

De volgende uitvoerclaim is altijd ingesteld op `live.com`:

```XML
<OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" AlwaysUseDefaultValue="true" />
```

### <a name="use-case"></a>Use-case

Er zijn vier scenario's voor uitvoerclaims:

- **De uitvoer verzamelen van vorderingen van de gebruiker** - wanneer u nodig hebt voor het verzamelen van informatie van de gebruiker, zoals Geboortedatum, moet u de claim toevoegen de **OutputClaims** verzameling. De claims die worden weergegeven voor de gebruiker moeten opgeven de **UserInputType**, zoals `TextBox` of `DropdownSingleSelect`. Als de zelf-gecontroleerde technisch profiel een validatie technisch profiel die dezelfde claim uitvoert bevat, wordt de claim voor de gebruiker niet aanwezig in Azure AD B2C. Als er geen eventuele uitvoerclaim om aan de gebruiker te presenteren, slaat Azure AD B2C het technische profiel.
- **Instellen van een standaardwaarde is opgegeven in een uitvoerclaim** - zonder dat het verzamelen van gegevens van de gebruiker of het opvragen van de gegevens uit het technische validatieprofiel. De `LocalAccountSignUpWithLogonEmail` zelf een technisch profiel ingesteld door de bevestigde de **uitgevoerd SelfAsserted invoer** claim moet worden `true`.
- **Een profiel van de technische retourneert de uitvoerclaims** -uw technisch profiel kan een technische validatie-profiel dat bepaalde claims retourneert aanroepen. U kunt naar de claims boven en retourneren aan de volgende indelingsstappen in de gebruikersbeleving. Bijvoorbeeld, wanneer u zich aanmeldt met een lokale account, de zelf-gecontroleerde technisch profiel met de naam `SelfAsserted-LocalAccountSignin-Email` roept de validatie van een technisch profiel met de naam `login-NonInteractive`. In dit technisch profiel valideert de referenties van de gebruiker en geeft ook het gebruikersprofiel. Such as 'userPrincipalName', 'displayName', 'givenName' and 'surName'.
- **Uitvoer van de claims via uitvoer claimtransformatie**

In het volgende voorbeeld wordt de `LocalAccountSignUpWithLogonEmail` zelf door technisch profiel ziet u het gebruik van gebruikersclaims en stelt de bevestigde **uitgevoerd SelfAsserted invoer** naar `true`. De `objectId`, `authenticationSource`, `newUser` claims zijn uitvoer van de `AAD-UserWriteUsingLogonEmail` technische validatie profileren en worden niet weergegeven aan de gebruiker.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    <Item Key="language.button_continue">Create</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
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
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surName" />
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
  </ValidationTechnicalProfiles>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>

```

## <a name="persist-claims"></a>Claims behouden

Als de **PersistedClaims** element niet aanwezig is, de zelf-gecontroleerde technisch profiel blijven niet behouden van de gegevens naar Azure AD B2C. In plaats daarvan wordt een aanroep aan een technische validatie-profiel dat verantwoordelijk is voor het behouden van de gegevens. Het registratiebeleid gebruikt bijvoorbeeld de `LocalAccountSignUpWithLogonEmail` zelf door bevestigde technisch profiel voor het verzamelen van het nieuwe profiel. De `LocalAccountSignUpWithLogonEmail` technisch profiel roept de validatie van technisch profiel voor het maken van het account in Azure AD B2C.

## <a name="validation-technical-profiles"></a>Technische validatie-profielen

Een profiel van de technische wordt gebruikt voor het valideren van sommige of alle van de uitvoerclaims van de verwijzende technisch profiel. De invoer claims van het technische validatieprofiel moeten worden weergegeven in de uitvoerclaims van de zelf-gecontroleerde technisch profiel. Het technische validatieprofiel valideert de invoer van de gebruiker en een fout kunt terugkeren naar de gebruiker. 

Het technische validatieprofiel mag technisch profiel in het beleid, zoals [Azure Active Directory](active-directory-technical-profile.md) of een [REST-API](restful-technical-profile.md) technische profielen. In het vorige voorbeeld de `LocalAccountSignUpWithLogonEmail` technisch profiel valideert de signinName bestaat niet in de map. Zo niet, in het technische profiel van de validatie van een lokaal account maakt en retourneert de object-id, authenticationSource, nieuwegebruiker. De `SelfAsserted-LocalAccountSignin-Email` technisch profiel aanroepen de `login-NonInteractive` technisch profiel van de validatie van de gebruikersreferenties te valideren.

U kunt ook het technische profiel van een REST-API aanroept met uw zakelijke logica, invoerclaims overschrijven of gebruikersgegevens verrijken door de verdere integratie met zakelijke line-of-business-toepassing. Zie voor meer informatie, [validatie technisch profiel](validation-technical-profile.md)

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| setting.showContinueButton | Nee | Geeft de knop Doorgaan. Mogelijke waarden: `true` (standaard), of `false` |
| setting.showCancelButton | Nee | Geeft de knop Annuleren. Mogelijke waarden: `true` (standaard), of `false` |
| setting.operatingMode | Nee | Voor een aanmeldingspagina bepaalt deze eigenschap het gedrag van het veld gebruikersnaam, zoals validatie voor invoer- en foutberichten. Verwachte waarden: `Username` of `Email`. |
| ContentDefinitionReferenceId | Ja | De id van de [inhoud definitie](contentdefinitions.md) die zijn gekoppeld aan dit technisch profiel. |
| EnforceEmailVerification | Nee | Voor aanmelding bij of profiel bewerken, e-mailverificatie wordt afgedwongen. Mogelijke waarden: `true` (standaard), of `false`. | 
| setting.showSignupLink | Nee | De knop Aanmelden weergegeven. Mogelijke waarden: `true` (standaard), of `false` |
| setting.retryLimit | Nee | Hiermee bepaalt u het aantal keren dat die een gebruiker proberen kan om de gegevens die wordt vergeleken met het technische profiel van een validatie te bieden. Bijvoorbeeld, probeert een gebruiker om u te registreren met een account dat bestaat al en wordt geprobeerd totdat de limiet bereikt.
| SignUpTarget | Nee | De aanmelding doel exchange-id. Wanneer de gebruiker op de knop aanmelden klikt, wordt de opgegeven exchange-id in Azure AD B2C uitgevoerd. |

## <a name="cryptographic-keys"></a>Cryptografische sleutels

De **CryptographicKeys** element wordt niet gebruikt.













