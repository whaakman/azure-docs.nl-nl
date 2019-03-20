---
title: Stream live met on-premises coderingsprogramma's die multi-bitrate streams - Azure maken | Microsoft Docs
description: 'Dit onderwerp wordt beschreven hoe u een kanaal dat een multi-bitrate live stream van een on-premises coderingsprogramma ontvangt instelt. De stroom kan vervolgens worden afgeleverd bij client-afspeeltoepassing via een of meer streaming-eindpunten, met een van de volgende protocollen voor adaptive streaming: HLS, Smooth Streaming, DASH.'
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: cenkd;juliako
ms.openlocfilehash: da20e4601b75bcb22546d21f6ad218ac9ba2728b
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58188353"
---
# <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders"></a>Werken met kanalen die een multi-bitrate livestream van on-premises encoders ontvangen

> [!NOTE]
> Vanaf 12 mei 2018 livekanalen wordt niet langer ondersteuning voor het RTP/MPEG-2-transportstroom-opnameprotocol. Voer een migratie uit van RTP/MPEG-2 naar RTMP- of gefragmenteerde MP4 (Smooth Streaming) opnameprotocollen.

## <a name="overview"></a>Overzicht
In Azure Media Services, een *kanaal* vertegenwoordigt een pijplijn voor het verwerken van live-streaming-inhoud. Een kanaal ontvangt live invoerstromen op twee manieren:

* Een on-premises live codering verzendt een multi-bitrate RTMP of Smooth Streaming (gefragmenteerde MP4) naar het kanaal dat is niet ingeschakeld voor live coderen met Media Services streamen. De opgenomen streams doorgegeven via de kanalen zonder verdere verwerking. Deze methode wordt aangeroepen *pass-through*. Een live coderingsprogramma kan ook een single-bitrate stream verzenden naar een kanaal dat is niet ingeschakeld voor live codering, maar we raden niet aan dat. Media Services voorziet in de stroom voor klanten die deze aanvragen.

  > [!NOTE]
  > Met behulp van een passthrough-methode is de meest voordelige manier om live te streamen.


* Een on-premises live codering verzendt een single-bitrate stream naar het kanaal dat is ingeschakeld voor live coderen met Media Services in een van de volgende indelingen: RTMP of Smooth Streaming (gefragmenteerde MP4). Het kanaal voert live codering van de binnenkomende single-bitrate stream naar een multi-bitrate (adaptieve) videostream. Media Services voorziet in de stroom voor klanten die deze aanvragen.

Beginnen met de release van Media Services 2.10 wanneer u een kanaal maakt, kunt u opgeven hoe u wilt dat het kanaal voor het ontvangen van de invoerstroom. U kunt ook opgeven of u wilt dat het kanaal live codering van uw stroom. U hebt hiervoor twee opties:

* **Passeren**: Geef deze waarde op als u van plan bent te gebruiken van een on-premises live codering met een multi-bitrate stream (een Pass Through-stream) als uitvoer. In dit geval doorstuurt de stroom inkomende naar de uitvoer zonder eventuele te coderen. Dit is het gedrag van een kanaal vóór de 2,10 release. Dit artikel bevat informatie over het werken met kanalen van dit type.
* **Live Encoding**: Kies deze waarde als u van plan bent uw single-bitrate live stream naar een multi-bitrate stream coderen met Media Services. Verlaten van een live encoding kanaal in een **met** status worden kosten in rekening gebracht. Het is raadzaam om uw actieve kanalen onmiddellijk te beëindigen nadat uw live-streaming-gebeurtenis is voltooid om extra per uur kosten te voorkomen. Media Services voorziet in de stroom voor klanten die deze aanvragen.

> [!NOTE]
> Dit artikel worden de kenmerken van kanalen die niet geschikt zijn voor het uitvoeren van realtime codering. Zie voor meer informatie over het werken met kanalen die geschikt zijn voor het uitvoeren van realtime codering [Live streamen met Azure Media Services om multi-bitrate streams te maken](media-services-manage-live-encoder-enabled-channels.md).
>
>Raadpleeg voor meer informatie over aanbevolen on premises coderingsprogramma's, [aanbevolen on premises coderingsprogramma's](media-services-recommended-encoders.md).

