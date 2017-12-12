---
title: Inrichten doorvoer voor Azure Cosmos DB | Microsoft Docs
description: Informatie over het instellen van de ingerichte doorvoer voor uw Azure Cosmos DB containsers, verzamelingen, grafieken en tabellen.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: 69501b07369a8d5da15cf9bc7d75f07999bf0999
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Doorvoer instellen voor Azure DB die Cosmos-containers

U kunt de doorvoer instellen voor uw Azure DB die Cosmos-containers in de Azure-portal of met behulp van de client-SDK's. 

De volgende tabel bevat de doorvoer die beschikbaar zijn voor containers:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Container één partitie</strong></p></td>
            <td valign="top"><p><strong>Gepartitioneerde Container</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaal doorvoer</p></td>
            <td valign="top"><p>400 aanvraageenheden per seconde</p></td>
            <td valign="top"><p>2500 aanvraageenheden per seconde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale doorvoer</p></td>
            <td valign="top"><p>10.000 aanvraageenheden per seconde</p></td>
            <td valign="top"><p>Onbeperkt</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>De doorvoer instellen via de Azure-portal

1. In een nieuw venster openen de [Azure-portal](https://portal.azure.com).
2. Klik op de linkerbalk op **Azure Cosmos DB**, of klik op **meer Services** aan de onderkant en schuif vervolgens naar **Databases**, en klik vervolgens op **Azure Cosmos DB**.
3. Selecteer uw account Cosmos DB.
4. Klik in het nieuwe venster **Data Explorer (Preview)** in het navigatiemenu.
5. Vouw uw database en de container in het nieuwe venster en klik vervolgens op **schaal & instellingen**.
6. Typ in het nieuwe venster de nieuwe waarde van de doorvoer in de **doorvoer** vak en klik vervolgens op **opslaan**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>De doorvoer instellen met behulp van de SQL-API voor .NET

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Veelgestelde vragen over doorvoer

**Kan ik mijn doorvoer ingesteld op minder dan 400 RU/s**

400 RU/s is de minimale doorvoer die beschikbaar zijn op verzamelingen met één partitie Cosmos-DB (2500 RU/s is de minimale voor gepartitioneerde verzamelingen). Aanvraag eenheden worden ingesteld met intervallen van 100 RU/s, maar doorvoer kan niet worden ingesteld op 100 RU/s of een willekeurige waarde kleiner is dan 400 RU/s. Als u naar een voordelige methode zoekt te ontwikkelen en testen van de Cosmos-DB, kunt u de gratis [Azure Cosmos DB Emulator](local-emulator.md), die u lokaal kunt implementeren zonder kosten. 

**Hoe stel ik througput met de MongoDB-API**

Er is geen extensie MongoDB-API om in te stellen doorvoer. De aanbeveling is het gebruik van de SQL-API, zoals wordt weergegeven in [de doorvoer instellen met behulp van de SQL-API voor .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het inrichten en gaat wereld-schaal met Cosmos DB, [partitionering en schalen met Cosmos DB](partition-data.md).
