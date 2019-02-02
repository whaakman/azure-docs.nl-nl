---
title: Sociaal account van de claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C | Microsoft Docs
description: Sociaal account claims transformatievoorbeelden voor de identiteit ervaring Framework-Schema van de Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 52ec7c83b4070a4c38963b3ab12f58f923fa889d
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55562621"
---
# <a name="social-accounts-claims-transformations"></a>Sociale accounts claimtransformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In Azure Active Directory (Azure AD) B2C, sociaal account-id's zijn opgeslagen in een `userIdentities` kenmerk van een **alternativeSecurityIdCollection** claimtype. Elk item in de **alternativeSecurityIdCollection** Hiermee geeft u de uitgever (identity providernaam, zoals facebook.com) en de `issuerUserId`, dit is een unieke gebruikers-id voor de verlener.

```JSON
"userIdentities": [{
    "issuer": "google.com",
    "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"
  },
  {
    "issuer": "facebook.com",
    "issuerUserId": "MTIzNDU="
  }]
```

In dit artikel biedt voorbeelden voor het gebruik van de transformaties sociaal account claims van het schema Identiteitservaring-Framework in Azure AD B2C. Zie voor meer informatie, [ClaimsTransformations](claimstransformations.md).

## <a name="createalternativesecurityid"></a>CreateAlternativeSecurityId

