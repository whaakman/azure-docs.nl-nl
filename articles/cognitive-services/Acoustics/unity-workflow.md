---
title: Zelf studie over het ontwerpen van een project-unit-eenheid
titlesuffix: Azure Cognitive Services
description: In deze zelf studie wordt de ontwerp werk stroom voor project akoestische in eenheid beschreven.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: fd00e4105ce4edae9d014df2a83c5ae3aaf778da
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854265"
---
# <a name="project-acoustics-unity-design-tutorial"></a>Zelf studie over het ontwerpen van een project-unit-eenheid
In deze zelf studie worden de ontwerp hulpprogramma's en werk stroom voor project akoestische in eenheid beschreven.

Vereisten:
* Unity 2018.2 + voor Windows
* Een eenheids scène met een geïntegreerde akoestische activa

Voor deze zelf studie kunt u op twee manieren een eenheids scène met een geïntegreerde geluids activum verkrijgen:
* [Voeg het project toe aan uw eenheids project](unity-integration.md), ga vervolgens [een Azure batch account](create-azure-account.md)en [maken uw eenheids scène](unity-baking.md)
* U kunt ook de [voorbeeld inhoud van de project akoestische-eenheid](unity-quickstart.md)gebruiken.

## <a name="review-design-process-concepts"></a>Concepten van het ontwerp proces controleren
In Project akoestische worden de DSP-methoden (common audio Digital signalering processing) gebruikt voor het verwerken van uw bronnen en hebt u de controle over bekende akoestische eigenschappen, waaronder bedekking, natte/droge mix, en reverberation staart lengte (RT60). Maar het [concept ontwerp proces](design-process.md) van het belangrijkste project is dat in plaats van deze eigenschappen rechtstreeks in te stellen, u kunt bepalen hoe simulatie resultaten worden gebruikt om deze eigenschappen te best uren. De standaard instellingen voor elk besturings element vertegenwoordigen fysiek nauw keurige akoestische.

## <a name="design-acoustics-for-each-source"></a>Ontwerp akoestische ontwerpen voor elke bron
De geluids elementen van het project bieden een aantal apparaatspecifieke ontwerpen voor akoestische ontwerp. Zo kunt u de mix in een scène beheren door bepaalde bronnen te benadrukken en anderen te benadrukken.

### <a name="adjust-distance-based-attenuation"></a>Afzwakking op afstand aanpassen
De audio-DSP die wordt verschaft door de spatializer-invoeg toepassing voor de **project geluids** eenheid, respecteert de op afstand gebaseerde verzwakking die is ingebouwd in de Unity-editor. Besturings elementen voor verzwakking op afstand bevinden zich in het **audio bron** onderdeel dat is gevonden in het deel venster **Inspector** van geluids bronnen, onder **3D-geluids instellingen**:

![Scherm afbeelding van het deel venster Opties voor afstands verzwakking](media/distance-attenuation.png)

Akoestische handelingen voeren een berekening uit in het vak simulatie regio, gecentreerd rond de locatie van de speler. Als een geluids bron zich buiten deze simulatie regio bevindt, heeft alleen de geometrie in het vak invloed op de geluids doorgifte (zoals bedekking) die redelijkerwijs goed werkt wanneer occluders zich in de buurt van de speler bevinden. In het geval dat de speler zich in open ruimte bevindt, maar de occluders zich in de buurt van de geluids bron van de afstand bevinden, kan het geluid onrealistisch worden disoccluded. Onze voorgestelde tijdelijke oplossing is om ervoor te zorgen dat de geluids verzwakking op 0 bij ongeveer 45 m is uitgeschakeld, de horizontale afstand van de speler tot aan de rand van het vak.

![Scherm opname van het SpeakerMode-optie paneel](media/speaker-mode.png)

### <a name="adjust-occlusion-and-transmission"></a>Bedekking en overdracht aanpassen
Als u het **AcousticsAdjust** -script aan een bron koppelt, worden afstemmings parameters voor die bron ingeschakeld. Als u het script wilt koppelen, klikt u op **onderdeel toevoegen** onder aan het deel venster **Inspector** en navigeert u naar **scripts > akoestische aanpassingen**. Het script heeft zes besturings elementen:

![Scherm opname van Unity AcousticsAdjust-script](media/acoustics-adjust.png)

* **Akoestische functies inschakelen** : Hiermee bepaalt u of er geluids elementen worden toegepast op deze bron. Als u dit selectie vakje uitschakelt, wordt de bron ruimtelijk met HRTFs of pannen, maar er is geen akoestische. Dit betekent dat er geen obstakel-, bedekking-of dynamische reverberation-para meters zoals niveau en verval tijd. Reverberation wordt nog steeds toegepast met een vast niveau en een verval tijd.
* **Bedekking** : een vermenigvuldiger Toep assen op het niveau van de bedekking DB berekend door het akoestische systeem. Als deze vermenigvuldiger groter is dan 1, wordt bedekking exaggerated, terwijl waarden kleiner dan 1 het bedekking effect op subtieler maken en met een waarde van 0 bedekking wordt uitgeschakeld.
* **Trans Mission (DB)** : Stel de verzwakking (in dB) in die door trans missie via geometrie wordt veroorzaakt. Stel deze schuif regelaar in op het laagste niveau om de verzen ding uit te scha kelen. Akoestische spatializes de eerste droge audio als een scène geometrie (Portal). Verzen ding biedt een extra droge aankomst die in de richting van het gezichts vermogen wordt ruimtelijk. Houd er rekening mee dat de curve voor het afzwakken van afstand voor de bron ook wordt toegepast.

