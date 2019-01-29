---
title: TechnicalProfiles | Microsoft Docs
description: Geef het TechnicalProfiles-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: c197ead3a7b0f61b9dd5f1e3ea0bd197c23a2778
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180761"
---
# <a name="technicalprofiles"></a>TechnicalProfiles

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Een **TechnicalProfiles** element bevat een set technische profielen die worden ondersteund door de claimprovider. Elke claimprovider beschikken over een of meer technische profielen die bepalen de eindpunten en de protocollen die nodig zijn om te communiceren met de claimprovider. Een claimprovider kan meerdere technische profielen hebben.

```XML
<ClaimsProvider>
  <DisplayName>Display name</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Technical profile identifier">
      <DisplayName>Display name of technical profile</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">URL of service</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
      </Metadata>
      <InputTokenFormat>JWT</InputTokenFormat>
      <OutputTokenFormat>JWT</OutputTokenFormat>
      <CryptographicKeys>
        <Key ID="Key identifier" StorageReferenceId="Storage key container identifier"/>
        ...
      </CryptographicKeys>
      <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <InputClaimsTransformations>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </PersistedClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <OutputClaimsTransformations>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="Technical profile identifier" />
        ...
      </ValidationTechnicalProfiles>
      <SubjectNamingInfo ClaimType="Claim type identifier" />
      <IncludeTechnicalProfile ReferenceId="Technical profile identifier" />
      <UseTechnicalProfileForSessionManagement ReferenceId="Technical profile identifier" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

De **TechnicalProfile** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
|---------|---------|---------|
| Id | Ja | Een unieke id van het technische profiel. Het technische profiel kan worden verwezen met behulp van deze id van andere elementen in het beleid-bestand. Bijvoorbeeld, **OrchestrationSteps** en **ValidationTechnicalProfile**. |

De **TechnicalProfile** bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| Domain | 0:1 | De domeinnaam voor het technische profiel. Bijvoorbeeld, als uw technisch profiel Hiermee geeft u de Facebook-id-provider, is de domeinnaam Facebook.com. |
| DisplayName | 0:1 | De naam van het technische profiel die kan worden weergegeven aan gebruikers. |
| Description | 0:1 | De beschrijving van het technische profiel die kan worden weergegeven aan gebruikers. |
| Protocol | 0:1 | Het protocol dat wordt gebruikt voor de communicatie met de andere partij. |
| Metagegevens | 0:1 | Een verzameling van sleutel/waarde-paren die worden gebruikt door het protocol voor communicatie met het eindpunt in de loop van een transactie. |
| InputTokenFormat | 0:1 | De indeling van de invoer-token. Mogelijke waarden: `JSON`, `JWT`, `SAML11`, of `SAML2`. De `JWT` waarde vertegenwoordigt een JSON Web Token aan de hand van IETF-specificatie. De `SAML11` waarde vertegenwoordigt een beveiligingstoken SAML 1.1 aan de hand van OASIS-specificatie.  De `SAML2` waarde vertegenwoordigt een SAML 2.0-security-token aan de hand van OASIS-specificatie. |
| OutputTokenFormat | 0:1 | De indeling van de token van de uitvoer. Mogelijke waarden: `JSON`, `JWT`, `SAML11`, of `SAML2`. |
| CryptographicKeys | 0:1 | Een lijst van cryptografische sleutels die worden gebruikt in het technische profiel. |
| InputClaimsTransformations | 0:1 | Een lijst met vooraf gedefinieerde verwijzingen naar claimtransformaties die moeten worden uitgevoerd voordat er claims worden verzonden naar de claimprovider of de relying party. |
| InputClaims | 0:1 | Een lijst van de eerder gedefinieerde verwijzingen naar claimtypen die worden uitgevoerd als invoer in het technische profiel. |
| PersistedClaims | 0:1 | Een lijst van de eerder gedefinieerde verwijzingen naar claimtypen die door de claimprovider die is gekoppeld aan het technische profiel zijn opgeslagen. |
| OutputClaims | 0:1 | Een lijst van de eerder gedefinieerde verwijzingen naar claimtypen die worden uitgevoerd als uitvoer in het technische profiel. |
| OutputClaimsTransformations | 0:1 | Een lijst met vooraf gedefinieerde verwijzingen naar claimtransformaties die moeten worden uitgevoerd nadat de claims worden ontvangen van de claimprovider. |
| ValidationTechnicalProfiles | 0: n | Een lijst met verwijzingen naar andere technische profielen die gebruikmaakt van het technische profiel ter validatie. Zie voor meer informatie, [validatie technisch profiel](validation-technical-profile.md)|
| SubjectNamingInfo | 0:1 | Hiermee bepaalt u de productie van de onderwerpnaam in tokens waarbij de naam van het onderwerp op een afzonderlijk van claims is opgegeven. Bijvoorbeeld, OAuth of SAML.  |
| IncludeClaimsFromTechnicalProfile | 0:1 | Een id van een technisch profiel van waaruit u wilt dat alle van de invoer- en claims moeten worden toegevoegd aan het technische profiel. De waarnaar wordt verwezen, technisch profiel moet worden gedefinieerd in hetzelfde beleidsbestand. | 
| IncludeTechnicalProfile |0:1 | Een id van een technisch profiel van waaruit u wilt dat alle gegevens moeten worden toegevoegd aan het technische profiel. De waarnaar wordt verwezen, technisch profiel moet zich in hetzelfde beleidsbestand. |
| UseTechnicalProfileForSessionManagement | 0:1 | Een ander technisch profiel moet worden gebruikt voor het sessiebeheer van de. |
|EnabledForUserJourneys| 0:1 |Bepaalt of het technische profiel wordt uitgevoerd in een gebruikersbeleving.  |

### <a name="protocol"></a>Protocol

De **Protocol** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Name | Ja | De naam van een geldig protocol dat wordt ondersteund door Azure AD B2C die wordt gebruikt als onderdeel van het technische profiel. Mogelijke waarden: `OAuth1`, `OAuth2`, `SAML2`, `OpenIdConnect`, `WsFed`, `WsTrust`, `Proprietary`, `session management`, `self-asserted`, of `None`. |
| Handler | Nee | Als de protocolnaam van het is ingesteld op `Proprietary`, geef de volledig gekwalificeerde naam van de assembly die door Azure AD B2C wordt gebruikt om te bepalen van de protocolhandler. |

### <a name="metadata"></a>Metagegevens

Een **metagegevens** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| Item | 0: n | De metagegevens die is gekoppeld aan het technische profiel. Elk type van het technische profiel heeft een andere set metagegevensitems. Zie het gedeelte van een technisch profiel typen, voor meer informatie. |

#### <a name="item"></a>Item

De **Item** element van de **metagegevens** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Sleutel | Ja | De van de metagegevenssleutel. Zie voor de lijst met metagegevensitems van elk type technisch profiel. |

### <a name="cryptographickeys"></a>CryptographicKeys

De **CryptographicKeys** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| Sleutel | 1: n | Een cryptografische sleutel die wordt gebruikt in dit technisch profiel. |

#### <a name="key"></a>Sleutel

De **sleutel** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Nee | Een unieke id van een bepaalde sleutelpaar waarnaar wordt verwezen vanuit andere elementen in het beleid-bestand. |
| StorageReferenceId | Ja | Een id van een storage-container sleutel waarnaar wordt verwezen vanuit andere elementen in het beleid-bestand. |

### <a name="inputclaimstransformations"></a>InputClaimsTransformations

De **InputClaimsTransformations** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| InputClaimsTransformation | 1: n | De id van een claimtransformatie die moet worden uitgevoerd voordat er claims worden verzonden naar de claimprovider of de relying party. Een claimtransformatie kan worden gebruikt om claims van bestaande ClaimsSchema wijzigen of nieuwe labels te genereren. |

#### <a name="inputclaimstransformation"></a>InputClaimsTransformation

De **InputClaimsTransformation** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| referenceId | Ja | Een id van een claimtransformatie is al gedefinieerd in het beleid of bovenliggende beleid-bestand. |

### <a name="inputclaims"></a>InputClaims

De **InputClaims** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| InputClaim | 1: n | Een verwachte invoer claimtype. |

#### <a name="inputclaim"></a>InputClaim 

De **InputClaim** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Ja | De id van een claimtype al gedefinieerd in de sectie ClaimsSchema in het beleid of bovenliggende beleid-bestand. |
| Standaardwaarde | Nee | Een standaardwaarde te gebruiken voor het maken van een claim als de claim door ClaimTypeReferenceId aangegeven bestaat niet, zodat de resulterende claim kan worden gebruikt als een InputClaim door het technische profiel. |
| PartnerClaimType | Nee | De id van het claimtype van de externe partner die het opgegeven beleid claimtype worden toegewezen aan. Als het kenmerk PartnerClaimType niet opgegeven is, is klikt u vervolgens het claimtype opgegeven beleid toegewezen aan het claimtype partner met dezelfde naam. Gebruik deze eigenschap wanneer de naam van het type claim af van de andere partij wijkt. Bijvoorbeeld, is de claimnaam van de eerste 'givenName', terwijl een claim met de naam 'first_name' maakt gebruik van de partner. |

### <a name="persistedclaims"></a>PersistedClaims

De **PersistedClaims** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| PersistedClaim | 1: n | Het claimtype om vast te leggen. |

#### <a name="persistedclaim"></a>PersistedClaim 

De **PersistedClaim** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Ja | De id van een claimtype al gedefinieerd in de sectie ClaimsSchema in het beleid of bovenliggende beleid-bestand. |
| Standaardwaarde | Nee | Een standaardwaarde te gebruiken voor het maken van een claim als de claim door ClaimTypeReferenceId aangegeven bestaat niet, zodat de resulterende claim kan worden gebruikt als een InputClaim door het technische profiel. |
| PartnerClaimType | Nee | De id van het claimtype van de externe partner die het opgegeven beleid claimtype worden toegewezen aan. Als het kenmerk PartnerClaimType niet opgegeven is, is klikt u vervolgens het claimtype opgegeven beleid toegewezen aan het claimtype partner met dezelfde naam. Gebruik deze eigenschap wanneer de naam van het type claim af van de andere partij wijkt. Bijvoorbeeld, is de claimnaam van de eerste 'givenName', terwijl een claim met de naam 'first_name' maakt gebruik van de partner. |

### <a name="outputclaims"></a>OutputClaims

De **OutputClaims** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| OutputClaim | 1: n | Verwachte uitvoer claimtype. |

#### <a name="outputclaim"></a>OutputClaim 

De **OutputClaim** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Ja | De id van een claimtype al gedefinieerd in de sectie ClaimsSchema in het beleid of bovenliggende beleid-bestand. |
| Standaardwaarde | Nee | Een standaardwaarde te gebruiken voor het maken van een claim als de claim door ClaimTypeReferenceId aangegeven bestaat niet, zodat de resulterende claim kan worden gebruikt als een InputClaim door het technische profiel. |
|AlwaysUseDefaultValue |Nee |Forceren dat de standaardwaarde wordt gebruikt.  |
| PartnerClaimType | Nee | De id van het claimtype van de externe partner die het opgegeven beleid claimtype worden toegewezen aan. Als het kenmerk PartnerClaimType niet opgegeven is, is klikt u vervolgens het claimtype opgegeven beleid toegewezen aan het claimtype partner met dezelfde naam. Gebruik deze eigenschap wanneer de naam van het type claim af van de andere partij wijkt. Bijvoorbeeld, is de claimnaam van de eerste 'givenName', terwijl een claim met de naam 'first_name' maakt gebruik van de partner. |

### <a name="outputclaimstransformations"></a>OutputClaimsTransformations

De **OutputClaimsTransformations** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| OutputClaimsTransformation | 1: n | De id's van claimtransformaties die moeten worden uitgevoerd voordat er claims worden verzonden naar de claimprovider of de relying party. Een claimtransformatie kan worden gebruikt om claims van bestaande ClaimsSchema wijzigen of nieuwe labels te genereren. |

#### <a name="outputclaimstransformation"></a>OutputClaimsTransformation

De **OutputClaimsTransformation** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| referenceId | Ja | Een id van een claimtransformatie is al gedefinieerd in het beleid of bovenliggende beleid-bestand. |

### <a name="validationtechnicalprofiles"></a>ValidationTechnicalProfiles

De **ValidationTechnicalProfiles** element bevat het volgende element:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| ValidationTechnicalProfile | 1: n | De id's van technische profielen die worden gebruikt valideren sommige of alle van de uitvoerclaims van de verwijzende technisch profiel. Alle van de invoer claims van de waarnaar wordt verwezen, technisch profiel moet worden weergegeven in de uitvoerclaims van de verwijzende technisch profiel. |

#### <a name="validationtechnicalprofile"></a>ValidationTechnicalProfile

De **ValidationTechnicalProfile** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| referenceId | Ja | Een id van een technisch profiel al gedefinieerd in het beleid of bovenliggende beleid-bestand. |

###  <a name="subjectnaminginfo"></a>SubjectNamingInfo

De **SubjectNamingInfo** bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| ClaimType | Ja | Een id van een claimtype al gedefinieerd in de sectie ClaimsSchema in het beleid-bestand. |

### <a name="includetechnicalprofile"></a>IncludeTechnicalProfile

De **IncludeTechnicalProfile** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| referenceId | Ja | Een id van een technisch profiel al gedefinieerd in het beleid of bovenliggende beleid-bestand. |

### <a name="usetechnicalprofileforsessionmanagement"></a>UseTechnicalProfileForSessionManagement

De **UseTechnicalProfileForSessionManagement** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| referenceId | Ja | Een id van een technisch profiel al gedefinieerd in het beleid of bovenliggende beleid-bestand. |

### <a name="enabledforuserjourneys"></a>EnabledForUserJourneys
De **ClaimsProviderSelections** in een gebruiker reis definieert u de lijst met opties voor de selectie van claims-provider en de volgorde daarvan. Met de **EnabledForUserJourneys** element dat u hebt gefilterd, welke claimprovider beschikbaar voor de gebruiker is. De **EnabledForUserJourneys** element bevat een van de volgende waarden:

- **Altijd**, uitvoeren van het technische profiel.
- **Nooit**, het technische profiel overslaan. 
- **OnClaimsExistence** uitvoeren alleen wanneer een bepaalde claim, opgegeven in het technische profiel bestaat. 
- **OnItemExistenceInStringCollectionClaim**, alleen wanneer een item bestaat in een tekenreeks verzameling claim worden uitgevoerd. 
- **OnItemAbsenceInStringCollectionClaim** alleen wanneer een item bestaat niet in een tekenreeks verzameling claim worden uitgevoerd.

Met behulp van **OnClaimsExistence**, **OnItemExistenceInStringCollectionClaim** of **OnItemAbsenceInStringCollectionClaim**, moet u het volgende opgeven metagegevens: **ClaimTypeOnWhichToEnable** Hiermee geeft u het claimtype moet worden geëvalueerd, **ClaimValueOnWhichToEnable** Hiermee geeft u de waarde die moet worden vergeleken.

De volgende technische profiel wordt alleen uitgevoerd als de **identityProviders** tekenreeks verzameling bevat de waarde van `facebook.com`:

```XML
<TechnicalProfile Id="UnLink-Facebook-OAUTH">
  <DisplayName>Unlink Facebook</DisplayName>
...
    <Metadata>
        <Item Key="ClaimTypeOnWhichToEnable">identityProviders</Item>
        <Item Key="ClaimValueOnWhichToEnable">facebook.com</Item>
    </Metadata>        
...
  <EnabledForUserJourneys>OnItemExistenceInStringCollectionClaim</EnabledForUserJourneys>
</TechnicalProfile>  
```












