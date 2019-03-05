---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 662aced6df27febdf29f2645725962763e89cfa2
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752550"
---
Open Unity en open het project in de map `Unity`.

Open **Build Settings** door **File** -> **Build Settings** te selecteren.

Selecteer in de sectie **Platform** de optie **Android**. Wijzig **Build System** in **Gradle** en schakel het selectievakje bij **Export project** in.

Selecteer **Switch Platform** om het platform te wijzigen in **Android**. U kunt worden gevraagd Android-ondersteuningsonderdelen te installeren als deze ontbreken.

![Buil-instellingen voor Unity](./media/spatial-anchors-unity/unity-android-build-settings.png)

Sluit het venster **Build Settings**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>De ARCore SDK voor Unity downloaden en importeren

Download bestand `unitypackage` in de [ARCore SDK voor Unity-releases](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). Weer terug in het Unity-project selecteert u **Assets** -> **Import Package** -> **Custom Package...** en vervolgens het bestand `unitypackage` dat u eerder hebt gedownload. Controleer in het dialoogvenster **Import Unity Package** of alle bestanden zijn geselecteerd en selecteer **Import**.
