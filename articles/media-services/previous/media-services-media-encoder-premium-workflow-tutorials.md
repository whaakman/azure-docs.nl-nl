---
title: Avanced Media Encoder Premium Workflow-zelfstudies
description: Dit document bevat scenario's die laten zien hoe u geavanceerde taken met Media Encoder Premium Workflow en ook hoe u werkstromen maken met Workflow Designer.
services: media-services
documentationcenter: ''
author: xstof
manager: femila
editor: ''
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 79b2aa22307e4742d00d9422535d52c80485e2db
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249031"
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Geavanceerde zelfstudies voor Media Encoder Premium Workflow
## <a name="overview"></a>Overzicht
Dit document bevat scenario's die laten hoe zien het aanpassen van werkstromen met **Workflow Designer**. U vindt de daadwerkelijke Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

## <a name="toc"></a>INHOUDSOPGAVE
De volgende onderwerpen komen aan bod:

* [MXF naar een single-bitrate MP4-codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Een nieuwe werkstroom starten](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Met behulp van de Media-bestandsinvoer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Mediastreams inspecteren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Toevoegen van een video coderingsprogramma voor. MP4-bestand genereren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [De audiostream codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Audio en Video-streams multiplexing naar een MP4-container](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [Het MP4-bestand schrijven](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Een Media Services-Asset gemaakt uit het uitvoerbestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [De voltooide werkstroom lokaal testen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [MXF coderen in multi-bitrate MP4s - dynamische pakketten ingeschakeld](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Toevoegen van een of meer extra MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [De namen van de uitvoer configureren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Toevoegen van een afzonderlijke audiospoor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [De 'Console' SMIL-bestand toe te voegen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [MXF coderen in multi-bitrate MP4 - verbeterde blauwdruk](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [Overzicht van de werkstroom te verbeteren](#workflow-overview-to-enhance)
  * [Naamgevingsregels voor bestanden](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [Eigenschappen van onderdeel naar de hoofdmap van de werkstroom publiceren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Uitvoerbestand namen zijn afhankelijk van de gepubliceerde eigenschapswaarden zijn gegenereerd](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Miniaturen toe te voegen aan multi-bitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [Overzicht van de werkstroom om toe te voegen miniatuurweergaven](#workflow-overview-to-add-thumbnails-to)
  * [Toe te voegen JPG-codering](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [Omgaan met de conversie van de werkruimte](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [De miniaturen schrijven](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [Fouten opsporen in een werkstroom](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Werkstroom is voltooid](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Op basis van tijd trimmen van multi-bitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Overzicht van de werkstroom om te beginnen met het inkorten aan toevoegen](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Met behulp van de Stream-Trimmer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Werkstroom is voltooid](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Maak kennis met het script onderdeel](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [Uitvoeren van scripts in een werkstroom: Hallo wereld](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [Op basis van een kader trimmen van multi-bitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Overzicht van de blauwdruk om te beginnen met het inkorten aan toevoegen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [Met behulp van de lijst Clip XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [Wijzigen van de lijst met een videoclip uit een onderdeel in een script vastgelegd](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [Een ClippingEnabled gemak eigenschap toe te voegen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a id="MXF_to_MP4"></a>MXF naar een single-bitrate MP4-codering
In deze sectie ziet u hoe u een single-bitrate maken. MP4-bestand met AAC-HE gecodeerde audio van een. MXF-bestand voor invoer.

### <a id="MXF_to_MP4_start_new"></a>Een nieuwe werkstroom starten
Workflow Designer openen en selecteer Bestand > nieuwe werkruimte > transcoderen blauwdruk

De nieuwe werkstroom ziet u drie elementen:

* Primaire bronbestand
* Illustratie van de XML-lijst
* Uitvoerasset bestand  

![Nieuwe coderingsworkflow nauwkeurig af](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Nieuwe coderingsworkflow nauwkeurig af*

### <a id="MXF_to_MP4_with_file_input"></a>Met behulp van de Media-bestandsinvoer
Om het mediabestand voor invoer accepteert, wordt een gestart met het toevoegen van een Media-bestand invoer onderdeel. Een onderdeel toevoegen aan de werkstroom, zoek in het zoekvak van de opslagplaats en de gewenste post naar het ontwerpoppervlak deelvenster slepen. Herhaal de actie voor de invoer voor Media-bestand en verbinding maken met het onderdeel primaire bronbestand voor de bestandsnaam invoeren pincode van de invoer voor Media-bestand.

![Invoer voor mediabestanden verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Invoer voor mediabestanden verbonden*

In eerste instantie te gebruiken bij het ontwerpen van een aangepaste werkstroom een juiste voorbeeldbestand identificeren. Om dit te doen, klikt u op de achtergrond van de ontwerpfunctie deelvenster en zoekt u de eigenschap primaire bronbestand in het deelvenster met de rechter eigenschap. Klik op het mappictogram en selecteer het gewenste bestand voor het testen van de werkstroom. Het onderdeel Media-bestandsinvoer inspecteert het bestand en vult de uitvoer pincodes om de details van de voorbeeldbestand die wordt geïnspecteerd weer te geven.

![Invoer voor mediabestanden ingevuld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Invoer voor mediabestanden ingevuld*

Nu dat de invoer is ingevuld, wordt de volgende stap is het instellen van uitvoer instellingen voor codering. Vergelijkbaar met hoe de primaire bronbestand is geconfigureerd, nu configureren voor de eigenschap uitvoer map variabele zojuist eronder.

![Geconfigureerde eigenschappen voor invoer en uitvoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Geconfigureerde eigenschappen voor invoer en uitvoer*

### <a id="MXF_to_MP4_streams"></a>Mediastreams inspecteren
Vaak is het gewenste weten hoe de stroom ziet, zoals hiervandaan stroomt het via de werkstroom. Als u wilt een stroom op elk gewenst moment in de werkstroom controleren, klikt u een uitvoer- of invoer van de pincode op een van de onderdelen. In dit geval kunt u proberen te klikken op de pincode van de uitvoer niet-gecomprimeerde Video uit de invoer van Media-bestand. Een dialoogvenster wordt geopend waarmee de uitgaande video controleren.

![Controle van de pincode niet-gecomprimeerde Video-uitvoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Controle van de pincode niet-gecomprimeerde Video-uitvoer*

In dit geval ziet dat de video een invoer 1920 x 1080 24 frames per seconde in 4 bevat: 2:2 sampling voor een video van bijna twee minuten.

### <a id="MXF_to_MP4_file_generation"></a>Toevoegen van een video coderingsprogramma voor. MP4-bestand genereren
Nu een niet-gecomprimeerde Video en meerdere niet-gecomprimeerde Audio-uitvoer pincodes zijn beschikbaar voor gebruik van de invoer van de Media-bestand. Als u wilt de inkomende video coderen, moet een codering onderdeel worden toegevoegd aan de werkstroom - in dit geval voor het genereren van. MP4-bestanden.

Toevoegen als u wilt de videostream te H.264 coderen, het onderdeel AVC Video Encoder naar het ontwerpoppervlak voor pijplijnen. Dit onderdeel wordt van een videostream van decomprimeren als invoer en voorziet in een gecomprimeerde AVC-videostream van de pincode van de uitvoer.

![Niet-verbonden AVC-coderingsprogramma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Niet-verbonden AVC-coderingsprogramma*

De eigenschappen ervan te bepalen hoe de codering precies gebeurt. Laten we eens enkele van de belangrijkste instellingen hebben:

* Uitvoer breedte en hoogte van de uitvoer: Hiermee bepaalt u de resolutie van de gecodeerde video. 640 x 360 is in dit geval een goede instelling.
* Framesnelheid: als instelt op passthrough deze dan alleen neemt de framesnelheid bron is het mogelijk is dit echter negeren. De conversie van dergelijke framesnelheid is niet beweging-gecompenseerd.
* Profiel en niveau: Hiermee bepaalt u de AVC profiel en het niveau. Om gemakkelijk meer informatie over de verschillende niveaus en -profielen, klikt u op het vraagtekenpictogram in de component AVC Video Encoder en de help-pagina ziet u meer informatie over elk van de niveaus. In dit voorbeeld gebruikt u Main-profiel op het niveau van 3.2 (de standaardinstelling).
* Beheermodus en Bitrate (kbps): in dit scenario kiezen voor een constante uitvoer voor 1200 kbps bitrate (CBR)
* Video-indeling: bevat informatie over de VUI (Video bruikbaarheid informatie) die wordt geschreven naar de stream H.264 (side-informatie die worden kan gebruikt door een decoder voor het verbeteren van de weergave, maar niet essentieel is voor het correct worden gedecodeerd):
* NTSC (standaard voor de Verenigde Staten of Japan, met behulp van 30 fps)
* PAL (standaard voor Europa, met behulp van 25 fps)
* GOP grootte modus: vaste grootte van de GOP instellen voor ons doel met een sleutel-Interval van 2 seconden met GOPs gesloten. De instelling van 2 seconden zorgt ervoor dat de compatibiliteit met dynamische verpakking Azure Media Services biedt.

Als u wilt het coderingsprogramma AVC feed, verbinding maken met de pincode niet-gecomprimeerde Video-uitvoer van het onderdeel Media bestand invoer voor de invoer niet-gecomprimeerde Video-pincode van het coderingsprogramma AVC.

![Verbonden AVC-coderingsprogramma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Verbonden AVC Main-coderingsprogramma*

### <a id="MXF_to_MP4_audio"></a>De audiostream codering
De oorspronkelijke niet-gecomprimeerde audiostream moet op dit moment nog steeds moeten worden gecomprimeerd. Voor de compressie van de audiostream toevoegen een onderdeel AAC-codering (Dolby) naar de werkstroom.

![Niet-verbonden AVC-coderingsprogramma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Niet-verbonden AAC-coderingsprogramma*

Er is een incompatibel: Er is slechts een enkele niet-gecomprimeerde audio invoer pincode van het coderingsprogramma AAC terwijl meer dan waarschijnlijk de invoer voor Media-bestand twee verschillende niet-gecomprimeerde audiostreams beschikbaar heeft: één voor de linker audio-kanaal en één voor het recht. (Als u bent om met een rond geluid, is dat de zes kanalen.) Het is dus niet mogelijk rechtstreeks verbinding maken de audio van de bron-Media-bestandsinvoer in de audio AAC-codering. De AAC-component wordt verwacht dat een zogenaamde "interleaved" audiostream: één stroom waaraan de linker- en de juiste kanalen interleaved met elkaar. Zodra we weten dat uit onze media-bronbestand dat Audionummers op welke positie in de bron, kunnen we dergelijke interleaved audio streamen met de posities correct toegewezen spreker links en rechts genereren.

Niemand wil eerst een interleaved stream genereren van de vereiste bron audio-kanalen. Het onderdeel Audio Stream Interleaver doet dit voor ons. Toe te voegen aan de werkstroom en verbinding maken met de audio-uitvoer van de Media-bestandsinvoer erin.

![Audio Stream Interleaver verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Audio Stream Interleaver verbonden*

Nu dat we een interleaved audiostream hebben, opgeven we nog steeds niet waar de posities naar links of rechts spreker wilt toewijzen. Als u wilt dit opgeeft, kunnen we gebruikmaken van de spreker positie toewijzer van.

![Toevoegen van een spreker positie toewijzer van](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Toevoegen van een spreker positie toewijzer van*

Configureren van de spreker positie toewijzer voor gebruik met een stereo invoerstroom via een coderingsprogramma vooraf ingestelde Filter van 'Aangepaste' en het kanaal vooraf ingesteld met de naam "2.0 (L, R)." (Dit wordt toegewezen de positie links sprekerherkenning aan 1 kanaal en de positie van de juiste sprekerherkenning aan kanaal 2.)

Koppel de uitvoer van de spreker positie toewijzer van aan de invoerpoort van de AAC-codering. Vervolgens geeft u het coderingsprogramma AAC om te werken met een '2.0 (L, R)"kanaal vooraf ingesteld, zodat deze te bekommeren om stereo audio als invoer.

### <a id="MXF_to_MP4_audio_and_fideo"></a>Audio en Video-streams multiplexing naar een MP4-container
Opgegeven onze AVC gecodeerde videostream en onze AAC gecodeerd audiostream, we beide in kunt vastleggen een. MP4-container. Het proces van de verschillende streams combineren in een enkele heet 'multiplexing' (of 'muxing'). We zijn in dit geval de audio en video stromen in één samenhangend interleaving. MP4-pakket. Het onderdeel dat dit voor coördineert een. MP4-container wordt de ISO MPEG-4-Multiplexer genoemd. Voeg een naar het ontwerpoppervlak voor pijplijnen en zowel de Video AVC Encoder en de AAC-coderingsprogramma verbinden met de ingevoerde gegevens.

![Verbonden MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Verbonden MPEG4 Multiplexer*

### <a id="MXF_to_MP4_writing_mp4"></a>Het MP4-bestand schrijven
Bij het schrijven van een uitvoerbestand, wordt het onderdeel uitvoer in een bestand gebruikt. We kunnen verbinden met dit de uitvoer van de ISO MPEG-4-Multiplexer zodat de uitvoer wordt geschreven naar schijf. U doet dit door verbinden met de pincode van de uitvoer Container (MPEG-4) de ingevoerde pincode schrijven van de uitvoer in een bestand.

![Uitvoer in een bestand verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Uitvoer in een bestand verbonden*

De bestandsnaam aan die wordt gebruikt, wordt bepaald door de eigenschap van het bestand. Terwijl deze eigenschap vastgelegd op een bepaalde waarde worden kan, wordt waarschijnlijk een wil deze in te stellen via een expressie in plaats daarvan.

De werkstroom automatisch bepalen van de uitvoer de van naameigenschap van een expressie-bestand, klikt u op de knop naast de naam van het bestand (naast het pictogram van de map). Selecteer in de vervolgkeuzelijst "Expressie." Hiermee wordt de expressie-editor. Schakel eerst de inhoud van de editor.

![Lege expressie-Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Lege expressie-Editor*

De expressie-editor kunt u een letterlijke waarde, gecombineerd met een of meer variabelen opgeven. Variabelen beginnen met een dollarteken. Als u de sleutel $ bereikt, ziet u een vervolgkeuzelijst met de keuze van beschikbare variabelen in de editor. In ons geval gebruiken we een combinatie van de uitvoer directory en de variabele van de naam base invoerbestand:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Uit de expressie-Editor gevuld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Uit de expressie-Editor gevuld*

> [!NOTE]
> Als u wilt bekijken van een bestand voor uitvoer van de coderingstaak in Azure, moet u een waarde in de expressie-editor opgeven.
>
>

Als u de expressie door ok bevestigt, voorbeeld van het eigenschappenvenster wat de eigenschap bestand lost op dit moment waarde.

![Expressie voor uitvoer dir wordt opgelost](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Expressie voor uitvoer dir wordt opgelost*

### <a id="MXF_to_MP4_asset_from_output"></a>Een Media Services-Asset gemaakt uit het uitvoerbestand
Terwijl we een MP4-bestand voor uitvoer geschreven hebben, moet er nog steeds om aan te geven dat dit bestand deel uitmaakt van de uitvoerasset welke mediaservices genereert als gevolg van deze werkstroom wordt uitgevoerd. Om dit te bereiken, wordt het knooppunt Uitvoerasset bestand op het canvas werkstroom gebruikt. Alle binnenkomende bestanden in dit knooppunt maken deel uit van de resulterende Azure Media Services-asset.

Verbinding met het maken van het onderdeel uitvoer in een bestand met de component Uitvoerasset bestand om te voltooien van de werkstroom.

![Werkstroom is voltooid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Werkstroom is voltooid*

### <a id="MXF_to_MP4_test"></a>De voltooide werkstroom lokaal testen
Als u wilt testen van de werkstroom lokaal, druk op de knop afspelen op de werkbalk aan de bovenkant. Als de werkstroom uitgevoerd, moet u de uitvoer gegenereerd in de geconfigureerde uitvoermap kunt inspecteren. Hier ziet u de voltooide MP4-uitvoerbestand dat uit het bestand van de invoerbron MXF is gecodeerd.

## <a id="MXF_to_MP4_with_dyn_packaging"></a>Ingeschakeld voor dynamische pakketten MXF in MP4 - multi-bitrate codering
In dit scenario maakt u een set met meerdere bitrate MP4-bestanden met AAC gecodeerde audio van één. MXF-bestand voor invoer.

Wanneer de uitvoer van een multi-bitrate asset vereist is voor gebruik in combinatie met de functies voor dynamische pakketten die worden aangeboden door Azure Media Services, meerdere GOP uitgelijnde MP4-bestanden van elk een andere bitrate en de resolutie moet worden gegenereerd. Om dit te doen, de [MXF-codering in een single-bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) scenario beschikken we nu een goed uitgangspunt.

![Starten van de werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Starten van de werkstroom*

### <a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Toevoegen van een of meer extra MP4-uitvoer
Elke MP4-bestand in de resulterende asset voor Azure Media Services biedt ondersteuning voor een andere bitrate en de resolutie. Laten we een of meer MP4-uitvoerbestanden toevoegen aan de werkstroom.

Om ervoor te zorgen dat we onze video encoders die zijn gemaakt met dezelfde instellingen hebben, is het meest geschikte de al bestaande AVC Video Encoder dupliceren en configureren van een andere combinatie van resolutie en bitrate (We voegen een van de 960 x 540 op 25 frames per seconde met 2,5 Mbps ). Als u wilt het bestaande coderingsprogramma dupliceren, kopiëren plakken op het ontwerpoppervlak voor pijplijnen.

Verbinding met het maken van de pincode van de niet-gecomprimeerde Video-uitvoer van de invoer van Media-bestand met onze nieuwe AVC-component.

![Tweede AVC encoder is verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Tweede AVC encoder is verbonden*

Pas nu de configuratie voor onze nieuwe AVC encoder om uit te voeren 960 x 540 met 2,5 Mbps. (Gebruik de eigenschappen 'uitvoer breedte","Uitvoer hoogte"en 'Bitrate (kbps)' voor dit.)

Krijgen we willen het resulterende actief samen met Azure Media Services dynamische pakketten gebruiken, moet het streaming-eindpunt kunnen genereren op basis van deze MP4-bestanden HLS/Fragmented MP4/DASH-fragmenten die precies zijn uitgelijnd met elkaar op een manier die clients die zijn schakelen tussen verschillende bitsnelheden een enkele goede continue video en audio ervaring krijgen. Als u wilt u dit kunt doen, moeten we om ervoor te zorgen dat de grootte voor beide MP4-bestanden in de eigenschappen van beide AVC coderingsprogramma's de GOP ('groeperen van foto's ') is ingesteld op 2 seconden, dat kan worden gedaan door:

* de grootte GOP modus instellen op vaste GOP grootte en
* de sleutel-Frame-Interval op twee seconden.
* ook worden het besturingselement GOP IDR GOP gesloten om te controleren of alle GOPs permanent op hun eigen zonder afhankelijkheden

Wijzigen zodat deze werkstroom gemakkelijker te begrijpen, de eerste AVC encoder en ' AVC Video Encoder 640 x 360 1200 kbps ' en het tweede AVC coderingsprogramma "AVC Video Encoder 960 x 540 2500 kbps."

Een tweede ISO MPEG-4 Multiplexer en een tweede uitvoer in een bestand toevoegen. Verbinding maken met de multiplexer naar de nieuwe AVC encoder en zorg ervoor dat de uitvoer wordt omgeleid naar de uitvoer in een bestand. Vervolgens ook verbinding maken met de AAC audio encoder uitvoer naar de nieuwe multiplexer van invoer. De uitvoer in een bestand kunnen op zijn beurt vervolgens worden verbonden met het knooppunt Uitvoerasset bestand toe te voegen aan de Media Services-activa die worden gemaakt.

![Tweede mixer- en uitvoer in een bestand verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Tweede mixer- en uitvoer in een bestand verbonden*

De multiplexer Chunk modus GOP count of duur configureren voor compatibiliteit met Azure Media Services dynamische pakketten, en de GOPs per segment ingesteld op 1. (Dit moet de standaardwaarde.)

![Mixer Chunk-modi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Mixer Chunk-modi*

Opmerking: kunt u dit proces herhalen voor elke andere bitrate resolutie combinaties en die u wilt hebt toegevoegd aan de uitvoer van de asset.

### <a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>De namen van de uitvoer configureren
Er is meer dan één enkel bestand toegevoegd aan de uitvoerasset. Dit biedt Zorg ervoor dat de bestandsnamen voor elk van de uitvoerbestanden van elkaar verschillen en een naamgevingsconventie misschien zelfs toepassen daarom het wissen van de bestandsnaam wat u wilt omgaan is met een noodzakelijk.

Benoemen van bestanden-uitvoer kan worden beheerd via de expressies in de ontwerpfunctie. Open het eigenschappenvenster voor een van de onderdelen van de uitvoer in een bestand en de expressie-editor voor de eigenschap van het bestand. Onze eerste uitvoerbestand is geconfigureerd via de volgende expressie (Zie de zelfstudie voor die van [MXF naar een single-bitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Dit betekent dat onze bestandsnaam wordt bepaald door twee variabelen: de uitvoermap om in te schrijven en de naam van het uitvoerbestand bron. De eerste wordt weergegeven als een eigenschap in de hoofdmap van de werkstroom en de laatste wordt bepaald door het inkomende bestand. De uitvoermap is wat u gebruikt voor het lokale testen; Deze eigenschap wordt overschreven door de werkstroom-engine wanneer de werkstroom wordt uitgevoerd door de cloud-gebaseerde Mediaprocessor in Azure Media Services.
Als u wilt geven onze uitvoerbestanden een consistente uitvoer naamgeving, wijzigen voor de eerste expressie die u wilt de naamgeving van bestanden:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

en de tweede naar:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Voer een tussenliggende test uitvoeren om ervoor te zorgen dat beide MP4-uitvoerbestanden juist worden gegenereerd.

### <a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Toevoegen van een afzonderlijke audiospoor
Zoals we later zien zullen wanneer er een ISM-bestand aan de slag met onze MP4-bestanden voor uitvoer gegenereerd, moet we ook een audio-alleen MP4-bestand als het audiospoor voor onze adaptief streamen. Dit bestand is gemaakt, een extra mixer toevoegen aan de werkstroom (ISO-MPEG-4 Multiplexer) en pincode voor de AAC-encoder-uitvoer verbinden met de ingevoerde pincode voor nummer 1.

![Audio mixer toegevoegd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Audio mixer toegevoegd*

Maak een derde onderdeel van de uitvoer in een bestand voor het uitvoeren van de uitgaande stroom van de mixer en configureren van de expressie als de naamgeving van bestanden:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Audio mixer uitvoer in een bestand maken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Audio mixer uitvoer in een bestand maken*

### <a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Toevoegen van de. ISM SMIL-bestand
Voor de dynamische verpakking om te werken in combinatie met zowel MP4-bestanden (en de MP4 alleen audio) in onze Media Services-activa, moeten we ook een manifestbestand (ook wel een 'SMIL'-bestand: gesynchroniseerd Multimedia Integration Language). Dit bestand wordt aan Azure Media Services wordt aangegeven welke MP4-bestanden zijn beschikbaar voor dynamische pakketten en welke van beide om te overwegen voor het audio streamen. Een typische manifestbestand voor een set MP4 van met een enkele audiostream ziet er zo uit:

```xml
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>
```

De ISM-bestand bevat binnen een switchinstructie een verwijzing naar elk van de afzonderlijke video MP4-bestanden en naast deze ook een (of meer) audiobestand verwijzingen naar een MP4-bestand met alleen de audio.

Genereren van het manifestbestand voor onze set MP4 van kan worden gedaan via een component, genaamd de "AMS Manifest schrijver." Als u wilt gebruiken, sleep deze naar het oppervlak en verbind de 'Schrijven voltooid' uitvoer pincodes uit drie onderdelen van uitvoer in een bestand met het Manifest van de schrijver van AMS-invoer. Controleer of de uitvoer van de schrijver AMS Manifest van de verbinding naar het bestand/Uitvoerasset.

Net als bij onze andere bestand uitvoer-onderdelen, configureer de uitvoernaam van ISM-bestand met een expressie:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Onze klaar werkstroom ziet eruit als het hieronder:

![Voltooide MXF naar multi-bitrate MP4-werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Voltooide MXF naar multi-bitrate MP4-werkstroom*

## <a id="MXF_to__multibitrate_MP4"></a>MXF coderen in multi-bitrate MP4 - verbeterde blauwdruk
In de [vorige werkstroom scenario](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) we hebben gezien hoe een enkele MXF-invoeractivum kan worden geconverteerd naar een uitvoerasset met multi-bitrate MP4-bestanden, alleen audio MP4-bestand en een manifestbestand voor gebruik in combinatie met Azure Media Services dynamische pakketten.

In dit scenario laat zien hoe sommige van de aspecten kunnen worden verbeterd en gemakkelijker gemaakt.

### <a id="MXF_to_multibitrate_MP4_overview"></a>Overzicht van de werkstroom te verbeteren
![Multibitrate MP4-werkstroom te verbeteren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4-werkstroom te verbeteren*

### <a id="MXF_to__multibitrate_MP4_file_naming"></a>Naamgevingsregels voor bestanden
In de vorige werkstroom opgegeven we een eenvoudige expressie als basis voor het genereren van bestandsnamen van de uitvoer. Hebben we enkele duplicatie echter: alle afzonderlijke uitvoer bestand onderdelen van deze expressie opgegeven.

Bijvoorbeeld, is onze onderdeel van de uitvoer bestand voor de eerste videobestand geconfigureerd met deze expressie:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Voor de tweede uitvoer video, hebben we een expressie, zoals:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Zou het niet schoner, minder fout gevoelig en handig zijn als we kunnen sommige van deze duplicaten verwijderen, en dingen in plaats daarvan meer configureerbaar maken? Gelukkig kunnen we: van de ontwerpfunctie expressie mogelijkheden in combinatie met de mogelijkheid om te maken van aangepaste eigenschappen in de hoofdmap van onze werkstroom biedt een extra laag voor uw gemak bedoeld;.

Stel dat we u filename configuratie-station van de bitsnelheden van de afzonderlijke MP4-bestanden. Deze bitsnelheden die zult streven we ernaar te configureren in één centrale plaats (in de hoofdmap van onze graph), van waar ze gaat worden geopend om te configureren en het genereren van station bestandsnaam. U doet dit door beginnen we door het publiceren van de eigenschap bitrate van beide encoders AVC naar de hoofdmap van de werkstroom, zodat deze toegankelijk is vanaf zowel de hoofdmap ook-02-de AVC coderingsprogramma's. (Zelfs als in twee verschillende punten weergegeven, is er slechts één onderliggende waarde.)

### <a id="MXF_to__multibitrate_MP4_publishing"></a>Eigenschappen van onderdeel naar de hoofdmap van de werkstroom publiceren
De eerste AVC encoder openen, gaat u naar de eigenschap Bitrate (kbps) en kies publiceren in de vervolgkeuzelijst.

![De eigenschap bitrate publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*De eigenschap bitrate publiceren*

Het dialoogvenster publiceren om te publiceren naar de hoofdmap van onze graph werkstroom configureren met een gepubliceerde naam 'video1bitrate' en een leesbare weergavenaam van 'Bitrate Video 1'. Configureren van een aangepaste naam 'Bitsnelheden Streaming' genoemd en klik op publiceren.

![De eigenschap bitrate publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Publiceren dialoogvenster voor bitrate-eigenschap*

Herhaal dezelfde voor de bitrate-eigenschap van de tweede AVC encoder en geef deze de naam 'video2bitrate' met de naam "Video 2 Bitrate", weergegeven in de dezelfde aangepaste groep 'Bitsnelheden Streaming'.

Als we nu de eigenschappen van de basis-werkstroom controleren, bekijken we onze aangepaste groep met de twee gepubliceerde eigenschappen weergegeven. Beide zijn de waarde van hun respectieve AVC encoder bitrate spiegelen.

![Eigenschappen van gepubliceerde bitrate in de hoofdmap van de werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Wanneer we toegang tot deze eigenschappen uit code of een expressie willen, kunnen we dit doen als volgt:

* vanuit de Inlinecode vanuit een component direct onder de hoofdmap: node.getPropertyAsString('.. / video1bitrate', null)
* in een expressie: ${ROOT_video1bitrate}

We de groep "Bitsnelheden Streaming" voltooien door het publiceren van onze audiotrack bitrate ook erop. In de eigenschappen van het coderingsprogramma AAC, zoek de instelling van de Bitrate en selecteer publiceren in de vervolgkeuzelijst naast het. Publiceren naar de hoofdmap van de grafiek met de naam 'audio1bitrate' en de weergavenaam "Bitrate Audio 1" in onze aangepaste groep 'Bitsnelheden Streaming'.

![Publiceren dialoogvenster voor audio bitrate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Publiceren dialoogvenster voor audio bitrate*

![Resulterende video en audio-eigenschappen in de hoofdmap](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Resulterende video en audio-eigenschappen in de hoofdmap*

Wijzigen van een van deze drie waarden ook configureren en wijzigt de waarden op de betreffende onderdelen die zijn gekoppeld aan (en waar gepubliceerd vanuit).

### <a id="MXF_to__multibitrate_MP4_output_files"></a>Uitvoerbestand namen zijn afhankelijk van de gepubliceerde eigenschapswaarden zijn gegenereerd
In plaats van hardcoderen onze gegenereerde bestandsnamen, we kunnen nu wijzigen onze expressie bestandsnaam op elk van de onderdelen van de uitvoer in een bestand te vertrouwen op de bitrate-eigenschappen die we hebben gepubliceerd in de hoofdmap van de grafiek. Beginnen met onze eerste uitvoer in een bestand, zoek de bestandseigenschap en bewerken van de expressie als volgt:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Verschillende parameters die in deze expressie kunnen worden geopend en ingevoerd door de dollarteken op het toetsenbord in het expressievenster. Een van de beschikbare parameters is onze video1bitrate-eigenschap die we eerder hebt gepubliceerd.

![Toegang tot parameters in een expressie](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Toegang tot parameters in een expressie*

Doe hetzelfde voor de uitvoer in een bestand voor onze tweede video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

en voor het bestand met alleen audio-uitvoer:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Als we nu de bitrate voor het gebruik van de video of audio-bestanden wijzigen, het respectieve coderingsprogramma zal opnieuw worden geconfigureerd en de bitrate-bestand met de naam van overeenkomst wordt alle automatisch worden herkend.

## <a id="thumbnails_to__multibitrate_MP4"></a>Miniaturen toe te voegen aan multi-bitrate MP4-uitvoer
Een werkstroom die wordt gegenereerd vanaf [een multi-bitrate MP4-uitvoer van een invoer MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we bekijken in de miniaturen toe te voegen aan de uitvoer.

### <a id="thumbnails_to__multibitrate_MP4_overview"></a>Overzicht van de werkstroom om toe te voegen miniatuurweergaven
![Multibitrate MP4-werkstroom te starten vanaf](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4-werkstroom te starten vanaf*

### <a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Toe te voegen JPG-codering
Het hart van onze miniaturen genereren is het onderdeel JPG Encoder kunt om uit te voeren, JPG-bestanden.

![JPG-coderingsprogramma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG-coderingsprogramma*

Geen echter rechtstreeks verbinding maken onze stream niet-gecomprimeerde Video uit de invoer van Media-bestand in het JPG-coderingsprogramma. In plaats daarvan wordt de verwacht op afzonderlijke frames worden overgedragen. Dit kunnen we doen via het onderdeel Video Frame-Gate.

![Een frame-poort verbinding te maken met het JPG-coderingsprogramma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Een frame-poort verbinding te maken met het JPG-coderingsprogramma*

De frame-gate elke zoveel seconden of kaders kunt een video frame om door te geven. Het interval en het tijdzoneverschil met die in dat geval kan worden geconfigureerd in de eigenschappen.

![Eigenschappen van video-Frame-Gate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Eigenschappen van video-Frame-Gate*

We maken een miniatuur elke minuut door de modus instelt op tijd (seconden) en het Interval en 60.

### <a id="thumbnails_to__multibitrate_MP4_color_space"></a>Omgaan met de conversie van de werkruimte
Hoewel het lijkt het logisch dat zowel niet-gecomprimeerde Video pincodes van de frame-gate en de Media-bestandsinvoer kunnen nu worden verbonden, zouden we ontvangt een waarschuwing als we dit zou doen.

![Invoerkleur ruimte fout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Invoerkleur ruimte fout*

Dit komt doordat de manier waarop in welke kleur informatie wordt weergegeven in de oorspronkelijke onbewerkte niet-gecomprimeerde videostream, die afkomstig zijn van onze MXF, verschilt van wat het JPG-coderingsprogramma wordt verwacht. Meer specifiek, wordt een zogenaamde 'kleur ruimte' van 'RGB' of 'Weg in grijswaarden' verwacht stromen. Dit betekent dat binnenkomende videostream van de Video Frame-Gate moet hebben een conversie met betrekking tot de kleurenruimte eerst toegepast.

Sleep naar de werkstroom de kleur ruimte Converter - Intel en deze verbinden met onze frame-gate.

![Een kleur ruimte conversieprogramma verbinding te maken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Een kleur ruimte conversieprogramma verbinding te maken*

Kies in het eigenschappenvenster de BGR 24-vermelding in de lijst met vooraf gedefinieerde instellingen.

### <a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>De miniaturen schrijven
Verschillende van onze MP4-video, het onderdeel JPG coderingsprogramma levert meer dan één bestand. Als u wilt omgaan met dit, een onderdeel van de scène zoeken JPG-bestand Writer kan worden gebruikt: het neemt de binnenkomende JPG miniaturen en schrijft deze, elke bestandsnaam wordt voorafgegaan door een ander nummer. (Het nummer doorgaans die wijzen op het aantal seconden/eenheden in de stroom die de miniatuur van is getekend.)

![Maak kennis met de schrijver scène zoeken JPG-bestand](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Maak kennis met de schrijver scène zoeken JPG-bestand*

De eigenschap mappad uitvoer configureren met de expressie: ${ROOT_outputWriteDirectory}

en de eigenschap Filename voorvoegsel met:

    ${ROOT_sourceFileBaseName}_thumb_

Het voorvoegsel bepaalt hoe de miniaturen bestanden worden wordt genoemd. Ze worden voorafgegaan door een getal dat aangeeft van de miniatuur positie in de stroom.

![Eigenschappen van de zoekopdracht JPG-bestand Writer scène](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Eigenschappen van de zoekopdracht JPG-bestand Writer scène*

Verbinding maken met de schrijver scène zoeken JPG-bestand naar het knooppunt Uitvoerasset bestand.

### <a id="thumbnails_to__multibitrate_MP4_errors"></a>Fouten opsporen in een werkstroom
De invoer van de kleur ruimte converter verbinden met de onbewerkte niet-gecomprimeerde video-uitvoer. Nu een lokale test uitvoeren voor de werkstroom uitvoeren. Er is een goede kans dat de werkstroom plotseling wordt niet langer uitgevoerd en wijzen met een rood kader op het onderdeel waarvoor een fout opgetreden:

![Kleur ruimte Converter fout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Kleur ruimte Converter fout*

Klik op het kleine red "E"-pictogram in de rechterbovenhoek hoek van de kleur ruimte Converter onderdeel om te zien wat de reden van de codering poging is mislukt.

![Kleur ruimte Converter foutberichtvenster](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Kleur ruimte Converter foutberichtvenster*

Het blijkt, zoals u kunt zien, dat is de kleurruimte van de standaard voor de kleur ruimte converter binnenkomende rec601 voor onze aangevraagde conversie van YUV naar RGB. Blijkbaar onze stream wordt de rec601 niet aangegeven. (Rec 601 is een standaard voor het coderen van geïnterlinieerde analoge video signalen in digitale vorm van de video. Het geeft een actieve regio die betrekking hebben op 720 helderheid voorbeelden en 360 chrominantie voorbeelden per regel. De kleur system-codering wordt ook wel YCbCr 4:2:2.)

U kunt dit verhelpen, geven we over de metagegevens van de stroom die we nu omgaan met rec601 inhoud. We gebruiken een Video Data Type Updater-onderdeel, gaan we tussen onze onbewerkte brongegevens en de kleurcomponent voor de conversie van ruimte om dit te doen. Dit type gegevens updater kunt u de handmatige update van bepaalde video gegevens type-eigenschappen. Configureren om aan te geven een kleur ruimte Standard van 'Rec 601'. Dit zorgt ervoor dat de Video Data Type Updater voor het taggen van de stroom met de kleur "Rec 601" ruimte als er geen kleurruimte nog gedefinieerd is. (Niet overschrijft alle bestaande metagegevens, tenzij het vakje overschrijven is geselecteerd.)

![Kleur ruimte standaard op het Type gegevens Updater bijwerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Kleur ruimte standaard op het Type gegevens Updater bijwerken*

### <a id="thumbnails_to__multibitrate_MP4_finish"></a>Werkstroom is voltooid
Nu dat onze werkstroom is voltooid, voert u een andere test uitvoeren om te zien wordt doorgegeven.

![Voltooide werkstroom voor multi-mp4-uitvoer met miniaturen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Voltooide werkstroom voor multi-mp4-uitvoer met miniaturen*

## <a id="time_based_trim"></a>Op basis van tijd trimmen van multi-bitrate MP4-uitvoer
Een werkstroom die wordt gegenereerd vanaf [een multi-bitrate MP4-uitvoer van een invoer MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we nu kijken naar de bronvideo op basis van tijdstempels bijsnijden.

### <a id="time_based_trim_start"></a>Overzicht van de werkstroom om te beginnen met het inkorten aan toevoegen
![Starten van de werkstroom om toe te voegen trimming naar](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Starten van de werkstroom om toe te voegen trimming naar*

### <a id="time_based_trim_use_stream_trimmer"></a>Met behulp van de Stream-Trimmer
De Stream Trimmer-onderdeel kunt u het begin en einde van een basis van informatie (seconden, minuten,...) tijdsinstellingen invoerstroom trim. De trimmer biedt geen ondersteuning voor verkleinen op basis van een frame.

![Stream Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Stream Trimmer*

In plaats van rechtstreeks de AVC coderingsprogramma's en spreker positie toewijzer koppelen aan de invoerpoort van Media-bestand, gaan we tussen die de trimmer stream. (Één voor de video signaal en één voor de interleaved audiosignaal).

![Stream Trimmer tussen plaatsen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Stream Trimmer tussen plaatsen*

We gaan de trimmer configureren zodat we alleen video en audio tussen 15 seconden en 60 seconden in de video wordt verwerkt.

Ga naar de eigenschappen van de Video Stream Trimmer en configureer beide begintijd (15 s) en de eindtijd (60 s) eigenschappen. Zorg ervoor dat zowel onze trimmer audio en video altijd naar dezelfde begin- en eindwaarden zijn geconfigureerd, publiceren we die naar de hoofdmap van de werkstroom.

![Begineigenschap van tijd uit Stream Trimmer publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Begineigenschap van tijd uit Stream Trimmer publiceren*

![De eigenschap dialoogvenster publiceren voor begintijd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*De eigenschap dialoogvenster publiceren voor begintijd*

![De eigenschap dialoogvenster publiceren voor eindtijd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*De eigenschap dialoogvenster publiceren voor eindtijd*

Als we nu de hoofdmap van onze werkstroom controleren, zijn beide eigenschappen netjes weergegeven en configureerbare van daaruit.

![Gepubliceerde eigenschappen die beschikbaar zijn in de hoofdmap](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Gepubliceerde eigenschappen die beschikbaar zijn in de hoofdmap*

Nu opent u de eigenschappen van het inkorten van de audio trimmer en zowel begin- en eindtijden configureren met een expressie die naar de gepubliceerde eigenschappen in de hoofdmap van de werkstroom verwijst.

Voor de audio bijsnijden begintijd:

    ${ROOT_TrimmingStartTime}

en voor de eindtijd:

    ${ROOT_TrimmingEndTime}

### <a id="time_based_trim_finish"></a>Werkstroom is voltooid
![Werkstroom is voltooid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Werkstroom is voltooid*

## <a id="scripting"></a>Maak kennis met het script onderdeel
Onderdelen van scripts kunnen willekeurige scripts tijdens de fasen van de uitvoering van onze werkstroom uitvoeren. Er zijn vier verschillende scripts die kunnen worden uitgevoerd, elk met een specifieke kenmerken en hun eigen plaats in de levenscyclus van werkstroom:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **lifeCycleScript**

De documentatie van het onderdeel in een script vastgelegd gaat in meer detail voor elk van de bovenstaande. In [in de volgende sectie](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), wordt de **realizeScript** scripting onderdeel wordt gebruikt voor het maken van een xml cliplist op elk gewenst moment, wanneer de werkstroom wordt gestart. Met dit script wordt tijdens de installatie van onderdelen, die slechts één keer wordt uitgevoerd in de levenscyclus van genoemd.

### <a id="scripting_hello_world"></a>Uitvoeren van scripts in een werkstroom: Hallo wereld
Sleep een onderdeel in een script vastgelegd naar het ontwerpoppervlak voor pijplijnen en wijzig de naam (bijvoorbeeld ' SetClipListXML').

![Een script onderdeel toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Een script onderdeel toevoegen*

Wanneer u de eigenschappen van het onderdeel in een script vastgelegd inspecteren, wordt de vier verschillende scripttypen worden weergegeven, elk op een ander script kunnen worden geconfigureerd.

![Script onderdeeleigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Script onderdeeleigenschappen*

Schakel de processInputScript en open de editor voor de realizeScript. We zijn nu ingesteld en klaar om te beginnen met scripting.

Scripts zijn geschreven in op Groovy, een dynamisch gecompileerde scripttaal voor het Java platform dat compatibel is met Java behoudt. De meeste Java-code is eigenlijk geldige Groovy code.

We gaan een eenvoudige hello world groovy script schrijven in de context van onze realizeScript. Voer de volgende in de editor:

    node.log("hello world");

Nu uitvoeren van een lokale testuitvoering. Controleer na het uitvoeren (via het tabblad van het systeem op het onderdeel in een script vastgelegd) de eigenschap Logboeken.

![Hello world-logboekuitvoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hello world-logboekuitvoer*

Het knooppuntobject dat we het Logmethode aanroept, verwijst naar onze huidige 'knooppunt' of het onderdeel dat we nu uitvoeren van scripts in. Elk onderdeel is daarom de mogelijkheid om de uitvoer-logboekgegevens, beschikbaar via het tabblad systeem. In dit geval wordt de letterlijke tekenreeks "Hallo wereld"-uitvoer Belangrijk om te begrijpen hier is dat deze bewijzen kan moet een waardevol hulpmiddel voor het opsporen van fouten, zodat u inzicht in wat het script daadwerkelijk doet.

Uit binnen onze omgeving uitvoeren van scripts hebben we ook toegang tot de eigenschappen van andere onderdelen. Probeer dit:

```java
    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up to other nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);
```

Ons ziet onze logboekvenster u het volgende:

![De uitvoer voor toegang tot de Knooppuntpaden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*De uitvoer voor toegang tot de Knooppuntpaden*

## <a id="frame_based_trim"></a>Op basis van een kader trimmen van multi-bitrate MP4-uitvoer
Een werkstroom die wordt gegenereerd vanaf [een multi-bitrate MP4-uitvoer van een invoer MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we bekijken in de bronvideo frame op basis van het bijsnijden.

### <a id="frame_based_trim_start"></a>Overzicht van de blauwdruk om te beginnen met het inkorten aan toevoegen
![Werkstroom om te beginnen met het inkorten aan toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Werkstroom om te beginnen met het inkorten aan toevoegen*

### <a id="frame_based_trim_clip_list"></a>Met behulp van de lijst Clip XML
In alle vorige workflow-zelfstudies, hebben we het Media-bestand invoer onderdeel gebruikt als onze video-invoerbron. Voor dit specifieke scenario, gebruikt u de Clip lijst Source-component in plaats daarvan. Dit moet niet per se de beste manier van werken; de bron van de lijst met Clip alleen gebruiken als er is een echte reden om dit te doen (net zoals in het volgende geval is, waar we maakt gebruik van de mogelijkheden van een videoclip lijst trimming).

Als u wilt overschakelen van onze Media-bestand-invoer voor de bron van de lijst met Clip, sleept u het brononderdeel Clip lijst naar het ontwerpvenster en de pincode Clip lijst XML-verbinding met de Clip lijst XML-knooppunt van de workflow designer. Hierdoor wordt de bron van de lijst met Clip met uitvoer pincodes, gevuld op basis van onze video-invoer. Nu verbinden met de niet-gecomprimeerde Video en Audio op niet-gecomprimeerde pincodes van de bron van de lijst met Clip de respectieve AVC coderingsprogramma's en Audio Stream Interleaver. De Media-bestandsinvoer nu verwijderen.

![De invoer van Media-bestand vervangen door de bron van de lijst met Clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*De invoer van Media-bestand vervangen door de bron van de lijst met Clip*

Het brononderdeel Clip lijst wordt als invoer een "Clip lijst XML." Bij het selecteren van het bronbestand om te testen met lokaal, wordt deze lijst clip xml wordt automatisch ingevuld voor u.

![Eigenschap van XML-lijst van een videoclip automatisch ingevuld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Eigenschap van XML-lijst van een videoclip automatisch ingevuld*

Op zoek naar iets dichter bij het XML-bestand, is dit hoe als volgt uitziet:

![Dialoogvenster met de lijst clip bewerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Dialoogvenster met de lijst clip bewerken*

Dit wordt echter niet de mogelijkheden van het XML-bestand clip lijst weergegeven. Een optie we hebben is het toevoegen van een element 'Trim' onder de video en audio source, als volgt:

![Een beperkende-element toe te voegen aan de lijst met clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Een beperkende-element toe te voegen aan de lijst met clip*

Als u de XML-lijst clip als volgt hierboven wijzigen en uitvoeren van een lokale testuitvoering, ziet u de video correct zijn verwijderd tussen 10 en 20 seconden in de video.

Deze dezelfde cliplist xml hoeft niet overeenkomstig wat gebeurt er wanneer u echter een lokaal uitvoeren, niet hetzelfde effect als toegepast in een werkstroom die wordt uitgevoerd in Azure Media Services. Wanneer Azure Premium Encoder wordt gestart, wordt het cliplist XML-bestand gegenereerd telkens opnieuw, op basis van het invoerbestand dat de coderingstaak is opgegeven. Dit betekent dat alle wijzigingen die we in het XML-bestand doen helaas zou worden overschreven.

Om de cliplist xml wordt gewist wanneer een coderingstaak wordt gestart, we kunnen opnieuw genereren op elk gewenst moment alleen na het begin van de werkstroom. Deze aangepaste acties kunnen worden uitgevoerd via het zogenaamde 'Script onderdeel'. Zie voor meer informatie, [Maak kennis met het onderdeel in een script vastgelegd](media-services-media-encoder-premium-workflow-tutorials.md#scripting).

Sleep een onderdeel in een script vastgelegd naar het ontwerpoppervlak voor pijplijnen en wijzig de "SetClipListXML."

![Een script onderdeel toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Een script onderdeel toevoegen*

Wanneer u de eigenschappen van het onderdeel in een script vastgelegd inspecteren, zijn de vier typen van de ander script wordt weergegeven, elk op een ander script kunnen worden geconfigureerd.

![Script onderdeeleigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Script onderdeeleigenschappen*

### <a id="frame_based_trim_modify_clip_list"></a>Wijzigen van de lijst met een videoclip uit een onderdeel in een script vastgelegd
Voordat we de cliplist XML-code die wordt gegenereerd tijdens het opstarten van de werkstroom schrijven kunt, moet er toegang hebben tot de cliplist XML-eigenschappen en inhoud. We kunnen dit doen als volgt:

```java
    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);
```

![Lijst met binnenkomende clip is vastgelegd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Lijst met binnenkomende clip is vastgelegd*

Eerst moeten we een manier om te bepalen vanaf het moment waarop tot het moment waarop we willen de video knippen. Als u wilt dit gemakkelijk de gebruiker minder technische van de werkstroom, publiceert u twee eigenschappen in de hoofdmap van de grafiek. U doet dit door met de rechtermuisknop op het ontwerpoppervlak voor pijplijnen en selecteer 'Eigenschap toevoegen':

* Eerste eigenschap: "ClippingTimeStart" van het type: "TIJDCODE"
* Tweede eigenschap: "ClippingTimeEnd" van het type: "TIJDCODE"

![Dialoogvenster Eigenschappen voor knippen begintijd toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Dialoogvenster Eigenschappen voor knippen begintijd toevoegen*

![Gepubliceerde eigenschappen van de tijd in de hoofdmap van de werkstroom knippen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Gepubliceerde eigenschappen van de tijd in de hoofdmap van de werkstroom knippen*

Beide eigenschappen op een geschikte waarde configureren:

![Configureren van het begin knippen en beëindigen van eigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Configureren van het begin knippen en beëindigen van eigenschappen*

Nu uit binnen ons script, we hebben toegang tot beide eigenschappen als volgt:

```java
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);
```

![Logboekvenster met begin en einde van knippen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Logboekvenster met begin en einde van knippen*

Laten we de tekenreeksen tijdcode parseren naar een handiger om te gebruiken met behulp van een eenvoudige reguliere expressie:

```java
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);
```

![Venster met de uitvoer van de geparseerde tijdcode logboek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Venster met de uitvoer van de geparseerde tijdcode logboek*

Met deze informatie bij de hand, kunnen we nu de xml cliplist aanleiding van de begin- en eindtijden voor de gewenste frame nauwkeurig knippen van de film wijzigen.

![Scriptcode om toe te voegen elementen knippen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Scriptcode om toe te voegen elementen knippen*

Dit is gedaan door middel van bestandsbewerkingen normale tekenreeks. De resulterende XML-lijst gewijzigde illustratie is teruggeschreven naar de eigenschap clipListXML in de hoofdmap van de werkstroom via de methode 'setProperty'. Het logboekvenster na het uitvoeren van een andere test zou beschrijven we het volgende:

![De resulterende lijst van de clip logboekregistratie](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*De resulterende lijst van de clip logboekregistratie*

Doe een test uitgevoerd om te zien hoe de video en audio-stromen hebben is afgekapt. Als u meer dan één testuitvoering met verschillende waarden voor de punten trimming doen, zult u merken dat deze wordt niet worden gehouden echter! De reden hiervoor is dat de designer, in tegenstelling tot de Azure-runtime, niet de xml cliplist elke uitvoering overschrijven. Dit betekent dat alleen de eerste keer dat u hebt ingesteld de in- en om punten, ervoor het XML-bestand om te zetten dat zorgt, alle andere tijden, onze component guard (als (clipListXML.indexOf ("<trim>") == -1)) wordt voorkomen dat de werkstroom van het toevoegen van een ander knippen element wanneer er al een aanwezig zijn.

Om onze werkstroom handig is dat het lokaal testen toevoegen we aanbevolen house-keeping code die als een beperkende-element al aanwezig is inspecteert. Als dit het geval is, kunnen we deze verwijderen voordat u doorgaat met het wijzigen van het XML-bestand met de nieuwe waarden. In plaats van gewoon tekenreeksmanipulatie, is het waarschijnlijk door dit doen door middel van echte xml-objectmodel parseren.

Voordat we deze code echter toevoegen kunt, moet er eerst een aantal importinstructie toe aan het begin van ons script toevoegen:

```java
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;
```

Hierna kunnen we de vereiste reinigingstape code toevoegen:

```java
    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();
```

Deze code wordt boven op het punt waarop we de beperkende elementen aan het cliplist XML-bestand toevoegen.

We kunnen op dit moment worden uitgevoerd en onze werkstroom als zoveel als we willen terwijl de ooit toegepaste wijzigingen tijd wijzigen.    

### <a id="frame_based_trim_clippingenabled_prop"></a>Een ClippingEnabled gemak eigenschap toe te voegen
Als u niet altijd bijsnijden wilt moeten worden uitgevoerd, gaan we voltooien uit onze werkstroom door toe te voegen een handige Booleaanse vlag die aangeeft of we inschakelen bijsnijden willen / knippen.

Net als voorheen is een nieuwe eigenschap in de hoofdmap van de werkstroom met de naam 'ClippingEnabled' publiceren van het type 'Boole-waarde'.

![Een eigenschap voor het inschakelen van knipsel gepubliceerd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Een eigenschap voor het inschakelen van knipsel gepubliceerd*

Met de onderstaande eenvoudige guard-component, we kunnen controleren als trimming vereist is en bepalen of onze lijst clip als zodanig worden gewijzigd moet of niet.

```java
    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }
```

### <a id="code"></a>Volledige code

```java
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from the clip list xml by parsing the xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements to cliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }
```

## <a name="also-see"></a>Zie ook
[Introductie van Premium Encoding in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Het gebruik van Premium Encoding in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[De inhoud op aanvraag met Azure mediaservice-codering](media-services-encode-asset.md#media-encoder-premium-workflow)

[Indelingen en codecs voor Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md)

[Werkstroom voorbeeldbestanden](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Explorer-hulpmiddel](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
