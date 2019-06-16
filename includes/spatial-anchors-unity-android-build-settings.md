---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 9798e5f76881be38fb27e1f428565caba6e50bf2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135081"
---
Open **Build Settings** door **File** > **Build Settings** te selecteren.

Selecteer in de sectie **Platform** de optie **Android**. Wijziging de **Build-systeem** naar **Gradle** en zorg ervoor dat de **Project exporteren** selectievakje niet is ingeschakeld.

Selecteer **Switch Platform** om het platform te wijzigen in **Android**. Unity vraagt u mogelijk ondersteuning voor Android om onderdelen te installeren als ze ontbreekt.

![Unity Build Settings venster](./media/spatial-anchors-unity/unity-android-build-settings.png)

Sluit het venster **Build Settings**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>De ARCore SDK voor Unity downloaden en importeren

Download de `unitypackage` -bestand uit de [ARCore SDK voor Unity 1.7 releases](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.7.0). Terug in de Unity-project, selecteer **activa** > **pakket importeren** > **Custom Package** en selecteer vervolgens de `unitypackage` bestand u hebt gedownload. In de **Unity-pakket importeren** in het dialoogvenster, controleert u of alle bestanden zijn geselecteerd en selecteer vervolgens **importeren**.
