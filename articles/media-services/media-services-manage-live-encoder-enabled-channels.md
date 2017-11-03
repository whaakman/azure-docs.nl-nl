---
title: Live streamen met Azure Media Services om multi-bitrate streams te maken | Microsoft Docs
description: 'Dit onderwerp beschrijft het instellen van een kanaal dat een single-bitrate live stream ontvangt van een on-premises coderingsprogramma en voert deze live codering naar adaptive bitrate stream met Media Services. De stroom kan vervolgens worden afgeleverd bij afspelen clienttoepassingen via een of meer Streaming-eindpunten, met een van de volgende protocollen voor adaptief streamen: MPEG-DASH HLS, Smooth Stream.'
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: d5f76d532b236e67a4e69eb820e2cfc3033a80c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="live-streaming-using-azure-media-services-to-create-multi-bitrate-streams"></a>Live streamen met Azure Media Services om multi-bitrate streams te maken
## <a name="overview"></a>Overzicht
Azure Media Services (AMS) een **kanaal** vertegenwoordigt een pijplijn voor het verwerken van live streaming inhoud. Een **kanaal** live invoer streams ontvangt op twee manieren:

* Een on-premises live codering verzendt een stream met één bitsnelheid naar het kanaal dat is ingeschakeld voor het uitvoeren van de live codering met Media Services in een van de volgende indelingen: RTP (MPEG-TS), RTMP of Smooth Streaming (gefragmenteerde MP4). Het kanaal codeert de inkomende single-bitrate stream vervolgens live naar een (adaptieve) multi-bitrate videostream. Desgevraagd levert Media Services de stream aan klanten.
* Een on-premises live codering verzendt een multi-bitrate **RTMP** of **Smooth Streaming** (gefragmenteerde MP4) naar het kanaal dat niet is ingeschakeld voor het uitvoeren van live codering met AMS. De opgenomen streams **kanaal**s zonder verdere verwerking. Deze methode wordt aangeroepen **pass-through**. U kunt de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming uitvoeren: MediaExcel, Ateme, stel communicatie, Envivio, Cisco en Elemental. De volgende live coderingsprogramma's voeren RTMP: Adobe Flash Media Live coderingsprogramma (FMLE), Telestream Wirecast, Haivision, Teradek en Tricaster coderingsprogramma's.  Een live coderingsprogramma kan ook een stream met één bitsnelheid verzenden naar een kanaal dat niet is ingeschakeld voor Live Encoding, maar dit wordt niet aanbevolen. Desgevraagd levert Media Services de stream aan klanten.
  
  > [!NOTE]
  > Een Pass Through-methode is de meest voordelige manier om live te streamen.
  > 
  > 

Beginnen met de release van Media Services 2.10 wanneer u een kanaal maakt, kunt u op welke manier u wilt dat voor dit kanaal voor het ontvangen van de invoerstroom en of u wilt voor het kanaal live codering van uw stream uitvoeren. U hebt hiervoor twee opties:

