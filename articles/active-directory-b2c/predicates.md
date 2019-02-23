---
title: Predikaten en PredicateValidations - Azure Active Directory B2C | Microsoft Docs
description: Sociaal account claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 360fd8e7ab0f7a85dbeed2bdbc7da379cbcfe91a
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737022"
---
# <a name="predicates-and-predicatevalidations"></a>Predikaten en PredicateValidations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De **predikaten** en **PredicateValidations** elementen kunnen u een validatieproces om ervoor te zorgen dat alleen juist opgemaakte gegevens worden ingevoerd in uw Azure Active Directory (Azure AD) B2C-tenant .  

Het volgende diagram toont de relatie tussen de elementen:  

![Predicaten](./media/predicates/predicates.png)

## <a name="predicates"></a>Predicaten  

De **predicaat** element definieert een basisvalidatie om te controleren of de waarde van een claimtype en retourneert `true` of `false`. De validatie wordt uitgevoerd met behulp van een opgegeven **methode** -element en een set **Parameter** elementen die relevant zijn voor de methode. Een predikaat kunt bijvoorbeeld controleren of de lengte van de waarde van een tekenreeks-claim wordt binnen het bereik van de minimale en maximale parameters die zijn opgegeven en of de waarde van een tekenreeks-claim bevat een tekenset. De **UserHelpText** element biedt een foutbericht weergegeven voor gebruikers als de controle is mislukt. De waarde van **UserHelpText** element kan worden gelokaliseerd met behulp van [aanpassing van taal](localization.md).

De **predikaten** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| Predicaat | 1: n | Een lijst met predicaten. | 

De **predicaat** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor het predicaat. Andere elementen kunt deze id in het beleid. |
| Methode | Ja | De methode dat moet worden gebruikt voor validatie. Mogelijke waarden: **IsLengthRange**, **MatchesRegex**, **IncludesCharacters**, of **IsDateRange**. De **IsLengthRange** waarde wordt gecontroleerd of de lengte van de waarde van een tekenreeks-claim binnen het bereik van de minimale en maximale parameters opgegeven is. De **MatchesRegex** waarde wordt gecontroleerd of de waarde van een tekenreeks-claim overeenkomt met een reguliere expressie. De **IncludesCharacters** waarde gecontroleerd of de waarde van een tekenreeks-claim een tekenset bevat. De **IsDateRange** waarde wordt gecontroleerd of de waarde van een datum-claim tussen een scala aan de minimale en maximale parameters die zijn opgegeven is. |

De **predicaat** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| UserHelpText | 1:1 | Een foutbericht weergegeven voor gebruikers als de controle is mislukt. Deze tekenreeks kan worden gelokaliseerd met behulp van de [taalaanpassing](localization.md) |
| Parameters | 1:1 | De parameters voor het type van de validatie van de tekenreeks. | 

De **Parameters** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| Parameter | 1: n | De parameters voor het type van de validatie van de tekenreeks. | 

De **Parameter** element bevat de volgende kenmerken:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| Id | 1:1 | De id van de parameter. |

Het volgende voorbeeld wordt een `IsLengthRange` methode met de parameters `Minimum` en `Maximum` die het bereik van de lengte van de tekenreeks opgeven:

```XML
<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  <UserHelpText>The password must be between 8 and 64 characters.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
      <Parameter Id="Maximum">64</Parameter>
  </Parameters>
</Predicate>
```

Het volgende voorbeeld wordt een `MatchesRegex` methode met de parameter `RegularExpression` die aangeeft dat een reguliere expressie:

```XML
<Predicate Id="PIN" Method="MatchesRegex">
  <UserHelpText>The password must be numbers only.</UserHelpText>
  <Parameters>
    <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
  </Parameters>
</Predicate>
```

Het volgende voorbeeld wordt een `IncludesCharacters` methode met de parameter `CharacterSet` die de set tekens bevat:

```XML
<Predicate Id="Lowercase" Method="IncludesCharacters">
  <UserHelpText>a lowercase letter</UserHelpText>
  <Parameters>
    <Parameter Id="CharacterSet">a-z</Parameter>
  </Parameters>
</Predicate>
```

