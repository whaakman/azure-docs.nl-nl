---
title: Project akoestische Unreal ontwerp zelfstudie
titlesuffix: Azure Cognitive Services
description: In deze zelfstudie worden de ontwerp-werkstroom voor Project akoestische in Unreal en Wwise beschreven.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 57bde67ac2259b3847f59f95eaefba9c6fddf13e
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316198"
---
# <a name="project-acoustics-unrealwwise-design-tutorial"></a>Project akoestische Unreal/Wwise ontwerp zelfstudie
In deze zelfstudie worden de ontwerp-installatie en de werkstroom voor Project akoestische in Unreal en Wwise beschreven.

Vereiste software:
* Een Unreal-project met de Project akoestische Wwise en Unreal-invoegtoepassingen

Als u een Unreal project met Project akoestische, kunt u het volgende doen:
* Ga als volgt de [Project akoestische Unreal integratie](unreal-integration.md) instructies voor het Project akoestische toevoegen aan uw Unreal project
* Of gebruik de [Project akoestische voorbeeldproject](unreal-quickstart.md).

## <a name="setup-project-wide-wwise-properties"></a>Brede, door het project Wwise eigenschappen instellen
Wwise heeft globale obstakel en bedekking curven die invloed hebben op hoe de invoegtoepassing Project akoestische het Wwise audio DSP-stations.

### <a name="design-wwise-occlusion-curves"></a>Wwise bedekking curven ontwerpen
Wanneer het Project akoestische actief is er wordt gereageerd op het volume bedekking, lage-doorlaten (LPF) en hoge filter (HPF) curven in Wwise instellen. Het is raadzaam om het instellen van uw volume curve-type naar Lineair met een waarde van de dB-100 voor een bedekking-waarde van 100.

Met deze instelling als de simulatie Project akoestische een bedekking van minimaal-18-dB berekent deze invoer op voor de curve op X = 18, en de bijbehorende Y-waarde is de afname toegepast. Hiertoe halve bedekking, moet u het eindpunt instellen op dB in plaats van-100-dB-50, of in dB-200 exaggerate bedekking. U kunt aanpassen en verfijnen van een curve die geschikt is voor uw game.
 
![Schermafbeelding van Wwise bedekking curve-editor](media/wwise-occlusion-curve.png)

### <a name="disable-wwise-obstruction-curves"></a>Wwise obstakel curven uitschakelen
De Wwise obstakel curven invloed hebben op het niveau van de droge geïsoleerd maar Project akoestische maakt gebruik van besturingselementen voor ontwerp en simulatie om af te dwingen NAT/droge ratio's. U wordt aangeraden de curve van obstakel volume uitschakelen. Als u wilt de wetness ontwerpen, Wetness aanpassen control verderop kan worden gebruikt.
 
Als u obstakel LPF/HPF curven voor andere doeleinden gebruikt, zorg ervoor dat u deze hebt ingesteld op Y = 0 X = 0 (dat wil zeggen, er is geen LPF of HPF als er geen obstakel).

![Schermafbeelding van Wwise obstakel curve-editor](media/wwise-obstruction-curve.png)

### <a name="design-project-acoustics-mixer-parameters"></a>Project akoestische mixer-parameters ontwerpen
U kunt globale weerklank eigenschappen beheren door naar het tabblad mixer-invoegtoepassing van het Project akoestische Bus te gaan. Dubbelklik op "Project akoestische Mixer (aangepast)" om de mixer-invoegtoepassing instellingen deelvenster te openen.

U ziet ook dat de mixer-invoegtoepassing een optie 'Spatialization uitvoeren heeft'. Als u echter liever van Project akoestische ingebouwde spatialization, schakel het selectievakje 'Spatialization uitvoeren' in en kies uit de HRTF of schuif. Zorg ervoor dat u het uitschakelen van een droge Aux-bussen die u hebt ingesteld, anders u twee keer het pad hoort. Gebruik de 'Wetness aanpassen' en "Weerklank tijd schaalfactor" algemeen beheer op de combinatie weerklank uitoefenen. Houd er rekening mee moet u opnieuw opstarten Unreal en vervolgens opnieuw genereren soundbanks voordat play om op te halen mixer-invoegtoepassing wijzigingen in de configuratie, zoals het selectievakje 'Spatialization uitvoeren' bereikt.

![Schermafbeelding van Project akoestische Wwise mixer-invoegtoepassing opties](media/mixer-plugin-global-settings.png)

## <a name="set-project-acoustics-design-controls-in-the-wwise-actor-mixer-hierarchy"></a>Set-Project akoestische besturingselementen in de hiërarchie van de actor-mixer Wwise ontwerpen
De parameters van het besturingselement van een afzonderlijke actor-mixer, dubbelklik op de Actor-Mixer, en klik vervolgens op de tab van Mixer-invoegtoepassing. Hier kunt u moet zich om alle parameters op het niveau van de per-geluid te wijzigen. Deze waarden worden gecombineerd met de labels instellen vanaf de Unreal (Zie hieronder). Bijvoorbeeld, als het Project akoestische Unreal invoegtoepassing Outdoorness aanpassing ingesteld op een object aan 0,5 en Wwise sets die worden-0.25, de resulterende Outdoorness aanpassing toegepast op die goed is 0,25.

