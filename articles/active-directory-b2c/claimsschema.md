---
title: ClaimsSchema - Azure Active Directory B2C | Microsoft Docs
description: Geef het ClaimsSchema-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6c41890922e2235190d8844a573522846b42c779
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434497"
---
# <a name="claimsschema"></a>ClaimsSchema

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De **ClaimsSchema** element definieert de claimtypen aan die kunnen worden verwezen als onderdeel van het beleid. Claims schema is de plek waar u de claims declareren. Een claim mag de naam van de eerste, laatste naam, weergavenaam, telefoonnummer en meer. ClaimsSchema-element bevat een lijst met **ClaimType** elementen. De **ClaimType** element bevat de **Id** kenmerk, dit de claimnaam is. 

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="Id">
      <DisplayName>Surname</DisplayName>
      <DataType>string</DataType>
      <DefaultPartnerClaimTypes>
        <Protocol Name="OAuth2" PartnerClaimType="family_name" />
        <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
        <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
      </DefaultPartnerClaimTypes>
      <UserHelpText>Your surname (also known as family name or last name).</UserHelpText>
      <UserInputType>TextBox</UserInputType>
```

## <a name="claimtype"></a>ClaimType

De **ClaimType** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor het claimtype. Andere elementen kunt deze id in het beleid. |

De **ClaimType** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| Weergavenaam | 0:1 | De titel die wordt weergegeven voor gebruikers op verschillende schermen. De waarde kan zijn [gelokaliseerde](localization.md). |
| Gegevenstype | 0:1 | Het type van de claim. De gegevenstypen van Booleaanse waarde, datum, datum/tijd, int, long, string, stringCollection, alternativeSecurityIdCollection kan worden gebruikt. |
| DefaultPartnerClaimTypes | 0:1 | De standaardwaarde partner claim typen om te gebruiken voor een opgegeven protocol. De waarde kan worden overschreven de **PartnerClaimType** opgegeven in de **InputClaim** of **OutputClaim** elementen. Dit element gebruiken om op te geven van de standaardnaam voor een protocol.  |
| Masker | 0:1 | Een optionele tekenreeks van het maskeren van tekens die kunnen worden toegepast bij het weergeven van de claim. Het aantal 324-232-4343 telefoon kan bijvoorbeeld als XXX-XXX-4343 worden gemaskeerd. |
| UserHelpText | 0:1 | Een beschrijving van het claimtype die handig is voor gebruikers om te begrijpen van het doel zijn. De waarde kan zijn [gelokaliseerde](localization.md). |
| UserInputType | 0:1 | Het type besturingselement voor invoer die beschikbaar voor de gebruiker zijn moet bij het handmatig invoeren van de claimgegevens voor het claimtype. Zie de gebruiker invoertypen gedefinieerd verderop in deze pagina. |
| Beperking | 0:1 | De beperkingen van de waarde voor deze claim, zoals een reguliere expressie (reguliere expressie) of een lijst met geldige waarden. De waarde kan zijn [gelokaliseerde](localization.md). |
PredicateValidationReference| 0:1 | Een verwijzing naar een **PredicateValidationsInput** element. De **PredicateValidationReference** elementen kunnen u een validatieproces om ervoor te zorgen dat alleen juist opgemaakte gegevens wordt ingevoerd. Zie voor meer informatie, [predikaten](predicates.md). |

### <a name="defaultpartnerclaimtypes"></a>DefaultPartnerClaimTypes

De **DefaultPartnerClaimTypes** mag het volgende element:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| Protocol | 0: n | Lijst met protocollen met hun standaardpartner de naam van claim. |

De **Protocol** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Naam | Ja | De naam van een geldig protocol dat wordt ondersteund door Azure AD B2C. Mogelijke waarden zijn: OAuth1, OAuth2, SAML2, OpenIdConnect, WsFed of WsTrust. |
| PartnerClaimType | Ja | De naam van het claim moet worden gebruikt. |

In het volgende voorbeeld, wanneer de Identity-Ervaringsframework met een SAML2-id-provider of een relying party-toepassing communiceert, de **achternaam** claim is toegewezen aan `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`, met OpenIdConnect en OAuth2, de claim is toegewezen aan `family_name`.

```XML
<ClaimType Id="surname">
  <DisplayName>Surname</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OAuth2" PartnerClaimType="family_name" />
    <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
    <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
  </DefaultPartnerClaimTypes>
