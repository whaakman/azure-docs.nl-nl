---
title: Project akoestische snelstartgids met Unity
titlesuffix: Azure Cognitive Services
description: Met behulp van voorbeelden voor inhoud, Project akoestische experimenteren met besturingselementen in de eenheid van het ontwerpen en implementeren op Windows-bureaublad.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/14/2019
ms.author: kegodin
ms.openlocfilehash: b4eedabbc47738eb2f5797ffd67a3e3ebc9529ca
ms.sourcegitcommit: f68b0e128f0478444740172f54e92b453df696be
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58136223"
---
# <a name="project-acoustics-unity-quickstart"></a>Quick Start Project akoestische Unity
Gebruik Project akoestische voorbeeld inhoud voor Unity om te experimenteren met besturingselementen voor ontwerp simulatie van garantie.

Softwarevereisten:
* [Unity 2018.2 +](http://unity3d.com) voor Windows
* [Project akoestische voorbeeld-inhoudspakket](https://www.microsoft.com/en-us/download/details.aspx?id=57346)

Wat inbegrepen in het Voorbeeldpakket?
* Unity-scène met geometrie, gezonde bronnen en spelen besturingselementen
* Project akoestische-invoegtoepassing 
* Voltooide akoestische activa voor de scène

## <a name="import-the-sample-package"></a>De Voorbeeldpakket importeren
De Voorbeeldpakket importeren naar een nieuw Unity-project. 
* In Unity, gaat u naar **activa > pakket importeren > Custom Package...**

    ![Pakket importeren](media/import-package.png)  

* Kies **ProjectAcoustics.unitypackage**

Als u het pakket in een bestaand project importeert, Zie [Unity-integratie](unity-integration.md) voor extra stappen en notities.

## <a name="restart-unity"></a>Opnieuw opstarten van Unity
Het gedeelte bake van de toolkit akoestische vereist de scripting runtime-versie van .NET 4.x. Importeren van het pakket worden de instellingen voor de Unity-speler bijgewerkt. Start opnieuw op de eenheid voor deze instelling van kracht te laten worden.

U kunt controleren of deze instelling van kracht door het openen van de **Player Settings**:

![Instellingen van de speler](media/player-settings.png)

![.NET 4.5](media/net45.png)

## <a name="experiment-with-design-controls"></a>Experimenteer met besturingselementen voor ontwerp
Open de scène voorbeeld in de **ProjectAcousticsSample** map en klik op de afspeelknop in de Unity-editor. Gebruik W, A, S, D- en de muis om te navigeren. Als u wilt vergelijken hoe de scène met en zonder Acoustics klinkt, drukt u op **R** totdat de overlaytekst rood wordt en zegt "Acoustics: Disabled." Als u sneltoetsen wilt zien voor meer besturingselementen, drukt u op **F1**. Besturingselementen zijn ook bruikbaar met de rechtermuisknop op de actie uit te voeren, schakel vervolgens links te klikken op als de bewerking wilt uitvoeren.

Het script **AcousticsAdjust** is gekoppeld aan de geluid bronnen in de scène voorbeeld, waarmee de per-source ontwerpparameters. 

![AcousticsAdjust](media/acoustics-adjust.png)

De volgende verkent enkele van de effecten die kunnen worden gemaakt met de opgegeven besturingselementen. Zie voor gedetailleerde informatie over elk besturingselement, de [Project akoestische Unity ontwerp zelfstudie](unreal-workflow.md).

### <a name="modify-distance-based-attenuation"></a>Afname op basis van afstand wijzigen
De audio DSP geleverd door de **Project akoestische** ruimtelijk-invoegtoepassing voor Unity respecteert de per-source afstand gebaseerde afname ingebouwd in de Unity-Editor. Besturingselementen voor afname op basis van afstand zijn de **Audio bron** onderdeel gevonden de **Inspector** Configuratiescherm van geluid gegevensbronnen onder **3D-geluidsinstellingen**:

![Afstand afname](media/distance-attenuation.png)

Project akoestische uitvoeren berekening in een "regio simulatie" gecentreerd rond de player-locatie. Omdat de activa akoestische in het Voorbeeldpakket met een grootte van de regio simulatie van 45m rond de speler sparen zijn, moet de geluid afname worden ontworpen om aan te vallen op 0 bij ongeveer 45 m.

### <a name="modify-occlusion-and-transmission"></a>Wijzigen van bedekking en -overdracht
* Als de **bedekking** vermenigvuldiger is groter dan 1 (de standaardwaarde is 1), bedekking wordt exaggerated. Het instellen van deze minder dan 1 wordt bedekking gevolgen subtielere.

* Als u wilt inschakelen via de wand verzending, gaan de **verzending (dB)** schuifregelaar uit het laagste niveau. 

### <a name="modify-wetness-for-a-source"></a>Wetness voor een gegevensbron wijzigen
* U kunt wijzigen hoe snel wetness wordt gewijzigd met afstand met de **perceptuele afstand verdraaien**. **Akoestische project** berekeningen NAT niveaus in de ruimte van de simulatie, die variëren probleemloos met afstand en perceptuele afstand aanwijzingen geven. De afstand verdraaien verhogen verhoogt dit effect door te verhogen met betrekking tot afstand NAT niveaus. Controleer de afstand gebaseerde weerklank wijzigen subtielere afbeeldingsvervormingen waarden kleiner dan 1. Dit effect kan ook worden aangepast in detail nauwkeurigere door aan te passen de **Wetness (dB)**.

* De vervaltijd in de ruimte verhogen door aan te passen de **Decay schaal**. Als het simulatieresultaat voor een bepaalde bron-listener locatie paar een Vervaltijd van 1.5s, en de **Decay schaal** is ingesteld op 2, is de vervaltijd toegepast op de bron 3 's.

## <a name="next-steps"></a>Volgende stappen
* Volledige details lezen op de [op basis van een Unity-Project akoestische besturingselementen ontwerpen](unity-workflow.md)
* Meer informatie de concepten achter de [proces ontwerpen](design-process.md)
* [Maak een Azure-account](create-azure-account.md) verkennen van de vooraf bake en maken van processen

