---
title: Toegang tot wijziging feed in Azure Cosmos DB Azure Cosmos DB
description: Dit artikel beschrijft de verschillende opties beschikbaar om te lezen en toegang in Azure Cosmos DB Azure Cosmos DB-wijzigingenfeed.
author: rafats
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rafats
ms.openlocfilehash: 596bb31ea2734b389ac8b2d6741907da04730648
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629160"
---
# <a name="reading-azure-cosmos-db-change-feed"></a>Lezen Azure Cosmos DB-wijzigingenfeed

U kunt werken met de Azure Cosmos DB-wijzigingenfeed met behulp van een van de volgende opties:

* Met behulp van Azure Functions
* Met behulp van de change feed processor-bibliotheek
* Met behulp van de Azure Cosmos DB SQL API-SDK

## <a name="using-azure-functions"></a>Met behulp van Azure Functions

Azure Functions is de eenvoudigste en aanbevolen optie. Wanneer u een Azure Cosmos DB-trigger in een Azure Functions-toepassing maakt, kunt u de container om verbinding te maken en de Azure-functie wordt geactiveerd wanneer er sprake is van een wijziging in de container. Triggers kunnen worden gemaakt met behulp van de Azure Functions-portal, de Azure Cosmos DB-portal of programmatisch met de SDK's. Visual Studio en Visual Studio Code bieden ondersteuning voor het schrijven van Azure Functions en u kunt zelfs de CLI van Azure Functions gebruiken voor platformoverschrijdende ontwikkeling. U kunt schrijven en fouten opsporen in de code op uw bureaublad en klikt u vervolgens de functie met één klik implementeren. Zie [Serverless database computing met behulp van Azure Functions](serverless-computing-database.md) en [met behulp van de change feed met Azure Functions](change-feed-functions.md)) artikelen voor meer informatie.

## <a name="using-the-change-feed-processor-library"></a>Met behulp van de change feed processor-bibliotheek

De change feed processor-bibliotheek wordt ingeschakeld, complexiteit en toch hebt u een volledige controle over de wijzigingenfeed. De bibliotheek volgt het patroon waarnemer, waarbij de verwerking van de functie wordt aangeroepen door de bibliotheek. Als u een hoge doorvoer wijzigingenfeeds hebt, kunt u meerdere clients om te lezen van de wijzigingenfeed instantiëren. Omdat u change feed processor-bibliotheek, wordt deze automatisch de belasting tussen de verschillende clients delen zonder dat u hoeft voor het implementeren van deze logica. Alle complexiteit wordt verwerkt door de bibliotheek. Als u wilt dat uw eigen load balancer, dan kunt u implementeren `IParitionLoadBalancingStrategy` voor de partitie van een aangepaste strategie voor het verwerken van change feed. Zie voor meer informatie, [met behulp van de change feed processor-bibliotheek](change-feed-processor.md).

## <a name="using-the-azure-cosmos-db-sql-api-sdk"></a>Met behulp van de Azure Cosmos DB SQL API-SDK

Met de SDK krijgt u een laag niveau besturingselement van de feed wijzigen. U kunt het controlepunt beheren, toegang tot een bepaalde logische partitie sleutel, enzovoort. Als u meerdere lezers hebt, kunt u `ChangeFeedOptions` lezen verdelen naar verschillende threads of andere clients. 

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan naar meer informatie over de wijzigingenfeed in de volgende artikelen:

* [Overzicht van de wijzigingenfeed](change-feed.md)
* [Met behulp van de change feed met Azure Functions](change-feed-functions.md)
* [Met behulp van de change feed processor-bibliotheek](change-feed-processor.md)