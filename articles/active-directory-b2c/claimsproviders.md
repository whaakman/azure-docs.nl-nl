---
title: ClaimsProviders - Azure Active Directory B2C | Microsoft Docs
description: Geef de ClaimsProvider-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 2d862d07a65c3fb28b49a82692ea575f787b9750
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60313848"
---
# <a name="claimsproviders"></a>ClaimsProviders

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Een claimprovider bestaat uit een set [technische profielen](technicalprofiles.md). Elke claimprovider beschikken over een of meer technische profielen die bepalen de eindpunten en de protocollen die nodig zijn om te communiceren met de claimprovider. Een claimprovider kan meerdere technische profielen hebben. Meerdere technische profielen kunnen bijvoorbeeld worden gedefinieerd omdat de claimprovider biedt ondersteuning voor meerdere protocollen, verschillende eindpunten met verschillende mogelijkheden, of andere claims op verschillende assurance niveaus worden vrijgegeven. Kan het zijn aanvaardbaar is voor het vrijgeven van gevoelige claims in een gebruikersbeleving, maar niet in een andere.

```XML
<ClaimsProviders>
  <ClaimsProvider>
    <Domain>Domain name</Domain>
    <DisplayName>Display name</DisplayName>
    <TechnicalProfiles>
      </TechnicalProfile>
        ...
      </TechnicalProfile>
        ...
    </TechnicalProfiles>
  </ClaimsProvider>
  ...
</ClaimsProviders>
```

De **ClaimsProviders** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| ClaimsProvider | 1: n | Een erkende claimprovider die kan worden gebruikt in verschillende gebruikers reizen. |

## <a name="claimsprovider"></a>ClaimsProvider

De **ClaimsProvider** element bevat de volgende onderliggende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ---------- | ----------- |
| Domain | 0:1 | Een tekenreeks zijn met de naam van de claimprovider. Bijvoorbeeld, als uw claimprovider het technische profiel Facebook bevat, is de domeinnaam Facebook.com. Deze domeinnaam wordt gebruikt voor alle technische profielen die zijn gedefinieerd in de claimprovider, tenzij deze overschreven door het technische profiel. Naam van het domein kan ook worden verwezen een **domain_hint**. Zie voor meer informatie de **aanmelden omleiden naar een provider voor sociale** sectie van [instellen direct aanmelden met behulp van Azure Active Directory B2C](direct-signin.md). |
| DisplayName | 0:1 | Een tekenreeks zijn met de naam van de claimprovider die kan worden weergegeven aan gebruikers. |
| [TechnicalProfiles](technicalprofiles.md) | 0:1 | Een set technische profielen die worden ondersteund door de claimprovider |

**ClaimsProvider** organiseert hoe uw technische profielen in verband met de claimprovider. Het volgende voorbeeld ziet u de Azure Active Directory-claimprovider met de technische Azure Active Directory-profielen:

```XML
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AAD-Common">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      ...
    </TechnicalProfile>
      ...
    <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWritePhoneNumberUsingObjectId">
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

Het volgende voorbeeld ziet de Facebook-claimprovider met de **Facebook-OAUTH** technisch profiel.

```XML
<ClaimsProvider>
  <Domain>facebook.com</Domain>
  <DisplayName>Facebook</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Facebook-OAUTH">
      <DisplayName>Facebook</DisplayName>
      <Protocol Name="OAuth2" />
        ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
