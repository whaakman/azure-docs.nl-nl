---
title: Specificatie van Azure Media Services live gefragmenteerde MP4-liveopname | Microsoft Docs
description: Deze specificatie beschrijving van het protocol en de indeling voor gefragmenteerde MP4-opname op basis van live streaming voor Azure Media Services. U kunt Azure Media Services gebruiken om te streamen van live-evenementen en inhoud in realtime uitzenden met behulp van Azure als het cloud-platform. Dit document wordt ook beschreven aanbevolen procedures voor het bouwen van uiterst redundante en robuuste live mechanismen opnemen.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: ''
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: c6ff386913ed66cf4f74cb577bb8ca58e6932ada
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228875"
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Specificatie van Azure Media Services live gefragmenteerde MP4-liveopname
Deze specificatie beschrijving van het protocol en de indeling voor gefragmenteerde MP4-opname op basis van live streaming voor Azure Media Services. Media Services biedt een service voor live streamen die klanten gebruiken kunnen voor het streamen van live-evenementen uit te zenden van inhoud in realtime met behulp van Azure als het cloud-platform. Dit document wordt ook beschreven aanbevolen procedures voor het bouwen van uiterst redundante en robuuste live mechanismen opnemen.

## <a name="1-conformance-notation"></a>1. Conformiteit van notatie
De sleutel 'moet,""Moet niet,"'Vereist,'"Geldt,""wordt niet,""SHOULD,""Moet niet"woorden 'Aanbevolen,' 'Mei,' en 'Optioneel' in dit document zijn om te worden geïnterpreteerd als ze worden beschreven in RFC 2119.

## <a name="2-service-diagram"></a>2. Servicediagram
Het volgende diagram toont de architectuur op hoog niveau van de live-streamingservice in Media Services:

1. Een live coderingsprogramma pushes live-feeds aan kanalen die zijn gemaakt en ingericht via de Azure Media Services SDK.
1. Kanalen, programma's en streaming-eindpunten in Media Services verwerkt alle functionaliteiten voor live streamen, waaronder opnemen, opmaken, DVR, beveiliging, schaalbaarheid en redundantie in de cloud.
1. (Optioneel) kunnen klanten kiezen voor het implementeren van een laag Azure Content Delivery Network tussen het streaming-eindpunt en de clienteindpunten.
1. Client-eindpunten stroom van het streaming-eindpunt met behulp van protocollen HTTP adaptieve Streaming. Voorbeelden zijn onder meer Microsoft Smooth Streaming, Dynamic Adaptive Streaming via HTTP (DASH of MPEG-DASH) en Apple HTTP Live Streaming (HLS).

