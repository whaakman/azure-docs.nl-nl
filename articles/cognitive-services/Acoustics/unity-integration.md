---
title: Integratie en implementatie van unit-project akoestische eenheid
titlesuffix: Azure Cognitive Services
description: In dit artikel wordt uitgelegd hoe u de integratie van de invoeg toepassing project akoestische unit-eenheid in uw Unity-project kunt zien.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 54bc98e0ddba0292c6a5dbb07f2bbdfce6a1cb45
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933146"
---
# <a name="project-acoustics-unity-integration"></a>Integratie van de eenheid project akoestische unit
In dit artikel wordt uitgelegd hoe u de integratie van de invoeg toepassing project akoestische unit-eenheid in uw Unity-project kunt zien.

Software vereisten:
* [Unity 2018.2 +](https://unity3d.com) voor Windows
* [Eenheids pakket voor project akoestische](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>De invoeg toepassing importeren
Importeer de akoestische UnityPackage naar uw project. 
* Ga in eenheid naar **activa > import pakket > aangepast pakket...**

    ![Scherm opname van het menu pakket eenheids import](media/import-package.png)  

* Kies **ProjectAcoustics. unitypackage**

* Klik op de knop **importeren** om het Unity-pakket in uw project te integreren  

    ![Scherm opname van unit import package-dialoog venster](media/import-dialog.png)  

Als u de invoeg toepassing in een bestaand project importeert, heeft het project mogelijk al een **mcs. RSP** -bestand in de hoofdmap van het project, waarmee de C# opties voor de compiler worden opgegeven. U moet de inhoud van dat bestand samen voegen met het bestand mcs. RSP dat wordt meegeleverd met de invoeg toepassing voor project akoestische.

## <a name="enable-the-plugin"></a>De invoeg toepassing inschakelen
Het maken-gedeelte van de akoestische Toolkit vereist de .NET 4. x Scripting runtime-versie. Bij het importeren van pakketten worden de instellingen voor Unity Player bijgewerkt. De eenheid moet opnieuw worden opgestart om deze instelling van kracht te laten worden.

![Scherm afbeelding van het deel venster instellingen voor Unity Player](media/player-settings.png)

![Scherm afbeelding van het deel venster instellingen voor Unity Player met .NET 4,5 geselecteerd](media/net45.png)

## <a name="set-up-audio-dsp"></a>Audio-DSP instellen
De geluids fragmenten van het project omvatten audio runtime DSP die kan worden geïntegreerd in het spatializer Framework van unit-audio-engine. Het bevat zowel HRTF als gesleepte spatialization. Schakel de project akoestische DSP in door de instellingen voor unit-audio te openen met behulp van **> project instellingen > audio**en vervolgens de **geluids** fragmenten van het project te selecteren als de **Spatializer-invoeg toepassing** voor uw project. Zorg ervoor dat de **DSP-buffer grootte** is ingesteld op de beste prestaties.

![Scherm afbeelding van het deel venster instellingen voor Unity-project](media/project-settings.png)  

![Scherm afbeelding van het deel venster instellingen voor Unity Spatializer met project akoestische Spatializer geselecteerd](media/choose-spatializer.png)

Open vervolgens de audio mixer (**Window > audio mixer**). Zorg ervoor dat u ten minste één mixer hebt, met één groep. Als dat niet het geval is, klikt u op de knop + rechtsvan mixers. Klik met de rechter muisknop op de onderkant van de kanaal strook in de sectie effecten en voeg het effect van het mixer voor het **project akoestische** toe. Houd er rekening mee dat er slechts één akoestische mixer voor het project tegelijk wordt ondersteund.

![Scherm opname van unit-audio mixer die als host fungeert voor project akoestische mixer](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Akoestische geluids bronnen inschakelen
Maak een audio bron. Klik op het selectie vakje onder aan het Inspector-deel venster van AudioSource dat **Spatialize**bevat. Zorg ervoor dat **ruimtelijke Blend** is ingesteld op volledige 3D.  

![Scherm opname van unit-audio bron paneel](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Akoestische ontwerp inschakelen
Koppel het script **AcousticsAdjust** aan een geluids bron in uw scène om aanvullende para meters voor de bron ontwerp in te scha kelen door te klikken op **onderdeel toevoegen** en scripts te kiezen **> geluids aanpassingen**:

![Scherm opname van Unity AcousticsAdjust-script](media/acoustics-adjust.png)

## <a name="next-steps"></a>Volgende stappen
* [Maken uw scène met project akoestische voor Unity](unity-baking.md)
* [Een Azure batch-account maken](create-azure-account.md) om uw scène in de cloud te maken
* Verken het ontwerp proces voor het ontwikkelen van de [project akoestische eenheid](unity-workflow.md).

