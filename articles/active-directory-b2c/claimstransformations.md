---
title: ClaimsTransformations - Azure Active Directory B2C | Microsoft Docs
description: De definitie van het element ClaimsTransformations in de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: bc6cc7b07d3dce43a666b3e5b0a958b41cdd3131
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651505"
---
# <a name="claimstransformations"></a>ClaimsTransformations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De **ClaimsTransformations** element bevat een lijst met claims transformatiefuncties die kunnen worden gebruikt in gebruiker reizen als onderdeel van een [aangepast beleid](active-directory-b2c-overview-custom.md). Een bepaalde claim converteert een claimtransformatie naar een andere naam. In de claimtransformatie geeft u de transformatie-methode, bijvoorbeeld een item toe te voegen aan de verzameling van een tekenreeks of het wijzigen van het geval van een tekenreeks.

Zodanig dat de lijst met claims transformatiefuncties die kunnen worden gebruikt in de trajecten van de gebruiker, moet een ClaimsTransformations XML-element worden gedeclareerd in het gedeelte BuildingBlocks van het beleid.

```xml
<ClaimsTransformations>
  <ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
    ...
  </ClaimsTransformation>
</ClaimsTransformations>
```

De **ClaimsTransformation** element bevat de volgende kenmerken:

| Kenmerk |Vereist | Description |
| --------- |-------- | ----------- |
| Id |Ja | Een id die wordt gebruikt voor het aanduiden van de claimtransformatie. De id wordt naar verwezen vanuit andere XML-elementen in het beleid. |
| TransformationMethod | Ja | De transformatie te gebruiken in de claimtransformatie-methode. Elke claimtransformatie heeft een eigen waarden. Zie de [claims transformatie verwijzing](#claims-transformations-reference) voor een volledige lijst van de beschikbare waarden. |

## <a name="claimstransformation"></a>ClaimsTransformation

De **ClaimsTransformation** element bevat de volgende elementen:

```xml
<ClaimsTransformation Id="<identifier>" TransformationMethod="<method>">
  <InputClaims>
    ...
  </InputClaims>
  <InputParameters>
    ...
  </InputParameters>                
  <OutputClaims>
    ...
  </OutputClaims>
</ClaimsTransformation>
```


| Element | Gevallen | Description |
| ------- | -------- | ----------- |
| InputClaims | 0:1 | Een lijst met **InputClaim** elementen die claimtypen die worden uitgevoerd opgeeft als invoer voor de claimtransformatie. Elk van deze elementen bevat een verwijzing naar een ClaimType al gedefinieerd in de sectie ClaimsSchema in het beleid. |
| InputParameters | 0:1 | Een lijst met **InputParameter** elementen die zijn opgegeven als invoer voor de claimtransformatie.  
| OutputClaims | 0:1 | Een lijst met **OutputClaim** elementen die opgeeft claim typen die worden gegenereerd nadat de ClaimsTransformation is aangeroepen. Elk van deze elementen bevat een verwijzing naar een ClaimType al gedefinieerd in de sectie ClaimsSchema. |

### <a name="inputclaims"></a>InputClaims

De **InputClaims** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| InputClaim | 1: n | Een verwachte invoer claimtype. |

#### <a name="inputclaim"></a>InputClaim

De **InputClaim** element bevat de volgende kenmerken:

| Kenmerk |Vereist | Description |
| --------- | ----------- | ----------- |
| ClaimTypeReferenceId |Ja | Een verwijzing naar een ClaimType al gedefinieerd in de sectie ClaimsSchema in het beleid. |
| TransformationClaimType |Ja | Een id om te verwijzen naar een transformatie claimtype. Elke claimtransformatie heeft een eigen waarden. Zie de [claims transformatie verwijzing](#claims-transformations-reference) voor een volledige lijst van de beschikbare waarden. |

### <a name="inputparameters"></a>InputParameters

De **invoerparameters** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| InputParameter | 1: n | Een verwachte invoerparameter. |

#### <a name="inputparameter"></a>InputParameter

| Kenmerk | Vereist |Description |
| --------- | ----------- |----------- |
| Id | Ja | Een id die is een verwijzing naar een parameter van de claims transformatie-methode. Elke claims transformatie-methode heeft zijn eigen waarden. Zie de tabel van de transformatie claims voor een volledige lijst van de beschikbare waarden. |
| Gegevenstype | Ja | Het type gegevens van de parameter, zoals tekenreeks, Boole-waarde, Int of datum/tijd aan de hand van de opsomming gegevenstype in het beleid voor aangepaste XML-schema. Dit type wordt gebruikt om de rekenkundige bewerkingen correct wordt uitgevoerd. Elke claimtransformatie heeft een eigen waarden. Zie de [claims transformatie verwijzing](#claims-transformations-reference) voor een volledige lijst van de beschikbare waarden. |
| Value | Ja | Een waarde die verbatim wordt doorgegeven aan de transformatie. Sommige van de waarden zijn willekeurige, sommige u kiezen uit de claims transformatie-methode. |

### <a name="outputclaims"></a>OutputClaims

De **OutputClaims** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| OutputClaim | 0: n | Verwachte uitvoer claimtype. |

#### <a name="outputclaim"></a>OutputClaim 

De **OutputClaim** element bevat de volgende kenmerken:

| Kenmerk |Vereist | Description |
| --------- | ----------- |----------- |
| ClaimTypeReferenceId | Ja | Een verwijzing naar een ClaimType al gedefinieerd in de sectie ClaimsSchema in het beleid.
| TransformationClaimType | Ja | Een id om te verwijzen naar een transformatie claimtype. Elke claimtransformatie heeft een eigen waarden. Zie de [claims transformatie verwijzing](#claims-transformations-reference) voor een volledige lijst van de beschikbare waarden. |
 
Als de invoer claim en de uitvoerclaim zijn van hetzelfde type (tekenreeks of Booleaanse waarde), kunt u de dezelfde invoer claim als de uitvoerclaim. In dit geval verandert de claimtransformatie de invoer claim met de uitvoerwaarde.

## <a name="example"></a>Voorbeeld

U kunt bijvoorbeeld de laatste versie van de voorwaarden van de services die de gebruiker heeft geaccepteerd opslaan. Wanneer u de voorwaarden van de services bijwerkt, kunt u vraagt de gebruiker om te accepteren van de nieuwe versie. In het volgende voorbeeld wordt de **HasTOSVersionChanged** claimtransformatie vergelijkt de waarde van de **TOSVersion** claim met de waarde van de **LastTOSAcceptedVersion**claimen en retourneert vervolgens de Booleaanse waarde **TOSVersionChanged** claim.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="TOSVersionChanged">
      <DisplayName>Indicates if the TOS version accepted by the end user is equal to the current version</DisplayName>
      <DataType>boolean</DataType>
    </ClaimType>
    <ClaimType Id="TOSVersion">
      <DisplayName>TOS version</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    <ClaimType Id="LastTOSAcceptedVersion">
      <DisplayName>TOS version accepted by the end user</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <ClaimsTransformation Id="HasTOSVersionChanged" TransformationMethod="CompareClaims">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="TOSVersion" TransformationClaimType="inputClaim1" />
        <InputClaim ClaimTypeReferenceId="LastTOSAcceptedVersion" TransformationClaimType="inputClaim2" />
      </InputClaims>
      <InputParameters>
        <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
      </InputParameters>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="TOSVersionChanged" TransformationClaimType="outputClaim" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

## <a name="claims-transformations-reference"></a>Claims transformaties verwijzing

Zie de volgende pagina's met naslaginformatie voor voorbeelden van claimtransformaties:

- [Boolean](boolean-transformations.md)
- [datum](date-transformations.md)
- [Geheel getal zijn](integer-transformations.md)
- [JSON](json-transformations.md)
- [Algemeen](general-transformations.md)
- [Sociaal account](social-transformations.md)
- [String](string-transformations.md)
- [StringCollection](stringcollection-transformations.md)

