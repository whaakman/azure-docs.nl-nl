---
title: Overzicht van Live streamen met Azure Media Services | Microsoft Docs
description: Dit onderwerp biedt een overzicht van live streamen met Azure Media Services v3.
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
ms.date: 11/08/2018
ms.author: juliako
ms.openlocfilehash: a4569505cb9a42f6682391a8b06725dea5e539dc
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344970"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Live streamen met Azure Media Services v3

Bij het leveren van live streaming-gebeurtenissen met Azure Media Services wordt u doorgaans gebruik van de volgende onderdelen:

* Een camera die wordt gebruikt voor het uitzenden van een gebeurtenis.
* Een coderingsprogramma voor live video dat signalen van de camera (of een ander apparaat, zoals een laptop) converteert naar stromen die worden verzonden naar de service voor Live Streaming. De signalen kunnen ook advertentie SCTE 35 en Ad-hints. 
* De Media Services Live Streaming-service kunt u opnemen, preview, inpakken, registreren, versleutelen en uitzenden van de inhoud aan uw klanten of aan een CDN voor verdere distributie.

In dit artikel biedt een gedetailleerd overzicht en schema's van de belangrijkste onderdelen die betrokken zijn bij het live streamen met Media Services bevat.

## <a name="overview-of-main-components"></a>Overzicht van de belangrijkste onderdelen

