---
title: Overzicht van Live streamen met Azure Media Services | Microsoft Docs
description: In dit onderwerp geeft een overzicht van live streamen met Azure Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: fb63502e-914d-4c1f-853c-4a7831bb08e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 6f500f25129470a679c75cae6cd1abc9d71b72a7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-live-streaming-using-azure-media-services"></a>Overzicht van Live streamen met Azure Media Services
## <a name="overview"></a>Overzicht
Live streaming-gebeurtenissen met Azure Media Services leveren betrokken de volgende onderdelen zijn meestal:

* Een camera die wordt gebruikt voor het uitzenden van een gebeurtenis.
* Een coderingsprogramma voor live video dat de signalen van de camera converteert naar stromen die worden verzonden naar een service voor live streamen.

    Eventueel meerdere op tijd gesynchroniseerde coderingsprogramma’s. Voor bepaalde kritieke live gebeurtenissen die een zeer hoge beschikbaarheid en kwaliteit vereisen, kunt u het beste redundante coderingsprogramma’s (actief/actief) met tijdsynchronisatie gebruiken voor een naadloze failover zonder verlies van gegevens.
* Een service voor live streamen waarmee u het volgende kunt doen:

  * Live-inhoud met verschillende protocollen voor live streamen opnemen (bijvoorbeeld RTMP of Smooth Streaming).
  * (Optioneel) Uw stream coderen in een stream met een adaptieve bitsnelheid.
  * Een voorbeeld van uw livestream bekijken.
  * De opgenomen inhoud vastleggen en opnemen om deze later te streamen (Video-on-Demand).
  * De inhoud rechtstreeks aan uw klanten leveren via algemene protocollen voor streaming (bijvoorbeeld MPEG DASH, Smooth of HLS) of aan een Content Delivery Network (CDN) voor verdere distributie.

Met **Microsoft Azure Media Services** (AMS) kunt u de inhoud van uw live-streaming opnemen, coderen, een voorbeeld bekijken, opslaan en leveren.

Als u uw inhoud levert aan klanten, is het uw doel een video van hoge kwaliteit te leveren aan verschillende apparaten met verschillende netwerkomstandigheden. Om dit te bereiken, live coderingsprogramma's te gebruiken voor het coderen van uw stream naar een multi-bitrate (adaptieve bitrate) videostream.  Als u op verschillende apparaten wilt streamen, gebruikt u de [dynamische pakketten](media-services-dynamic-packaging-overview.md) van Media Services om uw stream met verschillende protocollen op dynamische wijze opnieuw toe te voegen aan een pakket. Media Services ondersteunt de levering van de volgende Adaptive Bitrate Streaming-technologieën: HLS (HTTP Live Streaming), Smooth Streaming, MPEG DASH.

In Azure Media Services wordt alle functionaliteiten voor live streamen afgehandeld door **kanalen**, **programma's** en **streaming-eindpunten**, waaronder opnemen, opmaken, DVR, beveiliging, schaalbaarheid en redundantie.

Een **kanaal** vertegenwoordigt een pijplijn voor de verwerking van inhoud voor live-streaming. Een kanaal kan op de volgende manieren live invoerstromen ontvangen:

* Een on-premises live codering verzendt multi-bitrate **RTMP** of **Smooth Streaming** (gefragmenteerd MP4) naar het kanaal dat is geconfigureerd voor **passthrough**-levering. **Passthrough**-levering vindt plaats wanneer de opgenomen streams het **kanaal** passeren zonder verdere verwerking. U kunt de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming uitvoeren: MediaExcel, Ateme, stel communicatie, Envivio, Cisco en Elemental. De volgende live coderingsprogramma's voeren RTMP: Adobe Flash Media Live coderingsprogramma (FMLE), Telestream Wirecast, Haivision, Teradek en Tricaster-transcoders.  Een live coderingsprogramma kan ook een stream met één bitsnelheid verzenden naar een kanaal dat niet is ingeschakeld voor Live Encoding, maar dit wordt niet aanbevolen. Desgevraagd levert Media Services de stream aan klanten.

  > [!NOTE]
  > Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
  > 
  > 
* Een on-premises live codering verzendt een single-bitrate stream naar het kanaal dat is ingeschakeld voor het uitvoeren van live codering met Media Services in een van de volgende indelingen: RTMP of Smooth Streaming (gefragmenteerde MP4). RTP (MPEG-TS) wordt ook ondersteund, mits u een specifieke verbinding met de Azure-datacenter hebben. De volgende live coderingsprogramma's met RTMP uitvoer bekend zijn bij het werken met kanalen van dit type: Telestream Wirecast, FMLE. Het kanaal codeert de inkomende single-bitrate stream vervolgens live naar een (adaptieve) multi-bitrate videostream. Desgevraagd levert Media Services de stream aan klanten.