Het volgende diagram staat voor een live-streaming-werkstroom die gebruikmaakt van een on-premises live coderingsprogramma om multi-bitrate RTMP- of gefragmenteerde MP4 (Smooth Streaming)-streams als uitvoer.

![Live-werkstroom][live-overview]

## <a id="scenario"></a>Algemeen scenario voor live streamen
De volgende stappen beschrijven de taken die betrokken zijn bij het maken van algemene toepassingen voor live streamen.

1. Sluit een videocamera aan op een computer. Start en configureer een on-premises live codering met een multi-bitrate RTMP- of gefragmenteerde MP4 (Smooth Streaming) stream als uitvoer. Zie [Azure Media Services RTMP-ondersteuning en live coderingsprogramma's](https://go.microsoft.com/fwlink/?LinkId=532824) voor meer informatie.

    U kunt ook deze stap uitvoeren nadat u uw kanaal hebt gemaakt.
2. Een kanaal maken en starten.

3. URL voor opnemen ophalen het kanaal.

    Het live coderingsprogramma gebruikt de URL voor opnemen voor het verzenden van de stream naar het kanaal.
4. De voorbeeld-URL van het kanaal worden opgehaald.

    Gebruik deze URL om te controleren of de livestream goed door het kanaal wordt ontvangen.
5. Maak een programma.

    Wanneer u de Azure-portal, het maken van een programma ook een asset gemaakt.

    Wanneer u de .NET SDK of REST gebruiken, moet u een asset maken en opgeven voor het gebruik van deze asset bij het maken van een programma.
6. Publiceer de asset die is gekoppeld aan het programma.   

    >[!NOTE]
    >Wanneer uw Azure Media Services-account wordt gemaakt, een **standaard** streaming-eindpunt wordt toegevoegd aan uw account in de **gestopt** staat. Het streaming-eindpunt van waar u inhoud wilt streamen, moet de status **Wordt uitgevoerd** hebben.

7. Start het programma wanneer u klaar bent om te streamen en te archiveren.

8. Het live coderingsprogramma kan desgewenst een signaal ontvangen dat een advertentie moet worden gestart. De advertentie wordt ingevoegd in de uitvoerstream.

9. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren.

10. Het programma te verwijderen (en verwijder desgewenst de asset).     

## <a id="channel"></a>Beschrijving van een kanaal en de bijbehorende onderdelen
### <a id="channel_input"></a>Kanaal-invoer (opnemen) configuraties
#### <a id="ingest_protocols"></a>Streaming-opnameprotocol
Media Services biedt ondersteuning voor live-feeds met behulp van multi-bitrate gefragmenteerd MP4 en multi-bitrate RTMP als protocollen voor streaming opnemen. Wanneer het RTMP opnemen streaming-protocol is ingeschakeld, worden twee eindpunten (invoer) opnemen voor het kanaal worden gemaakt:

* **Primaire URL**: Hiermee geeft u de volledig gekwalificeerde URL van de primaire RTMP van het kanaal eindpunt opnemen.
* **Secundaire URL** (optioneel): Hiermee geeft u de volledig gekwalificeerde URL van de secundaire RTMP van het kanaal eindpunt opnemen.

Gebruik de URL van de secundaire als u wilt voor het verbeteren van de duurzaamheid en fouttolerantie geboden van uw opname stream (evenals encoder failover en fouttolerantie geboden), met name voor de volgende scenario's:

- Één encoder dubbele pushen naar zowel primaire als secundaire URL's:

    Het belangrijkste doel van dit scenario is over meer tolerantie voor netwerkfluctuaties en geleidelijk. Sommige RTMP coderingsprogramma's niet verwerken netwerkuitval goed. Als een netwerk verbinding verbreken gebeurt, kan een coderingsprogramma stoppen met coderen en de gebufferde gegevens vervolgens niet verzenden wanneer een reconnect gebeurt. Dit zorgt ervoor dat wijzigingen en verlies van gegevens. Netwerkuitval kan gebeuren vanwege een ongeldige netwerk of onderhoud aan de Azure. Primaire/secundaire URL's verminderen van de problemen met het netwerk en een gecontroleerde upgradeproces bieden. Telkens wanneer een geplande netwerk verbinding verbreken gebeurt, Media Services beheert de primaire en secundaire verbinding verbreekt en biedt een vertraagde verbroken tussen de twee. Coderingsprogramma's klikt u vervolgens hebt tijd om te blijven verzenden van gegevens en het opnieuw. De volgorde van de verbinding verbreekt kan willekeurig, maar er is een vertraging optreden tussen primaire/secundaire of secundaire/primaire URL's. In dit scenario is het coderingsprogramma nog steeds de één storingspunt is.

- Meerdere encoders, met elke encoder pushen naar een toegewezen:

    In dit scenario biedt zowel encoder en neemt redundantie. In dit scenario, encoder1 verstuurd naar de URL van de primaire en encoder2 verstuurd naar de secundaire URL. Wanneer een coderingsprogramma is mislukt, kan het coderingsprogramma andere gegevens blijven verzenden. Redundantie van de gegevens kan worden beheerd omdat Media Services niet van primaire en secundaire URL's op hetzelfde moment verbroken wordt. In dit scenario wordt ervan uitgegaan dat coderingsprogramma's, tijd gesynchroniseerd exact dezelfde gegevens bevatten.  

- Meerdere encoders dubbele pushen naar zowel primaire als secundaire URL's:

    In dit scenario pushen zowel encoders gegevens naar de primaire en secundaire URL's. Dit biedt de beste betrouwbaarheid en fouttolerantie, evenals de gegevensredundantie. In dit scenario kan tolereren beide storingen van de codering en de verbinding verbreekt, zelfs als een coderingsprogramma niet meer werkt. Wordt ervan uitgegaan dat coderingsprogramma's gesynchroniseerd zijn en exact dezelfde gegevens bevatten.  

Zie voor meer informatie over het live coderingsprogramma's RTMP [Azure Media Services RTMP-ondersteuning en Live coderingsprogramma's](https://go.microsoft.com/fwlink/?LinkId=532824).

#### <a name="ingest-urls-endpoints"></a>URL's (eindpunten) voor opnemen
Een kanaal biedt een invoereindpunt (URL voor opnemen) dat u in het live coderingsprogramma, opgeeft zodat de encoder kunt pushen stromen naar uw kanalen.   

Wanneer u het kanaal maakt, kunt u de opname-URL's krijgen. U kunt deze URL's ophalen, het kanaal niet hoeft te zijn de **met** staat. Wanneer u klaar om te beginnen met het pushen van gegevens naar het kanaal bent, het kanaal moet zich in de **met** staat. Nadat het kanaal begint het ophalen van gegevens, kunt u uw stroom via de URL van de Preview-versie kunt bekijken.

Hebt u een optie van het opnemen van een gefragmenteerde MP4 (Smooth Streaming) live stream via een SSL-verbinding. Als u wilt opnemen via SSL, zorg ervoor dat u de URL voor opnemen bijwerkt naar HTTPS. U kunt geen op dit moment RTMP opnemen via SSL.

#### <a id="keyframe_interval"></a>Interval voor sleutelframes
Wanneer u een on-premises live coderingsprogramma gebruikt voor het genereren van multi-bitrate stream, bevat het interval sleutelframes de duur van de groep afbeeldingen (GOP) gebruikt door deze externe encoders. Nadat het kanaal deze binnenkomende stream ontvangt, kunt u uw live stream kunt leveren voor afspelen van clienttoepassingen op een van de volgende indelingen: Smooth Streaming, Dynamic Adaptive Streaming via HTTP (DASH) en HTTP Live Streaming (HLS). Als u bent u live streamt, wordt dynamisch HLS altijd geleverd. Media Services berekent standaard automatisch de HLS segment verpakking hoogte-breedteverhouding (fragmenten per segment) op basis van het interval sleutelframes die wordt ontvangen van het live coderingsprogramma.

De volgende tabel ziet u hoe de duur van het segment wordt berekend:

| Interval voor sleutelframes | HLS segment packaging ratio (FragmentsPerSegment) | Voorbeeld |
| --- | --- | --- |
| Kleiner dan of gelijk aan 3 seconden |3:1 |Als KeyFrameInterval (of GOP) 2 seconden, is de standaard HLS segment verpakking verhouding 3 tot en met 1. Hiermee maakt u een 6-seconde HLS-segment. |
| 3 tot 5 seconden |2:1 |Als KeyFrameInterval (of GOP) 4 seconden, is de standaard HLS segment verpakking verhouding 2 tot en met 1. Hiermee maakt u een 8-seconde HLS-segment. |
| Groter is dan 5 seconden |1:1 |Als KeyFrameInterval (of GOP) 6 seconden, is de standaard HLS segment verpakking verhouding 1 op 1. Hiermee maakt u een 6-seconde HLS-segment. |

U kunt de verhouding fragmenten per segment wijzigen door het configureren van de uitvoer van het kanaal en FragmentsPerSegment instellen op ChannelOutputHls.

U kunt ook de waarde voor de sleutelframes interval wijzigen door de eigenschap KeyFrameInterval op ChannelInput. Als u KeyFrameInterval expliciet instelt, segmenteren de HLS verpakking verhouding die fragmentspersegment wordt berekend via de regels die eerder zijn beschreven.  

Als u zowel KeyFrameInterval en FragmentsPerSegment expliciet instelt, Media Services maakt gebruik van de waarden die u hebt ingesteld.

#### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die video publiceren naar dit kanaal mogen definiëren. Een toegestane IP-adres kan worden opgegeven als een van de volgende:

* Één IP-adres (bijvoorbeeld: 10.0.0.1)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld 10.0.0.1(255.255.252.0))

