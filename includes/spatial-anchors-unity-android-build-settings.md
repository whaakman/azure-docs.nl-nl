---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 33c932c36cd6de730d3768d596a214c442d74ae1
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632854"
---
Open **Build Settings** door **File** > **Build Settings** te selecteren.

Selecteer in de sectie **Platform** de optie **Android**. Wijziging de **Build-systeem** naar **Gradle** en selecteer **Project exporteren**.

Selecteer **Switch Platform** om het platform te wijzigen in **Android**. Unity vraagt u mogelijk ondersteuning voor Android om onderdelen te installeren als ze ontbreekt.

![Unity Build Settings venster](./media/spatial-anchors-unity/unity-android-build-settings.png)

Sluit het venster **Build Settings**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>De ARCore SDK voor Unity downloaden en importeren

Download de `unitypackage` -bestand uit de [ARCore SDK voor Unity 1.5 releases](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.5.0). Terug in de Unity-project, selecteer **activa** > **pakket importeren** > **Custom Package** en selecteer vervolgens de `unitypackage` bestand u hebt gedownload. In de **Unity-pakket importeren** in het dialoogvenster, controleert u of alle bestanden zijn geselecteerd en selecteer vervolgens **importeren**.
