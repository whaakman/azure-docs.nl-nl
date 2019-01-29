---
title: Een technisch profiel van Claims transformatie definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer een Claims transformatie technisch profiel in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: a204e8cdc20a6897c40d4d5f68217a2922371737
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55148605"
---
# <a name="define-a-claims-transformation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een technisch profiel van claims transformatie in een aangepast Azure Active Directory B2C-beleid definiëren

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Een technisch profiel van claims transformatie kunt u aan te roepen uitvoerclaims transformaties te manipuleren van waarden van claims of standaardwaarden instellen voor een set uitvoerclaims valideren van claims.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `Proprietary`. De **handler** kenmerk mag de volledig gekwalificeerde naam van de handler-assembly voor protocol dat wordt gebruikt door Azure AD B2C: `Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Het volgende voorbeeld ziet u een claims transformatie technisch profiel:

```XML
<TechnicalProfile Id="Facebook-OAUTH-UnLink">
    <DisplayName>Unlink Facebook</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="output-claims"></a>Gebruikersclaims

De **OutputClaims** element is verplicht. U dient ten minste één uitvoergegevensset claim die wordt geretourneerd door het technische profiel. Het volgende voorbeeld laat zien hoe kunt u standaardwaarden instellen in de uitvoerclaims:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="ageGroup" DefaultValue="Undefined" />
  <OutputClaim ClaimTypeReferenceId="ageGroupValueChanged" DefaultValue="false" />
</OutputClaims>
```

## <a name="output-claims-transformations"></a>Uitvoer claims transformaties

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van claims of nieuwe labels te genereren. De volgende technische profiel-aanroepen de **RemoveAlternativeSecurityIdByIdentityProvider** transformatie claims. Deze transformatie-verwijdert een sociale identificeren van de verzameling van vorderingen **AlternativeSecurityIds**. Zijn de uitvoerclaims van deze technische profiel **identityProvider2**, die is ingesteld op `facebook.com`, en **AlternativeSecurityIds**, die de lijst met sociale identiteiten die zijn gekoppeld aan deze bevat gebruiker nadat facebook.com identiteit is verwijderd.

```XML
<ClaimsTransformations>
  <ClaimsTransformation Id="RemoveAlternativeSecurityIdByIdentityProvider"
TransformationMethod="RemoveAlternativeSecurityIdByIdentityProvider">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="IdentityProvider2"
TransformationClaimType="identityProvider" />
      <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds"
TransformationClaimType="collection" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds"
TransformationClaimType="collection" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
...
<TechnicalProfile Id="Facebook-OAUTH-UnLink">
    <DisplayName>Unlink Facebook</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider2" DefaultValue="facebook.com" AlwaysUseDefaultValue="true" />
    </OutputClaims>
    <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="RemoveAlternativeSecurityIdByIdentityProvider" />
    </OutputClaimsTransformations>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

Het technische profiel van claims transformatie kunt u een claimtransformatie van een gebruikersbeleving indelingsstap uitvoeren. In het volgende voorbeeld wordt de indelingsstap roept een van de technische profielen ontkoppelen zoals **ontkoppelen-Facebook-OAUTH**. In dit technisch profiel roept de claims transformatie technisch profiel **RemoveAlternativeSecurityIdByIdentityProvider**, die een nieuwe genereert **AlternativeSecurityIds2** claim met de lijst met sociale identiteiten voor gebruikers, bij het verwijderen van de Facebook-identiteit van de verzamelingen.

```XML
<UserJourney Id="AccountUnLink">
  <OrchestrationSteps>
    ...
    <OrchestrationStep Order="8" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="UnLinkFacebookExchange" TechnicalProfileReferenceId="UnLink-Facebook-OAUTH" />
        <ClaimsExchange Id="UnLinkMicrosoftExchange" TechnicalProfileReferenceId="UnLink-Microsoft-OAUTH" />
        <ClaimsExchange Id="UnLinkGitHubExchange" TechnicalProfileReferenceId="UnLink-GitHub-OAUTH" />
      </ClaimsExchanges>
    </OrchestrationStep>
    ...
  </OrchestrationSteps>
</UserJourney>
```

## <a name="use-a-validation-technical-profile"></a>Een technisch validatieprofiel gebruiken

Een claims transformatie technisch profiel kan worden gebruikt om informatie te valideren. In het volgende voorbeeld wordt de [bevestigde technisch profiel](self-asserted-technical-profile.md) met de naam **LocalAccountSignUpWithLogonEmail** vraagt de gebruiker gevraagd het e-mailbericht twee keer, en vervolgens roept de [technische validatie profiel](validation-technical-profile.md) met de naam **valideren-e-mailbericht** voor het valideren van de e-mailberichten. De **valideren-e-mailbericht** technisch profiel roept de claimtransformatie **AssertEmailAreEqual** om te vergelijken van de twee claims **e** en **emailRepeat** , en een uitzondering genereert als dat niet gelijk zijn aan op basis van de opgegeven vergelijking.

```XML
<ClaimsTransformations>
  <ClaimsTransformation Id="AssertEmailAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
      <InputClaim ClaimTypeReferenceId="emailRepeat" TransformationClaimType="inputClaim2" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    </InputParameters>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Het aanroepen van de claims transformatie technisch profiel de **AssertEmailAreEqual** claims transformatie worden bevestigd met dat e-mailberichten opgegeven door de gebruiker hetzelfde zijn.

```XML
<TechnicalProfile Id="Validate-Email">
  <DisplayName>Unlink Facebook</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailRepeat" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
  </OutputClaims>
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAreEqual" />
  </OutputClaimsTransformations>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

Een door zelf bevestigde technisch profiel kan aanroepen van het technische validatieprofiel en het foutbericht wordt weergegeven zoals opgegeven in de **UserMessageIfClaimsTransformationStringsAreNotEqual** metagegevens.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>User ID signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    ...
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">The email addresses you provided are not the same</Item>
  </Metadata>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="emailRepeat" />
    ...
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="Validate-Email" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```
