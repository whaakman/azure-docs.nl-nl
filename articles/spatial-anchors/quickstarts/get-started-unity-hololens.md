---
title: 'Quickstart: HoloLens Unity-app maken met Azure Spatial Anchors | Microsoft Docs'
description: In deze quickstart leert u een HoloLens-app maken met Unity en met behulp van Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: c8fc99426751e9aaaec53cbb6b64a84b6e7f6734
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57894322"
---
# <a name="quickstart-create-a-hololens-unity-app-using-azure-spatial-anchors"></a>Quickstart: HoloLens Unity-app maken met Azure Spatial Anchors

In deze quickstart wordt besproken hoe u een HoloLens Unity-app maakt met behulp van [Azure Spatial Anchors](../overview.md). Azure Spatial Anchors is een platformoverschrijdende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Als u klaar bent, hebt u een HoloLens-app met Unity gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * Build-instellingen voor Unity voorbereiden
> * De Spatial Anchors-account-id en -accountsleutel configureren
> * Het HoloLens Visual Studio-project exporteren
> * Het project implementeren en uitvoeren in een HoloLens-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Een Windows-machine met <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a>, <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 +</a> geïnstalleerd met de **universele Windows-platformontwikkeling** werkbelasting, en <a href="https://git-scm.com/download/win" target="_blank">Git voor Windows</a>.
- Een HoloLens-apparaat waarvoor de [ontwikkelaarsmodus](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) is ingeschakeld. Dit artikel vereist een HoloLens-apparaat met de [Update voor Windows van 10 oktober 2018](https://docs.microsoft.com/en-us/windows/mixed-reality/release-notes-october-2018) (ook wel bekend als RS5). Als u wilt bijwerken naar de nieuwste release op HoloLens, opent u de app **Instellingen**, gaat u naar **Bijwerken en beveiliging** en selecteert u vervolgens de knop **Controleren op updates**.
- Op de app moet de mogelijkheid **SpatialPerception** zijn ingesteld onder **Build Settings**->**Player Settings**->**Publishing Settings**->**Capabilities**.
- Voor de app moet **Virtual Reality Supported** met **Windows Mixed Reality SDK** zijn ingeschakeld onder **Build Settings**->**Player Settings**->**XR Settings**.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Voorbeeldproject openen in Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Open Unity en open het project in de map `Unity`.

Open **Build Settings** door **File** -> **Build Settings** te selecteren.

in de sectie **Platform** selecteert u **Universeel Windows-platform**. Wijzig vervolgens **Doelapparaat** in **HoloLens**.

Selecteer **Switch Platform** om het platform te wijzigen in **Universeel Windows-platform**. Unity gevraagd UWP-ondersteuning om onderdelen te installeren als ze ontbreekt.

![Build-instellingen voor Unity](./media/get-started-unity-hololens/unity-build-settings.png)

Sluit het venster **Build Settings**.

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

Ga in het deelvenster **Project** naar `Assets/AzureSpatialAnchorsPlugin/Examples` en open het scènebestand `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sla de scène op door **File** -> **Save** te selecteren.

## <a name="export-the-hololens-visual-studio-project"></a>Het HoloLens Visual Studio-project exporteren

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Selecteer **Build** om een dialoogvenster te openen. Selecteer een map om het HoloLens Visual Studio-project te exporteren.

Als het exporteren is voltooid, wordt er een map weergegeven met het geëxporteerde HoloLens-project.

## <a name="deploy-the-hololens-application"></a>HoloLens-toepassing implementeren

Dubbelklik in de map op `HelloAR U3D.sln` om het project in Visual Studio te openen.

Wijzig **Solution Configuration** in **Release**, wijzig **Solution Platform** in **x86** en selecteer **Device** uit de opties voor het implementatiedoel.

![Configuratie van Visual Studio](./media/get-started-unity-hololens/visual-studio-configuration.png)

Start het HoloLens-apparaat, meld u aan en sluit het aan op de pc via een USB-kabel.

Selecteer **Debug** > **Start debugging** om de app te implementeren en de foutopsporing te starten.

Volg de instructies in de app om een anker te plaatsen en terug te halen.

Stop de app in Visual Studio door **Stop Debugging** te selecteren of door op **Shift+F5** te drukken.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
