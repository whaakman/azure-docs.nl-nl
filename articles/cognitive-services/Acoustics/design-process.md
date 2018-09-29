---
title: Overzicht van het ontwerp voor akoestische - Cognitive Services
description: Dit document wordt beschreven hoe u de bedoeling van uw ontwerp in alle drie fasen van de werkstroom Project akoestische express.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 8f594be67c4677fae00cb01598d3899e30dae1e8
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433221"
---
# <a name="design-process-overview"></a>Overzicht van het ontwerp
U kunt de bedoeling van uw ontwerp in alle drie fasen van de werkstroom Project akoestische express: vooraf verdient scène setup, gezonde bron plaatsing en na bake ontwerp. Het proces vereist minder markeringen die zijn gekoppeld aan volumes weerklank behoudt designer bepalen hoe een scène geluiden plaatsen.

## <a name="pre-bake-design"></a>Ontwerp vooraf maken
Het installatieproces vooraf bake scène produceert de scène en metagegevens die worden gebruikt voor de simulatie geluid wave, waaronder te selecteren welke elementen van de scène wordt opgenomen in de simulatie occlusions, weerspiegeling en weerklank opgeven. De metagegevens van de scène is de selectie van akoestische materialen voor elk element in de scène. Het akoestisch materiaal beheren geluid energieverbruik weergegeven van elke surface.

De standaard opname coëfficiënt voor alle oppervlakken is 0,04, sterk reflecteert. U kunt fraaie uiterlijk en spelen effecten bereiken door het afstemmen van de coëfficiënten voor opname van verschillende materialen in de scène, die met name belangrijk naar listeners zijn wanneer ze de overgang van het ene gebied van de scène naar een andere horen. Bijvoorbeeld, verbetert overstappen van een donkere reverberant kamer naar een heldere, niet-reverberant outdoor scène de impact van de overgang. Als u wilt dit resultaat te bereiken, de coëfficiënten voor opname in het materiaal outdoor scène hogere af te stemmen.

De tijd weerklank van een bepaald materiaal in een kamer is omgekeerd gerelateerd aan de coëfficiënt opname met de meeste materiaal met waarden voor opname in het bereik 0,01-0.20. Materialen met opname coëfficiënten buiten dit bereik zijn zeer absorberend.

![Het diagram met de weerklank](media/ReverbTimeGraph.png)

De [bake UI doorlopen](bake-ui-walkthrough.md) worden de vooraf bake besturingselementen in detail beschreven.

## <a name="sound-source-placement"></a>Plaatsing van geluid bron
Voxels en test weergeven tijdens runtime kan helpen bij het opsporen van problemen met geluid bronnen binnen de geometrie voxelized wordt vastgelopen. Het raster voxel en test met punten weergegeven, klikt u op het bijbehorende selectievakje in het menu Gizmos in de rechterbovenhoek van de scène-weergave. Als de bron-geluid is binnen een muur voxel, kunt u deze in een voxel lucht verplaatsen.

![Gizmos Menu](media/GizmosMenu.png)  

De weergave voxel kunt u eenvoudiger bepalen als visuele onderdelen in de game een transformatie is toegepast om ze te hebben. Als dit het geval is, de dezelfde transformatie toepassen op de GameObject die als host fungeert de **akoestische Manager**.

## <a name="post-bake-design"></a>Na het maken van ontwerp
Bake resultaten worden opgeslagen in het bestand ACE als bedekking en weerklank parameters voor alle bron-listener locatie sets in de scène. Deze fysiek accuraat resultaat kan worden gebruikt voor het project als- en een goed startpunt voor het ontwerpen van is. Het ontwerpproces voor na bake Hiermee geeft u regels voor het transformeren van de parameters van het resultaat bake tijdens runtime.

### <a name="distance-based-attenuation"></a>Op basis van afstand afname
De audio DSP geleverd door de **Microsoft Acoustics** ruimtelijk-invoegtoepassing voor Unity respecteert de per-source afstand gebaseerde afname ingebouwd in de Unity-Editor. Besturingselementen voor afname op basis van afstand zijn de **Audio bron** onderdeel gevonden de **Inspector** Configuratiescherm van geluid gegevensbronnen onder **3D-geluidsinstellingen**:

![Afstand afname](media/distanceattenuation.png)

### <a name="tuning-scene-parameters"></a>Scène-parameters afstemmen
Als u wilt aanpassen parameters voor alle bronnen, klikt u op de strook kanaal in van Unity **Audio Mixer**, en pas de parameters op de **akoestische Mixer** effect.

![Aanpassing van Mixer](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Bron parameters afstemmen
Bezig met koppelen van de **AcousticsSourceCustomization** script naar een gegevensbron kunt afstemmen parameters voor die bron. Als u wilt koppelen het script, klikt u op **onderdeel toevoegen** aan de onderkant van de **Inspector** deelvenster en vervolgens naar **Scripts > akoestische bron aanpassing**. Het script heeft drie parameters:

![Aanpassing van bron](media/SourceCustomization.png)

* **Weerklank Power aanpassen** -Hiermee past u de kracht weerklank in dB. Positieve waarden moeten een geluid meer reverberant, terwijl u negatieve waarden van een geluid meer droge.
* **Verval schaal** -Hiermee past u een vermenigvuldiger voor de vervaltijd. Bijvoorbeeld, als het resultaat bake Hiermee geeft u een Vervaltijd van 750 milliseconden, maar deze waarde is ingesteld op 1.5, is de vervaltijd toegepast op de bron 1,125 milliseconden.
* **Inschakelen van akoestische** -bepaalt of akoestische is toegepast op deze gegevensbron. Wanneer dit selectievakje uitschakelt, wordt de bron spatialized met HRTFs, maar zonder akoestische, wat betekent dat zonder obstakel bedekking en dynamische weerklank parameters zoals niveau en decay tijd. Weerklank wordt nog steeds toegepast met een vaste niveau en de vervaltijd.

Verschillende bronnen mogelijk verschillende instellingen voor een bepaalde effecten fraaie uiterlijk of spelen. Dialoogvenster is een mogelijke voorbeeld. Het menselijke oor is meer attuned naar weerklank in spraak, terwijl dialoogvenster moet vaak worden begrijpelijke voor spelen. U kunt voor dit account zonder dat het dialoogvenster niet-diegetic door de kracht van weerklank omlaag aan te passen.
