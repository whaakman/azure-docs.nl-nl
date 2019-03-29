---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 355d89b1fd7d7cc8c9db58122d51c6ff7dff5f3b
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58622583"
---
Open in Unity, het project in de `Unity` map.

Open **Build Settings** door **File** > **Build Settings** te selecteren.

Selecteer in de sectie **Platform** de optie **Android**. Wijziging de **Build-systeem** naar **Gradle** en selecteer **Project exporteren**.

Selecteer **Switch Platform** om het platform te wijzigen in **Android**. Unity vraagt u mogelijk ondersteuning voor Android om onderdelen te installeren als ze ontbreekt.

![Unity Build Settings venster](./media/spatial-anchors-unity/unity-android-build-settings.png)

Sluit het venster **Build Settings**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>De ARCore SDK voor Unity downloaden en importeren

Download de `unitypackage` -bestand uit de [ARCore SDK voor Unity 1.5 releases](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). Terug in de Unity-project, selecteer **activa** > **pakket importeren** > **Custom Package** en selecteer vervolgens de `unitypackage` bestand u hebt gedownload. In de **Unity-pakket importeren** in het dialoogvenster, controleert u of alle bestanden zijn geselecteerd en selecteer vervolgens **importeren**.