* **Geen** : deze waarde opgeven als u gebruiken van een on-premises live coderingsprogramma die multi-bitrate stream (Pass Through-stream wilt) wordt uitgevoerd. In dit geval wordt de stroom inkomende doorgegeven aan de uitvoer zonder alle codering. Dit is het gedrag van een kanaal vóór 2,10 release.  Zie voor meer informatie over het werken met kanalen van dit type, [Live streamen met on-premises-coderingsprogramma's die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md).
* **Standaard** – Selecteer deze waarde als u van plan bent uw single-bitrate live stream naar een multi-bitrate stream coderen met Media Services. Let erop dat er een facturering invloed voor live codering en u rekening mee houden moet dat als u een kanaal voor live codering in de status 'Actief' wordt facturering worden kosten in rekening.  Het is raadzaam om uw actieve kanalen onmiddellijk te beëindigen nadat uw live-streaming gebeurtenis is voltooid om extra per uur kosten te voorkomen.

> [!NOTE]
> In dit onderwerp worden de kenmerken van de kanalen die zijn ingeschakeld voor het uitvoeren van live codering (**standaard** coderingstype). Zie voor informatie over het werken met kanalen die niet zijn ingeschakeld voor live codering kan uitvoeren, [Live streamen met on-premises-coderingsprogramma's die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md).
> 
> Leest de [overwegingen](media-services-manage-live-encoder-enabled-channels.md#Considerations) sectie.
> 
> 

## <a name="billing-implications"></a>De implicaties van facturering
Een kanaal voor live codering begint zodra het statusovergangen naar 'Actief' via de API is facturering.   U kunt ook de status weergeven in de Azure-portal of in het hulpprogramma Azure Media Services Explorer (http://aka.ms/amse).

De volgende tabel ziet hoe kanaal statussen worden toegewezen aan facturering statussen in de API en de Azure portal. Houd er rekening mee dat de statussen enigszins verschillen tussen de API en de Portal UX zijn Als u een kanaal is in de status 'Actief' via de API of met de status 'Gereed' of 'Streaming' in de Azure portal facturering actief zijn.
Als u wilt stoppen met het kanaal van u verdere facturering, hebt u Stop het kanaal via de API of in de Azure-portal.
U bent zelf verantwoordelijk voor het stoppen van de kanalen wanneer u klaar bent met het kanaal voor live codering.  Fout bij stoppen van een codering kanaal leidt tot voortdurende facturering.

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

### <a name="automatic-shut-off-for-unused-channels"></a>Automatisch afsluiten uit voor ongebruikte kanalen
Beginnen met 25 januari 2016 Media Services uitgerold een update die automatisch een kanaal (met live codering ingeschakeld) gestopt nadat deze in een niet-gebruikte status actief is geweest gedurende een lange periode. Dit geldt voor de kanalen die geen actieve programma's hebben en die een invoer bijdrage-kanaal voor langere tijd niet hebben ontvangen.

De drempelwaarde voor een niet-gebruikte periode is nominaal 12 uur, maar kan worden gewijzigd.

## <a name="live-encoding-workflow"></a>Werkstroom voor Live codering
Het onderstaande diagram ziet u een live streaming-werkstroom waarbij een kanaal een single-bitrate stream in een van de volgende protocollen ontvangt: RTMP, Smooth Streaming of RTP (MPEG-TS); deze codeert stream naar een multi-bitrate stream vervolgens. 

![Live-werkstroom][live-overview]

## <a id="scenario"></a>Algemeen Scenario voor Live streamen
Hieronder volgen de algemene stappen voor het maken van veelvoorkomende toepassingen voor live streamen.

> [!NOTE]
> De maximum aanbevolen duur van een live gebeurtenis is momenteel acht uur. Neem contact op met amslived op Microsoft.com als u moet een kanaal voor langere tijd wilt uitvoeren. Let erop dat er een facturering invloed voor live codering en u rekening mee houden moet dat als u een kanaal voor live codering in de status 'Actief' wordt elk uur facturering worden kosten in rekening.  Het is raadzaam om uw actieve kanalen onmiddellijk te beëindigen nadat uw live-streaming gebeurtenis is voltooid om extra per uur kosten te voorkomen. 
> 
> 

1. Sluit een videocamera aan op een computer. Start en configureer een on-premises live coderingsprogramma dat een **één** bitrate stream in een van de volgende protocollen: RTMP, Smooth Streaming of RTP (MPEG-TS). 
   
    Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.
2. Maak en start een kanaal. 
3. Haal de URL voor opnemen voor het kanaal op. 
   
    De URL voor opnemen wordt gebruikt door het live coderingsprogramma om de stream naar het kanaal te verzenden.
4. Haal de voorbeeld-URL voor het kanaal op. 
   
    Gebruik deze URL om te controleren of de livestream goed door het kanaal wordt ontvangen.
5. Maak een programma. 
   
    Wanneer u de Azure-portal, het maken van een programma ook een asset gemaakt. 
   
    Wanneer u SDK voor .NET of REST u moet een asset maken en geef dit activum worden gebruikt bij het maken van een programma. 
6. Publiceer de asset die zijn gekoppeld aan het programma.   
   
    >[!NOTE]
    >Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Het streaming-eindpunt van waar u inhoud wilt streamen, moet de status **Wordt uitgevoerd** hebben. 
    
7. Start het programma wanneer u klaar bent om te streamen en te archiveren.
8. Het live coderingsprogramma kan desgewenst een signaal ontvangen dat een advertentie moet worden gestart. De advertentie wordt ingevoegd in de uitvoerstream.
9. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren.
10. Verwijder het programma (en verwijder desgewenst de asset).   

> [!NOTE]
> Het is belangrijk dat u niet vergeet een Live Encoding kanaal te stoppen. Let er is een uur facturering gevolgen voor live codering en u moet rekening mee houden dat als u een kanaal voor live codering in de status 'Actief' wordt facturering worden kosten in rekening.  Het is raadzaam om uw actieve kanalen onmiddellijk te beëindigen nadat uw live-streaming gebeurtenis is voltooid om extra per uur kosten te voorkomen. 
> 
> 

## <a id="channel"></a>De invoer-kanaal (opnemen) configuraties
### <a id="Ingest_Protocols"></a>Streaming-opnameprotocol
Als de **coderingstype** is ingesteld op **standaard**, geldige opties zijn:

* **RTP** (MPEG-TS): MPEG-2-transportstroom via RTP.  
* Single-bitrate **RTMP**
* Single-bitrate **gefragmenteerde MP4** (Smooth Streaming)

#### <a name="rtp-mpeg-ts---mpeg-2-transport-stream-over-rtp"></a>RTP (MPEG-TS) - MPEG-2-transportstroom via RTP.
Typische gebruiksscenario's: 

Professionele omroeporganisaties werken doorgaans met geavanceerde lokale live coderingsprogramma's van leveranciers zoals elementair technologieën, Ericsson, Ateme, Imagine of Envivio voor het verzenden van een stroom. Vaak wordt gebruikt in combinatie met een IT-afdeling en particuliere netwerken.

Overwegingen:

* Het gebruik van een bepaald programma transportstroom (SharePoint Team Services) invoer wordt ten zeerste aanbevolen. 
* U kunt maximaal 8 audio-stromen met MPEG-2 TS via RTP invoeren. 
* De video-stream moet een gemiddelde bitrate hieronder 15 Mbps
* De cumulatieve gemiddelde bitrate audio stromen moeten uit minder dan 1 Mbps
* De ondersteunde codecs zijn als volgt:
  
  * MPEG-2 / H.262 Video 
    
    * Belangrijkste profiel (4:2:0)
    * Hoge-profiel (4:2:0, 4:2:2)
    * 422 profiel (4:2:0, 4:2:2)
  * MPEG-4 AVC / H.264-Video  
    
    * Basislijn, Main, hoge-profiel (8-bits versie 4:2:0)
    * Profiel voor maximaal 10 (10-bits versie 4:2:0)
    * Hoge 422 profiel (10-bits versie 4:2:2)
  * AAC MPEG-2-Kredietbrief Audio 
    
    * Mono, Stereo, rond (5.1, 7.1)
    * MPEG-2-stijl ADTS verpakking
  * Dolby Digital (AC-3) Audio 
    
    * Mono, Stereo, rond (5.1, 7.1)
  * MPEG-Audio (laag II en III) 
    
    * Mono, Stereo
* Aanbevolen uitzending coderingsprogramma's omvatten:
  
  * Stel communicatie Selenio ENC 1
  * Stel communicatie Selenio ENC 2
  * Live elemental

#### <a id="single_bitrate_RTMP"></a>Single-bitrate RTMP
Overwegingen:

* De stroom inkomende mogen niet multi-bitrate video
* De video-stream moet een gemiddelde bitrate hieronder 15 Mbps
* De audiostroom moet een gemiddelde bitrate lager dan 1 Mbps
* De ondersteunde codecs zijn als volgt:
* MPEG-4 AVC / H.264-Video
* Basislijn, Main, hoge-profiel (8-bits versie 4:2:0)
* Profiel voor maximaal 10 (10-bits versie 4:2:0)
* Hoge 422 profiel (10-bits versie 4:2:2)
* AAC MPEG-2-Kredietbrief Audio
* Mono, Stereo, rond (5.1, 7.1)
* 44,1 kHz samplefrequentie
* MPEG-2-stijl ADTS verpakking
* Aanbevolen coderingsprogramma's zijn onder andere:
* Telestream Wirecast
* Media Flash Live codering

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Single-bitrate Fragmented MP4 (Smooth Streaming);
Typische gebruiksscenario's:

On-premises gebruiken live coderingsprogramma's van leveranciers zoals elementair technologieën, Ericsson, Ateme, Envivio de invoerstroom verzenden via het open internet naar een nabijgelegen Azure Datacenter.

Overwegingen:

Hetzelfde als voor [single-bitrate RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP).

#### <a name="other-considerations"></a>Andere overwegingen
* U kunt het invoerprotocol niet wijzigen terwijl het kanaal of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* Maximale resolutie voor de binnenkomende videostream 1920 x 1080 is en maximaal 60 velden/seconde als interlaced of 30 frames per seconde als progressief.

### <a name="ingest-urls-endpoints"></a>URL's (eindpunten) voor opnemen
Een kanaal biedt een invoereindpunt (de URL voor opnemen) dat u in het live coderingsprogramma opgeeft zodat het coderingsprogramma kunt push streams in de kanalen.

Als u een kanaal hebt gemaakt, kunt u de URL voor opnemen ophalen. Als u deze URL's, het kanaal heeft geen zich in de **met** status. Wanneer u klaar om te beginnen met gegevens in het kanaal worden gepusht bent, moet deze in de **met** status. Zodra het kanaal ophalen van gegevens start, kunt u uw stream via de URL van de preview kunt bekijken.

Hebt u een optie voor het opnemen van gefragmenteerde MP4 (Smooth Streaming) livestream via een SSL-verbinding. Als u wilt opnemen via SSL, zorg voor het bijwerken van de URL voor opnemen naar HTTPS. Houd er rekening mee dat op dit moment AMS biedt geen ondersteuning voor SSL met aangepaste domeinen.  

### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die zijn toegestaan voor het publiceren van video naar dit kanaal definiëren. Toegestane IP-adressen kunnen worden opgegeven als een enkel IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld '10.0.0.1(255.255.252.0)').

Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, zijn er geen IP-adressen toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.

## <a name="channel-preview"></a>Kanaal preview
### <a name="preview-urls"></a>Voorbeeld-URL 's
Kanalen bieden een preview-eindpunt (preview URL) die u gebruikt om te bekijken en valideren van uw stream voor verdere verwerking en levering.

Wanneer u het kanaal hebt gemaakt, kunt u de voorbeeld-URL ophalen. Als u de URL, het kanaal heeft geen zich in de **met** status.

Zodra het kanaal ophalen van gegevens start, kunt u uw stream te bekijken.

> [!NOTE]
> Momenteel de preview-stroom kan alleen worden geleverd in Fragmented MP4 (Smooth Streaming) indeling ongeacht het type van de opgegeven invoer. U kunt de [http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor) player om de Smooth Stream te testen. U kunt ook een speler gehost in Azure portal gebruiken om uw stream weer te geven.
> 
> 

### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die zijn toegestaan om verbinding met het preview-eindpunt te definiëren. Als er geen IP-adressen worden opgegeven elk IP-adres krijgt. Toegestane IP-adressen kunnen worden opgegeven als een enkel IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld 10.0.0.1(255.255.252.0)).

## <a name="live-encoding-settings"></a>Live codering instellingen
Deze sectie wordt beschreven hoe de instellingen voor het live coderingsprogramma binnen het kanaal kunnen worden aangepast, wanneer de **Type codering** van een kanaal is ingesteld op **standaard**.

> [!NOTE]
> Wanneer meerdere taal nummers invoeren en tijdens het doorzoeken van live codering met Azure, alleen RTP voor invoer van meerdere talen wordt ondersteund. U kunt maximaal 8 audio-stromen met MPEG-2 TS via RTP definiëren. Meerdere audio houdt met RTMP of Smooth streaming opnemen wordt momenteel niet ondersteund. Bij het uitvoeren van live codering met [lokale live coderen](media-services-live-streaming-with-onprem-encoders.md), geldt er geen dergelijke beperking, omdat een kanaal wat wordt verzonden naar AMS passeren zonder verdere verwerking.
> 
> 

### <a name="ad-marker-source"></a>AD-markering bron
U kunt opgeven dat de bron voor advertentiemarkeringssignalen opgeven. Standaardwaarde is **Api**, wat aangeeft dat het live coderingsprogramma binnen het kanaal naar een asynchrone luisteren moet **Advertentiemarkerings-API**.

De geldige optie is **Scte35** (alleen toegestaan als het streaming opnemen-protocol is ingesteld op RTP (MPEG-TS). Wanneer Scte35 wordt opgegeven, wordt het live coderingsprogramma SCTE 35 signalen uit de invoerstroom RTP (MPEG-TS) parseren.

### <a name="cea-708-closed-captions"></a>CEA 708 bijschriften gesloten
Een optionele vlag Dit geeft aan het live coderingsprogramma voor het negeren van alle gegevens van de bijschriften CEA 708 ingesloten in de binnenkomende video. Wanneer de vlag is ingesteld op false (standaard), het coderingsprogramma detecteert en opnieuw CEA 708 gegevens invoegen in de video uitvoerstromen.

### <a name="video-stream"></a>Video-Stream
Optioneel. Beschrijft de invoerstroom video. Als dit veld niet is opgegeven, wordt de standaardwaarde wordt gebruikt. Deze instelling is alleen toegestaan als de invoer streaming-protocol is ingesteld op RTP (MPEG-TS).

#### <a name="index"></a>Index
Een op nul gebaseerde index die aangeeft welke video invoerstroom moet worden verwerkt door het live coderingsprogramma binnen het kanaal. Deze instelling geldt alleen als opnemen streaming-protocol is RTP (MPEG-TS).

Standaardwaarde is nul. Het verdient aanbeveling om in een bepaald programma transportstroom (SharePoint Team Services) te verzenden. Als de invoerstroom meerdere programma's bevat, het live coderingsprogramma parseert programma kaart tabel (bet) in de invoer, identificeert de invoer met een naam van het type stream van MPEG-2-Video of H.264 en gerangschikt in de volgorde die is opgegeven in de BET. De op nul gebaseerde index wordt vervolgens gebruikt om op te halen de n-de vermelding in de overeenkomst.

### <a name="audio-stream"></a>Audio-Stream
Optioneel. Hierin wordt beschreven in de invoer audio stromen. Als dit veld niet is opgegeven, wordt de standaardwaarden opgegeven toepassing. Deze instelling is alleen toegestaan als de invoer streaming-protocol is ingesteld op RTP (MPEG-TS).

#### <a name="index"></a>Index
Het verdient aanbeveling om in een bepaald programma transportstroom (SharePoint Team Services) te verzenden. Als de invoerstroom bevat meerdere programma's, het live coderingsprogramma binnen het kanaal wordt geparseerd programma kaart tabel (bet) in de invoer, identificeert de invoer met een naam van het type stream van MPEG-2 AAC ADTS of systeem A AC-3 of systeem AC-3-B of persoonlijke PES MPEG-2 of MPEG-1 Audio- of Audio MPEG-2, en worden in de volgorde die is opgegeven in de BET. De op nul gebaseerde index wordt vervolgens gebruikt om op te halen de n-de vermelding in de overeenkomst.

#### <a name="language"></a>Taal
De taal-id van de audiostroom, conform ISO 639-2, zoals eng Als dat niet aanwezig is, is de standaardinstelling en (niet-gedefinieerd).

Er mag maximaal 8 audiostroom sets opgegeven als invoer voor het kanaal MPEG-2 TS via RTP. Echter, kunnen er geen twee vermeldingen met dezelfde waarde voor Index.

### <a id="preset"></a>Systeem-definitie
Hiermee geeft u de vooraf ingestelde moet worden gebruikt door het live coderingsprogramma binnen dit kanaal. De enige toegestane waarde is momenteel **Default720p** (standaard).

Houd er rekening mee dat als u aangepaste standaardinstellingen moet, u neemt contact op met amslived op Microsoft.com.

**Default720p** wordt de video coderen in de volgende 7 lagen.

#### <a name="output-video-stream"></a>Video uitvoerstroom
| BitRate | Breedte | Hoogte | MaxFPS | Profiel | Naam voor uitvoer van stroom |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hoog |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Main |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Main |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Main |Video_512x288_850kbps |
| 550 |384 |216 |30 |Main |Video_384x216_550kbps |
| 350 |340 |192 |30 |Basislijn |Video_340x192_350kbps |
| 200 |340 |192 |30 |Basislijn |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Audio-uitvoerstroom
Audio is naar stereo AAC Kredietbrief op 64 kbps, samplefrequentie van 44,1 kHz gecodeerd.

## <a name="signaling-advertisements"></a>Advertenties-signalering
Wanneer het kanaal Live Encoding is ingeschakeld heeft, moet u een onderdeel in de pipeline die verwerking video hebben en kunt bewerken. U kunt signaal voor het kanaal slates en/of advertenties invoegen in de uitgaande adaptieve bitrate stream. Smartphones zijn nog steeds de installatiekopieën die u gebruiken kunt voor de dekking van de invoer live feed in bepaalde gevallen (bijvoorbeeld tijdens een commerciële einde). Reclame signalen zijn tijd gesynchroniseerd signalen die u in de uitgaande stroom insluiten uitgelegd van de video speler actie te ondernemen speciale – zoals overschakelen naar een aankondiging op het juiste moment. Zie dit [blog](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) voor een overzicht van de SCTE 35 signalering mechanisme dat wordt gebruikt voor dit doel. Hieronder volgt een typisch scenario dat u in uw live gebeurtenis kan implementeren.

1. Hebben de doelgroep ophalen van een installatiekopie van een pre-gebeurtenissen voordat de gebeurtenis wordt gestart.
2. Het ophalen van een installatiekopie van een post-gebeurtenissen na afloop van de gebeurtenis viewers hebben.
3. De doelgroep ophalen van een installatiekopie van een FOUTGEBEURTENIS als er een probleem opgetreden tijdens de gebeurtenis (bijvoorbeeld stroomstoring in het stadium) hebben.
4. Een AD-BREAK-afbeelding voor het verbergen van de live gebeurtenis feed tijdens een commerciële einde verzenden.

Hieronder vindt u de eigenschappen die u instellen kunt wanneer-signalering advertenties. 

### <a name="duration"></a>Duur
De duur in seconden van de commerciële einde. Dit moet een positieve waarde dan nul worden om te beginnen met het einde van de commerciële. Wanneer een commerciële einde wordt uitgevoerd en de duur is ingesteld op nul met de CueId die overeenkomt met het commerciële continu-einde wordt die pauze wordt geannuleerd.

### <a name="cueid"></a>CueId
Een unieke ID voor de commerciële onderbreking moet worden gebruikt door downstream-toepassing voor het nemen van de juiste actie (s). Moet een positief geheel getal. U kunt deze waarde instelt op een willekeurige positief geheel getal of een upstream-systeem voor het bijhouden van de hint-ID's gebruiken. Controleer de id's op een positief geheel getal normaliseren vóór het verzenden via de API.

### <a name="show-slate"></a>Lei weergeven
Optioneel. Geeft het live coderingsprogramma overschakelen naar de [standaard lei](media-services-manage-live-encoder-enabled-channels.md#default_slate) installatiekopie tijdens een commerciële einde en de binnenkomende videofeed verbergen. Audio gedempt is ook tijdens lei. Standaard is **false**. 

De installatiekopie die gebruikt worden opgegeven via de standaard lei asset Id-eigenschap op het moment van het maken van het kanaal. De lei wordt aangepast aan de grootte van de installatiekopie weer. 

## <a name="insert-slate--images"></a>Lei afbeeldingen invoegen
Het live coderingsprogramma binnen het kanaal kan overschakelen naar een afbeelding lei een signaal ontvangen. Het kan ook een signaal ontvangen een continu lei te beëindigen. 

Het live coderingsprogramma kan worden geconfigureerd als u wilt overschakelen naar een afbeelding lei en het binnenkomende video signaal in bepaalde situaties – bijvoorbeeld verbergen tijdens een ad-einde. Als een dergelijke lei niet is geconfigureerd, wordt niet invoervideo tijdens die pauze ad gemaskeerd.

### <a name="duration"></a>Duur
De duur van de lei in seconden. Dit heeft moet een positieve waarde dan nul om te beginnen met de status. Als er een continu lei is en een duur van nul is opgegeven, wordt die lei continu worden beëindigd.

### <a name="insert-slate-on-ad-marker"></a>Slate invoegen op advertentiemarkering
Ingesteld op true, wordt deze instelling configureert wanneer u het live coderingsprogramma invoegen van een installatiekopie lei tijdens een ad-einde. De standaardwaarde is true. 

### <a id="default_slate"></a>Standaard lei Asset-Id

Optioneel. Hiermee geeft u de Asset-Id van het Media Services-actief die de installatiekopie van het lei bevat. De standaardwaarde is null. 


>[!NOTE] 
>Voordat u het kanaal maakt, moet de lei installatiekopie met de volgende beperkingen worden geüpload als een toegewezen actief (er worden geen andere bestanden moet in deze asset). Deze afbeelding wordt alleen gebruikt als het live coderingsprogramma een lei als gevolg van een ad-einde wordt ingevoegd of wordt expliciet een lei invoegen is aangegeven. Het live coderingsprogramma kan ook gaan in een modus lei tijdens bepaalde foutcondities – bijvoorbeeld als het ingevoerde signaal verloren is. Er is momenteel geen optie voor het gebruik van een aangepaste installatiekopie wanneer het live coderingsprogramma die een 'invoer signaal vermist' staat invoert. U kunt stemmen voor deze functie [hier](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel).


* Maximaal 1920 x 1080 in resolutie.
* Maximaal 3 MB groot.
* De bestandsnaam moet een *.jpg-extensie hebben.
* De afbeelding moet worden geüpload naar een Asset als de enige AssetFile in dat Asset en deze AssetFile moeten worden gemarkeerd als het primaire bestand. De Asset niet opslag versleuteld.

Als de **standaard leistenen Asset Id** niet is opgegeven, en **lei op ad-markering invoegen** is ingesteld op **true**, een standaardinstallatiekopie van het Azure Media Services wordt gebruikt voor het verbergen van de invoer video de stroom. Audio gedempt is ook tijdens lei. 

## <a name="channels-programs"></a>Programma's van kanaal
Een kanaal is gekoppeld aan programma's waarmee u het publiceren en opslaan van segmenten in een live stream kunt beheren. Kanalen beheren programma's. De kanaal-/ programmarelatie lijkt erg op traditionele media waarbij een kanaal een constante stream met inhoud heeft en een programma is afgestemd op een bepaalde getimede gebeurtenis op dat kanaal.

U kunt het aantal uren opgeven dat u de opgenomen inhoud voor het programma wilt behouden door de lengte voor **Archiefvenster** in te stellen. Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur. De lengte van een archiefvenster bepaalt ook de maximale hoeveelheid tijd die clients terug in de tijd kunnen zoeken vanaf de huidige live positie. Programma's kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elk programma dat is gekoppeld aan een Asset die de gestreamde inhoud opslaat. Een asset is toegewezen aan een blok-blob-container in Azure Storage-account en de bestanden in de asset worden opgeslagen als blobs in de container. Voor het publiceren van het programma, zodat uw klanten de stroom kunnen bekijken, moet u een OnDemand-locator voor de gekoppelde asset maken. Met deze locator kunt u een streaming-URL maken die u aan uw clients kunt leveren.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Zo kan het voor uw bedrijf nodig zijn zes uur van een programma te archiveren, maar alleen de laatste tien minuten uit te zenden. Hiervoor moet u twee gelijktijdig actieve programma's maken. Het ene programma wordt ingesteld om zes uur van de gebeurtenis te archiveren, maar het programma wordt niet gepubliceerd. Het andere programma wordt ingesteld om tien minuten te archiveren en dit programma wordt wel gepubliceerd.

Gebruik bestaande programma's niet voor nieuwe gebeurtenissen. In plaats daarvan maakt en start een nieuw programma voor elke gebeurtenis, zoals beschreven in de sectie Programming Live Streaming-toepassingen.

Start het programma wanneer u klaar bent om te streamen en te archiveren. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren. 

Als u de gearchiveerde inhoud wilt verwijderen, stopt en verwijdert u het programma en verwijdert u vervolgens de gekoppelde asset. Een asset kan niet worden verwijderd als deze wordt gebruikt door een programma. U moet eerst het programma verwijderen. 

Zelfs na het stoppen en verwijderen van het programma kunnen gebruikers de gearchiveerde inhoud als video op aanvraag streamen, mits u de asset niet hebt verwijderd.

Als u de gearchiveerde inhoud wilt behouden maar deze niet langer voor streaming beschikbaar mag zijn, verwijdert u de streaming-locator.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Een voorbeeld van een live feed ophalen
Wanneer Live Encoding is ingeschakeld, kunt u nu een voorbeeld van de live feed ophalen omdat het het kanaal is bereikt. Dit kan zijn een waardevol hulpmiddel om te controleren of het kanaal daadwerkelijk wordt uw live feed is bereikt. 

## <a id="states"></a>Kanaal statussen en hoe statussen worden toegewezen aan de facturering modus
De huidige status van een Kanaal. Mogelijke waarden:

* **Gestopt**. Dit is de beginstatus van het Kanaal nadat het is gemaakt. In deze status kunnen de eigenschappen van het Kanaal worden bijgewerkt, maar is streaming niet toegestaan.
* **Starten van**. Het Kanaal wordt gestart. In deze status zijn streaming en updates niet toegestaan. Als er een fout optreedt, wordt het Kanaal teruggezet naar de status Stopped.
* **Met**. Het Kanaal kan nu live streams verwerken.
* **Stoppen van**. Het Kanaal wordt gestopt. In deze status zijn streaming en updates niet toegestaan.
* **Verwijderen van**. Het Kanaal wordt verwijderd. In deze status zijn streaming en updates niet toegestaan.

In de volgende tabel wordt het verband tussen de verschillende Kanaalstatussen en de facturering weergegeven. 

| Kanaalstatus | Portal UI-indicatoren | In rekening gebracht? |
| --- | --- | --- |
| Starting |Starting |Nee (overgangsstatus) |
| Running |Ready (er worden geen programma's uitgevoerd)<br/>of<br/>Streaming (er wordt ten minste een programma uitgevoerd) |Ja |
| Stopping |Stopping |Nee (overgangsstatus) |
| Stopped |Stopped |Nee |

> [!NOTE]
> Op dit moment het kanaal start gemiddelde ongeveer twee minuten, maar soms kan maximaal 20 + minuten duren. Opnieuw instellen van kanaal kunnen tot vijf minuten duren.
> 
> 

## <a id="Considerations"></a>Overwegingen
* Wanneer een kanaal van **standaard** coderingstype optreedt in een verlies van invoer bron/bijdrage feed, wordt deze gecompenseerd door de bron-video/audio vervangen door een fout lei en stilte. Het kanaal blijven verzenden van een lei totdat de invoer/bijdrage feed wordt hervat. Het is raadzaam om een kanaal live niet ingesteld blijven status langer dan 2 uur. Het gedrag van het kanaal op opnieuw verbinden met invoer voorbij dat punt, kan niet worden gegarandeerd, is het gedrag van het antwoord op een opdracht voor opnieuw instellen. U moet het kanaal stoppen, verwijderen en een nieuwe maken.
* U kunt het invoerprotocol niet wijzigen terwijl het kanaal of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* Telkens wanneer u de configuratie van het live coderingsprogramma, Roep de **opnieuw** methode voor het kanaal. Voordat u het kanaal opnieuw instelt, moet u stop het programma. Nadat u het kanaal opnieuw instelt, start het programma opnieuw.
* Een kanaal kan worden gestopt, alleen wanneer de status actief heeft, en alle programma's op het kanaal zijn gestopt.
* Standaard kunt u alleen 5 kanalen toevoegen aan uw Media Services-account. Dit is een dynamische quota voor alle nieuwe accounts. Zie voor meer informatie [quota's en beperkingen](media-services-quotas-and-limitations.md).
* U kunt het invoerprotocol niet wijzigen terwijl het kanaal of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* U wordt alleen gefactureerd als het kanaal in de **met** status. Raadpleeg voor meer informatie [dit](media-services-manage-live-encoder-enabled-channels.md#states) sectie.
* De maximum aanbevolen duur van een live gebeurtenis is momenteel acht uur. Neem contact op met amslived op Microsoft.com als u een kanaal voor langere tijd wilt uitvoeren.
* Zorg ervoor dat u hebt het streaming-eindpunt van waaruit u inhoud wilt streamen in de **met** status.
* Wanneer meerdere taal nummers invoeren en tijdens het doorzoeken van live codering met Azure, alleen RTP voor invoer van meerdere talen wordt ondersteund. U kunt maximaal 8 audio-stromen met MPEG-2 TS via RTP definiëren. Meerdere audio houdt met RTMP of Smooth streaming opnemen wordt momenteel niet ondersteund. Bij het uitvoeren van live codering met [lokale live coderen](media-services-live-streaming-with-onprem-encoders.md), geldt er geen dergelijke beperking, omdat een kanaal wat wordt verzonden naar AMS passeren zonder verdere verwerking.
* De codering voorinstelling maakt gebruik van het principe van 'max framesnelheid' van 30 fps. Als de invoer is 60fps / 59.97i, de ingevoerde frames zijn verwijderd/de-interlaced op 30/29,97 fps. Als de invoer 50fps/50i is, zijn de ingevoerde frames verwijderd/de-interlaced op 25 fps. Als de invoer 25 fps is, wordt de uitvoer blijft bij 25 fps.
* Vergeet niet te stoppen YOUR kanalen wanneer u klaar bent. Als u dit niet, blijven facturering.

## <a name="known-issues"></a>Bekende problemen
* Opstarttijd-kanaal is verbeterd met een gemiddelde van 2 minuten, maar soms van toegenomen vraag kan nog steeds maximaal 20 + minuten duren.
* RTP ondersteuning is voor professionele omroeporganisaties geleverd. Raadpleeg de opmerkingen bij RTP in [dit](https://azure.microsoft.com/blog/2015/04/13/an-introduction-to-live-encoding-with-azure-media-services/) blog.
* Lei afbeeldingen moeten voldoen aan beschreven beperkingen [hier](media-services-manage-live-encoder-enabled-channels.md#default_slate). Als u probeert een kanaal maken met een standaard lei die groter is dan 1920 x 1080, de aanvraag zal uiteindelijk fout optreedt.
* Nogmaals... STOP deze kanalen Vergeet niet wanneer u klaar bent streaming. Als u dit niet, blijven facturering.

## <a name="next-step"></a>Volgende stap
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen
[Live Streaming-gebeurtenissen met Azure mediaservices leveren](media-services-overview.md)

[Kanalen waarmee live encoding uitvoeren van een afzonderlijke bitrate naar adaptieve bitrate stream met Portal maken](media-services-portal-creating-live-encoder-enabled-channel.md)

[Maken van de kanalen waarmee live encoding uitvoeren van een afzonderlijke bitrate naar adaptieve bitrate stream met .NET SDK](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[Kanalen met REST API beheren](https://docs.microsoft.com/rest/api/media/operations/channel)
 
[Media Services-concepten](media-services-concepts.md)

[Azure mediaservices gefragmenteerde MP4 Live opnemen specificatie](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

