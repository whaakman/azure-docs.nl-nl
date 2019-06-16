---
title: StringCollection claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: StringCollection claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 98453daeb34d093b49cdcc636f68c3d7ae017126
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66512442"
---
# <a name="stringcollection-claims-transformations"></a>StringCollection claims transformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van de tekenreeks verzameling claimtransformaties van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Hiermee wordt een claim tekenreeks naar een nieuwe stringCollection claim toegevoegd. 

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | item | string | Het ClaimType worden toegevoegd aan de uitvoerclaim. |
| InputClaim | Verzameling | stringCollection | [Optioneel] Indien opgegeven, wordt de claimtransformatie opgehaald van de items uit deze verzameling en wordt het item wordt toegevoegd aan het einde van de verzameling uitvoerclaim. |
| OutputClaim | Verzameling | stringCollection | De ClaimTypes die worden gegenereerd nadat deze ClaimsTransformation is aangeroepen. |

Gebruik deze transformatie als u wilt toevoegen van een tekenreeks naar een nieuwe of bestaande stringCollection vorderingen. Wordt vaak gebruikt een **AAD-UserWriteUsingAlternativeSecurityId** technisch profiel. Voordat u een nieuw sociale account is gemaakt, **CreateOtherMailsFromEmail** claimtransformatie leest het ClaimType en de waarde die moet worden toegevoegd de **otherMails** ClaimType. 

De volgende claimtransformatie wordt toegevoegd de **e** ClaimType naar **otherMails** ClaimType.

```XML
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
  - **verzameling**: ["someone@outlook.com"]
  - **item**: "admin@contoso.com"
- Uitvoerclaims: 
  - **verzameling**: ["someone@outlook.com","admin@contoso.com"]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Voegt een queryreeks-parameter toe aan een nieuwe stringCollection claim. 

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | Verzameling | stringCollection | [Optioneel] Indien opgegeven, wordt de claimtransformatie opgehaald van de items uit deze verzameling en wordt het item wordt toegevoegd aan het einde van de verzameling uitvoerclaim. |
| InputParameter | item | string | De waarde die moet worden toegevoegd aan de uitvoerclaim. |
| OutputClaim | Verzameling | stringCollection | De ClaimTypes die worden geproduceerd nadat deze ClaimsTransformation is aangeroepen. |

Gebruik deze transformatie als u wilt een string-waarde toevoegen aan een nieuwe of bestaande stringCollection vorderingen. Het volgende voorbeeld wordt een constante e-mailadres (admin@contoso.com) naar de **otherMails** claim. 

```XML
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
  - **verzameling**: ["someone@outlook.com"]
- Invoerparameters 
  - **item**: "admin@contoso.com"
- Uitvoerclaims:
  - **verzameling**: ["someone@outlook.com","admin@contoso.com"]

## <a name="getsingleitemfromstringcollection"></a>GetSingleItemFromStringCollection

Hiermee haalt u het eerste item uit de opgegeven tekenreeks-verzameling. 

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | Verzameling | stringCollection | De ClaimTypes die door de claimtransformatie worden gebruikt om op te halen van het item. |
| OutputClaim | extractedItem | string | De ClaimTypes die worden gegenereerd nadat deze ClaimsTransformation is aangeroepen. Het eerste item in de verzameling. |

Het volgende voorbeeld leest de **otherMails** claimen en retourneert het eerste item in de **e** claim. 

```XML
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
  - **verzameling**: ["someone@outlook.com","someone@contoso.com"]
- Uitvoerclaims: 
  - **extractedItem**: "someone@outlook.com"

