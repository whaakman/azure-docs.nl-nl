---
title: Een query uitvoeren op uw Azure Search-index met Azure Portal | Microsoft Docs
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
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a23372112e17703a3399e1bdc9eaf73b85a1f80d


---
# <a name="query-your-azure-search-index-using-the-azure-portal"></a>Een query uitvoeren op uw Azure Search-index met Azure Portal
> [!div class="op_single_selector"]
> * [Overzicht](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

In dit artikel wordt beschreven hoe u een query op de Azure Search-index kunt uitvoeren in Azure Portal.

Voordat u deze procedure begint, moet u al [een Azure Search-index hebben gemaakt](search-what-is-an-index.md) en moet deze index [gevuld zijn met gegevens](search-what-is-data-import.md).

## <a name="i-go-to-your-azure-search-blade"></a>I. Naar uw Azure Search-blade gaan
1. Klik op Alle resources in het menu aan de linkerkant van [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Selecteer uw Azure Search-service

## <a name="ii-select-the-index-you-would-like-to-search"></a>II. De index selecteren waarin u wilt zoeken
1. Selecteer de index waarin u wilt zoeken vanaf de tegel Indexen.

![](./media/search-explorer/pick-index.png)

## <a name="iii-click-on-the-search-explorer-tile"></a>III. Op de tegel Search Explorer klikken
![](./media/search-explorer/search-explorer-tile.png)

## <a name="iii-start-searching"></a>III. Beginnen met zoeken
1. Als u wilt zoeken in uw Azure Search-index, begint u te typen in het veld *Querytekenreeks* en kiest u vervolgens **Zoeken**.
   
   * Wanneer u de Search Explorer gebruikt, kunt u een van de [queryparameters](https://msdn.microsoft.com/library/dn798927.aspx) opgeven
2. De queryresultaten worden in de sectie *Resultaten* weergegeven in de onbewerkte JSON zoals u die zou ontvangen in een HTTP-antwoordtekst als u een zoekaanvraag zou doen in de Azure Search REST-API.
3. De queryreeks worden automatisch verdeeld in de juiste aanvraag-URL om een HTPP-aanvraag te verzenden naar de Azure Search REST-API.

![](./media/search-explorer/search-bar.png)




<!--HONumber=Nov16_HO2-->