Beginnen met de release van Media Services 2.10 wanneer u een kanaal maakt, kunt u op welke manier u wilt dat voor dit kanaal voor het ontvangen van de invoerstroom en of u wilt voor het kanaal live codering van uw stream uitvoeren. U hebt hiervoor twee opties:

* **Geen** (pass-through): deze waarde opgeven als u gebruiken van een on-premises live coderingsprogramma die multi-bitrate stream (Pass Through-stream wilt) wordt uitgevoerd. In dit geval wordt de stroom inkomende doorgegeven aan de uitvoer zonder alle codering. Dit is het gedrag van een kanaal vóór 2,10 release.  
* **Standaard** – Selecteer deze waarde als u van plan bent uw single-bitrate live stream naar een multi-bitrate stream coderen met Media Services. Deze methode is voor het snel voor incidentele gebeurtenissen omhoog schalen voordeliger. Let erop dat er een facturering invloed voor live codering en u rekening mee houden moet dat als u een kanaal voor live codering in de status 'Actief' wordt facturering worden kosten in rekening.  Het is raadzaam om uw actieve kanalen onmiddellijk te beëindigen nadat uw live-streaming gebeurtenis is voltooid om extra per uur kosten te voorkomen.

## <a name="comparison-of-channel-types"></a>Vergelijking van kanaaltypen
Volgende tabel bevat een handleiding voor het vergelijken van de twee kanaal-typen worden ondersteund in Media Services

| Functie | Pass Through-kanaal | Standard-kanaal |
| --- | --- | --- |
| Single-bitrate-invoer is gecodeerd in meerdere bitsnelheden in de cloud |Nee |Ja |
| Maximale resolutie van het aantal lagen |1080p, 8 lagen 60 + fps |720p, 6 lagen 30 fps |
| Invoer-protocollen |RTMP, Smooth Streaming |RTMP, Smooth Streaming- en RTP |
| Prijs |Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad 'Live Video' |Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) |
| Maximale uitvoeringstijd |24 x 7 |8 uur |
| Ondersteuning voor het invoegen van slates |Nee |Ja |
| Ondersteuning voor ad-signalering |Nee |Ja |
| Pass Through-CEA 608/708 bijschriften |Ja |Ja |
| Mogelijkheid om te herstellen vanaf korte vertragingen in bijdrage feed |Ja |Nee (kanaal beginnen slating na 6 + seconden zonder invoergegevens) |
| Ondersteuning voor niet-uniforme GOPs invoer |Ja |Nee, invoer moet worden gecorrigeerd 2sec GOPs |
| Ondersteuning voor variabele frame snelheid invoer |Ja |Nee, moet dat invoer framesnelheid worden vastgesteld.<br/>Secundaire variaties zijn toegestaan, bijvoorbeeld tijdens hoge beweging schermen. Maar codering kan niet verwijderen naar 10 frames per seconde. |
| Automatische-signalen kanalen als invoer feed is verbroken |Nee |Na 12 uur, als er is geen programma uitvoeren |

## <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Werken met kanalen die een multi-bitrate livestream van on-premises encoders ontvangen (pass-through)
Het volgende diagram toont de belangrijkste onderdelen van het AMS-platform die betrokken zijn bij de **passthrough**-werkstroom.