![Schermafbeelding van geluid mixer-instellingen in Wwise actor-mixer hiërarchie per](media/per-sound-mixer-settings.png)

### <a name="ensure-the-aux-bus-has-dry-send-and-output-bus-has-wet-send"></a>Zorg ervoor dat de bus aux heeft droge verzenden en uitvoer bus heeft NAT verzenden
Houd er rekening mee dat de installatie vereiste actor-mixer de gebruikelijke uitwisselingen droge routering in Wwise. Het genereert weerklank signaal op de actor-mixer uitvoer-bus (ingesteld op het Project akoestische Bus) en droge signaal langs de gebruiker gedefinieerde aux-bus. Deze routering is vereist om functies van de Wwise mixer-invoegtoepassing API die gebruikmaakt van de invoegtoepassing Project akoestische Wwise.

![Schermafbeelding van Wwise editor van richtlijnen voor het ontwerpen van stem van Project akoestische](media/voice-design-guidelines.png)
 
### <a name="set-up-distance-attenuation-curves"></a>Afstand afname curven instellen
Zorg ervoor dat eventuele afname curve die worden gebruikt door de actor-mixers met behulp van Project akoestische hebt zelfgedefinieerde aux verzenden is ingesteld op "uitvoer bus volume." Wwise doet dit om de zojuist gemaakte afname curven standaard. Als u een bestaand project migreert bent, Controleer de instellingen van de curve. 

De simulatie Project akoestische heeft standaard een straal van 45 meters om de locatie van de speler. Het algemeen wordt aangeraden de curve afname instelt op-200 dB rond die afstand. Deze afstand is niet een vaste beperking. Voor sommige klinkt wapens kunt u een grotere radius. In dergelijke gevallen is het voorbehoud dat alleen geometrie binnen 45 m van de locatie van de speler wordt opgenomen. Als de speler in een kamer wordt en een geluid bron buiten de ruimte en 100 miljoen opgeslagen is, worden deze goed occluded. Als de bron in een ruimte is en de speler is buiten en 100 miljoen weg, wordt niet deze worden naar behoren occluded.

![Schermafbeelding van Wwise afname curven](media/atten-curve.png)

## <a name="set-up-scene-wide-project-acoustics-properties"></a>Scène gehele Project akoestische eigenschappen instellen

De actor akoestische ruimte wordt aangegeven dat veel besturingselementen die het gedrag van het systeem wijzigen en die zijn gebruikt bij het opsporen.

![Schermafbeelding van Unreal akoestische ruimte besturingselementen](media/acoustics-space-controls.png)

* **Akoestische gegevens:** Dit veld moet een actief voltooide akoestische uit de map inhoud/akoestische worden toegewezen. De invoegtoepassing Project akoestische wordt de map inhoud/akoestische automatisch toegevoegd aan de verpakte mappen van uw project.
* **Grootte van de tegel:** De afmetingen van het gebied rond de listener die u wilt dat akoestische gegevens in het RAM-geheugen geladen. Zolang de listener tests onmiddellijk rond de speler worden geladen in, de resultaten zijn hetzelfde als het laden van akoestische gegevens voor alle tests. Grotere tegels meer RAM-geheugen gebruiken, maar beperken van schijf-i/o
* **Automatisch Stream:** Wanneer dit is ingeschakeld, worden automatisch geladen in nieuwe tegels als de listener van de rand van een geladen regio bereikt. Als uitgeschakeld, moet u nieuwe tegels handmatig via code of blauwdrukken laden
* **Schaal van cache:** bepaalt de grootte van de cache die wordt gebruikt voor akoestische query's. Een kleinere cache minder RAM-geheugen gebruikt, maar CPU-gebruik voor elke query kan verhogen.
* **Akoestische ingeschakeld:** Een besturingselement foutopsporing inschakelen snel A / B bij het omschakelen van de simulatie akoestische. Dit besturingselement wordt genegeerd in de verzending van configuraties. De controle is handig voor het vinden van als een bijzondere audio bug zijn afkomstig van de berekeningen akoestische of een ander probleem in het project Wwise.
* **Afstanden bijwerken:** Gebruik deze optie als u wilt de gegevens vooraf voltooide akoestische voor afstand query's gebruiken. Deze query's zijn vergelijkbaar met ray webcasts, maar ze zijn vooraf berekende dus nemen veel minder CPU. Het gebruik van een voorbeeld is voor discrete weerspiegeling van het dichtstbijzijnde oppervlak voor de listener. Als u wilt volledig gebruikmaken van dit, moet u met code of blauwdruk afstanden query.
* **Tekenen statistieken:** Terwijl de UE `stat Acoustics` krijgt u met CPU-gegevens, deze status weer ziet u de momenteel geladen kaart, RAM-gebruik, en er nog andere statusinformatie in de rechterbovenhoek van het scherm.
* **Tekenen Voxels:** Overlay voxels dicht bij de listener met de voxel grid gebruikt tijdens runtime interpolatie. Als een zender zich in een runtime-voxel, mislukt dit akoestische query's.
* **Tekenen tests:** De tests voor deze scène weergeven. Ze worden verschillende kleuren, afhankelijk van de load-status.
* **Tekenen afstanden:** Als de Update afstanden is ingeschakeld, weergegeven dit een vak op het dichtstbijzijnde oppervlak voor de listener in quantized richtingen rond de listener.

