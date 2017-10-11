---
title: Azure Media Services tot gefragmenteerde MP4 live opnemen specificatie | Microsoft Docs
description: Deze specificatie beschrijft het protocol en de indeling voor gefragmenteerde MP4 gebaseerde live streaming opname voor Azure Media Services. U kunt Azure Media Services gebruiken om te streamen live gebeurtenissen en inhoud in realtime uitzenden met behulp van Azure als het cloudplatform. Dit document wordt ook beschreven best practices voor het bouwen van maximaal redundante en robuuste live mechanismen opnemen.
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 6250b73504bec765b8299060a29e84e771791cc9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services tot gefragmenteerde MP4 live opnemen specificatie
Deze specificatie beschrijft het protocol en de indeling voor gefragmenteerde MP4 gebaseerde live streaming opname voor Azure Media Services. Media Services biedt een service voor live streamen waarmee klanten kunnen stream live gebeurtenissen en de inhoud in realtime uitzenden met behulp van Azure als het cloudplatform. Dit document wordt ook beschreven best practices voor het bouwen van maximaal redundante en robuuste live mechanismen opnemen.

## <a name="1-conformance-notation"></a>1. Conformiteit notatie
De sleutel 'Moet' 'Moet geen,' 'Is vereist,' "Geldt," "wordt niet," 'SHOULD', 'Mag niet' woorden 'Aanbevolen,' 'Kunnen' en 'Optioneel' in dit document moeten worden geïnterpreteerd als ze worden beschreven in RFC 2119.

## <a name="2-service-diagram"></a>2. Diagram van de service
Het volgende diagram toont de architectuur op hoog niveau van de service voor live streamen in Media Services:

1. Een live coderingsprogramma duwt live-feeds kanalen die zijn gemaakt en zijn ingericht via Azure Media Services SDK.
2. Kanalen, programma's en streaming-eindpunten in Media Services verwerkt alle functionaliteiten voor live streamen, waaronder opnemen, opmaken, DVR, beveiliging, schaalbaarheid en redundantie cloud.
3. Klanten kunnen eventueel kiezen voor het implementeren van een laag Azure Content Delivery Network tussen het streaming-eindpunt en de clienteindpunten.
4. Client-eindpunten stroom van het streaming-eindpunt met behulp van protocollen HTTP adaptief streamen. Voorbeelden zijn Microsoft Smooth Streaming, dynamische adaptief streamen via HTTP (DASH of MPEG-DASH) en Apple HTTP Live Streaming (HLS).

