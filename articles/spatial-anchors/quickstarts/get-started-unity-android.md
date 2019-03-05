---
title: 'Quickstart: Android Unity-app maken met Azure Spatial Anchors | Microsoft Docs'
description: In deze quickstart leert u een Android-app maken met Unity en met behulp van Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: e92c812ffc8b72fe79248c602e48ff01ef9fefcb
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961004"
---
# <a name="quickstart-create-an-android-unity-app-with-azure-spatial-anchors"></a>Quickstart: Een Android Unity-app maken met Azure Spatial Anchors

In deze quickstart wordt besproken hoe u een Android Unity-app maakt met behulp van [Azure Spatial Anchors](../overview.md). Spatial Anchors is een platformoverschrijdende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Als u klaar bent, hebt u een ARCore Android-app met Unity gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * Build-instellingen voor Unity voorbereiden
> * De ARCore SDK voor Unity downloaden en importeren
> * De Spatial Anchors-account-id en -accountsleutel configureren
> * Het Android Studio-project exporteren
> * Implementeren en uitvoeren op een Android-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Een Windows- of macOS-machine met <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a> en <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a> geïnstalleerd.
- Een <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">ontwikkelaar-geactiveerd</a> en <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">voor ARCore geschikt</a> Android-apparaat.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Open het voorbeeldproject in Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Configureer de account-id en -sleutel

Ga in het deelvenster **Project** naar `Assets/AzureSpatialAnchorsPlugin/Examples` en open het scènebestand `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sla de scène op door **Bestand** -> **Opslaan** te selecteren.

## <a name="export-the-android-studio-project"></a>Exporteer het Android Studio-project

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Selecteer **Exporteren** om een dialoogvenster te openen. Selecteer vervolgens een map om het Android Studio-project te exporteren.

Wanneer de export voltooid is, wordt een map weergegeven met het geëxporteerde Android Studio-project met een submap met de naam **HelloAR U3D**.

## <a name="deploy-the-android-application"></a>Implementeer de Android-toepassing

Open Android Studio en selecteer **Een bestaand Android Studio-project openen**. Selecteer vervolgens de **HelloAR U3D**-submap van het geëxporteerde Android Studio-project en klik op **OK**.

Tijdens het openen wordt een prompt weergegeven waarin wordt gevraagd de Gradle-wrapper te gebruiken. Selecteer **OK** om de Gradle-wrapper te gebruiken en open het project.

Start het Android-apparaat, meld u aan en maak verbinding met de pc via een USB-kabel.

Selecteer **Uitvoeren** via de werkbalk van Android Studio.

![Android Studio implementeren en uitvoeren](./media/get-started-unity-android/android-studio-deploy-run.png)

Selecteer het Android-apparaat in het dialoogvenster **Implementatiedoel selecteren** en selecteer **OK** om de app op het Android-apparaat uit te voeren.

Volg de instructies in de app om een anker te plaatsen en terug te halen.

> [!NOTE]
> Als u bij het uitvoeren van de app de camera niet als achtergrond ziet (u ziet bijvoorbeeld lege, blauwe of andere structuren) dan moet u waarschijnlijk assets opnieuw in Unity importeren. De app stoppen. Kies in het bovenste menu in Unity **Assets -> Alles opnieuw importeren**. Voer de vervolgens opnieuw app uit.

Stop de app door **Stop** te selecteren in de werkbalk van Android Studio.

![Android Studio Stop](./media/get-started-unity-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