Het volgende voorbeeld wordt een `IsDateRange` methode met de parameters `Minimum` en `Maximum` die het datumbereik opgeven met een indeling van `yyyy-MM-dd` en `Today`.

```XML
<Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 1970-01-01 and today.">
  <Parameters>
    <Parameter Id="Minimum">1970-01-01</Parameter>
    <Parameter Id="Maximum">Today</Parameter>
  </Parameters>
</Predicate>
```

## <a name="predicatevalidations"></a>PredicateValidations 

Terwijl de predicaten definiëren de validatie om te controleren op basis van een claimtype de **PredicateValidations** groep van een set predicaten om te vormen van een gebruiker Invoervalidatie die kan worden toegepast op een claimtype. Elke **PredicateValidation** element bevat een set **PredicateGroup** elementen die een set bevatten **PredicateReference** elementen die verwijst naar een **Predicaat**. Als u wilt de validatie worden gehaald, de waarde van de claim moet worden verwerkt alle tests van een predicaat onder alle van de **PredicateGroup** met hun set **PredicateReference** elementen.

```XML
<PredicateValidations>
  <PredicateValidation Id="">
    <PredicateGroups>
      <PredicateGroup Id="">
        <UserHelpText></UserHelpText>
        <PredicateReferences MatchAtLeast="">
          <PredicateReference Id="" />
          ...
        </PredicateReferences>
      </PredicateGroup>
      ...
    </PredicateGroups>
  </PredicateValidation>
...
</PredicateValidations>
```

De **PredicateValidations** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| PredicateValidation | 1: n | Een lijst van predikaat validatie. | 

De **PredicateValidation** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor de validatie van het predicaat. De **ClaimType** element gebruikt deze id in het beleid. |

De **PredicateValidation** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| PredicateGroups | 1: n | Een lijst van predikaat groepen. | 

De **PredicateGroups** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| PredicateGroup | 1: n | Een lijst met predicaten. | 

De **PredicateGroup** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor het predicaat groep.  |

De **PredicateGroup** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| UserHelpText | 1:1 |  Een beschrijving van het predikaat die kan nuttig zijn voor gebruikers om te weten welke waarde ze moeten typen. | 
| PredicateReferences | 1: n | Een lijst met predicaatverwijzingen. | 

De **PredicateReferences** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| MatchAtLeast | Nee | Hiermee geeft u op dat de waarde moet overeenkomen met ten minste dat veel definities voor de invoer predikaat moet zijn geaccepteerd. |

De **PredicateReferences** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| PredicateReference | 1: n | Een verwijzing naar een predicaat. | 

De **PredicateReference** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor de validatie van het predicaat.  |


## <a name="configure-password-complexity"></a>Wachtwoordcomplexiteit configureren

Met **predikaten** en **PredicateValidationsInput** u de complexiteitsvereisten voldoen voor wachtwoorden die worden geleverd door een gebruiker bij het maken van een account kunt beheren. Azure AD B2C maakt standaard gebruik van sterke wachtwoorden. Azure AD B2C biedt ook ondersteuning voor configuratieopties voor het beheren van de complexiteit van wachtwoorden die klanten kunnen gebruiken. U kunt wachtwoordcomplexiteit definiëren met behulp van deze predicaat elementen: 

- **IsLengthBetween8And64** met behulp van de `IsLengthRange` methode, valideert u dat het wachtwoord moet 8 tot 64 tekens.
- **Kleine letters** met behulp van de `IncludesCharacters` methode, valideert u dat het wachtwoord bevat een kleine letter.
- **Hoofdletters** met behulp van de `IncludesCharacters` methode, valideert u dat het wachtwoord bevat een hoofdletter.
- **Aantal** met behulp van de `IncludesCharacters` methode, valideert u dat het wachtwoord bevat een cijfer.
- **Symbool** met behulp van de `IncludesCharacters` methode, valideert u dat het wachtwoord bevat een van de volgende symbolen `@#$%^&*\-_+=[]{}|\:',?/~"();!`
- **PINCODE** met behulp van de `MatchesRegex` methode, valideert u dat het wachtwoord alleen getallen bevat.
- **AllowedAADCharacters** met behulp van de `MatchesRegex` methode, valideert u dat het wachtwoord alleen ongeldig teken is opgegeven.
- **DisallowedWhitespace** met behulp van de `MatchesRegex` methode, valideert dat het wachtwoord niet beginnen of eindigen met een spatie.

