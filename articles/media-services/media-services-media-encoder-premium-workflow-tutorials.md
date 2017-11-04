---
title: Zelfstudies Avanced Media Encoder Premium werkstroom
description: Dit document bevat scenario's die laten zien hoe u geavanceerde taken met Media Encoder Premium werkstroom uitvoert en ook het complexe werkstromen maken met Workflow Designer.
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 565497bd5a35e3c4d69d29512307cf3ca2364bdd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Geavanceerde werkstroom voor Media Encoder Premium zelfstudies
## <a name="overview"></a>Overzicht
Dit document bevat scenario's die laten zien hoe u werkstromen met aanpassen **Workflow Designer**. U vindt de daadwerkelijke Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

## <a name="toc"></a>INHOUDSOPGAVE
De volgende onderwerpen komen aan bod:

* [MXF naar een single-bitrate MP4-codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Een nieuwe werkstroom starten](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Met behulp van de invoer van Media-bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Streaming media te bekijken](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Toevoegen van een video encoder voor. MP4-bestand genereren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [De audiostroom codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Audio en Video-streams multiplex in een MP4-container](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [Schrijven van het MP4-bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Een Media Services-activum maken van het bestand voor uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [De voltooide werkstroom lokaal testen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [MXF in multibitrate MP4s - codering dynamische pakketten ingeschakeld](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Toevoegen van een of meer extra MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [De bestandsnamen van de uitvoer configureren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Toevoegen van een afzonderlijke-nummer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [Toevoegen van de. ISM SMIL-bestand](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [MXF in multibitrate MP4 - verbeterde blauwdruk codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [Overzicht van de werkstroom te verbeteren](#workflow-overview-to-enhance)
  * [Naamgevingsregels voor bestanden](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [Eigenschappen van onderdeel naar de hoofdmap van de werkstroom publiceren](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Uitvoerbestand namen zijn afhankelijk van de gepubliceerde eigenschapswaarden hebt gegenereerd](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Miniaturen toe te voegen aan multibitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [Overzicht van de werkstroom miniatuurweergaven toevoegen](#workflow-overview-to-add-thumbnails-to)
  * [Toe te voegen JPG-codering](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [Omgaan met de conversie van de werkruimte](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [Schrijven van de miniatuurweergaven](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [Fouten opsporen in een werkstroom](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Werkstroom is voltooid](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Op basis van tijd worden ingekort door multibitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Overzicht van de werkstroom om toe te voegen bijsnijden naar](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Met behulp van de stroom Trimmer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Werkstroom is voltooid](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Introductie van het onderdeel voor scripts](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [Uitvoeren van scripts in een werkstroom: Hallo wereld](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [Op basis van het frame worden ingekort door multibitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Overzicht van blauwdruk om toe te voegen bijsnijden naar](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [Met behulp van de lijst Clip XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [De lijst clip van een onderdeel in een script vastgelegd wijzigen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [De eigenschap van een ClippingEnabled gemak toe te voegen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a id="MXF_to_MP4"></a>MXF naar een single-bitrate MP4-codering
In dit scenario maakt u een single bitrate. MP4-bestand met AAC HE gecodeerde audio van een. Het invoerbestand MXF.

### <a id="MXF_to_MP4_start_new"></a>Een nieuwe werkstroom starten
Workflow Designer openen en selecteer 'File '-' nieuwe werkruimte'-' transcoderen blauwdruk'

De nieuwe werkstroom wordt 3 elementen weergeven:

* Primaire bronbestand
* Clip XML-lijst
* Uitvoerasset bestand  

![Nieuwe codering werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Nieuwe codering werkstroom*

### <a id="MXF_to_MP4_with_file_input"></a>Met behulp van de invoer van Media-bestand
Om te kunnen accepteren onze invoer mediabestand, begint een met het Media bestand invoer onderdelen toevoegen. Een onderdeel toevoegen aan de werkstroom, zoekt in het zoekvak opslagplaats en sleep de gewenste vermelding naar het deelvenster ontwerpen. Dit doen voor de invoer Media-bestand en het onderdeel primaire bronbestand verbinding met de bestandsnaam invoer pincode van de invoer Media-bestand.

![Verbonden mediabestand invoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Verbonden mediabestand invoer*

Voordat we veel anders doen kunt, moet we eerst om aan te geven voor de workflow designer wat we willen graag voor het ontwerpen van de werkstroom met voorbeeldbestand. Om dit te doen, klikt u op de achtergrond van de ontwerpfunctie deelvenster en zoekt u naar de primaire bronbestand-eigenschap in het deelvenster met de rechter-eigenschap. Klik op het mappictogram en selecteer de gewenste bestand voor het testen van de werkstroom met. Zodra dit is gebeurd, wordt het onderdeel Media bestandsinvoer inspecteren van het bestand en bijbehorende pincodes uitvoer naar aanleiding van het bestand dat wordt geïnspecteerd vullen.

![Invoer van mediabestand ingevuld](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Invoer van mediabestand ingevuld*

Terwijl u hiermee met welke invoer willen we werken, niet duidelijk nog waarin de uitvoer van de gecodeerde moet gaat u naar. Vergelijkbaar met hoe de primaire bronbestand is geconfigureerd, de uitvoer map variabele eigenschap, net onder nu configureren.

![Geconfigureerde invoer en uitvoer eigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Geconfigureerde invoer en uitvoer eigenschappen*

### <a id="MXF_to_MP4_streams"></a>Streaming media te bekijken
Vaak is het gewenste weten hoe de stroom lijkt erop dat stromen via de werkstroom. Om te controleren op een stroom op elk gewenst moment in de werkstroom, klik op een uitvoer- of invoer van de pincode op een van deze onderdelen. In dit geval probeert te klikken op de pincode van de uitvoer niet-gecomprimeerde Video van onze invoer Media-bestand. Een dialoogvenster wordt geopend waarmee de uitgaande video controleren.

![De pincode van de uitvoer niet-gecomprimeerde Video bekijken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*De pincode van de uitvoer niet-gecomprimeerde Video bekijken*

In ons geval vertelt ons bijvoorbeeld dat we je omgaan met een invoer 1920 x 1080 op 24 frames per seconde in 4:2:2 steekproeven voor een video van bijna 2 minuten.

### <a id="MXF_to_MP4_file_generation"></a>Toevoegen van een video encoder voor. MP4-bestand genereren
Houd er rekening mee dat nu, een niet-gecomprimeerde Video's en meerdere niet-gecomprimeerde Audio-uitvoer pincodes zijn beschikbaar voor gebruik van onze invoer Media-bestand. De binnenkomende video codeert, moet een codering onderdeel - in dit geval voor het genereren van. MP4-bestanden.

Als u wilt coderen van de video-stream naar H.264 toevoegen het onderdeel AVC Video Encoder naar het ontwerpoppervlak. Dit onderdeel wordt een decomprimeren videostream als invoer en biedt een gecomprimeerde AVC-videostream op de pincode van de uitvoer.

![Niet-verbonden AVC-codering](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Niet-verbonden AVC-codering*

De eigenschappen kunt u bepalen hoe de codering precies wordt uitgevoerd. We hebben een overzicht van enkele van de belangrijkste instellingen:

* Uitvoer breedte en hoogte van de uitvoer: deze bepalen de resolutie van de gecodeerde video. In ons geval gaan we met 640 x 360
* Framesnelheid: als de waarde passthrough deze dan alleen neemt de framesnelheid bron is het mogelijk is dit echter negeren. Houd er rekening mee dat dergelijke framesnelheid conversie is niet beweging-gecompenseerd.
* Profiel en het niveau: deze bepalen de AVC-profiel en het niveau. U gemakkelijk kunt meer informatie over de verschillende niveaus en -profielen, klikt u op het vraagteken voor het onderdeel AVC Video Encoder en de help-pagina wordt meer details over elk van de niveaus weergeven. Voor onze voorbeeld we kiezen Main-profiel op niveau 3.2 (de standaardinstelling).
* Beoordeel besturingselement modus en Bitrate (kbps): in ons scenario we kiezen voor een constante uitvoer voor 1200 kbps bitrate (CBR)
* Video-indeling: dit is over de VUI (Video bruikbaarheid informatie) die in de stroom H.264 (side-informatie die mogelijk gebruikt door een decoder voor het verbeteren van de weergave, maar niet essentieel is voor het correct worden ontsleuteld) opgehaald geschreven:
* NTSC (standaard voor VS of Japan, met behulp van 30 fps)
* PAL (standaard voor Europa, met behulp van 25 fps)
* Modus van de grootte GOP: geconfigureerd vaste GOP grootte voor onze toepassing met een Interval van twee seconden van de sleutel met GOPs gesloten. Dit zorgt ervoor compatibiliteit met dynamische pakketten Azure Media Services biedt.

Voor de feed onze AVC-codering, verbinding maken met de pincode niet-gecomprimeerde Video-uitvoer van het onderdeel Media bestand invoer voor de invoer niet-gecomprimeerde Video-pincode van het coderingsprogramma AVC.

![Verbonden AVC-codering](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Verbonden AVC Main-codering*

### <a id="MXF_to_MP4_audio"></a>De audiostroom codering
Op dit moment hebben we video gecodeerd, maar de oorspronkelijke, niet-gecomprimeerde audiostroom nog moet worden gecomprimeerd. Voor dit gaan we AAC codering door het onderdeel AAC codering (Dolby). Deze toevoegen aan de werkstroom.

![Niet-verbonden AVC-codering](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Niet-verbonden AAC codering*

Er is een incompatibiliteit: Er is slechts één niet-gecomprimeerde audio-invoer pincode vanuit het coderingsprogramma AAC terwijl meer dan waarschijnlijk Media bestand invoer twee verschillende niet-gecomprimeerde audiostroom beschikbaar heeft: één voor het linkerdeelvenster audio kanaal en één voor het recht. (Als u bent omgaan met rond geluid, dat is 6 kanalen.) Het is dus niet mogelijk rechtstreeks verbinding maken de audio van de invoerbron van Media-bestand in de AAC audio-encoder. Het onderdeel AAC verwacht een zogenaamde 'interleaved' audiostroom: één stream met de linker- en de juiste kanalen interleaved met elkaar. Wanneer we weten uit onze bronbestand media wat zijn welke audio-nummers in welke positie in de bron kan er dergelijke interleaved audiostroom met de posities correct toegewezen spreker voor links of rechts gegenereerd.

Een wilt eerst een interleaved stroom gegenereerd op basis van de vereiste bron audio-kanalen. Het onderdeel Audio Stream Interleaver verwerken dit voor ons. Toe te voegen aan de werkstroom en verbinding maken met de audio-uitvoer van de invoer van Media-bestand in de App.

![Verbonden audiostroom Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Verbonden audiostroom Interleaver*

Nu dat we een interleaved audiostroom hebben, opgeven er nog steeds niet waar de linker- of spreker posities wilt toewijzen. Om dit opgeeft, kunnen we gebruikmaken van de spreker positie wel statusrapporten.

![Toevoegen van een spreker positie wel statusrapporten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Toevoegen van een spreker positie wel statusrapporten*

Configureer spreker positie wel statusrapporten voor gebruik met een aansluiting invoerstroom via een Encoder vooraf ingestelde Filter 'Custom' en het kanaal voorinstelling "2.0 (L, R)" genoemd. (Hierbij worden toegewezen de positie links spreker aan kanaal 1 en de juiste spreker positie aan 2-kanaal.)

De uitvoer van de spreker positie wel statusrapporten verbinding met de invoer van de Encoder AAC. Vervolgens geeft u het coderingsprogramma AAC voor gebruik met een '2.0 (L, R)' kanaal vooraf ingesteld, zodat deze te gaan met stereogeluid als invoer.

### <a id="MXF_to_MP4_audio_and_fideo"></a>Audio en Video-streams multiplex in een MP4-container
Gegeven onze AVC gecodeerde videostream en onze AAC gecodeerd audiostroom, kunnen we beide in vastleggen een. MP4-container. Het proces van de combinatie van verschillende stromen in een enkel heet 'multiplex' (of 'muxing'). We zijn in dit geval de audio en video stromen in één samenhangende interleaving. MP4-pakket. Het onderdeel waarmee u deze optie voor coördineert een. MP4-container wordt ISO MPEG-4 Multiplexer genoemd. Voeg een naar het ontwerpoppervlak en zowel de AVC Video-codering als het coderingsprogramma AAC verbinding met de ingevoerde gegevens.

![Verbonden MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Verbonden MPEG4 Multiplexer*

### <a id="MXF_to_MP4_writing_mp4"></a>Schrijven van het MP4-bestand
Bij het schrijven van een bestand voor uitvoer, wordt het onderdeel uitvoer in een bestand gebruikt. We kunnen verbinding maken dit met de uitvoer van de ISO MPEG-4 Multiplexer zodat de uitvoer wordt geschreven naar schijf. U doet dit door verbinding maken met de pincode van de uitvoer Container (MPEG-4) de schrijven invoer pincode van de uitvoer van het bestand.

![Verbonden uitvoer in een bestand](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Verbonden uitvoer in een bestand*

De bestandsnaam die wordt gebruikt, wordt bepaald door de eigenschap van het bestand. Die eigenschap kan zijn vastgelegd op een opgegeven waarde, wordt waarschijnlijk een wilt deze in te stellen via een expressie in plaats daarvan.

De werkstroom automatisch bepalen de uitvoer van de eigenschap name van een expressie-bestand, klikt u op de buton naast de naam van het bestand (naast het pictogram van de map). Selecteer vervolgens 'Expression' in de vervolgkeuzelijst. Er verschijnt nu de expressie-editor. Schakel eerst de inhoud van de editor.

![Lege expressie-Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Lege expressie-Editor*

De expressie-editor kan geen letterlijke waarde, gecombineerd met een of meer variabelen invoeren. Variabelen beginnen met een dollarteken. Als u de sleutel $ raakt, wordt de editor voor een vervolgkeuzelijst met een keuze van de beschikbare variabelen weergegeven. In ons geval gebruikt u een combinatie van de map uitvoervariabele en de variabele base invoerbestand naam:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Gevuld uit de expressie-Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Gevuld uit de expressie-Editor*

> [!NOTE]
> Om te zien van een bestand voor uitvoer van de coderingstaak in Azure, moet u een waarde in de expressie-editor opgeven.
>
>

Als u de expressie te raken ok bevestigt, wordt het eigenschappenvenster bekijken wat op dit moment waarde van het bestand eigenschap wordt omgezet.

![Expressie voor uitvoer dir wordt opgelost](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Expressie voor uitvoer dir wordt opgelost*

### <a id="MXF_to_MP4_asset_from_output"></a>Een Media Services-activum maken van het bestand voor uitvoer
Terwijl we een MP4-bestand voor uitvoer geschreven hebben, moet er nog steeds om aan te geven dat dit bestand hoort op de uitvoerasset die mediaservices wordt gegenereerd als gevolg van deze werkstroom wordt uitgevoerd. Hiertoe wordt het knooppunt Uitvoerasset bestand op het canvas werkstroom gebruikt. Alle binnenkomende bestanden in dit knooppunt maakt deel uit van het resulterende Azure Media Services actief zijn.

Verbinding met het maken van het onderdeel uitvoer in een bestand met het onderdeel Uitvoerasset bestand voor het voltooien van de werkstroom.

![Werkstroom is voltooid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Werkstroom is voltooid*

### <a id="MXF_to_MP4_test"></a>De voltooide werkstroom lokaal testen
Als u wilt de werkstroom lokaal testen, druk op de knop afspelen op de werkbalk bovenaan. Als de werkstroom uitgevoerd, Inspecteer de uitvoer die wordt gegenereerd in de geconfigureerde uitvoermap. Hier ziet u het voltooide MP4-uitvoerbestand dat uit het bestand van de invoerbron MXF is gecodeerd.

## <a id="MXF_to_MP4_with_dyn_packaging"></a>Codering MXF in MP4 - multibitrate dynamische pakketten ingeschakeld
In dit scenario maakt u een set van meerdere bitrate MP4-bestanden met AAC gecodeerde audio van één. Het invoerbestand MXF.

Wanneer een multi-bitrate asset uitvoer vereist is voor gebruik in combinatie met de functies voor dynamische pakketten die worden aangeboden door Azure Media Services, meerdere GOP uitgelijnde MP4-bestanden van elk een andere bitrate en de resolutie moet worden gegenereerd. Om dit te doen, de [MXF naar een single-bitrate MP4-codering](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) scenario bevat ons een goed uitgangspunt.

![Werkstroom starten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Werkstroom starten*

### <a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Toevoegen van een of meer extra MP4-uitvoer
Alle MP4-bestanden in onze resulterende Azure Media Services actief wordt ondersteuning voor een andere bitrate en resolutie. Stel een of meer MP4 uitvoerbestanden toevoegen aan de werkstroom.

Om ervoor te zorgen dat we hebben onze video encoders gemaakt met dezelfde instellingen, het gemakkelijkst de al bestaande AVC Video Encoder dupliceren en configureren van een andere combinatie van resolutie en bitrate (we toevoegen van een van de 960 x 540 op 25 frames per seconde met 2,5 Mbps ). Als u wilt de bestaande encoder dupliceren, kopie plakt u deze op het ontwerpoppervlak.

Verbinding maken met de pincode van de niet-gecomprimeerde Video-uitvoer van de invoer van de Media-bestand in onze nieuwe AVC-component.

![Tweede AVC encoder verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Tweede AVC encoder verbonden*

Nu aan te passen de configuratie voor onze nieuwe AVC-encoder naar uitvoer 960 x 540 met 2,5 Mbps. (Gebruik de eigenschappen 'uitvoer breedte', 'Uitvoer hoogte' en 'Bitrate (kbps)' voor deze.)

Opgegeven willen we gebruiken de resulterende asset samen met Azure Media Services dynamische pakketten, het streaming-eindpunt moet geschikt is voor het genereren van deze MP4-bestanden HLS/Fragmented MP4/DASH-fragmenten die exact zijn uitgelijnd met elkaar op een manier die clients die zijn schakelen tussen verschillende bitsnelheden ophalen uit een enkele smooth continue video en audio-ervaring. Als u dit kunt doen, moeten we om ervoor te zorgen dat de grootte voor beide MP4-bestanden in de eigenschappen van beide AVC coderingsprogramma's de GOP ('groep afbeeldingen') is ingesteld op 2 seconden, dat kan worden gedaan door:

* Als u de modus GOP formaat op vaste GOP grootte en
* het Interval van de Frame-sleutel op twee seconden.
* ook ingesteld worden gesloten GOP om alle GOP ervoor te zorgen dat het GOP IDR besturingselement permanent op hun eigen zonder afhankelijkheden

Als u onze workflow handig om te begrijpen, wijzig de naam de eerste AVC-encoder en ' AVC-Video Encoder 640 x 360 1200kbps ' en de tweede AVC-encoder ' AVC-Video Encoder 960 x 540 2500 kbps '.

Een tweede ISO MPEG-4 Multiplexer en de uitvoer van een tweede bestand toevoegen. De multiplexer verbinding met de nieuwe AVC-encoder en zorg ervoor dat de uitvoer naar de uitvoer van het bestand wordt geleid. Sluit ook de AAC audio encoder uitvoer naar de nieuwe multiplexer van invoer. De uitvoer van het bestand kunnen op zijn beurt vervolgens worden verbonden met het knooppunt Uitvoerasset bestand toe te voegen aan de Media Services-Asset die wordt gemaakt.

![Tweede mixer en uitvoer in een bestand verbonden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Tweede mixer en uitvoer in een bestand verbonden*

De multiplexer Chunk modus instelt op GOP aantal of de duur voor compatibiliteit met Azure Media Services dynamische pakketten, en de GOPs per segment ingesteld op 1. (Dit is de standaardwaarde.)

![Mixer Chunk modi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Mixer Chunk modi*

Opmerking: u mogelijk wilt dit proces herhalen voor alle andere bitrate resolutie combinaties en die u wilt hebt toegevoegd aan de uitvoer van de asset.

### <a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>De bestandsnamen van de uitvoer configureren
Er is meer dan één enkel bestand toegevoegd aan de uitvoerasset. Dit biedt moet zorg ervoor dat de namen van de bestanden voor elk van de uitvoerbestanden worden van elkaar verschillen en mogelijk zelfs toepassen van een naamgevingsconventie daarom het wissen van de bestandsnaam bent u omgaan is met.

Uitvoer benoemen kan worden beheerd via de expressies in de ontwerpfunctie. Open het eigenschappenvenster voor een van de onderdelen van de uitvoer in een bestand en de expressie-editor voor de eigenschap bestand openen. Ons eerste uitvoerbestand via de volgende expressie is geconfigureerd (Zie de zelfstudie voor het beste van [MXF naar een single-bitrate MP4-uitvoer](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Dit betekent dat onze filename wordt bepaald door twee variabelen: de uitvoermap om in te schrijven en base naam van het bronbestand. De systeemprestatiemeteritems als een eigenschap in de hoofdmap van de werkstroom en de laatste wordt bepaald door het inkomende bestand. Houd er rekening mee dat de uitvoermap die u gebruikt voor het lokale testen; Deze eigenschap wordt overschreven door de werkstroom-engine wanneer de werkstroom wordt uitgevoerd door de cloud-gebaseerde Mediaprocessor in Azure Media Services.
Als u ons uitvoerbestanden geven een consistente uitvoer naamgeving, wijzigen voor de eerste expressie voor de naamgeving van bestanden:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

en de tweede naar:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Uitvoeren van een tussenliggende test uitvoeren om ervoor te zorgen dat beide MP4-bestanden voor uitvoer correct worden gegenereerd.

### <a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Toevoegen van een afzonderlijke-nummer
Later zullen we zien wanneer er een ISM-bestand om te gaan met onze MP4-bestanden voor uitvoer gegenereerd, wordt ook moet een MP4-bestand alleen audio as-nummer voor onze adaptief streamen. Dit bestand maken, een extra mixer toevoegen aan de werkstroom (Multiplexer ISO-MPEG-4) en de AAC-encoder uitvoer pincode verbinden met de pincode van de invoer voor Track 1.

![Audio mixer toegevoegd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Audio mixer toegevoegd*

Maken van een derde onderdeel van de uitvoer in een bestand voor het uitvoeren van de uitgaande stroom van de mixer en de expressie als de naamgeving van bestanden configureren:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Audio mixer maken van de uitvoer van bestand](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Audio mixer maken van de uitvoer van bestand*

### <a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Toevoegen van de. ISM SMIL-bestand
Voor de dynamische pakketten werkt in combinatie met zowel MP4-bestanden (als de MP4 alleen audio) in onze asset Media Services, moet er ook een manifestbestand (ook wel een 'SMIL'-bestand: Synchronized Multimedia Integration Language). Dit bestand geeft u aan Azure Media Services welke MP4-bestanden zijn beschikbaar voor dynamische pakketten en welke van die in overweging moet nemen voor de audio-streaming. Een typische manifestbestand voor een set MP4 van met een enkele audiostroom ziet er als volgt:

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

De ISM-bestand bevat binnen een switch-instructie een verwijzing naar elk van de afzonderlijke video MP4-bestanden en naast ook een (of meer) audiobestand verwijzingen naar een MP4 alleen met de audio.

Genereren van het manifestbestand voor onze van MP4-set kan worden uitgevoerd via een onderdeel met de naam van de 'AMS Manifest Writer'. Als u wilt gebruiken, sleep deze naar het oppervlak en de 'Schrijven voltooid' uitvoer pincodes uit drie onderdelen van uitvoer in een bestand verbinding met de schrijver voor AMS Manifest-invoer. Controleer of de uitvoer van de schrijver AMS Manifest verbinding naar het bestand/Uitvoerasset.

Net als bij onze andere bestand uitvoer-onderdelen, configureren de uitvoernaam van de ISM-bestand met een expressie:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Onze klaar workflow lijkt op het onderstaande:

![Voltooide MXF aan multibitrate MP4-werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Voltooide MXF aan multibitrate MP4-werkstroom*

## <a id="MXF_to__multibitrate_MP4"></a>MXF in multibitrate MP4 - verbeterde blauwdruk codering
In de [vorige werkstroom procedure](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) we hebt gezien hoe een enkel MXF invoer actief kan worden geconverteerd naar een uitvoerasset met multi-bitrate MP4-bestanden, een alleen-audio MP4-bestand en een manifestbestand voor gebruik in combinatie met Azure Media Services dynamische pakketten.

In dit scenario wordt weergegeven hoe sommige van de aspecten kunnen worden verbeterd en eenvoudiger gemaakt.

### <a id="MXF_to_multibitrate_MP4_overview"></a>Overzicht van de werkstroom te verbeteren
![Werkstroom voor het verbeteren van Multibitrate MP4](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Werkstroom voor het verbeteren van Multibitrate MP4*

### <a id="MXF_to__multibitrate_MP4_file_naming"></a>Naamgevingsregels voor bestanden
In de vorige werkstroom we een eenvoudige expressie als basis voor het genereren van bestandsnamen uitvoer opgegeven. Hebben we sommige duplicatie echter: alle van de opgegeven dergelijke expressie van de onderdelen van afzonderlijke uitvoer-bestand.

Bijvoorbeeld: onze bestand output-component voor het eerste videobestand wordt geconfigureerd met deze expressie:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Wij hebben een expressie zoals terwijl voor de tweede de uitvoer van de video:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Zou niet het schonere minder fout foutgevoelige en gemakkelijker als we kan Verwijder enkele van deze duplicaten en dingen in plaats daarvan meer configureerbaar maken? Gelukkig kunnen we: de ontwerpfunctie expressie mogelijkheden in combinatie met de mogelijkheid te maken van aangepaste eigenschappen in de hoofdmap van de werkstroom Geef ons een extra beveiligingslaag gemak.

Stel, dat we je filename configuratie-station van de bitsnelheden van de afzonderlijke MP4-bestanden. Deze bitsnelheden die hebt streven we ernaar te configureren op één centrale plaats (in de hoofdmap van de grafiek) van waar ze moeten toegankelijk zijn voor het configureren en station bestandsnaam wordt gegenereerd. U doet dit door begin met het publiceren van de eigenschap bitrate van beide AVC coderingsprogramma's naar de hoofdmap van de werkstroom, zodat deze toegankelijk is vanaf zowel de basis ook vanuit de AVC coderingsprogramma's. (Zelfs als in twee verschillende plaatsen weergegeven, er is slechts één onderliggende waarde.)

### <a id="MXF_to__multibitrate_MP4_publishing"></a>Eigenschappen van onderdeel naar de hoofdmap van de werkstroom publiceren
De eerste AVC-encoder openen, gaat u naar de eigenschap Bitrate (kbps) en kies uit de vervolgkeuzelijst publiceren.

![De eigenschap bitrate publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*De eigenschap bitrate publiceren*

Het dialoogvenster publish publiceren naar de hoofdmap van de grafiek werkstroom configureren met de gepubliceerde naam 'video1bitrate' en de leesbare weergavenaam 'Bitrate Video 1'. Configureren van een aangepaste groepsnaam 'Bitsnelheden Streaming' genoemd en publiceren bereikt.

![De eigenschap bitrate publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Publishing dialoogvenster voor de eigenschap bitrate*

Herhaal dezelfde voor de eigenschap bitrate van de tweede AVC-encoder en noem deze 'video2bitrate' met de weergavenaam 'Video 2 Bitrate' in dezelfde aangepaste groep 'Bitsnelheden Streaming'.

Als we nu de eigenschappen van de basis-werkstroom controleren, zien we onze aangepaste groep met de twee gepubliceerde eigenschappen worden weergegeven. Beide zijn opgetreden bij het de waarde van hun respectieve AVC encoder bitrate weergeven.

![Eigenschappen van gepubliceerde bitrate op basis van de werkstroom](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Wanneer we toegang tot deze eigenschappen van code of van een expressie willen, kunnen we dit doen als volgt:

* vanuit Inlinecode van een onderdeel direct onder de hoofdmap: node.getPropertyAsString('.. / video1bitrate', null)
* in een expressie: ${ROOT_video1bitrate}

De groep 'Bitsnelheden Streaming' we met onze-nummer bitrate ook erop publiceren om te voltooien. In de eigenschappen van het coderingsprogramma AAC, zoekt u de instelling Bitrate en selecteer publiceren in de vervolgkeuzelijst naast. Publiceren naar de hoofdmap van de grafiek met de naam 'audio1bitrate' en naam "Audio 1 Bitrate" binnen onze aangepaste groep 'Bitsnelheden Streaming' weergeven.

![Publishing dialoogvenster voor audio bitrate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Publishing dialoogvenster voor audio bitrate*

![Resulterende video en audio-eigenschappen in de hoofdmap van](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Resulterende video en audio-eigenschappen in de hoofdmap van*

Houd er rekening mee dat als u een van deze drie wijzigt ook opnieuw configureert waarden en Hiermee wijzigt u de waarden op de betreffende onderdelen die zijn gekoppeld aan (en indien gepubliceerd vanuit).

### <a id="MXF_to__multibitrate_MP4_output_files"></a>Uitvoerbestand namen zijn afhankelijk van de gepubliceerde eigenschapswaarden hebt gegenereerd
In plaats van hardcoderen onze gegenereerde bestandsnamen we kunnen nu wijzigen onze expressie filename op elk van de onderdelen van de uitvoer in een bestand te vertrouwen op de bitrate-eigenschappen die we zojuist hebt gepubliceerd in de hoofdmap van de grafiek. Beginnen met onze eerste uitvoer in een bestand, de eigenschap van het bestand niet vinden en bewerken van de expressie als volgt:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

De andere parameters in deze expressie kunnen worden geopend en die zijn ingevoerd door de dollarteken op het toetsenbord ingedrukt terwijl u in het expressievenster roept. Een van de beschikbare parameters is onze video1bitrate-eigenschap die we eerder hebt gepubliceerd.

![Toegang tot de parameters in een expressie](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Toegang tot de parameters in een expressie*

Doe hetzelfde voor de bestandsuitvoer voor onze tweede video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

en voor de bestand met alleen audio-uitvoer:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Als we nu de bitrate voor een van de video of audio-bestanden wijzigen, de respectieve encoder opnieuw wordt geconfigureerd en de naamconventie bitrate-bestand wordt ingewilligd alle automatische.

## <a id="thumbnails_to__multibitrate_MP4"></a>Miniaturen toe te voegen aan multibitrate MP4-uitvoer
Een werkstroom die wordt gegenereerd vanaf [een multibitrate MP4-uitvoer van een invoer MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we nu zoeken in de miniaturen toe te voegen aan de uitvoer.

### <a id="thumbnails_to__multibitrate_MP4_overview"></a>Overzicht van de werkstroom miniatuurweergaven toevoegen
![Multibitrate MP4-werkstroom starten vanuit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4-werkstroom starten vanuit*

### <a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Toe te voegen JPG-codering
Het hart van onze miniaturen generatie worden het onderdeel JPG-Encoder kunnen uitvoer JPG-bestanden.

![JPG-codering](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG-codering*

We kan geen onze stroom niet-gecomprimeerde Video echter rechtstreeks verbinding uit de invoer van Media-bestand in het coderingsprogramma JPG. Deze verwacht in plaats daarvan afzonderlijke frames worden verstrekt. Dit we kunt doen via het onderdeel Video Frame-Gate.

![Verbinding maken met een frame-gate de JPG-encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Verbinding maken met een frame-gate de JPG-encoder*

De frame-gate om de zoveel seconden of frames kunt een video frame om door te geven. Het interval en het tijdstip waarop dit gebeurt offset is geconfigureerd in de eigenschappen.

![Eigenschappen van video Frame-Gate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Eigenschappen van video Frame-Gate*

We maken een miniatuur elke minuut door de modus in te stellen op tijd (seconden) en het Interval en 60.

### <a id="thumbnails_to__multibitrate_MP4_color_space"></a>Omgaan met de conversie van de werkruimte
Terwijl het logisch dat beide pincodes niet-gecomprimeerde Video van de frame-gate en de Media bestand-invoer kunnen nu worden verbonden lijkt, zou er een waarschuwing krijgen als we zou doen.

![Invoerkleur ruimte fout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Invoerkleur ruimte fout*

Dit komt doordat de manier waarop in welke kleur informatie wordt weergegeven in de oorspronkelijke onbewerkte niet-gecomprimeerde videostream, afkomstig zijn van onze MXF wijkt af van wat de JPG-Encoder verwacht. Meer specifiek, wordt een zogenaamde 'kleur ruimte' van 'RGB-' of 'Grijswaarden' verwacht stromen. Dit betekent dat binnenkomende videostream van de Video Frame-Gate moet een conversie met betrekking tot de kleurenruimte als eerste toegepast.

Sleep naar de werkstroom de kleur ruimte Converter - Intel en verbinden met onze frame-gate.

![Verbinding maken met een conversieprogramma voor kleur ruimte](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Verbinding maken met een conversieprogramma voor kleur ruimte*

Kies in het eigenschappenvenster de BGR 24-vermelding in de lijst met vooraf ingestelde.

### <a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Schrijven van de miniatuurweergaven
Verschillende op onze MP4-video, het onderdeel JPG-codering wordt uitvoerbestand meer dan één. Om door te gaan met dit, een onderdeel van de schrijver Scene zoeken JPG-bestand kan worden gebruikt: automatisch uitvoeren van de binnenkomende JPG miniaturen en de geschreven, elke bestandsnaam wordt voorafgegaan door een ander nummer. (Het getal dat het aantal seconden/eenheden in de stroom die de miniatuur is afkomstig van meestal aangeeft.)

![Introductie van de schrijver Scene zoeken JPG-bestand](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Introductie van de schrijver Scene zoeken JPG-bestand*

De eigenschap map uitvoerpad configureren met de expressie: ${ROOT_outputWriteDirectory}

en de eigenschap Filename voorvoegsel met:

    ${ROOT_sourceFileBaseName}_thumb_

Het voorvoegsel wordt bepaald hoe de miniatuur bestanden worden wordt genoemd. Ze wordt voorafgegaan door een getal dat aangeeft van de miniatuur positie in de stroom.

![Eigenschappen van de zoekopdracht JPG-bestand Writer Scene](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Eigenschappen van de zoekopdracht JPG-bestand Writer Scene*

Verbinding maken met de schrijver Scene zoeken JPG-bestand naar het knooppunt Uitvoerasset bestand.

### <a id="thumbnails_to__multibitrate_MP4_errors"></a>Fouten opsporen in een werkstroom
De invoer van de ruimte kleur converter verbinding met de onbewerkte niet-gecomprimeerde video-uitvoer. Nu een lokale test uitvoeren voor de werkstroom uitvoeren. Er is een goede kans dat de werkstroom plotseling wordt niet langer uitgevoerd en de wijzen op met een rode omtrek op het onderdeel dat is een fout opgetreden:

![Kleur ruimte Converter fout](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Kleur ruimte Converter fout*

Klik op het weinig red "E" pictogram in de rechterbovenhoek hoek van het onderdeel kleur ruimte Converter om te zien wat is de reden van de codering poging is mislukt.

![Kleur ruimte Converter foutberichtvenster](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Kleur ruimte Converter foutberichtvenster*

Het blijkt, zoals u kunt zien, dat de kleurruimte van de standaard voor de kleur ruimte converter binnenkomende moet rec601 voor onze aangevraagde conversie van YUV naar RGB heeft. Onze stroom wordt niet blijkbaar van rec601 aangegeven. (Rec 601 is een standaard voor het coderen van elkaar kruisende analoge video signalen in digitale video vorm. Hiermee wordt een actieve regio die betrekking hebben op 720 helderheid voorbeelden en 360 chrominantie voorbeelden per regel. De kleur system codering wordt ook wel YCbCr 4:2:2.)

U kunt dit verhelpen, geven we op de metagegevens van onze stroom die we je rec601 inhoud betreft. We gebruiken een Video gegevens Type Updater onderdeel, die we hebt gebracht tussen onze onbewerkte bron- en de component ruimte conversie om dit te doen. Dit type gegevens updater kunt u de handmatige update van bepaalde video gegevens type-eigenschappen. Configureren om aan te geven een kleur ruimte Standard van 'Rec 601'. Hierdoor wordt de Video gegevens Type Updater voor het taggen van de stroom met de 'Rec 601' kleurruimte als er geen kleurruimte nog gedefinieerd is. (Niet overschrijft alle bestaande metagegevens, tenzij u het selectievakje onderdrukking is ingeschakeld.)

![Kleur ruimte standaard op het Type gegevens Updater bijwerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Kleur ruimte standaard op het Type gegevens Updater bijwerken*

### <a id="thumbnails_to__multibitrate_MP4_finish"></a>Werkstroom is voltooid
Nu ons onze werkstroom is voltooid, doet u een andere test uitvoeren om het doorgeven te bekijken.

![Voltooide werkstroom voor meerdere mp4-uitvoer met miniaturen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Voltooide werkstroom voor meerdere mp4-uitvoer met miniaturen*

## <a id="time_based_trim"></a>Op basis van tijd worden ingekort door multibitrate MP4-uitvoer
Een werkstroom die wordt gegenereerd vanaf [een multibitrate MP4-uitvoer van een invoer MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we nu kijkt naar de bronvideo op basis van tijdstempels bijsnijden.

### <a id="time_based_trim_start"></a>Overzicht van de werkstroom om toe te voegen bijsnijden naar
![Werkstroom om toe te voegen bijsnijden te starten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Werkstroom om toe te voegen bijsnijden te starten*

### <a id="time_based_trim_use_stream_trimmer"></a>Met behulp van de stroom Trimmer
De component stroom Trimmer kunt wissen van het begin en einde van de invoerstroom op timing informatie (seconden, minuten,...). De trimmer biedt geen ondersteuning voor op basis van het frame-opmaak.

![Stroom Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Stroom Trimmer*

In plaats van rechtstreeks de AVC coderingsprogramma's en spreker positie wel statusrapporten koppelt aan de invoer van Media-bestand, moet we Between die de stroom trimmer plaatsen. (Één voor het signaal video en één voor het interleaved audio signaal).

![Stroom Trimmer tussen plaatsen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Stroom Trimmer tussen plaatsen*

Laten we de trimmer zodanig configureren dat we alleen video en audio tussen 15 en 60 seconden in de video wordt verwerkt.

Ga naar de eigenschappen van de Video stroom Trimmer en zowel de begintijd (15s) en de eigenschappen van de eindtijd (60s) configureren. Zorg ervoor dat zowel onze trimmer audio en video altijd naar dezelfde begin- en eindwaarde zijn geconfigureerd, komen we deze naar de hoofdmap van de werkstroom.

![Start tijdeigenschap van stroom Trimmer publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Start tijdeigenschap van stroom Trimmer publiceren*

![Dialoogvenster Eigenschappen voor de begintijd publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Dialoogvenster Eigenschappen voor de begintijd publiceren*

![Dialoogvenster Eigenschappen voor de eindtijd publiceren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Dialoogvenster Eigenschappen voor de eindtijd publiceren*

Als we nu de hoofdmap van de werkstroom controleren, wordt er door beide eigenschappen netjes weergegeven en kunnen worden geconfigureerd vanaf daar zijn.

![Gepubliceerde eigenschappen die beschikbaar zijn in de hoofdmap van](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Gepubliceerde eigenschappen die beschikbaar zijn in de hoofdmap van*

Nu de eigenschappen bijsnijden openen vanuit de audio trimmer en zowel begin- en eindtijden configureren met een expressie die naar de gepubliceerde eigenschappen in de hoofdmap van de werkstroom verwijst.

Voor de begintijd audio bijsnijden:

    ${ROOT_TrimmingStartTime}

en voor de eindtijd:

    ${ROOT_TrimmingEndTime}

### <a id="time_based_trim_finish"></a>Werkstroom is voltooid
![Werkstroom is voltooid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Werkstroom is voltooid*

## <a id="scripting"></a>Introductie van het onderdeel voor scripts
Script onderdelen kunnen willekeurige scripts uitvoeren tijdens de uitvoering fasen van de werkstroom. Er zijn vier verschillende scripts die kunnen worden uitgevoerd, elk met specifieke kenmerken en hun eigen plaats in de levenscyclus van werkstroom:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **lifeCycleScript**

De documentatie van het onderdeel in een script vastgelegd gaat in meer detail voor elk van de bovenstaande. In [in de volgende sectie](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), wordt de **realizeScript** scripting onderdeel wordt gebruikt voor het maken van een xml cliplist onderweg wanneer de werkstroom wordt gestart. Dit script wordt aangeroepen tijdens de installatie van onderdelen, die slechts één keer wordt uitgevoerd in de levenscyclus.

### <a id="scripting_hello_world"></a>Uitvoeren van scripts in een werkstroom: Hallo wereld
Sleep een onderdeel in een script vastgelegd naar het ontwerpoppervlak en wijzig de naam (bijvoorbeeld ' SetClipListXML').

![Script onderdelen toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Script onderdelen toevoegen*

Wanneer u de eigenschappen van het onderdeel in een script vastgelegd inspecteert, wordt de vier verschillende scripttypen worden weergegeven, elke configureerbare naar een ander script.

![Eigenschappen van scripts](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Eigenschappen van scripts*

Schakel de processInputScript en open de editor voor de realizeScript. We je nu ingesteld en klaar om te beginnen met scripting.

Scripts worden geschreven in Groovy, een dynamisch gecompileerde scripttaal die voor de Java-platform die compatibel is met Java behoudt. De meeste Java-code is daadwerkelijk, geldige Groovy code.

We gaan een eenvoudige hello world groovy script schrijven in de context van onze realizeScript. Voer de volgende in de editor:

    node.log("hello world");

Een lokale testuitvoering nu uitvoeren. Controleer na het uitvoeren (via het tabblad van het systeem voor het onderdeel in een script vastgelegd) de eigenschap Logboeken.

![Hello world logboekuitvoer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hello world logboekuitvoer*

Het knooppuntobject we de logboek-methode aanroept, verwijst naar onze huidige 'knooppunt' of het onderdeel dat we je scripting in. Elk onderdeel heeft als zodanig kunnen uitvoer logboekgegevens, beschikbaar via het tabblad systeem. In dit geval uitvoer we van de letterlijke tekenreeks "Hallo wereld". Belangrijk te begrijpen hier dat dit als een foutopsporing waardevol hulpmiddel bewijzen te, zonder dat u inzicht in wat het script daadwerkelijk doen.

Van in onze scriptomgeving hebben we ook toegang tot de eigenschappen van andere onderdelen. Probeer dit:

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

Onze logboekvenster beschrijven we het volgende:

![De uitvoer voor toegang tot de Knooppuntpaden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*De uitvoer voor toegang tot de Knooppuntpaden*

## <a id="frame_based_trim"></a>Op basis van het frame worden ingekort door multibitrate MP4-uitvoer
Een werkstroom die wordt gegenereerd vanaf [een multibitrate MP4-uitvoer van een invoer MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we nu kijkt naar de bronvideo op basis van het frame bijsnijden.

### <a id="frame_based_trim_start"></a>Overzicht van blauwdruk om toe te voegen bijsnijden naar
![Workflow om toe te voegen bijsnijden naar](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Workflow om toe te voegen bijsnijden naar*

### <a id="frame_based_trim_clip_list"></a>Met behulp van de lijst Clip XML
In alle vorige werkstroom zelfstudies gebruikt we het onderdeel Media bestand invoer als onze video-invoerbron. Voor dit specifieke scenario echter worden gebruikt het brononderdeel Clip lijst in plaats daarvan. Houd er rekening mee dat dit mag geen de beste manier van werken; de bron van de lijst Clip alleen gebruiken wanneer er een echte reden om dit te doen (zoals de hieronder geval waar we doorvoeren gebruikmaakt van de mogelijkheden van clip lijst bijsnijden).

Sleep het brononderdeel Clip lijst naar het ontwerpoppervlak van onze invoer Media-bestand naar de bron van de lijst Clip wilt overschakelen, en de pincode Clip lijst XML-verbinding met het Clip lijst XML-knooppunt van de workflow designer. Dit moet de bron van de lijst Clip met uitvoer pincodes, vullen volgens onze invoervideo. Nu verbinding maken met de niet-gecomprimeerde Video en Audio van niet-gecomprimeerde pincodes van de de bron van de lijst Clip naar de respectieve AVC coderingsprogramma's en Audio-stroom Interleaver. Nu de invoer van Media bestand verwijderen.

![De invoer van Media-bestand vervangen met de bron van de lijst Clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*De invoer van Media-bestand vervangen met de bron van de lijst Clip*

Het brononderdeel Clip lijst wordt als invoer een 'Clip lijst XML'. Wanneer u het bronbestand te testen met lokaal selecteert, is deze clip lijst xml automatisch ingevuld voor u.

![Automatisch ingevuld Clip lijst XML-eigenschap](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatisch ingevuld Clip lijst XML-eigenschap*

Zoek een beetje dichter bij de xml, is dit hoe het eruit:

![Dialoogvenster met de lijst clip bewerken](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Dialoogvenster met de lijst clip bewerken*

Dit komt echter niet overeen met de mogelijkheden van de xml van de lijst clip. Een optie die we hebben is het toevoegen van een 'Trim'-element in zowel de video en audio bron, als volgt:

![Een beperkende-element toe te voegen aan de lijst clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Een beperkende-element toe te voegen aan de lijst clip*

Als u de XML-lijst clip als volgt hierboven wijzigen en uitvoeren van een lokale test uitvoeren, ziet u de video correct zijn afgekapt tussen 10 en 20 seconden in de video.

In tegenstelling tot wat er gebeurt als u echter een lokale uitvoering doen, zou deze zeer dezelfde cliplist xml niet hetzelfde effect als toegepast in een werkstroom die wordt uitgevoerd in Azure Media Services hebben. Wanneer Azure Premium-codering wordt gestart, wordt de xml cliplist gegenereerd telkens opnieuw op basis van het bestand voor invoer dat de coderingstaak is opgegeven. Dit betekent dat eventuele wijzigingen die we op de xml doen helaas zou worden overschreven.

Om de cliplist xml wordt gewist wanneer een codeertaak wordt gestart, we kan opnieuw worden gegenereerd deze onderweg direct na het begin van de werkstroom. Deze aangepaste acties kunnen worden uitgevoerd via wat een 'in een script vastgelegd onderdeel' wordt genoemd. Zie voor meer informatie [introductie van het onderdeel in een script vastgelegd](media-services-media-encoder-premium-workflow-tutorials.md#scripting).

Sleep een onderdeel in een script vastgelegd naar het ontwerpoppervlak en wijzig deze in 'SetClipListXML'.

![Script onderdelen toevoegen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Script onderdelen toevoegen*

Wanneer u de eigenschappen van het onderdeel in een script vastgelegd inspecteert, wordt de vier verschillende scripttypen worden weergegeven, elke configureerbare naar een ander script.

![Eigenschappen van scripts](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Eigenschappen van scripts*

### <a id="frame_based_trim_modify_clip_list"></a>De lijst clip van een onderdeel in een script vastgelegd wijzigen
Voordat we de cliplist XML-code die wordt gegenereerd tijdens het opstarten van de werkstroom opnieuw schrijven kunt, moet we toegang hebben tot de eigenschap cliplist-xml en inhoud. We kunnen dit doen als volgt:

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Binnenkomende clip lijst aan te melden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Binnenkomende clip lijst aan te melden*

Er moet eerst een manier om te bepalen van welke dat tot dat moment willen we de video knippen. Om dit handige aan de gebruiker minder technische van de werkstroom, twee eigenschappen te publiceren naar de hoofdmap van de grafiek. U doet dit door het ontwerpoppervlak Klik met de rechtermuisknop en selecteer 'Eigenschap toevoegen':

* Eerste eigenschap: 'ClippingTimeStart' van het type: 'TIJDCODE'
* Tweede eigenschap: 'ClippingTimeEnd' van het type: 'TIJDCODE'

![Dialoogvenster van eigenschappen toevoegen voor de begintijd knippen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Dialoogvenster van eigenschappen toevoegen voor de begintijd knippen*

![Gepubliceerde eigenschappen van de tijd knipsel in de hoofdmap van de werkstroom van](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Gepubliceerde eigenschappen van de tijd knipsel in de hoofdmap van de werkstroom van*

Beide eigenschappen om een geschikte waarde te configureren:

![Configureren van het begin knippen en eindigen van eigenschappen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Configureren van het begin knippen en eindigen van eigenschappen*

Nu uit in onze script we hebben toegang tot beide eigenschappen, als volgt:

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Venster van het logboek met begin en einde van knippen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Venster van het logboek met begin en einde van knippen*

Laten we de tekenreeksen tijdcode parseren naar een handiger om te gebruiken met behulp van een eenvoudige reguliere expressie:

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

![Het venster met de uitvoer van de geparseerde tijdcode](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Het venster met de uitvoer van de geparseerde tijdcode*

Met deze informatie op dat moment kunnen we nu de xml cliplist zodat de begin- en eindtijden voor gewenste frame nauwkeurige knipsel van de film wijzigen.

![Scriptcode toevoegen van elementen knippen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Scriptcode toevoegen van elementen knippen*

Dit is gedaan door de normale tekenreeks bestandsbewerkingen. De resulterende gewijzigde illustratie lijst xml teruggeschreven naar de eigenschap clipListXML in de hoofdmap van de werkstroom met de methode 'setProperty'. Het logboekvenster na het uitvoeren van een andere test zou laat zien dat het volgende:

![De resulterende clip lijst logboekregistratie](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*De resulterende clip lijst logboekregistratie*

Voer een testuitvoering om te zien hoe de video en audio-stromen hebben is afgekapt. Als u meer dan één testuitvoering met verschillende waarden voor de punten bijsnijden doet, ziet u dat deze wordt geen rekening worden gehouden echter! De reden hiervoor is dat de ontwerpfunctie, in tegenstelling tot de Azure-runtime niet de xml cliplist elke uitvoeren overschrijven. Dit betekent dat alleen de eerste keer u hebt ingesteld de in- en out verwijst, de xml wordt te transformeren, alle andere tijden, onze component guard (als (clipListXML.indexOf ('<trim>') == van -1)) verhindert dat de werkstroom voor het toevoegen van een ander trim element wanneer Er is al aanwezig.

Om ervoor onze workflow handige lokaal testen, toevoegen we best house-keeping code die inspecteert als een beperkende element aanwezig was. Als dit het geval is, kunnen we het verwijderen voordat u doorgaat met het wijzigen van de xml met de nieuwe waarden. In plaats van gewone tekenreeksmanipulatie, is het waarschijnlijk veiliger om dit te doen via echte xml-objectmodel parseren.

Voordat we deze code toebehoort echter toevoegen kunt, moet er een aantal importinstructies eerst toevoegen aan het begin van het script:

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

Hierna kunnen we de vereiste reinigingstape code toevoegen:

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

Deze code gaat net boven het punt waarop we de beperkende elementen aan de cliplist-xml toevoegen.

We kunnen op dit moment worden uitgevoerd en onze werkstroom zoals net als we willen dat bij dat de wijzigingen toegepast ooit tijd wijzigen.    

### <a id="frame_based_trim_clippingenabled_prop"></a>De eigenschap van een ClippingEnabled gemak toe te voegen
Als u niet altijd bijsnijden wilt uitgevoerd, gaan we voltooien uit onze werkstroom door een handige Booleaanse vlag die aangeeft of we inschakelen knippen willen / knipsel toe te voegen.

Publiceren voordat, net zoals een nieuwe eigenschap aan de hoofdmap van onze werkstroom aangeroepen 'ClippingEnabled' type 'Boolean'.

![Een eigenschap voor het inschakelen van knipsel gepubliceerd](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Een eigenschap voor het inschakelen van knipsel gepubliceerd*

Met de onderstaande eenvoudige guard component, kunnen we controleren of bijsnijden vereist is en besluit als onze lijst clip als zodanig worden gewijzigd moet of niet.

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <a id="code"></a>Volledige code
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


## <a name="also-see"></a>Zie ook
[Inleiding tot Premium codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Het gebruik van Premium codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Codering van inhoud op aanvraag met Azure mediaservice](media-services-encode-asset.md#media-encoder-premium-workflow)

[Indelingen en codecs voor Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md)

[Werkstroom voorbeeldbestanden](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Explorer-hulpprogramma](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
