---
title: Wachtwoordcomplexiteit in het aangepaste beleid - Azure AD B2C | Microsoft Docs
description: Vereisten voor wachtwoordcomplexiteit voor wachtwoorden in aangepast beleid configureren
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: eb187a120399089f1c3c145a06fbe993f50fb92b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-password-complexity-in-custom-policies"></a>Wachtwoordcomplexiteit configureren in het aangepaste beleid

> [!NOTE]
> **Deze functie is in preview.**  Neem contact op met [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com) om uw testtenant met deze functie is ingeschakeld.  Niet testen dit op productie tenants.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel is een geavanceerde beschrijving van de werking van wachtwoordcomplexiteit en gebruik van Azure AD B2C aangepast beleid is ingeschakeld.

## <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: Complexiteitsvereisten voor wachtwoorden configureren

Azure Active Directory B2C (Azure AD B2C) ondersteunt de complexiteitsvereisten voor wachtwoorden die worden geleverd door een eindgebruiker bij het maken van een account te wijzigen.  Azure AD B2C gebruikt standaard **sterke** wachtwoorden.  Azure AD B2C ondersteunt ook configuratieopties voor het beheren van de complexiteit van wachtwoorden die klanten kunnen gebruiken.  In dit artikel wordt gesproken over het wachtwoordcomplexiteit in aangepast beleid configureren.  Het is ook mogelijk om te gebruiken [wachtwoordcomplexiteit configureren in het ingebouwde beleid](active-directory-b2c-reference-password-complexity.md).

## <a name="prerequisites"></a>Vereisten

Een Azure AD B2C-tenant die is geconfigureerd voor het voltooien van een lokaal account sign-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-complexity-in-custom-policy"></a>Wachtwoordcomplexiteit in aangepast beleid configureren

Als u wilt configureren wachtwoordcomplexiteit in aangepast beleid, de algemene structuur van het aangepaste beleid moet bevatten een `ClaimsSchema`, `Predicates`, en `InputValidations` -element in `BuildingBlocks`.

```XML
  <BuildingBlocks>
    <ClaimsSchema>...</ClaimsSchema>
    <Predicates>...</Predicates>
    <InputValidations>...</InputValidations>
  </BuildingBlocks>
```

Het doel van deze elementen is als volgt:

- Elke `Predicate` element definieert een eenvoudige tekenreeks validatiecontrole die waar of onwaar retourneert.
- De `InputValidations` element heeft een of meer `InputValidation` elementen.  Elke `InputValidation` is opgesteld met behulp van een reeks `Predicate` elementen. Dit element kunt u Booleaanse aggregaties uitvoeren (vergelijkbaar met `and` en `or`).
- De `ClaimsSchema` wordt gedefinieerd welke claim wordt gevalideerd.  Vervolgens definieert die `InputValidation` regel wordt gebruikt om te valideren dat claim.

### <a name="defining-a-predicate-element"></a>Een predikaat element definiëren

Predikaten hebben twee typen van de methode: IsLengthRange of MatchesRegex. We bekijken een voorbeeld van beide.  We hebben eerst een voorbeeld van MatchesRegex die overeenkomen met een reguliere expressie wordt gebruikt.  In dit voorbeeld overeenkomt met de tekenreeks met getallen.

```XML
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
```

Volgende bekijken we een voorbeeld van IsLengthRange.  Deze methode heeft een minimale en maximale tekenreekslengte.

```XML
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
```

Gebruik de `HelpText` kenmerk op te geven van een foutbericht voor eindgebruikers als de controle is mislukt.  Deze tekenreeks kan worden gelokaliseerd met behulp van de [functie voor het aanpassen van taal](active-directory-b2c-reference-language-customization.md).

### <a name="defining-an-inputvalidation-element"></a>Een element InputValidation definiëren

Een `InputValidation` is een samenvoeging van `PredicateReferences`. Elke `PredicateReferences` moet worden voldaan opdat de `InputValidation` mislukt.  Echter, in de `PredicateReferences` element gebruikt een kenmerk genaamd `MatchAtLeast` om op te geven hoeveel `PredicateReference` controles moeten true wordt geretourneerd.  Optioneel definiëren een `HelpText` kenmerk voor het onderdrukken van het foutbericht is gedefinieerd de `Predicate` elementen waarnaar wordt verwezen.

```XML
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
```

### <a name="defining-a-claimsschema-element"></a>Een element ClaimsSchema definiëren

De claimtypen `newPassword` en `reenterPassword` worden beschouwd als speciale, zodat de namen niet wijzigen.  De gebruikersinterface van de gebruiker valideert opnieuw het wachtwoord correct ingevoerd tijdens het maken van het account op basis van deze `ClaimType` elementen.  Dezelfde vinden `ClaimType` elementen, zoeken in de TrustFrameworkBase.xml in uw starter pack.  Wat is er nieuw in dit voorbeeld is dat we van deze elementen overschrijven voor het definiëren van een `InputValidationReference`. De `ID` kenmerk van dit nieuwe-element verwijst naar de `InputValidation` element dat is gedefinieerd.

```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
```

### <a name="putting-it-all-together"></a>Kort samengevat

Dit voorbeeld ziet u hoe de onderdelen in elkaar passen om een beleid werken.  In dit voorbeeld gebruiken:

1. Volg de instructies in de vereiste [aan de slag](active-directory-b2c-get-started-custom.md) om te downloaden, configureren en TrustFrameworkBase.xml en TrustFrameworkExtensions.xml uploaden
1. Maak een SignUporSignIn.xml-bestand met de inhoud van het voorbeeld in deze sectie.
1. Bijwerken SignUporSignIn.xml vervangen `yourtenant` met de naam van uw Azure AD B2C-tenant.
1. Het beleidsbestand SignUporSignIn.xml laatste uploaden.

In dit voorbeeld bevat een validatie voor wachtwoorden van de pincode en één voor sterke wachtwoorden:

- Zoek naar `PINpassword`. Dit `InputValidation` element valideert een pincode van een willekeurige lengte.  Dit wordt niet gebruikt op dit moment omdat deze niet wordt verwezen in de `InputValidationReference` -element in `ClaimType`. 
- Zoek naar `PasswordValidation`. Dit `InputValidation` element valideert een wachtwoord is 8 tot 16 tekens en bevat 3 4 van hoofdletters, kleine letters, cijfers of symbolen.  Ernaar wordt verwezen in `ClaimType`.  Deze regel wordt daarom in dit beleid wordt afgedwongen.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="yourtenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin">
 <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    <Predicates>
      <Predicate Id="Lowercase" Method="MatchesRegex" HelpText="a lowercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[a-z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="MatchesRegex" HelpText="an uppercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[A-Z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="MatchesRegex" HelpText="a number">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="MatchesRegex" HelpText="a symbol">
        <Parameters>
          <Parameter Id="RegularExpression">^[!@#$%^*()]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
      <InputValidation Id="PINpassword">
        <PredicateReferences Id="PINGroup">
          <PredicateReference Id="PIN" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
  </BuildingBlocks>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```
