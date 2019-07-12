---
title: Meerdere masters voordelen van Azure Cosmos DB
description: Begrijp de voordelen van meerdere masters in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: mjbrown
ms.openlocfilehash: c78e5e4f8d396d777738bddfd6baf086c0b2ecf4
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789291"
---
# <a name="understand-multi-master-benefits-in-azure-cosmos-db"></a>Inzicht in meerdere masters voordelen in Azure Cosmos DB

Cosmos DB-Accountoperators moeten de juiste globale distributie configureren om te controleren of de latentie, beschikbaarheid en vereisten voor hun toepassingen RTO kiezen. Azure Cosmos-accounts geconfigureerd met meerdere locaties voor schrijven bieden aanzienlijke voordelen via accounts met één schrijven met inbegrip van locatie, schrijven 99,999% beschikbaarheids-SLA, < 10 ms latentie SLA in het 99e percentiel en RTO schrijven = 0 in een regionaal noodgeval.

## <a name="comparison-of-features"></a>Vergelijking van functies

|Vereisten voor toepassingsimplementatie|Meerdere locaties voor schrijven|Locatie van enkele schrijven|Opmerking|
|---|---|---|---|
|Schrijven van SLA van de latentie van < 10ms bij P99|**Ja**|Nee|Accounts met één locatie schrijven worden extra regio-overschrijdende netwerklatentie voor elke schrijfbewerking.|
|Lees de SLA latentie van < 10ms bij P99|**Ja**|Ja| |
|SLA van 99,999% schrijven|**Ja**|Nee|Accounts met één locatie schrijven garanderen SLA van 99,99%|
|RTO = 0|**Ja**|Nee|Geen uitvaltijd voor schrijfbewerkingen in regionale noodsituaties. Accounts met één schrijfbewerking locatie hebben RTO van 15 minuten.|

## <a name="next-steps"></a>Volgende stappen

Als u nog steeds EnableMultipleWriteLocations uitschakelen in uw Azure Cosmos-account wilt, kunt u [open een ondersteuningsticket](https://azure.microsoft.com/support/create-ticket/).
