---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 53f480b8858e2bbe7d4699d8637ecaa5ab0c08a3
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305252"
---
Open in Unity, het project in de `Unity` map.

Open **Build Settings** door **File** > **Build Settings** te selecteren.

Selecteer in de sectie **Platform** de optie **Android**. Wijziging de **Build-systeem** naar **Gradle** en selecteer **Project exporteren**.

Selecteer **Switch Platform** om het platform te wijzigen in **Android**. Unity vraagt u mogelijk ondersteuning voor Android om onderdelen te installeren als ze ontbreekt.

![Unity Build Settings venster](./media/spatial-anchors-unity/unity-android-build-settings.png)

Sluit het venster **Build Settings**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>De ARCore SDK voor Unity downloaden en importeren

Download bestand `unitypackage` in de [ARCore SDK voor Unity-releases](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). Terug in de Unity-project, selecteer **activa** > **pakket importeren** > **Custom Package** en selecteer vervolgens de `unitypackage` bestand u hebt gedownload. In de **Unity-pakket importeren** in het dialoogvenster, controleert u of alle bestanden zijn geselecteerd en selecteer vervolgens **importeren**.
