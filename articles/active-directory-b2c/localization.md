---
title: Lokalisatie - Azure Active Directory B2C | Microsoft Docs
description: Geef de lokalisatie-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 8f252b536c80ad997f3c0eb10b10d5cb8c330fc6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187552"
---
# <a name="localization"></a>Lokalisatie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De **lokalisatie** element kunt u ter ondersteuning van meerdere landinstellingen of talen in het beleid voor de gebruiker reizen. De lokalisatieondersteuning in het beleid kunt u:

- Stel de expliciete lijst van de ondersteunde talen in een beleid en kies een taal.
- Geef taalspecifieke tekenreeksen en verzamelingen.

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

De **lokalisatie** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Ingeschakeld | Nee | Mogelijke waarden: `true` of `false`. |

De **lokalisatie** element bevat een XML-elementen

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1: n | Lijst met ondersteunde talen. | 
| LocalizedResources | 0: n | Lijst met gelokaliseerde resources. |

## <a name="supportedlanguages"></a>SupportedLanguages

De **SupportedLanguages** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Standaardtaal | Ja | De taal die moet worden gebruikt als de standaardlocatie voor gelokaliseerde bronnen. |
| MergeBehavior | Nee | De opsommingswaarden van een van de waarden die worden samengevoegd, samen met eventuele ClaimType aanwezig zijn in een bovenliggende beleid met dezelfde id. Dit kenmerk gebruiken wanneer u een claim die is opgegeven in het Basisbeleid overschrijven. Mogelijke waarden: `Append`, `Prepend`, of `ReplaceAll`. De `Append` waarde geeft aan dat het verzamelen van gegevens aanwezig moet worden toegevoegd aan het einde van de verzameling die is opgegeven in het bovenliggende beleid. De `Prepend` waarde geeft aan dat het verzamelen van gegevens aanwezig zijn voordat u de verzameling die is opgegeven in het bovenliggende beleid moet worden toegevoegd. De `ReplaceAll` waarde geeft aan dat het verzamelen van gegevens die zijn gedefinieerd in het bovenliggende beleid moet worden genegeerd, in plaats daarvan de gegevens die zijn gedefinieerd in het huidige beleid gebruiken. |

### <a name="supportedlanguages"></a>SupportedLanguages

De **SupportedLanguages** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1: n | Inhoud weergeven die aan een taalcode per RFC 5646 - Tags voor het identificeren van talen voldoet. | 

## <a name="localizedresources"></a>LocalizedResources

De **LocalizedResources** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor het aanduiden van gelokaliseerde bronnen. |

De **LocalizedResources** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0: n | Definieert de hele verzamelingen in verschillende culturen. Een verzameling kan verschillende aantal items en andere tekenreeksen voor verschillende culturen hebben. Voorbeelden van verzamelingen zijn de inventarisaties die worden weergegeven in de claimtypen. Bijvoorbeeld, wordt een land/regio-lijst weergegeven voor de gebruiker in een vervolgkeuzelijst. |
| LocalizedStrings | 0: n | Hiermee definieert u alle van de tekenreeksen die, met uitzondering van de tekenreeksen die worden weergegeven in verzamelingen, in verschillende culturen. |

### <a name="localizedcollections"></a>LocalizedCollections

De **LocalizedCollections** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1: n | Lijst met ondersteunde talen. |

#### <a name="localizedcollection"></a>LocalizedCollection

De **LocalizedCollection** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| ElementType | Ja | Verwijst naar een ClaimType-element of een element van de gebruikersinterface in het beleid-bestand. |
| ElementId | Ja | Een tekenreeks met een verwijzing naar een claimtype al gedefinieerd in de sectie ClaimsSchema dat wordt gebruikt als **ElementType** is ingesteld op een ClaimType. |
| TargetCollection | Ja | De doelverzameling. |

De **LocalizedCollection** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| Item | 0: n | Hiermee definieert u een beschikbare optie voor de gebruiker selecteert voor een claim in de gebruikersinterface, zoals een waarde in een vervolgkeuzelijst. |

De **Item** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Tekst | Ja | De gebruiksvriendelijke weergavereeks die de gebruiker in de gebruikersinterface voor deze optie moet worden weergegeven. |
| Value | Ja | De tekenreeks claim waarde die is gekoppeld met deze optie selecteert. |

Het volgende voorbeeld ziet u het gebruik van de **LocalizedCollections** element. Deze bevat twee **LocalizedCollection** elementen, één voor Engels en een andere naam voor het Spaans. Beide ingesteld de **beperking** verzameling van de claim `Gender` met een lijst met items voor het Engels en Spaans.

```XML
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

```

### <a name="localizedstrings"></a>LocalizedStrings

De **LocalizedStrings** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| LocalizedString | 1: n | Een gelokaliseerde tekenreeks. |