```XML
<Predicates>
  <Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
    <UserHelpText>The password must be between 8 and 64 characters.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
      <Parameter Id="Maximum">64</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Lowercase" Method="IncludesCharacters">
    <UserHelpText>a lowercase letter</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">a-z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Uppercase" Method="IncludesCharacters">
    <UserHelpText>an uppercase letter</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">A-Z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Number" Method="IncludesCharacters">
    <UserHelpText>a digit</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">0-9</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Symbol" Method="IncludesCharacters">
    <UserHelpText>a symbol</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\:',?/`~"();!</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="PIN" Method="MatchesRegex">
    <UserHelpText>The password must be numbers only.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="AllowedAADCharacters" Method="MatchesRegex">
    <UserHelpText>An invalid character was provided.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">(^([0-9A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~"();! ]|(\.(?!@)))+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="DisallowedWhitespace" Method="MatchesRegex">
    <UserHelpText>The password must not begin or end with a whitespace character.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">(^\S.*\S$)|(^\S+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>
```

Nadat u de eenvoudige validaties gedefinieerd, kunt u ze samen combineren en een set van wachtwoordbeleidsregels die u in uw beleid gebruiken kunt maken:

- **SimplePassword** valideert de DisallowedWhitespace, AllowedAADCharacters en IsLengthBetween8And64
- **StrongPassword** de IsLengthBetween8And64 DisallowedWhitespace, AllowedAADCharacters, valideert. De laatste groep `CharacterClasses` wordt uitgevoerd een extra set predikaten met `MatchAtLeast` ingesteld op 3. Het wachtwoord moet tussen 8 en 16 tekens lang en drie van de volgende tekens bevatten: Kleine letters, hoofdletters, getal of symbool.
- **CustomPassword** alleen DisallowedWhitespace, AllowedAADCharacters valideert. Gebruiker kan dus een wachtwoord met een lengte opgeven, zolang de tekens geldig zijn.

```XML
<PredicateValidations>
  <PredicateValidation Id="SimplePassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="StrongPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
       </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="CharacterClasses">
        <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
        <PredicateReferences MatchAtLeast="3">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Voeg in het claimtype, de **PredicateValidationReference** element en geeft u de id als een van de predicaat validaties, zoals SimplePassword, StrongPassword of CustomPassword.

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>Enter password</AdminHelpText>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
  <PredicateValidationReference Id="StrongPassword" />
</ClaimType>
```

Hieronder ziet u hoe de elementen zijn ingedeeld als Azure AD B2C een foutmelding weergegeven wordt:

![Predikaat proces](./media/predicates/predicates-pass.png)

## <a name="configure-a-date-range"></a>Configureren van een bepaalde periode

Met de **predikaten** en **PredicateValidations** elementen kunt u de minimale en maximale datumwaarden van bepalen de **UserInputType** met behulp van een `DateTimeDropdown`. Om dit te doen, maakt u een **predicaat** met de `IsDateRange` methode en de minimale en maximale parameters.

```XML
<Predicates>
  <Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 01-01-1980 and today.">
    <Parameters>
      <Parameter Id="Minimum">1980-01-01</Parameter>
      <Parameter Id="Maximum">Today</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

Voeg een **PredicateValidation** met een verwijzing naar de `DateRange` predicaat.

```XML
<PredicateValidations>
  <PredicateValidation Id="CustomDateRange">
    <PredicateGroups>
      <PredicateGroup Id="DateRangeGroup">
        <PredicateReferences>
          <PredicateReference Id="DateRange" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Voeg in uw claimtype **PredicateValidationReference** element en geeft u de id van `CustomDateRange`. 
    
```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date of Birth</DisplayName>
  <DataType>date</DataType>
  <AdminHelpText>The user's date of birth.</AdminHelpText>
  <UserHelpText>Your date of birth.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
  <PredicateValidationReference Id="CustomDateRange" />
</ClaimType>
 ```