In Media Services zijn [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) verantwoordelijk voor het verwerken inhoud voor live-streaming. Een LiveEvent biedt een invoereindpunt (URL voor opnemen) dat u vervolgens aan een on-premises live codering bieden. De LiveEvent live invoerstromen ontvangt van het live coderingsprogramma in RTMP of Smooth Streaming-indeling en maakt deze beschikbaar voor streamen via een of meer [door](https://docs.microsoft.com/rest/api/media/streamingendpoints). Een [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) kunt u de publicatie, registratie en DVR-vensterinstellingen van de live stream kunt beheren. De LiveEvent biedt ook een preview-eindpunt (de voorbeeld-URL) die u gebruikt om te bekijken en uw stream te valideren voordat deze verder wordt verwerkt en geleverd. 

Media Services biedt **dynamische verpakking**, waarmee u preview uit te zenden van uw inhoud in MPEG-DASH, HLS, Smooth Streaming streaming-indelingen zonder dat u hoeft voor handmatig opnieuw te verpakken in een van deze streaming-indelingen. U kunt afspelen met HLS, DASH of Smooth compatibel spelers. U kunt ook [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) om uw stream te testen.

Media Services kunt u uw inhoud dynamisch wordt versleuteld bezorgen (**dynamische versleuteling**) met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.

Indien gewenst, kunt u ook toepassen **dynamisch filteren**, die kan worden gebruikt voor het beheren van de aantal sporen te wissen, indelingen bitsnelheden, die worden verzonden naar de spelers. Media Services biedt ook ondersteuning voor advertentie-invoeging.

### <a name="new-live-encoding-improvements"></a>Nieuwe verbeteringen in Live encoding

De volgende nieuwe verbeteringen zijn uitgevoerd in de nieuwste versie.

- Nieuwe modus voor lage latentie. Zie voor meer informatie, [latentie](#latency).
- Verbeterde RTMP-ondersteuning (verbeterde stabiliteit en meer bron encoder-ondersteuning).
- Beveiligde RTMPS opnemen.

    Als u een LiveEvent maakt, krijgt u 4 URL's voor opnemen. Opname van de 4 URL's zijn bijna identiek, hetzelfde streaming-token (AppId), alleen de poort nummer onderdeel verschilt. Twee van de URL's zijn primaire en back-up voor RTMPS.   
- ondersteuning voor het transcoderen van 24 uur per dag. 
- Verbeterde ondersteuning voor ad-signalering in RTMP via SCTE35.

## <a name="liveevent-types"></a>LiveEvent typen

Een [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: live codering en Pass Through-query. 

### <a name="live-encoding-with-media-services"></a>Live codering met Media Services

![Live encoding](./media/live-streaming/live-encoding.png)

Een on-premises live codering verzendt een single-bitrate stream naar het LiveEvent dat is ingeschakeld voor live coderen met Media Services in een van de volgende protocollen: RTMP of Smooth Streaming (gefragmenteerde MP4). De LiveEvent voert vervolgens live codeert de inkomende single-bitrate stream naar een (adaptieve) multi-bitrate videostream. Desgevraagd levert Media Services de stream aan klanten.

Geef bij het maken van dit type LiveEvent, **Basic** (LiveEventEncodingType.Basic).

### <a name="pass-through"></a>Pass-through

![Pass Through-query](./media/live-streaming/pass-through.png)

Pass Through-query is geoptimaliseerd voor live streams langlopende of 24 x 7 lineair live codering met behulp van een on-premises live codering. De on-premises-codering verzendt multi-bitrate **RTMP** of **Smooth Streaming** (gefragmenteerd MP4) naar de LiveEvent die is geconfigureerd voor **pass-through** levering. De **pass-through** levering vindt plaats wanneer de opgenomen streams **LiveEvent**passeren zonder verdere verwerking. 

Pass Through-LiveEvents kan ondersteuning voor maximaal 4K resolutie en HEVC passeren gebruikt in combinatie met de Smooth Streaming-opnameprotocol. 

Geef bij het maken van dit type LiveEvent, **geen** (LiveEventEncodingType.None).

> [!NOTE]
> Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
> 

## <a name="liveevent-types-comparison"></a>LiveEvent typen vergelijking 

De volgende tabel vergelijkt de functies van de twee LiveEvent typen.

| Functie | Pass Through-LiveEvent | Standard LiveEvent |
| --- | --- | --- |
| Single-bitrate-invoer is gecodeerd in meerdere bitsnelheden in de cloud |Nee |Ja |
| Maximale resolutie, aantal lagen |4Kp30  |720p, 6 lagen, 30 fps |
| Invoer-protocollen |RTMP, Smooth Streaming |RTMP, Smooth Streaming |
| Prijs |Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad "Live Video" |Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) |
| Maximale uitvoeringstijd |24x7 |24x7 |
| Ondersteuning voor slates invoegen |Nee |Ja |
| Ondersteuning voor ad via de API-signalering|Nee |Ja |
| Ondersteuning voor ad via SCTE35 inband-signalering|Ja |Ja |
| Pass Through-CEA 608/708-ondertiteling |Ja |Ja |
| Mogelijkheid om te herstellen van korte vertragingen in bijdrage feed |Ja |Nee (LiveEvent begint slating na 6 + seconden zonder invoergegevens)|
| Ondersteuning voor niet-uniforme invoer GOPs |Ja |Nee – invoer moet worden gecorrigeerd GOPs 2 sec. |
| Ondersteuning voor variabele frame tarief invoer |Ja |Nee – moet dat invoer framesnelheid worden opgelost.<br/>Kleine variaties zijn toegestaan, bijvoorbeeld tijdens hoge beweging schermen. Maar het coderingsprogramma kan niet verwijderen naar 10 frames per seconde. |
| Automatisch-signalen van LiveEvent als invoer-kanaal is verbroken |Nee |Na 12 uur, als er geen LiveOutput uitgevoerd |

## <a name="liveevent-states"></a>LiveEvent Staten 

De huidige status van een LiveEvent. Mogelijke waarden:

|Status|Beschrijving|
|---|---|
|**Gestopt**| Dit is de beginstatus van de LiveEvent nadat het is gemaakt (tenzij autostart is geselecteerd opgegeven.) Er is geen facturering vindt plaats in deze status. In deze status kunnen de eigenschappen van het LiveEvent worden bijgewerkt, maar streaming niet toegestaan.|
|**Starten**| De LiveEvent wordt gestart. Er is geen facturering vindt plaats in deze status. In deze status zijn streaming en updates niet toegestaan. Als een fout optreedt, wordt de LiveEvent teruggezet naar de status gestopt.|
|**Wordt uitgevoerd**| De LiveEvent is kan nu live streams verwerken. Het is nu facturering van gebruik. U moet de LiveEvent om te voorkomen dat verdere facturering stoppen.|
|**Stoppen**| De LiveEvent wordt gestopt. Geen facturering vindt plaats in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.|
|**Verwijderen**| De LiveEvent wordt verwijderd. Geen facturering vindt plaats in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.|

## <a name="liveoutput"></a>LiveOutput

Een [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) kunt u de publicatie, registratie en DVR-vensterinstellingen van de live stream kunt beheren. De relatie LiveEvent en LiveOutput is vergelijkbaar met traditionele media waarbij een kanaal (LiveEvent) is een constante stream met inhoud en een programma (LiveOutput) is afgestemd op bepaalde getimede gebeurtenis op dat LiveEvent.
U kunt opgeven dat het aantal uren dat u wilt behouden van de opgenomen inhoud voor de LiveOutput door in te stellen de **ArchiveWindowLength** eigenschap. **ArchiveWindowLength** is een ISO 8601 timespan-duur van de het archiefvenster (Digital Video Recorder of DVR). Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur. 

**ArchiveWindowLength** een archiefvenster bepaalt ook het maximum aantal wanneer clients terug in tijd kan zoeken vanaf de huidige live positie. LiveOutputs kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elke LiveOutput is gekoppeld aan een [Asset](https://docs.microsoft.com/rest/api/media/assets), en gegevens naar een container in de Azure-opslag die is gekoppeld aan de Media Services-account. Als u wilt de LiveOutput publiceren, moet u een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) voor de gekoppelde asset. Met deze locator kunt u een streaming-URL maken die u aan uw clients kunt leveren.

Een LiveEvent ondersteunt maximaal drie gelijktijdig actieve LiveOutputs zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Bijvoorbeeld: uw bedrijf nodig is voor het uitzenden van een live lineaire feed voor 24 x 7, maar u wilt maken van "opnamen" van programma's gedurende de dag kunnen aanbieden aan klanten als on-demand inhoud voor de weergave bijwerken.  In dit scenario voor u eerst maken een primaire LiveOutput, met een korte archiefvenster van 1 uur of minder voor klanten om af te stemmen in als de primaire live stream. U zou een StreamingLocator maken voor deze LiveOutput en deze publiceren naar uw toepassing of website als de feed 'Live'.  Als de feed wordt uitgevoerd, kunt u een tweede gelijktijdige LiveOutput via een programma maken aan het begin van een weergeven (of 5 minuten vroeg voor aantal ingangen waaruit het later opnieuw.) Deze tweede LiveOutput 5 minuten na het einde van het programma of de gebeurtenis kan worden gestopt en vervolgens kunt u een nieuwe StreamingLocator voor het publiceren van dit programma als een activum op aanvraag in de catalogus van uw toepassing maken.  U kunt Herhaal dit proces meerdere keren voor andere programma grenzen of markeringen die u wilt delen onmiddellijk als op aanvraag, terwijl de 'Live' feed van de eerste LiveOutput blijft om uit te zenden van de lineaire feed.  Bovendien kunt u profiteren van de ondersteuning van dynamisch Filter trim de kop en staart van het archief van de LiveOutput die u voor 'overlapping veiligheid' geïntroduceerd tussen programma's en een meer nauwkeurige begin en einde van de inhoud maar liefst. Gearchiveerde inhoud kan ook worden verstuurd naar een [transformeren](https://docs.microsoft.com/rest/api/media/transforms) voor codering of frame nauwkeurig sub-clipping naar meerdere uitvoerindelingen moet worden gebruikt als syndication naar andere services.

## <a name="streamingendpoint"></a>Streamingendpoint zo

Wanneer de stream naar de LiveEvent stroomt, kunt u de streaminggebeurtenis starten door een Asset, LiveOutput en StreamingLocator te maken. Hiermee wordt de stream gearchiveerd en beschikbaar maken aan kijkers via de [streamingendpoint zo](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Wanneer uw Media Services-account is gemaakt wordt een standaardstreaming-eindpunt wordt toegevoegd aan uw account in de status gestopt. Als u wilt uw inhoud wilt streamen en te profiteren van dynamische pakketten en dynamische versleuteling, is het streaming-eindpunt van waaruit u inhoud streamen wilt in de uitvoeringsstatus van.

## <a name="latency"></a>Latentie

Deze sectie wordt besproken typische resultaten die u ziet wanneer u de instellingen voor lage latentie en verschillende spelers. De resultaten variëren op basis van CDN en netwerklatentie.

De nieuwe LowLatency als functie wilt gebruiken, kunt u instellen de **StreamOptionsFlag** naar **LowLatency** op de LiveEvent. Zodra de stroom actief en werkend is, kunt u de [Azure Media Player](http://ampdemo.azureedge.net/) (en) demo pagina toe en stel de opties voor afspelen 'Met lage latentie methodiek profiel' te gebruiken.

### <a name="pass-through-liveevents"></a>Pass Through-LiveEvents

||2s GOP lage latentie ingeschakeld|1s GOP lage latentie ingeschakeld|
|---|---|---|
|STREEPJES in AMP|per 10|8s versie|
|HLS op systeemeigen iOS-speler|14s|per 10|
|HLS. JS in Mixer-speler|30 s|16s versie|

## <a name="billing"></a>Billing

Een LiveEvent begint zodra de status in 'Actief verandert' facturering. Als u wilt stoppen met het LiveEvent van facturering, moet u de LiveEvent stoppen.

> [!NOTE]
> Wanneer **LiveEventEncodingType** op uw [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) is ingesteld op Basic-, Media Services wordt automatisch signalen eventuele LiveEvent die zich nog in de status 'Running' 12 uur nadat de invoer feed verbroken is en er zijn geen LiveOutputs wordt uitgevoerd. Echter, wordt u nog steeds worden gefactureerd voor de tijd die de LiveEvent in de status 'Running is'.
>

De volgende tabel ziet u hoe LiveEvent Staten toegewezen en de facturering.

| LiveEvent status | Is het facturering? |
| --- | --- |
| Starten |Nee (overgangsstatus) |
| In uitvoering |JA |
| Stoppen |Nee (overgangsstatus) |
| Gestopt |Nee |

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)