## <a name="actor-specific-acoustics-design-controls"></a>Actor-specifieke akoestische ontwerp besturingselementen
Deze besturingselementen ontwerp zijn gericht op een afzonderlijk onderdeel van de audio in Unreal.

![Schermopname van het onderdeel voor Unreal Audio-besturingselementen](media/audio-component-controls.png)

* **Bedekking vermenigvuldiger:** Hiermee bepaalt u het effect van bedekking. Waarden > 1 bedekking wordt ingegaan. Waarden < 1 wordt worden geminimaliseerd.
* **Aanpassing van wetness:** Aanvullende weerklank dB
* **Vermenigvuldiger voor Decay tijd:** Besturingselementen voor de RT60 multiplicatively, op basis van de uitvoer van de simulatie akoestische
* **Aanpassing van outdoorness:** Hiermee bepaalt u hoe buitenshuis de weerklank is. Waarden dichter bij 0 zijn meer de binnenshuis, dichter bij 1 meer buitenshuis. Deze aanpassing is additieve, zodat de binnenshuis, instellen op -1 wordt afdwingen instellen op + 1 afdwingt buitenshuis.
* **Verzending Db:** Een extra via-de-prikbord geluid renderen met dit volume in combinatie met afname van de regel van zicht op basis van afstand.
* **NAT-breedteverhouding afstand verdraaien:** Hiermee past u de weerklank kenmerken op de broncomputer alsof het dichter/verder verlaat, zonder gevolgen voor het pad.
* **Akoestische Parameters weergeven:** Gegevens weergeven voor foutopsporing rechtstreeks boven op het onderdeel in het spel. (alleen voor niet-verzending configuraties)

## <a name="blueprint-functionality"></a>Blauwdruk functionaliteit
De actor akoestische ruimte zijn toegankelijk via de blauwdruk, functies, zoals het laden van een kaart of wijzigen van instellingen via niveau scripting bieden. We bieden twee voorbeelden.

### <a name="add-finer-grained-control-over-streaming-load"></a>Fijnmaziger controle over het streamen van load toevoegen
Voor het beheren van de akoestische gegevens streaming zelf in plaats van streaming automatisch op basis van de positie van de speler kunt u de functie van de blauwdruk Force Load tegel:

![Schermafbeelding van de blauwdruk Streaming opties in Unreal](media/blueprint-streaming.png)

* **Doel:** De actor AcousticsSpace
* **Center positie:** Het midden van de regio waarvoor gegevens worden geladen
* **Tests buiten de tegel verwijderen:** Als dit selectievakje inschakelt, worden alle tests in de nieuwe regio niet uit het RAM-geheugen verwijderd. Als dit selectievakje uitschakelt, wordt de nieuwe regio in het geheugen geladen terwijl het verlaten van de bestaande tests ook in het geheugen geladen
* **Blokkeren bij voltooiing:** Kunt u de tegel een synchrone bewerking laden

Blokgrootte moet al worden ingesteld voordat u aanroept Force Load tegel. U kunt bijvoorbeeld, er ongeveer als volgt een ACE-bestand laden, het instellen van de grootte van uw tegel en het streamen in een regio doen:

![Schermafbeelding van de Streaming-Setup-opties in Unreal](media/streaming-setup.png)

### <a name="optionally-query-for-surface-proximity"></a>(Optioneel) een query voor surface nabijheid
Als u wilt zien hoe dicht oppervlakken zijn in een bepaalde richting rond de listener, kunt u de Query afstand-functie. Deze functie kan nuttig zijn voor het aansturen van gerichte vertraagde weerspiegeling, of voor andere game logica aangestuurd door surface nabijheid. De query is goedkoper zijn dan een ray cast omdat de resultaten worden opgehaald uit de opzoektabel akoestische.

![Schermopname van voorbeeld van de blauwdruk afstand query](media/distance-query.png)

* **Doel:** De actor AcousticsSpace
* **Zoek richting:** De richting om op te vragen, gecentreerd op de listener
* **Afstand:** Als de query is gelukt, wordt de afstand naar het dichtstbijzijnde pijplijnen
* **Geretourneerde waarde:** Booleaans: waar als de query is gelukt, anders ONWAAR

## <a name="next-steps"></a>Volgende stappen
* Ontdek de concepten achter de [proces ontwerpen](design-process.md)
* [Maak een Azure-account](create-azure-account.md) te maken van uw eigen scène


