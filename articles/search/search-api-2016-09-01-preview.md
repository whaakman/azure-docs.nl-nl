---
title: Azure Search Service REST API-versie 2016 09 01-Preview-| Microsoft Docs
description: Azure Search Service REST API-versie 2016-09-01-Preview bevat experimentele functies zoals synoniemen en moreLikeThis zoekopdrachten.
services: search
documentationcenter: na
author: mhko
manager: jlembicz
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 10/25/2017
ms.author: nateko
ms.openlocfilehash: 082c207f892fcc277d30d66c6165dd9920d3ab27
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Azure Search Service REST-API: 2016-versie-09-01-Preview
Dit artikel is de documentatie bij `api-version=2016-09-01-Preview`. Deze preview breidt de huidige versie van de algemeen beschikbaar [api-version = 2016-09-01](https://msdn.microsoft.com/library/dn798935.aspx), door de volgende experimentele functies:

* [Synoniemen API](search-synonyms.md) synoniem maps uploaden en zoekopdracht uitbreiden.
* [`moreLikeThis`queryparameter](search-more-like-this.md) om documenten die relevant voor een bepaald document zijn te vinden.

Zorg ervoor dat voor het doel van de preview-API-versie `api-version=2016-09-01-Preview` om te proberen deze experimentele functies. Het volgende voorbeeld ziet u hoe de preview-api-versie is opgegeven bij het maken van een meer achtige deze query.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Preview-functies zijn beschikbaar voor het testen en experimenteren met het doel van de feedback te verzamelen en nog worden gewijzigd. **We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.**

Azure Search-service is beschikbaar in meerdere versies. Raadpleeg [Search Serviceversiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie.
