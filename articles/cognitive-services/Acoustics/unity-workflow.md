---
title: Zelfstudie voor project akoestische Unity-ontwerp
titlesuffix: Azure Cognitive Services
description: Deze zelfstudie wordt de ontwerp-werkstroom beschreven voor akoestische in Unity-Project.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 01783aa12f586f61583b1503c796f9b523770104
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58310622"
---
# <a name="project-acoustics-unity-design-tutorial"></a>Zelfstudie voor project akoestische Unity-ontwerp
Deze zelfstudie wordt de werkstroom en ontwerphulpprogramma's beschreven voor akoestische in Unity-Project.

Vereisten:
* Unity 2018.2 + voor Windows
* Een scène Unity aan een asset voltooide akoestische

Voor deze zelfstudie krijgt u een Unity-scène met een voltooide akoestische asset op twee manieren:
* [Akoestische Project toevoegen aan uw Unity-project](unity-integration.md), klikt u vervolgens [ophalen van een Azure Batch-account](create-azure-account.md), klikt u vervolgens [maken van uw Unity-scène](unity-baking.md)
* Of gebruik de [Project akoestische Unity-voorbeeldinhoud](unity-quickstart.md).

## <a name="review-design-process-concepts"></a>Proces-ontwerpconcepten controleren
Project akoestische maakt gebruik van algemene audio digitale signaalverwerking (DSP)-methoden voor het verwerken van uw bronnen en geeft die u controle over vertrouwde akoestische eigenschappen met inbegrip van bedekking, NAT/droge mix en weerklank tail-lengte (RT60). Maar de belangrijkste [Project akoestische proces ontwerp](design-process.md) die in plaats van rechtstreeks met het instellen van deze eigenschappen is, wordt u beheren hoe simulatieresultaten worden gebruikt om deze eigenschappen te stimuleren. De standaardinstellingen voor elk besturingselement vertegenwoordigen fysiek nauwkeurige akoestische.

## <a name="design-acoustics-for-each-source"></a>Ontwerp akoestische voor elke bron
Project akoestische biedt een aantal besturingselementen voor bron-specifieke akoestische ontwerp. Hiermee kunt u beheren van de samenstelling in een scène met enkele bronnen die benadrukken en deactivering emphasizing anderen.

### <a name="adjust-distance-based-attenuation"></a>Op basis van afstand afname aanpassen
De audio DSP geleverd door de **Project akoestische** ruimtelijk-invoegtoepassing voor Unity respecteert de per-source afstand gebaseerde afname ingebouwd in de Unity-Editor. Besturingselementen voor afname op basis van afstand zijn de **Audio bron** onderdeel gevonden de **Inspector** Configuratiescherm van geluid gegevensbronnen onder **3D-geluidsinstellingen**:

![Schermafbeelding van Unity afstand afname deelvenster met opties](media/distance-attenuation.png)

Akoestische uitvoeren berekening in een "regio simulatie" gecentreerd rond de player-locatie. Als een goede bron ver van de speler is, buiten deze regio simulatie beïnvloedt alleen geometrie in het vak de geluid doorgifte (zoals waardoor bedekking) die redelijk goed werkt wanneer occluders zijn in de buurt van de speler. Echter, in gevallen wanneer de speler in de open ruimte is, maar de occluders in de buurt van de verafgelegen geluid bron zijn, het geluid kunt worden onrealistisch disoccluded. Onze voorgestelde tijdelijke oplossing is om in dergelijke gevallen ervoor te zorgen dat het geluid afname uit op 0 bij ongeveer 45 m, de standaard horizontale afstand van de speler aan de rand van het vak valt.

![Schermafbeelding van Unity SpeakerMode optiepaneel](media/speaker-mode.png)

### <a name="adjust-occlusion-and-transmission"></a>Bedekking en verzending aanpassen
Bezig met koppelen van de **AcousticsAdjust** script naar een gegevensbron kunt afstemmen parameters voor die bron. Als u wilt koppelen het script, klikt u op **onderdeel toevoegen** aan de onderkant van de **Inspector** deelvenster en vervolgens naar **Scripts > akoestische aanpassen**. Het script heeft zes besturingselementen:

![Schermafbeelding van Unity AcousticsAdjust script](media/acoustics-adjust.png)

* **Inschakelen van akoestische** -bepaalt of akoestische is toegepast op deze gegevensbron. Wanneer dit selectievakje uitschakelt, wordt de bron worden spatialized met HRTFs of pannen, maar er is geen akoestische. Dit betekent dat er geen obstakel, bedekking of dynamische weerklank parameters zoals niveau en decay tijd. Weerklank wordt nog steeds toegepast met een vaste niveau en de vervaltijd.
* **Bedekking** -een vermenigvuldiger van toepassing op het niveau van de bedekking dB berekend door het systeem akoestische. Als deze vermenigvuldiger groter dan 1 is, bedekking wordt exaggerated, terwijl waarden minder dan 1 maken het effect van bedekking subtielere en een waarde van 0 schakelt bedekking.
* **Overdracht (dB)** -de afname (in de database) veroorzaakt door een overdracht via geometrie instellen. Deze schuifregelaar ingesteld op het laagste niveau om uit te schakelen verzending. Akoestische spatializes de initiële droge audio als binnenkomen rond scène geometrie (portaling). Verzending biedt een extra droge aankomst die in de richting van de regel van zicht is spatialized. Houd er rekening mee dat de curve afstand afname van de bron ook wordt toegepast.