Als er geen IP-adressen worden opgegeven en er geen regeldefinitie bestaat, is geen IP-adres toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.

### <a name="channel-preview"></a>Kanaalvoorbeeld
#### <a name="preview-urls"></a>Voorbeeld-URL 's
Kanalen bevatten een preview-eindpunt (de voorbeeld-URL) die u gebruikt om te bekijken en uw stream te valideren voordat deze verder wordt verwerkt en geleverd.

U kunt de voorbeeld-URL ophalen bij het maken van het kanaal. Voor u om de URL te krijgen, het kanaal niet hoeft te zijn de **met** staat. Nadat het kanaal begint het ophalen van gegevens, kunt u uw stroom bekijken.

Op dit moment de preview-stream kan worden geleverd alleen in gefragmenteerde MP4 (Smooth Streaming)-indeling, ongeacht het type van de opgegeven ingang. U kunt de [Smooth Streaming Health Monitor](https://playready.directtaps.net/smoothstreaming/) player om het smooth stream te testen. U kunt ook een speler die wordt gehost in Azure portal om uw stream weer te geven.

#### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die zijn toegestaan verbinding maken met de preview-eindpunt definiëren. Als er geen IP-adressen zijn opgegeven, wordt elk IP-adres is toegestaan. Een toegestane IP-adres kan worden opgegeven als een van de volgende:

* Één IP-adres (bijvoorbeeld: 10.0.0.1)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22)
* Een IP-adresbereik die gebruikmaakt van een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld 10.0.0.1(255.255.252.0))

