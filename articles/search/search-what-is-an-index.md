---
title: Een Azure Search-index maken | Microsoft Azure | Gehoste service voor zoeken in de cloud
description: Wat is een index in Azure Search en hoe wordt deze gebruikt?
services: search
documentationcenter: 
author: ashmaka
ms.assetid: a395e166-bf2e-4fca-8bfc-116a46c5f7b1
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 7fc45273c0f71c727b7087949cc63bbb4111f866
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-search-index"></a>Een Azure Search-index maken
> [!div class="op_single_selector"]
> * [Overzicht](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

## <a name="what-is-an-index"></a>Wat is een index?
Een *index* is een permanente opslag van *documenten* en andere constructies die worden gebruikt door een Azure Search-service. Een document is een eenheid die bestaat uit gegevens die kunnen worden doorzocht in uw index. Een e-commercedetailhandel heeft bijvoorbeeld een document voor elk item dat wordt verkocht, een nieuwsbureau heeft een document voor elk artikel, enzovoort. Deze begrippen aan betrouwbaardere database-equivalenten toewijzen: een *index* lijkt conceptueel gezien op een *tabel* en *documenten* lijken ruwweg op *rijen* in een tabel.

Wanneer u documenten toevoegt of uploadt en zoekopdrachten naar Azure Search verzendt, worden uw aanvragen naar een specifieke index in uw zoekservice verzonden.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Veldtypen en kenmerken in een Azure Search-index
Bij het definiëren van het schema moet u de naam, het type en de kenmerken van elk veld in de index opgeven. Het veldtype classificeert de gegevens die in dat veld worden opgeslagen. Kenmerken worden ingesteld op afzonderlijke velden om op te geven hoe het veld wordt gebruikt. De volgende tabellen bevatten de typen en kenmerken die u kunt opgeven.

### <a name="field-types"></a>Veldtypen
| Type | Beschrijving |
| --- | --- |
| *Edm.String* |Tekst die van tokens kan worden voorzien om te zoeken in de volledige tekst (woordafbreking, afleiding, enzovoort). |
| *Collection(Edm.String)* |Een lijst met tekenreeksen die van tokens kan worden voorzien om te zoeken in de volledige tekst. Er is geen theoretische bovengrens voor het aantal items in een verzameling, maar de bovengrens van 16 MB voor de nettolading geldt voor alle verzamelingen. |
| *Edm.Boolean* |Bevat de waarden waar/niet waar. |
| *Edm.Int32* |32-bits waarden van een heel getal. |
| *Edm.Int64* |64-bits waarden van een heel getal. |
| *Edm.Double* |Numerieke gegevens met dubbele precisie. |
| *Edm.DateTimeOffset* |Datum- en tijdwaarden die worden weergegeven in de OData-V4-indeling (bijvoorbeeld `yyyy-MM-ddTHH:mm:ss.fffZ` of `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Een punt voor een geografische locatie op de wereld. |

Gedetailleerdere informatie over ondersteunde Azure-Search[-gegevenstypen vindt u hier](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="field-attributes"></a>Veldkenmerken
| Kenmerk | Beschrijving |
| --- | --- |
| *Sleutel* |Een tekenreeks met de unieke id van elk document. Deze reeks wordt gebruikt om op te zoeken. Elke index moet een sleutel hebben. Slechts één veld kan de sleutel zijn en het type moet zijn ingesteld op Edm.String. |
| *Ophalen mogelijk* |Hiermee geeft u op of een veld in een zoekresultaat kan worden geretourneerd. |
| *Filterbaar* |Hiermee kan het veld in een filterquery's worden gebruikt. |
| *Sorteerbaar* |Hiermee kan een query de zoekresultaten sorteren op basis van dit veld. |
| *Facetten mogelijk* |Hiermee kunt u een veld gebruiken in een [meervoudige navigatie](search-faceted-navigation.md)structuur om op de gebruiker te filteren. Doorgaans werken velden met terugkerende waarden die u kunt gebruiken om meerdere documenten te groeperen (bijvoorbeeld meerdere documenten die in een bepaalde merk- of servicecategorie vallen) het beste als facetten. |
| *Doorzoekbaar* |Hiermee kunt u in dit veld in de volledige tekst zoeken. |

Gedetailleerdere informatie over ondersteunde Azure-Search[-indexkenmerken vindt u hier](https://docs.microsoft.com/rest/api/searchservice/Create-Index).

## <a name="guidance-for-defining-an-index-schema"></a>Richtlijnen voor het definiëren van een indexschema
Bij het ontwerpen van uw index moet u in de planningsfase de tijd nemen om elke beslissing goed te overwegen. Als u uw index opzet, is het belangrijk dat u in uw achterhoofd houdt wat de gebruiker en uw bedrijf nodig hebben, aangezien aan elk veld de [relevante kenmerken](https://docs.microsoft.com/rest/api/searchservice/Create-Index) moeten worden toegewezen. Als u een index wilt wijzigen nadat deze is geïmplementeerd, moet u de gegevens opnieuw opbouwen en laden.

Als u de vereisten voor gegevensopslag wilt wijzigen, kunt u de capaciteit vergroten of verkleinen door partities toe te voegen of te verwijderen. Zie[Uw zoekservice in Azure beheren](search-manage.md) of [Servicelimieten](search-limits-quotas-capacity.md) voor meer informatie.

