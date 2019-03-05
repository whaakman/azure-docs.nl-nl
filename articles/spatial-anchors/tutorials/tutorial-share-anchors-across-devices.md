---
title: Zelfstudie - sessies en apparaten met Azure Spatial Anchors delen | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Spatial Anchors-id's deelt tussen apparaten in Unity met een back-end-service.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: df7f8aa5b49e3fe17be3b17a6e0f5d8861b26253
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753466"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors"></a>Zelfstudie: Sessies en apparaten met Azure Spatial Anchors delen

In deze zelfstudie leert u hoe u [Azure Spatial Anchors](../overview.md) gebruikt om:

1. Bladwijzers in één sessie te maken en deze vervolgens te zoeken in een andere sessie op hetzelfde of een ander apparaat. Bijvoorbeeld op een andere dag.
2. Bladwijzers te maken die kunnen worden gevonden door meerdere apparaten op dezelfde plaats op hetzelfde moment.

![Persistentie](./media/persistence.gif)

Spatial Anchors is een platformoverschrijdende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Wanneer u klaar bent, hebt u een app die kan worden geïmplementeerd op twee of meer apparaten. Azure Spatial Anchors dat door één exemplaar is gemaakt, kan worden gedeeld met andere.

U leert het volgende:

> [!div class="checklist"]
> * Implementeer een ASP.NET Core-web-app in Azure die kan worden gebruikt voor het delen van bladwijzers en deze opslaat in het geheugen voor een bepaalde tijd.
> * Configureer de scène AzureSpatialAnchorsLocalSharedDemo binnen het Unity-voorbeeld van onze quickstarts om te profiteren van de web-app Sharing Anchors.
> * Implementeer en voer uit op een of meer apparaten.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Houd er rekening mee dat, hoewel u in deze zelfstudie gebruik zult maken van Unity en een ASP.NET Core-web-app, dit alleen een voorbeeld is om te laten zien hoe het delen van Azure Spatial Anchor-id's naar andere apparaten werkt. U kunt andere talen en technologieën gebruiken om hetzelfde doel te bereiken. De ASP.NET Core-web-app die wordt gebruikt in deze zelfstudie heeft bovendien een afhankelijkheid van .NET Core 2.2 SDK. Het werkt goed op reguliere Azure-web-apps (Windows), maar werkt momenteel niet in Azure-web-apps voor Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Uw Sharing Anchor-service implementeren

Open Visual Studio en open het project in de map `Sharing\SharingServiceSample`.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een ASP.NET Core-web-app in Azure geïmplementeerd en vervolgens geconfigureerd en een Unity-app geïmplementeerd. U hebt Spatial Anchors met de app gemaakt en gedeeld met andere apparaten met behulp van uw ASP.NET Core-web-app. Voor meer informatie over het verbeteren van uw ASP.NET Core-web-app, zodat deze Cosmos DB gebruikt voor het opslaan van de gedeelde Spatial Anchors, gaat u naar de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Zelfstudie: Cosmos DB gebruiken om bladwijzers op te slaan](./tutorial-use-cosmos-db-to-store-anchors.md)