![stroom opnemen][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. Indeling voor bitstream – ISO 14496-12-gefragmenteerde MP4
De draadindeling voor live streaming-opnameprotocol worden besproken dit document is gebaseerd op [ISO-14496-12]. Zie voor een gedetailleerde uitleg van gefragmenteerde MP4-indeling en -extensies zowel voor video-on-demand-bestanden en live streaming opnemen, [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).

### <a name="live-ingest-format-definitions"></a>Live opnemen indeling definities
De volgende lijst bevat speciale indeling definities die van toepassing op live te worden opgenomen in Azure Media Services:

1. De **ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken moeten worden verzonden met elke aanvraag (HTTP POST). Deze vakken moeten worden verzonden aan het begin van de stroom en elk gewenst moment om te hervatten van de stroom moet opnieuw verbinding te maken met het coderingsprogramma opnemen. Zie de sectie 6 in [1] voor meer informatie.
1. Sectie 3.3.2 in [1] definieert een optioneel vak met de naam **StreamManifestBox** voor live opnemen. Vanwege de routering logica van de Azure load balancer is met behulp van dit vak afgeschaft. Het vak moet niet worden gebruikt als opnemen in Media Services. Als dit selectievakje in aanwezig is, Media Services op de achtergrond wordt deze genegeerd.
1. De **TrackFragmentExtendedHeaderBox** vak gedefinieerd in 3.2.3.2 in [1] moet aanwezig zijn voor elk fragment.
1. Versie 2 van de **TrackFragmentExtendedHeaderBox** box moet worden gebruikt voor het genereren van mediasegmenten waarvoor de dezelfde URL's in meerdere datacenters. Het fragment index-veld is vereist voor cross-datacenter failover van de index op basis van streaming-indelingen, zoals Apple HLS en MPEG-DASH op basis van een index. Cross-datacenter om failover mogelijk te, het fragment index moet worden gesynchroniseerd tussen meerdere coderingsprogramma's en zelfs in het coderingsprogramma wordt opnieuw opgestart of fouten worden verhoogd met 1 voor elk fragment opeenvolgende media.
1. Sectie 3.3.6 in [1] definieert een vak met de naam **MovieFragmentRandomAccessBox** (**mfra**) die kunnen worden verzonden aan het einde van live-opname om aan te geven van end-van-stream (EOS) naar het kanaal. Vanwege de logica voor opname van Media Services, met behulp van EOS is afgeschaft, en de **mfra** vak voor live-opname niet moet worden verzonden. Als de verzonden, Media Services op de achtergrond wordt deze genegeerd. Als u de status van de opname-punt herstellen, raden wij aan dat u [kanaal opnieuw](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels). We raden u ook aan dat u [programma stoppen](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) beëindigen van een presentatie en stream.
1. De duur van de MP4-fragment moet constante, te verminderen van de grootte van de clientmanifesten. Een constante MP4 fragment duur verbetert tevens methodiek van de client downloaden via het gebruik van tags herhalen. De duur kan om te compenseren voor niet-geheel getal framesnelheden variëren.
1. De duur van de MP4-fragment moet ongeveer 2 tot 6 seconden.
1. MP4 fragment tijdstempels en indexen (**TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` en `fragment_index`) moeten in oplopende volgorde binnenkomen. Hoewel Media Services tegen dubbele fragmenten is, is deze mogelijkheid om opnieuw te rangschikken fragmenten op basis van de media-tijdlijn beperkt.

## <a name="4-protocol-format--http"></a>4. Indeling protocol: HTTP
ISO-gefragmenteerde MP4 gebaseerde live opnemen voor Media Services maakt gebruik van een standard langdurige HTTP POST-aanvraag te verzenden gecodeerde mediagegevens die in gefragmenteerde MP4-indeling naar de service wordt geleverd. Elke HTTP-POST verstuurt een complete gefragmenteerd MP4 bitstream ('stream'), vanaf het begin met header vakken (**ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken ), en doorgaan met een reeks fragmenten (**moof** en **mdat** vakken). URL-syntaxis voor de HTTP POST-aanvraag, Zie sectie 9.2 in [1]. Er is een voorbeeld van de POST-URL: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Vereisten
Hier volgen de gedetailleerde vereisten:

1. Het coderingsprogramma moet eerst de uitzending een HTTP POST-aanvraag met een lege 'hoofdtekst' (nul inhoudslengte) verzenden met behulp van dezelfde opname-URL. Dit kan helpen de encoder snel te detecteren of de live-opname-eindpunt geldig is en als er verificatie of andere voorwaarden die vereist zijn. Per HTTP-protocol, kan geen de server terugsturen van een HTTP-antwoord totdat de gehele aanvraag, met inbegrip van de hoofdtekst van het bericht is ontvangen. Gezien de aard van de langdurige van live-gebeurtenis, zonder deze stap wordt het coderingsprogramma niet mogelijk voor het detecteren van alle fouten, totdat het is voltooid met het verzenden van alle gegevens.
1. Het coderingsprogramma moet eventuele fouten of verificatiecontroles verwerken vanwege (1). Als (1) is voltooid met een 200-respons worden voortgezet.
1. Het coderingsprogramma moet een nieuwe HTTP POST-aanvraag starten met de gefragmenteerde MP4-stream. De nettolading van de moet beginnen met de vakken kop, gevolgd door fragmenten. Houd er rekening mee dat de **ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken (in deze volgorde) moeten worden verzonden met elke aanvraag, zelfs als het coderingsprogramma opnieuw verbinding maken moet omdat de vorige aanvraag is beëindigd voorafgaand aan het einde van de stroom. 
1. Het coderingsprogramma moet gesegmenteerde overdrachtscodering voor het uploaden, omdat het is niet mogelijk om te voorspellen van de lengte van de gehele inhoud van de live-gebeurtenis gebruiken.
1. Wanneer de gebeurtenis is afgelopen, na het verzenden van de laatste fragment, moet het coderingsprogramma zonder problemen de gesegmenteerde overdrachtscodering berichtenstroom (de meeste HTTP-client-stacks verwerkt automatisch) eindigen. Het coderingsprogramma moet wachten op de service om te retourneren van de laatste responscode en vervolgens de verbinding verbreken. 
1. Het coderingsprogramma niet moet gebruiken de `Events()` zelfstandig naamwoord zoals beschreven in 9.2 in [1] voor live-opname in Media Services.
1. Als de HTTP POST-aanvraag wordt beëindigd of met een TCP-fout voorafgaand aan het einde van de stroom een optreedt time-out, moet het coderingsprogramma uitgeven van een nieuwe POST-aanvraag met behulp van een nieuwe verbinding en volg de bovenstaande vereisten. Het coderingsprogramma moet bovendien opnieuw verzenden van de vorige twee MP4-fragmenten voor elk nummer in de stroom en hervatten zonder een onderbreking in de tijdlijn van de media. Opnieuw verzenden van de laatste twee MP4-fragmenten voor elk nummer zorgt ervoor dat er geen gegevens verloren gaan. Met andere woorden, als een stroom zowel een audio- en een video traceren bevat en de huidige POST-aanvraag is mislukt, de encoder moet opnieuw verbinding maken en verzenden van de laatste twee fragmenten voor het audiospoor die eerder met succes zijn verzonden, en de laatste twee fragmenten voor de video bijhouden, die eerder met succes zijn verzonden, om ervoor te zorgen dat er geen gegevens verloren gaan. Het coderingsprogramma moet onderhouden van een "forward" buffer van media-fragmenten, die het opnieuw wanneer verbinding wordt gemaakt.

## <a name="5-timescale"></a>5. Tijdschaal
[[MS-SSTR] ](https://msdn.microsoft.com/library/ff469518.aspx) beschrijft het gebruik van tijdschaal voor **SmoothStreamingMedia** (sectie 2.2.2.1), **StreamElement** (sectie 2.2.2.3), **StreamFragmentElement** () Sectie 2.2.2.6), en **LiveSMIL** (sectie 2.2.7.3.1). Als de waarde tijdschaal niet aanwezig is, is de standaardwaarde gebruikt 10,000,000 (10 MHz). Hoewel de indelingsspecificatie Smooth Streaming niet gebruik van andere waarden tijdschaal blokkeert, meeste encoder-implementaties maken gebruik van deze standaard waarde (10 MHz) voor het genereren van Smooth Streaming opnemen van gegevens. Vanwege de [dynamische verpakking van Azure Media](media-services-dynamic-packaging-overview.md) functie, raden wij aan dat u een 90-KHz tijdschaal voor video-streams en 44,1 of 48.1 KHz voor audiostreams. Als andere tijdschaal waarden worden gebruikt voor verschillende gegevensstromen, kan de stream-niveau tijdschaal moet worden verzonden. Zie voor meer informatie, [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. Definitie van 'stream'
Stream is de basiseenheid van bewerking in de live-opname voor het opstellen van live presentaties, streaming failover en het scenario's voor redundantie. Stream wordt gedefinieerd als een unieke, gefragmenteerde MP4-bitstream die één track kan bevatten of meerdere nummers. Een volledige live presentatie kan een of meer stromen, afhankelijk van de configuratie van de live coderingsprogramma's bevatten. De volgende voorbeelden ziet u verschillende opties van het gebruik van stromen voor het opstellen van een volledige live presentatie.

**Voorbeeld:** 

Een klant wil maken van een live streaming presentatie met de volgende audio/video-bitsnelheden:

Video: 3000 kbps, 1500 kbps, 750 kbps

Audio-128 k

### <a name="option-1-all-tracks-in-one-stream"></a>Optie 1: Alle sporen in één stroom
Bij deze optie een één-coderingsprogramma genereert alle audio/video-nummers en worden ze vervolgens in een gefragmenteerde MP4-bitstream. De gefragmenteerde MP4-bitstream wordt verzonden via één HTTP POST-verbinding. In dit voorbeeld is er slechts één stroom voor deze live presentatie.

![Streams-een bijhouden][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>Optie 2: Elke bijhouden in een afzonderlijke stream
Bij deze optie, de encoder een nummer in elk fragment MP4-bitstream wordt geplaatst en plaatst vervolgens alle van de stromen via afzonderlijke HTTP-verbindingen. Dit kan worden gedaan met een coderingsprogramma of met meerdere coderingsprogramma's. De opname van live gegevens ziet deze live presentatie zoals bestaat uit vier streams.

![Streams afzonderlijke sporen te wissen][image3]

### <a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Optie 3: Bundel audiotrack met de laagste bitrate video bijhouden in één stream
Bij deze optie kiest de klant het audiospoor met de laagste-bitrate video track bundelen in één fragment MP4 bitstream, en dat de andere twee video sporen als afzonderlijke streams. 

![Streams-audio en video-nummers][image4]

### <a name="summary"></a>Samenvatting
Dit is niet een volledige lijst van alle mogelijke opname-opties voor dit voorbeeld. Als een sterker nog, wordt een groepering van de nummers in stromen ondersteund door live-opname. Klanten en leveranciers encoder kunnen kiezen voor hun eigen implementaties op basis van de technische complexiteit, encoder-capaciteit en redundantie en failover-overwegingen. In de meeste gevallen is er echter slechts één audiotrack voor de hele live presentatie. Het is dus belangrijk om te controleren of de gezondheid van de opname-stream met de audiotrack. Deze overweging vaak resulteert in het audiospoor plaatsen in een eigen stream (zoals in optie 2) of deze bundeling met de laagste-bitrate video bijhouden (zoals in de optie 3). Bovendien, voor betere redundantie en fouttolerantie geboden kunnen verzenden van de dezelfde audiotrack in twee verschillende stromen (optie 2 met redundante audionummers) of het audiospoor met ten minste twee van de laagste-bitrate video-nummers (optie 3 met audio gebundeld in ten minste twee bundeling video-streams) wordt sterk aanbevolen voor live te worden opgenomen in Media Services.

## <a name="7-service-failover"></a>7. Failover van de service
Gezien de aard van live streamen, goede failover-ondersteuning, het is essentieel voor ervoor te zorgen dat de beschikbaarheid van de service. Media Services is ontworpen voor het afhandelen van verschillende typen fouten, met inbegrip van netwerkfouten, server-fouten en opslagproblemen. Wanneer gebruikt in combinatie met de juiste failoverlogica aan de live coderingsprogramma, kunnen klanten een zeer betrouwbare service voor live streamen vanuit de cloud kunnen bereiken.

In deze sectie wordt besproken hoe service failover-scenario's. In dit geval wordt de fout doet zich ergens in de service en het doet zich voor als er is een netwerkfout. Hier volgen enkele aanbevelingen voor de implementatie van het coderingsprogramma voor het verwerken van de failover van de service:

1. Gebruik een 10 seconden time-out voor de TCP-verbinding tot stand brengen. Als een poging tot het maken van de verbinding langer duurt dan 10 seconden, de bewerking afbreken en probeer het opnieuw. 
1. Gebruik een korte time-out voor het verzenden van de HTTP-aanvraag bericht segmenten. Als de duur van de doel-MP4-fragment N seconden, gebruikt u de time-out voor een verzenden tussen N en 2 N seconden; Als de duur van de MP4-fragment 6 seconden, gebruikt u bijvoorbeeld een time-out van 6 tot en met 12 seconden. Als er een time-out optreedt, opnieuw instellen van de verbinding, opent u een nieuwe verbinding en hervatten van stream opnemen met de nieuwe verbinding. 
1. Onderhouden een rolling buffer die de laatste twee fragmenten voor elke bijhouden die zijn correct en volledig zijn verzonden naar de service heeft.  Als de HTTP POST-aanvraag voor een stroom wordt beëindigd of een time-out optreedt voorafgaand aan het einde van de stroom, opent u een nieuwe verbinding en beginnen met een andere HTTP POST-aanvraag, de stream-headers verzenden opnieuw verzenden van de laatste twee fragmenten voor elk nummer en de stroom hervatten zonder een d iscontinuity in de tijdlijn van de media. Dit vermindert de kans op gegevensverlies.
1. Het is raadzaam om het coderingsprogramma stelt geen beperking voor het aantal nieuwe pogingen om een verbinding tot stand brengen of hervatten streaming nadat een TCP-fout optreedt.
1. Na een TCP-fout:
  
    a. De huidige verbinding moet worden gesloten en een nieuwe verbinding moet worden gemaakt voor een nieuwe HTTP POST-aanvraag.

    b. De nieuwe HTTP POST-URL moet hetzelfde zijn als de oorspronkelijke POST-URL.
  
    c. De nieuwe HTTP POST moet stream headers bevatten (**ftyp legt vast**, **vak Server Manifest Live**, en **moov** vakken) die zijn identiek aan de stream-headers in het eerste bericht.
  
    d. De laatste twee fragmenten verzonden voor elk nummer moeten opnieuw worden verzonden en streaming moet hervatten zonder een onderbreking in de tijdlijn van de media. Het fragment MP4 tijdstempels oplopen continu, zelfs via HTTP POST-aanvragen.
1. Het coderingsprogramma moet de HTTP POST-aanvraag beëindigen als gegevens niet tegen een tarief evenredig met de duur van de MP4-fragment wordt verzonden.  Een HTTP POST-aanvraag die geen gegevens verzendt kunt voorkomen dat van Media Services van het coderingsprogramma snel verbreken in het geval van een service-update. Om deze reden, de HTTP-POST voor sparse (ad signaal) houdt moeten worden kortstondig wordt beëindigd zodra de sparse fragment wordt verzonden.

## <a name="8-encoder-failover"></a>8. Failover van het coderingsprogramma
Failover van het coderingsprogramma is het tweede type failoverscenario dat moet worden verholpen voor een end-to-end levering van live streaming. In dit scenario treedt het probleem op aan de kant van het coderingsprogramma. 

![Failover van het coderingsprogramma][image5]

De volgende verwachtingen zijn van toepassing uit de live-opname-eindpunt als encoder failover gebeurt:

1. Een nieuw exemplaar van het coderingsprogramma moet worden gemaakt om door te gaan streaming, zoals wordt geïllustreerd in het diagram (Stream voor 3000k video met stippellijn).
1. De nieuwe encoder moet dezelfde URL gebruiken voor HTTP POST-aanvragen als het mislukte exemplaar.
1. De nieuwe encoder POST-aanvraag moet opnemen met de dezelfde gefragmenteerde MP4-header vakken als het mislukte exemplaar.
1. De nieuwe encoder moet correct worden gesynchroniseerd met alle andere actieve coderingsprogramma's voor een live presentatie voor het genereren van gesynchroniseerde audio/video-voorbeelden met uitgelijnde fragment grenzen.
1. De nieuwe stroom moet semantisch overeen met de vorige stroom en uitwisselbaar op het niveau van de kop- en fragment.
1. De nieuwe encoder moet proberen om gegevensverlies te minimaliseren. De `fragment_absolute_time` en `fragment_index` van media fragmenten vanaf het punt waar de encoder laatst geëindigd moeten vergroten. De `fragment_absolute_time` en `fragment_index` op een manier die doorlopende moet verhogen, maar is het toegestaan om te introduceren een onderbreking, indien nodig. Media Services negeert fragmenten die al is ontvangen en verwerkt, dus is het beter om de fout aan de zijkant fragmenten dan te introduceren wijzigingen in de tijdlijn van de media opnieuw verzenden. 

## <a name="9-encoder-redundancy"></a>9. Coderingsprogramma redundantie
Voor bepaalde kritieke live gebeurtenissen die aanvraag nog hogere beschikbaarheid en kwaliteit vereisen, het wordt aangeraden dat u actief / actief redundante coderingsprogramma's gebruiken voor een naadloze failover zonder verlies van gegevens.

![Coderingsprogramma redundantie][image6]

Zoals wordt geïllustreerd in het volgende diagram, pushen twee groepen van encoders twee exemplaren van elke stroom tegelijkertijd naar de live-service. Deze instelling wordt ondersteund, omdat Media Services dubbele fragmenten op basis van de stream-ID en het fragment timestamp uitfilteren kunt. De resulterende live te streamen en te archiveren is één exemplaar van alle stromen die het best mogelijke aggregatie is van de twee bronnen. Bijvoorbeeld, in een hypothetische uiterste geval, zolang er is een coderingsprogramma (deze hebben niet hetzelfde account worden) uitgevoerd op een willekeurig moment in de tijd voor elke stroom, de resulterende live stream van de service is continue zonder verlies van gegevens. 

De vereisten voor dit scenario zijn bijna hetzelfde als de vereisten in het geval is 'Encoder failover', met uitzondering dat de tweede set coderingsprogramma's worden uitgevoerd op hetzelfde moment als de primaire coderingsprogramma's.

## <a name="10-service-redundancy"></a>10. Serviceredundantie
Voor zeer redundante wereldwijde distributie, moet u soms regio-overschrijdende back-up voor het afhandelen van regionale rampen hebben. Voortbouwend op de 'Encoder redundantie'-topologie, kunnen klanten kiezen om een redundante service-implementatie in een andere regio die verbonden met de tweede set coderingsprogramma's. Klanten kunnen ook werken met een Content Delivery Network-provider voor het implementeren van een globale Traffic Manager voor de twee implementaties naadloos om clientverkeer te routeren. De vereisten voor het coderingsprogramma's zijn hetzelfde als de aanvraag 'Encoder redundantie'. De enige uitzondering hierop is dat de tweede set encoders moet worden gekoppeld aan een andere live eindpunt opnemen. Het volgende diagram ziet u deze instellingen:

![Serviceredundantie][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Speciale typen opname-indelingen
Deze sectie wordt besproken speciale typen van live-opname-indelingen die zijn ontworpen voor het afhandelen van specifieke scenario's.

### <a name="sparse-track"></a>Sparse bijhouden
Wanneer u een live streaming presentatie met een rijke Clientervaring, vaak het nodig is voor het verzenden van gebeurtenissen tijd gesynchroniseerd of signalen in-band met de belangrijkste mediagegevens. Een voorbeeld hiervan is dynamische live advertentie-invoeging. Dit type gebeurtenis signalering wijkt af van reguliere audio/video-streaming vanwege de aard van de sparse. Met andere woorden, de signalering gegevens meestal gebeurt niet continu en het interval kan het moeilijk te voorspellen zijn. Het concept van sparse bijhouden is ontworpen voor het opnemen en uitzenden van in-band signalering van gegevens.

De volgende stappen zijn een aanbevolen implementatie voor het opnemen van sparse bijhouden:

1. Maak een afzonderlijke gefragmenteerde MP4 bitstream met sparse sporen, zonder audio/video sporen te wissen.
1. In de **vak Server Manifest Live** zoals gedefinieerd in de sectie 6 in [1], gebruikt u de *parentTrackName* parameter opgeven voor de naam van de bovenliggende bijhouden. Zie voor meer informatie de sectie 4.2.1.2.1.2 in [1].
1. In de **vak Server Manifest Live**, **manifestOutput** moet worden ingesteld op **waar**.
1. Gezien de sparse aard van de signalering gebeurtenis, is het raadzaam het volgende:
   
    a. Aan het begin van de live-gebeurtenis verzendt de codering de eerste kop vakken naar de service, waardoor de service voor het registreren van de sparse bijhouden in het manifest van de client.
   
    b. Het coderingsprogramma moet de HTTP POST-aanvraag beëindigen wanneer gegevens worden niet verzonden. Een langdurige HTTP POST die geen gegevens verzendt kunt voorkomen dat Media Services snel loskoppelen van het coderingsprogramma in het geval van een service-update of de server opnieuw opstarten. In dergelijke gevallen worden de media-server is tijdelijk geblokkeerd in een ontvangstbewerking op de socket.
   
    c. Tijdens het tijdstip waarop signalering van gegevens niet beschikbaar is, de encoder SHOULD sluit de HTTP POST vragen. Terwijl de POST-aanvraag actief is, moet het coderingsprogramma gegevens verzenden.

    d. Bij het verzenden van sparse fragmenten, de encoder een expliciete content-length-header ingesteld als deze beschikbaar is.

    e. Bij het verzenden van sparse fragmenten op basis van een nieuwe verbinding, moet de encoder beginnen met het verzenden van de vakken kop, gevolgd door de nieuwe fragmenten. Dit is bedoeld voor gevallen waarin failover gebeurt daartussenin, en de nieuwe sparse verbinding is tot stand brengen met een nieuwe server die niet zichtbaar is voor de sparse track voordat.

    f. Het fragment sparse bijhouden wordt beschikbaar gesteld aan de client wanneer de bijbehorende bovenliggende bijhouden fragment dat een waarde gelijk aan of groter timestamp heeft voor de client beschikbaar wordt gesteld. Bijvoorbeeld, als de sparse fragment een tijdstempel van t heeft = 1000, verwacht wordt dat nadat de client ziet "video" (ervan uitgaande dat de naam van de bovenliggende bijhouden is "video") "fragment timestamp 1000 of meer, kan deze de sparse fragment t downloaden = 1000. Houd er rekening mee dat de werkelijke signaal voor een andere positie in de tijdlijn van de presentatie kan worden gebruikt voor het opgegeven doel. In dit voorbeeld is het mogelijk dat de sparse fragment van t = 1000 heeft een XML-nettolading is voor het invoegen van een AD-tenant in een positie die een paar seconden later opnieuw.

    g. De nettolading van sparse bijhouden fragmenten kan zich in verschillende indelingen (zoals XML, tekst of binaire waarde), afhankelijk van het scenario.

### <a name="redundant-audio-track"></a>Redundante audiotrack
In een typisch HTTP adaptieve streaming scenario (bijvoorbeeld Smooth Streaming of streepje), vaak het geval is, is er slechts één audiotrack in de hele presentatie. In tegenstelling tot de video worden bijgehouden, die meerdere kwaliteitsniveaus voor de client om de verkeersbelasting in foutcondities hebben, kan het audiospoor een single point of failure zijn als de opname van gegevens van de stroom met de audiotrack verbroken is. 

U lost dit probleem, ondersteunt Media Services live-opname van redundante audionummers. Het idee is dat de dezelfde audiotrack kan worden verzonden meerdere keren in verschillende stromen. Hoewel de service alleen het audiospoor één keer in het manifest van de client registreert, kan deze redundante audionummers gebruiken als back-ups voor het ophalen van audio fragmenten als de primaire audiotrack problemen heeft. Als u wilt redundante audionummers opnemen, de encoder moet:

1. De dezelfde audiotrack in meerdere fragment MP4 bitstreams maken. De redundante audionummers moeten semantisch overeenkomen met de hetzelfde fragment tijdstempels en uitwisselbaar op het niveau van de kop- en fragment.
1. Zorg ervoor dat de "audio" vermelding in de Live Server Manifest (sectie 6 in [1]) hetzelfde voor alle redundante audionummers is.

De volgende implementatie wordt aanbevolen voor redundante Audionummers:

1. Elke unieke audiotrack verzenden in een stroom op zichzelf. Een redundante stream ook verzenden voor elk van deze stromen audiotrack, waar de tweede stream van de eerste verschilt alleen door de id in de HTTP POST-URL: {protocol} :// {server address} / {punt path}/Streams({identifier}) publiceren.
1. Gebruik afzonderlijke streams voor het verzenden van de twee laagste video bitsnelheden. Elk van deze stromen moet ook een kopie van elke unieke audiotrack bevatten. Wanneer er meerdere talen worden ondersteund, moeten deze stromen audionummers voor elke taal bevatten.
1. Exemplaren van de afzonderlijke server (codering) gebruiken om te coderen en verzenden van de redundante stromen die worden vermeld (1) en (2). 

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
