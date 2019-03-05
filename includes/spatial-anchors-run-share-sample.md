---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 5f1e0153b1f919bc9d7e921d2a1b3ae745b2b01f
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753474"
---
## <a name="open-the-sample-project-in-unity"></a>Het voorbeeldproject openen in Unity

[!INCLUDE [Clone Sample Repo](spatial-anchors-clone-sample-repository.md)]

## <a name="to-set-up-for-an-android-device"></a>Instellen voor een Android-apparaat

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

## <a name="to-set-up-for-an-ios-device"></a>Instellen voor een iOS-apparaat

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

Ga in het deelvenster **Project** naar `Assets/AzureSpatialAnchorsPlugin/Examples` en open het scènebestand `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Voer in het deelvenster **Inspector** de `Sharing Anchors Service url` (van uw ASP.NET-web-app-implementatie in Azure) als de waarde voor `Base Sharing Url`, waarbij u `index.html` door `api/anchors` vervangt. Het zou er dus als volgt uit moeten zien: `https://<app_name>.azurewebsites.net/api/anchors`.

Sla de scène op door **File** -> **Save** te selecteren.

## <a name="to-deploy-to-an-android-device"></a>Android-apparaat implementeren

Start het Android-apparaat, meld u aan en maak verbinding met de pc via een USB-kabel.

Open **Build Settings** door **File** -> **Build Settings** te selecteren.

Plaats een vinkje onder **Scenes In Build**, naast de `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo`-scène en wis de vinkjes voor alle overige scènes.

Controleer of er geen vinkje staat voor **Export Project**. Klik op **Build And Run**. U wordt gevraag het `.apk`-bestand op te slaan. Geef het een willekeurige naam.

Volg de instructies in de app. U kunt kiezen voor **Create & Share Anchor** of **Locate Shared Anchor**. Met de eerste optie kunt u een anker maken dat later op hetzelfde of een ander apparaat kan worden gevonden. Met de tweede optie kunt u, indien u de app eerder hebt uitgevoerd (op hetzelfde of op een ander apparaat), eerder gedeelde ankers vinden.

## <a name="to-deploy-to-an-ios-device"></a>Implementeren op een iOS-apparaat

Open **Build Settings** door **File** -> **Build Settings** te selecteren.

Plaats een vinkje onder **Scenes In Build**, naast de `AzureSpatialAnchorsPlugin/Examples/AzureSpatialAnchorsLocalSharedDemo`-scène en wis de vinkjes voor alle overige scènes.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

Volg de instructies in de app. U kunt kiezen voor **Create & Share Anchor** of **Locate Shared Anchor**. Met de eerste optie kunt u een anker maken dat later op hetzelfde of een ander apparaat kan worden gevonden. Met de tweede optie kunt u, indien u de app eerder hebt uitgevoerd (op hetzelfde of op een ander apparaat), eerder gedeelde ankers vinden.

In Xcode: stop de app door op **Stop** te drukken.
