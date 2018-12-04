---
title: BuildingBlocks - Azure Active Directory B2C | Microsoft Docs
description: Geef het BuildingBlocks-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 475c53a899c540389b512ff9acfc0f6b099b78df
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836301"
---
# <a name="buildingblocks"></a>BuildingBlocks

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De **BuildingBlocks** element wordt toegevoegd in de [TrustFrameworkPolicy](trustframeworkpolicy.md) element.

```XML
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">

  <BuildingBlocks>
    <ClaimsSchema>
      ...
    </ClaimsSchema>
    <Predicates>
    ...
    </Predicates>
    <PredicateValidations>
    ...
    </PredicateValidations>
    <ClaimsTransformations>
      ...
    </ClaimsTransformations>
    <ContentDefinitions>
      ...
    </ContentDefinitions>
    <Localization>
      ...
    </Localization>
 </BuildingBlocks>
```

De **BuildingBlocks** element bevat de volgende elementen die moeten worden opgegeven in de volgorde gedefinieerd:

- [ClaimsSchema](claimsschema.md) -definieert de claimtypen aan die kunnen worden verwezen als onderdeel van het beleid. Het schema van claims is de plek waar u uw claimtypen declareren. Een claimtype is vergelijkbaar met een variabele in vele programmatische talen. U kunt met het claimtype kunt verzamelen van gegevens van de gebruiker van uw toepassing, claims ontvangen van sociale id-providers, verzenden en ontvangen van gegevens van een aangepast REST-API of alle interne gegevens die worden gebruikt door het aangepaste beleid worden opgeslagen. 

- [Predikaten en PredicateValidationsInput](predicates.md) -kunt u een validatieproces om ervoor te zorgen dat alleen juist opgemaakte gegevens worden ingevoerd in een claim uitvoeren.
 
- [ClaimsTransformations](claimstransformations.md) -bevat een lijst met claimtransformaties die kunnen worden gebruikt in uw beleid.  Een claim converteert een claimtransformatie naar een andere. In de claimtransformatie geeft u een methode voor transformatie, zoals: 
    - Het wijzigen van het geval van een claim tekenreeks voor de versie die is opgegeven. Bijvoorbeeld, wijzigen van een tekenreeks van kleine letters naar hoofdletters.
    - Vergelijking van twee claims en een claim met de waarde true die aangeeft dat de claims, anders overeenkomen onwaar geretourneerd.
    - Het maken van een claim tekenreeks van de opgegeven parameter in het beleid.
    - Het maken van een willekeurige tekenreeks met behulp van de generator van willekeurige getallen.
    - Opmaak van een claim op basis van de opgegeven tekenreeks. Deze transformatie maakt gebruik van de C# `String.Format` methode.

- [ContentDefinitions](contentdefinitions.md) -URL bevat voor HTML5-sjablonen te gebruiken in de gebruikersbeleving. In een aangepast beleid definieert een inhoudsdefinitie de HTML5-pagina URI die wordt gebruikt voor een specifieke stap in de gebruikersbeleving. Bijvoorbeeld: het aanmelden of registreren voor wachtwoord opnieuw instellen of foutpagina's. U kunt het uiterlijk wijzigen door de LoadUri voor het HTML5-bestand te overschrijven. Of u kunt nieuwe inhoudsdefinities maken op basis van uw behoeften. Dit element mag een gelokaliseerde bronnen verwijzing met behulp van een lokalisatie-ID.

- [Lokalisatie](localization.md) -kunt u meerdere talen te ondersteunen. De lokalisatieondersteuning in het beleid kunt u de lijst met ondersteunde talen in een beleid instellen en kies een taal. Taalspecifieke tekenreeksen en verzamelingen worden ook ondersteund.


