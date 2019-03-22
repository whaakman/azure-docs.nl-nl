---
title: Project akoestische Unity-integratie en implementatie
titlesuffix: Azure Cognitive Services
description: Deze procedures wordt uitgelegd integratie van het Project akoestische Unity-invoegtoepassing in uw Unity-project.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/14/2019
ms.author: kegodin
ms.openlocfilehash: 7d8edd7b092ee1ed4f953d753b2bbe864e075378
ms.sourcegitcommit: f68b0e128f0478444740172f54e92b453df696be
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58137728"
---
# <a name="project-acoustics-unity-integration"></a>Project akoestische Unity-integratie
Deze procedures wordt uitgelegd integratie van het Project akoestische Unity-invoegtoepassing in uw Unity-project.

Softwarevereisten:
* [Unity 2018.2 +](http://unity3d.com) voor Windows
* [Project akoestische Unity-pakket](https://www.microsoft.com/en-us/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>De invoegtoepassing te importeren
Importeer de akoestische UnityPackage aan uw project. 
* In Unity, gaat u naar **activa > pakket importeren > Custom Package...**

    ![Pakket importeren](media/import-package.png)  

* Kies **ProjectAcoustics.unitypackage**

Als u de invoegtoepassing in een bestaand project importeert, het project mogelijk al een **mcs.rsp** bestand in de hoofdmap van het project, die Hiermee geeft u opties voor het C#-compileerprogramma. U moet de inhoud van het bestand met de mcs.rsp-bestand dat wordt geleverd met de invoegtoepassing Project akoestische samenvoegen.

## <a name="enable-the-plugin"></a>De invoegtoepassing inschakelen
Het gedeelte bake van de toolkit akoestische vereist de scripting runtime-versie van .NET 4.x. Importeren van het pakket worden de instellingen voor de Unity-speler bijgewerkt. Start opnieuw op de eenheid voor deze instelling van kracht te laten worden.

![Instellingen van de speler](media/player-settings.png)

![.NET 4.5](media/net45.png)

## <a name="set-up-audio-dsp"></a>Audio-DSP instellen
Project akoestische bevat audio runtime DSP die kan worden geïntegreerd in het kader van Unity-engine audio ruimtelijk. Dit omvat zowel HRTF gebaseerde en panning spatialization. Het Project akoestische DSP inschakelen door het openen van de audio Unity-instellingen met behulp van **bewerken > instellingen voor het Project > Audio**, vervolgens de optie **Project akoestische** als de **ruimtelijk-invoegtoepassing** voor uw project. Zorg ervoor dat de **DSP-buffergrootte** is ingesteld op de beste prestaties.

![Instellingen voor het project](media/project-settings.png)  

![Project akoestische ruimtelijk](media/choose-spatializer.png)

Open vervolgens de Audio-Mixer (**venster > Audio Mixer**). Zorg ervoor dat u hebt ten minste één Mixer met één groep. Als u dit niet doet, klikt u op de knop '+' aan de rechterkant van **Mixers**. Met de rechtermuisknop op de onderkant van de strook kanaal in de sectie effecten en voeg de **Project akoestische Mixer** effect. Houd er rekening mee dat er slechts één Project akoestische Mixer wordt ondersteund op een tijdstip.

![Audio Mixer](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Akoestische op geluid bronnen inschakelen
Maak een audiobron. Klik op het selectievakje aan de onderkant van van de AudioSource inspector deelvenster met de melding dat **Spatialize**. Zorg ervoor dat **ruimtelijke Blend** is ingesteld op de volledige 3D.  

![Audiobron](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Akoestische ontwerp inschakelen
Koppelen van het script **AcousticsAdjust** met een goede bron in uw scène waarmee extra bron ontwerpparameters door te klikken op **onderdeel toevoegen** en het kiezen van **Scripts > akoestische aanpassen** :

![AcousticsAdjust](media/acoustics-adjust.png)

## <a name="next-steps"></a>Volgende stappen
* [Een scène met akoestische voor Unity-Project maken](unity-baking.md)
* [Azure Batch-account maken](create-azure-account.md) te maken van uw scène in de cloud
* Verken de [Project akoestische Unity-ontwerpproces](unity-workflow.md).

