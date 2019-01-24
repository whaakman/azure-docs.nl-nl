---
title: Tekenreeks van de claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: Tekenreeks claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1a382b845b621e47d30869a1081549b7f30348aa
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54850728"
---
# <a name="string-claims-transformations"></a>Tekenreeks claims transformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van de tekenreeks claimtransformaties van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="assertstringclaimsareequal"></a>AssertStringClaimsAreEqual 

Vergelijk twee claims en een uitzondering genereert als ze niet op basis van de vergelijking van de opgegeven inputClaim1, inputClaim2 en stringComparison gelijk zijn.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim1 | string | Eerste claim type, dat moet worden vergeleken. |
| inputClaim | inputClaim2 | string | Tweede van de claim type, dat moet worden vergeleken. |
| InputParameter | stringComparison | string | vergelijking van gegevensreeksen, een van de waarden: Volgnummer, OrdinalIgnoreCase. |

De **AssertStringClaimsAreEqual** claimtransformatie wordt altijd uitgevoerd vanuit een [validatie technisch profiel](validation-technical-profile.md) die wordt aangeroepen door een [door zelf bevestigde technisch profiel](self-asserted-technical-profile.md). De **UserMessageIfClaimsTransformationStringsAreNotEqual** metagegevens van de zelf-gecontroleerde technisch profiel bepaalt het foutbericht dat wordt weergegeven voor de gebruiker.

![AssertStringClaimsAreEqual uitvoering](./media/string-transformations/assert-execution.png)

U kunt dit claims transformatie om ervoor te zorgen, twee ClaimTypes dezelfde waarde hebben. Als dat niet het geval is, moet u een foutbericht dat wordt gegenereerd. Het volgende voorbeeld wordt gecontroleerd dat de **strongAuthenticationEmailAddress** ClaimType gelijk is aan **e** ClaimType. Anders wordt wordt een foutbericht dat gegenereerd. 

```XML
<ClaimsTransformation Id="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="strongAuthenticationEmailAddress" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
  </InputParameters>
</ClaimsTransformation>
```


De **aanmelding niet-interactieve** validatie technisch profiel aanroepen de **AssertEmailAndStrongAuthenticationEmailAddressAreEqual** transformatie claims.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

De zelf-gecontroleerde technisch profiel roept de validatie **aanmelding niet-interactieve** technisch profiel.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">Custom error message the email addresses you provided are not the same.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim1**: someone@contoso.com
    - **inputClaim2**: someone@outlook.com
 - Invoerparameters die zijn opgegeven:
    - **stringComparison**: ordinalIgnoreCase
- Resultaat: Fout opgetreden

## <a name="changecase"></a>ChangeCase 

De aanvraag van de opgegeven claim te verlagen of hoofdletters, afhankelijk van de operator wijzigt.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Het ClaimType die zijn gewijzigd. |
| InputParameter | toCase | string | Een van de volgende waarden: `LOWER` of `UPPER`. |
| OutputClaim | outputClaim | string | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen. |

Deze claimtransformatie gebruiken om elke tekenreeks ClaimType te verlagen of hoofdletters te wijzigen.  

```XML
<ClaimsTransformation Id="ChangeToLower" TransformationMethod="ChangeCase">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="toCase" DataType="string" Value="LOWER" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **e-mailbericht**: SomeOne@contoso.com
- Invoerparameters die zijn opgegeven:
    - **toCase**: LOWER
- Uitvoerclaims:
    - **e-mailbericht**: someone@contoso.com

## <a name="createstringclaim"></a>CreateStringClaim 

Hiermee maakt u een reeks claim uit de opgegeven invoer parameter in het beleid.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
|----- | ----------------------- | --------- | ----- |
| InputParameter | waarde | string | De tekenreeks die moet worden ingesteld |
| OutputClaim | createdClaim | string | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen met de waarde die is opgegeven in de invoerparameter. |

Gebruik die dit claims transformatie naar instellen een tekenreeks ClaimType waarde.

```XML
<ClaimsTransformation Id="CreateTermsOfService" TransformationMethod="CreateStringClaim">
  <InputParameters>
    <InputParameter Id="value" DataType="string" Value="Contoso terms of service..." />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="TOS" TransformationClaimType="createdClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerparameter:
    - **Waarde**: Contoso servicevoorwaarden...
- Uitvoerclaims:
    - **createdClaim**: Het ClaimType TOS bevat de waarde 'Contoso servicevoorwaarden...'.

## <a name="compareclaims"></a>CompareClaims

