---
title: Azure Search Service REST API-versie 2017 11 11-Preview-| Microsoft Docs
description: Azure Search Service REST API-versie 2017-11-11-Preview bevat experimentele functies zoals synoniemen en moreLikeThis zoekopdrachten.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2018
ms.author: HeidiSteen
ms.openlocfilehash: 3a9ff6697357dec443691b261ae6fc0477603a9c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure Search Service REST-API: Versie 2017-11-11-Preview
In dit artikel beschrijft de `api-version=2017-11-11-Preview` versie van Azure Search service REST API, bieden de volgende experimentele functies:

+ [Cognitieve zoeken](cognitive-search-concept-intro.md), een nieuwe verrijking mogelijkheid in Azure Search indexeren die latente informatie in niet-tekstwaarden bronnen en deze omzetten in volledige tekst doorzoekbare inhoud in Azure Search niet gedifferentieerde tekst wordt gevonden.

De preview-API is gelijk aan de algemeen beschikbaar API met uitzondering van de volgende twee bewerkingen:

+ [Vaardigheden maken (api-version = 2017-11-11-Preview)](ref-create-skillset.md)
+ [Maak indexeerfunctie (api-version = 2017-11-11-Preview)](ref-create-indexer.md)

Zorg ervoor dat de preview-API-versie als doel `api-version=2017-11-11-Preview` bij het evalueren van de functies van evaluatieversies. De voorbeeldsyntaxis van de volgende ziet u een aanroep van de preview-API-versie.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

> [!NOTE]
> Preview-functies zijn beschikbaar voor het testen en experimenteren met het doel van de feedback te verzamelen en nog worden gewijzigd. **We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.**

Azure Search-service is beschikbaar in meerdere versies. Raadpleeg [API-versies](search-api-versions.md) voor meer informatie.
