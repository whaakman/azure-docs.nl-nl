---
title: Stream live met on-premises-coderingsprogramma's die multi-bitrate streams - Azure maken | Microsoft Docs
description: 'Dit onderwerp wordt beschreven hoe u een kanaal dat een multi-bitrate live stream van een on-premises coderingsprogramma ontvangt instelt. De stroom kan vervolgens worden afgeleverd bij afspelen clienttoepassingen via een of meer streaming-eindpunten, met een van de volgende protocollen voor adaptief streamen: HLS, Smooth Streaming streepje.'
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 04/12/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 3f6569d32708c42247e0ffec70389f2e0f07389e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="live-streaming-with-on-premises-encoders-that-create-multi-bitrate-streams"></a>Live streamen met on-premises-coderingsprogramma's die multi-bitrate streams maken
## <a name="overview"></a>Overzicht
In Azure Media Services een *kanaal* vertegenwoordigt een pijplijn voor het verwerken van inhoud live-streaming. Een kanaal ontvangt live invoer gegevensstromen in twee manieren:

* Een lokale live codering verzendt een multi-bitrate RTMP of Smooth Streaming (gefragmenteerde MP4) stream naar het kanaal dat niet is ingeschakeld voor het uitvoeren van live codering met Media Services. De opgenomen streams doorgeven kanalen zonder verdere verwerking. Deze methode wordt aangeroepen *pass-through*. U kunt de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming als uitvoer hebt: Media Excel, Ateme, stel communicatie, Envivio, Cisco en Elemental. De volgende live coderingsprogramma's hebben RTMP als uitvoer: Adobe Flash Live Mediacoderingsprogramma, Telestream Wirecast Haivision, Teradek en TriCaster. Een live coderingsprogramma kan ook een single-bitrate stream naar een kanaal dat niet is ingeschakeld voor live codering verzenden, maar die niet aanbevolen. De stroom levert Media Services aan klanten die deze vragen.

  > [!NOTE]
  > Een Pass Through-methode is de meest voordelige manier om live te streamen.


* Een on-premises live codering verzendt een single-bitrate stream naar het kanaal dat is ingeschakeld voor het uitvoeren van live codering met Media Services in een van de volgende indelingen: RTP (MPEG-TS), RTMP of Smooth Streaming (gefragmenteerde MP4). Het kanaal voert vervolgens live codering van de binnenkomende single-bitrate stream naar een multi-bitrate (adaptieve) videostream. De stroom levert Media Services aan klanten die deze vragen.

Beginnen met de release van Media Services 2.10 wanneer u een kanaal maakt, kunt u opgeven hoe u uw kanaal voor het ontvangen van de invoerstroom wilt. U kunt ook opgeven of u wilt dat het kanaal live codering van de stroom kan uitvoeren. U hebt hiervoor twee opties:

* **Doorgeven**: deze waarde opgeven als u wilt gebruiken van een on-premises live coderingsprogramma waarvoor een multi-bitrate stream (Pass Through-stream) als uitvoer. In dit geval wordt doorstuurt de stroom inkomende naar de uitvoer zonder alle codering. Dit is het gedrag van een kanaal vóór de release 2,10. In dit onderwerp biedt informatie over het werken met kanalen van dit type.
* **Live Encoding**: Kies deze waarde als u van plan bent uw single-bitrate live stream naar een multi-bitrate stream coderen met Media Services. Houd er rekening mee dat als u een live codering kanaal in een **met** staat, worden kosten. Het is raadzaam om uw actieve kanalen onmiddellijk te beëindigen nadat uw live-streaming-gebeurtenis is voltooid om extra per uur kosten te voorkomen. De stroom levert Media Services aan klanten die deze vragen.

