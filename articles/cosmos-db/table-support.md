---
title: Ondersteuning voor Azure-tabelopslag in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe de tabel-API van Azure Cosmos DB en Azure-opslagtabellen samenwerken.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 114286b45df5f47e81bd2b990c8b50c8b7b7a482
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43185363"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Ontwikkelen met de tabel-API van Azure Cosmos DB en Azure-tabelopslag

De tabel-API van Azure Cosmos DB en Azure-tabelopslag hebben hetzelfde gegevensmodel voor tabellen en bieden via de bijbehorende SDK’s dezelfde bewerkingen voor maken, verwijderen, bijwerken en uitvoeren van query’s. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Ontwikkelen met de tabel-API van Azure Cosmos DB

Op dit moment beschikt de [tabel-API van Azure Cosmos DB](table-introduction.md) over vier SDK’s voor ontwikkelen: 
- [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget) .NET SDK. Deze bibliotheek heeft dezelfde klassen en methodes als de openbare [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), maar biedt ook de mogelijkheid verbinding te maken met Azure Cosmos DB-accounts met behulp van de tabel-API. Houd er rekening mee dat de `Microsoft.Azure.CosmosDB.Table`-bibliotheek momenteel alleen beschikbaar is voor .NET Standard, deze is nog niet beschikbaar voor .NET Core.
- [Python SDK](table-sdk-python.md). De nieuwe Python SDK voor Azure Cosmos DB is de enige SDK die ondersteuning biedt voor Azure-tabelopslag in Python. Deze SDK is verbonden met zowel Azure-tabelopslag als met de tabel-API van Azure Cosmos DB.
- [Java SDK](table-sdk-java.md). Met deze Azure Storage-SDK kan verbinding worden gemaakt met Azure Cosmos DB-accounts met behulp van de tabel-API.
- [Node.js SDK](table-sdk-nodejs.md). Met deze Azure Storage-SDK kan verbinding worden gemaakt met Azure Cosmos DB-accounts met behulp van de tabel-API.

Aanvullende informatie over het werken met de tabel-API is beschikbaar in het artikel [Veelgestelde vragen: Ontwikkelen met de tabel-API](faq.md#table).

## <a name="developing-with-azure-table-storage"></a>Ontwikkelen met Azure-tabelopslag

Azure-tabelopslag beschikt over deze SDK’s voor ontwikkelen:

- [WindowsAzure.Storage .NET SDK](https://www.nuget.org/packages/WindowsAzure.Storage/). Met deze bibliotheek kunt u werken met de opslagtabelservice.
- [Python SDK](table-sdk-python.md). De Python SDK voor Azure Cosmos DB-tabellen biedt ook ondersteuning voor de opslagtabelservice.
- [Azure Storage SDK voor Java](https://github.com/azure/azure-storage-java). Deze Azure Storage-SDK biedt een clientbibliotheek in Java voor het gebruik van Azure-tabelopslag.
- [Node.js SDK](table-sdk-nodejs.md). Deze SDK biedt een Node.js-pakket en een JavaScript-clientbibliotheek die compatibel is met de browser, voor het gebruik van de opslagtabelservice.
- [AzureRmStorageTable PowerShell-module](https://www.powershellgallery.com/packages/AzureRmStorageTable/1.0.0.7). Deze PowerShell-module bevat cmdlets voor het werken met opslagtabellen.
- [Azure Storage-clientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp/). Met deze bibliotheek kunt u toepassingen bouwen in Azure Storage.
- [Clientbibliotheek voor Azure-opslagtabellen voor Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Dit project biedt een Ruby-pakket waarmee u eenvoudig toegang kunt krijgen tot Azure-opslagtabelservices.
- [PHP-clientbibliotheek voor Azure-opslagtabellen](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Dit project biedt een PHP-clientbibliotheek waarmee u eenvoudig toegang kunt krijgen tot Azure-opslagtabelservices.


   