Bepalen of een tekenreeks-claim gelijk aan een andere is. Het resultaat is een nieuwe Booleaanse ClaimType met de waarde `true` of `false`.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim1 | string | Eerst claimtype, die moet worden vergeleken. |
| inputClaim | inputClaim2 | string | Ten tweede claimtype, die moet worden vergeleken. |
| InputParameter | operator | string | Mogelijke waarden: `EQUAL` of `NOT EQUAL`. |
| InputParameter | ignoreCase | booleaans | Hiermee geeft u op of de aanvraag van de tekenreeksen met elkaar worden vergeleken moet worden genegeerd door deze vergelijking. |
| OutputClaim | outputClaim | booleaans | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen. |

Gebruik die deze transformatie om te controleren als een claim gelijk aan een andere claim is vorderingen. Bijvoorbeeld, de volgende controles transformatie claims als de waarde van de **e** claim is gelijk aan de **Verified.Email** claim.

```XML
<ClaimsTransformation Id="CheckEmail" TransformationMethod="CompareClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="Email" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="Verified.Email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="SameEmailAddress" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim1**: someone@contoso.com
    - **inputClaim2**: someone@outlook.com
- Invoerparameters die zijn opgegeven:
    - **operator**:  NIET GELIJK AAN
    - **ignoreCase**: true
- Uitvoerclaims:
    - **outputClaim**: true

## <a name="compareclaimtovalue"></a>CompareClaimToValue

Bepaalt of een claimwaarde gelijk aan de waarde van de invoerparameter is.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim1 | string | Type van de claim, die moet worden vergeleken. |
| InputParameter | operator | string | Mogelijke waarden: `EQUAL` of `NOT EQUAL`. |
| InputParameter | compareTo | string | vergelijking van gegevensreeksen, een van de waarden: Volgnummer, OrdinalIgnoreCase. |
| InputParameter | ignoreCase | booleaans | Hiermee geeft u op of de aanvraag van de tekenreeksen met elkaar worden vergeleken moet worden genegeerd door deze vergelijking. |
| OutputClaim | outputClaim | booleaans | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen. |

U kunt dit claims transformatie om te controleren of een claim gelijk is aan een waarde die u hebt opgegeven. Bijvoorbeeld, de volgende controles transformatie claims als de waarde van de **termsOfUseConsentVersion** claim is gelijk aan `v1`.

```XML
<ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="compareTo" DataType="string" Value="V1" />
    <InputParameter Id="operator" DataType="string" Value="not equal" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld
- Invoerclaims:
    - **inputClaim1**: v1
- Invoerparameters die zijn opgegeven:
    - **compareTo**: V1
    - **operator**: GELIJK AAN 
    - **ignoreCase**: true
- Uitvoerclaims:
    - **outputClaim**: true

## <a name="createrandomstring"></a>CreateRandomString

Hiermee maakt u een willekeurige tekenreeks met behulp van de generator van willekeurige getallen. Als de generator van willekeurige getallen van het type is `integer`desgewenst een seedparameter en een maximum aantal kunnen worden geleverd. Een optionele tekenreeks-indeling-parameter zorgt ervoor dat de uitvoer moet worden geformatteerd met het en een optionele base64-parameter geeft aan of de uitvoer base64-gecodeerd randomGeneratorType [guid, geheel getal] outputClaim (tekenreeks).

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputParameter | randomGeneratorType | string | Hiermee geeft u de willekeurige waarde die moet worden gegenereerd, `GUID` (globaal unieke ID) of `INTEGER` (een getal). |
| InputParameter | stringFormat | string | [Optioneel] Indeling van de willekeurige waarde. |
| InputParameter | base64 | booleaans | [Optioneel] De willekeurige waarde converteren naar base64. Als de indeling van tekenreeks wordt toegepast, wordt de waarde na de indeling van tekenreeks naar base64 gecodeerd. |
| InputParameter | maximumNumber | int | [Optioneel] Voor `INTEGER` randomGeneratorType alleen. Geef het maximum aantal. |
| InputParameter | seed  | int | [Optioneel] Voor `INTEGER` randomGeneratorType alleen. Geef de seed voor de willekeurige waarde. Opmerking: dezelfde seed resulteert in dezelfde volgorde van willekeurige getallen. |
| OutputClaim | outputClaim | string | De ClaimTypes die worden geproduceerd nadat deze transformatie claims is aangeroepen. De willekeurige waarde. |

Voorbeeld van de volgende genereert een globale unieke ID. Dit claims transformatie wordt gebruikt om de willekeurige UPN (principal-naam van gebruiker).

