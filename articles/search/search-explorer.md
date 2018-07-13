---
title: Search explorer voor het uitvoeren van query's indexen in Azure Search | Microsoft Docs
description: Leer hoe u Search explorer gebruiken voor het uitvoeren van query's indexen in Azure Search.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 520d9e7b1899c54d922ff6fb77e0901f9609b029
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004130"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Over het gebruik Search explorer in query-indexen in Azure Search 

Dit artikel leest u hoe u query's een bestaande Azure Search index met **Search explorer** in Azure portal. U kunt Search explorer gebruiken om in te dienen eenvoudige of volledige Lucene-queryreeksen naar elke bestaande index in uw service.

## <a name="open-the-service-dashboard"></a>De servicedashboard openen
1. Klik in de snelbalk aan de linkerkant van [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) op **Alle resources**.
2. Selecteer uw Azure Search-service.

## <a name="select-an-index"></a>Een index selecteren

Selecteer via de tegel **Indexen** de index waarin u wilt zoeken.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Search explorer openen

Klik op de tegel Search explorer te openen de zoekbalk en het deelvenster met resultaten.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Beginnen met zoeken

Wanneer u Search explorer gebruikt, kunt u opgeven [queryparameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) te formuleren van de query.

1. Typ in **Queryreeks** een query en druk vervolgens op **Zoeken**. 

   De queryreeks wordt automatisch geparseerd in de juiste aanvraag-URL om een HTTP-aanvraag te verzenden naar de Azure Search REST-API.   
   
   U kunt elke geldige eenvoudige of volledige Lucene-querysyntaxis gebruiken om de aanvraag te maken. Het teken `*` komt overeen met een lege of niet-opgegeven zoekopdracht die alle documenten in willekeurige volgorde retourneert.

2. In **Resultaten** worden de queryresultaten weergegeven als onbewerkte JSON, identiek aan de nettolading die wordt geretourneerd in een HTTP-antwoordtekst wanneer aanvragen via een programma worden uitgegeven.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Volgende stappen

De volgende resources bieden extra informatie over querysyntaxis en voorbeelden.

 + [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Voorbeelden van Lucene-querysyntaxis](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [OData-filterexpressiesyntaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 