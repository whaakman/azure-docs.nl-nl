---
title: Configureren van de complexiteit van wachtwoorden met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de vereisten voor wachtwoordcomplexiteit met behulp van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: de9da180b3a09ecd09c5541a6608653099b4d8b0
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54846894"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Configureren van de complexiteit van wachtwoorden met behulp van aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In Azure Active Directory (Azure AD) B2C, kunt u de complexiteitsvereisten voldoen voor wachtwoorden die worden geleverd door een gebruiker bij het maken van een account configureren. Azure AD B2C gebruikt standaard **sterke** wachtwoorden. In dit artikel leest u hoe het configureren van de complexiteit van wachtwoorden in [aangepast beleid](active-directory-b2c-overview-custom.md). Het is ook mogelijk om te configureren van de complexiteit van wachtwoorden in [gebruikersstromen](active-directory-b2c-reference-password-complexity.md).

## <a name="prerequisites"></a>Vereisten

Voer de stappen in [aan de slag met aangepaste beleidsregels in Active Directory B2C](active-directory-b2c-get-started-custom.md).

## <a name="add-the-elements"></a>De elementen toe te voegen

1. Kopieer de *SignUpOrSignIn.xml* bestand dat u met de beginnerspakket gedownload en geef deze de naam *SingUpOrSignInPasswordComplexity.xml*.
2. Open de *SingUpOrSignInPasswordComplexity.xml* bestand en wijzig de **PolicyId** en de **PublicPolicyUri** naar een nieuwe naam. Bijvoorbeeld, *B2C_1A_signup_signin_password_complexity*.
3. Voeg de volgende **ClaimType** elementen met de id's van `newPassword` en `reenterPassword`:

    ```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    ```

4. [Predikaten](predicates.md) methode typen hebben `IsLengthRange` of `MatchesRegex`. De `MatchesRegex` type wordt gebruikt om een reguliere expressie. De `IsLengthRange` type heeft een minimale en maximale tekenreekslengte. Toevoegen een **predikaten** element op de **BuildingBlocks** element als deze niet met de volgende bestaat **predicaat** elementen:

    ```XML
    <Predicates>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    ```

5. Elke **InputValidation** element wordt samengesteld met behulp van de gedefinieerde **predicaat** elementen. Dit element kunt u Booleaanse aggregatiefuncties die vergelijkbaar met zijn `and` en `or`. Voeg een **InputValidations** element op de **BuildingBlocks** element als deze niet met de volgende bestaat **InputValidation** element:

    ```XML
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
    </InputValidations>
    ```

6. Zorg ervoor dat de **PolicyProfile** technisch profiel bevat de volgende elementen:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn"/>
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect"/>
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
        </InputClaims>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="displayName"/>
          <OutputClaim ClaimTypeReferenceId="givenName"/>
          <OutputClaim ClaimTypeReferenceId="surname"/>
          <OutputClaim ClaimTypeReferenceId="email"/>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub"/>
      </TechnicalProfile>
    </RelyingParty>
    ```

7. Sla het beleidsbestand.

## <a name="test-your-policy"></a>Het beleid testen

Bij het testen van uw toepassingen in Azure AD B2C, kan het nuttig zijn om de Azure AD B2C-token dat is geretourneerd naar `https://jwt.ms` om te kunnen controleren van de claims in het.

### <a name="upload-the-files"></a>De bestanden uploaden

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer **Identity-Ervaringsframework**.
5. Klik op de pagina aangepast beleid **uploaden beleid**.
6. Selecteer **het beleid overschrijven als deze bestaat**, en zoek en selecteer de *SingUpOrSignInPasswordComplexity.xml* bestand.
7. Klik op **Uploaden**.

### <a name="run-the-policy"></a>Het beleid uitvoeren

1. Open het beleid dat u hebt gewijzigd. Bijvoorbeeld, *B2C_1A_signup_signin_password_complexity*.
2. Voor **toepassing**, selecteer uw toepassing die u eerder hebt geregistreerd. Om te zien van het token wordt de **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **Nu uitvoeren**.
4. Selecteer **Meld u nu**, voer een e-mailadres en een nieuw wachtwoord invoeren. Richtlijnen wordt voor wachtwoordbeperkingen weergegeven. De gebruikersinformatie ingevoerd en klik vervolgens op **maken**. Hier ziet u de inhoud van het token dat is geretourneerd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [configureren met behulp van aangepaste beleidsregels in Azure Active Directory B2C wachtwoordwijziging](active-directory-b2c-reference-password-change-custom.md).


