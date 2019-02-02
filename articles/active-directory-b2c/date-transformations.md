---
title: Datum van de claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: Datum van de claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: d36abb669490b3d3f6818c018b3844a82ecd0617
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564783"
---
# <a name="date-claims-transformations"></a>Datum claims transformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel biedt voorbeelden voor het gebruik van de gegevenstransformaties claims van het schema Identiteitservaring-Framework in Azure Active Directory (Azure AD) B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="assertdatetimeisgreaterthan"></a>AssertDateTimeIsGreaterThan

Controleert of een datum en tijd claim (tekenreeksgegevenstype) is hoger dan een tweede datum en tijd claim (gegevenstype string) en een uitzondering genereert.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | leftOperand | string | Eerste claim-type moet later zijn dan het tweede argument. |
| inputClaim | rightOperand | string | Tweede van de claim type, dat eerder zijn dan de eerste claim moet. |
| InputParameter | AssertIfEqualTo | booleaans | Hiermee geeft u op of deze verklaring moet worden verwerkt als de linkeroperand gelijk aan de rechteroperand is. |
| InputParameter | AssertIfRightOperandIsNotPresent | booleaans | Hiermee geeft u op of deze verklaring moet worden verwerkt als de rechteroperand ontbreekt. |
| InputParameter | TreatAsEqualIfWithinMillseconds | int | Hiermee geeft u het aantal milliseconden om toe te staan tussen de twee datums en tijden die rekening houden met de tijden gelijk moet zijn (bijvoorbeeld aan een account voor tijdsverschil). |

De **AssertDateTimeIsGreaterThan** claimtransformatie wordt altijd uitgevoerd vanuit een [validatie technisch profiel](validation-technical-profile.md) die wordt aangeroepen door een [door zelf bevestigde technisch profiel](self-asserted-technical-profile.md). De **DateTimeGreaterThan** metagegevens van de zelf-gecontroleerde technisch profiel bepaalt het foutbericht dat het technische profiel aan de gebruiker biedt.

![AssertStringClaimsAreEqual uitvoering](./media/date-transformations/assert-execution.png)

Het volgende voorbeeld vergelijkt de `currentDateTime` claim met de `approvedDateTime` claim. Er is een fout gegenereerd als `currentDateTime` is hoger dan `approvedDateTime`. De transformatie worden waarden als gelijk als ze binnen 5 minuten (30000 in milliseconden) verschil behandeld.

```XML
<ClaimsTransformation Id="AssertApprovedDateTimeLaterThanCurrentDateTime" TransformationMethod="AssertDateTimeIsGreaterThan">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="approvedDateTime" TransformationClaimType="leftOperand" />
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="rightOperand" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
    <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
    <InputParameter Id="TreatAsEqualIfWithinMillseconds" DataType="int" Value="300000" />
  </InputParameters>
</ClaimsTransformation>
```

De `login-NonInteractive` validatie technisch profiel aanroepen de `AssertApprovedDateTimeLaterThanCurrentDateTime` transformatie claims.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertApprovedDateTimeLaterThanCurrentDateTime" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

De zelf-gecontroleerde technisch profiel roept de validatie **aanmelding niet-interactieve** technisch profiel.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="DateTimeGreaterThan">Custom error message if the provided left operand is greater than the right operand.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **leftOperand**: 2018-10-01T15:00:00.0000000Z
    - **rightOperand**: 2018-10-01T14:00:00.0000000Z
- Resultaat: Fout opgetreden

## <a name="convertdatetodatetimeclaim"></a>ConvertDateToDateTimeClaim

Converteert een **datum** ClaimType naar een **datum-/** ClaimType. De claimtransformatie converteert de tijdnotatie en 12:00:00 AM toegevoegd aan de datum.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | date | Het ClaimType moet worden geconverteerd. |
| OutputClaim | outputClaim | Datum/tijd | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen. |

Het volgende voorbeeld ziet u de conversie van de claim `dateOfBirth` (gegevenstype datum) op een andere claim `dateOfBirthWithTime` (gegevenstype datum/tijd).

```XML
  <ClaimsTransformation Id="ConvertToDateTime" TransformationMethod="ConvertDateToDateTimeClaim">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="dateOfBirth" TransformationClaimType="inputClaim" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="dateOfBirthWithTime" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **inputClaim**: 2019-06-01
- Uitvoerclaims:
    - **outputClaim**: 1559347200 (juni 1 2019 12:00:00 uur)

## <a name="getcurrentdatetime"></a>GetCurrentDateTime

Ophalen van de huidige UTC-datum en tijd en de waarde toevoegen aan een ClaimType.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | currentDateTime | Datum/tijd | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen. |

```XML
<ClaimsTransformation Id="GetSystemDateTime" TransformationMethod="GetCurrentDateTime">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="currentDateTime" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

* Uitvoerclaims:
    * **currentDateTime**: 1534418820 (augustus 16 2018 11:27:00 uur)

## <a name="datetimecomparison"></a>DateTimeComparison

Bepalen of een datum/tijd later, eerder of gelijk zijn aan een andere. Het resultaat is een nieuwe Booleaanse ClaimType Boole-waarde met een waarde van `true` of `false`.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | firstDateTime | Datum/tijd | De eerste datum en tijd om te vergelijken of eerder of later zijn dan de tweede datum/tijd. Null-waarde is een uitzondering genereert. |
| InputClaim | secondDateTime | Datum/tijd | De tweede datum en tijd om te vergelijken of eerder of later zijn dan de eerste datum/tijd. Null-waarde wordt beschouwd als de huidige datetTime. |
| InputParameter | operator | string | Een van de volgende waarden: dezelfde, later zijn dan of ouder is dan. |
| InputParameter | timeSpanInSeconds | int | De timespan toevoegen aan de eerste datum/tijd. |
| OutputClaim | Resultaat | booleaans | Het ClaimType dat wordt gegenereerd nadat deze ClaimsTransformation is aangeroepen. |

Gebruik die deze transformatie om te bepalen of twee ClaimTypes gelijk, later of ouder is dan alle andere zijn vorderingen. U kunt bijvoorbeeld de laatste keer dat een gebruiker de voorwaarden van de services (TOS) geaccepteerd opslaan. Na 3 maanden, kunt u de gebruiker om de TOS opnieuw toegang te vragen.
Als u wilt de claimtransformatie uitvoeren, moet u eerst om op te halen van de huidige datum/tijd en de laatste tijd gebruiker accepteert ook de instructies.

```XML
<ClaimsTransformation Id="CompareLastTOSAcceptedWithCurrentDateTime" TransformationMethod="DateTimeComparison">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="firstDateTime" />
    <InputClaim ClaimTypeReferenceId="extension_LastTOSAccepted" TransformationClaimType="secondDateTime" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="later than" />
    <InputParameter Id="timeSpanInSeconds" DataType="int" Value="7776000" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="result" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **firstDateTime**: 2018-01-01T00:00:00.100000Z
    - **secondDateTime**: 2018-04-01T00:00:00.100000Z
- Invoerparameters die zijn opgegeven:
    - **operator**: later zijn dan
    - **timeSpanInSeconds**: 7776000 (90 dagen)
- Uitvoerclaims:
    - **resultaat**: true
