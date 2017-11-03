---
title: Een upgrade naar de Azure Search Service REST API versie 2016-09-01 | Microsoft Docs
description: Een upgrade naar de Azure Search Service REST API versie 2016-09-01
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: f6a189c2e314b91c490583a86d8bacca8ec78a0f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="upgrading-to-the-azure-search-service-rest-api-version-2016-09-01"></a>Een upgrade naar de Azure Search Service REST API versie 2016-09-01
Als u versie 2015-02-28 of 2015-02-28-Preview van de [Azure Search Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), dit artikel helpt u bij het bijwerken van uw toepassing gebruik de volgende algemeen beschikbaar API-versie, 2016-09-01.

Versie 2016-09-01 van de REST API bevat een aantal wijzigingen uit eerdere versies. Dit zijn vooral achterwaarts compatibel, zodat uw code te wijzigen, moet alleen minimale inspanning, afhankelijk van welke versie u hebt gebruikt vóór nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe API-versie.

> [!NOTE]
> Uw Azure Search-service-exemplaar ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt blijven gebruiken van een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a>Wat is er nieuw in versie 2016-09-01
Versie 2016-09-01 is de tweede algemeen beschikbaar versie van de Azure Search Service REST-API. Nieuwe functies in deze API-versie:

* [Aangepaste analyzers](https://aka.ms/customanalyzers), waarmee u controle over het proces van het converteren van tekst in worden geïndexeerd en doorzoekbare tokens krijgen.
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) en [Azure Table Storage](search-howto-indexing-azure-tables.md) indexeerfuncties waarmee u kunt eenvoudig gegevens importeren uit Azure storage in Azure Search op een planning of op aanvraag.
* [Veld toewijzingen](search-indexer-field-mappings.md), waarmee u aanpassen hoe indexeerfuncties gegevens importeren in Azure Search.
* ETags, waarmee u kunt de definities van indexen, Indexeerfuncties en gegevensbronnen op een gelijktijdigheid-veilige manier bijwerken. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het upgraden
Als u een van versie 2015-02-28 upgrade, hoeft u waarschijnlijk te wijzigingen aanbrengt aan uw code anders dan het versienummer wijzigen. De enige situaties waarin u code wijzigen mogelijk moet zijn wanneer:

* Uw code mislukt wanneer het niet-herkende eigenschappen worden geretourneerd in een API-reactie. Standaard moet worden genegeerd in uw toepassing eigenschappen die niet wordt herkend.
* Uw code zich blijft voordoen API-aanvragen en probeert opnieuw te verzenden naar de nieuwe API-versie. Bijvoorbeeld: dit kan gebeuren als uw toepassing blijft voortzetting tokens die zijn geretourneerd door de Search-API draaien (voor meer informatie zoekt `@search.nextPageParameters` in de [Search API Reference](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).

Als beide situaties van toepassing is, moet u mogelijk uw code dienovereenkomstig wijzigen. Anders wordt er geen wijzigingen moeten niet nodig, tenzij u wilt beginnen met de [nieuwe functies](#WhatsNew) van 2016-09-01-versie.

Als u een van versie 2015-02-28-Preview upgrade, de bovenstaande ook van toepassing, maar u moet ook rekening houden sommige preview-functies zijn niet beschikbaar in versie 2016-09-01:

* Azure Blob Storage-indexeerfunctie ondersteuning voor CSV-bestanden en blobs met JSON-matrices.
* Synoniemen
* 'Meer als volgt' query 's

Als uw code deze functies gebruikt, is het niet mogelijk om te werken naar 2016-09-01 zonder uw gebruik ervan worden verwijderd.

> [!IMPORTANT]
> Maak onthouden, preview-API's zijn bedoeld voor testen en evalueren en mag niet worden gebruikt in een productieomgeving.
> 
> 

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van de Azure Search Service REST API, Zie de onlangs bijgewerkt [API Reference](https://msdn.microsoft.com/library/azure/dn798935.aspx) op MSDN.

Uw feedback is Welkom op Azure Search. Als u problemen ondervindt, gerust ons om hulp te vragen op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) of [StackOverflow](http://stackoverflow.com/). Als u een vraag over de Azure Search op StackOverflow vraagt, controleert u of deze met een label `azure-search`.

Hartelijk dank voor het gebruik van Azure Search.

