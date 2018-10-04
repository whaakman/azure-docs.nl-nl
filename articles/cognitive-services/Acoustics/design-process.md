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
ms.openlocfilehash: b6bb04d9cec690198de663189dacd41fcbe960eb
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248601"
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

Akoestische uitvoeren berekening in een "regio simulatie" gecentreerd rond de player-locatie. Als een goede bron ver van de speler is, buiten deze regio simulatie beïnvloedt alleen geometrie in het vak de geluid doorgifte (zoals waardoor bedekking) die redelijk goed werkt wanneer occluders zijn in de buurt van de speler. Echter, in gevallen wanneer de speler in de open ruimte is, maar de occluders in de buurt van de verafgelegen geluid bron zijn, het geluid kunt worden onrealistisch disoccluded. Onze voorgestelde tijdelijke oplossing is om in dergelijke gevallen ervoor te zorgen dat het geluid afname uit op 0 bij ongeveer 45m, de standaard horizontale afstand van de speler aan de rand van het vak valt.

### <a name="tuning-scene-parameters"></a>Scène-parameters afstemmen
Als u wilt aanpassen parameters voor alle bronnen, klikt u op de strook kanaal in van Unity **Audio Mixer**, en pas de parameters op de **akoestische Mixer** effect.

![Aanpassing van Mixer](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Bron parameters afstemmen
Bezig met koppelen van de **AcousticsDesign** script naar een gegevensbron kunt afstemmen parameters voor die bron. Als u wilt koppelen het script, klikt u op **onderdeel toevoegen** aan de onderkant van de **Inspector** deelvenster en vervolgens naar **Scripts > akoestische ontwerp**. Het script heeft zes besturingselementen:

![AcousticsDesign](media/AcousticsDesign.png)

* **Bedekking Factor** -een vermenigvuldiger van toepassing op het niveau van de bedekking dB berekend door het systeem akoestische. Als deze vermenigvuldiger groter dan 1 is, bedekking wordt exaggerated, terwijl waarden minder dan 1 maken het effect van bedekking subtielere en een waarde van 0 schakelt bedekking.
* **Overdracht (dB)** -de afname (in de database) veroorzaakt door een overdracht via geometrie instellen. Deze schuifregelaar ingesteld op het laagste niveau om uit te schakelen verzending. Akoestische spatializes de initiële droge audio als binnenkomen rond scène geometrie (portaling). Verzending biedt een extra droge aankomst die in de richting van de regel van zicht is spatialized. Houd er rekening mee dat de curve afstand afname van de bron ook wordt toegepast.
* **Wetness aanpassen (dB)** -de kracht weerklank in dB worden aangepast op basis van de afstand tussen de bron. Positieve waarden moeten een geluid meer reverberant, terwijl u negatieve waarden van een geluid meer droge. Klik op het besturingselement met de curve (groene lijn) om de curve-editor. Wijzigen van de curve door te klikken met de linkermuisknop om toe te voegen punten en sleept deze punten om de functie die u wilt. De x-as is afstand van de bron en de y-as is weerklank aanpassing in dB. Raadpleeg deze [Unity handmatig](https://docs.unity3d.com/Manual/EditingCurves.html) voor meer informatie over het bewerken van curven. Als u wilt opnieuw met het instellen van de curve naar de standaard, klik met de rechtermuisknop op **Wetness aanpassen** en selecteer **opnieuw**.
* **Verval schaal** -Hiermee past u een vermenigvuldiger voor de vervaltijd. Bijvoorbeeld, als het resultaat bake Hiermee geeft u een Vervaltijd van 750 milliseconden, maar deze waarde is ingesteld op 1.5, is de vervaltijd toegepast op de bron 1,125 milliseconden.
* **Inschakelen van akoestische** -bepaalt of akoestische is toegepast op deze gegevensbron. Wanneer dit selectievakje uitschakelt, wordt de bron spatialized met HRTFs, maar zonder akoestische, wat betekent dat zonder obstakel bedekking en dynamische weerklank parameters zoals niveau en decay tijd. Weerklank wordt nog steeds toegepast met een vaste niveau en de vervaltijd.
* **Aanpassing van outdoorness** -een-additieve aanpassing op van het systeem akoestische schatting van hoe "buitenshuis' de weerklank op een bron moet geluid. Deze instelling dan op 1 een bron altijd goed volledig buitenshuis, tijdens het instellen op-1 wordt hierdoor een bron geluid binnenshuis.

Verschillende bronnen mogelijk verschillende instellingen voor een bepaalde effecten fraaie uiterlijk of spelen. Dialoogvenster is een mogelijke voorbeeld. Het menselijke oor is meer attuned naar weerklank in spraak, terwijl dialoogvenster moet vaak worden begrijpelijke voor spelen. U kunt voor dit account zonder dat het dialoogvenster niet-diegetic door over te stappen de **Wetness aanpassen** naar beneden, passen de **perceptuele afstand verdraaien** parameter hieronder wordt beschreven, toe te voegen sommige **Verzending** voor sommige droge audio boost doorgegeven door de muren en/of vermindering van de **bedekking Factor** van 1 tot en met meer geluid binnenkomen via portals hebben.

Bezig met koppelen van de **AcousticsDesignExperimental** script naar een bron zorgt ervoor dat extra parameters voor experimentele afstemmen voor die bron. Als u wilt koppelen het script, klikt u op **onderdeel toevoegen** aan de onderkant van de **Inspector** deelvenster en vervolgens naar **Scripts > akoestische ontwerp experimentele**. Er is momenteel een experimenteel besturingselement:

![AcousticsDesignExperimental](media/AcousticsDesignExperimental.png)

* **Perceptuele afstand verdraaien** -toepassing een exponentiële kromtrekken aan de afstand wordt gebruikt voor het berekenen van de testmodus NAT-verhouding. Het systeem akoestische berekent NAT niveaus in de ruimte die variëren probleemloos met afstand en perceptuele afstand aanwijzingen geven. Kromtrekken waarden die groter zijn dan 1 dit effect exaggerate door te verhogen met betrekking tot afstand weerklank niveaus, zodat de geluid "afstand", terwijl minder dan 1 Controleer de weerklank op basis van een afstand van meer subtiele, waardoor het geluid meer wijziging kromtrekken waarden 'presenteren'.

