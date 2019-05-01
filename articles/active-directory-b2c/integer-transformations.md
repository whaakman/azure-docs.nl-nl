---
title: Geheel getal van de claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: Geheel getal claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 20c109f188acfe37292aa31dfe057ced8b73199b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64694754"
---
# <a name="integer-claims-transformations"></a>Geheel getal claims transformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van de transformaties geheel getal claims van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim 

Een lange gegevenstype converteert naar een tekenreeks-gegevenstype.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | lang | Het ClaimType moet worden geconverteerd naar een tekenreeks. |
| OutputClaim | outputClaim | string | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen. |

In dit voorbeeld wordt de `numericUserId` claim met waardetype Long-waarde wordt geconverteerd naar een `UserId` claim met waardetype string.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim**: 12334 (long)
- Uitvoerclaims: 
    - **outputClaim**: "12334" (tekenreeks)

