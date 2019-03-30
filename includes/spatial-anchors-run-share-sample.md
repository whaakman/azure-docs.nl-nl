---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 397a8a9b07b4d7a88d0345399ac4abcc3e738a82
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631148"
---
## <a name="set-up-your-device"></a>Uw apparaat instellen

Open in Unity, het project in de `Unity` map.

![Unity-venster](./media/spatial-anchors-unity/unity-window.png)

### <a name="set-up-an-android-device"></a>Een Android-apparaat instellen

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Instellen van een iOS-apparaat

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>De account-id en sleutel configureren

Ga in het deelvenster **Project** naar `Assets/AzureSpatialAnchorsPlugin/Examples` en open het scènebestand `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

In de **Inspector** deelvenster, voer de `Sharing Anchors Service url` (van uw ASP.NET-webtoepassing Azure-implementatie) als de waarde voor `Base Sharing Url`, waarbij `index.html` met `api/anchors`. Het moet er als volgt: `https://<app_name>.azurewebsites.net/api/anchors`.

Sla de scène op door **Bestand** > **Opslaan** te selecteren.

## <a name="deploy-to-your-device"></a>Uw apparaat implementeren

### <a name="deploy-to-android-device"></a>Implementeren op Android-apparaat

Meld u aan op uw Android-apparaat en deze op uw computer verbinding maken met behulp van een USB-kabel.

Open **Build Settings** door **File** > **Build Settings** te selecteren.

Onder **schermen In bouwt**, schakel het selectievakje naast de `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` scène en schakel het selectievakje markeert van alle andere schermen.

Zorg ervoor dat **Project exporteren** geen een vinkje. Selecteer **bouwen en uitvoeren**. U wordt gevraagd om op te slaan uw `.apk` bestand. U kunt een willekeurige naam voor het kiezen.

Volg de instructies in de app. U kunt selecteren **maken en delen anker** of **gedeeld anker Zoek**. Het eerste scenario maakt u een anker die later op hetzelfde apparaat of op een ander account worden kan. Het tweede scenario, kunt als u de app al hebt uitgevoerd op hetzelfde apparaat of op een ander account, u eerder hebt gedeeld ankers. Nadat u uw scenario kiest, leidt de app u verdere instructies om wat te doen. Bijvoorbeeld: u wordt gevraagd uw apparaat voor het verzamelen van omgevingsgegevens herinrichten. Later op, zult u plaatst u een anker in de hele wereld, wordt geduld terwijl deze wordt geüpload, enzovoort.

### <a name="deploy-to-an-ios-device"></a>Implementeren op een iOS-apparaat

Open **Build Settings** door **File** > **Build Settings** te selecteren.

Onder **schermen In bouwt**, schakel het selectievakje naast de `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo` scène en schakel het selectievakje markeert van alle andere schermen.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Volg de instructies in de app. U kunt selecteren **maken en delen anker** of **gedeeld anker Zoek**. Het eerste scenario maakt u een anker die later op hetzelfde apparaat of op een ander account worden kan. Het tweede scenario, kunt als u de app al hebt uitgevoerd op hetzelfde apparaat of op een ander account, u eerder hebt gedeeld ankers. Nadat u uw scenario kiest, leidt de app u verdere instructies om wat te doen. Bijvoorbeeld: u wordt gevraagd uw apparaat voor het verzamelen van omgevingsgegevens herinrichten. Later op, zult u plaatst u een anker in de hele wereld, wordt geduld terwijl deze wordt geüpload, enzovoort.

In Xcode, stopt u de app door te selecteren **stoppen**.
