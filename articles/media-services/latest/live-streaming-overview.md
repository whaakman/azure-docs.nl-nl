---
title: Overzicht van Live streamen met Azure Media Services | Microsoft Docs
description: Dit onderwerp biedt een overzicht van live streamen met Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/06/2018
ms.author: juliako
ms.openlocfilehash: b8c9375d8ad915200cbc8b2e1a62979fd1b7d179
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238269"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Live streamen met Azure Media Services v3

Live streaming-gebeurtenissen met Azure Media Services leveren betrokken de volgende onderdelen zijn meestal:

* Een camera die wordt gebruikt voor het uitzenden van een gebeurtenis.
* Een coderingsprogramma voor live video dat signalen van de camera (of een ander apparaat, zoals een laptop) converteert naar stromen die worden verzonden naar de Media Services live streaming service. De signalen ook bevatten reclame SCTE 35 en Ad-hints. 
* De Media Services service voor live streamen kunt u opnemen, bekijken, pakket, opnemen, coderen en zenden van de inhoud op uw klanten, of op een CDN voor verdere distributie.

In dit artikel biedt een gedetailleerd overzicht en schema's van de belangrijkste onderdelen die zijn betrokken bij live streamen met Media Services bevat.

## <a name="overview-of-main-components"></a>Overzicht van de belangrijkste onderdelen

In Media Services [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) zijn verantwoordelijk voor het verwerken van live streaming inhoud. Een LiveEvent biedt een invoereindpunt (de URL voor opnemen) dat u vervolgens met een on-premises live codering opgeven. De LiveEvent live invoer gegevensstromen ontvangt van het live coderingsprogramma in RTMP of Smooth Streaming-indeling en beschikbaar voor streaming via een of meer [streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints). Een [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) kunt u de publicatie, opnemen en DVR vensterinstellingen van de live stream. De LiveEvent biedt ook een preview-eindpunt (preview URL) die u gebruikt om te bekijken en valideren van uw stream voor verdere verwerking en levering. 

Media Services biedt **dynamische pakketten**, waarmee u kunt bekijken en uw inhoud in MPEG DASH, HLS, Smooth Streaming-broadcast streaming-indelingen die handmatig opnieuw te verpakken in een van deze streaming-indelingen. U kunt afspelen met HLS, DASH of Smooth compatibel spelers. U kunt ook [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) om uw stream te testen.

Media Services kunt u de inhoud versleuteld dynamisch bezorgen (**dynamische versleuteling**) met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digitale rechten (DRM) beheersystemen: Microsoft PlayReady Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties (PlayReady, Widevine en FairPlay) naar geautoriseerde clients.

Indien gewenst, kunt u ook toepassen **dynamisch filteren**, die kan worden gebruikt om te bepalen het aantal houdt, indelingen, bitsnelheden die worden verzonden naar de spelers. Media Services biedt ook ondersteuning voor ad-invoegen.


## <a name="liveevent-types"></a>LiveEvent typen

Een [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) een van twee typen: live codering en Pass Through-query. 

### <a name="live-encoding-with-media-services"></a>Live codering met Media Services

![Live codering](./media/live-streaming/live-encoding.png)

Een on-premises live codering verzendt een single-bitrate stream naar het LiveEvent dat is ingeschakeld voor live codering met Media Services in een van de volgende protocollen kan uitvoeren: RTMP of Smooth Streaming (gefragmenteerde MP4). De LiveEvent voert live codering van de binnenkomende single-bitrate stream naar een (adaptieve) multi-bitrate videostream. Desgevraagd levert Media Services de stream aan klanten.

Geef bij het maken van dit type LiveEvent **Basic** (LiveEventEncodingType.Basic).

### <a name="pass-through"></a>Pass-through

![Pass Through-query](./media/live-streaming/pass-through.png)

Pass Through-query is geoptimaliseerd voor live gegevensstromen langlopende of 24 x 7 lineaire live codering met behulp van een on-premises live codering. De on-premises-codering verzendt multi-bitrate **RTMP** of **Smooth Streaming** (gefragmenteerd MP4) naar de LiveEvent die is geconfigureerd voor **pass-through** levering. De **pass-through** levering vindt plaats wanneer de opgenomen streams doorgeven **LiveEvent**s zonder verdere verwerking. 

