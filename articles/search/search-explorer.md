---
title: Een query uitvoeren voor een index (Portal - Azure Search) | Microsoft Docs
description: Een zoekopdracht in de Search Explorer van Azure Portal uitvoeren.
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 07/10/2017
ms.author: ashmaka
ms.openlocfilehash: dd68d8ed073bf7b8666ddef35a2f1f84df690b4b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a>Een query uitvoeren voor een Azure Search-index met behulp van Search Explorer van Azure Portal
> [!div class="op_single_selector"]
> * [Overzicht](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

In dit artikel ziet u hoe u een query kunt uitvoeren voor een Azure Search-index met behulp van **Search Explorer** van Azure Portal. U kunt Search Explorer gebruiken om eenvoudige of volledige Lucene-queryreeksen te verzenden naar elke bestaande index in uw service.

## <a name="open-the-service-dashboard"></a>De servicedashboard openen
1. Klik in de snelbalk aan de linkerkant van [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) op **Alle resources**.
2. Selecteer uw Azure Search-service.

## <a name="select-an-index"></a>Een index selecteren

Selecteer via de tegel **Indexen** de index waarin u wilt zoeken.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Search Explorer openen

Klik op de tegel Search Explorer om de zoekbalk en het resultatenvenster te openen.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Beginnen met zoeken

Wanneer u Search Explorer gebruikt, kunt u [queryparameters](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) opgeven om de query te formuleren.

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