> [!NOTE]
> In dit onderwerp wordt beschreven kenmerken van de kanalen die niet zijn ingeschakeld voor live codering kan uitvoeren. Zie voor meer informatie over het werken met kanalen die zijn ingeschakeld voor het uitvoeren van live codering [Live streamen met Azure Media Services om multi-bitrate streams te maken](media-services-manage-live-encoder-enabled-channels.md).
>
>

Het onderstaande diagram ziet u een live-streaming-werkstroom die gebruikmaakt van een on-premises live coderingsprogramma om multi-bitrate RTMP of gefragmenteerde MP4 (Smooth Streaming) streams als uitvoer.

![Live-werkstroom][live-overview]

## <a id="scenario"></a>Algemeen scenario voor live-streaming
De volgende stappen beschrijven taken voor het maken van algemene toepassingen voor live-streaming.

1. Sluit een videocamera aan op een computer. Start en configureer een on-premises live coderingsprogramma dat een multi-bitrate RTMP of gefragmenteerde MP4 (Smooth Streaming) stroom als uitvoer. Zie [Azure Media Services RTMP-ondersteuning en live coderingsprogramma's](http://go.microsoft.com/fwlink/?LinkId=532824) voor meer informatie.

    U kunt deze stap ook uitvoeren nadat u uw kanaal hebt gemaakt.
2. Een kanaal maken en starten.

3. URL voor opnemen ophalen het kanaal.

    Het live coderingsprogramma gebruikt de URL voor opnemen voor het verzenden van de stream naar het kanaal.
4. De URL van de preview kanaal niet ophalen.

    Gebruik deze URL om te controleren of de livestream goed door het kanaal wordt ontvangen.
5. Maak een programma.

    Wanneer u de Azure-portal gebruikt, een programma maken ook een asset gemaakt.

    Wanneer u de SDK voor .NET of REST gebruiken, moet u een asset maken en geef dit activum worden gebruikt bij het maken van een programma.
6. Publiceer de asset die is gekoppeld aan het programma.   

    >[!NOTE]
    >Wanneer uw Azure Media Services-account is gemaakt, een **standaard** streaming-eindpunt wordt toegevoegd aan uw account in de **gestopt** status. Het streaming-eindpunt van waar u inhoud wilt streamen, moet de status **Wordt uitgevoerd** hebben.

7. Start het programma wanneer u klaar bent om te streamen en te archiveren.

8. Het live coderingsprogramma kan desgewenst een signaal ontvangen dat een advertentie moet worden gestart. De advertentie wordt ingevoegd in de uitvoerstream.

9. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren.

10. Verwijder het programma (en verwijder desgewenst de asset).     

## <a id="channel"></a>Beschrijving van een kanaal en de bijbehorende onderdelen
### <a id="channel_input"></a>Channel-invoer (opnemen) configuraties
#### <a id="ingest_protocols"></a>Streaming-opnameprotocol
Media Services ondersteunt live-feeds opnemen met behulp van multi-bitrate gefragmenteerd MP4 en multi-bitrate RTMP als protocollen voor streaming. Wanneer het RTMP opnemen streaming-protocol is ingeschakeld, worden twee eindpunten (invoer) opnemen voor het kanaal worden gemaakt:

* **Primaire URL**: Hiermee geeft u de volledige URL van het kanaal primaire RTMP eindpunt opnemen.
* **Secundaire URL** (optioneel): Hiermee geeft u de volledige URL van het kanaal secundaire RTMP eindpunt opnemen.

Gebruik de secundaire URL als u wilt verbeteren de duurzaamheid en Foutentolerantie van uw opnemen stroom (evenals encoder failover en Foutentolerantie), met name voor de volgende scenario's:

- Één encoder double pushen naar primaire en secundaire URL's:

    Het belangrijkste doel van dit scenario is om meer tolerantie netwerk schommelingen en geleidelijk te bieden. Sommige RTMP coderingsprogramma's niet verwerken netwerk goed wordt verbroken. Als een netwerk verbinding verbreken gebeurt, kan een coderingsprogramma stoppen met coderen en verzend de buffer opgeslagen gegevens niet in als een reconnect gebeurt. Dit zorgt ervoor dat wijzigingen en verlies van gegevens. Netwerk wordt verbroken vanwege een onjuiste netwerk of onderhoud kan gebeuren de Azure-zijde. Primaire en secundaire URL's verlagen van de problemen met het netwerk en voorzien in een gecontroleerde upgradeproces. Telkens wanneer een geplande netwerk verbinding verbreken gebeurt, Media Services beheert de primaire en secundaire verbroken en biedt een vertraagde verbreken tussen de twee. Coderingsprogramma's hebt tijd voor het verzenden van gegevens behouden en het opnieuw. De volgorde van de verbinding verbreekt kan willekeurig, maar er is een vertraging optreden tussen primaire en secundaire of secundaire/primaire URL's. In dit scenario is het coderingsprogramma nog steeds de storingspunt.

- Meerdere encoders met elke encoder pushen naar een toegewezen:

    Dit scenario biedt zowel codering en redundantie voor opnemen. In dit scenario encoder1 verstuurd naar de URL van de primaire en encoder2 verstuurd naar de secundaire URL. Wanneer een coderingsprogramma is mislukt, kunt de andere encoder houden verzenden van gegevens. Redundantie van de gegevens kan worden beheerd omdat Media Services niet primaire en secundaire URL's op hetzelfde moment verbroken wordt. Dit scenario veronderstelt dat coderingsprogramma's gesynchroniseerd zijn en dezelfde gegevens bieden.  

- Meerdere encoders double pushen naar primaire en secundaire URL's:

    In dit scenario push-beide coderingsprogramma's gegevens naar de primaire en secundaire URL's. Dit biedt de beste betrouwbaarheid en fouttolerantie, evenals gegevensredundantie. In dit scenario kan tolereren beide storingen van de codering en de verbinding verbreekt, zelfs als een encoder niet meer werkt. Er wordt vanuit gegaan dat coderingsprogramma's gesynchroniseerd zijn en dezelfde gegevens bieden.  

Zie voor meer informatie over het live coderingsprogramma's RTMP [Azure Media Services RTMP-ondersteuning en Live coderingsprogramma's](http://go.microsoft.com/fwlink/?LinkId=532824).

#### <a name="ingest-urls-endpoints"></a>URL's (eindpunten) voor opnemen
Een kanaal biedt een invoereindpunt (de URL voor opnemen) dat u in het live coderingsprogramma opgeeft zodat het coderingsprogramma kunt push streams in de kanalen.   

Wanneer u het kanaal hebt gemaakt, kunt u de URL voor opnemen ophalen. U kunt deze URL's ophalen, het kanaal heeft geen zich in de **met** status. Wanneer u klaar om te beginnen met het kanaal-gegevens worden gepusht, wordt het kanaal moet in de **met** status. Nadat het kanaal wordt gestart ophalen van gegevens, kunt u uw stream via de URL van de preview bekijken.

Hebt u een optie voor het opnemen van een gefragmenteerde MP4 (Smooth Streaming) livestream via een SSL-verbinding. Als u wilt opnemen via SSL, zorg voor het bijwerken van de URL voor opnemen naar HTTPS. U kan niet op dit moment RTMP opnemen via SSL.

#### <a id="keyframe_interval"></a>Keyframe interval
Wanneer u een on-premises live coderingsprogramma gebruikt voor het genereren van multi-bitrate stream, bevat het interval keyframe de duur van de groep van afbeeldingen (GOP) gebruikt door deze externe encoder. Nadat het kanaal heeft deze binnenkomende stroom ontvangen, kunt u uw live stream kunt leveren voor afspelen van clienttoepassingen op een van de volgende indelingen: Smooth Streaming, dynamische adaptief streamen via HTTP-(KOPPELTEKEN) en HTTP Live Streaming (HLS). Wanneer u bezig bent met het live streamen wordt dynamisch HLS altijd geleverd. Standaard berekend automatisch de HLS segment verpakking verhouding (fragmenten per segment) op basis van het keyframe interval dat ontvangen van het live coderingsprogramma Media Services.

De volgende tabel ziet u hoe de duur van het segment wordt berekend:

| Keyframe interval | HLS segment verpakking verhouding (FragmentsPerSegment) | Voorbeeld |
| --- | --- | --- |
| Kleiner dan of gelijk aan 3 seconden |3:1 |Als KeyFrameInterval (of GOP) 2 seconden, is de verhouding tussen het standaard HLS segment verpakking 3 op 1. Hiermee maakt u een HLS 6 seconde segment. |
| 3 tot 5 seconden |2:1 |Als KeyFrameInterval (of GOP) 4 seconden, is de verhouding tussen het standaard HLS segment verpakking 2 op 1. Hiermee maakt u een HLS-segment 8 seconde. |
| Meer dan 5 seconden |1:1 |Als KeyFrameInterval (of GOP) 6 seconden, is de verhouding tussen het standaard HLS segment verpakking 1 op 1. Hiermee maakt u een HLS 6 seconde segment. |

U kunt de verhouding tussen het fragmenten per segment wijzigen door het configureren van de uitvoer van het kanaal en FragmentsPerSegment instellen op ChannelOutputHls.

U kunt ook de intervalwaarde keyframe wijzigen door de eigenschap KeyFrameInterval op ChanneInput. Als u KeyFrameInterval expliciet is ingesteld, Segmenteer de HLS verpakking verhouding die fragmentspersegment wordt berekend via de regels die eerder zijn beschreven.  

Als u zowel KeyFrameInterval als FragmentsPerSegment expliciet instelt, gebruikt Media Services de waarden die u instelt.

#### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die zijn toegestaan voor het publiceren van video naar dit kanaal definiëren. Een toegestane IP-adres kan worden opgegeven als een van de volgende:

* Een enkel IP-adres (bijvoorbeeld: 10.0.0.1)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld 10.0.0.1(255.255.252.0))

Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, wordt geen IP-adres toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.

