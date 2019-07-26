---
title: BuildingBlocks-Azure Active Directory B2C | Microsoft Docs
description: Specificeer het element BuildingBlocks van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5ab2b11749aa57065a1a4d688b02fed97731ab7c
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464749"
---
# <a name="buildingblocks"></a>BuildingBlocks

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Het  element BuildingBlocks wordt toegevoegd aan het element [TrustFrameworkPolicy](trustframeworkpolicy.md) .

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

Het  element BuildingBlocks bevat de volgende elementen die moeten worden opgegeven in de gedefinieerde volg orde:

- [ClaimsSchema](claimsschema.md) : definieert de claim typen waarnaar kan worden verwezen als onderdeel van het beleid. Het claim schema is de plaats waar u uw claim typen declareert. Een claim type is vergelijkbaar met een variabele in veel programmeer talen. U kunt het claim type gebruiken om gegevens te verzamelen van de gebruiker van uw toepassing, claims van sociale id-providers te ontvangen, gegevens te verzenden en te ontvangen van een aangepaste REST API, of interne gegevens op te slaan die door uw aangepaste beleid worden gebruikt. 

- [Predikaten en PredicateValidationsInput](predicates.md) : Hiermee kunt u een validatie proces uitvoeren om ervoor te zorgen dat alleen goed gevormde gegevens in een claim worden ingevoerd.
 
- [ClaimsTransformations](claimstransformations.md) -bevat een lijst met claim transformaties die kunnen worden gebruikt in uw beleid.  Een claim transformatie zet een claim om in een andere. In de claim transformatie geeft u een transformatie methode op, zoals: 
    - Het hoofdletter gebruik van een teken reeks claim wijzigen naar de waarde die is opgegeven. U kunt bijvoorbeeld een teken reeks wijzigen van kleine letters in hoofd letters.
    - Vergelijken van twee claims en het retour neren van een claim met de waarde True, wat aangeeft dat de claims overeenkomen, anders false.
    - Er wordt een teken reeks claim gemaakt op basis van de gegeven para meter in het beleid.
    - Een wille keurige teken reeks maken met de generator voor wille keurige getallen.
    - Het format teren van een claim volgens de gegeven teken reeks voor opmaak. Deze trans formatie maakt C# `String.Format` gebruik van de-methode.
    
- InputValidation: met dit element kunt u Booleaanse aggregaties uitvoeren die vergelijkbaar zijn met *en* en *of*.

- [ContentDefinitions](contentdefinitions.md) : bevat URL'S voor HTML5-sjablonen die u in uw gebruikers traject kunt gebruiken. In een aangepast beleid definieert een inhouds definitie de HTML5-pagina-URI die wordt gebruikt voor een opgegeven stap in het traject van de gebruiker. Bijvoorbeeld het aanmelden of aanmelden, het opnieuw instellen van het wacht woord of de fout pagina's. U kunt het uiterlijk wijzigen door de LoadUri voor het HTML5-bestand te overschrijven. U kunt ook nieuwe inhouds definities maken op basis van uw behoeften. Dit element kan een gelokaliseerde bronnen verwijzing bevatten die gebruikmaakt van een lokalisatie-ID.

- [Lokalisatie](localization.md) : biedt ondersteuning voor meerdere talen. Met de lokalisatie ondersteuning in beleids regels kunt u de lijst met ondersteunde talen in een beleid instellen en een standaard taal kiezen. Taalspecifieke teken reeksen en verzamelingen worden ook ondersteund.


