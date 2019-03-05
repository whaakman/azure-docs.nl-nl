---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: b802c9dbd0cef65325cb03538b68b49c57b85bb3
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56890952"
---
Selecteer **Build** om een dialoogvenster te openen. Selecteer vervolgens een map om het Xcode-project te exporteren.

Als het exporteren is voltooid, wordt er een map weergegeven met het geëxporteerde Xcode-project.

### <a name="open-the-xcode-project"></a>Xcode-project openen

Voer in de projectmap met het geëxporteerde Xcode-project de volgende opdracht uit om de vereiste CocoaPods voor het project te installeren:

```bash
pod install --repo-update
```

U kunt nu `Unity-iPhone.xcworkspace` openen om het project in Xcode te openen:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Als u de fout `library not found for -lPods-Unity-iPhone` ziet, hebt u waarschijnlijk het bestand `.xcodeproj` geopend in plaats van `.xcworkspace`. Open `.xcworkspace` en probeer het opnieuw.

Selecteer het hoofdknooppunt **Unity-iPhone** om de projectinstellingen weer te geven en selecteer het tabblad **Algemeen**.

Selecteer onder **Ondertekenen** de optie **Automatically manage signing**. Selecteer in het dialoogvenster dat verschijnt de optie **Enable Automatic** om de instellingen voor de build opnieuw in te stellen.

Controleer of onder **Deployment Info** de optie **Deployment Info** is ingesteld op `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>De app implementeren op uw iOS-apparaat

Verbind het iOS-apparaat met de Mac en stel het **actieve schema** in voor uw iOS-apparaat.

![Apparaat selecteren](./media/spatial-anchors-unity/select-device.png)

Selecteer **Build and then run the current scheme**.

![Implementeren en uitvoeren](./media/spatial-anchors-unity/deploy-run.png)