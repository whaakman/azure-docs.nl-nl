---
title: Booleaanse waarde van de claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: Booleaanse waarde claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b1e56e9126b1dd93ed790da1526b64c49524149d
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381275"
---
# <a name="boolean-claims-transformations"></a>Booleaanse claimtransformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van de claimtransformaties Booleaanse van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="andclaims"></a>AndClaims

Voert een bewerking en van twee Booleaanse inputClaims en stelt de outputClaim met het resultaat van de bewerking.

| Item  | TransformationClaimType  | Gegevenstype  | Opmerkingen |
|-------| ------------------------ | ---------- | ----- |
| InputClaim | inputClaim1 | booleaans | De eerste ClaimType om te evalueren. |
| InputClaim | inputClaim2  | booleaans | De tweede ClaimType om te evalueren. |
|outputClaim | outputClaim | booleaans | De ClaimTypes die worden geproduceerd nadat deze transformatie claims is aangeroepen (waar of ONWAAR). |

De volgende claimtransformatie wordt gedemonstreerd hoe u en twee Booleaanse ClaimTypes: `isEmailNotExist`, en `isSocialAccount`. De uitvoerclaim `presentEmailSelfAsserted` is ingesteld op `true` als de waarde van beide invoerclaims `true`. In een orchestration-stap kunt u een voorwaarde om vooraf een zelf-gecontroleerde pagina alleen als een sociaal account e-mailadres leeg is.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isEmailNotExist" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isSocialAccount" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentEmailSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Uitvoerclaims:
    - **outputClaim**: false


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

Controleert of Booleaanse waarden van twee claims gelijk zijn, en een uitzondering genereert als ze niet.

| Item | TransformationClaimType  | Gegevenstype  | Opmerkingen |
| ---- | ------------------------ | ---------- | ----- |
| inputClaim | inputClaim | booleaans | Het ClaimType moeten worden gecontroleerd. |
| Invoerparameters |valueToCompareTo | booleaans | De waarde om te vergelijken (waar of ONWAAR). |

De **AssertBooleanClaimIsEqualToValue** claimtransformatie wordt altijd uitgevoerd vanuit een [validatie technisch profiel](validation-technical-profile.md) die wordt aangeroepen door een [door zelf bevestigde technisch profiel](self-asserted-technical-profile.md). De **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** metagegevens van de zelf-gecontroleerde technisch profiel bepaalt het foutbericht dat het technische profiel aan de gebruiker biedt.

![AssertStringClaimsAreEqual uitvoering](./media/boolean-transformations/assert-execution.png)

De volgende claimtransformatie ziet u hoe u om te controleren of de waarde van een Booleaanse ClaimType met een `true` waarde. Als de waarde van de `accountEnabled` ClaimType is ingesteld op false, een foutbericht dat wordt gegenereerd.

```XML
<ClaimsTransformation Id="AssertAccountEnabledIsTrue" TransformationMethod="AssertBooleanClaimIsEqualToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="accountEnabled" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
</ClaimsTransformation>
```


De `login-NonInteractive` validatie technisch profiel aanroepen de `AssertAccountEnabledIsTrue` transformatie claims.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

De zelf-gecontroleerde technisch profiel roept de validatie **aanmelding niet-interactieve** technisch profiel.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Custom error message if account is disabled.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim**: false
    - **valueToCompareTo**: true
- Resultaat: Fout opgetreden

## <a name="notclaims"></a>NotClaims

Voert een niet-bewerking van de Booleaanse inputClaim en stelt de outputClaim met het resultaat van de bewerking.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | booleaans | De claim te worden uitgevoerd. |
| outputClaim | outputClaim | booleaans | De ClaimTypes die worden gegenereerd nadat deze ClaimsTransformation is aangeroepen (waar of ONWAAR). |

Deze claimtransformatie gebruiken om uit te voeren logische onderhandeling op een claim.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim**: false
- Uitvoerclaims:
    - **outputClaim**: true

## <a name="orclaims"></a>OrClaims 

Een of van twee Booleaanse inputClaims berekent en stelt de outputClaim met het resultaat van de bewerking.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | booleaans | De eerste ClaimType om te evalueren. |
| InputClaim | inputClaim2 | booleaans | De tweede ClaimType om te evalueren. |
| outputClaim | outputClaim | booleaans | De ClaimTypes die worden geproduceerd nadat deze ClaimsTransformation is aangeroepen (waar of ONWAAR). |

De volgende claimtransformatie wordt gedemonstreerd hoe u `Or` twee Booleaanse ClaimTypes. In de orchestration-stap kunt u een voorwaarde om vooraf een zelf-gecontroleerde pagina gebruiken als de waarde van een van de claims `true`.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="OrClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedNotExists" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="inputClaim2" />
  </InputClaims>                    
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentTOSSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Uitvoerclaims:
    - **outputClaim**: true

