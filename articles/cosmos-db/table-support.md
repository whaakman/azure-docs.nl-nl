---
title: Ondersteuning van Azure Table Storage in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe Azure Cosmos DB tabel API en Azure Storage-tabellen samenwerken.
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: 378f00f2-cfd9-4f6b-a9b1-d1e4c70799fd
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 1513fc53501f1cfec93134841fbef9a8552dd43c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Ontwikkelen met Azure Cosmos DB tabel API en Azure Table storage

Azure Cosmos DB tabel API en Azure Table storage delen de dezelfde tabel-gegevensmodel en de dezelfde maken, verwijderen, bijwerken en querybewerkingen via hun SDK's weergeven. 

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Ontwikkelen met de Azure DB tabel API-Cosmos

Op dit moment de [Azure Cosmos DB tabel API](table-introduction.md) één .NET SDK beschikbaar is, heeft de [Windows Azure-opslag Premium Table (preview)](https://aka.ms/premiumtablenuget). Deze bibliotheek heeft de dezelfde klassen en handtekeningen als de openbare [Windows Azure-opslag-SDK](https://www.nuget.org/packages/WindowsAzure.Storage), maar heeft ook de mogelijkheid om te verbinden met Azure DB die Cosmos-accounts met behulp van de tabel-API (preview). Zie voor een Quick Start en een zelfstudie met de Azure-API voor tabel Cosmos DB, de volgende artikelen:
- Snelstartgids: [Azure Cosmos DB: een .NET-toepassing met behulp van de tabel-API maken](create-table-dotnet.md)
- Zelfstudie: [Azure Cosmos DB: ontwikkelen met de tabel API in .NET](tutorial-develop-table-dotnet.md)

Als u meer informatie over het werken met de tabel-API is beschikbaar in de [Veelgestelde vragen over: met de tabel-API ontwikkelen](faq.md#develop-with-the-table-api-preview) artikel.

## <a name="developing-with-the-azure-table-storage"></a>Ontwikkelen met de Azure-tabelopslag

[Azure Table storage](table-storage-overview.md) heeft veel beschikbaar SDK's en zelfstudies beschikbaar is, die allemaal zijn nu beschikbaar in de [Azure Table storage](table-storage-overview.md) sectie. Deze artikelen worden bijgewerkt als interoperabiliteit tussen de Azure Table-opslag-SDK's en Azure Cosmos DB tabel-API's beschikbaar is.  





