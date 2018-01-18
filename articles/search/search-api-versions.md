---
title: API-versies van Azure Search | Microsoft Docs
description: Versie-beleid voor Azure Search REST-API's en de clientbibliotheek in de .NET SDK.
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/15/2018
ms.author: brjohnst
ms.openlocfilehash: 1a7f5b9c53d3258e5f8eda40401c61a85971d8c7
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="api-versions-in-azure-search"></a>API-versies in Azure Search
Azure Search regelmatig updates van de functie is uitgevouwen. Soms, maar niet altijd nodig deze updates ons voor het publiceren van een nieuwe versie van de API voor achterwaartse compatibiliteit. Publiceren van een nieuwe versie, kunt u bepalen wanneer en hoe u de search-service-updates in uw code integreren.

We proberen als een regel voor het publiceren van nieuwe versies alleen indien nodig, omdat deze mogelijk moeite om uw code voor het gebruik van een nieuwe API-versie te upgraden. Er wordt een nieuwe versie alleen publiceren als we wilt wijzigen van een bepaald aspect van de API op een manier die achterwaartse compatibiliteit. Dit kan gebeuren vanwege verbeteringen in bestaande functies of vanwege de nieuwe functies die surface area van bestaande API wijzigen.

Er volgt u dezelfde regel voor SDK-updates. De Azure Search SDK volgt de [semantische versioning](http://semver.org/) regels, wat betekent dat de versie ervan bestaat uit drie delen: primaire, secundaire en build-nummer (bijvoorbeeld 1.1.0). We brengen een nieuwe primaire versie van de SDK alleen in geval van wijzigingen die problemen met achterwaartse compatibiliteit. Voor vaste functie-updates, wordt de secundaire versie wordt verhoogd en voor oplossingen voor problemen we alleen de build-versie wordt verhoogd.

> [!NOTE]
> Uw Azure Search-service-exemplaar ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt blijven gebruiken van een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK, bepaalt de versie van de SDK die u in de desbetreffende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven code uit te voeren die zonder wijzigingen zelfs als de service is bijgewerkt zodat een nieuwere versie van de API wordt ondersteund.

## <a name="snapshot-of-current-versions"></a>Momentopname van huidige versies
Hieronder wordt een momentopname van de huidige versies van alle interfaces voor Azure Search geprogrammeerd.

| Interfaces | Meest recente hoofdversie | Status |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |3.0 |In het algemeen beschikbaar, November 2016 wordt uitgebracht |
| [.NET SDK Preview](https://aka.ms/search-sdk-preview) |4.0.1-preview |Voorbeeld van mei 2017 uitgebracht |
| [Service REST API](https://docs.microsoft.com/rest/api/searchservice/) |2016-09-01 |Algemeen beschikbaar |
| [Service REST API-voorbeeld](search-api-2016-09-01-preview.md) |2016-09-01-preview |Preview |
| [.NET Management SDK](https://aka.ms/search-mgmt-sdk) |2.0 |Algemeen beschikbaar |
| [REST-API voor beheer](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Algemeen beschikbaar |

Voor de REST-API's, inclusief de `api-version` op elke aanroep is vereist. Hierdoor kunt gemakkelijk gericht op een specifieke versie, zoals een preview-API. Het volgende voorbeeld ziet u hoe de `api-version` parameter opgegeven:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2016-09-01

> [!NOTE]
> Hoewel elke aanvraag heeft een `api-version`, het is raadzaam dat u dezelfde versie voor alle API-aanvragen. Dit is vooral van toepassing wanneer nieuwe API-versies introduceren kenmerken of bewerkingen die niet worden herkend door vorige versies. De combinatie van API-versies kan hebben ongewenste gevolgen en moeten worden vermeden.
>
> De REST-API-Service en de REST-API Management zijn samengestelde onafhankelijk van elkaar. Er is overeenkomsten in versienummers toevallige.

Algemeen beschikbaar (of GA) API's kunnen worden gebruikt in productie en zijn onderworpen aan Azure serviceovereenkomsten. Preview-versies hebben experimentele functies die niet altijd worden gemigreerd naar een GA-versie. **We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.**

## <a name="about-preview-and-generally-available-versions"></a>Over de versies van Preview en algemeen beschikbaar
Azure Search versies altijd vooraf experimentele functies via de REST-API eerst vervolgens via voorlopige versies van de .NET SDK.

Preview-functies niet worden gegarandeerd dat wordt gemigreerd naar een GA-release. Terwijl de functies in een GA-versie worden beschouwd als stabiel en waarschijnlijk niet wijzigen met uitzondering van kleine neerwaarts compatibele oplossingen en verbeteringen, zijn preview-functies beschikbaar voor testen en experimenteren met het doel van het verzamelen van feedback op functie ontwerpen en implementeren.

Echter, omdat preview-functies nog worden gewijzigd, wordt u aangeraden tegen schrijven productiecode waarmee een afhankelijkheid op preview-versies. Als u van een oudere versie van de preview gebruikmaakt, raden wij migreren naar de versie van de algemeen beschikbaar (GA).

Voor de .NET SDK: richtlijnen voor de migratie van de code kan worden gevonden op [Upgrade van de .NET SDK](search-dotnet-sdk-migration.md).

Algemene beschikbaarheid betekent dat Azure Search nu onder service level agreement (SLA). De SLA kan worden gevonden op [Azure Search-Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
