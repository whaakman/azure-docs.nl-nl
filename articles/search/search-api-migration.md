---
title: Upgraden naar de meest recente versie van Azure Search Service REST API | Microsoft Docs
description: Upgraden naar de meest recente versie van Azure Search Service REST API
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 3848f317fd6d760961756f132edf9cbcb5f431ee
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32181967"
---
# <a name="upgrading-to-the-latest-azure-search-service-rest-api-version"></a>Upgraden naar de meest recente versie van Azure Search Service REST API
Als u een eerdere versie van de [Azure Search Service REST API](https://docs.microsoft.com/rest/api/searchservice/), dit artikel helpt u bij het bijwerken van uw toepassing gebruik van de meest recente algemeen beschikbaar API-versie, 2017-11-11.

Versie 2017-11-11 van de REST API bevat een aantal wijzigingen uit eerdere versies. Dit zijn vooral achterwaarts compatibel, zodat uw code te wijzigen, moet alleen minimale inspanning, afhankelijk van welke versie u hebt gebruikt vóór nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe API-versie.

> [!NOTE]
> Uw Azure Search-service-exemplaar ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt blijven gebruiken van een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2017-11-11"></a>Wat is er nieuw in versie 2017-11-11
Versie 2017-11-11 is de meest recente algemeen beschikbare versie van de Azure Search Service REST-API. Nieuwe functies in deze API-versie:

* [Synoniemen](search-synonyms.md). De nieuwe synoniemen-functie kunt u vergelijkbare termen definiëren en vouw het bereik van de query.
* [Ondersteuning voor het efficiënt indexeren tekst blobs](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#IndexingPlainText). De nieuwe `text` bij het parseren van modus voor Azure Blob-indexeerfuncties aanzienlijk verbeterd dankzij de indexering.
* [Service-statistieken API](https://aka.ms/azure-search-stats). De huidige gebruik en beperkingen van de resources in Azure Search met deze nieuwe API weergeven.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het upgraden
Als u een upgrade vanuit een GA-versie uitvoert hoeft 2015-02-28 of 2016-09-01, u waarschijnlijk te wijzigingen aanbrengt aan uw code anders dan het versienummer wijzigen. De enige situaties waarin u code wijzigen mogelijk moet zijn wanneer:

* Uw code mislukt wanneer het niet-herkende eigenschappen worden geretourneerd in een API-reactie. Standaard moet worden genegeerd in uw toepassing eigenschappen die niet wordt herkend.
* Uw code zich blijft voordoen API-aanvragen en probeert opnieuw te verzenden naar de nieuwe API-versie. Bijvoorbeeld: dit kan gebeuren als uw toepassing blijft voortzetting tokens die zijn geretourneerd door de Search-API draaien (voor meer informatie zoekt `@search.nextPageParameters` in de [Search API Reference](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Als beide situaties van toepassing is, moet u mogelijk uw code dienovereenkomstig wijzigen. Anders wordt er geen wijzigingen moeten niet nodig, tenzij u wilt beginnen met de [nieuwe functies](#WhatsNew) van 2017-11-11-versie.

Als u een upgrade van een preview-api-versie uitvoert, de bovenstaande ook van toepassing, maar u moet ook rekening houden sommige preview-functies zijn niet beschikbaar in versie 2017-11-11:

* Azure Blob Storage-indexeerfunctie ondersteuning voor CSV-bestanden en blobs met JSON-matrices.
* 'Meer als volgt' query 's

Als uw code deze functies gebruikt, is het niet mogelijk om te werken naar 2017-11-11 zonder uw gebruik ervan worden verwijderd.

> [!IMPORTANT]
> Maak onthouden, preview-API's zijn bedoeld voor testen en evalueren en mag niet worden gebruikt in een productieomgeving.
> 
> 

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van de Azure Search Service REST API, Zie de onlangs bijgewerkt [API Reference](https://docs.microsoft.com/rest/api/searchservice/) op MSDN.

Uw feedback is Welkom op Azure Search. Als u problemen ondervindt, gerust ons om hulp te vragen op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) of [StackOverflow](http://stackoverflow.com/). Als u een vraag over de Azure Search op StackOverflow vraagt, controleert u of deze met een label `azure-search`.

Hartelijk dank voor het gebruik van Azure Search.

