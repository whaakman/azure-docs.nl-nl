---
title: Zelf studie voor project akoestische Unreal-ontwerp
titlesuffix: Azure Cognitive Services
description: In deze zelf studie wordt de ontwerp werk stroom voor project akoestische in Unreal en WWise beschreven.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 817a11171c5b4b4ef205e5fbb04f9b6d6d85b248
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854252"
---
# <a name="project-acoustics-unrealwwise-design-tutorial"></a>Unreal/WWise ontwerp zelf studie voor project akoestische
In deze zelf studie worden de ontwerp instellingen en werk stromen voor project akoestische in Unreal en WWise beschreven.

Software vereisten:
* Een Unreal-project met het project akoestische WWise-en Unreal-invoeg toepassingen

Als u een Unreal-project wilt ophalen met project akoestische, kunt u het volgende doen:
* Volg de [Unreal-integratie](unreal-integration.md) instructies van het project voor het toevoegen van project akoestische aan uw Unreal-project
* Of gebruik het project [akoestische voor beeld](unreal-quickstart.md)van het project.

## <a name="setup-project-wide-wwise-properties"></a>Eigenschappen voor het project Wide WWise instellen
WWise heeft wereld wijde obstructie-en bedekking-curven die van invloed zijn op hoe de invoeg toepassing project akoestische de WWise-audio-DSP bewaart.

### <a name="design-wwise-occlusion-curves"></a>WWise bedekking-curven ontwerpen
Wanneer het project is ingesteld op actief, reageert dit op de bedekking-volume, low-pass filter (LPF) en HPF-curven (high-pass filter) die u in WWise hebt ingesteld. We raden u aan om uw volume curve type in te stellen op lineair met de waarde-100 dB voor een bedekking-waarde van 100.

Als deze instelling is ingesteld, wordt door de simulatie van het project akoestische een bedekking van-18 dB berekend op basis van de onderstaande curve op X = 18 en de bijbehorende Y-waarde is de verzwakking toegepast. Als u half bedekking wilt uitvoeren, stelt u het eind punt in op-50 dB in plaats van-100 dB of op-200 dB tot exaggerate bedekking. U kunt elke curve die het beste geschikt is voor uw game aanpassen en verfijnen.
 
![Scherm opname van WWise bedekking curve-editor](media/wwise-occlusion-curve.png)

### <a name="disable-wwise-obstruction-curves"></a>WWise obstructie-curven uitschakelen
De WWise obstructie-curven zijn van invloed op het droog niveau in isolatie, maar de geluids elementen van het project maakt gebruik van ontwerp controles en simulatie om natte/droge ratio's af te dwingen. Het is raadzaam om de volume curve voor obstructies uit te scha kelen. Als u de wetness wilt ontwerpen, gebruikt u het besturings element Wetness aanpassen dat later wordt beschreven.
 
Als u obstructie LPF/HPF-curven voor andere doel einden gebruikt, moet u ervoor zorgen dat u ze hebt ingesteld op Y = 0 bij X = 0 (dat wil zeggen: er is geen LPF of HPF wanneer er geen obstakels zijn).

![Scherm opname van de WWise-obstructie curve-editor](media/wwise-obstruction-curve.png)

### <a name="design-project-acoustics-mixer-parameters"></a>Mixer parameters voor het ontwerp van het project
U kunt algemene galm eigenschappen beheren door naar het tabblad invoeg toepassing van de project akoestische bus te gaan. Dubbel klik op ' project Akoestisches mixer (aangepast) ' om het deel venster instellingen van de mixer-invoeg toepassing te openen.

U kunt ook zien dat de mixer-invoeg toepassing een "spatialization uitvoeren"-optie heeft. Als u liever de ingebouwde spatialization van het akoestische project wilt gebruiken, schakelt u het selectie vakje ' spatialization uitvoeren ' in en kiest u van HRTF of pannen. Zorg ervoor dat u eventuele droge aux-Busses die u hebt ingesteld, uitschakelt. als dat niet het geval is, hoort u het directe pad twee keer. Gebruik het ' Wetness-aanpassings niveau ' en ' galm tijd schaal factor ' om wereld wijde controle uit te oefenen op de galm mix. Opmerking: u moet Unreal opnieuw starten, vervolgens soundbanks opnieuw genereren voordat u op PLAY drukt om de configuratie wijzigingen voor de invoeg toepassing op te halen, zoals het selectie vakje spatialization uitvoeren.

