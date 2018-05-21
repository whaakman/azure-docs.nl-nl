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
ms.openlocfilehash: e5cd7fcd0c853f03dbafb4d95b8459dcc83aecdf
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Azure Search Service REST-API: 2016-versie-09-01-Preview
Dit artikel bevat een overzicht van de preview-functies `api-version=2016-09-01-Preview`. Deze preview breidt de vorige versie van de algemeen beschikbaar [api-version = 2016-09-01](https://docs.microsoft.com/rest/api/searchservice), door de volgende experimentele functies:

* [`moreLikeThis` queryparameter](search-more-like-this.md) om documenten die relevant voor een bepaald document zijn te vinden. Deze functie is al in de eerdere previews. Als u deze API met een eerdere api-versie bellen, kunt u blijven gebruiken die versie.


## <a name="how-to-call-a-preview-api"></a>Het aanroepen van een preview-API

Het volgende voorbeeld ziet u hoe de preview-api-versie is opgegeven bij het maken van een meer achtige deze query.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Preview-functies zijn beschikbaar voor het testen en experimenteren met het doel van de feedback te verzamelen en nog worden gewijzigd. **We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.**

Azure Search-service is beschikbaar in meerdere versies. Raadpleeg [Search Serviceversiebeheer](https://docs.microsoft.com/azure/search/search-api-versions) voor meer informatie.