Pass Through-LiveEvents kunt ondersteuning voor maximaal 4K-resolutie en HEVC doorgeven gebruikt in combinatie met Smooth Streaming-opnameprotocol. 

Geef bij het maken van dit type LiveEvent **geen** (LiveEventEncodingType.None).

> [!NOTE]
> Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
> 

## <a name="liveevent-types-comparison"></a>Vergelijking van de typen LiveEvent 

De volgende tabel vergelijkt de functies van de twee LiveEvent typen.

| Functie | Pass Through-LiveEvent | Basic LiveEvent |
| --- | --- | --- |
| Single-bitrate-invoer is gecodeerd in meerdere bitsnelheden in de cloud |Nee |Ja |
| Maximale resolutie van het aantal lagen |4Kp30  |720p, 6 lagen 30 fps |
| Invoer-protocollen |RTMP, Smooth Streaming |RTMP, Smooth Streaming |
| Prijs |Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad 'Live Video' |Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) |
| Maximale uitvoeringstijd |24x7 |24x7 |
| Ondersteuning voor het invoegen van slates |Nee |Ja |
| Ondersteuning voor ad-signalering via API|Nee |Ja |
| Ondersteuning voor ad-signalering via SCTE35 inband|Ja |Ja |
| Pass Through-CEA 608/708 bijschriften |Ja |Ja |
| Mogelijkheid om te herstellen vanaf korte vertragingen in bijdrage feed |Ja |Nee (LiveEvent beginnen slating na 6 + seconden zonder invoergegevens)|
| Ondersteuning voor niet-uniforme GOPs invoer |Ja |Nee, invoer moet worden gecorrigeerd 2 sec GOPs |
| Ondersteuning voor variabele frame snelheid invoer |Ja |Nee, moet dat invoer framesnelheid worden vastgesteld.<br/>Secundaire variaties zijn toegestaan, bijvoorbeeld tijdens hoge beweging schermen. Maar codering kan niet verwijderen naar 10 frames per seconde. |
| Automatische-signalen van LiveEvent wanneer invoer feed is verbroken |Nee |Na 12 uur, als er geen LiveOutput uitgevoerd |

## <a name="liveevent-states"></a>LiveEvent statussen 

De huidige status van een LiveEvent. Mogelijke waarden:

|Status|Beschrijving|
|---|---|
|**Gestopt**| Dit is de beginstatus van de LiveEvent na het maken ervan (tenzij autostart geselecteerd is opgegeven.) Er is geen facturering treedt op in deze staat. In deze toestand is, de LiveEvent-eigenschappen kunnen worden bijgewerkt, maar streaming is niet toegestaan.|
|**Starten**| De LiveEvent wordt gestart. Er is geen facturering treedt op in deze staat. In deze status zijn streaming en updates niet toegestaan. Als een fout optreedt, wordt de LiveEvent de status gestopt.|
|**Uitgevoerd**| De LiveEvent is geschikt voor het verwerken van live gegevensstromen. Het is nu gebruik facturering. U moet de LiveEvent om te voorkomen dat verdere facturering stoppen.|
|**Stoppen**| De LiveEvent wordt gestopt. Er is geen facturering treedt op in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.|
|**Verwijderen**| De LiveEvent wordt verwijderd. Er is geen facturering treedt op in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.|

## <a name="liveoutput"></a>LiveOutput

Een [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) kunt u de publicatie, opnemen en DVR vensterinstellingen van de live stream. De relatie LiveEvent en LiveOutput is vergelijkbaar met traditionele media waarbij een kanaal (LiveEvent) is een constante stream met inhoud en een programma (LiveOutput) is afgestemd op een bepaalde getimede gebeurtenis op dat LiveEvent.
U kunt opgeven dat het aantal uren dat u de opgenomen inhoud voor de LiveOutput behouden wilt door de **ArchiveWindowLength** eigenschap. **ArchiveWindowLength** is een ISO 8601 timespan-duur van de lengte van een archiefvenster (digitale Video doos of DVR). Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur. 