![Live-werkstroom](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Zie [Werken met kanalen die een multi-bitrate livestream van on-premises coderingsprogramma’s ontvangen](media-services-live-streaming-with-onprem-encoders.md) voor meer informatie.

## <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services
Het volgende diagram toont de belangrijke onderdelen van het AMS-platform die betrokken zijn bij de Live Streaming-werkstroom waarbij een kanaal live codering kan uitvoeren met Media Services.

![Live-werkstroom](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Zie [Werken met kanalen waarmee Live Encoding kan worden uitgevoerd met Azure Media Services](media-services-manage-live-encoder-enabled-channels.md) voor meer informatie.

## <a name="description-of-a-channel-and-its-related-components"></a>Beschrijving van een kanaal en de bijbehorende onderdelen
### <a name="channel"></a>Kanaal
In Media Services [kanaal](https://docs.microsoft.com/rest/api/media/operations/channel)s zijn verantwoordelijk voor het verwerken van live streaming inhoud. Een kanaal biedt een invoereindpunt (de URL voor opnemen) dat u vervolgens aan een live transcoder leveren. Het kanaal live invoer gegevensstromen ontvangt van de live transcoder en wordt het beschikbaar gemaakt voor streaming via een of meer streaming-eindpunten. Kanalen bieden ook een preview-eindpunt (preview URL) die u gebruikt om te bekijken en valideren van uw stream voor verdere verwerking en levering.

U kunt de URL voor opnemen en de voorbeeld-URL ophalen bij het maken van het kanaal. Als u deze URL's, moet het kanaal niet worden gestart. Wanneer u klaar bent om te pushen van gegevens van een live transcoder in het kanaal, kan het kanaal moet worden gestart. Zodra de live transcoder ophalen van gegevens start, kunt u uw stream te bekijken.

Elke Media Services-account kan meerdere kanalen, meerdere programma's en meerdere streaming-eindpunten bevatten. Afhankelijk van de behoeften van de bandbreedte en beveiliging, kunnen de StreamingEndpoint services worden toegewezen aan een of meer kanalen. Alle StreamingEndpoint kunt halen uit een kanaal.

### <a name="program"></a>Programma
Een [programma](https://docs.microsoft.com/rest/api/media/operations/program) kunt u het publiceren en opslaan van segmenten in een live stream. Kanalen beheren programma's. De kanaal-/programmarelatie lijkt erg op traditionele media waarbij een kanaal een constante stream met inhoud heeft en een programma is afgestemd op een bepaalde getimede gebeurtenis op dat kanaal.
U kunt opgeven dat het aantal uren dat u behouden van de opgenomen inhoud voor het programma wilt door de **ArchiveWindowLength** eigenschap. Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur.

ArchiveWindowLength bepaalt ook dat de maximale hoeveelheid tijd die clients terug in tijd kan zoeken vanaf de huidige live positie. Programma's kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elk programma is gekoppeld aan een asset. U moet een locator voor de gekoppelde asset maken voor het publiceren van het programma. Met deze locator kunt u een streaming-URL maken die u aan uw clients kunt leveren.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Zo kan het voor uw bedrijf nodig zijn zes uur van een programma te archiveren, maar alleen de laatste tien minuten uit te zenden. Hiervoor moet u twee gelijktijdig actieve programma's maken. Het ene programma wordt ingesteld om zes uur van de gebeurtenis te archiveren, maar het programma wordt niet gepubliceerd. Het andere programma wordt ingesteld om tien minuten te archiveren en dit programma wordt wel gepubliceerd.

## <a name="billing-implications"></a>De implicaties van facturering
Een kanaal begint zodra het statusovergangen naar 'Actief' via de API is facturering.  

De volgende tabel ziet hoe kanaal statussen worden toegewezen aan facturering statussen in de API en de Azure portal. Houd er rekening mee dat de statussen enigszins verschillen tussen de API en de Portal UX zijn Als u een kanaal is in de status 'Actief' via de API of met de status 'Gereed' of 'Streaming' in de Azure portal facturering actief zijn.

Als u wilt stoppen met het kanaal van u verdere facturering, hebt u Stop het kanaal via de API of in de Azure-portal.
U bent zelf verantwoordelijk voor het stoppen van de kanalen wanneer u klaar bent met het kanaal. Stop het kanaal is mislukt, leidt tot voortdurende facturering.

> [!NOTE]
> Als u werkt met standaard kanalen, automatisch AMS signalen een kanaal dat nog in de status 'Actief' 12 uur nadat de invoer feed verloren en wordt er zijn geen programma's uitvoeren. Echter, wordt u nog steeds worden gefactureerd voor de tijd die het kanaal in de status 'Actief is'.
>
>

### <a id="states"></a>Kanaal statussen en hoe ze worden toegewezen aan de facturering modus
De huidige status van een Kanaal. Mogelijke waarden:

* **Gestopt**. Dit is de initiële status van het kanaal na het maken ervan (tenzij autostart is geselecteerd in de portal.) Er is geen facturering treedt op in deze staat. In deze status kunnen de eigenschappen van het Kanaal worden bijgewerkt, maar is streaming niet toegestaan.
* **Starten van**. Het Kanaal wordt gestart. Er is geen facturering treedt op in deze staat. In deze status zijn streaming en updates niet toegestaan. Als er een fout optreedt, wordt het Kanaal teruggezet naar de status Stopped.
* **Met**. Het Kanaal kan nu live streams verwerken. Het is nu gebruik facturering. U moet het kanaal om te voorkomen dat verdere facturering stoppen.
* **Stoppen van**. Het Kanaal wordt gestopt. Er is geen facturering treedt op in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.
* **Verwijderen van**. Het Kanaal wordt verwijderd. Er is geen facturering treedt op in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.

In de volgende tabel wordt het verband tussen de verschillende Kanaalstatussen en de facturering weergegeven.

| Kanaalstatus | Portal UI-indicatoren | Is het facturering? |
| --- | --- | --- |
| Starting |Starting |Nee (overgangsstatus) |
| Running |Ready (er worden geen programma's uitgevoerd)<br/>of<br/>Streaming (er wordt ten minste een programma uitgevoerd) |JA |
| Stopping |Stopping |Nee (overgangsstatus) |
| Stopped |Stopped |Nee |

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen
[Azure mediaservices gefragmenteerde MP4 Live opnemen specificatie](media-services-fmp4-live-ingest-overview.md)

[Werken met kanalen die voor het uitvoeren van Live codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)

[Werken met kanalen die een multi-bitrate livestream van on-premises encoders ontvangen](media-services-live-streaming-with-onprem-encoders.md)

[Quota's en beperkingen](media-services-quotas-and-limitations.md).  

[Media Services-concepten](media-services-concepts.md)