### <a name="channel-preview"></a>Kanaal preview
#### <a name="preview-urls"></a>Voorbeeld-URL 's
Kanalen bieden een preview-eindpunt (preview URL) die u gebruikt om te bekijken en valideren van uw stream voor verdere verwerking en levering.

Wanneer u het kanaal hebt gemaakt, kunt u de voorbeeld-URL ophalen. Voor u om de URL te krijgen, het kanaal heeft geen zich in de **met** status. Nadat het kanaal wordt gestart ophalen van gegevens, kunt u uw stream bekijken.

Op dit moment de preview-stroom kan worden geleverd alleen in gefragmenteerde MP4 (Smooth Streaming)-indeling, ongeacht het type van de opgegeven invoer. U kunt de [Smooth Streaming Health Monitor](http://smf.cloudapp.net/healthmonitor) player om de smooth stream te testen. Ook kunt u een speler die wordt gehost in Azure portal om uw stream weer te geven.

#### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die zijn toegestaan om verbinding met het preview-eindpunt te definiëren. Als geen IP-adressen worden opgegeven, wordt elk IP-adres krijgt. Een toegestane IP-adres kan worden opgegeven als een van de volgende:

* Een enkel IP-adres (bijvoorbeeld: 10.0.0.1)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld 10.0.0.1(255.255.252.0))

