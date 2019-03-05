---
title: 'Zelfstudie: delen met sessies en apparaten met Azure Spatial Anchors en een Azure Cosmos DB-back-end | Microsoft Docs'
description: In deze zelfstudie leert u hoe u Azure Spatial Anchors-id's deelt tussen apparaten in Unity met een back-end-service en Azure Cosmos DB.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: f0cd42fc37727099ed95a1c6fc2d427b7862412e
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753437"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors-and-an-azure-cosmos-db-back-end"></a>Zelfstudie: delen met sessies en apparaten met Azure Spatial Anchors en een Azure Cosmos DB-back-end

In deze zelfstudie leert u hoe u [Azure Spatial Anchors](../overview.md) gebruikt om:

1. Ankers in één sessie te maken en deze vervolgens te zoeken in een andere sessie op hetzelfde of een ander apparaat. Bijvoorbeeld op een andere dag.
2. Maak ankers die door meerdere apparaten op dezelfde plaats op hetzelfde moment kunnen worden gevonden.

![Persistentie](./media/persistence.gif)

[Azure Spatial Anchors](../overview.md) is een platformoverschrijdende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Wanneer u klaar bent, hebt u een app die kan worden geïmplementeerd op twee of meer apparaten. Azure Spatial Anchors die door één exemplaar zijn gemaakt, delen de id's met de andere exemplaren met behulp van Cosmos DB.

U leert het volgende:

> [!div class="checklist"]
> * Implementeer een ASP.NET Core Web-App in Azure die kan worden gebruikt voor het delen van ankers, waarna ze worden opgeslagen in Cosmos DB.
> * Configureer de scène AzureSpatialAnchorsLocalSharedDemo binnen het Unity-voorbeeld van onze Quickstarts om de web-app Sharing Anchors te kunnen gebruiken.
> * Implementeer en voer uit op een of meer apparaten.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Hoewel u in deze zelfstudie gebruik maakt van Unity en Azure Cosmos DB, is dit alleen om een voorbeeld te laten zien voor het delen van Azure Spatial Anchor-id's op andere apparaten. U kunt andere talen en back-endtechnologieën gebruiken om hetzelfde doel te bereiken. De ASP.NET Core Web-App die wordt gebruikt in deze zelfstudie heeft bovendien een afhankelijkheid van .NET Core-SDK 2.2. Het werkt goed op reguliere Azure Web Apps (Windows), maar werkt momenteel niet in Azure Web Apps voor Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Noteer de `Connection String`. U hebt deze later nodig.

## <a name="deploy-your-sharing-anchors-service"></a>Sharing Anchors Service implementeren

Open Visual Studio en open het project in de map `Sharing\SharingServiceSample`.

### <a name="configure-the-service-so-that-it-uses-your-cosmos-db"></a>De service configureren voor gebruik met Cosmos DB

In **Solution Explorer** opent u `SharingService\Startup.cs`.

Zoek de regel `#define INMEMORY_DEMO` boven aan het bestand en maak er commentaar van. Sla het bestand op.

In **Solution Explorer** opent u `SharingService\appsettings.json`.

Zoek de eigenschap `StorageConnectionString` en stel de waarde in op de `Connection String` die u hebt genoteerd in de [stap voor het maken van een databaseaccount](#create-a-database-account). Sla het bestand op.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure Cosmos DB gebruikt voor het delen van anker-id's op apparaten. Ga voor meer informatie over de Azure Spatial Anchors-bibliotheek door naar de handleiding over het maken en vinden van ankers.

> [!div class="nextstepaction"]
> [Create and locate anchors using Azure Spatial Anchors](../create-locate-anchors-overview.md) (Ankers maken en vinden met Azure Spatial Anchors)
