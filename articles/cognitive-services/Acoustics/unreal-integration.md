---
title: Unreal-en WWise-integratie in Project akoestische
titlesuffix: Azure Cognitive Services
description: Deze beschrijving beschrijft de integratie van de Unreal-en WWise-invoeg toepassingen van het project in uw project.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: 5511dd6b9a7d77c0988a94fef747a30d25bb4fc3
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706627"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Unreal-en WWise-integratie in Project akoestische
Met deze procedure kunt u gedetailleerde integratie stappen van het invoeg toepassing project akoestische conversies in uw bestaande Unreal-en WWise-Game project. 

Software vereisten:
* [Unreal-Engine](https://www.unrealengine.com/) 4,20 of 4,21
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018,1.\*
* [WWise-invoeg toepassing voor Unreal](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  * Als u een directe integratie van de WWise-SDK gebruikt in plaats van de WWise Unreal-invoeg toepassingen, raadpleegt u de Unreal-invoeg toepassing voor project akoestische en past u WWise-API-aanroepen aan.

Als u project akoestische wilt gebruiken met een andere audio-engine dan WWise, moet u een uitbrei ding aanvragen op het [discussie forum van het project akoestische](https://github.com/microsoft/ProjectAcoustics/issues). U kunt de Unreal-invoeg toepassing voor project akoestische gebruiken om een query uit te voeren op akoestische gegevens en vervolgens API-aanroepen naar uw engine te maken.

## <a name="download-project-acoustics"></a>Project akoestische downloaden
Als u dat nog niet hebt gedaan, downloadt u het [Unreal & WWise-invoeg pakket](https://www.microsoft.com/download/details.aspx?id=58090)voor het project. 

We hebben een Unreal-Engine en een WWise-mixer-invoeg toepassing opgenomen in het pakket. De Unreal-invoeg toepassing biedt de integratie van de editor en runtime. Tijdens de gameplay berekent de Unreal-invoeg toepassing van het project de para meters, zoals bedekking voor elk spel object elk frame. Deze para meters worden omgezet naar WWise API-aanroepen.

## <a name="review-integration-steps"></a>Integratie stappen controleren

Dit zijn de belangrijkste stappen om het pakket te installeren en te implementeren in uw game.
1. De project akoestische WWise mixer-invoeg toepassing installeren
2. (Re) Implementeer WWise in uw spel. In deze stap wordt de invoeg toepassing mixer door gegeven aan uw spel project.
3. De Unreal-invoeg toepassing voor project akoestische toevoegen aan uw spel
4. WWise van de Unreal-invoeg toepassing uitbreiden
5. Games bouwen en controleren of python is ingeschakeld
6. Stel uw WWise-project in om project akoestische te gebruiken
7. Audio-instellingen in Unreal

## <a name="1-install-the-project-acoustics-mixer-plugin"></a>1. De invoeg toepassing voor project akoestische mixers installeren
* Open WWise Launcher en selecteer vervolgens op het tabblad **plugins** onder **nieuwe invoeg toepassingen installeren**de optie **toevoegen uit map**. 

    ![Scherm opname van het installeren van een invoeg toepassing in WWise Launcher](media/wwise-install-new-plugin.png)

* Kies de `AcousticsWwisePlugin\ProjectAcoustics` map die is opgenomen in het pakket dat u hebt gedownload. Deze bevat de WWise mixer-invoeg toepassing.

* WWise installeert de invoeg toepassing. De geluids items van het project worden nu weer gegeven in de lijst geïnstalleerde invoeg toepassingen in WWise.
![Scherm opname van de lijst met WWise geïnstalleerde invoeg toepassingen na de installatie van de project akoestische](media/unreal-integration-post-mixer-plugin-install.png)

## <a name="2-redeploy-wwise-into-your-game"></a>2. (Re) Implementeer WWise in uw game
Implementeer WWise opnieuw op uw game, zelfs als u al een WWise hebt geïntegreerd. Hiermee wordt de WWise-invoeg toepassing voor project akoestische verzameld.

* **Engine-invoeg toepassing:** Als WWise is geïnstalleerd als een game-invoeg toepassing in een C++ Unreal-project, slaat u deze stap over. Als het wordt geïnstalleerd in plaats van een engine-invoeg toepassing, bijvoorbeeld omdat uw Unreal-project alleen blauw drukken is, is WWise-implementatie met onze mixer-invoeg toepassing complexer. Maak een dummy, leeg Unreal C++ -project, sluit het als Unreal-editor wordt geopend en volg de resterende procedure om WWise in dit dummy-project te implementeren. Kopieer vervolgens de geïmplementeerde WWise-invoeg toepassing.
 
* Klik in WWise Launcher op het tabblad **Unreal Engine** en klik vervolgens op het Hamburger menu naast **recente Unreal-Engine projecten** en selecteer **Bladeren naar project**. Open het Unreal-project `.uproject` bestand van uw game.

    ![Scherm opname van het tabblad Unreal van WWise Launcher](media/wwise-unreal-tab.png)

* Klik vervolgens op **WWise integreren in Project** of **WWise wijzigen in Project**. Deze stap (re) integreert WWise binaire bestanden in uw project, met inbegrip van de project akoestische mixer-invoeg toepassing.

* **Engine-invoeg toepassing:** Als u WWise als een engine-invoeg toepassing gebruikt en het dummy-project als hierboven hebt gemaakt, kopieert u de `[DummyUProject]\Plugins\Wwise` map WWise geïmplementeerd: `[UESource]\Engine\Plugins\Wwise`en plakt u deze over. `[DummyUProject]`is het lege Unreal C++ -projectmap en `[UESource]` is waar u de Unreal-Engine bronnen hebt geïnstalleerd. Zodra u klaar bent met kopiëren, kunt u het dummy-project verwijderen.

## <a name="3-add-the-project-acoustics-unreal-plugin-to-your-game"></a>3. De Unreal-invoeg toepassing voor project akoestische toevoegen aan uw spel
 
* Kopieer de `Unreal\ProjectAcoustics` map in het invoeg toepassings pakket en maak een nieuwe `[UProjectDir]\Plugins\ProjectAcoustics`map, `UProjectDir` waarbij de projectmap van het spel is dat `.uproject` het bestand bevat.
  * **Engine-invoeg toepassing**: Als u WWise als een engine-invoeg toepassing gebruikt, moet u ook project akoestische als Unreal-Engine-invoeg toepassing gebruiken. In plaats van de bovenstaande doelmap gebruikt u: `[UESource]\Engine\Plugins\ProjectAcoustics`.

* Controleer of er een `Wwise` map naast de `ProjectAcoustics` map wordt weer geven. De WWise-invoeg toepassing bevat naast binaire bestanden voor de mixer-invoeg toepassing die u (opnieuw) in stap 2 hierboven hebt geïmplementeerd.

## <a name="4-extend-wwises-unreal-plugin-functionality"></a>4. WWise van de Unreal-invoeg toepassing uitbreiden
* De Unreal-invoeg toepassing voor project akoestische vereist extra gedrag van de WWise Unreal plugin-API per [deze richt lijnen](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html). We hebben een batch-bestand opgenomen om de patch procedure te automatiseren. 
* In `Plugins\ProjectAcoustics\Resources`, uitvoeren `PatchWwise.bat`. In de onderstaande voorbeeld afbeelding wordt het voorbeeld project van AcousticsGame gebruikt.

    ![Scherm opname van het Windows Verkenner-venster met de patch WWise](media/patch-wwise-script.png)

* Als u de DirectX SDK niet hebt geïnstalleerd, moet u een opmerking plaatsen bij de regel met DXSDK_DIR in`[UProject]\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`

    ![Scherm opname van code-editor met DXSDK-opmerkingen](media/directx-sdk-comment.png)

## <a name="5-build-game-and-check-python-is-enabled"></a>5. Games bouwen en controleren of python is ingeschakeld

* Compileer uw game en controleer of deze correct is gebouwd. Als dat niet het geval is, controleert u de vorige stappen zorgvuldig voordat u doorgaat. 
* Open het project in de Unreal-editor. 
* **Engine-invoeg toepassing:** Als u de ProjectAcoustics as-engine-invoeg toepassing gebruikt, moet u er ook voor zorgen dat deze is ingeschakeld, vermeld onder ingebouwde invoeg toepassingen.
* Er wordt een nieuwe modus weer gegeven, die aangeeft dat de geluids van het project is geïntegreerd.

    ![Scherm opname van Unreal met de akoestische modus Full](media/acoustics-mode-full.png)

* Controleer of de python-invoeg toepassing voor Unreal is ingeschakeld. Dit is vereist voor een juiste werking van de integratie van de editor.

    ![Scherm opname van het inschakelen van python-uitbrei dingen in de Unreal-editor](media/ensure-python.png)

## <a name="6-wwise-project-setup"></a>6. WWise-project instellen

Een voor beeld van een WWise-project is opgenomen in de down load samples. We raden u aan dit naast deze instructies te bekijken. De onderstaande scherm afbeeldingen worden uit dit project gehaald.

### <a name="bus-setup"></a>Bus-installatie
* De Unreal-invoeg toepassing voor project akoestische zoekt naar de gekoppelde mixer-invoeg toepassing op een ****** bus met deze `Project Acoustics Bus`exacte naam:. Maak een nieuwe audio bus met deze naam. De mixer-invoeg toepassing kan in verschillende configuraties worden gebruikt, maar we nemen er nu rekening mee dat deze alleen voor de verwerking van galming wordt toegepast. Deze bus heeft het gemengde galm signaal voor alle bronnen die akoestische gebruiken. De IT-upstream kan worden gecombineerd in elke wille keurige combi natie van een bus, een voor beeld wordt hieronder weer gegeven, afkomstig uit het WWise-voorbeeld project dat in de voorbeeld down load is opgenomen.

    ![Scherm opname van WWise Busses met project akoestische bus](media/acoustics-bus.png)

* De kanaal configuratie op de bus moet worden ingesteld op een van de volgende `1.0, 2.0, 4.0, 5.1 or 7.1`:. Andere configuraties hebben geen uitvoer op deze bus.

    ![Scherm opname van kanaal configuratie opties voor de project akoestische bus](media/acoustics-bus-channel-config.png)

* Ga nu naar de gegevens van de geluids bus van het project en zorg ervoor dat u het tabblad invoeg toepassing voor de mixer kunt zien

    ![Scherm opname van WWise waarin wordt weer gegeven hoe u het tabblad invoeg toepassing voor de projector kunt inschakelen voor de geluids bus van het project](media/mixer-tab-enable.png)

* Ga vervolgens naar het tabblad invoeg toepassing voor mixer en voeg de invoeg toepassing voor de project akoestische mixers toe aan de bus

    ![Screenshow van WWise bus waarin wordt getoond hoe u de project akoestische mixer-invoeg toepassing kunt toevoegen](media/add-mixer-plugin.png)

### <a name="actor-mixer-hierarchy-setup"></a>Setup actor-mixer-hiërarchie
* Uit het oogpunt van prestaties past de geluids kwaliteit van het project op alle bronnen tegelijk audio-DSP toe. Hiervoor moet de invoeg toepassing worden gebruikt als een mixer-invoeg toepassing. WWise vereist dat mixer-invoeg toepassingen zich op de uitvoer bus bevinden, hoewel de uitvoer bus doorgaans het droge uitvoer signaal uitvoert. Voor de geluids sterkte van het project moet het droge signaal worden gerouteerd via aux Busses terwijl het natte signaal `Project Acoustics Bus`wordt uitgevoerd op de. Het volgende proces ondersteunt geleidelijke migratie naar deze signaal stroom.

* Stel dat u een bestaand project hebt met een actor-mixer-hiërarchie met beeld, wapens en anderen op het hoogste niveau. Elk heeft een bijbehorende uitvoer bus voor de droge mix. Stel dat u beeld wilt migreren om akoestische te gebruiken. Maak eerst een corresponderende aux-bus om hun droge submix te maken die een onderliggende van de beeld-uitvoer bus is. We hebben bijvoorbeeld het voor voegsel ' droge ' in de afbeelding hieronder gebruikt voor het ordenen van deze, hoewel de exacte naam niet van belang is. Meters of effecten die u op de beeld-bus had, functioneren net als voorheen.

    ![Scherm opname van aanbevolen WWise droge mix instellen](media/wwise-dry-mix-setup.png)

* Wijzig vervolgens de bus-uitvoer structuur voor de beeld actor-mixer als volgt, waarbij de project akoestische bus is ingesteld als uitvoer bus en Dry_Footsteps is ingesteld als een door de gebruiker gedefinieerde aux-bus.

    ![Scherm afbeelding van aanbevolen WWise actor mixer-Setup](media/actor-mixer-bus-settings.png)

* Alle beeld verkrijgen nu akoestische behandeling en voeren hun galm uit op de projector van het project akoestische. Het droge signaal wordt gerouteerd via Dry_Footsteps en wordt zo ruimtelijk gewend.

* De geluids fragmenten van het project zijn alleen van toepassing op geluiden met een 3D-locatie in de wereld. De volgende [WWise-documentatie](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/)moeten de plaatsings eigenschappen worden ingesteld zoals weer gegeven. De "3D spatialization"-instelling kan "position" of "position + orientation" zijn.

    ![Scherm opname van aanbevolen WWise actor Positioning-instellingen](media/wwise-positioning.png)

* Het is niet mogelijk om de uitvoer bus in te stellen op een andere bus die upstream combineert in **project akoestische bus** . WWise legt deze vereiste op mixer-invoeg toepassingen.

* Als u een onderliggend element in de beeld actor-mixer-hiërarchie niet wilt gebruiken, kunt u altijd ' bovenliggende ' overschrijven ' gebruiken om dit te doen.

* Als u met spelletjes of door de gebruiker gedefinieerde verzen dingen voor een actor-mixer in het spel gebruikt, schakelt u deze uit op deze actor-mixer om te voor komen dat er twee keer meer worden toegepast.

### <a name="spatialization"></a>Spatialization
Standaard past de WWise mixer-invoeg toepassing van het project de convolutie-galm toe, waardoor WWise spatialization kan worden gesleept. Wanneer u project akoestische in deze standaard alleen-galm configuratie gebruikt, kunt u een kanaal configuratie-en spatialization-methode gebruiken voor uw droge mix, zodat u bijna elke spatializer kunt combi neren met een galm van het project akoestische. Uw opties zijn onder andere [op Ambisonics gebaseerde binaural spatializers](https://www.audiokinetic.com/products/ambisonics-in-wwise/) en [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound).
 
Project akoestische bevat een optionele spatializer die zowel op objecten gebaseerde HRTF-rendering als op basis van een object ondersteunt. Schakel het selectie vakje ' spatialization uitvoeren ' in de instellingen van de mixer-invoeg toepassing in en kies tussen HRTF of pannen en schakel door de gebruiker gedefinieerde aux-verzen dingen hierboven in. De spatialization-modus kan in realtime niet worden gewijzigd, omdat er een nieuwe bank moet worden gegenereerd. U moet Unreal opnieuw starten, vervolgens soundbanks opnieuw genereren voordat u op hervatten drukt om de configuratie wijzigingen voor de invoeg toepassing op te halen, zoals het selectie vakje spatialization uitvoeren.

![Scherm opname van de spatialization-instellingen van de WWise mixer-invoeg toepassing](media/mixer-spatial-settings.png)

Helaas kunnen andere op objecten gebaseerde spatializer-invoeg toepassingen op dit moment niet worden ondersteund omdat ze zijn geïmplementeerd als mixer-invoeg toepassingen. het is momenteel niet toegestaan meerdere mixer-invoeg toepassingen toe te staan die aan één actor-mixer zijn toegewezen.  

## <a name="7-audio-setup-in-unreal"></a>7. Audio-instellingen in Unreal
* Eerst moet u uw game niveau maken om een akoestische activa te maken, die in `Content\Acoustics`worden geplaatst. Raadpleeg de [zelf studie voor Unreal maken](unreal-baking.md) en hervat deze. Een aantal pre-geïntegreerde niveaus zijn opgenomen in het voorbeeld pakket.
* Maak een actor voor akoestische ruimte in uw scène. Maak slechts een van deze actors op een niveau, aangezien deze de akoestische voor het hele niveau vertegenwoordigt. 

    ![Scherm opname van de Unreal-editor die het maken van de akoestische Space actor weergeeft](media/create-acoustics-space.png)

* Wijs nu de geïntegreerde akoestische-gegevensasset toe aan de gegevens sleuf van de akoestische ruimte op de akoestische Space actor. Uw scène heeft nu akoestische beelden.

    ![Scherm opname van de Unreal-editor voor howing geluids toewijzing van activa](media/acoustics-asset-assign.png)

* Voeg nu een lege actor toe en Ga als volgt te werk:

    ![Scherm opname van de Unreal-editor die het gebruik van akoestische onderdelen in een lege actor weergeeft](media/acoustics-component-usage.png)

1. Voeg een geluid geluids onderdeel toe aan de actor. Dit onderdeel breidt het WWise-audio onderdeel uit met functionaliteit voor het akoestische project.
2. Het vak afspelen op begin is standaard ingeschakeld. Hiermee wordt de gekoppelde WWise-gebeurtenis geactiveerd wanneer het niveau wordt opgestart.
3. Gebruik het selectie vakje akoestische para meters weer geven om informatie over fout opsporing op het scherm af te drukken over de bron.
    ![Scherm opname van het Unreal editor-geluids paneel op de geluids bron, waarbij de waarden voor fout opsporing zijn ingeschakeld](media/debug-values.png)
4. Een WWise-gebeurtenis toewijzen per gewone WWise-werk stroom
5. Zorg ervoor dat ruimtelijke audio gebruiken is uitgeschakeld. Als u op dit moment project akoestische gebruikt voor een bepaald audio onderdeel, kunt u de ruimtelijke audio-engine van WWise niet tegelijkertijd gebruiken voor geluids fragmenten.

U bent nu klaar. Ga door de scène en verken de akoestische effecten.

## <a name="next-steps"></a>Volgende stappen
* [Ontwerp](unreal-workflow.md) zelf studie voor project akoestische in Unreal/WWise
* [Meer informatie over hoe u Bakes kunt doen](unreal-baking.md) voor uw game scènes 
