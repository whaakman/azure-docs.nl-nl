---
title: Quick start voor project akoestische met Unreal
titlesuffix: Azure Cognitive Services
description: Experimenteer met voorbeeld inhoud met behulp van ontwerp besturings elementen voor project akoestische in Unreal en WWise en implementeer het op Windows Desktop.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: 06023b2758d09fe8ebe7c1301ef1a03d9c54aa41
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704775"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Unreal/WWise Quick start voor project akoestische
In deze Quick start gaat u experimenteren met de ontwerp besturings elementen voor project akoestische, met behulp van de meegeleverde voorbeeld inhoud voor de Unreal-Engine en WWise.

Software vereisten:
* [Unreal-Engine](https://www.unrealengine.com/) 4,21
* [AudioKinetic WWise](https://www.audiokinetic.com/products/wwise/) 2018.1.6

## <a name="download-the-sample-package"></a>Het voorbeeld pakket downloaden
Down load het [Unreal + WWise-voorbeeld pakket voor project akoestische](https://www.microsoft.com/download/details.aspx?id=58090). Het voorbeeld pakket bevat een Unreal-Engine project, het WWise-project voor dat Unreal-project en de-invoeg toepassing voor project akoestische WWise.

## <a name="set-up-the-project-acoustics-sample-project"></a>Het voorbeeld project van het project akoestische instellen
Als u het voorbeeld project van het project akoestische Unreal/WWise wilt instellen, moet u eerst de invoeg toepassing project Akoestisches in WWise installeren. Implementeer vervolgens de binaire bestanden van WWise in het Unreal-project en pas de Unreal-invoeg toepassing van WWise voor de ondersteuning van project akoestische.

### <a name="install-the-project-acoustics-wwise-plugin"></a>De WWise-invoeg toepassing voor project akoestische installeren
Open WWise Launcher en selecteer vervolgens op het tabblad **plugins** onder **nieuwe invoeg toepassingen installeren**de optie **toevoegen uit map**. Kies de `AcousticsWwisePlugin\ProjectAcoustics` map die is opgenomen in het pakket dat u hebt gedownload.

![Scherm opname van WWise Launcher met de optie installatie WWise-invoeg toepassing](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>WWise binaire bestanden toevoegen aan het project akoestische Unreal-voorbeeld project
Klik in WWise Launcher op het tabblad **Unreal Engine** en klik vervolgens op het Hamburger menu naast **recente Unreal-Engine projecten** en selecteer **Bladeren naar project**. Open het voorbeeld project `.uproject` bestand Unreal in het pakket. `AcousticsSample\AcousticsGame\AcousticsGame.uproject`

![Scherm opname van het tabblad WWise Launcher Unreal](media/wwise-unreal-tab.png)

Klik vervolgens naast het voorbeeld project van het project akoestisch op **WWise integreren in Project**.

![Scherm opname van WWise Launcher met akoestische Game Unreal-project](media/wwise-acoustics-game-project.png)

### <a name="extend-wwises-unreal-plugin-functionality"></a>WWise van de Unreal-invoeg toepassing uitbreiden
De Unreal-invoeg toepassing voor project akoestische vereist extra gedrag van de WWise Unreal plugin-API. Voer het batch bestand uit dat is meegeleverd met de Unreal-invoeg toepassing voor project akoestische om deze wijzigingen te automatiseren:
* In `AcousticsGame\Plugins\ProjectAcoustics\Resources`, uitvoeren `PatchWwise.bat`.

    ![Scherm opname van het Windows Verkenner-venster met script voor het patch WWise-project](media/patch-wwise-script.png)

* Als de DirectX SDK niet is geïnstalleerd, is het mogelijk dat u, afhankelijk van de versie van WWise die u gebruikt, een opmerking moet toevoegen aan de `DXSDK_DIR` regel `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`met de volgende opdrachten:

    ![Scherm opname van code-editor met DXSDK-opmerkingen](media/directx-sdk-comment.png)

### <a name="open-the-unreal-project"></a>Open het Unreal-project. 
U wordt gevraagd om de modules opnieuw te bouwen. Klik op Ja.

>Als het project niet kan worden geopend wanneer er fouten zijn opgetreden, controleert u of u de WWise-invoeg toepassing voor project akoestische hebt geïnstalleerd in dezelfde versie van WWise die wordt gebruikt in het voorbeeld project van het project akoestische.

>Als u [AudioKinetic WWise](https://www.audiokinetic.com/products/wwise/) 2018.1.6 niet gebruikt, moet u de geluids banken opnieuw genereren voordat audio in het voorbeeld project wordt afgespeeld.

## <a name="experiment-with-project-acoustics-design-controls"></a>Experimenteer met ontwerp controles voor project akoestische
Luister naar hoe de scène klinkt door te klikken op de knop afspelen in de Unreal-editor. Gebruik op bureau blad W, A, S, D en de muis om door te gaan. Als u sneltoetsen wilt zien voor meer besturingselementen, drukt u op **F1**. Hier volgen enkele ontwerp activiteiten die u kunt proberen:

### <a name="modify-occlusion-and-transmission"></a>Bedekking en overdracht wijzigen
Er zijn het ontwerp besturings elementen per bron project op elke Unreal-geluids Actor:

![Scherm afbeelding van de ontwerp besturings elementen van de Unreal-editor](media/demo-scene-sound-source-design-controls.png)

Als de **bedekking** vermenigvuldiger groter is dan 1 (de standaard waarde is 1), wordt bedekking exaggerated. Als u deze instelling kleiner dan 1 instelt, wordt het bedekking-effect subtieler.

Verplaats de schuif regelaar voor **verzen ding (DB)** naar het laagste niveau om de overdracht via de wand mogelijk te maken. 

### <a name="modify-wetness-for-a-source"></a>Wetness voor een bron wijzigen
Als u wilt wijzigen hoe snel wetness verandert met afstand, gebruikt u de afdraaiing van **Perceptueel op afstand**. Met de geluids sterkte van het project worden natte niveaus in de ruimte van simulaties berekend, wat soepel en op afstand verloopt en de hints op waarneming bieden. Verhoog de afstands verdraaiing exaggerates dit effect door toenemende natte niveaus te verhogen. Het verdraaien van waarden kleiner dan 1 maken de op afstand gebaseerde reverberation subtieler. Dit effect kan ook worden aangepast in gedetailleerde Details door de **Wetness (DB)** aan te passen.

Verhoog de verval tijd in de ruimte door de **schaal van de verval tijd**aan te passen. Houd rekening met de gevallen waarin het simulatie resultaat een verval tijd van 1,5 s is. Als u de schaal van de **verval tijd** instelt op 2, resulteert dit in een verval tijd die wordt toegepast op de bron van 3 s.

### <a name="modify-distance-based-attenuation"></a>Afzwakking op afstand wijzigen
De project akoestische WWise mixer-invoeg toepassing respecteert de op afstand gebaseerde demping per bron op basis van WWise. Als u deze curve wijzigt, wordt het niveau van het droog-pad gewijzigd. Met de invoeg toepassing voor project akoestische wordt het natte niveau aangepast om de natte, droge mix te behouden die is opgegeven door de besturings elementen simulatie en ontwerp.

![Scherm afbeelding van het deel venster WWise-verzwakking met demping naar nul voor simulatie grens](media/demo-sounds-attenuation.png)

Bij een project akoestische wordt de berekening in het vak simulatie regio gecentreerd rond elke gesimuleerde speler locatie. De akoestische activa in het voor beeld van het pakket zijn geïntegreerde met een simulatie van een straal van 45 m en de verzwakkingen waren ontworpen om 0 vóór 45 m te dalen. Hoewel deze wegvalheid geen strikte vereiste is, is dit het voor behoud dat alleen de geometrie binnen 45 m van de listener occlude.

## <a name="next-steps"></a>Volgende stappen
* [De invoeg toepassing project akoestische](unreal-integration.md) in uw Unreal-project integreren
* [Een Azure-account maken](create-azure-account.md) voor uw eigen processen


