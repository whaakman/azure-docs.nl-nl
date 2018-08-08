---
title: Een upgrade naar de meest recente versie van Azure Search Service REST API | Microsoft Docs
description: Een upgrade naar de meest recente versie van Azure Search Service REST API
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 2efe7769f68988f3c0d52c8806b78c1b96d8c639
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620226"
---
# <a name="upgrading-to-the-latest-azure-search-service-rest-api-version"></a>Een upgrade naar de meest recente versie van Azure Search Service REST API
Als u een eerdere versie van de [Azure Search Service REST API](https://docs.microsoft.com/rest/api/searchservice/), in dit artikel ziet u een upgrade van uw toepassing voor het gebruik van de meest recente algemeen beschikbaar API-versie 2017-11-11.

Versie 2017-11-11 van de REST-API bevat enkele wijzigingen van eerdere versies. Dit zijn meestal achterwaarts compatibel, zodat u uw code wijzigt, moet alleen minimale inspanning, afhankelijk van welke versie u hebt gebruikt vóór nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe API-versie.

> [!NOTE]
> Het exemplaar van uw Azure Search-service ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt echter ook doorgaan met een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2017-11-11"></a>Wat is er nieuw in versie 2017-11-11
Versie 2017-11-11 is de meest recente algemeen beschikbare versie van de Azure Search Service REST API. Nieuwe functies in deze API-versie zijn onder andere:

* [Synoniemen](search-synonyms.md). De nieuwe synoniemenfunctie voor kunt u definiëren equivalente termen en vouw het bereik van de query.
* [Ondersteuning voor het efficiënt indexeren tekst blobs](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#IndexingPlainText). De nieuwe `text` parseermodus voor Azure Blob-indexeerfuncties aanzienlijk verbetert de prestaties van indexering.
* [Statistieken API-service](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics). Bekijk de huidige gebruik en limieten van resources in Azure Search met deze nieuwe API.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het bijwerken
Als u een upgrade van een GA-versie uitvoert 2015-02-28 of 2016-09-01, u waarschijnlijk geen te wijzigen in uw code, anders dan het versienummer wijzigen. De enige situaties waarin u mogelijk nodig hebt om code te wijzigen zijn wanneer:

* Uw code mislukt wanneer het niet-herkende eigenschappen worden geretourneerd in een API-reactie. Standaard moet uw toepassing worden genegeerd eigenschappen die niet wordt herkend.
* Uw code zich blijft voordoen API-aanvragen en wil ze opnieuw te verzenden naar de nieuwe API-versie. Dit kan bijvoorbeeld gebeuren als uw toepassing zich blijft voortzetting van tokens die zijn geretourneerd door de Search-API voordoen (Zoek voor meer informatie, `@search.nextPageParameters` in de [Search API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Als een van deze situaties op u van toepassing, moet u mogelijk uw code dienovereenkomstig wijzigen. Anders geen wijzigingen nodig moeten zijn, tenzij u wilt beginnen met behulp van de [nieuwe functies](#WhatsNew) van 2017-11-11-versie.

Als u een van een preview-api-versie upgrade, de bovenstaande ook van toepassing, maar u wel rekening mee dat sommige preview-functies zijn niet beschikbaar in versie 2017-11-11:

* Indexeerfunctie-ondersteuning van de Azure Blob-opslag voor CSV-bestanden en blobs met JSON-matrices.
* 'Meer als volgt' query 's

Als uw code maakt gebruik van deze functies, wordt het niet mogelijk om te upgraden naar 2017-11-11 zonder te verwijderen van uw gebruik van deze.

> [!IMPORTANT]
> . Houd er rekening mee, preview-API's zijn bedoeld voor test- en evaluatiedoeleinden en mag niet worden gebruikt in een productieomgeving.
> 
> 

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van de Azure Search Service REST API nodig hebt, raadpleegt u de onlangs bijgewerkte [API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/) op MSDN.

We stellen uw feedback op Azure Search. Als u problemen ondervindt, gerust ons vragen voor meer informatie over op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) of [StackOverflow](http://stackoverflow.com/). Als u bent een vraag stellen over Azure Search op StackOverflow, zorg ervoor dat u code met `azure-search`.

Hartelijk dank voor het gebruik van Azure Search.

