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
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: c0c8f1aee75c4ee5cc35758b71ef573637fd3edd
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Ontwikkelen met Azure Cosmos DB tabel API en Azure Table storage

Azure Cosmos DB tabel API en Azure Table storage delen de dezelfde tabel-gegevensmodel en de dezelfde maken, verwijderen, bijwerken en querybewerkingen via hun SDK's weergeven. 

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Ontwikkelen met de Azure DB tabel API-Cosmos

Op dit moment de [Azure Cosmos DB tabel API](table-introduction.md) heeft vier beschikbaar voor het ontwikkelen van SDK's: 
- [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget) .NET SDK. Deze bibliotheek heeft de dezelfde klassen en handtekeningen als de openbare [Windows Azure-opslag-SDK](https://www.nuget.org/packages/WindowsAzure.Storage), maar heeft ook de mogelijkheid om te verbinden met Azure DB die Cosmos-accounts met behulp van de tabel-API. 
- [Python SDK](table-sdk-python.md). De nieuwe Azure Cosmos DB Python SDK is de enige SDK die ondersteuning biedt voor Azure Table storage in Python. Deze SDK maakt verbinding met Azure Table storage en Azure Cosmos DB tabel-API.
- [Java SDK](table-sdk-java.md). Deze SDK van Azure Storage biedt de mogelijkheid verbinding maken met Azure DB die Cosmos-accounts met behulp van de tabel-API.
- [Node.js-SDK](table-sdk-nodejs.md). Deze SDK van Azure Storage biedt de mogelijkheid verbinding maken met Azure DB die Cosmos-accounts met behulp van de tabel-API.

Als u meer informatie over het werken met de tabel-API is beschikbaar in de [Veelgestelde vragen over: met de tabel-API ontwikkelen](faq.md#develop-with-the-table-api) artikel.

## <a name="developing-with-the-azure-table-storage"></a>Ontwikkelen met de Azure-tabelopslag

[Azure Table storage](table-storage-overview.md) heeft veel beschikbaar SDK's en zelfstudies beschikbaar is, die allemaal zijn nu beschikbaar in de [Azure Table storage](table-storage-overview.md) sectie. Deze artikelen worden bijgewerkt als interoperabiliteit tussen de Azure Table-opslag-SDK's en Azure Cosmos DB tabel-API's beschikbaar is.  





