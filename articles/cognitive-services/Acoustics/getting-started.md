---
title: Aan de slag met Project akoestische
titlesuffix: Azure Cognitive Services
description: Deze snelstartgids wordt beschreven hoe integreren van de invoegtoepassing in uw Unity-project, het maken van uw scène en het toepassen van de akoestische geluid gegevensbronnen.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 9569b161dd5c6c79c5333d585f9ab4fcdb55fa97
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57533983"
---
# <a name="getting-started-with-project-acoustics"></a>Aan de slag met Project akoestische
Deze snelstartgids wordt beschreven hoe integreren van de invoegtoepassing in uw Unity-project, het maken van uw scène en het toepassen van de akoestische geluid gegevensbronnen. Voor deze Quick Start, moet u eerst maken een [Azure batch-account](create-azure-account.md). Deze handleiding wordt ervan uitgegaan dat bekend zijn met Unity.

## <a name="download-the-plugin"></a>De invoegtoepassing downloaden
Registreren [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) voor deelname aan het Designer-Preview.

## <a name="supported-platforms-for-quickstart"></a>Ondersteunde platforms voor Quick Start
* [Unity 2018.2 +](https://www.unity3d.com)
  * Vereist als u uw project op de **.NET 4.x Equivalent** scripting runtime-versie 
  * Vereist dat de eenheid op basis van een Windows-editor

## <a name="import-the-plugin"></a>De invoegtoepassing te importeren
Importeer de akoestische UnityPackage aan uw project. 
* In Unity, gaat u naar **activa > pakket importeren > Custom Package...**

![Pakket importeren](media/ImportPackage.png)  

* Choose **MicrosoftAcoustics.unitypackage**

Als u de invoegtoepassing in een bestaand project importeert, het project mogelijk al een **mcs.rsp** bestand in de hoofdmap van het project, die Hiermee geeft u opties voor het C#-compileerprogramma. U moet de inhoud van het bestand met de mcs.rsp-bestand dat wordt geleverd met de invoegtoepassing Project akoestische samenvoegen.

## <a name="enable-the-plugin"></a>De invoegtoepassing inschakelen
Het gedeelte bake van de toolkit akoestische vereist de scripting runtime-versie van .NET 4.x. Importeren van het pakket worden de instellingen voor de Unity-speler bijgewerkt. Start opnieuw op de eenheid voor deze instelling van kracht te laten worden.

![Instellingen van de speler](media/PlayerSettings.png)

![.NET 4.5](media/Net45.png)

## <a name="create-a-navigation-mesh"></a>Een navigatie-net maken
Gebruikmaken van de standaard [Unity werkstroom](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) te maken van een net navigatie voor uw project. Zie voor meer informatie over het gebruik van uw eigen netten navigatiedeelvenster de [bake UI doorlopen](bake-ui-walkthrough.md).

## <a name="mark-static-meshes-for-acoustics"></a>Statische netten voor akoestische markeren
Open de akoestische venster met behulp **venster > akoestische** in Unity. In dit venster kunt naast de Inspector worden gekoppeld.

![Open akoestische venster](media/WindowAcoustics.png)

Selecteer in het venster van de hiërarchie van Unity, ongedaan maken alle geselecteerde items. In de akoestische **Object** tabblad klikt u op het selectievakje "Akoestische geometrie" markeert alle netten en terreinen in uw scène als akoestische geometry.

Op de **materiaal** tabblad, het akoestisch materiaal toewijzen aan materiaal dat wordt gebruikt in uw scène. De **standaard** materiaal is gelijk aan concreet opname. Zie voor meer informatie over het opgeven van de eigenschappen van uw eigen materialen, de [ontwerp proces pagina](design-process.md).

![Tabblad materiaal](media/MaterialsTab.png)

## <a name="preview-the-probes"></a>Voorbeeld van de tests
Op de **tests** tabblad **berekenen**. Deze berekening duurt een paar minuten, afhankelijk van het scène-grootte. Wanneer de berekening is voltooid, ziet u zwevende gebied in de weergave scène die de locaties voor akoestische simulatie, met de naam "test punten" markeren. Als u dicht genoeg bij elkaar voor een object in het venster scène, ziet u ook de scène voxelization. De groene voxels moet uitgelijnd met de objecten die u hebt gemarkeerd als geometry. De test-punten en voxel geeft kunnen worden gezet in het menu Gizmos in de rechterbovenhoek van het scène-weergave.

![GizmosPreview](media/BakePreviewWithGizmos.png)

## <a name="bake-the-scene"></a>Maken van de scène
In de **verdient** tabblad, Voer uw Azure-referenties in en klikt u op **verdient**. Als u geen Azure Batch-account hebt, raadpleegt u [in dit scenario voor onze aanbevolen accountconfiguratie](create-azure-account.md).
Wanneer de bake is voltooid, wordt het bestand automatisch gedownload naar de **activa/AcousticsData** map in uw project.

## <a name="set-up-audio-runtime-dsp"></a>Audio runtime DSP instellen
We de audio runtime DSP voor akoestische insluiten in de Unity ruimtelijk framework en integreer deze met HRTF gebaseerde spatialization. Als u akoestische verwerking, overschakelt naar de **Microsoft Acoustics** ruimtelijk door te gaan naar **bewerken > instellingen voor het Project > Audio**, en selecteer **Microsoft Acoustics** Als de **ruimtelijk invoegtoepassing** voor uw project. Zorg er ook voor de **DSP-buffergrootte** is ingesteld op de beste prestaties.

![Instellingen voor het project](media/ProjectSettings.png)  

![Project akoestische ruimtelijk](media/ChooseSpatializer.png)

Open de Audio Mixer (**venster > Audio Mixer**). Zorg ervoor dat u hebt ten minste één Mixer met één groep. Als u dit niet doet, klikt u op de knop '+' aan de rechterkant van **Mixers**. Met de rechtermuisknop op de onderkant van de strook kanaal in de sectie effecten en voeg de **Microsoft akoestische Mixer** effect. Houd er rekening mee dat er slechts één Project akoestische Mixer wordt ondersteund op een tijdstip.

![Audio Mixer](media/AudioMixer.png)

## <a name="set-up-the-acoustics-lookup-table"></a>Instellen van de opzoektabel akoestische
Sleep en zet de **Microsoft Acoustics** prefab uit het deelvenster project in uw scène:

![Akoestische Prefab](media/AcousticsPrefab.png)

Klik op de **ProjectAcoustics** Game-Object en Ga naar het deelvenster inspector. Geef de locatie van uw bake resultaat die (de. Bestand optimaal **activa/AcousticsData**) door slepen en neerzetten in het script akoestische Manager of door te klikken op de knop cirkel naast het tekstvak.

![Akoestische Manager](media/AcousticsManager.png)  

## <a name="apply-acoustics-to-sound-sources"></a>Akoestische van toepassing op geluid bronnen
Maak een audiobron. Klik op het selectievakje aan de onderkant van van de AudioSource inspector deelvenster met de melding dat **Spatialize**. Zorg ervoor dat **ruimtelijke Blend** is ingesteld op de volledige 3D.  

![Audiobron](media/AudioSource.png)

## <a name="apply-post-bake-design"></a>Na bake ontwerp toepassen
U kunt het script toevoegen **AcousticsAdjust** met een goede bron in uw scène waarmee extra bron ontwerpparameters door te klikken op **onderdeel toevoegen** en het kiezen van **Scripts > akoestische Aanpassen**:

![AcousticsAdjust](media/AcousticsAdjust.png)

Er zijn ook parameters op de **Microsoft akoestische Mixer**. Zie voor meer informatie over het ontwerpen van na bake [parameters ontwerpen](design-process.md).

## <a name="next-steps"></a>Volgende stappen
* Probeer de [voorbeeld scène](sample-walkthrough.md)
* Meer informatie over de volledige set [functies maken](bake-ui-walkthrough.md)
* Ontdek meer gedetailleerde [parameters ontwerpen](design-process.md)

