---
title: Naslag voor OData search.score - Azure Search
description: OData search.score-functie in Azure Search-query's.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: dc444216c4677b9970b867e92aa5ae259a197220
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079689"
---
# <a name="odata-searchscore-function-in-azure-search"></a>OData `search.score` functie in Azure Search

Wanneer u een query verzenden naar Azure Search zonder de [ **$orderby** parameter](search-query-odata-orderby.md), de resultaten die keert u terug in aflopende volgorde worden gesorteerd op relevantiescore. Zelfs wanneer u gebruik **$orderby**, de relevantiescore wordt gebruikt om te doorbreken ties standaard. Maar is soms het handig om te gebruiken van de relevantiescore als een eerste sorteercriteria en andere criteria als de beslissen. De `search.score` functie kunt u om dit te doen.

## <a name="syntax"></a>Syntaxis

De syntaxis voor `search.score` in **$orderby** is `search.score()`. De functie `search.score` neemt geen parameters. Het kan worden gebruikt met de `asc` of `desc` sorteervolgorde aanduiding, net als elke andere-component in de **$orderby** parameter. Deze kan overal in de lijst met sorteercriteria weergegeven.

## <a name="example"></a>Voorbeeld

Hotels in aflopende volgorde sorteren `search.score` en `rating`, en klik vervolgens in oplopende volgorde op afstand van de opgegeven coördinaten zodat tussen twee hotels met identieke beoordeling, het beste een eerste wordt weergegeven:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Volgende stappen  

- [Overzicht van taal van OData-expressie voor Azure Search](query-odata-filter-orderby-syntax.md)
- [Naslaginformatie over de syntaxis van de OData-expressie voor Azure Search](search-query-odata-syntax-reference.md)
- [Documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