De **LocalizedString** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| ElementType | Ja | Een verwijzing naar een element van het type claim of een element van de gebruikersinterface in het beleid. Mogelijke waarden: `ClaimType`, `UxElement`, `ErrorMessage`, `Predicate`, of. De `ClaimType` waarde wordt gebruikt om een van de claim-kenmerken, zoals opgegeven in de StringId lokaliseren. De `UxElement` waarde wordt gebruikt om een van de gebruikersinterface-elementen die zijn opgegeven in de StringId lokaliseren. De `ErrorMessage` waarde wordt gebruikt om een van de foutberichten systeem zoals opgegeven in de StringId lokaliseren. De `Predicate` waarde wordt gebruikt voor het lokaliseren van een van de [predicaat](predicates.md) foutberichten worden weergegeven, zoals opgegeven in de StringId. De `InputValidation` waarde wordt gebruikt voor het lokaliseren van een van de [PredicateValidation](predicates.md) foutberichten die zijn opgegeven in de StringId groep. |
| ElementId | Ja | Als **ElementType** is ingesteld op `ClaimType`, `Predicate`, of `InputValidation`, dit element bevat een verwijzing naar een claimtype al gedefinieerd in de sectie ClaimsSchema. | 
| StringId | Ja | Als **ElementType** is ingesteld op `ClaimType`, dit element bevat een verwijzing naar een kenmerk van een claimtype. Mogelijke waarden: `DisplayName`, `AdminHelpText`, of `PatternHelpText`. De `DisplayName` waarde wordt gebruikt voor het instellen van de weergavenaam van de claim. De `AdminHelpText` waarde wordt gebruikt om de naam van het help-tekst van de claim-gebruiker ingesteld. De `PatternHelpText` waarde wordt gebruikt voor het instellen van de claim patroon help-tekst. Als **ElementType** is ingesteld op `UxElement`, dit element bevat een verwijzing naar een kenmerk van een element van de gebruikersinterface. Als **ElementType** is ingesteld op `ErrorMessage`, dit element Hiermee geeft u de id van een foutbericht weergegeven. Zie [gelokaliseerde tekenreeks id's](localization-string-ids.md) voor een volledige lijst van de `UxElement` id's.|


Het volgende voorbeeld ziet een gelokaliseerde pagina voor het registreren. De eerste drie **LocalizedString** waarden stelt u het kenmerk claim. De derde verandert de waarde van de knop Doorgaan. Het laatste item wijzigt het foutbericht.

```XML
<LocalizedResources Id="api.selfasserted.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

Het volgende voorbeeld ziet u een gelokaliseerde de **UserHelpText** van **predicaat** met Id `IsLengthBetween8And64`. En een gelokaliseerde **UserHelpText** van **PredicateGroup** met Id `CharacterClasses` van **PredicateValidation** met Id `StrongPassword`.

```XML
<PredicateValidation Id="StrongPassword">
  <PredicateGroups>
    ...
    <PredicateGroup Id="CharacterClasses">
    ...
    </PredicateGroup>
  </PredicateGroups>
</PredicateValidation>

...

<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  ...
</Predicate>
...


<LocalizedString ElementType="InputValidation" ElementId="StrongPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>

<LocalizedString ElementType="Predicate" ElementId="IsLengthBetween8And64" StringId="HelpText">The password must be between 8 and 64 characters.</LocalizedString>              
```

## <a name="set-up-localization"></a>Lokalisatie instellen

In dit artikel wordt beschreven hoe u ter ondersteuning van meerdere landinstellingen of talen in het beleid voor gebruikers reizen. Lokalisatie zijn drie stappen vereist: de expliciete lijst van de ondersteunde talen taalspecifieke tekenreeksen en verzamelingen bieden, en de ContentDefinition voor de pagina bewerken.

### <a name="set-up-the-explicit-list-of-supported-languages"></a>Instellen van de expliciete lijst van ondersteunde talen

Onder de **BuildingBlocks** element toevoegen de **lokalisatie** element met de lijst met ondersteunde talen. Het volgende voorbeeld laat zien hoe de lokalisatieondersteuning voor zowel Engels (standaard) en Spaans definiëren:

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
</Localization>
```

### <a name="provide-language-specific-strings-and-collections"></a>Taalspecifieke tekenreeksen en verzamelingen bieden 

Voeg **LocalizedResources** elementen binnen de **lokalisatie** element na het sluiten van de **SupportedLanguages** element. U toevoegen **LocalizedResources** -elementen voor elke pagina (inhoudsdefinitie) en elke taal die u wilt ondersteunen. Voor het aanpassen van de uniforme pagina voor registreren of aanmelden, meld u aan en meervoudige verificatie (MFA) pagina's voor het Engels, Spaans en Frankrijk, voeg u de volgende **LocalizedResources** elementen.  
- Uniforme pagina voor registreren of aanmelden, Engels `<LocalizedResources Id="api.signuporsignin.en">`
- Meld u aan of aanmelden pagina Unified Spaans `<LocalizedResources Id="api.signuporsignin.es">`
- Uniforme pagina voor registreren of aanmelden, Frankrijk `<LocalizedResources Id="api.signuporsignin.fr">` 
- Meld u aan, Engels `<LocalizedResources Id="api.localaccountsignup.en">`
- Meld u aan, Spaans `<LocalizedResources Id="api.localaccountsignup.es">`
- Meld u aan, Frankrijk `<LocalizedResources Id="api.localaccountsignup.fr">`
- MFA, Engels `<LocalizedResources Id="api.phonefactor.en">`
- MFA, Spaans `<LocalizedResources Id="api.phonefactor.es">`
- MFA, Frankrijk `<LocalizedResources Id="api.phonefactor.fr">`

Elke **LocalizedResources** element bevat alle van de vereiste **LocalizedStrings** elementen met meerdere **LocalizedString** elementen en  **LocalizedCollections** elementen met meerdere **LocalizedCollection** elementen.  Het volgende voorbeeld wordt de Engelse lokalisatie van de pagina voor het registreren: 

Opmerking: In dit voorbeeld wordt een verwijzing naar `Gender` en `City` claimtypen. Zorg dat u deze claims definiëren voor het gebruik van dit voorbeeld. Zie voor meer informatie, [ClaimsSchema](claimsschema.md).

```XML
<LocalizedResources Id="api.localaccountsignup.en">

 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
   <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
      <Item Text="New York" Value="NY" />
      <Item Text="Paris" Value="Paris" />
      <Item Text="London" Value="London" />
    </LocalizedCollection>
  </LocalizedCollections>

  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

De lokalisatie van de pagina voor het registreren voor Spaans.

```XML
<LocalizedResources Id="api.localaccountsignup.es">

 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
   <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
      <Item Text="Nueva York" Value="NY" />
      <Item Text="París" Value="Paris" />
      <Item Text="Londres" Value="London" />
    </LocalizedCollection>
  </LocalizedCollections>

  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Dirección de correo electrónico</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Dirección de correo electrónico que puede usarse para ponerse en contacto con usted.</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Introduzca una dirección de correo electrónico.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Crear</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">Ya existe un usuario con el id. especificado. Elija otro diferente.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

### <a name="edit-the-contentdefinition-for-the-page"></a>De ContentDefinition voor de pagina bewerken 

Voor elke pagina die u wilt om te lokaliseren, geeft u de taalcodes gezocht de **ContentDefinition**.

Engels (en) en Spaans (es) aangepaste tekenreeksen in het volgende voorbeeld worden toegevoegd aan de pagina voor het registreren. De **LocalizedResourcesReferenceId** voor elk **LocalizedResourcesReference** is hetzelfde als de landinstellingen, maar u kunt een willekeurige tekenreeks als de id. Voor elke combinatie van taal en een pagina die u verwijzen naar de bijbehorende **LocalizedResources** u eerder hebt gemaakt.

```XML
<ContentDefinition Id="api.localaccountsignup">
...
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
  </LocalizedResourcesReferences>
</ContentDefinition>
```

Het volgende voorbeeld ziet u het laatste XML-bestand:

```XML
<BuildingBlocks>
  <ContentDefinitions>
    <ContentDefinition Id="api.localaccountsignup">
      <!-- Other content definitions elements... -->
      <LocalizedResourcesReferences MergeBehavior="Prepend">
         <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
         <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
      </LocalizedResourcesReferences>
    </ContentDefinition>
    <!-- More content definitions... -->
  </ContentDefinitions>

  <Localization Enabled="true">

    <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
      <SupportedLanguage>en</SupportedLanguage>
      <SupportedLanguage>es</SupportedLanguage>
      <!-- More supported language... -->
    </SupportedLanguages>

    <LocalizedResources Id="api.localaccountsignup.en">
      <LocalizedCollections>
        <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
          <Item Text="Female" Value="F" />
          <Item Text="Male" Value="M" />
          <!-- More items... -->
        </LocalizedCollection>
        <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
          <Item Text="New York" Value="NY" />
          <Item Text="Paris" Value="Paris" />
          <Item Text="London" Value="London" />
        </LocalizedCollection>
        <!-- More localized collections... -->
      </LocalizedCollections>
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
      <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
       <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
        <!-- More localized strings... -->
      </LocalizedStrings>
    </LocalizedResources>

    <LocalizedResources Id="api.localaccountsignup.es">
      <LocalizedCollections>
       <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
          <Item Text="Femenino" Value="F" />
          <Item Text="Masculino" Value="M" />
        </LocalizedCollection>
        <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
          <Item Text="Nueva York" Value="NY" />
          <Item Text="París" Value="Paris" />
          <Item Text="Londres" Value="London" />
        </LocalizedCollection>
      </LocalizedCollections>
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Dirección de correo electrónico</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Dirección de correo electrónico que puede usarse para ponerse en contacto con usted.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Introduzca una dirección de correo electrónico.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_continue">Crear</LocalizedString>
      <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">Ya existe un usuario con el id. especificado. Elija otro diferente.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
    <!-- More localized resources... -->
  </Localization>
</BuildingBlocks>
```