![Scherm opname van de opties voor de invoeg toepassing WWise mixer van project akoestische](media/mixer-plugin-global-settings.png)

## <a name="set-project-acoustics-design-controls-in-the-wwise-actor-mixer-hierarchy"></a>Ontwerp besturings elementen voor project akoestische sets instellen in de WWise actor-mixer-hiërarchie
Als u de para meters van een afzonderlijke actor-mixer wilt beheren, dubbelklikt u op de actor-mixer en klikt u vervolgens op het tabblad invoeg toepassing voor de mixer. Hier kunt u de para meters per geluids niveau wijzigen. Deze waarden worden gecombineerd met de instellingen van de Unreal (zie hieronder). Als de Unreal-invoeg toepassing van het project bijvoorbeeld de aanpassing van de onderdrukking van een object op 0,5 instelt en WWise instelt op-0,25, is de resulterende aanpassing van de degelijkings kracht op dat geluid 0,25.

![Scherm opname van instellingen per geluids mixer in de WWise actor-mixer-hiërarchie](media/per-sound-mixer-settings.png)

### <a name="ensure-the-aux-bus-has-dry-send-and-output-bus-has-wet-send"></a>Zorg ervoor dat de aux-bus droog verzenden heeft en dat de uitvoer bus natte Send is
Houd er rekening mee dat de vereiste actor-mixer-instelling de gebruikelijke droge en natte route ring verwisselt in WWise. Er wordt een galm signaal gegenereerd op de uitvoer bus van de actor-mixer (ingesteld op de geluids bus van het project) en het droge signaal langs de door de gebruiker gedefinieerde aux-bus. Deze route ring is vereist vanwege functies van de API van de WWise-mixer die door het project akoestische WWise-invoeg toepassing wordt gebruikt.

![Scherm opname van de WWise-editor voor de ontwerp richtlijnen voor project akoestische](media/voice-design-guidelines.png)
 
### <a name="set-up-distance-attenuation-curves"></a>Curven voor afstands verzwakking instellen
Zorg ervoor dat een verzwakkings curve die wordt gebruikt door actor-mixers met behulp van project akoestische, door de gebruiker gedefinieerde aux-verzen ding is ingesteld op het uitvoer-bus volume. WWise doet dit standaard voor nieuwe verzwakkings curven. Als u een bestaand project wilt migreren, controleert u de instellingen van de curve.

De geluids simulatie van het project heeft standaard een straal van 45 meters rond de locatie van de speler. U wordt aangeraden uw verzwakkings curve in te stellen op-200 dB rond die afstand. Deze afstand is geen harde beperking. Voor sommige klanken, zoals wapens, wilt u mogelijk een grotere straal. In dergelijke gevallen is het voor behoud dat alleen de geometrie binnen 45 m van de locatie van de speler deel zal uitmaken. Als de speler zich in een ruimte bevindt en een geluids bron zich buiten de ruimte bevindt en 100 miljoent, wordt de juiste occluded. Als de bron zich in een ruimte bevindt en de speler buiten en 100 m is, is deze niet correct occluded.

![Scherm opname van WWise-verzwakkings curven](media/atten-curve.png)

### <a name="post-mixer-equalization"></a>Nabewerking van mixer ###
 U kunt ook een post mixer-equalizer toevoegen. U kunt de project akoestische bus beschouwen als een typische galm bus (in de standaard modus galm) en een filter hierop plaatsen om de egalisatie uit te voeren. U ziet een voor beeld van dit in het project akoestische WWise-voorbeeld project.

![Scherm opname van WWise na mixer EQ](media/wwise-post-mixer-eq.png)

Zo kunt u met een filter voor hoge controle de bas afhandelen van near-veld opnamen die een bekeer bare galm van de klank leveren. U kunt ook meer controle over het post-maken bereiken door de EQ via RTPCs aan te passen, zodat u de kleur van galm kunt wijzigen tijdens game tijd.

