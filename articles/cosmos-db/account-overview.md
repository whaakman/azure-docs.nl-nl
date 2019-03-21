---
title: Werken met Azure Cosmos DB-accounts
description: Dit artikel wordt beschreven hoe maken en gebruiken van Azure Cosmos DB-accounts
author: dharmas-cosmos
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: e40b763aa2a36e3afd85a82b66e393ac78d3afaa
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57839573"
---
# <a name="work-with-azure-cosmos-account"></a>Werken met een Azure Cosmos-account

Azure Cosmos DB is een volledig beheerde platform-as-a-service (PaaS). Om te beginnen met Azure Cosmos DB, moet u eerst een Azure Cosmos-account maken in uw Azure-abonnement. Uw Azure Cosmos-account bevat een unieke DNS-naam en u kunt een account kunt beheren met behulp van Azure portal, Azure CLI of met behulp van verschillende taalspecifieke SDK's. Zie voor meer informatie, [over het beheren van uw Azure Cosmos-account](how-to-manage-database-account.md).

Het Azure Cosmos-account is de fundamentele eenheid van wereldwijde distributie en hoge beschikbaarheid. U kunt voor het wereldwijd distribueren van uw gegevens en doorvoer meerdere Azure-regio's, toevoegen en verwijderen van Azure-regio's voor uw Azure Cosmos-account op elk gewenst moment. U kunt uw Azure Cosmos-account om één of meerdere regio's voor schrijven configureren. Zie voor meer informatie, [toevoegen en verwijderen van Azure-regio's aan uw Azure Cosmos-account](how-to-manage-database-account.md). U kunt configureren dat de [standaardconsistentie](consistency-levels.md) op Azure Cosmos-account. Azure Cosmos DB biedt uitgebreide Sla's dat doorvoer en latentie in het 99e percentiel, consistentie en beschikbaarheid. Zie voor meer informatie, [Sla's voor Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Als u wilt beheren veilig toegang tot de gegevens in uw Azure Cosmos-account, kunt u de master-sleutels die zijn gekoppeld aan uw account. Naar meer beveiligde toegang tot uw gegevens, kunt u een VNET-service-eindpunt en een IP-firewall kunt configureren voor uw Azure Cosmos-account. 

## <a name="elements-in-an-azure-cosmos-account"></a>Elementen in een Azure Cosmos-account

Azure Cosmos DB-container is de fundamentele schaalbaarheid. U kunt een onbeperkt aantal ingerichte doorvoer (RU/s) en storage vrijwel hebben op de container. Azure Cosmos DB partities transparant van de container met behulp van de logische partitie-sleutel die u opgeeft als u wilt uw ingerichte doorvoer en opslag elastisch schalen. Zie voor meer informatie, [werken met Azure Cosmos-containers en objecten](databases-containers-items.md).

Op dit moment kunt u maximaal 100 Azure Cosmos-accounts in een Azure-abonnement maken. Één Azure Cosmos-account kunt vrijwel onbeperkte hoeveelheid gegevens en ingerichte doorvoer beheren. Voor het beheren van uw gegevens en ingerichte doorvoer, kunt u een of meer Azure Cosmos-databases onder uw account en in die database maken, kunt u een of meer containers maken. De volgende afbeelding ziet u de hiërarchie van elementen in een Azure Cosmos-account:

![Hiërarchie van een Azure Cosmos-account](./media/account-overview/hierarchy.png)

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan naar meer informatie over het beheren van uw Azure Cosmos-account of andere concepten die zijn gekoppeld aan Azure Cosmos DB Zie:

* [Procedures voor uw Azure Cosmos-account beheren](how-to-manage-database-account.md)
* [Wereldwijde distributie](distribute-data-globally.md)
* [Consistentieniveaus](consistency-levels.md)
* [Werken met Azure Cosmos-containers en objecten](databases-containers-items.md)
* [VNET-service-eindpunt voor uw Azure Cosmos-account](vnet-service-endpoint.md)
* [IP-firewall voor uw Azure Cosmos-account](firewall-support.md)
* [Instructies voor het toevoegen en verwijderen van Azure-regio's aan uw Azure Cosmos-account](how-to-manage-database-account.md)
* [Azure Cosmos DB SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
