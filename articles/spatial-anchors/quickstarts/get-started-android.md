---
title: 'Quickstart: Android-app maken met Azure Spatial Anchors | Microsoft Docs'
description: In deze quickstart leert u een Android-app bouwen met behulp van Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 64e5e78f93488b2c375c617e8857c84ba2171f00
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822392"
---
# <a name="quickstart-create-an-android-app-with-azure-spatial-anchors"></a>Quickstart: Een Android-app maken met Azure Spatial Anchors

In deze quickstart wordt besproken hoe u een Android-app maakt met behulp van [Azure Spatial Anchors](../overview.md) in Java of C++/NDK. Azure Spatial Anchors is een platformoverschrijdende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Als u klaar bent, hebt u een ARCore Android-app gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * Spatial Anchors-account-id en -accountsleutel configureren
> * Implementeren en uitvoeren op een Android-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Een Windows- of macOS-machine met <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a>.
  - Om het NDK-voorbeeld te bouwen, moet u ook de NDK en CMake 3.6 SDK-hulpprogramma’s in Android Studio installeren.
- Een <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">door een ontwikkelaar geactiveerd</a> en <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">voor ARCore geschikt</a> Android-apparaat.
- Uw app moet gericht zijn op ARCore 1.5 (ondersteuning voor ARCore 1.6+ volgt binnenkort)

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Voorbeeldproject openen

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Als u het Android NDK-voorbeeld aan het bouwen bent, moet u `arcore_c_api.h` vanaf [hier](https://raw.githubusercontent.com/google-ar/arcore-android-sdk/v1.5.0/libraries/include/arcore_c_api.h) downloaden en in `Android\NDK\libraries\include` plaatsen.

Open Android Studio.

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Selecteer **Bestaand Android Studio-project openen** en selecteer het project dat zich bevindt in `Android/Java/`.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Selecteer **Een bestaand Android Studio-project openen** en selecteer het project dat zich bevindt in `Android/NDK/`.

***

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

De volgende stap is het gebruiken van de account-id en -sleutel die eerder zijn vastgelegd bij het opzetten van de Spatial Anchors-resource om de app te configureren.

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Open `Android/Java/app/src/main/java/com/microsoft/sampleandroid/AzureSpatialAnchorsActivity.java`.

Zoek het `SpatialAnchorsAccountKey`-veld en vervang `Set me` met de accountsleutel.

Zoek het `SpatialAnchorsAccountId`-veld en vervang `Set me` met de account-id.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Open `Android/NDK/app/src/main/cpp/spatial_services_application.cc`.

Zoek het `SpatialAnchorsAccountKey`-veld en vervang `Set me` met de accountsleutel.

Zoek het `SpatialAnchorsAccountId`-veld en vervang `Set me` met de account-id.

***

## <a name="deploy-the-app-to-your-android-device"></a>De app op uw Android-apparaat implementeren

Start het Android-apparaat, meld u aan en maak verbinding met de pc via een USB-kabel.

Selecteer **Uitvoeren** via de werkbalk van Android Studio.

![Android Studio implementeren en uitvoeren](./media/get-started-android/android-studio-deploy-run.png)

Selecteer het Android-apparaat in het dialoogvenster **Implementatiedoel selecteren** en selecteer **OK** om de app op het Android-apparaat uit te voeren.

Volg de instructies in de app om een anker te plaatsen en terug te halen.

Stop de app door **Stop** te selecteren in de werkbalk van Android Studio.

![Android Studio stoppen](./media/get-started-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
