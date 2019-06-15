---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: b46a2b18309851bbe2934980137a53d2de6f6efc
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135358"
---
## <a name="set-up-your-device-in-unity"></a>Instellen van uw apparaat in Unity

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

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

Onder **schermen In bouwt**, zorg ervoor dat alle schermen hebben een vinkje naast.

Zorg ervoor dat **Project exporteren** geen een vinkje. Selecteer **bouwen en uitvoeren**. U wordt gevraagd om op te slaan uw `.apk` bestand. U kunt een willekeurige naam voor het kiezen.

Zodra de app wordt gestart, in de **kiest u een Demo** dialoogvenster, gebruikt u de pijl naar links of rechts selecteren de **LocalShare** optie en tik op **gaan!** . Volg de instructies in de app. U kunt selecteren **maken en delen anker** of **gedeeld anker Zoek**.

Het eerste scenario maakt u een anker die later op hetzelfde apparaat of op een ander account worden kan.
Het tweede scenario, kunt als u de app al hebt uitgevoerd op hetzelfde apparaat of op een ander account, u eerder hebt gedeeld ankers. Nadat u uw scenario kiest, leidt de app u verdere instructies om wat te doen. Bijvoorbeeld: u wordt gevraagd uw apparaat voor het verzamelen van omgevingsgegevens herinrichten. U gaat later plaatst u een anker in de hele wereld, wacht op te slaan en enzovoort.

### <a name="deploy-to-an-ios-device"></a>Implementeren op een iOS-apparaat

Open **Build Settings** door **File** > **Build Settings** te selecteren.

Onder **schermen In bouwt**, zorg ervoor dat alle schermen hebben een vinkje naast.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Zodra de app wordt gestart, in de **kiest u een Demo** dialoogvenster, gebruikt u de pijl naar links of rechts selecteren de **LocalShare** optie en tik op **gaan!** . Volg de instructies in de app. U kunt selecteren **maken en delen anker** of **gedeeld anker Zoek**.

Het eerste scenario maakt u een anker die later op hetzelfde apparaat of op een ander account worden kan.
Het tweede scenario, kunt als u de app al hebt uitgevoerd op hetzelfde apparaat of op een ander account, u eerder hebt gedeeld ankers. Nadat u uw scenario kiest, leidt de app u verdere instructies om wat te doen. Bijvoorbeeld: u wordt gevraagd uw apparaat voor het verzamelen van omgevingsgegevens herinrichten. U gaat later plaatst u een anker in de hele wereld, wacht op te slaan en enzovoort.

In Xcode, stopt u de app door te selecteren **stoppen**.
