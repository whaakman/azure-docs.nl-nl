---
title: Snelstartgids - een Unity iOS-app maken met Azure ruimtelijke ankers | Microsoft Docs
description: In deze quickstart leert u een iOS-app maken met Unity en met behulp van Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 37856c0833ecde1478d4bd588b8e3122e8eac0ca
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135237"
---
# <a name="quickstart-create-a-unity-ios-app-with-azure-spatial-anchors"></a>Quickstart: Een Unity iOS-app maken met Azure ruimtelijke ankers

Deze quickstart bevat informatie over het maken van een Unity iOS-app via [Azure ruimtelijke ankers](../overview.md). Azure Spatial Anchors is een platformoverstijgende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Als u klaar bent, hebt u een ARKit iOS-app met Unity gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * Build-instellingen voor Unity voorbereiden
> * Downloaden en importeren van de Unity ARKit-invoegtoepassing
> * Spatial Anchors-account-id en -accountsleutel configureren
> * Xcode-project exporteren
> * Implementeren en uitvoeren op een iOS-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Een macOS-machine met daarop <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3+</a>, <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10</a> en <a href="https://cocoapods.org" target="_blank">CocoaPods</a> geïnstalleerd.
- GIT geïnstalleerd via HomeBrew. Voer de volgende opdracht in een enkele regel van de Terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Voer `brew install git`.
- Een door een ontwikkelaar geactiveerd en <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">met ARKit compatibel</a> iOS-apparaat.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Downloaden en openen van het voorbeeldproject Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [iOS Unity Build Settings](../../../includes/spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

Ga in het deelvenster **Project** naar `Assets/AzureSpatialAnchorsPlugin/Examples` en open het scènebestand `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sla de scène op door **File** -> **Save** te selecteren.

## <a name="export-the-xcode-project"></a>Xcode-project exporteren

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

[!INCLUDE [Configure Xcode](../../../includes/spatial-anchors-unity-ios-xcode.md)]

Volg de instructies in de app om een anker te plaatsen en terug te halen.

> [!NOTE]
> Als u bij het uitvoeren van de app de camera niet als achtergrond ziet (u ziet bijvoorbeeld lege, blauwe of andere structuren) dan moet u waarschijnlijk assets opnieuw in Unity importeren. De app stoppen. Kies in het bovenste menu in Unity **Assets -> Alles opnieuw importeren**. Voer de vervolgens opnieuw app uit.

In Xcode stopt u de app door op **Stop** te drukken.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