</ClaimType>
```

De JWT-token dat is uitgegeven door Azure AD B2C, als gevolg hiervan wordt weggelaten de `family_name` in plaats van ClaimType naam **achternaam**.
 
```JSON
{
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  "auth_time": 1535013501,
  "given_name": "David",
  "family_name": "Williams",
  "name": "David Williams",
}
```

### <a name="mask"></a>Masker

De **masker** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Type | Ja | Het type van het masker claim. Mogelijke waarden: `Simple` of `Regex`. De `Simple` waarde geeft aan dat een eenvoudige tekstmasker wordt toegepast op de toonaangevende gedeelte van een tekenreeks-claim. De `Regex` waarde geeft aan dat een reguliere expressie wordt toegepast op de claim tekenreeks als geheel.  Als de `Regex` waarde is opgegeven, wordt een optioneel kenmerk moet ook worden gedefinieerd met de reguliere expressie te gebruiken. |
| reguliere expressie | Nee | Als **Type** is ingesteld op `Regex`, geef de reguliere expressie te gebruiken.

Het voorbeeld follwing configureert u een **PhoneNumber** claim met de `Simple` masker:

```XML
<ClaimType Id="PhoneNumber">
  <DisplayName>Phone Number</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Simple">XXX-XXX-</Mask>  
  <UserHelpText>Your telephone number.</UserHelpText>
</ClaimType>
```

De Identity-Ervaringsframework wordt het telefoonnummer dat tijdens het verbergen van de eerste zes cijfers weergegeven:

![Met behulp van claimtype met masker](./media/claimsschema/mask.png)

Het voorbeeld follwing configureert u een **AlternateEmail** claim met de `Regex` masker:

```XML
<ClaimType Id="AlternateEmail">
  <DisplayName>Please verify the secondary email linked to your account</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Regex" Regex="(?&lt;=.).(?=.*@)">*</Mask>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```

De Identity-Ervaringsframework wordt alleen de eerste letter van het e-mailadres en de domeinnaam van het e-mailbericht weergegeven:

![Met behulp van claimtype met masker](./media/claimsschema/mask-regex.png)


### <a name="restriction"></a>Beperking

De **beperking** element mag het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| MergeBehavior | Nee | De methode die wordt gebruikt voor het samenvoegen van opsommingswaarden met een ClaimType in een bovenliggende beleid met dezelfde id. Gebruik dit kenmerk wanneer u een claim die is opgegeven in het Basisbeleid overschrijven. Mogelijke waarden: `Append`, `Prepend`, of `ReplaceAll`. De `Append` waarde is een verzameling van gegevens die moet worden toegevoegd aan het einde van de verzameling die is opgegeven in het bovenliggende beleid. De `Prepend` waarde is een verzameling van gegevens die moeten worden toegevoegd voordat u de verzameling die is opgegeven in het bovenliggende beleid. De `ReplaceAll` waarde is een verzameling van gegevens die zijn opgegeven in het bovenliggende beleid die moet worden genegeerd. |

De **beperking** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| Opsomming | 1: n | De beschikbare opties in de gebruikersinterface voor de gebruiker selecteert voor een claim, zoals een waarde in een vervolgkeuzelijst. |
| Patroon | 1:1 | De reguliere expressie te gebruiken. |

### <a name="enumeration"></a>Opsomming

De **opsomming** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Tekst | Ja | De tekenreeks die weergeven die aan de gebruiker in de gebruikersinterface voor deze optie wordt weergegeven. |
|Waarde | Ja | De waarde van de claim die is gekoppeld aan deze optie selecteert. |
| SelectByDefault | Nee | Geeft aan of deze optie standaard in de gebruikersinterface moet worden geselecteerd. Mogelijke waarden: True of False. |

Het volgende voorbeeld wordt een **plaats** vervolgkeuzelijst lijst claim met een standaardwaarde ingesteld op `New York`:

```XML
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```
Plaats vervolgkeuzelijst met een standaardwaarde ingesteld op New York:

![Vervolgkeuzelijst stad](./media/claimsschema/dropdownsingleselect.png)


### <a name="pattern"></a>Patroon

De **patroon** element mag de volgende kenmerken:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| RegularExpression | Ja | De reguliere expressie die claims van dit type moeten overeenkomen met geldig. |
| Help-tekst | Nee | Het patroon of reguliere expressie voor deze claim. |

Het volgende voorbeeld wordt een **e** claim met een reguliere expressie invoer validatie en help-tekst:

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OpenIdConnect" PartnerClaimType="email" />
  </DefaultPartnerClaimTypes>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9.!#$%&amp;'^_`{}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" HelpText="Please enter a valid email address." />
    </Restriction>
 </ClaimType>
```

De Identity-Ervaringsframework wordt de e-adres-claim met een e-mailbericht indeling Invoervalidatie weergegeven:

![Met behulp van claimtype met patroon](./media/claimsschema/pattern.png)

## <a name="userinputtype"></a>UserInputType

Azure AD B2C ondersteunt een groot aantal invoertypen van de gebruiker, zoals een tekstvak, wachtwoord en vervolgkeuzelijst lijst die kunnen worden gebruikt bij het handmatig invoeren van claimgegevens voor het claimtype. Moet u de **UserInputType** wanneer u gegevens verzamelen van de gebruiker met behulp van een [door technisch profiel zelf de bevestigde](self-asserted-technical-profile.md).

### <a name="textbox"></a>Tekstvak

De **tekstvak** type gebruikersinvoer wordt gebruikt voor een tekstvak met één regel.

![Met behulp van claimtype met tekstvak](./media/claimsschema/textbox.png)

```XML
<ClaimType Id="displayName">
  <DisplayName>Display Name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your display name.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

