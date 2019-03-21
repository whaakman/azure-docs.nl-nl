---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: e8daaaf5b6b15eb3095f11e94c707a33b4b18e28
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305281"
---
Selecteer **bouwen**. In het dialoogvenster dat wordt geopend, moet u een map voor het exporteren van de Xcode-project te selecteren.

Wanneer de export voltooid is, verschijnt er een map met de geëxporteerde Xcode-project.

> [!NOTE]
> Als er een venster waarin u wordt gevraagd of u wilt vervangen of toe te voegen verschijnt, raden wij aan dat u selecteert **Append** omdat deze sneller. U hoeft alleen te selecteren **vervangen** als u activa in uw scène wijzigt. (Bijvoorbeeld, als u wilt toevoegen, verwijderen, of relaties tussen bovenliggende/onderliggende wijzigen of als u toevoegen wilt, verwijderen of wijzigen.) Als u alleen wijzigingen in de bron code **Append** moeten voldoende.

### <a name="open-the-xcode-project"></a>Xcode-project openen

Voer deze opdracht in de Terminal te installeren de CocoaPods die nodig zijn voor het project in de map geëxporteerde Xcode-project:

```bash
pod install --repo-update
```

Nu kunt u openen `Unity-iPhone.xcworkspace` aan het project openen in Xcode:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Als u ziet een `library not found for -lPods-Unity-iPhone` fout, u waarschijnlijk hebt geopend de `.xcodeproj` bestand in plaats van de `.xcworkspace` bestand. 

Selecteer de hoofdmap **Unity-iPhone** knooppunt wilt weergeven van de instellingen van het project en selecteer vervolgens de **algemene** tabblad.

Onder **ondertekening**, zorg ervoor dat **ondertekening automatisch beheren** is ingeschakeld. Als dat niet het geval is, het inschakelen en selecteer vervolgens **inschakelen automatische** in het dialoogvenster dat wordt weergegeven om de buildinstellingen te herstellen.

Controleer of onder **Deployment Info** de optie **Deployment Info** is ingesteld op `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>De app implementeren op uw iOS-apparaat

Verbind het iOS-apparaat met de Mac en stel het **actieve schema** in voor uw iOS-apparaat.

![Apparaat selecteren](./media/spatial-anchors-unity/select-device.png)

Selecteer **Build and then run the current scheme** (Het huidige schema compileren en uitvoeren).

![Implementeren en uitvoeren](./media/spatial-anchors-unity/deploy-run.png)