Hiermee maakt u een JSON-weergave van de gebruiker alternativeSecurityId eigenschap die kunnen worden gebruikt bij het aanroepen van Azure Active Directory. Zie voor meer informatie, [AlternativeSecurityId van schema](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#AlternativeSecurityIdType).

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | sleutel | string | Het ClaimType die Hiermee geeft u de unieke gebruikers-id die wordt gebruikt door de sociale id-provider. |
| InputClaim | identityProvider | string | Het ClaimType die Hiermee geeft u de naam sociaal account-id-provider, bijvoorbeeld facebook.com. |
| OutputClaim | alternativeSecurityId | string | Het ClaimType dat wordt gegenereerd nadat de ClaimsTransformation is aangeroepen. Bevat informatie over de identiteit van een gebruiker sociaal account. De **verlener** is de waarde van de `identityProvider` claim. De **issuerUserId** is de waarde van de `key` claim in Base 64-indeling. |

Gebruik dit claims transformatie voor het genereren van een `alternativeSecurityId` ClaimType. Deze wordt gebruikt door alle sociale id-provider technische profielen, zoals `Facebook-OAUTH`. De volgende claimtransformatie ontvangt de gebruiker sociaal account-ID en de naam van de id-provider. De uitvoer van deze technische profiel is een JSON-tekenreeks die kan worden gebruikt in Azure AD directory-services.

```XML
<ClaimsTransformation Id="CreateAlternativeSecurityId" TransformationMethod="CreateAlternativeSecurityId">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="socialIdpUserId" TransformationClaimType="key" />
    <InputClaim ClaimTypeReferenceId="identityProvider" TransformationClaimType="identityProvider" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="alternativeSecurityId" TransformationClaimType="alternativeSecurityId" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **sleutel**: 12334
    - **identityProvider**: Facebook.com
- Uitvoerclaims:
    - **alternativeSecurityId**: {"verlener": "facebook.com", 'issuerUserId': "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}

## <a name="additemtoalternativesecurityidcollection"></a>AddItemToAlternativeSecurityIdCollection

Voegt een `AlternativeSecurityId` naar een `alternativeSecurityIdCollection` claim.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | item | string | Het ClaimType worden toegevoegd aan de uitvoerclaim. |
| InputClaim | verzameling | alternativeSecurityIdCollection | De ClaimTypes die worden gebruikt door de claimtransformatie als beschikbaar in het beleid. Indien opgegeven, de claimtransformatie wordt toegevoegd de `item` aan het einde van de verzameling. |
| OutputClaim | verzameling | alternativeSecurityIdCollection | De ClaimTypes die worden gegenereerd nadat deze ClaimsTransformation is aangeroepen. De nieuwe verzameling met beide items uit de invoer `collection` en `item`. |

Het volgende voorbeeld wordt een nieuwe sociale identiteit met een bestaand account gekoppeld. Een nieuwe sociale identiteit koppelen:
1. In de **AAD-UserReadUsingAlternativeSecurityId** en **AAD-UserReadUsingObjectId** technische profielen uitvoer van de gebruiker **alternativeSecurityIds** claim.
1. Vraag de gebruiker zich aanmelden met een van de id-providers die niet gekoppeld aan deze gebruiker zijn.
1. Met behulp van de **CreateAlternativeSecurityId** claims transformatie, maakt u een nieuw **alternativeSecurityId** claimtype met de naam `AlternativeSecurityId2`
1. Roep de **AddItemToAlternativeSecurityIdCollection** claims transformatie om toe te voegen de **AlternativeSecurityId2** aan de bestaande claim **AlternativeSecurityIds** claim.
1. Behouden de **alternativeSecurityIds** claim aan het gebruikersaccount

```XML
<ClaimsTransformation Id="AddAnotherAlternativeSecurityId" TransformationMethod="AddItemToAlternativeSecurityIdCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId2" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **item**: {"verlener": "facebook.com", 'issuerUserId': "MTIzNDU=" }
    - **verzameling**: [{"verlener": "live.com", 'issuerUserId': "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw" } ]
- Uitvoerclaims:
    - **verzameling**: [{"verlener": "live.com", 'issuerUserId': "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}, {"verlener": "facebook.com", 'issuerUserId': "MTIzNDU=" } ]

## <a name="getidentityprovidersfromalternativesecurityidcollectiontransformation"></a>GetIdentityProvidersFromAlternativeSecurityIdCollectionTransformation

Retourneert lijst met certificaatverleners van de **alternativeSecurityIdCollection** in een nieuwe claim **stringCollection** claim.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | alternativeSecurityIdCollection | alternativeSecurityIdCollection | Het ClaimType moet worden gebruikt om op te halen van de lijst met id-providers (verlener). |
| OutputClaim | identityProvidersCollection | stringCollection | De ClaimTypes die worden gegenereerd nadat deze ClaimsTransformation is aangeroepen. Lijst met id-providers koppelen aan de invoerclaim alternativeSecurityIdCollection |

De volgende claimtransformatie leest de gebruiker **alternativeSecurityIds** claim en extraheert de lijst met namen van de id-providers die zijn gekoppeld aan dat account. Uitvoer gebruiken **identityProvidersCollection** om de lijst met id-providers die zijn gekoppeld aan het account van de gebruiker weer te geven. Of, in de lijst met id-providers op basis van uitvoer filteren op de pagina id-provider selecteren **identityProvidersCollection** claim. Gebruiker kan dus selecteren om te koppelen van nieuwe sociale identiteit die nog niet is gekoppeld aan het account.

```XML
<ClaimsTransformation Id="ExtractIdentityProviders" TransformationMethod="GetIdentityProvidersFromAlternativeSecurityIdCollectionTransformation">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityIds" TransformationClaimType="alternativeSecurityIdCollection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="identityProviders" TransformationClaimType="identityProvidersCollection" />
  </OutputClaims>
</ClaimsTransformation>
```

- Invoerclaims:
    - **alternativeSecurityIdCollection**: [ { "issuer": "google.com", "issuerUserId": "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}, {"verlener": "facebook.com", 'issuerUserId': "MTIzNDU=" } ]
- Uitvoerclaims:
    - **identityProvidersCollection**: ["facebook.com", "google.com"]

## <a name="removealternativesecurityidbyidentityprovider"></a>RemoveAlternativeSecurityIdByIdentityProvider

Hiermee verwijdert u een **AlternativeSecurityId** uit een **alternativeSecurityIdCollection** claim.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | identityProvider | string | Het ClaimType met de naam van de id-provider moet worden verwijderd uit de verzameling. |
| InputClaim | verzameling | alternativeSecurityIdCollection | De ClaimTypes die worden gebruikt door de claimtransformatie. De id-provider verwijdert de claimtransformatie uit de verzameling. |
| OutputClaim | verzameling | alternativeSecurityIdCollection | De ClaimTypes die worden gegenereerd nadat deze ClaimsTransformation is aangeroepen. De nieuwe verzameling, nadat de identityProvider is verwijderd uit de verzameling. |

Het volgende voorbeeld wordt een van de sociale identiteit met een bestaand account ontkoppeld. Een sociale id ontkoppelen:
1. In de **AAD-UserReadUsingAlternativeSecurityId** en **AAD-UserReadUsingObjectId** technische profielen uitvoer van de gebruiker **alternativeSecurityIds** claim.
2. Vraag de gebruiker selecteren welk sociale account verwijderen uit de lijst met id-providers die gekoppeld aan deze gebruiker zijn.
3. Aanroepen van een claims transformatie technisch profiel die roept de **RemoveAlternativeSecurityIdByIdentityProvider** claims transformatie, die de geselecteerde sociale identiteit, met behulp van de naam van de id-provider wordt verwijderd.
4. Behouden de **alternativeSecurityIds** claim aan het gebruikersaccount.

```XML
<ClaimsTransformation Id="RemoveAlternativeSecurityIdByIdentityProvider" TransformationMethod="RemoveAlternativeSecurityIdByIdentityProvider">
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="secondIdentityProvider" TransformationClaimType="identityProvider" />
        <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
    </InputClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds" TransformationClaimType="collection" />
    </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformations>
```

### <a name="example"></a>Voorbeeld

- Invoerclaims:
    - **identityProvider**: facebook.com
    - **verzameling**: [{"verlener": "live.com", 'issuerUserId': "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw"}, {"verlener": "facebook.com", 'issuerUserId': "MTIzNDU=" } ]
- Uitvoerclaims:
    - **verzameling**: [{"verlener": "live.com", 'issuerUserId': "MTA4MTQ2MDgyOTI3MDUyNTYzMjcw" } ]
