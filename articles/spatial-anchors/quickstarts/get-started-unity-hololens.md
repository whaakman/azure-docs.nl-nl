---
title: 'Snelstart: maken van een Unity HoloLens-app met Azure ruimtelijke ankers | Microsoft Docs'
description: In deze quickstart leert u een HoloLens-app maken met Unity en met behulp van Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: c29819d817138f2512420584947763247837a9ea
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "67135205"
---
# <a name="quickstart-create-a-unity-hololens-app-that-uses-azure-spatial-anchors"></a>Quickstart: Maken van een Unity HoloLens-app die gebruikmaakt van Azure ruimtelijke ankers

In deze snelstartgids maakt u een Unity HoloLens-app die gebruikmaakt van [Azure ruimtelijke ankers](../overview.md). Ruimtelijke ankers is een platformoverschrijdende developer-service waarmee u het maken van gemengde realiteit ervaringen met objecten die hun locatie opslaan op apparaten na verloop van tijd. Als u klaar bent, hebt u een HoloLens-app met Unity gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

- Maak een account ruimtelijke ankers.
- Instellingen voor het bouwen van Unity voorbereiden.
- Configureer de ruimtelijke ankers account-id en de accountsleutel.
- Exporteer het HoloLens Visual Studio-project.
- De app implementeren en uitvoeren op een apparaat HoloLens.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze snelstart:


- U moet een Windows-computer waarop <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3</a> of hoger en <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> of later zijn geïnstalleerd. Uw Visual Studio-installatie omvat de **universele Windows-platformontwikkeling** werkbelasting. Installeer <a href="https://git-scm.com/download/win" target="_blank">Git voor Windows</a>.
- U moet een HoloLens-apparaat waarop [modus voor ontwikkelaars](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) ingeschakeld. [Update voor Windows 10 oktober 2018](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018) (ook wel bekend als RS5) moet worden geïnstalleerd op het apparaat. Als u wilt bijwerken naar de nieuwste release op HoloLens, opent u de **instellingen** app, gaat u naar **bijwerken en beveiliging**, en selecteer vervolgens **controleren op updates**.
- Op uw app, moet u inschakelen de **SpatialPerception** mogelijkheid. Deze instelling is in **Build Settings** > **Player Settings** > **publicatie-instellingen**  >   **Mogelijkheden**.
- Op uw app, moet u inschakelen **Virtual Reality ondersteund** met **Windows Mixed Reality SDK**. Deze instelling is in **Build Settings** > **Player Settings** > **XR instellingen**.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Downloaden en openen van het voorbeeldproject Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

Open **Build Settings** door **File** > **Build Settings** te selecteren.

in de sectie **Platform** selecteert u **Universeel Windows-platform**. Wijzig de **doelapparaat** naar **HoloLens**.

Selecteer **Switch Platform** om het platform te wijzigen in **Universeel Windows-platform**. Unity vraagt u mogelijk UWP-ondersteuning om onderdelen te installeren als ze ontbreekt.

![Unity Build Settings venster](./media/get-started-unity-hololens/unity-build-settings.png)

Sluit het venster **Build Settings**.

## <a name="configure-the-account-identifier-and-key"></a>De account-id en sleutel configureren

In de **Project** deelvenster, Ga naar `Assets/AzureSpatialAnchorsPlugin/Examples` en open de `AzureSpatialAnchorsBasicDemo.unity` scène-bestand.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sla de scène op door **Bestand** > **Opslaan** te selecteren.

## <a name="export-the-hololens-visual-studio-project"></a>Het HoloLens Visual Studio-project exporteren

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Selecteer **bouwen**. Selecteer een map waarin u voor het exporteren van het HoloLens Visual Studio-project in het dialoogvenster.

Wanneer de export voltooid is, verschijnt er een map met de geëxporteerde HoloLens-project.

## <a name="deploy-the-hololens-application"></a>HoloLens-toepassing implementeren

Dubbelklik in de map **HelloAR U3D.sln** aan het project openen in Visual Studio.

Wijzigt de **oplossingsconfiguratie** te **Release**, wijzigen de **Oplossingsplatform** te **x86**, en selecteer **apparaat**  van de doel-implementatieopties.

Als u HoloLens 2, gebruikt u **ARM** als de **Oplossingsplatform**, in plaats van **x86**.

   ![Visual Studio-configuratie](./media/get-started-unity-hololens/visual-studio-configuration.png)

Schakel het apparaat HoloLens, meld u aan en verbind het apparaat met de PC met behulp van een USB-kabel.

Selecteer **Debug** > **Start debugging** om de app te implementeren en de foutopsporing te starten.

Volg de instructies in de app om een anker te plaatsen en terug te halen.

In Visual Studio, stopt u de app erin **Stop Debugging** of Shift + F5.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Ruimtelijke ankers delen tussen apparaten](../tutorials/tutorial-share-anchors-across-devices.md)
