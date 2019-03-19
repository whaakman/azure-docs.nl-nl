---
title: StringCollection claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: StringCollection claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: c312433832f7402eaff8b40c4e0a2a61397f6f87
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58123501"
---
# <a name="stringcollection-claims-transformations"></a>StringCollection claims transformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van de tekenreeks verzameling claimtransformaties van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Hiermee wordt een claim tekenreeks naar een nieuwe stringCollection claim toegevoegd. 

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | item | string | Het ClaimType worden toegevoegd aan de uitvoerclaim. |
| InputClaim | verzameling | stringCollection | [Optioneel] Indien opgegeven, wordt de claimtransformatie opgehaald van de items uit deze verzameling en wordt het item wordt toegevoegd aan het einde van de verzameling uitvoerclaim. |
| OutputClaim | verzameling | stringCollection | De ClaimTypes die worden gegenereerd nadat deze ClaimsTransformation is aangeroepen. |

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
| InputClaim | verzameling | stringCollection | [Optioneel] Indien opgegeven, wordt de claimtransformatie opgehaald van de items uit deze verzameling en wordt het item wordt toegevoegd aan het einde van de verzameling uitvoerclaim. |
| InputParameter | item | string | De waarde die moet worden toegevoegd aan de uitvoerclaim. |
| OutputClaim | verzameling | stringCollection | De ClaimTypes die worden geproduceerd nadat deze ClaimsTransformation is aangeroepen. |

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
| InputClaim | verzameling | stringCollection | De ClaimTypes die door de claimtransformatie worden gebruikt om op te halen van het item. |
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