### <a name="emailbox"></a>EmailBox

De **EmailBox** type gebruikersinvoer wordt gebruikt voor een standaard e-invoerveld.

![Met behulp van claimtype met emailbox](./media/claimsschema/emailbox.png)

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>EmailBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+(?:\.[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+)*@(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?\.)+[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?$" HelpText="Please enter a valid email address." />
  </Restriction>
</ClaimType>
```

### <a name="password"></a>Wachtwoord

De **wachtwoord** type gebruikersinvoer wordt gebruikt om vast te leggen van een wachtwoord ingevoerd door de gebruiker.

![Met behulp van claimtype met wachtwoord](./media/claimsschema/password.png)

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
</ClaimType>
```

### <a name="datetimedropdown"></a>DateTimeDropdown

De **DateTimeDropdown** type gebruikersinvoer wordt gebruikt voor een set vervolgkeuzelijsten om te selecteren van een dag, maand en jaar. U kunt predicaten en PredicateValidations elementen gebruiken voor het beheren van de minimale en maximale waarden. Zie voor meer informatie de **configureren van een bepaalde periode** sectie van [predicaten en PredicateValidations](predicates.md).

![Met behulp van claimtype met datetimedropdown](./media/claimsschema/datetimedropdown.png)

```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date Of Birth</DisplayName>
  <DataType>date</DataType>
  <UserHelpText>The date on which you were born.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
</ClaimType>
```

### <a name="radiosingleselect"></a>RadioSingleSelect

De **RadioSingleSelect** type gebruikersinvoer wordt gebruikt voor een verzameling van keuzerondjes waarmee de gebruiker om een optie te selecteren.

![Met behulp van claimtype met radiodsingleselect](./media/claimsschema/radiosingleselect.png)

```XML
<ClaimType Id="color">
  <DisplayName>Preferred color</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Blue" Value="Blue" SelectByDefault="false" />
    <Enumeration Text="Green " Value="Green" SelectByDefault="false" />
    <Enumeration Text="Orange" Value="Orange" SelectByDefault="true" />
  </Restriction>
</ClaimType>    
```

### <a name="dropdownsingleselect"></a>DropdownSingleSelect

De **DropdownSingleSelect** type gebruikersinvoer wordt gebruikt voor een vervolgkeuzelijst waarmee de gebruiker om een optie te selecteren.

![Met behulp van claimtype met dropdownsingleselect](./media/claimsschema/dropdownsingleselect.png)

```XML
<ClaimType Id="city">
  <DisplayName>City where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

### <a name="checkboxmultiselect"></a>CheckboxMultiSelect

De **CheckboxMultiSelect** type gebruikersinvoer wordt gebruikt voor een verzameling van selectievakjes waarmee de gebruiker meerdere opties selecteren.

![Met behulp van claimtype met checkboxmultiselect](./media/claimsschema/checkboxmultiselect.png)

```XML
<ClaimType Id="languages">
  <DisplayName>Languages you speak</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="English" Value="English" SelectByDefault="true" />
    <Enumeration Text="France " Value="France" SelectByDefault="false" />
    <Enumeration Text="Spanish" Value="Spanish" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

### <a name="readonly"></a>alleen-lezen

De **Readonly** type gebruikersinvoer wordt gebruikt voor een alleen-lezenveld om de claim en de waarde weer te geven.

![Met behulp van claimtype met alleen-lezen](./media/claimsschema/readonly.png)

```XML
<ClaimType Id="membershipNumber">
  <DisplayName>Membership number</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your membership number (read only)</UserHelpText>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```


### <a name="paragraph"></a>Alinea

De **alinea** type gebruikersinvoer wordt gebruikt voor een veld waarin de tekst wordt alleen in een alineatag. Bijvoorbeeld, &lt;p&gt;tekst&lt;/p&gt;.

![Met behulp van claimtype lid](./media/claimsschema/paragraph.png)

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>A claim responsible for holding response messages to send to the relying party</AdminHelpText>
  <UserHelpText>A claim responsible for holding response messages to send to the relying party</UserHelpText>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cant sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```

Om weer te geven van een van de **opsomming** waarden in een **responseMsg** claim, gebruikt u `GetMappedValueFromLocalizedCollection` of `CreateStringClaim` transformatie claims. Zie voor meer informatie, [tekenreeks Claimtransformaties](string-transformations.md) 
