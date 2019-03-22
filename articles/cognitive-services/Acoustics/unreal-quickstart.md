---
title: Project akoestische snelstartgids met Unreal
titlesuffix: Azure Cognitive Services
description: Met behulp van voorbeelden voor inhoud, Project akoestische experimenteren met besturingselementen in Unreal en Wwise ontwerpen en implementeren op Windows-bureaublad.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 1575c4f4a1c96a84823f76e8e98e76de3c2ace86
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313019"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Project akoestische Unreal/Wwise snelstartgids
In deze Quick Start, gaat u met Project akoestische ontwerp besturingselementen met behulp van de opgegeven voorbeelden voor inhoud voor de Unreal Engine en Wwise experimenteren.

Softwarevereisten:
* [Unreal Engine 4.21](https://www.unrealengine.com/)
* [Wwise 2018.1.6](https://www.audiokinetic.com/products/wwise/)

## <a name="download-the-sample-package"></a>Van het Voorbeeldpakket downloaden
Download de [Project akoestische Unreal + Wwise voorbeeld van een pakket](https://www.microsoft.com/download/details.aspx?id=58090). De Voorbeeldpakket bevat een project Unreal Engine, het project Wwise voor dat Unreal project en de invoegtoepassing Project akoestische Wwise.

## <a name="set-up-the-project-acoustics-sample-project"></a>Het voorbeeldproject akoestische Project instellen
Als u het voorbeeldproject Project akoestische Unreal/Wwise instelt, moet u eerst de Project-akoestische-invoegtoepassing installeren in Wwise. Vervolgens de Wwise binaire bestanden aan het project Unreal implementeren en aanpassen van de Wwise Unreal invoegtoepassing ter ondersteuning van akoestische Project.

### <a name="install-the-project-acoustics-wwise-plugin"></a>De Project akoestische Wwise-invoegtoepassing installeren
Wwise starten, klikt u vervolgens Open in de **invoegtoepassingen** tabblad onder **nieuwe invoegtoepassingen installeren**, selecteer **uit map toevoegen**. Kies de `AcousticsWwisePlugin\ProjectAcoustics` directory die is opgenomen in het pakket dat u hebt gedownload.

![Schermafbeelding van Wwise starten met de optie Wwise-invoegtoepassing installeren](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Wwise binaire bestanden toevoegen aan het Project akoestische Unreal voorbeeldproject
Vanuit Wwise starten, klikt u op de **Unreal Engine** tabblad en klik vervolgens op het hamburgermenu naast **recente projecten van Unreal Engine** en selecteer **bladeren voor project**. Open het voorbeeldproject Unreal `.uproject` bestand in het pakket `AcousticsSample\AcousticsGame\AcousticsGame.uproject`.

![Schermafbeelding van Wwise starten Unreal tabblad](media/wwise-unreal-tab.png)

Klik vervolgens naast het voorbeeldproject Project akoestische op **Wwise integreren in een Project**.

![Schermafbeelding van Wwise starten akoestische Game Unreal Project weergeven](media/wwise-acoustics-game-project.png)

### <a name="extend-wwises-unreal-plugin-functionality"></a>De Wwise Unreal invoegtoepassing functionaliteit uitbreiden
De Project akoestische Unreal invoegtoepassing vereist extra gedrag van de invoegtoepassing Wwise Unreal API worden blootgesteld. Voer de batch-bestand dat is opgegeven met de Project akoestische Unreal-invoegtoepassing voor het automatiseren van deze wijzigingen:
* Binnen `AcousticsGame\Plugins\ProjectAcoustics\Resources`Voer `PatchWwise.bat`.

    ![Schermopname van Windows Explorer-venster met script patch Wwise project](media/patch-wwise-script.png)

* Als u geen de DirectX-SDK geïnstalleerd, moet u de regel met DXSDK_DIR in commentaar `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`

    ![Schermafbeelding van code-editor DXSDK als commentaar weergeven](media/directx-sdk-comment.png)

### <a name="open-the-unreal-project"></a>Open het Unreal Project. 
U kunt het opnieuw opbouwen van modules, wordt u gevraagd Klik op Ja.

Als het openen van het project op versiefouten mislukt, controleert u dat u de invoegtoepassing Project akoestische Wwise naar dezelfde versie van Wwise gebruikt in het Project akoestische-voorbeeldproject hebt geïnstalleerd.

## <a name="experiment-with-project-acoustics-design-controls"></a>Experimenteer met Project akoestische ontwerp besturingselementen
Luister naar hoe de scène geluiden door te klikken op de knop afspelen in de Unreal editor. Gebruik op het bureaublad, W, A, S, D en de muis om te navigeren. Als u sneltoetsen wilt zien voor meer besturingselementen, drukt u op **F1**. Hier volgen enkele ontwerp activiteiten om te proberen:

### <a name="modify-occlusion-and-transmission"></a>Wijzigen van bedekking en -overdracht
Er zijn per-source Project akoestische ontwerp besturingselementen op elke Unreal, gezonde actor:

![Schermopname van Editor Unreal akoestische ontwerp besturingselementen](media/demo-scene-sound-source-design-controls.png)

Als de **bedekking** vermenigvuldiger is groter dan 1 (de standaardwaarde is 1), bedekking wordt exaggerated. Het instellen van deze minder dan 1 wordt bedekking gevolgen subtielere.

Als u wilt inschakelen via de wand verzending, gaan de **verzending (dB)** schuifregelaar uit het laagste niveau. 

### <a name="modify-wetness-for-a-source"></a>Wetness voor een gegevensbron wijzigen
U kunt wijzigen hoe snel wetness wordt gewijzigd met afstand met de **perceptuele afstand verdraaien**. Project akoestische berekent NAT niveaus in de ruimte van de simulatie, die variëren probleemloos met afstand en perceptuele afstand aanwijzingen geven. De afstand verdraaien verhogen verhoogt dit effect door te verhogen met betrekking tot afstand NAT niveaus. Controleer de afstand gebaseerde weerklank wijzigen subtielere afbeeldingsvervormingen waarden kleiner dan 1. Dit effect kan ook worden aangepast in detail nauwkeurigere door aan te passen de **Wetness (dB)**.

De vervaltijd in de ruimte verhogen door aan te passen **Decay schaal**. Neem het geval wanneer het simulatieresultaat een Vervaltijd van 1,5 is s. Instellen van de **Decay schaal** 2 resulteert in een vervaltijd toegepast op de bron van 3 s.

### <a name="modify-distance-based-attenuation"></a>Afname op basis van afstand wijzigen
De Project akoestische Wwise mixer-invoegtoepassing respecteert de per bron op basis van afstand afname in Wwise ingebouwd. Wijzigen van deze curve, verandert het droge-path-niveau. De invoegtoepassing Project akoestische wordt het niveau van de NAT voor het onderhouden van de combinatie van de NAT-droge opgegeven door de simulatie en ontwerp besturingselementen aanpassen.

![Schermafbeelding van Wwise afname curve deelvenster met afname op nul voordat simulatie grens gaan](media/demo-sounds-attenuation.png)

Project akoestische doet berekening in een "regio simulatie" gecentreerd rond de locatie van elke gesimuleerde player. De akoestische activa in het Voorbeeldpakket met een simulatie regio radius 45 m zijn sparen en de attenuations zijn ontworpen om aan te vallen op 0 voordat 45 m. Terwijl deze wegvallen niet strikt vereist, wordt deze de voorbehoud dat alleen geometrie binnen 45 m van de listener geluiden wordt occlude.

## <a name="next-steps"></a>Volgende stappen
* [De Project-akoestische integreren](unreal-integration.md) invoegtoepassing naar uw Unreal project
* [Een Azure-account maken](create-azure-account.md) voor uw eigen processen