## <a name="set-up-scene-wide-project-acoustics-properties"></a>Eigenschappen van het project akoestische scène-breed instellen

De functie voor akoestische ruimte-actor biedt veel besturings elementen waarmee het gedrag van het systeem wordt gewijzigd en kan worden gebruikt bij het opsporen van fouten.

![Scherm opname van besturings elementen voor Unreal akoestische-ruimte](media/acoustics-space-controls.png)

* **Akoestische gegevens:** Aan dit veld moet een geïntegreerde-geluids activum worden toegewezen vanuit de map Content/akoestische. De invoeg toepassing voor project akoestische voegt automatisch de map Content/akoestische toe aan de pakket mappen van uw project.
* **Grootte van tegel:** De gebieden van de regio rond de listener die u wilt dat akoestische gegevens in het RAM-geheugen worden geladen. Zolang listener-tests direct rond de speler worden geladen in, zijn de resultaten hetzelfde als het laden van akoestische gegevens voor alle tests. Grotere tegels gebruiken meer RAM-geheugen, maar beperk de schijf-I/O
* **Automatische stroom:** Wanneer deze functie is ingeschakeld, wordt automatisch in nieuwe tegels geladen omdat de listener de rand van een geladen regio bereikt. Als deze is uitgeschakeld, moet u nieuwe tegels hand matig laden via code of blauw drukken
* **Cache schaal:** Hiermee bepaalt u de grootte van de cache die wordt gebruikt voor akoestische query's. Een kleinere cache maakt gebruik van minder RAM, maar kan het CPU-gebruik voor elke query verhogen.
* **Geluids voorzieningen ingeschakeld:** Een debug-controle voor het snel A/B inschakelen van de akoestische simulatie. Dit besturings element wordt genegeerd bij de configuratie van de verzen ding. Het besturings element is handig om te vinden of een bepaalde audio fout afkomstig is van de geluids berekeningen of een ander probleem in het WWise-project.
* **Update-afstanden:** Gebruik deze optie als u de pre-geïntegreerde akoestische informatie wilt gebruiken voor afstands query's. Deze query's zijn vergelijkbaar met Ray-casts, maar ze zijn vooraf berekend, waardoor het veel minder CPU vergt. Een voor beeld van een gebruik is voor discrete reflecties van het dichtstbijzijnde Opper vlak voor de listener. Als u dit volledig wilt benutten, moet u code of blauw drukken gebruiken om de afstanden op te vragen.
* **Statistieken tekenen:** Hoewel UE `stat Acoustics` u CPU-informatie kan bieden, wordt in deze status weergave de momenteel geladen ACE-bestand, het RAM-gebruik en andere status informatie weer gegeven in de linkerbovenhoek van het scherm.
* **Voxels tekenen:** Overlay voxels dicht bij de listener die het Voxel Raster weergeeft dat tijdens runtime interpolatie wordt gebruikt. Als een zender zich in een runtime-Voxel bevindt, mislukt het uitvoeren van akoestische query's.
* **Teken sondes:** Alle tests voor deze scène weer geven. Ze zijn verschillende kleuren, afhankelijk van hun laad status.
* **Afstanden tekenen:** Als update afstanden is ingeschakeld, wordt in het dichtstbijzijnde Opper vlak een vak weer gegeven met de listener in afspiegelende richtingen rond de listener.

## <a name="actor-specific-acoustics-design-controls"></a>Actor-specifieke akoestische ontwerp besturings elementen
Deze ontwerp besturings elementen bevinden zich in een afzonderlijk audio onderdeel in Unreal.

![Scherm afbeelding van de besturings elementen van het Unreal-audio onderdeel](media/audio-component-controls.png)

