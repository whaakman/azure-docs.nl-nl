---
title: Snelstartgids voor project akoestische
titlesuffix: Azure Cognitive Services
description: Experimenteer met voorbeeld inhoud door de ontwerp besturings elementen voor project akoestische in eenheid te zetten en te implementeren op Windows Desktop.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: a36a16b6cb64c300647d16695edfbb73f4884104
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854867"
---
# <a name="project-acoustics-unity-quickstart"></a>Snelstartgids voor project akoestische eenheid
Gebruik de voorbeeld inhoud van de project akoestische voor eenheid om te experimenteren met ontwerp besturings elementen met simulatie-ondersteuning.

Software vereisten:
* [Unity 2018.2 +](https://unity3d.com) voor Windows
* [Pakket met voorbeeld inhoud voor project akoestische](https://www.microsoft.com/download/details.aspx?id=57346)

Wat is opgenomen in het voorbeeld pakket?
* Eenheids scène met geometrie, geluids bronnen en gameplay-besturings elementen
* Invoeg toepassing voor project akoestische 
* Geïntegreerde akoestische activa voor de scène

## <a name="import-the-sample-package"></a>Het voorbeeld pakket importeren
Importeer het voorbeeld pakket naar een nieuw Unity-project. 
* Ga in eenheid naar **activa > import pakket > aangepast pakket...**

    ![Scherm opname van unit import pakket opties](media/import-package.png)  

* Kies **ProjectAcoustics. unitypackage**

Zie [Unit-integratie](unity-integration.md) voor aanvullende stappen en notities als u het pakket importeert in een bestaand project.

## <a name="restart-unity"></a>Eenheid opnieuw opstarten
Het maken-gedeelte van de akoestische Toolkit vereist de .NET 4. x Scripting runtime-versie. Bij het importeren van pakketten worden de instellingen voor Unity Player bijgewerkt. De eenheid moet opnieuw worden opgestart om deze instelling van kracht te laten worden.

U kunt controleren of deze instelling is doorgevoerd door de instellingen van de **speler**te openen:

![Scherm afbeelding van het deel venster instellingen voor Unity Player](media/player-settings.png)

![Scherm afbeelding van het deel venster instellingen voor Unity Player met .NET 4,5 geselecteerd](media/net45.png)

## <a name="experiment-with-design-controls"></a>Experimenteren met ontwerp besturings elementen
Open de voorbeeld scène in de map **ProjectAcousticsSample** en klik op de afspeel knop in de Unity-editor. Gebruik W, A, S, D en de muis om door te gaan. Als u wilt vergelijken hoe de scène met en zonder Acoustics klinkt, drukt u op **R** totdat de overlaytekst rood wordt en zegt "Acoustics: Disabled." Als u sneltoetsen wilt zien voor meer besturingselementen, drukt u op **F1**. Besturings elementen zijn ook bruikbaar door met de rechter muisknop te klikken om de actie te selecteren die u wilt uitvoeren en vervolgens te klikken met de linkermuisknop om de actie uit te voeren.

Het script **AcousticsAdjust** wordt gekoppeld aan de geluids bronnen in de voorbeeld scène, waardoor de ontwerp parameters per bron worden ingeschakeld. 

![Scherm opname van Unity AcousticsAdjust-script](media/acoustics-adjust.png)

Hieronder vindt u enkele van de effecten die kunnen worden geproduceerd met de beschik bare besturings elementen. Voor gedetailleerde informatie over elk besturings element, raadpleegt u de zelf studie over het ontwerpen van de [project akoestische unit-ontwerp](unreal-workflow.md).

### <a name="modify-distance-based-attenuation"></a>Afzwakking op afstand wijzigen
De audio-DSP die wordt verschaft door de spatializer-invoeg toepassing voor de **project geluids** eenheid, respecteert de op afstand gebaseerde verzwakking die is ingebouwd in de Unity-editor. Besturings elementen voor verzwakking op afstand bevinden zich in het **audio bron** onderdeel dat is gevonden in het deel venster **Inspector** van geluids bronnen, onder **3D-geluids instellingen**:

![Scherm afbeelding van het deel venster Opties voor afstands verzwakking](media/distance-attenuation.png)

Project akoestische voert een berekening uit in het vak simulatie regio, gecentreerd rond de locatie van de speler. Omdat de akoestische activa in het voorbeeld pakket zijn geïntegreerde met een simulatie regio grootte van 45m rondom de speler, moet de geluids afzwakking worden ontworpen om op ongeveer 45 m te vallen.

### <a name="modify-occlusion-and-transmission"></a>Bedekking en overdracht wijzigen
* Als de **bedekking** vermenigvuldiger groter is dan 1 (de standaard waarde is 1), wordt bedekking exaggerated. Als u deze instelling kleiner dan 1 instelt, wordt het bedekking-effect subtieler.

* Verplaats de schuif regelaar voor **verzen ding (DB)** naar het laagste niveau om de overdracht via de wand mogelijk te maken. 

### <a name="modify-wetness-for-a-source"></a>Wetness voor een bron wijzigen
* Als u wilt wijzigen hoe snel wetness verandert met afstand, gebruikt u de afdraaiing van **Perceptueel op afstand**. Met de geluids sterkte van het **project** worden natte niveaus in de ruimte van simulaties berekend, wat soepel en op afstand verloopt en de hints op waarneming bieden. Verhoog de afstands verdraaiing exaggerates dit effect door toenemende natte niveaus te verhogen. Het verdraaien van waarden kleiner dan 1 maken de op afstand gebaseerde reverberation subtieler. Dit effect kan ook worden aangepast in gedetailleerde Details door de **Wetness (DB)** aan te passen.

* Verhoog de verval tijd in de ruimte door de schaal voor de **verval tijd**aan te passen. Als het simulatie resultaat voor een bepaald locatie paar van de bron-listener een verval tijd van 1,5 s is en de **schaal voor de verval tijd** is ingesteld op 2, wordt de verval tijd die wordt toegepast op de bron 3s.

## <a name="next-steps"></a>Volgende stappen
* Lees de volledige informatie over de [ontwerp besturings elementen op basis van op eenheid gebaseerde project akoestische](unity-workflow.md)
* Bekijk de concepten verder achter het [ontwerp proces](design-process.md)
* [Een Azure-account maken](create-azure-account.md) om de maken-en maken-processen te verkennen

