---
title: Azure Search Service REST API-versie 2016 09 01-Preview-| Microsoft Docs
description: Azure Search Service REST API-versie 2016-09-01-Preview bevat experimentele functies zoals moreLikeThis zoekopdrachten.
author: mhko
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: reference
ms.date: 04/18/2018
ms.author: nateko
ms.openlocfilehash: 8eae54c912711a11c015737903b6898b98fd5159
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Azure Search Service REST-API: 2016-versie-09-01-Preview
Dit artikel is de documentatie bij `api-version=2016-09-01-Preview`. Deze preview breidt de huidige versie van de algemeen beschikbaar [api-version = 2016-09-01](https://docs.microsoft.com/rest/api/searchservice), door de volgende experimentele functies:

* [`moreLikeThis` queryparameter](search-more-like-this.md) om documenten die relevant voor een bepaald document zijn te vinden.

Zorg ervoor dat voor het doel van de preview-API-versie `api-version=2016-09-01-Preview` om te proberen deze experimentele functies. Het volgende voorbeeld ziet u hoe de preview-api-versie is opgegeven bij het maken van een meer achtige deze query.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Preview-functies zijn beschikbaar voor het testen en experimenteren met het doel van de feedback te verzamelen en nog worden gewijzigd. **We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.**

Azure Search-service is beschikbaar in meerdere versies. Raadpleeg [Search Serviceversiebeheer](https://docs.microsoft.com/azure/search/search-api-versions) voor meer informatie.