### <a name="adjust-reverberation"></a>Weerklank aanpassen
* **Wetness (dB)** -de kracht weerklank in dB worden aangepast op basis van de afstand tussen de bron. Positieve waarden moeten een geluid meer reverberant, terwijl u negatieve waarden van een geluid meer droge. Klik op het besturingselement met de curve (groene lijn) om de curve-editor. Wijzigen van de curve door te klikken met de linkermuisknop om toe te voegen punten en sleept deze punten om de functie die u wilt. De x-as is afstand van de bron en de y-as is weerklank aanpassing in dB. Zie voor meer informatie over het bewerken van curven [Unity handmatig](https://docs.unity3d.com/Manual/EditingCurves.html). Als u wilt opnieuw met het instellen van de curve naar de standaard, klik met de rechtermuisknop op **Wetness** en selecteer **opnieuw**.
* **Verval schaal** -Hiermee past u een vermenigvuldiger voor de vervaltijd. Bijvoorbeeld, als het resultaat bake Hiermee geeft u een Vervaltijd van 750 milliseconden, maar deze waarde is ingesteld op 1.5, is de vervaltijd toegepast op de bron 1,125 milliseconden.
* **Outdoorness** -een-additieve aanpassing op van het systeem akoestische schatting van hoe "buitenshuis' de weerklank op een bron moet geluid. Deze waarde instelt op 1 maakt een bron altijd goed volledig buitenmilieu, terwijl deze wordt ingesteld op -1 wordt een bron geluid volledig binnen.

Bezig met koppelen van de **AcousticsAdjustExperimental** script naar een bron zorgt ervoor dat extra parameters voor experimentele afstemmen voor die bron. Als u wilt koppelen het script, klikt u op **onderdeel toevoegen** aan de onderkant van de **Inspector** deelvenster en vervolgens naar **Scripts > akoestische aanpassen experimentele**. Er is momenteel een experimenteel besturingselement:

![Schermafbeelding van Unity AcousticsAdjustExperimental script](media/acoustics-adjust-experimental.png)

* **Perceptuele afstand verdraaien** -toepassing een exponentiële kromtrekken aan de afstand wordt gebruikt voor het berekenen van de testmodus NAT-verhouding. Het systeem akoestische berekent NAT niveaus in de ruimte die variëren probleemloos met afstand en perceptuele afstand aanwijzingen geven. Dit effect exaggerate afbeeldingsvervormingen waarden groter dan 1 door te verhogen met betrekking tot afstand weerklank niveaus, waardoor het geluid "afstand". Kromtrekken waarden minder dan 1 maken de weerklank op basis van afstand meer subtiele, en het geluid meer "aanwezig' wijzigen.

## <a name="design-acoustics-for-all-sources"></a>Ontwerp akoestische voor alle bronnen
Als u wilt aanpassen parameters voor alle bronnen, klikt u op de strook kanaal in van Unity **Audio Mixer**, en pas de parameters op de **Project akoestische Mixer** effect.

![Schermafbeelding van Project akoestische Unity Mixer aanpassing deelvenster](media/mixer-parameters.png)

* **Wetness aanpassen** -de kracht weerklank in dB worden aangepast voor alle bronnen in de scène op basis van de bron-listener afstand. Positieve waarden moeten een geluid meer reverberant, terwijl u negatieve waarden van een geluid meer droge.
* **Schaal RT60** - Multiplicatieve scalaire weerklank tijd.
* **Schuif gebruiken** -besturingselementen of audio wordt uitgevoerd als binaural (0) of meerdere kanalen pannen (1). Een willekeurige waarde behalve 1 geeft aan binaural. Binaural uitvoer is spatialized met HRTFs voor gebruik met hoofdtelefoon en meerdere kanalen uitvoer is spatialized met VBAP voor gebruik met meerdere kanalen omgeef spreker systemen. Als u met behulp van de Multikanaal panner ervoor dat u selecteert de luidsprekermodus die overeenkomt met uw instellingen voor apparaten, te vinden onder **projectinstellingen** > **Audio**.

## <a name="check-proper-sound-source-placement"></a>Plaatsing van de juiste geluid bron controleren
Geluid bronnen in bezet voxels geplaatst krijgt geen akoestische behandeling. Omdat voxels voorbij de geometrie zichtbaar scène uitbreidt, is het mogelijk om een bron binnen een voxel terwijl deze wordt door visual geometrie unoccluded weergegeven. U kunt Project akoestische voxels weergeven door het omschakelen van het selectievakje van de grid voxel in de **Gizmos** in het menu in de rechterbovenhoek van de **scène** weergeven.

![Schermafbeelding van Unity Gizmos menu](media/gizmos-menu.png)  

De weergave voxel kan ook helpen bepalen of visuele onderdelen in de game een transformatie is toegepast op deze hebt. Als dit het geval is, de dezelfde transformatie toepassen op de GameObject die als host fungeert de **akoestische Manager**.

### <a name="bake-time-vs-run-time-voxels"></a>Tijd versus uitvoeringstijd voxels maken
Het is mogelijk om weer te geven voxels in het editorvenster tijdens het spel ontwerpen en in het spel venster tijdens runtime. De grootte van de voxels is anders in deze weergaven. Dit komt doordat de interpolatie van akoestische runtime maakt gebruik van een betere voxel raster voor soepeler interpolatie resultaten. Plaatsing van geluid bron moet worden geverifieerd met behulp van de runtime-voxels.

Tijd voxels ontwerpen:

![Schermafbeelding van het Project akoestische voxels tijdens het ontwerpen](media/voxels-design-time.png)

Runtime-voxels:

![Schermafbeelding van het Project akoestische voxels tijdens runtime](media/voxels-runtime.png)

## <a name="next-steps"></a>Volgende stappen
* Bekijk casestudy's markeren van de concepten achter de [proces ontwerpen](design-process.md)

