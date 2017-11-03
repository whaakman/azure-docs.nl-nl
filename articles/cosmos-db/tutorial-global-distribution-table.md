---
title: Zelfstudie voor Azure DB Cosmos globale distributie voor tabel-API | Microsoft Docs
description: Informatie over het instellen van Azure DB die Cosmos globale distributie op basis van de tabel-API.
services: cosmos-db
keywords: globale distributie, tabel
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: e9cb4b5b886cec46c0483287460c720855867f38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Het instellen van Azure DB die Cosmos globale distributie op basis van de tabel-API

In dit artikel, laten we zien hoe de Azure portal gebruiken om setup Azure Cosmos DB globale distributie en maak verbinding met de tabel-API (preview).

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Configureren globale distributie met behulp van de [tabel-API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Verbinding maken met een voorkeursregio met behulp van de tabel-API

Om te profiteren van [globale distributie](distribute-data-globally.md), clienttoepassingen de voorkeur geordende lijst met regio's moeten worden gebruikt voor het document bewerkingen uitvoeren kunnen opgeven. Dit kan worden gedaan door het instellen van de `TablePreferredLocations` configuratiewaarde in de app-configuratie voor de preview van Azure-opslag-SDK. Op basis van de Azure DB die Cosmos-accountconfiguratie, huidige regionale beschikbaarheid en de voorkeur lijst is opgegeven, wordt het meest optimale eindpunt door de Azure-opslag-SDK schrijven uitvoeren en leesbewerkingen gekozen.

De `TablePreferredLocations` moet een door komma's gescheiden lijst met aanbevolen (multihoming) locaties bevatten voor leesbewerkingen. Elk clientexemplaar kunt u een subset van deze gebieden in de gewenste volgorde voor lage latentie leesbewerkingen opgeven. De regio's moeten de namen van hun [weergavenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx), bijvoorbeeld `West US`.

Alle leesbewerkingen wordt verzonden naar de eerste beschikbare regio in de `TablePreferredLocations` lijst. Als de aanvraag is mislukt, zal de client mislukken omlaag in de lijst voor de volgende regio, enzovoort.

De SDK wordt alleen poging tot lezen van de gebieden die zijn opgegeven `TablePreferredLocations`. Ja, bijvoorbeeld of de Account van de Database is beschikbaar in drie gebieden, maar de client geeft alleen twee van de niet-schrijven regio's voor `TablePreferredLocations`, en vervolgens geen leesbewerkingen buiten de regio schrijven, zelfs wanneer een failover kunnen worden geleverd.

Alle schrijfbewerkingen naar de huidige schrijven regio worden automatisch verzonden door de SDK.

Als de `TablePreferredLocations` eigenschap niet is ingesteld, wordt alle aanvragen van het huidige schrijven gebied kunnen worden geleverd.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

Dat is, die in deze zelfstudie is voltooid. U kunt informatie over het beheren van de consistentie van uw account globaal gerepliceerde door te lezen [consistentieniveaus in Azure Cosmos DB](consistency-levels.md). En Zie voor meer informatie over hoe globale databasereplicatie in Azure Cosmos DB werkt, [gegevens globaal met Azure Cosmos DB distribueren](distribute-data-globally.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Globale distributie op basis van de Azure-portal configureren
> * Globale distributie op basis van de DocumentDB APIs configureren

U kunt nu doorgaan met de volgende zelfstudie voor meer informatie over het ontwikkelen van lokaal via de lokale Azure DB die Cosmos-emulator.

> [!div class="nextstepaction"]
> [Lokaal ontwikkelen met de emulator](local-emulator.md)