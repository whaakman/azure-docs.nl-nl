---
title: Zelfstudie - Share Azure ruimtelijke ankers sessies en apparaten | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure ruimtelijke anker-id's tussen Android/iOS-apparaten in Unity met een back-end-service delen.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: ff9868dd7347812eb6ef566288ec364bc89b6955
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629305"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices"></a>Zelfstudie: Share Azure ruimtelijke ankers sessies en apparaten

In deze zelfstudie leert u hoe u [Azure ruimtelijke ankers](../overview.md) ankers maken tijdens één sessie en ga vervolgens naar deze op hetzelfde apparaat of een ander account. Deze dezelfde ankers kunnen ook worden gevonden door meerdere apparaten op dezelfde plaats en op hetzelfde moment.

![Persistentie](./media/persistence.gif)

Spatial Anchors is een platformoverschrijdende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Wanneer u klaar bent, hebt u een app die kan worden geïmplementeerd op twee of meer apparaten. Azure Spatial Anchors dat door één exemplaar is gemaakt, kan worden gedeeld met andere.

U leert het volgende:

> [!div class="checklist"]
> * Implementeer een ASP.NET Core-web-app in Azure die kan worden gebruikt voor het delen van bladwijzers en deze opslaat in het geheugen voor een bepaalde tijd.
> * Configureer de scène AzureSpatialAnchorsLocalSharedDemo binnen het Unity-voorbeeld van onze quickstarts om te profiteren van de web-app Sharing Anchors.
> * Implementeer en voer uit op een of meer apparaten.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Houd er rekening mee dat, hoewel u in deze zelfstudie gebruik zult maken van Unity en een ASP.NET Core-web-app, dit alleen een voorbeeld is om te laten zien hoe het delen van Azure Spatial Anchor-id's naar andere apparaten werkt. U kunt andere talen en technologieën voor back-end gebruiken om de hetzelfde doel te bereiken. De ASP.NET Core-web-app die wordt gebruikt in deze zelfstudie heeft bovendien een afhankelijkheid van .NET Core 2.2 SDK. Het werkt goed op reguliere Azure-web-apps (Windows), maar werkt momenteel niet in Azure-web-apps voor Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Open het voorbeeldproject in Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Uw Sharing Anchor-service implementeren

Open Visual Studio en open het project in de map `Sharing\SharingServiceSample`.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een ASP.NET Core-web-app in Azure geïmplementeerd en vervolgens geconfigureerd en een Unity-app geïmplementeerd. U hebt Spatial Anchors met de app gemaakt en gedeeld met andere apparaten met behulp van uw ASP.NET Core-web-app.

Voor meer informatie over het verbeteren van uw Web-App van ASP.NET Core, zodat het Azure Cosmos DB gebruikt voor het opslaan van uw gedeelde ruimtelijke anker-id's, gaat u naar de volgende zelfstudie. Azure Cosmos DB, persistentie krijgt tot uw Web-App van ASP.NET Core. Als dit dus uw app Maak vandaag nog een anker en dagen later terugkeren om opnieuw te zoeken met behulp van het anker-id die zijn opgeslagen in uw web-app te kunnen worden.

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Cosmos DB naar Store ankers gebruiken](./tutorial-use-cosmos-db-to-store-anchors.md)