* **Bedekking vermenigvuldiger:** Hiermee wordt het bedekking-effect bepaald. Met de waarden > 1 wordt de bedekking. Met de waarden < 1 wordt deze geminimaliseerd.
* **Wetness-aanpassing:** Extra galm-dB
* **Vermenigvuldigings tijd van het verval tijdstip:** Hiermee bepaalt u de RT60 multiplicatively op basis van de uitvoer van de akoestische simulatie
* **Aanpassing van de klep:** Hiermee bepaalt u hoe de reverberation buiten gaat. Waarden dichter bij 0 zijn meer binnenshuis, dichter bij 1 zijn meer buitenshuis. Deze aanpassing is additief, dus als u deze instelt op-1, wordt de indeur afgedwongen. Als u deze instelt op + 1, worden er buitenshuis afgedwongen.
* **Data Base voor verzen ding:** Genereer een extra via-the-Wall-geluids fragment met deze LOUDNESS gecombineerd met de afstands afzwakking op basis van het gezichts vermogen.
* **Natte ratio afstands verdraaien:** Hiermee past u de reverberation-kenmerken van de bron aan alsof deze dichter/verder weg waren, zonder dat dit van invloed is op het directe pad.
* **Afspelen bij starten:** Wissel knop om op te geven of het geluid automatisch moet worden afgespeeld bij het starten van de scène. Standaard ingeschakeld.
* **Akoestische para meters weer geven:** Fout opsporingsgegevens direct boven op het onderdeel weer geven. (alleen voor niet-verzend configuraties)

## <a name="blueprint-functionality"></a>Blauw druk-functie
De functie voor het maken van akoestische ruimte is toegankelijk via blauw drukken, waardoor er functionaliteit wordt geboden, zoals het laden van een kaart of het wijzigen van instellingen via niveau-scripting. Hier vindt u twee voor beelden.

### <a name="add-finer-grained-control-over-streaming-load"></a>Nauw keurige controle over streaming-belasting toevoegen
U kunt de functie voor het streamen van akoestische gegevens zelf in plaats van streaming automatisch laten beheren op basis van de positie van de speler, met behulp van de blauw druk tegel van de taak

![Scherm afbeelding van de opties voor het streamen van blauw drukken in Unreal](media/blueprint-streaming.png)

* **Stemming** De AcousticsSpace actor
* **Positie van Midden:** Het midden van de regio waarvoor gegevens moeten worden geladen
* **Ongedaan maken van het laden buiten de tegel:** Als u dit selectie vakje inschakelt, worden alle tests die geen deel uitmaken van de nieuwe regio, uit het RAM-geheugen verwijderd. Als u dit selectie vakje uitschakelt, wordt de nieuwe regio in het geheugen geladen terwijl de bestaande tests ook in het geheugen worden geladen
* **Blok keren bij voltooiing:** Hiermee wordt een synchrone bewerking door de tegel geladen

De tegel grootte moet al zijn ingesteld voordat de tegel geforceerde belasting aanroept. U kunt bijvoorbeeld het volgende doen om een ACE-bestand te laden, de tegel grootte en de stroom in een regio in te stellen:

![Scherm opname van opties voor het instellen van streaming in Unreal](media/streaming-setup.png)

De functie voor het maken van een blauw druk in dit voor beeld bevat de volgende para meters:

* **Stemming** De AcousticsSpace actor.
* **Nieuwe maken:** De akoestische gegevens activa die moeten worden geladen. Als u dit leeg laat of instelt op NULL, wordt de huidige maken verwijderd zonder dat er een nieuw bestand wordt geladen.

### <a name="optionally-query-for-surface-proximity"></a>Optioneel query voor oppervlakte nabijheid
Als u wilt zien hoe close-Opper vlakken zich in een bepaalde richting rond de listener bevinden, kunt u de functie voor query afstand gebruiken. Deze functie kan nuttig zijn bij het oplopen van gerichte reflecties of voor andere spel logica die wordt aangedreven door Surface nabijheid. De query is minder duur dan een Ray-cast, omdat de resultaten worden opgehaald uit de opzoek tabel van akoestische waarden.

![Scherm afbeelding van de voor beeld-blauw druk op de query](media/distance-query.png)

* **Stemming** De AcousticsSpace actor
* **Kijk richting:** De richting van de query in, gecentreerd bij de listener
* **Tussen** Als de query is geslaagd, wordt de afstand naar het dichtstbijzijnde Opper vlak
* **Retour waarde:** Booleaanse waarde: True als de query is geslaagd, anders onwaar

## <a name="next-steps"></a>Volgende stappen
* Bekijk de concepten achter het [ontwerp proces](design-process.md)
* [Een Azure-account maken](create-azure-account.md) om uw eigen scène te maken