```XML
<ClaimsTransformation Id="CreateRandomUPNUserName" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="GUID" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Voorbeeld

- Invoerparameters die zijn opgegeven:
    - **randomGeneratorType**: GUID
- Uitvoerclaims: 
    - **outputClaim**: bc8bedd2-aaa3-411e-bdee-2f1810b73dfc

Voorbeeld van de volgende genereert een willekeurige waarde van geheel getal tussen 0 en 1000. De waarde is geformatteerd met OTP_ {willekeurige waarde}.

```XML
<ClaimsTransformation Id="SetRandomNumber" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="INTEGER" />
    <InputParameter Id="maximumNumber" DataType="int" Value="1000" />
    <InputParameter Id="stringFormat" DataType="string" Value="OTP_{0}" />
    <InputParameter Id="base64" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="randomNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerparameters die zijn opgegeven:
    - **randomGeneratorType**: GEHEEL GETAL ZIJN
    - **maximumNumber**: 1000
    - **stringFormat**: OTP_{0}
    - **met base64**: false
- Uitvoerclaims: 
    - **outputClaim**: OTP_853


## <a name="formatstringclaim"></a>FormatStringClaim

De indeling van een claim op basis van de opgegeven tekenreeks. Deze transformatie maakt gebruik van de C# `String.Format` methode.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |string |Het ClaimType die als de indeling van tekenreeks fungeert {0} parameter. |
| InputParameter | stringFormat | string | De indeling van de tekenreeks, met inbegrip van de {0} parameter. |
| OutputClaim | outputClaim | string | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen. |

Gebruik deze transformatie naar een tekenreeks met één parameter-indeling vorderingen {0}. Het volgende voorbeeld wordt een **userPrincipalName**. Alle sociale id-provider technische profielen, zoals `Facebook-OAUTH` aanroepen de **CreateUserPrincipalName** voor het genereren van een **userPrincipalName**.   

```XML
<ClaimsTransformation Id="CreateUserPrincipalName" TransformationMethod="FormatStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="cpim_{0}@{RelyingPartyTenantId}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim**: 5164db16-3eee-4629-bfda-dcc3326790e9
- Invoerparameters die zijn opgegeven:
    - **stringFormat**:  cpim_{0}@{RelyingPartyTenantId}
- Uitvoerclaims:
    - **outputClaim**: cpim_5164db16-3eee-4629-bfda-dcc3326790e9@b2cdemo.onmicrosoft.com

## <a name="formatstringmultipleclaims"></a>FormatStringMultipleClaims

Maak twee claims op basis van de opgegeven tekenreeks. Deze transformatie maakt gebruik van de C# **String.Format** methode.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |string | Het ClaimType die als de indeling van tekenreeks fungeert {0} parameter. |
| InputClaim | inputClaim | string | Het ClaimType die als de indeling van tekenreeks fungeert {1} parameter. |
| InputParameter | stringFormat | string | De indeling van de tekenreeks, met inbegrip van de {0} en {1} parameters. |
| OutputClaim | outputClaim | string | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen. |

Gebruik deze transformatie naar een tekenreeks met twee parameters-indeling vorderingen {0} en {1}. Het volgende voorbeeld wordt een **displayName** met de opgegeven indeling:

```XML
<ClaimsTransformation Id="CreateDisplayNameFromFirstNameAndLastName" TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surName" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim1**: Jaap
    - **inputClaim2**: Fernando
- Invoerparameters die zijn opgegeven:
    - **stringFormat**: {0} {1}
- Uitvoerclaims:
    - **outputClaim**: Jaap Fernando

## <a name="getmappedvaluefromlocalizedcollection"></a>GetMappedValueFromLocalizedCollection

Opzoeken van een item uit een claim **beperking** verzameling.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | mapFromClaim | string | De claim met de tekst die moet worden opgezocht in de **restrictionValueClaim** claims met het **beperking** verzameling.  |
| OutputClaim | restrictionValueClaim | string | De claim waarin de **beperking** verzameling. Nadat de claimtransformatie is aangeroepen, bevat de waarde van deze claim de waarde van het geselecteerde item. |

