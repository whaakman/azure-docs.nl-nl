---
title: JSON van de claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: JSON claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e7096773f2aaa39abd965b4697f45a3b3f80f136
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54850838"
---
# <a name="json-claims-transformations"></a>JSON-transformaties claims

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van de JSON-claimtransformaties van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="getclaimfromjson"></a>GetClaimFromJson

Het opgegeven element ophalen uit een JSON-gegevens.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputJson | string | De ClaimTypes die door de claimtransformatie worden gebruikt om op te halen van het item. |
| InputParameter | claimToExtract | string | de naam van de JSON-element moet worden geëxtraheerd. |
| OutputClaim | extractedClaim | string | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen, de elementwaarde die is opgegeven in de _claimToExtract_ invoerparameter. |

In het volgende voorbeeld wordt de claimtransformatie geëxtraheerd de `emailAddress` -element van de JSON-gegevens: `{"emailAddress": "someone@example.com", "displayName": "Someone"}`

```XML
<ClaimsTransformation Id="GetEmailClaimFromJson" TransformationMethod="GetClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="customUserData" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="emailAddress" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="extractedEmail" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputJson**: {"emailAddress": "someone@example.com", "displayName": "Iemand"}
- Invoerparameter:
    - **claimToExtract**: emailAddress
- Uitvoerclaims: 
    - **extractedClaim**: someone@example.com


## <a name="getclaimsfromjsonarray"></a>GetClaimsFromJsonArray

Een lijst met opgegeven elementen ophalen van Json-gegevens.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | jsonSourceClaim | string | De ClaimTypes die door de claimtransformatie worden gebruikt om op te halen van de claims. |
| InputParameter | errorOnMissingClaims | booleaans | Geeft aan of een fout is inzetten als een van de claims ontbreekt. |
| InputParameter | includeEmptyClaims | string | Geef op of bevatten lege claims. |
| InputParameter | jsonSourceKeyName | string | Sleutelnaam van element |
| InputParameter | jsonSourceValueName | string | Waardenaam element |
| OutputClaim | Verzameling | String, int, Booleaanse waarde en datum/tijd |Lijst met claims om op te halen. De naam van de claim moet gelijk zijn aan de versie die is opgegeven _jsonSourceClaim_ invoer claim. |

In het volgende voorbeeld haalt de claimtransformatie de volgende claims: e-mailadres (tekenreeks), displayName (tekenreeks), membershipNum (int), actieve (boolean) en geboortedatum (datetime) van de JSON-gegevens.

```JSON
[{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value":true}, {"key":"birthdate","value":"1980-09-23T00:00:00Z"}]
```

```XML
<ClaimsTransformation Id="GetClaimsFromJson" TransformationMethod="GetClaimsFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="jsonSourceClaim" TransformationClaimType="jsonSource" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="errorOnMissingClaims" DataType="boolean" Value="false" />
    <InputParameter Id="includeEmptyClaims" DataType="boolean" Value="false" />
    <InputParameter Id="jsonSourceKeyName" DataType="string" Value="key" />
    <InputParameter Id="jsonSourceValueName" DataType="string" Value="value" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="membershipNum" />
    <OutputClaim ClaimTypeReferenceId="active" />
    <OutputClaim ClaimTypeReferenceId="birthdate" />
  </OutputClaims>
</ClaimsTransformation>
```    

- Invoerclaims:
    - **jsonSourceClaim**: [{"sleutel": "e", "waarde": "someone@example.com"}, {"sleutel": "displayName", "waarde": "Iemand"}, {"sleutel": "membershipNum", "waarde": 6353399}, {"sleutel": 'active', '' waarde '': true}, {"sleutel": "geboortedatum", "waarde": "1980-09-23T00:0 0:00Z"}]
- Invoerparameters die zijn opgegeven:
    - **errorOnMissingClaims**: false
    - **includeEmptyClaims**: false
    - **jsonSourceKeyName**: key
    - **jsonSourceValueName**: value
- Uitvoerclaims:
    - **e-mailbericht**: "someone@example.com"
    - **displayName**: "Iemand"
    - **membershipNum**: 6353399
    - **actieve**: true
    - **Geboortedatum**: 1980-09-23T00:00:00Z

## <a name="getnumericclaimfromjson"></a>GetNumericClaimFromJson

Hiermee haalt u een opgegeven numerieke (long)-element van een JSON-gegevens.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputJson | string | De ClaimTypes die door de claimtransformatie worden gebruikt om op te halen van de claim. |
| InputParameter | claimToExtract | string | De naam van de JSON-element om op te halen. |
| OutputClaim | extractedClaim | lang | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen, van het element waarde opgegeven in de _claimToExtract_ -invoerparameters. |

In het volgende voorbeeld wordt de claimtransformatie haalt de `id` -element van de JSON-gegevens.

```JSON
{
    "emailAddress": "someone@example.com", 
    "displayName": "Someone", 
    "id" : 6353399
}
```

```XML
<ClaimsTransformation Id="GetIdFromResponse" TransformationMethod="GetNumericClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="exampleInputClaim" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="id" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="membershipId" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputJson**: {"emailAddress": "someone@example.com", "displayName": 'Iemand', 'id': 6353399}
- Invoerparameters
    - **claimToExtract**: id
- Uitvoerclaims: 
    - **extractedClaim**: 6353399

## <a name="getsinglevaluefromjsonarray"></a>GetSingleValueFromJsonArray

Hiermee haalt u het eerste element van een matrix met JSON.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputJsonClaim | string | De ClaimTypes die door de claimtransformatie worden gebruikt voor het item ophalen uit de JSON-matrix. |
| OutputClaim | extractedClaim | string | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen, het eerste element in de JSON-matrix. |

In het volgende voorbeeld haalt de claimtransformatie het eerste element (e-mailadres) uit de JSON-matrix `["someone@example.com", "Someone", 6353399]`.

```XML
<ClaimsTransformation Id="GetEmailFromJson" TransformationMethod="GetSingleValueFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userData" TransformationClaimType="inputJsonClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputJsonClaim**: ["someone@example.com", "Iemand", 6353399]
- Uitvoerclaims: 
    - **extractedClaim**: someone@example.com

## <a name="xmlstringtojsonstring"></a>XmlStringToJsonString

XML-gegevens wordt omgezet in JSON-indeling.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | xml | string | De ClaimTypes die worden gebruikt door de claimtransformatie voor de gegevens uit XML converteren naar JSON-indeling. |
| OutputClaim | json | string | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen, worden de gegevens in JSON-indeling. |

```XML
<ClaimsTransformation Id="ConvertXmlToJson" TransformationMethod="XmlStringToJsonString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="intpuXML" TransformationClaimType="xml" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="outputJson" TransformationClaimType="json" />
  </OutputClaims>
</ClaimsTransformation>
```

In het volgende voorbeeld wordt omgezet de claimtransformatie in de volgende XML-gegevens JSON-indeling.

#### <a name="example"></a>Voorbeeld
Invoer claim:

```XML
<user>
  <name>Someone</name>
  <email>someone@example.com</email>
</user>
```

Uitvoerclaim:

```JSON
{
  "user": {
    "name":"Someone",
    "email":"someone@example.com"
  }
}
```

