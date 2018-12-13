---
title: API-versiebeheer voor .NET-SDK en REST-API's - Azure Search
description: Van het versiebeleid voor Azure Search REST API's en de clientbibliotheek in de .NET SDK.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 0cf5cac341cb36029c09ee2da5477323fac79cf5
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311640"
---
# <a name="api-versions-in-azure-search"></a>API-versies in Azure Search
Azure Search de regelmatig updates van de functie. Soms, maar niet altijd noodzakelijk een nieuwe versie van de API te behouden voor achterwaartse compatibiliteit. Publiceren van een nieuwe versie, kunt u bepalen wanneer en hoe u de search service-updates in uw code integreren.

Het team van Azure Search publiceert als een regel, nieuwe versies alleen indien nodig, omdat dit betrekking op sommige inspanning om uw code hebben kan voor het gebruik van een nieuwe API-versie te upgraden. Een nieuwe versie is alleen nodig als een bepaald aspect van de API is gewijzigd op een manier die achterwaartse compatibiliteit. Dergelijke wijzigingen kunnen gebeuren vanwege problemen in bestaande functies of vanwege de nieuwe functies waarmee bestaande API-gebied worden gewijzigd.

De dezelfde regel geldt voor SDK-updates. De Azure Search SDK volgt de [semantisch versiebeheer](http://semver.org/) regels, wat betekent dat de versie die bestaat uit drie delen: primaire, secundaire en build-nummer (bijvoorbeeld 1.1.0). Een nieuwe primaire versie van de SDK wordt vrijgegeven voor wijzigingen die voor achterwaartse compatibiliteit. Niet-belangrijke functie-updates van verkoopprognose nemen de secundaire versie en oplossingen voor problemen neemt alleen de build-versie.

> [!NOTE]
> Het exemplaar van uw Azure Search-service ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt echter ook doorgaan met een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK gebruikt, bepaalt de versie van de SDK die u gebruikt de corresponderende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven om uit te voeren die code zonder wijzigingen, zelfs als de service wordt bijgewerkt ter ondersteuning van een nieuwere API-versie.

## <a name="snapshot-of-current-versions"></a>Momentopname van de huidige versies
Hieronder wordt een momentopname van de huidige versies van alle interfaces voor het Azure Search programmeren.

| Interfaces | Meest recente primaire versie | Status |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |5.0 |In het algemeen beschikbaar, die zijn uitgebracht April 2018 |
| [Preview-versie van .NET SDK](https://aka.ms/search-sdk-preview) |4.0.1-Preview |Preview-versie, uitgebracht: mei 2017 |
| [Service REST API](https://docs.microsoft.com/rest/api/searchservice/) |2017-11-11 |Algemeen verkrijgbaar |
| [Service REST API-2017-11-11-Preview](search-api-2017-11-11-preview.md) |11-11-2017-preview |Preview |
| [.NET Management SDK](https://aka.ms/search-mgmt-sdk) |2.0 |Algemeen verkrijgbaar |
| [REST-API voor beheer](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Algemeen verkrijgbaar |

Voor de REST API's, met inbegrip van de `api-version` elke aanroep is vereist. Met behulp van `api-version` kunt u eenvoudig gericht op een specifieke versie, zoals een preview-API. Het volgende voorbeeld ziet u hoe de `api-version` parameter opgegeven:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2017-11-11

> [!NOTE]
> Hoewel elke aanvraag heeft een `api-version`, raden wij aan dat u dezelfde versie voor alle API-aanvragen. Dit is vooral van toepassing wanneer nieuwe API-versies introduceren kenmerken of bewerkingen die niet worden herkend door vorige versies. Met een combinatie van API-versies kan hebben ongewenste gevolgen en moeten worden vermeden.
>
> De REST API-Service en de REST API voor beheer zijn samengesteld onafhankelijk van elkaar. Er is overeenkomsten in versienummers toevallige.

Algemeen beschikbaar (of de algemene beschikbaarheid) API's kunnen worden gebruikt in productie en zijn onderworpen aan Azure serviceovereenkomsten. Preview-versies hebben experimentele functies die niet altijd zijn gemigreerd naar een GA-versie. **U wordt ten zeerste aangeraden om te voorkomen dat met behulp van de preview-API's in productietoepassingen.**

## <a name="about-preview-and-generally-available-versions"></a>Over versies Preview en algemeen beschikbaar
Azure Search versies altijd vooraf experimentele functies via de REST-API eerst, klikt u vervolgens via voorlopige versies van de .NET SDK.

Preview-functies zijn beschikbaar voor test- en experimenteren met het doel van het verzamelen van feedback op de functie ontwerpen en implementeren. Preview-functies kunnen daarom wijzigen gedurende een periode, mogelijk op een manier die achterwaartse compatibiliteit. Dit is in tegenstelling tot functies in een GA-versie, die zijn stabiel en waarschijnlijk niet zullen veranderen met uitzondering van kleine achterwaarts compatibel oplossingen en verbeteringen. Preview-functies kunnen niet altijd Zorg er ook voor het in een GA-versie.

Daarom raden we schrijven van productiecode die wordt afhankelijk van preview-versies. Als u een oudere versie van de Preview-versie, wordt u aangeraden de migratie naar de algemeen beschikbare (GA) versie.

Voor de .NET SDK: Richtlijnen voor migratie van code kan worden gevonden op [upgrade uitvoeren voor de .NET SDK](search-dotnet-sdk-migration.md).

Algemene beschikbaarheid betekent dat Azure Search nu onder de service level agreement (SLA is). De SLA kunt u vinden op [Azure Search Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