Het volgende voorbeeld worden opgezocht op basis van de sleutel van de fout beschrijving van het foutbericht. De **responseMsg** claim bevat een verzameling van foutberichten voor de eindgebruiker of moet worden verzonden naar de relying party.

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cant sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```
De claimtransformatie zoekt de tekst van het item en retourneert de waarde ervan. Als de beperking is gelokaliseerd met behulp van `<LocalizedCollection>`, retourneert de gelokaliseerde waarde van de claimtransformatie.

```XML
<ClaimsTransformation Id="GetResponseMsgMappedToResponseCode" TransformationMethod="GetMappedValueFromLocalizedCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="responseCode" TransformationClaimType="mapFromClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="responseMsg" TransformationClaimType="restrictionValueClaim" />         
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **mapFromClaim**: B2C_V1_90001
- Uitvoerclaims:
    - **restrictionValueClaim**: U kunt zich aanmelden, omdat u een secundaire.

## <a name="lookupvalue"></a>LookupValue

Zoek de waarde van een claim uit een lijst met waarden op basis van de waarde van een andere claim.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputParameterId | string | De claim die de zoekwaarde bevat |
| InputParameter | |string | Verzameling van de invoerparameters. |
| InputParameter | errorOnFailedLookup | booleaans | Bepalen of een fout wordt geretourneerd wanneer er geen overeenkomende opzoeken. |
| OutputClaim | inputParameterId | string | De ClaimTypes die worden geproduceerd nadat deze transformatie claims is aangeroepen. De waarde van de overeenkomende id. |

Het volgende voorbeeld zoekt u de domeinnaam in een van de invoerparameters verzamelingen. De claimtransformatie zoekt naar de domeinnaam in de id en retourneert de waarde (een toepassings-ID).

```XML
 <ClaimsTransformation Id="DomainToClientId" TransformationMethod="LookupValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="inputParameterId" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="contoso.com" DataType="string" Value="13c15f79-8fb1-4e29-a6c9-be0d36ff19f1" />
    <InputParameter Id="microsoft.com" DataType="string" Value="0213308f-17cb-4398-b97e-01da7bd4804e" />
    <InputParameter Id="test.com" DataType="string" Value="c7026f88-4299-4cdb-965d-3f166464b8a9" />
    <InputParameter Id="errorOnFailedLookup" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainAppId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation> 
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputParameterId**: test.com
- Invoerparameters die zijn opgegeven:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **microsoft.com**: 0213308f-17cb-4398-b97e-01da7bd4804e
    - **Test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **errorOnFailedLookup**: false
- Uitvoerclaims:
    - **outputClaim**:  c7026f88-4299-4cdb-965d-3f166464b8a9

## <a name="nullclaim"></a>NullClaim

De waarde van een bepaalde claim opschonen.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | claim_to_null | string | De claim de waarde null zijn. |

Gebruikt deze om het verwijderen van onnodige gegevens uit de eigenschappenverzameling van claims. De sessiecookie wordt dus niet kleiner zijn. Het volgende voorbeeld verwijdert u de waarde van de `TermsOfService` claimtype.

```XML
<ClaimsTransformation Id="SetTOSToNull" TransformationMethod="NullClaim">
  <OutputClaims>
  <OutputClaim ClaimTypeReferenceId="TermsOfService" TransformationClaimType="claim_to_null" />
  </OutputClaims>
</ClaimsTransformation>
```

- Invoerclaims:
    - **outputClaim**: Welkom bij Contoso-App. Als u doorgaat om te bladeren en deze website gebruikt, gaat u ermee akkoord om te voldoen en afhankelijk zijn van de volgende voorwaarden en bepalingen...
- Uitvoerclaims:
    - **outputClaim**: NULL

## <a name="parsedomain"></a>ParseDomain

Hiermee haalt u het domeingedeelte van een e-mailadres.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | emailAddress | string | Het ClaimType dat het e-mailadres bevat. |
| OutputClaim | domein | string | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen: het domein. |

Gebruik dit claims transformatie voor het parseren van de naam van het domein na het @-teken van de gebruiker. Dit kan nuttig zijn bij het verwijderen van persoonsgegevens (PII) uit de controlegegevens zijn. De volgende claimtransformatie ziet u hoe u parseren van de naam van het domein van een **e** claim.

```XML
<ClaimsTransformation Id="SetDomainName" TransformationMethod="ParseDomain">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="emailAddress" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="domain" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **emailAddress**: joe@outlook.com
- Uitvoerclaims:
    - **domein**: outlook.com

## <a name="setclaimsifstringsareequal"></a>SetClaimsIfStringsAreEqual