**ArchiveWindowLength** bepaalt ook het maximum aantal van de tijd die clients terug in tijd kan zoeken vanaf de huidige live positie. LiveOutputs kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elke LiveOutput is gekoppeld aan een [Asset](https://docs.microsoft.com/rest/api/media/assets), en registreert de gegevens in een container in Azure-opslag gekoppeld aan het Media Services-account. Als u wilt de LiveOutput publiceren, moet u een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) voor de gekoppelde asset. Met deze locator kunt u een streaming-URL maken die u aan uw clients kunt leveren.

Een LiveEvent ondersteunt maximaal drie gelijktijdig actieve LiveOutputs zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Bijvoorbeeld: uw bedrijf nodig is voor het uitzenden van een feed 24 x 7 live lineaire, maar u wilt maken 'opnamen' van programma's gedurende de dag aanbieden aan klanten als de inhoud op aanvraag voor weergave bijwerken.  In dit scenario eerst hebt u een primaire LiveOutput met een korte archiefvenster van 1 uur of minder voor klanten om af te stemmen in als de primaire live stream. U zou een StreamingLocator maken voor deze LiveOutput en deze publiceren naar uw toepassing of een website als de feed 'Live'.  Als de feed wordt uitgevoerd, kunt u een tweede gelijktijdige LiveOutput programmatisch maken aan het begin van een programma (of 5 minuten vroeg om te bieden een aantal ingangen later trim.) Deze tweede LiveOutput 5 minuten na het einde van het programma of de gebeurtenis kan worden gestopt en vervolgens kunt u een nieuwe StreamingLocator voor het publiceren van dit programma als een actief op aanvraag in de catalogus van uw toepassing maken.  U kunt dit proces herhalen meerdere keren voor andere programma grenzen of kenmerken dat u wilt delen onmiddellijk als op aanvraag, alle terwijl de 'Live' feed vanuit de eerste LiveOutput blijft de lineaire feed-broadcast.  Bovendien kunt u profiteren van de ondersteuning voor dynamische Filter trim de hoofd- en tail van het archief van de LiveOutput die u voor 'overlapping veiligheid' geïntroduceerd tussen programma's en het bereiken van een meer nauwkeurige begin en einde van de inhoud. Gearchiveerde inhoud kan ook worden verstuurd naar een [transformeren](https://docs.microsoft.com/rest/api/media/transforms) voor codering of frame nauwkeurige subclipping naar meerdere uitvoerindelingen moet worden gebruikt als extern gepubliceerd met andere services.

## <a name="streamingendpoint"></a>StreamingEndpoint

Zodra u de stroom die binnenkomen in de LiveEvent hebt, kunt u de streaminggebeurtenis starten door een Asset, LiveOutput en StreamingLocator maken. Hiermee wordt de stream gearchiveerd en beschikbaar gesteld aan kijkers via de [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Wanneer uw Media Services-account is gemaakt wordt een standaard streaming-eindpunt toegevoegd aan uw account in de status gestopt. Om te beginnen uw inhoud te streamen en te profiteren van dynamische pakketten en dynamische versleuteling, heeft het streaming-eindpunt van waaruit u inhoud streamen wilt de status wordt uitgevoerd.

## <a name="billing"></a>Billing

Een LiveEvent begint zodra de status in 'Actief verandert' facturering. Als u wilt stoppen met het LiveEvent van facturering, die u moet de LiveEvent stoppen.

> [!NOTE]
> Wanneer **LiveEventEncodingType** op uw [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) is ingesteld op het Basislidmaatschap, Media Services automatisch signalen eventuele LiveEvent die zich nog in de status 'Actief' 12 uur nadat de invoer feed verloren en wordt er zijn geen LiveOutputs die wordt uitgevoerd. Echter, wordt u nog steeds worden gefactureerd voor de tijd die de LiveEvent in de status 'Actief is'.
>

De volgende tabel ziet hoe LiveEvent statussen worden toegewezen aan de modus voor facturering.

| LiveEvent status | Is het facturering? |
| --- | --- |
| Starten |Nee (overgangsstatus) |
| In uitvoering |JA |
| Stoppen |Nee (overgangsstatus) |
| Gestopt |Nee |

## <a name="next-steps"></a>Volgende stappen

[Live streaming-zelfstudie](stream-live-tutorial-with-api.md)