### <a name="adjust-reverberation"></a>Reverberation aanpassen
* **Wetness (DB)** : Hiermee past u de galm kracht in dB aan op basis van de afstand van de bron. Positieve waarden maken een geluid meer reverberant, terwijl negatieve waarden een geluid droger maken. Klik op het besturings element curve (groene lijn) om de curve-editor weer te geven. Wijzig de curve door met de rechter muisknop te klikken om punten toe te voegen en deze punten te slepen om de gewenste functie te maken. De x-as is de afstand tussen de bron en de y-as is aanpassing van galm in dB. Zie deze [Unit-hand leiding](https://docs.unity3d.com/Manual/EditingCurves.html)voor meer informatie over het bewerken van curven. Klik met de rechter muisknop op **Wetness** en selecteer **opnieuw instellen**om de curve terug te zetten naar de standaard waarde.
* **Schaal van de verval tijd** : Hiermee wordt een vermenigvuldiger voor de verval tijd aangepast. Als het resultaat van de maken bijvoorbeeld een verval tijd van 750 milliseconden aangeeft, maar deze waarde is ingesteld op 1,5, is de verval tijd die wordt toegepast op de bron 1.125 milliseconden.
* **Buitenshuis** : een additief aanpassing aan de soliditeit van het akoestische systeem van de manier waarop de reverberation van een bron moet worden geschat. Als u deze waarde instelt op 1, wordt een bron altijd volledig geklankd en wordt deze ingesteld op-1 en wordt een bron geluid helemaal niet in de lucht.

Als u het **AcousticsAdjustExperimental** -script aan een bron koppelt, worden extra experimentele afstemmings parameters voor die bron ingeschakeld. Als u het script wilt koppelen, klikt u op **onderdeel toevoegen** onder aan het deel venster **Inspector** en navigeert u naar **scripts > akoestische het experiment**aan te passen. Er bevindt zich momenteel één experimenteel besturings element:

![Scherm opname van Unity AcousticsAdjustExperimental-script](media/acoustics-adjust-experimental.png)

* **Perceptueel, afstand kromie** : pas een exponentiële verdraaiing toe op de afstand die wordt gebruikt om de droge verhouding te berekenen. Het akoestische systeem berekent de natte niveaus gedurende de gehele ruimte, wat soepel en op afstand afwijkt van waarnemingen op afstand. Het verdraaien van waarden die groter zijn dan 1 exaggerate dit effect door de reverberation op afstand te verhogen, waardoor het geluid ' afstand ' wordt. Als u waarden kleiner dan 1 maakt, wordt de op afstand gebaseerde reverberation subtieler gewijzigd, waardoor het geluid ' Prest '.

## <a name="design-acoustics-for-all-sources"></a>Ontwerp akoestische ontwerpen voor alle bronnen
Als u de para meters voor alle bronnen wilt aanpassen, klikt u op de kanaal strook in de **audio mixer**van Unity en past u de para meters aan op het mixer effect van het **project akoestische** .

![Scherm opname van het deel venster voor het aanpassen van het mixer voor project akoestische unit](media/mixer-parameters.png)

* **Wetness aanpassen** : Hiermee past u de galm kracht in dB aan voor alle bronnen in de scène op basis van de afstand van de bron-listener. Positieve waarden maken een geluid meer reverberant, terwijl negatieve waarden een geluid droger maken.
* **RT60 Scale** -multiplicative scalair voor galm tijd.
* **Gebruik pannen om** te bepalen of audio wordt uitgevoerd als Binaural (0) of meerkanaalse panning (1). Een waarde van meer dan 1 geeft aan binaural. De binaural-uitvoer wordt ruimtelijk met HRTFs voor gebruik met een hoofd telefoon en meerkanaals uitvoer is ruimtelijk met VBAP voor gebruik met een meerkanaals surround-luidspreker systeem. Als u de multi kanaal-panner gebruikt, moet u ervoor zorgen dat u de luidspreker modus selecteert die overeenkomt met de apparaatinstellingen, die u vindt onder **project instellingen** > **Audio**.

## <a name="check-proper-sound-source-placement"></a>Controleer de juiste positie van de geluids bron
Geluids bronnen die in de bezette voxels worden geplaatst, krijgen geen akoestische behandeling. Omdat voxels voorbij de zicht bare scène geometrie breidt, is het mogelijk om een bron in een Voxel te plaatsen terwijl deze unoccluded wordt weer gegeven op visuele geometrie. U kunt de voxels van het project weer geven door het selectie vakje Voxel raster in het menu **Gizmos** in de rechter bovenhoek van de **scène** weergave in te scha kelen.

![Scherm opname van het Gizmos-menu](media/gizmos-menu.png)  

De Voxel-weer gave kan ook helpen bepalen of er op visuele onderdelen in het spel een trans formatie wordt toegepast. Als dit het geval is, past u dezelfde trans formatie toe op de GameObject die als host fungeert voor de **akoestische Manager**.

### <a name="bake-time-vs-run-time-voxels"></a>Maken tijd versus uitvoerings tijd voxels
Het is mogelijk om voxels in het venster Editor weer te geven tijdens de ontwerp fase en in het spel venster tijdens de uitvoering. De grootte van de voxels verschilt in deze weer gaven. Dit komt doordat de interpolatie van de akoestische runtime een fijn Voxel raster gebruikt voor vloeiende interpolatie resultaten. De plaatsing van de geluids bron moet worden gecontroleerd met behulp van de runtime-voxels.

Ontwerp tijd voxels:

![Scherm afbeelding van akoestische voxels tijdens de ontwerp fase](media/voxels-design-time.png)

Runtime-voxels:

![Scherm afbeelding van akoestische voxels tijdens runtime](media/voxels-runtime.png)

## <a name="next-steps"></a>Volgende stappen
* Bekijk Case-study's die de concepten achter het [ontwerp proces](design-process.md) markeren