Controleert of een reeks claim en `matchTo` invoerparameter gelijk zijn, en wordt de uitvoer claims met de waarde aanwezig zijn in `stringMatchMsg` en `stringMatchMsgCode` invoerparameters, samen met vergelijken resultaat uitvoerclaim, die worden ingesteld moet als `true` of `false` op basis van het resultaat van de vergelijking.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim | string | Het claimtype moet worden vergeleken. |
| InputParameter | matchTo | string | De tekenreeks die moet worden vergeleken met de `inputClaim`. |
| InputParameter | stringComparison | string | Mogelijke waarden: `Ordinal` of `OrdinalIgnoreCase`. |
| InputParameter | stringMatchMsg | string | Eerste waarde die moet worden ingesteld als tekenreeksen gelijk zijn. |
| InputParameter | stringMatchMsgCode | string | Tweede waarde die moet worden ingesteld als tekenreeksen gelijk zijn. |
| OutputClaim | outputClaim1 | string | Als tekenreeksen is gelijk aan, deze uitvoerclaim bevat de waarde van `stringMatchMsg` invoerparameter. |
| OutputClaim | outputClaim2 | string | Als tekenreeksen is gelijk aan, deze uitvoerclaim bevat de waarde van `stringMatchMsgCode` invoerparameter. |
| OutputClaim | stringCompareResultClaim | booleaans | De uitvoer van de resultaten vergelijken claimtype die moet worden ingesteld als `true` of `false` op basis van het resultaat van de vergelijking. |

U kunt dit claims transformatie om te controleren of een claim gelijk zijn aan de waarde die u hebt opgegeven. Bijvoorbeeld, de volgende controles transformatie claims als de waarde van de **termsOfUseConsentVersion** claim is gelijk aan `v1`. Zo ja, wijzig de waarde in `v2`. 

```XML
<ClaimsTransformation Id="CheckTheTOS" TransformationMethod="SetClaimsIfStringsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="v1" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="stringMatchMsg" DataType="string" Value="B2C_V1_90005" />
    <InputParameter Id="stringMatchMsgCode" DataType="string" Value="The TOS is upgraded to v2" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="outputClaim1" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeCode" TransformationClaimType="outputClaim2" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeResult" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim**: v1
- Invoerparameters die zijn opgegeven:
    - **matchTo**: V1
    - **stringComparison**: ordinalIgnoreCase 
    - **stringMatchMsg**:  B2C_V1_90005
    - **stringMatchMsgCode**:  De instructies wordt bijgewerkt naar versie 2
- Uitvoerclaims:
    - **outputClaim1**: B2C_V1_90005
    - **outputClaim2**: De instructies wordt bijgewerkt naar versie 2
    - **stringCompareResultClaim**: true

## <a name="setclaimsifstringsmatch"></a>SetClaimsIfStringsMatch

Controleert of een reeks claim en `matchTo` invoerparameter gelijk zijn, en wordt de uitvoer claims met de waarde aanwezig zijn in `outputClaimIfMatched` invoerparameter, samen met vergelijken resultaat uitvoerclaim, die worden ingesteld moet als `true` of `false` op basis van de resultaat van de vergelijking.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | claimToMatch | string | Het claimtype moet worden vergeleken. |
| InputParameter | matchTo | string | De tekenreeks die moet worden vergeleken met de inputClaim. |
| InputParameter | stringComparison | string | Mogelijke waarden: `Ordinal` of `OrdinalIgnoreCase`. |
| InputParameter | outputClaimIfMatched | string | De waarde die moet worden ingesteld als tekenreeksen gelijk zijn. |
| OutputClaim | outputClaim | string | Als tekenreeksen is gelijk aan, deze uitvoerclaim bevat de waarde van `outputClaimIfMatched` invoerparameter. Of null zijn, als de tekenreeksen niet overeen. |
| OutputClaim | stringCompareResultClaim | booleaans | De uitvoer van de resultaten vergelijken claimtype die moet worden ingesteld als `true` of `false` op basis van het resultaat van de vergelijking. |

Bijvoorbeeld, de volgende controles transformatie claims als de waarde van **ageGroup** claim is gelijk aan `Minor`. Zo ja, de waarde te retourneren `B2C_V1_90001`. 

```XML
<ClaimsTransformation Id="SetIsMinor" TransformationMethod="SetClaimsIfStringsMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="ageGroup" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="Minor" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="B2C_V1_90001" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isMinor" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isMinorResponseCode" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **claimToMatch**: Minderjarige
- Invoerparameters die zijn opgegeven:
    - **matchTo**: Minderjarige
    - **stringComparison**: ordinalIgnoreCase 
    - **outputClaimIfMatched**:  B2C_V1_90001
- Uitvoerclaims:
    - **isMinorResponseCode**: B2C_V1_90001
    - **isMinor**: true