![stroom opnemen][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. De indeling bitstream – ISO 14496 12 gefragmenteerd MP4
Draadindeling voor live streamen opnemen die worden beschreven dit document is gebaseerd op [ISO 14496 12]. Zie voor een gedetailleerde uitleg van gefragmenteerde MP4-indeling en extensies zowel voor video-on-demand bestanden en live streaming opname [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx).

### <a name="live-ingest-format-definitions"></a>Indeling definities voor Live opnemen
De volgende lijst bevat speciale indeling definities die van toepassing op live in Azure Media Services opnemen:

1. De **ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken bij elke aanvraag (HTTP POST) moeten worden verzonden. Deze vakken aan het begin van de stroom moeten worden verzonden en elk gewenst moment de encoder moet opnieuw verbinding maken om te hervatten stroom opnemen. Zie de sectie 6 in [1] voor meer informatie.
2. Sectie 3.3.2 in [1] definieert een optioneel het zogenaamde **StreamManifestBox** voor live opnemen. Vanwege de routering logica van de Azure load balancer is met behulp van dit vak afgeschaft. Het selectievakje niet moet aanwezig zijn bij het opnemen van Media Services. Als dit selectievakje aanwezig is, Media Services achtergrond deze genegeerd.
3. De **TrackFragmentExtendedHeaderBox** vak gedefinieerd in 3.2.3.2 in [1] moet aanwezig zijn voor elke fragment.
4. Versie 2 van de **TrackFragmentExtendedHeaderBox** vak moet worden gebruikt voor het genereren van mediasegmenten die identiek URL's in meerdere datacenters hebben. Het veld fragment index is REQUIRED voor cross-datacenter failover van een index gebaseerde streaming-indelingen zoals Apple HLS en MPEG-DASH op basis van een index. Om cross-datacenter failover, wordt de index fragment moet worden gesynchroniseerd tussen meerdere coderingsprogramma's en worden verhoogd met 1 voor elke fragment opeenvolgende media, zelfs in encoder opnieuw wordt opgestart of storingen.
5. Sectie 3.3.6 in [1] definieert een invoervak **MovieFragmentRandomAccessBox** (**mfra**) die aan het einde van live opname worden verzonden om aan te geven einde van stroom (EOS) naar het kanaal. Als gevolg van de logica opnemen van Media Services, met behulp van EOS is afgeschaft, en de **mfra** vak voor live opname niet moet worden verzonden. Als verzonden, Media Services achtergrond deze genegeerd. Als u de status van het punt opnemen herstellen, wordt aangeraden dat u [kanaal opnieuw](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels). Wordt ook aangeraden dat u [programma stoppen](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) naar einde van een presentatie en de stream.
6. De duur van MP4-fragment moet constant, de grootte van de clientmanifesten verminderen. Een constante MP4 verbetert ook methodiek voor client downloaden met behulp van het fragment duur Herhaal labels. De duur kan om te compenseren voor niet-integere framesnelheid variëren.
7. De duur van MP4-fragment moet liggen tussen ongeveer 2 en 6 seconden.
8. MP4 fragment tijdstempels en indexen (**TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` en `fragment_index`) moeten in oplopende volgorde binnenkomen. Hoewel Media Services flexibel omgaan met dubbele fragmenten, heeft dit dan beperkt de mogelijkheid om opnieuw te rangschikken fragmenten volgens de planning van de media.

## <a name="4-protocol-format--http"></a>4. Indeling protocol: HTTP
ISO gefragmenteerd MP4 gebaseerde live opnemen voor Media Services maakt gebruik van een standaard langlopende HTTP POST-aanvraag voor het verzenden van mediagegevens van het gecodeerde die wordt verpakt in gefragmenteerde MP4-indeling met de service. Elke HTTP POST verstuurt een complete gefragmenteerd MP4 bitstream ('stream'), vanaf het begin met header vakken (**ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken), en wilt doorgaan met een reeks fragmenten (**moof** en **mdat** vakken). Zie de sectie 9.2 in [1] voor URL-syntaxis voor de HTTP POST-aanvraag. Een voorbeeld van de POST-URL is: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Vereisten
Hier volgen de gedetailleerde vereisten:

1. Het coderingsprogramma moet eerst de uitzending verzenden van een HTTP POST-aanvraag met een leeg 'hoofdtekst' (nul inhoudslengte) met behulp van dezelfde URL voor de opname. Zo kunt u het coderingsprogramma snel detecteren of het eindpunt live opname geldig is en als er verificatie of andere voorwaarden. Per HTTP-protocol, kan niet de server teruggestuurd een HTTP-antwoord totdat de gehele request, met inbegrip van de hoofdtekst van bericht is ontvangen. Gezien de langlopende aard van een live gebeurtenis, zonder deze stap wordt het coderingsprogramma niet mogelijk voor het detecteren van alle fouten, totdat het klaar is met het verzenden van de gegevens.
2. Het coderingsprogramma moet eventuele fouten of verificatiecontroles verwerken vanwege (1). Als (1) is voltooid met een 200 antwoord worden voortgezet.
3. Het coderingsprogramma moet een nieuwe HTTP POST-aanvraag beginnen met de gefragmenteerde MP4-stream. De nettolading moet beginnen met de vakken koptekst, gevolgd door fragmenten. Houd er rekening mee dat de **ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken (in deze volgorde) moeten worden verzonden met elke aanvraag, zelfs als het coderingsprogramma opnieuw verbinding maken moet omdat het eerdere verzoek voorafgaand aan het einde van de stroom is beëindigd. 
4. Het coderingsprogramma moet gedeelde codering voor het uploaden, omdat het onmogelijk om te voorspellen van de hele lengte van de inhoud van de live gebeurtenis is voor overdracht gebruiken.
5. Wanneer de gebeurtenis is afgelopen, na het verzenden van het laatste fragment moet het coderingsprogramma probleemloos gesegmenteerde overdrachtscodering berichtenstroom (de meeste HTTP-client stacks verwerkt; het automatisch) eindigen. Het coderingsprogramma moet wachten op de service te retourneren van de laatste antwoordcode en vervolgens de verbinding verbreken. 
6. Het coderingsprogramma niet moet de `Events()` zelfstandig naamwoord zoals beschreven in 9.2 in [1] voor live opname in Media Services.
7. Als de HTTP POST-aanvraag wordt beëindigd of met een TCP-fout voorafgaand aan het einde van de stroom een optreedt time-out, moet het coderingsprogramma uitgeven van een nieuwe POST-aanvraag met behulp van een nieuwe verbinding en volg de bovenstaande vereisten. Het coderingsprogramma moet bovendien opnieuw verzenden van de vorige twee MP4 fragmenten voor elk nummer in de stroom en hervatten zonder een onderbreking in de tijdlijn van media. Opnieuw verzenden van de laatste twee MP4-fragmenten voor elk nummer zorgt ervoor dat er geen gegevens verloren gaan. Met andere woorden, als een stroom geen audio en video bijgehouden bevat en de huidige POST-aanvraag is mislukt, moet de encoder opnieuw verbinding maken en verzenden van de laatste twee fragmenten voor de audio bijhouden die eerder met succes zijn verzonden, en de laatste twee fragmenten voor de video bijhouden die eerder met succes zijn verzonden, om ervoor te zorgen dat er geen gegevensverlies is. Het coderingsprogramma moet onderhouden een "forward" buffer van media fragmenten die het opnieuw verzendt wanneer verbinding wordt gemaakt.

## <a name="5-timescale"></a>5. Tijdschaal
[[MS-SSTR] ](https://msdn.microsoft.com/library/ff469518.aspx) beschrijft het gebruik van tijdschaal voor **SmoothStreamingMedia** (sectie 2.2.2.1) **StreamElement** (sectie 2.2.2.3) **StreamFragmentElement** (sectie 2.2.2.6) en **LiveSMIL** (sectie 2.2.7.3.1). Als de waarde tijdschaal niet aanwezig is, is de standaardwaarde gebruikt 10,000,000 (10 MHz). Hoewel de indelingsspecificatie Smooth Streaming wordt geen informatie over het gebruik van andere waarden tijdschaal geblokkeerd, de meeste implementaties van het coderingsprogramma gebruikt deze standaard waarde (10 MHz) voor het genereren van Smooth Streaming opnemen van gegevens. Vanwege de [Azure Media dynamische pakketten](media-services-dynamic-packaging-overview.md) functie, het is raadzaam een tijdschaal 90 KHz te gebruiken voor video stromen en 44,1 of 48.1 KHz voor audio stromen. Als andere tijdschaal waarden worden gebruikt voor verschillende stromen, kan de stroom-niveau tijdschaal moet worden verzonden. Zie voor meer informatie [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. Definitie van 'stream'
Stroom is de basiseenheid voor bewerking in live opname voor het opstellen van live presentaties verwerken streaming failover en redundantie scenario's. Stroom wordt gedefinieerd als een unieke, gefragmenteerde MP4-bitstream met één track of meerdere nummers. Een volledige live presentatie kan een of meer stromen, afhankelijk van de configuratie van de live coderingsprogramma's bevatten. De volgende voorbeelden ziet diverse opties van het gebruik van streams voor het opstellen van een volledige live presentatie.

**Voorbeeld:** 

Een klant wil een live streaming presentatie met de volgende audio/video-bitsnelheden maken:

Video – 3000 kbps, 1500 kbps, 750 kbps

Audio-128 k

### <a name="option-1-all-tracks-in-one-stream"></a>Optie 1: Alle nummers in één stroom
Bij deze optie geen enkele coderingsprogramma genereert alle audio/video-nummers en verzamelt ze in één gefragmenteerde MP4 bitstream. De gefragmenteerde MP4-bitstream wordt verzonden via één HTTP POST-verbinding. In dit voorbeeld is er slechts één stream voor deze live presentatie.

![Streams-een bijhouden][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>Optie 2: Elk nummer in een afzonderlijke stream
Bij deze optie het coderingsprogramma plaatst een nummer in elke bitstream fragment MP4 en plaatst alle stromen via afzonderlijke HTTP-verbindingen. Dit kan worden gedaan met een coderingsprogramma of met meerdere coderingsprogramma's. De live opname ziet deze live presentatie als bestaat uit vier stromen.

![Streams afzonderlijke houdt][image3]

### <a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Optie 3: Audio bijhouden met de laagste bitrate video spoor bundelen in één stream
Bij deze optie kiest de klant de audio bijhouden met de laagste bitrate video bijhouden bundelen in één MP4-fragment bitstream, en laat de andere twee video sporen als afzonderlijke stromen. 

![Streams-audio en video-nummers][image4]

### <a name="summary"></a>Samenvatting
Dit is geen uitputtende lijst van alle mogelijke opname opties voor dit voorbeeld. Als een sterker nog, wordt een groepering van nummers in streams ondersteund door live opname. Klanten en leveranciers encoder kunt hun eigen implementaties op basis van de technische complexiteit, encoder-capaciteit en redundantie en overwegingen. In de meeste gevallen is er echter slechts één-nummer voor de hele live presentatie. Het is dus belangrijk om te controleren of de gezondheid van de stroom opnemen met de-nummer. Dit aandachtspunt vaak resulteert in het nummer in een eigen stroom (zoals in de optie 2) of het bundeling deze met de laagste bitrate video bijhouden (zoals in de optie 3). Ook, voor betere redundantie en fouttolerantie, verzendt de dezelfde audio bijhouden in twee verschillende stromen (optie 2 met redundant audio houdt) of de audio bijhouden met ten minste twee van de laagste bitrate video-nummers (optie 3 met audio gebundeld in ten minste twee video stromen) bundeling wordt nadrukkelijk aanbevolen voor live opnemen in Media Services.

## <a name="7-service-failover"></a>7. Failover van de service
Gegeven de aard van live streamen, zijn goede failover-ondersteuning is essentieel voor de beschikbaarheid van de service. Media Services is ontworpen voor het verwerken van verschillende typen fouten, waaronder netwerkfouten, server-fouten en opslagproblemen. Wanneer gebruikt in combinatie met de juiste failoverlogica vanaf de live codering, kunnen klanten een uiterst betrouwbare service voor live streamen vanuit de cloud kunnen bereiken.

In deze sectie bespreken we service failover-scenario's. In dit geval wordt de fout gebeurt er ergens in de service en het doet zich voor als een netwerkfout opgetreden. Hier volgen enkele aanbevelingen voor de uitvoering van de codering voor het verwerken van de service failover:

1. Gebruik een 10 seconden time-out voor de TCP-verbinding tot stand brengen. Als een poging de verbinding tot stand brengen langer dan 10 seconden duurt, afbreken van de bewerking en probeer het opnieuw. 
2. Gebruik een korte time-out voor het verzenden van bericht segmenten van de HTTP-aanvraag. Als de doel-MP4-fragment duur N seconden, gebruikt u de time-out voor een verzenden tussen N en 2 N seconden; Als de duur van MP4-fragment 6 seconden, gebruikt u bijvoorbeeld een time-out van 6 tot en met 12 seconden. Als er een time-out optreedt, opnieuw instellen van de verbinding en opent u een nieuwe verbinding hervatten stroom opnemen op de nieuwe verbinding. 
3. Houd een rolling buffer die is de laatste twee fragmenten voor elk nummer dat correct en volledig naar de service verzonden is.  Als de HTTP POST-aanvraag voor een stroom wordt beëindigd of time-out voorafgaand aan het einde van de stroom, opent u een nieuwe verbinding en beginnen met een andere HTTP POST-aanvraag, opnieuw verzenden van de stroom-headers, opnieuw verzenden van de laatste twee fragmenten voor elk nummer en hervatten van de stroom zonder een onderbreking in de tijdlijn media. Dit vermindert de kans op verlies van gegevens.
4. U wordt aangeraden dat het coderingsprogramma geen beperking is het aantal nieuwe pogingen om een verbinding tot stand brengen of hervatten streaming nadat een TCP-fout is opgetreden.
5. Nadat een TCP-fout:
  
    a. De huidige verbinding moet worden gesloten en een nieuwe verbinding moet worden gemaakt voor een nieuwe HTTP POST-aanvraag.

    b. De nieuwe HTTP POST-URL moet hetzelfde zijn als de oorspronkelijke POST-URL.
  
    c. De nieuwe HTTP POST moet bestaan uit kopteksten stream (**ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken) die identiek zijn aan de kopteksten stream in het eerste bericht.
  
    d. De laatste twee fragmenten verzonden voor elk nummer moeten opnieuw worden verzonden en streaming moet hervat zonder een onderbreking in de tijdlijn van media. De tijdstempels MP4-fragment toenemen continu, zelfs in een HTTP POST-aanvragen.
6. Het coderingsprogramma moet de HTTP POST-aanvraag beëindigd als er gegevens worden niet verzonden met een frequentie in overeenstemming met de duur van MP4-fragment.  Een HTTP POST-aanvraag die geen gegevens verzendt kunt voorkomen dat Media Services de encoder snel verbreken in het geval van een service-update. Daarom moet het HTTP POST-protocol voor sparse (ad signaal) houdt moeten worden kortstondig wordt beëindigd zodra de sparse fragment wordt verzonden.

## <a name="8-encoder-failover"></a>8. codering-failover
Failover van de codering is het tweede type van failover-scenario dat moet worden opgelost voor de levering van end-to-end-live-streaming. In dit scenario wordt optreedt de fout aan de kant van de encoder. 

![codering-failover][image5]

De volgende verwachtingen van het eindpunt live opname van toepassing wanneer encoder failover gebeurt:

1. Een nieuw exemplaar van de codering moet worden gemaakt om door te gaan streaming, zoals geïllustreerd in het diagram (Stream voor 3000k video met stippellijn).
2. De nieuwe encoder moet dezelfde URL gebruiken voor HTTP POST-aanvragen als de instantie is mislukt.
3. De nieuwe encoder POST-aanvraag moet opnemen met de dezelfde gefragmenteerde MP4-header vakken als de instantie is mislukt.
4. De nieuwe encoder moet juist worden gesynchroniseerd met alle andere actieve coderingsprogramma's voor dezelfde live presentatie gesynchroniseerde audio/video-voorbeelden met uitgelijnde fragment grenzen genereren.
5. De nieuwe stroom moet semantisch equivalent met de vorige stroom en uitwisselbaar op het niveau van-header en het fragment.
6. De nieuwe encoder moet proberen om gegevensverlies te minimaliseren. De `fragment_absolute_time` en `fragment_index` Media fragmenten vanaf het punt waar de encoder laatste gestopt moeten verhogen. De `fragment_absolute_time` en `fragment_index` in een ononderbroken wijze moet verhogen, maar is het toegestaan om te introduceren een onderbreking, indien nodig. Media Services negeert fragmenten dat al is ontvangen en verwerkt, dus is het beter err naast fragmenten dan introduceren aan onderbrekingen in de tijdlijn media opnieuw te verzenden. 

## <a name="9-encoder-redundancy"></a>9. codering redundantie
Voor bepaalde kritieke live gebeurtenissen dat verzoek zelfs hogere beschikbaarheid en kwaliteit vereisen, we u actieve redundante coderingsprogramma's gebruiken raden voor een naadloze failover zonder verlies van gegevens.

![codering redundantie][image6]

Zoals wordt geïllustreerd in dit diagram, push twee groepen coderingsprogramma's twee exemplaren van elke stroom tegelijkertijd naar de service voor live. Deze instelling wordt ondersteund, omdat Media Services dubbele fragmenten op basis van de stroom-ID en het fragment timestamp uitfilteren kunt. De resulterende livestream en -archief is één exemplaar van de stromen die de best mogelijke aggregatie van de twee bronnen. Bijvoorbeeld in een hypothetische extreme aanvraag, zolang er is een coderingsprogramma (deze heeft geen moet hetzelfde account) uitgevoerd op elk gewenst moment in de tijd van elke stroom, de resulterende live stream van de service is continue zonder gegevensverlies. 

De vereisten voor dit scenario zijn bijna gelijk zijn aan de vereisten in het geval 'Encoder failover' met de uitzondering die de tweede reeks coderingsprogramma's worden uitgevoerd op hetzelfde moment als de primaire coderingsprogramma's.

## <a name="10-service-redundancy"></a>10. redundantie van de service
Voor maximaal redundante globale distributie, moet u soms regio-overschrijdende back-up voor het afhandelen van regionale noodsituaties hebben. Uitbreiden van de topologie 'Encoder redundantie', kunnen klanten kiezen om een redundante service-implementatie in een andere regio die verbonden met de tweede reeks coderingsprogramma's. Klanten kunnen ook werken met een Content Delivery Network-provider voor het implementeren van een globale Traffic Manager voor de twee implementaties naadloos om clientverkeer te routeren. De vereisten voor het coderingsprogramma's zijn hetzelfde als de aanvraag 'Encoder redundantie'. De enige uitzondering hierop is dat de tweede set worden waarnaar wordt verwezen naar een andere live coderingsprogramma's moet eindpunt opnemen. Het volgende diagram ziet u deze installatie:

![redundantie van de service][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Speciale soorten opname-indelingen
Deze sectie wordt beschreven speciale soorten live opname-indelingen die zijn ontworpen voor het afhandelen van specifieke scenario's.

### <a name="sparse-track"></a>Sparse bijhouden
Wanneer u een live streaming presentatie met een interactieve ervaring, vaak het nodig is voor het verzenden van gebeurtenissen die tijd zijn gesynchroniseerd of signalen in-band met de belangrijkste mediagegevens. Een voorbeeld hiervan is dynamische live ad invoegen. Dit type gebeurtenis-signalering wijkt af van reguliere audio/video-streaming vanwege de sparse aard. Met andere woorden, de signalering gegevens meestal gebeurt niet continu en het interval kan zijn moeilijk te voorspellen. Het concept van sparse bijhouden is ontworpen voor opnemen en signalering van gegevens in-band-broadcast.

De volgende stappen zijn een aanbevolen implementatie voor het opnemen van sparse bijhouden:

1. Maak een afzonderlijke gefragmenteerde MP4 bitstream met sparse houdt, zonder audio/video houdt.
2. In de **vak Server Manifest Live** zoals gedefinieerd in de sectie 6 in [1], gebruiken de *parentTrackName* parameter om de naam van de bovenliggende bijhouden. Zie voor meer informatie de sectie 4.2.1.2.1.2 in [1].
3. In de **vak Server Manifest Live**, **manifestOutput** moet worden ingesteld op **true**.
4. Gezien de sparse aard van de gebeurtenis signalering, raden we het volgende:
   
    a. Aan het begin van de live gebeurtenis verzendt het coderingsprogramma de selectievakjes voor de eerste header aan de service, waarmee de service de sparse bijhouden registratie in het manifest van de client.
   
    b. HTTP POST-aanvraag moet worden beëindigd door de encoder wanneer gegevens worden niet verzonden. Een langlopende HTTP POST die geen gegevens verzendt kunt voorkomen dat Media Services de encoder snel verbreken in het geval van een service-update of de server opnieuw opstarten. In dergelijke gevallen is de server tijdelijk zijn geblokkeerd in een ontvangstbewerking op de socket.
   
    c. Tijdens de tijd wanneer signalering van gegevens is niet beschikbaar, de encoder SHOULD sluit het HTTP POST-protocol vragen. Terwijl de POST-aanvraag actief is, moet het coderingsprogramma gegevens verzenden.

    d. Bij het verzenden van sparse fragmenten instellen het coderingsprogramma een expliciete header content-length, als deze beschikbaar is.

    e. Bij het verzenden van sparse fragmenten op basis van een nieuwe verbinding, moet het coderingsprogramma beginnen met het verzenden van de header vakken, gevolgd door de nieuwe fragmenten. Dit is voor gevallen waarin failover tussen gebeurt en de nieuwe sparse verbinding is met een nieuwe server die niet zichtbaar is voor de sparse bijhouden voordat wordt gemaakt.

    f. Het fragment sparse bijhouden beschikbaar voor de client wanneer de bijbehorende bovenliggende bijhouden fragment dat een tijdstempelwaarde groter of gelijk beschikbaar wordt gesteld aan de client. Bijvoorbeeld, als de sparse fragment een tijdstempel van t heeft = 1000, wordt verwacht dat nadat de client worden gezien 'video' (ervan uitgaande dat de naam van de bovenliggende bijhouden is 'video') fragment tijdstempel 1000 of voorbij, het kan downloaden, de sparse fragment t = 1000. Houd er rekening mee dat het werkelijke signaal kan worden gebruikt voor een andere positie in de presentatie-tijdlijn voor het opgegeven doel. In dit voorbeeld is het mogelijk dat de sparse fragment van t = 1000 heeft een XML-nettolading is voor het invoegen van een ad dat een paar seconden later.

    g. De nettolading van sparse bijhouden fragmenten kan zich in verschillende indelingen (zoals XML, tekst of binair), afhankelijk van het scenario.

### <a name="redundant-audio-track"></a>Redundante-nummer
In een typische HTTP adaptieve streaming-scenario (bijvoorbeeld Smooth Streaming of streepje) vaak, is er slechts één-nummer in de hele presentatie. In tegenstelling tot de video nummers die meerdere kwaliteitsniveaus voor de client op fouten te hebben, mag de-nummer een potentieel risico als de opname van de stroom waarin het nummer verbroken is. 

Media Services ondersteunt u lost dit probleem, live opname van redundante audio houdt. Het idee is of de dezelfde-nummer kan worden verzonden naar meerdere keren in verschillende stromen. Hoewel de service alleen-nummer eenmaal in het manifest van de client registreert, kan deze redundante audio houdt als back-ups gebruiken voor het ophalen van audio fragmenten als de primaire-nummer problemen heeft. Als u wilt opnemen redundante audio houdt, het coderingsprogramma moet:

1. Maak de dezelfde-nummer in meerdere fragment MP4 bitstreams. Redundante audio nummers moeten semantisch gelijk zijn, met de dezelfde fragment tijdstempels en uitwisselbaar op het niveau van-header en het fragment.
2. Zorg ervoor dat de 'audio' vermelding in de Live Server Manifest (sectie 6 in [1]) hetzelfde voor alle redundante audio nummers is.

De volgende implementatie wordt aanbevolen voor redundante audio houdt:

1. Elke unieke-nummer in een stream zelfstandig verzenden. Ook verzenden van een redundant stroom voor elk van deze stromen-nummer, waarbij de tweede stroom van de eerste afwijkt alleen door de id in de HTTP POST-URL: {protocol} :// {server address} / {punt path}/Streams({identifier}) publiceren.
2. Gebruik afzonderlijke stromen voor het verzenden van de twee laagste video bitsnelheden. Elk van deze stromen moet ook een kopie van elke unieke nummer bevatten. Wanneer er meerdere talen worden ondersteund, moeten deze stromen audio houdt voor elke taal bevatten.
3. Instanties van de afzonderlijke server (coderingsprogramma) gebruiken om te coderen en verzenden van de redundante stromen vermeld in (1) en (2). 

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
