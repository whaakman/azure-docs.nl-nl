---
title: Voorbeelden van algemene claims transformatie voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: Voorbeelden van algemene claims transformatie voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8cae6ec9693c0fadba059e641fb75e68bbbaec92
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853088"
---
# <a name="general-claims-transformations"></a>Algemene claimtransformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van claimtransformaties algemene van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Controleert of de **inputClaim** of niet bestaat en stelt **outputClaim** op waar of ONWAAR dienovereenkomstig.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |Alle | De invoer claim waarvan het bestaan moet worden bevestigd. |
| OutputClaim | outputClaim | booleaans | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen. |

Gebruik die dit claims transformatie om te controleren of als een claim bestaat, en een waarde bevat. De geretourneerde waarde is een Booleaanse waarde die aangeeft of de claim bestaat. Volgende voorbeeld wordt gecontroleerd of het e-mailadres bestaat.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim**: someone@contoso.com
- Uitvoerclaims: 
    - **outputClaim**: true

## <a name="hash"></a>Hash

De opgegeven tekst zonder opmaak, met behulp van de salt en een geheim hash.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | Als tekst zonder opmaak | string | De invoer claim moeten worden versleuteld |
| InputClaim | Salt | string | De salt-parameter. U kunt maken als een willekeurige waarde, met behulp van `CreateRandomString` transformatie claims. |
| InputParameter | randomizerSecret | string | Verwijst naar een bestaande Azure AD B2C **Beleidssleutels**. Een nieuwe maken: Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen > Identity-Ervaringsframework**. Selecteer **Beleidssleutels** om de sleutels die beschikbaar in uw tenant zijn weer te geven. Selecteer **Toevoegen**. Voor **opties**, selecteer **handmatig**. Geef een naam (het voorvoegsel B2C_1A_ mogelijk automatisch worden toegevoegd.). Voer een geheim dat u gebruiken wilt, zoals 1234567890 het geheim in. Selecteer voor sleutelgebruik, **geheim**. Selecteer **Maken**. |
| OutputClaim | Hash | string | Het ClaimType dat wordt gegenereerd nadat deze transformatie claims is aangeroepen. De claim die is geconfigureerd in de `plaintext` inputClaim. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **Als tekst zonder opmaak**: MyPass@word1
    - **Salt**: 487624568
    - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Uitvoerclaims: 
    - **outputClaim**: CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=