### <a name="channel-output"></a>Kanaal-uitvoer
Zie voor meer informatie over het kanaal uitvoer de [sleutelframes interval](#keyframe_interval) sectie.

### <a name="channel-managed-programs"></a>Kanaal beheerde programma 's
Een kanaal is gekoppeld aan programma's die u gebruiken kunt voor het beheren van het publiceren en opslaan van segmenten in een live stream. Kanalen beheren programma's. De kanaal- / relatie is vergelijkbaar met traditionele media waarbij een kanaal een constante stream met inhoud heeft en een programma is afgestemd op bepaalde getimede gebeurtenis op dat kanaal.

U kunt het aantal uren opgeven dat u de opgenomen inhoud voor het programma wilt behouden door de lengte voor **Archiefvenster** in te stellen. Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur. Voor het archiefvenster bepaalt ook dat het maximum aantal wanneer clients terug in tijd kan zoeken vanaf de huidige live positie. Programma's kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elk programma dat is gekoppeld aan een asset die de gestreamde inhoud is opgeslagen. Een asset is toegewezen aan een blok-blob-container in de Azure storage-account en de bestanden in de asset worden opgeslagen als blobs in deze container. Voor het publiceren van het programma, zodat uw klanten de stroom kunnen bekijken, moet u een OnDemand-locator voor de gekoppelde asset maken. Deze locator kunt u een streaming-URL die u aan uw clients kunt leveren maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. U kunt publiceren en archiveren van verschillende onderdelen van een gebeurtenis naar behoefte. Denk bijvoorbeeld aan dat uw bedrijf nodig is zes uur van een programma te archiveren, maar voor het uitzenden van alleen de laatste tien minuten. Hiervoor moet u twee gelijktijdig actieve programma's maken. Een programma wordt ingesteld om zes uur van de gebeurtenis te archiveren, maar het programma wordt niet gepubliceerd. Het andere programma is ingesteld om 10 minuten te archiveren en dit programma is gepubliceerd.

Gebruik bestaande programma's niet voor nieuwe gebeurtenissen. In plaats daarvan een nieuw programma voor elke gebeurtenis maken. Start het programma wanneer u klaar bent om te streamen en te archiveren. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren.

Als u wilt verwijderen gearchiveerde inhoud, stoppen en het programma te verwijderen en verwijder vervolgens de gekoppelde asset. Een asset kan niet worden verwijderd als deze wordt gebruikt door een programma. Het programma moet eerst worden verwijderd.

Zelfs na het stoppen en verwijderen van het programma, kunnen gebruikers de gearchiveerde inhoud als video op aanvraag, streamen, totdat u de asset verwijderd. Als u wilt de gearchiveerde inhoud behouden maar deze niet langer beschikbaar voor streaming, verwijdert u de streaming-locator.

## <a id="states"></a>Kanaal Staten en facturering
Mogelijke waarden voor de huidige status van een kanaal zijn onder andere:

* **Gestopt**: Dit is de beginstatus van het kanaal nadat het is gemaakt. In deze status kunnen de eigenschappen van het kanaal worden bijgewerkt, maar is streaming niet toegestaan.
* **Vanaf**: Het kanaal wordt gestart. In deze status zijn streaming en updates niet toegestaan. Als er een fout optreedt, wordt het kanaal teruggezet naar de **gestopt** staat.
* **Uitvoeren**: Het kanaal kan live streams verwerken.
* **Stoppen**: Het kanaal wordt gestopt. In deze status zijn streaming en updates niet toegestaan.
* **Verwijderen van**: Het kanaal wordt verwijderd. In deze status zijn streaming en updates niet toegestaan.

In de volgende tabel wordt het verband tussen de verschillende kanaalstatussen en de facturering weergegeven.

| Kanaalstatus | Portal UI-indicatoren | In rekening gebracht? |
| --- | --- | --- |
| **Starten** |**Starten** |Nee (overgangsstatus) |
| **Wordt uitgevoerd** |**Gereed** (geen programma's uitgevoerd)<p><p>of<p>**Streaming** (ten minste één programma uitgevoerd) |Ja |
| **Stoppen** |**Stoppen** |Nee (overgangsstatus) |
| **Gestopt** |**Gestopt** |Nee |

## <a id="cc_and_ads"></a>Ondertiteling en advertentie-invoeging gesloten
De volgende tabel ziet u ondersteunde standaarden voor gesloten ondertiteling en ad invoegen.

| Standard | Opmerkingen |
| --- | --- |
| CEA-708 en EIA 608 (708/608) |CEA-708 en EIA 608 zijn ondertitels standaarden voor de Verenigde Staten en Canada.<p><p>Op dit moment wordt ondertiteling alleen ondersteund als in de gecodeerde invoerstroom uitgevoerd. U moet een live media encoder die 608 of 708-ondertiteling kunt invoegen in de gecodeerde stroom die wordt verzonden naar Media Services gebruiken. Media Services voorziet in de inhoud met ingevoegd bijschriften voor uw gebruikers. |
| TTML binnen .ismt (tekstsporen Smooth Streaming) |Media Services dynamische pakketten kunt uw clients om inhoud te streamen in een van de volgende indelingen: DASH, HLS of Smooth Streaming. Echter, als u de opname-gefragmenteerde MP4 (Smooth Streaming) met bijschriften in .ismt (tekstsporen Smooth Streaming), kunt u de stroom leveren aan clients alleen Smooth Streaming. |
| SCTE-35 |SCTE 35 is een digitale signalering systeem dat wordt gebruikt voor het invoegen van reclame hint. Het signaal downstream ontvangers gebruiken voor het genereren van advertenties in de stroom voor de toegewezen tijd. SCTE 35 moet worden verzonden als een sparse bijhouden in de invoerstroom.<p><p>Op dit moment de enige ondersteunde invoerstroom opmaken dat uitvoert ad signalen is gefragmenteerd MP4 (Smooth Streaming). De enige ondersteunde uitvoer indeling is ook Smooth Streaming. |

## <a id="considerations"></a>Overwegingen met betrekking tot
Wanneer u een on-premises live coderingsprogramma gebruikt voor het verzenden van een multi-bitrate stream naar een kanaal, gelden de volgende beperkingen:

* Zorg ervoor dat er voldoende vrije verbinding met Internet om gegevens te verzenden naar de opname-punten.
* Met behulp van een secundaire opname-URL vereist extra bandbreedte.
* De binnenkomende multi-bitrate stream kan maximaal 10 videokwaliteit niveaus (lagen) en een maximum van 5 audionummers hebben.
* De hoogste gemiddelde bitrate voor het gebruik van de video kwaliteitsniveaus moeten uit minder dan 10 Mbps.
* De statistische functie van de gemiddelde bitsnelheden voor alle video en audio gegevensstromen moet minder zijn dan 25 Mbps.
* U kunt het invoerprotocol terwijl het kanaal niet wijzigen of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* U kunt een single-bitrate opnemen in uw kanaal. Maar omdat het kanaal niet de stroom te verwerken heeft, de clienttoepassingen ontvangt ook een single-bitrate stream. (Wordt niet aanbevolen deze optie.)

Hier volgen andere overwegingen met betrekking tot het werken met kanalen en gerelateerde onderdelen:

* Telkens wanneer u de configuratie van het live coderingsprogramma, Roep de **opnieuw** methode voor het kanaal. Voordat u het kanaal opnieuw instelt, moet u het programma stoppen. Nadat u het kanaal opnieuw ingesteld, moet u het programma opnieuw starten.

  > [!NOTE]
  > Wanneer u het programma opnieuw start, moet u deze koppelen aan een nieuwe asset en een nieuwe locator maken. 
  
* Een kanaal kan worden gestopt, alleen wanneer deze zich in de **met** staat en alle programma's op het kanaal zijn gestopt.
* Standaard kunt u maximaal vijf kanalen toevoegen aan uw Media Services-account. Zie voor meer informatie, [quota en beperkingen](media-services-quotas-and-limitations.md).
* U wordt gefactureerd wanneer uw kanaal in de **met** staat. Zie voor meer informatie de [Channel Staten en facturering](media-services-live-streaming-with-onprem-encoders.md#states) sectie.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Feedback
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen
[Aanbevolen on premises coderingsprogramma 's](media-services-recommended-encoders.md)

[Azure Media Services gefragmenteerde MP4-leven specificatie-liveopname](media-services-fmp4-live-ingest-overview.md)

[Overzicht van Azure Media Services en algemene scenario 's](media-services-overview.md)

[Media Services-concepten](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
