---
title: Live streamen concepten in Azure Media Services - Live-evenementen en Live uitvoer | Microsoft Docs
description: In dit artikel biedt een overzicht van live streaming-concepten in Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/19/2019
ms.author: juliako
ms.openlocfilehash: a951ebd46335ad4639b8499283ddd30f13edd64e
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605655"
---
# <a name="live-events-and-live-outputs"></a>Live gebeurtenissen en live uitvoer

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Voor het configureren van uw live streaming-gebeurtenissen in Media Services v3, moet u inzicht in concepten die in dit artikel worden besproken.

> [!TIP]
> Voor klanten die migreren van Media Services v2 API's, de **Live gebeurtenis** entiteit vervangt **kanaal** in v2 en **uitvoer Live** vervangt **programma**.

## <a name="live-events"></a>Livegebeurtenissen

[Livegebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) zijn verantwoordelijk voor het opnemen en verwerken van de live videofeeds. Wanneer u een Live gebeurtenis maakt, wordt een primaire en secundaire invoereindpunt gemaakt waarmee u kunt een live signaal verzenden vanaf een externe coderingsprogramma. De externe live codering verzendt de bijdrage feed aan die een invoereindpunt met behulp van de [RTMP](https://www.adobe.com/devnet/rtmp.html) of [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (gefragmenteerde MP4) protocol invoer. Voor het RTMP-opnameprotocol voor transportstreams, de inhoud kan worden verzonden via de wissen (`rtmp://`) of veilig versleuteld op de kabel (`rtmps://`). Voor de Smooth Streaming-opnameprotocol worden gebruikt, zijn de ondersteunde URL-schema's `http://` of `https://`.  

## <a name="live-event-types"></a>Live gebeurtenistypen

Een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: Pass Through- en live codering. De typen zijn ingesteld tijdens het maken met behulp van [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype):

* **LiveEventEncodingType.None** - An on-premises live encoder sends a multiple bitrate stream. De opgenomen streams wordt doorgegeven via de Live gebeurtenis zonder verdere verwerking. 
* **LiveEventEncodingType.Standard** : een on-premises live codering verzendt een single-bitrate stream aan de Live gebeurtenis en Media Services wordt gemaakt van meerdere bitrate streams. Als de feed van de bijdrage van 720p of hogere resolutie, is de **Default720p** definitie wordt een set 6 resolutie/bitsnelheden paren coderen.
* **LiveEventEncodingType.Premium1080p** : een on-premises live codering verzendt een single-bitrate stream aan de Live gebeurtenis en Media Services wordt gemaakt van meerdere bitrate streams. De definitie Default1080p Hiermee geeft u de uitvoer set resolutie/bitsnelheden paren. 

### <a name="pass-through"></a>Pass-through

![passthrough](./media/live-streaming/pass-through.svg)

Bij gebruik van de pass-through **livegebeurtenis** bent u afhankelijk van de on-premises live-encoder voor het genereren van een multiple-bitrate videostream en het verzenden ervan als de bijdragefeed voor de livegebeurtenis (met behulp van RTMP of een gefragmenteerd MP4-protocol). De livegebeurtenis voert de binnenkomende videostream vervolgens zonder verdere verwerking uit. Dergelijke een Pass Through-Live-gebeurtenis is geoptimaliseerd voor langlopende live gebeurtenissen of 24 x lineair 365 live streamen. Bij het maken van dit type livegebeurtenis, geeft u None op (LiveEventEncodingType.None).

U kunt de bijdragefeed verzenden bij een resolutie tot maximaal 4K en een framesnelheid van 60 frames per seconde, met ofwel H.264/AVC- of H.265/HEVC-videocodecs, en AAC (AAC-LC, HE-AACv1 of HE-AACv2)-audiocodec.  Zie het artikel [Vergelijking van typen livegebeurtenissen](live-event-types-comparison.md) voor meer informatie.

> [!NOTE]
> Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
> 

Zie een .NET-codevoorbeeld in [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![live encoding](./media/live-streaming/live-encoding.svg)

Bij gebruik van live-codering met Media Services zou u uw on-premises live-encoder zodanig configureren dat één bitrate-video als bijdragefeed wordt verzonden naar de livegebeurtenis (met behulp van het protocol RTMP of gefragmenteerde MP4). U zou vervolgens instellen van een Live gebeurtenis zodat deze die binnenkomende single-bitrate codeert stream naar een [meerdere bitrate videostream](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), en wordt de uitvoer beschikbaar gemaakt voor levering aan apparaten via protocollen, zoals MPEG-DASH, HLS, af te spelen en het soepel te verwerken Streaming.

Wanneer u met live codering, kunt u de bijdrage alleen met een resolutie van 1080p resolutie tegen een tarief van het kader van 30 frames/seconde, met de video codec H.264/AVC en AAC-kanaal verzenden (AAC-LC, hij AACv1 of hij AACv2)-audiocodec. Houd er rekening mee dat Pass Through-Live gebeurtenissen resoluties maximaal 4 K op 60 frames per seconde ondersteunen kunnen. Zie het artikel [Vergelijking van typen livegebeurtenissen](live-event-types-comparison.md) voor meer informatie.

De oplossingen en bitrates die is opgenomen in de uitvoer van het live coderingsprogramma wordt bepaald door de voorinstelling. Als een **Standard** live coderingsprogramma (LiveEventEncodingType.Standard), dan zal de *Default720p* vooraf ingestelde geeft een reeks van 6 resolutie/bits rate-paren, die van 720 p op 3.5Mbps omlaag 192 p op 200 kbps. Als u met behulp van een **Premium1080p** live coderingsprogramma (LiveEventEncodingType.Premium1080p), dan zal de *Default1080p* vooraf ingestelde geeft een reeks van 6 resolutie/bits rate-paren, die van 1080 p op 3.5Mbps omlaag 180 p op 200 kbps. Zie [Systeemwaarden](live-event-types-comparison.md#system-presets) voor meer informatie.

> [!NOTE]
> Als u nodig hebt om aan te passen van de live voorinstelling voor encoding, opent u een ondersteuningsticket via Azure portal. Geef de gewenste tabel met resoluties en bitrates op. Controleer of er slechts één laag bij 720p (als vraagt een definitie voor een live codering-standaard) of 1080p (als een definitie voor een live coderingsprogramma Premium1080p aanvragen) en maximaal 6 lagen.

## <a name="live-event-creation-options"></a>Opties voor het maken van Live gebeurtenis

Bij het maken van een Live-gebeurtenis, kunt u de volgende opties opgeven:

* Het streaming-protocol voor de livegebeurtenis (momenteel worden de protocollen RTMP en Smooth Streaming ondersteund).<br/>U kunt de protocoloptie niet wijzigen terwijl de livegebeurtenis of de daaraan gekoppelde live-uitvoer worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u voor elk streaming-protocol een afzonderlijke livegebeurtenis.  
* IP-beperkingen voor de opname en voorbeeldweergave. U kunt de IP-adressen definiëren die zijn toegestaan om een video van deze livegebeurtenis op te nemen. Toegestane IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').<br/>Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, zijn er geen IP-adressen toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.<br/>De IP-adressen moeten een van de volgende indelingen hebben: IpV4-adres met 4 cijfers, CIDR-adresbereik.
* Bij het maken van de gebeurtenis kunt u opgeven dat deze automatisch wordt gestart. <br/>Wanneer autostart is ingesteld op True, wordt de Live gebeurtenis gestart na het maken ervan. De facturering begint zodra de Live gebeurtenis wordt gestart. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten. U kunt ook kun u de gebeurtenis wanneer u klaar bent om te streamen. 

    Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.

## <a name="live-event-ingest-urls"></a>URL's voor Live-gebeurtenis opnemen

Wanneer de livegebeurtenis is gemaakt, kunt u URL's voor opnemen ophalen die u aan de on-premises live-encoder levert. De live-encoder gebruikt deze URL's voor het invoeren van een live-stream. Zie voor meer informatie, [aanbevolen on-premises live coderingsprogramma's](recommended-on-premises-live-encoders.md). 

U kunt niet-vanity-URL's en vanity-URL's gebruiken. 

> [!NOTE] 
> Voor een URL voor opnemen worden voorspeld, stelt u de modus 'aangepaste'.

* Niet-vanity-URL

    Niet-vanity-URL is de standaardmodus in Media Services v3. U krijgt de livegebeurtenis mogelijk snel, maar de opname-URL is alleen bekend als de livegebeurtenis wordt gestart. De URL wordt gewijzigd als u de livegebeurtenis stopt of start. <br/>Niet-vanity is handig in scenario's waarbij een eindgebruiker wil streamen met een app waarbij de app een livegebeurtenis zo snel mogelijk wil downloaden en waarbij een dynamische opname-URL geen probleem is.
    
    Als een clienttoepassing niet nodig hebt voor het vooraf genereren van een URL voor opnemen voor de Live gebeurtenis wordt gemaakt, net, kunnen Media Services voor het automatisch genereren het toegangstoken voor de live-gebeurtenis.
* Vanity URL

    De vanity-modus heeft de voorkeur bij grote tv-media die gebruikmaken van hardware broadcast-encoders en die hun encoders niet opnieuw willen configureren als de livegebeurtenis wordt gestart. Ze willen een voorspellende opname-URL, die niet na verloop van tijd verandert.
    
    Als u wilt deze modus opgeven, stelt u `vanityUrl` te `true` tijdens het maken (de standaardwaarde is `false`). U moet ook uw eigen token doorgeven (`LiveEventInput.accessToken`) tijdens het maken. U opgeven de token waarde om te voorkomen dat een willekeurige token in de URL. Het toegangstoken heeft een geldige GUID-tekenreeks (met of zonder de afbreekstreepjes). Zodra de modus is ingesteld, kan deze kan niet worden bijgewerkt.

    Het toegangstoken moet uniek zijn in uw datacenter. Als uw toepassing moet een aangepaste URL moet worden gebruikt, is het aanbevolen maken altijd een nieuw exemplaar van de GUID voor uw toegangstoken (in plaats van opnieuw gebruiken van een bestaande GUID). 

    Gebruik de volgende API's voor de Vanity-URL inschakelen en het toegangstoken ingesteld op een geldige GUID (bijvoorbeeld `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`).  
    
    |Taal|Aangepaste URL inschakelen|Toegangstoken instellen|
    |---|---|---|
    |REST|[properties.vanityUrl](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[LiveEventInput.accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--vanity-url](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--access-token](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent.VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[LiveEventInput.AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Live naamgevingsregels URL voor opnemen

* De *willekeurige* tekenreeks hieronder is een 128-bits hexadecimaal getal (bestaande uit 32 tekens, van 0-9 en a-f).
* *uw toegangstoken* -de geldige GUID-tekenreeks die u bij het gebruik van de aangepaste modus instellen. Bijvoorbeeld `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *naam van een stream* -geeft aan dat de naam van de stream voor een specifieke verbinding. De waarde van de stream wordt meestal toegevoegd door het live coderingsprogramma dat u gebruikt. U kunt configureren dat het live coderingsprogramma voor het gebruik van een willekeurige naam voor het beschrijven van de verbinding, bijvoorbeeld: 'video1_audio1", 'video2_audio1', 'stream'.

#### <a name="non-vanity-url"></a>Niet-vanity-URL

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<auto-generated access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<auto-generated access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>Vanity URL

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>Live gebeurtenis de voorbeeld-URL

Zodra de Live-gebeurtenis wordt gestart op de bijdrage feed ontvangen, kunt u de preview-eindpunt om te bekijken en te valideren dat u de live stream ontvangt voordat u verder publiceert. Nadat u hebt gecontroleerd dat de stroom preview goed is, kunt u de Live-gebeurtenis kunt gebruiken om de live stream beschikbaar voor levering via een of meer (vooraf gemaakte) Streaming-eindpunten. Om dit te realiseren, maakt u een nieuw [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs) op de Live-gebeurtenis. 

> [!IMPORTANT]
> Zorg ervoor dat de video worden doorgestuurd naar de URL van de Preview-versie voordat u verdergaat!

## <a name="live-event-long-running-operations"></a>Live gebeurtenis langlopende bewerkingen

Zie voor meer informatie, [langlopende bewerkingen](media-services-apis-overview.md#long-running-operations)

## <a name="live-outputs"></a>Live-uitvoer

Zodra u de stroom doorgestuurd naar de Live gebeurtenis hebt, kunt u de streaminggebeurtenis starten door het maken van een [Asset](https://docs.microsoft.com/rest/api/media/assets), [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs), en [Streaming-Locator gemaakt](https://docs.microsoft.com/rest/api/media/streaminglocators). Live uitvoer wordt de stream gearchiveerd en beschikbaar maken aan kijkers via de [Streaming-eindpunt](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> Live-uitvoer starten zodra ze zijn gemaakt en stoppen wanneer ze worden verwijderd. Wanneer u de live-uitvoer verwijdert, verwijdert u de onderliggende Asset en de inhoud van de Asset niet. 

De relatie tussen een **Live gebeurtenis** en de bijbehorende **uitvoer Live** is vergelijkbaar met traditionele televisie uitzenden, waarbij een kanaal (Live gebeurtenis) een constante stream met video- en een (Live-opname vertegenwoordigt Uitvoer) is afgestemd op een specifiek tijdstip-segment (bijvoorbeeld 's avonds nieuws van 18:30:00 uur op 19:00 uur). U kunt met behulp van een Digital Video Recorder (DVR) televisie vastleggen – de vergelijkbare functie in Live gebeurtenissen wordt beheerd de **archiveWindowLength** eigenschap. Het is een ISO 8601-timespan duur (bijvoorbeeld PTHH:MM:SS), waarmee wordt Hiermee geeft u de capaciteit van de DVR en kan worden ingesteld van minimaal 3 minuten tot een maximum van 25 uur.

Het object Live uitvoer is vergelijkbaar met een tape recorder die wordt onderschept en -record de live stream in een activum in Media Services-account. De opgenomen inhoud wordt permanent worden opgeslagen in de Azure Storage-account dat is gekoppeld aan uw account in de container die wordt gedefinieerd door de resource actief. De Live-uitvoer kunt u voor het beheren van bepaalde eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom wordt opgeslagen in het archief opnemen (bijvoorbeeld, de capaciteit van de cloud-DVR) en of viewers bekijken van de live stream kunnen starten. Het archief op schijf is een circulaire archief "venster" die bevat alleen de hoeveelheid inhoud die is opgegeven in de eigenschap archiveWindowLength van de Live-uitvoer. Inhoud die buiten dit venster valt wordt automatisch verwijderd uit de opslagcontainer en kan niet worden hersteld. U kunt meerdere Live-uitvoer (maximaal drie maximum) maken op een Live-gebeurtenis met verschillende archief sleutellengten en -instellingen.  

Als u de Live uitvoer hebt gepubliceerd **Asset** met behulp van een **Streaming-Locator gemaakt**, blijft de Live-gebeurtenis (tot de lengte van het DVR-venster) worden weergegeven tot de vervaldatum of verwijdering, de Streaming-Locator afhankelijk van wat het eerste komt.

Zie voor meer informatie, [met behulp van een cloud-DVR](live-event-cloud-dvr.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)