### <a name="channel-output"></a>Kanaal-uitvoer
Zie voor informatie over de kanaal-uitvoer, de [Keyframe interval](#keyframe_interval) sectie.

### <a name="channel-managed-programs"></a>Programma's van Channel beheerd
Een kanaal is gekoppeld aan programma's die u gebruiken kunt om te bepalen voor het publiceren en opslaan van segmenten in een live stream. Kanalen beheren programma's. De kanaal-/ programmarelatie lijkt erg op traditionele media waarbij een kanaal een constante stream met inhoud heeft en een programma is afgestemd op een bepaalde getimede gebeurtenis op dat kanaal.

U kunt het aantal uren opgeven dat u de opgenomen inhoud voor het programma wilt behouden door de lengte voor **Archiefvenster** in te stellen. Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur. De lengte van een archiefvenster bepaalt ook de maximale hoeveelheid tijd die clients terug in de tijd kunnen zoeken vanaf de huidige live positie. Programma's kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elk programma dat is gekoppeld aan een asset die de gestreamde inhoud is opgeslagen. Een asset is toegewezen aan een blok-blob-container in Azure storage-account en de bestanden in de asset worden opgeslagen als blobs in de container. Voor het publiceren van het programma zodat uw klanten de stroom kunnen bekijken, moet u een OnDemand-locator voor de gekoppelde asset maken. U kunt deze locator kunt gebruiken voor het bouwen van een streaming-URL die u aan uw clients kunt leveren.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. U kunt publiceren en archiveren van verschillende onderdelen van een gebeurtenis naar behoefte. Stel bijvoorbeeld dat uw bedrijf nodig is voor het archiveren van een programma met de 6 uur, maar voor het uitzenden van alleen de laatste 10 minuten. Hiervoor moet u twee gelijktijdig actieve programma's maken. Een programma is ingesteld op 6 uur van de gebeurtenis te archiveren, maar het programma wordt niet gepubliceerd. Het andere programma wordt ingesteld om 10 minuten te archiveren en dit programma is gepubliceerd.

Gebruik bestaande programma's niet voor nieuwe gebeurtenissen. Maak in plaats daarvan een nieuw programma voor elke gebeurtenis. Start het programma wanneer u klaar bent om te streamen en te archiveren. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren.

Voor het verwijderen van de gearchiveerde inhoud stoppen en verwijderen van het programma en verwijder vervolgens de gekoppelde asset. Een asset kan niet worden verwijderd als deze wordt gebruikt door een programma. Het programma moet eerst worden verwijderd.

Zelfs na het stoppen en verwijderen van het programma, kunnen gebruikers de gearchiveerde inhoud als video op aanvraag streamen, totdat u de asset verwijderd. Als u wilt de gearchiveerde inhoud behouden maar deze niet langer beschikbaar voor streaming, verwijdert u de streaming-locator.

## <a id="states"></a>Kanaal statussen en facturering
Mogelijke waarden voor de huidige status van een kanaal zijn:

* **Gestopt**: dit is de beginstatus van het kanaal na het maken ervan. De kanaaleigenschappen kunnen worden bijgewerkt in deze toestand is, maar streaming is niet toegestaan.
* **Starten van**: het kanaal wordt gestart. In deze status zijn streaming en updates niet toegestaan. Als een fout optreedt, wordt het kanaal de **gestopt** status.
* **Met**: het kanaal live gegevensstromen kan verwerken.
* **Stoppen van**: het kanaal is wordt gestopt. In deze status zijn streaming en updates niet toegestaan.
* **Verwijderen van**: het kanaal wordt verwijderd. In deze status zijn streaming en updates niet toegestaan.

De volgende tabel ziet hoe kanaal toewijzen aan de facturering modus staat.

| Kanaalstatus | Portal indicatoren van de gebruikersinterface | In rekening gebracht? |
| --- | --- | --- | --- |
| **Starten** |**Starten** |Nee (overgangsstatus) |
| **Uitgevoerd** |**Gereed** (Er zijn geen actieve programma)<p><p>of<p>**Streaming** (ten minste één actieve programma) |Ja |
| **Stoppen** |**Stoppen** |Nee (overgangsstatus) |
| **Gestopt** |**Gestopt** |Nee |

## <a id="cc_and_ads"></a>Gesloten ondertiteling en ad invoegen
De volgende tabel ziet u ondersteunde standaarden voor gesloten ondertiteling en ad invoegen.

| Standard | Opmerkingen |
| --- | --- |
| CEA 708 en EIA 608 (708/608) |CEA 708 en EIA 608 zijn gesloten ondertiteling standaarden voor de Verenigde Staten en Canada.<p><p>Op dit moment wordt ondertiteling alleen ondersteund als uitgevoerd in de gecodeerde invoerstroom. U moet een live media encoder die 608 of 708 bijschriften kunt invoegen in de gecodeerde stroom die wordt verzonden met Media Services gebruiken. De inhoud met ingevoegde bijschriften biedt Media Services voor uw gebruikers. |
| TTML binnen .ismt (tekst houdt Smooth Streaming) |Media Services dynamische pakketten kan uw clients om inhoud te streamen in een van de volgende indelingen: DASH, HLS of Smooth Streaming. Echter, als u wilt opnemen gefragmenteerd MP4 (Smooth Streaming) met bijschriften binnen .ismt (tekst houdt Smooth Streaming), kunt u de stroom leveren aan clients alleen Smooth Streaming. |
| SCTE 35 |SCTE 35 is een digitale signalering systeem die wordt gebruikt voor de hint reclame invoegen. Het signaal downstream ontvangers gebruiken voor het splitsen van reclame naar de stroom voor de toegewezen tijd. SCTE 35 moet worden verzonden als een sparse bijhouden in de invoerstroom.<p><p>Op dit moment de enige ondersteunde invoerstroom opmaken dat uitvoert ad signalen is gefragmenteerd MP4 (Smooth Streaming). De enige ondersteunde uitvoer indeling is ook Smooth Streaming. |

## <a id="considerations"></a>Overwegingen
Als u een on-premises live codering verzendt een multi-bitrate stream naar een kanaal gebruikt, past u de volgende beperkingen:

* Zorg ervoor dat er voldoende vrije verbinding met Internet om gegevens te verzenden aan de punten opnemen.
* URL voor opnemen met behulp van een secundaire extra bandbreedte vereist.
* De stroom inkomende multi-bitrate kan maximaal 10 videokwaliteit niveaus (lagen) en maximaal 5 audio houdt hebben.
* De hoogste gemiddelde bitrate voor elk van de videokwaliteit niveaus moeten uit minder dan 10 Mbps.
* De statistische functie van de gemiddelde bitsnelheden voor alle video en audio gegevensstromen moeten uit minder dan 25 Mbps.
* U kunt het invoerprotocol terwijl het kanaal niet wijzigen of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* U kunt een single bitrate opnemen in het kanaal op. Maar omdat de stroom niet door het kanaal verwerkt wordt, de clienttoepassingen ontvangt ook een single-bitrate stream. (Wordt niet aanbevolen deze optie.)

Hier volgen andere overwegingen voor het werken met kanalen en verwante onderdelen:

* Telkens wanneer u de configuratie van het live coderingsprogramma, Roep de **opnieuw** methode voor het kanaal. Voordat u het kanaal opnieuw instelt, moet u stop het programma. Nadat u het kanaal opnieuw instelt, start het programma opnieuw.
* Een kanaal kan worden gestopt, alleen wanneer deze zich in de **met** status en alle programma's op het kanaal zijn gestopt.
* Standaard kunt u alleen 5 kanalen toevoegen aan uw Media Services-account. Zie voor meer informatie [quota's en beperkingen](media-services-quotas-and-limitations.md).
* U wordt gefactureerd wanneer het kanaal in de **met** status. Raadpleeg voor meer informatie de [Channel statussen en facturering](media-services-live-streaming-with-onprem-encoders.md#states) sectie.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen
[Azure Media Services tot gefragmenteerde MP4 live opnemen specificatie](media-services-fmp4-live-ingest-overview.md)

[Overzicht van Azure Media Services en algemene scenario 's](media-services-overview.md)

[Media Services-concepten](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
