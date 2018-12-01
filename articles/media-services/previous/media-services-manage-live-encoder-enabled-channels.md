---
title: Live streamen met Azure Media Services om multi-bitrate streams te maken | Microsoft Docs
description: 'In dit onderwerp wordt beschreven hoe u een kanaal dat een single-bitrate live stream ontvangt van een on-premises coderingsprogramma en voert vervolgens live coderen naar adaptieve bitrate stream met Media Services kunt instellen. De stroom kan vervolgens worden afgeleverd bij client-afspeeltoepassing via een of meer Streaming-eindpunten, met een van de volgende protocollen voor adaptive streaming: HLS, Smooth Stream, MPEG DASH.'
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: juliako;anilmur
ms.openlocfilehash: e7159a8e3acf45105a11cc4574f9474457bed3ea
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682653"
---
# <a name="live-streaming-using-azure-media-services-to-create-multi-bitrate-streams"></a>Live streamen met Azure Media Services om multi-bitrate streams te maken

> [!NOTE]
> Vanaf 12 mei 2018 livekanalen wordt niet langer ondersteuning voor het RTP/MPEG-2-transportstroom-opnameprotocol. Voer een migratie uit van RTP/MPEG-2 naar RTMP- of gefragmenteerde MP4 (Smooth Streaming) opnameprotocollen.

## <a name="overview"></a>Overzicht
In Azure Media Services (AMS), een **kanaal** vertegenwoordigt een pijplijn voor het verwerken van live streaming-inhoud. Een **kanaal** live invoerstromen ontvangen op twee manieren:

* Een on-premises live codering verzendt een single-bitrate stream naar het kanaal dat is ingeschakeld voor live coderen met Media Services in een van de volgende indelingen: RTMP of Smooth Streaming (gefragmenteerde MP4). Het kanaal codeert de inkomende single-bitrate stream vervolgens live naar een (adaptieve) multi-bitrate videostream. Desgevraagd levert Media Services de stream aan klanten.
* Een on-premises live codering verzendt een multi-bitrate **RTMP** of **Smooth Streaming** (gefragmenteerde MP4) naar het kanaal dat is niet ingeschakeld voor het uitvoeren van realtime codering met AMS. De opgenomen streams **kanaal**passeren zonder verdere verwerking. Deze methode wordt aangeroepen **pass-through**. U kunt de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming uitvoeren: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco en Elemental. De volgende live coderingsprogramma's voeren RTMP uit: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast, Haivision, Teradek- en Tricaster-coderingsprogramma's.  Een live coderingsprogramma kan ook een stream met één bitsnelheid verzenden naar een kanaal dat niet is ingeschakeld voor Live Encoding, maar dit wordt niet aanbevolen. Desgevraagd levert Media Services de stream aan klanten.
  
  > [!NOTE]
  > Met behulp van een passthrough-methode is de meest voordelige manier om live te streamen.
  > 
  > 

Beginnen met de release van Media Services 2.10 wanneer u een kanaal maakt, kunt u opgeven op welke manier u wilt gebruiken voor uw kanaal voor het ontvangen van de invoerstroom en of u wilt gebruiken voor het kanaal live codering van uw stroom. U hebt hiervoor twee opties:

* **Geen** – Geef deze waarde op als u van plan bent te gebruiken van een on-premises live coderingsprogramma die multi-bitrate stream (een Pass Through-stream) wordt uitgevoerd. In dit geval wordt de stroom inkomende doorgegeven aan de uitvoer zonder eventuele te coderen. Dit is het gedrag van een kanaal vóór 2,10 release.  Zie voor meer informatie gedetailleerde over het werken met kanalen van dit type [Live streamen met on-premises coderingsprogramma's die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md).
* **Standard** : Kies deze waarde als u van plan bent uw single-bitrate live stream naar een multi-bitrate stream coderen met Media Services. Let erop dat er een facturering invloed voor live codering en u eraan denken moet dat facturering kosten in als u een verlaten van een live encoding kanaal in de status 'Running gebracht rekening'.  Het verdient aanbeveling uw actieve kanalen onmiddellijk te beëindigen nadat uw live streaming gebeurtenissen is voltooid om extra per uur kosten te voorkomen.

> [!NOTE]
> In dit onderwerp worden de kenmerken van kanalen die geschikt zijn voor het uitvoeren van realtime codering (**Standard** type codering). Zie voor meer informatie over het werken met kanalen die niet geschikt zijn voor het uitvoeren van realtime codering [Live streamen met on-premises coderingsprogramma's die multi-bitrate streams maken](media-services-live-streaming-with-onprem-encoders.md).
> 
> Controleer de [overwegingen met betrekking tot](media-services-manage-live-encoder-enabled-channels.md#Considerations) sectie.
> 
> 

## <a name="billing-implications"></a>Gevolgen van facturering
Een live encoding kanaal begint de facturering zodra het statusovergangen naar "Uitvoeren" via de API is.   U kunt ook de status bekijken in Azure portal of in het hulpprogramma Azure Media Services Explorer (http://aka.ms/amse).

De volgende tabel ziet u hoe kanaal statussen worden toegewezen aan facturering statussen in de API en Azure-portal. De statussen zijn enigszins tussen de API en de Portal-UX. Zodra een kanaal in de status 'Actief' via de API of in de status 'Ready' of 'Streaming' in de Azure-portal is, wordt de facturering actief zijn.
Als u wilt stoppen met het kanaal van u verdere facturering, moet u Stop het kanaal via de API of in de Azure portal.
U bent verantwoordelijk voor het stoppen van uw kanalen wanneer u klaar bent met het kanaal voor live codering.  Fout bij stoppen van een codering kanaal zal blijven facturering tot gevolg hebben.

### <a id="states"></a>Kanaal Staten en hoe deze worden toegewezen en de facturering
De huidige status van een Kanaal. Mogelijke waarden:

* **Gestopt**. Dit is de beginstatus van het kanaal nadat het is gemaakt (tenzij autostart is geselecteerd in de portal.) Er is geen facturering vindt plaats in deze status. In deze status kunnen de eigenschappen van het Kanaal worden bijgewerkt, maar is streaming niet toegestaan.
* **Vanaf**. Het Kanaal wordt gestart. Er is geen facturering vindt plaats in deze status. In deze status zijn streaming en updates niet toegestaan. Als er een fout optreedt, wordt het Kanaal teruggezet naar de status Stopped.
* **Met**. Het Kanaal kan nu live streams verwerken. Het is nu facturering van gebruik. Het kanaal om te voorkomen dat verdere facturering, moet u stoppen. 
* **Stoppen**. Het Kanaal wordt gestopt. Geen facturering vindt plaats in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.
* **Verwijderen van**. Het Kanaal wordt verwijderd. Geen facturering vindt plaats in deze tijdelijke status. In deze status zijn streaming en updates niet toegestaan.

In de volgende tabel wordt het verband tussen de verschillende Kanaalstatussen en de facturering weergegeven. 

| Kanaalstatus | Portal UI-indicatoren | Is het facturering? |
| --- | --- | --- |
| Starten |Starten |Nee (overgangsstatus) |
| In uitvoering |Ready (er worden geen programma's uitgevoerd)<br/>of<br/>Streaming (er wordt ten minste een programma uitgevoerd) |JA |
| Stoppen |Stoppen |Nee (overgangsstatus) |
| Gestopt |Gestopt |Nee |

### <a name="automatic-shut-off-for-unused-channels"></a>Automatisch afsluiten uitschakelen voor niet-gebruikte kanalen
Beginnen met 25 januari 2016, Media Services geïmplementeerd een update die een kanaal automatisch gestopt (met live encoding ingeschakeld) nadat deze is gestart in een niet-gebruikte staat voor een lange periode. Dit geldt voor de kanalen die geen actieve programma's hebben en die een invoer bijdrage-kanaal voor langere tijd niet hebben ontvangen.

De drempelwaarde voor een niet-gebruikte periode is nominaal 12 uur, maar kan worden gewijzigd.

## <a name="live-encoding-workflow"></a>Live Encoding-werkstroom
Het volgende diagram staat voor een live streaming-werkstroom waarbij een kanaal een single-bitrate stream in een van de volgende protocollen ontvangt: RTMP of Smooth Streaming; deze codeert stream naar een multi-bitrate stream vervolgens. 

![Live-werkstroom][live-overview]

## <a id="scenario"></a>Algemeen Scenario voor Live streamen
Hieronder volgen de algemene stappen voor het maken van veelvoorkomende toepassingen voor live streamen.

> [!NOTE]
> De maximum aanbevolen duur van een live gebeurtenis is momenteel acht uur. Neem contact op met amslived@microsoft.com als u een kanaal voor langere tijd wilt uitvoeren. Er is een facturering invloed voor live codering en moet u eraan denken dat terwijl er een live encoding kanaal in de status 'Running' worden kosten per uur facturering.  Het verdient aanbeveling uw actieve kanalen onmiddellijk te beëindigen nadat uw live streaming gebeurtenissen is voltooid om extra per uur kosten te voorkomen. 
> 
> 

1. Sluit een videocamera aan op een computer. Start en configureer een on-premises live coderingsprogramma dat een **één** bitrate stream in een van de volgende protocollen: RTMP of Smooth Streaming. 
   
    Deze stap kan ook worden uitgevoerd nadat u uw kanaal hebt gemaakt.
2. Maak en start een kanaal. 
3. Haal de URL voor opnemen voor het kanaal op. 
   
    De URL voor opnemen wordt gebruikt door het live coderingsprogramma om de stream naar het kanaal te verzenden.
4. Haal de voorbeeld-URL voor het kanaal op. 
   
    Gebruik deze URL om te controleren of de livestream goed door het kanaal wordt ontvangen.
5. Maak een programma. 
   
    Wanneer u de Azure-portal, het maken van een programma ook een asset gemaakt. 
   
    Wanneer u .NET SDK of REST moet u een asset maken en opgeven voor het gebruik van deze asset bij het maken van een programma. 
6. Publiceer de asset die zijn gekoppeld aan het programma.   
   
    >[!NOTE]
    >Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Het streaming-eindpunt van waar u inhoud wilt streamen, moet de status **Wordt uitgevoerd** hebben. 
    
7. Start het programma wanneer u klaar bent om te streamen en te archiveren.
8. Het live coderingsprogramma kan desgewenst een signaal ontvangen dat een advertentie moet worden gestart. De advertentie wordt ingevoegd in de uitvoerstream.
9. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren.
10. Verwijder het programma (en verwijder desgewenst de asset).   

> [!NOTE]
> Het is heel belangrijk dat u niet vergeten een Livekanaal met codering stoppen. Let erop dat er elk uur facturering impact voor live codering en u eraan denken moet dat facturering kosten in als u een verlaten van een live encoding kanaal in de status 'Running gebracht rekening'.  Het verdient aanbeveling uw actieve kanalen onmiddellijk te beëindigen nadat uw live streaming gebeurtenissen is voltooid om extra per uur kosten te voorkomen. 
> 
> 

## <a id="channel"></a>De invoer van kanaal (opnemen) configuraties
### <a id="Ingest_Protocols"></a>Streaming-opnameprotocol
Als de **Type coderingsprogramma** is ingesteld op **Standard**, geldige opties zijn:

* Single-bitrate **RTMP**
* Single-bitrate **gefragmenteerd MP4** (Smooth Streaming)

#### <a id="single_bitrate_RTMP"></a>Single-bitrate RTMP
Overwegingen:

* De stroom inkomende mag niet multi-bitrate-video
* De video-stream moet een gemiddelde bitrate hieronder 15 Mbps
* De audiostream moet een gemiddelde bitrate hieronder 1 Mbps
* Hier volgen de ondersteunde codecs:
* MPEG-4 AVC / H.264-Video
* Basislijn, Main, hoge profiel (8-bits versie 4:2:0)
* Hoge 10-profiel (10-bits versie 4:2:0)
* Hoge 422-profiel (10-bits versie 4:2:2)
* MPEG-2 AAC-LC Audio
* Mono, Stereo, Omgeef (5.1, 7.1)
* steekproeffrequentie 44,1 kHz
* MPEG-2-stijl ADTS verpakking
* Aanbevolen coderingsprogramma's zijn onder andere:
* Telestream Wirecast
* Flash Media Live Encoder

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Single-bitrate Fragmented MP4 (Smooth Streaming);
Typische gebruiksscenario's:

Gebruik on-premises live coderingsprogramma's van leveranciers zoals Elemental technologieën, Ericsson, Ateme, Envivio voor het verzenden van de invoerstroom via het open internet op in de buurt Azure-Datacenter.

Overwegingen:

Hetzelfde als voor [single-bitrate RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP).

#### <a name="other-considerations"></a>Andere overwegingen
* U kunt het invoerprotocol niet wijzigen terwijl het kanaal of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* Maximale resolutie voor de binnenkomende videostream is 1920 x 1080, en maximaal 60 velden/seconde als geïnterlinieerde of 30 frames per seconde als progressieve.

### <a name="ingest-urls-endpoints"></a>URL's (eindpunten) voor opnemen
Een kanaal biedt een invoereindpunt (URL voor opnemen) dat u in het live coderingsprogramma, opgeeft zodat de encoder kunt pushen stromen naar uw kanalen.

Zodra u een kanaal hebt gemaakt, kunt u de opname-URL's krijgen. Als u deze URL's, het kanaal niet hoeft te zijn de **met** staat. Wanneer u klaar om te beginnen met het pushen van gegevens naar het kanaal bent, moet zich in de **met** staat. Nadat het kanaal begint het ophalen van gegevens, kunt u uw stroom via de URL van de Preview-versie kunt bekijken.

Hebt u een optie van het opnemen van gefragmenteerde MP4 (Smooth Streaming) live stream via een SSL-verbinding. Als u wilt opnemen via SSL, zorg ervoor dat u de URL voor opnemen bijwerkt naar HTTPS. Op dit moment biedt geen AMS ondersteuning voor SSL met aangepaste domeinen.  

### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die video publiceren naar dit kanaal mogen definiëren. Toegestane IP-adressen worden opgegeven als een enkel IP-adres (bijvoorbeeld '10.0.0.1'), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld ' 10.0.0.1/22') of een IP-bereik met behulp van een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').

Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, zijn er geen IP-adressen toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.

## <a name="channel-preview"></a>Kanaalvoorbeeld
### <a name="preview-urls"></a>Voorbeeld-URL 's
Kanalen bevatten een preview-eindpunt (de voorbeeld-URL) die u gebruikt om te bekijken en uw stream te valideren voordat deze verder wordt verwerkt en geleverd.

U kunt de voorbeeld-URL ophalen bij het maken van het kanaal. Als u de URL, het kanaal niet hoeft te zijn de **met** staat.

Nadat het kanaal begint het ophalen van gegevens, kunt u uw stream kunt bekijken.

> [!NOTE]
> De preview-stream momenteel alleen kan worden geleverd in gefragmenteerd MP4 (Smooth Streaming)-indeling, ongeacht het type van de opgegeven ingang.  U kunt een speler die worden gehost in Azure portal gebruiken om uw stream weer te geven.
> 
> 

### <a name="allowed-ip-addresses"></a>Toegestane IP-adressen
U kunt de IP-adressen die zijn toegestaan verbinding maken met de preview-eindpunt definiëren. Als er geen IP-adressen zijn opgegeven IP-adressen kunnen worden. Toegestane IP-adressen worden opgegeven als een enkel IP-adres (bijvoorbeeld '10.0.0.1'), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld ' 10.0.0.1/22') of een IP-bereik met behulp van een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').

## <a name="live-encoding-settings"></a>Live encoding-instellingen
Deze sectie wordt beschreven hoe de instellingen voor het live coderingsprogramma binnen het kanaal kunnen worden aangepast, wanneer de **coderingstype** van een kanaal is ingesteld op **Standard**.

> [!NOTE]
> Uw bijdrage feed mag alleen een één audiotrack – van meerdere audionummers opnemen wordt momenteel niet ondersteund. Bij het maken van live codering met [on-premises live codeert](media-services-live-streaming-with-onprem-encoders.md), kunt u een bijdrage in de Smooth Streaming-protocol met meerdere audionummers-kanaal verzenden.
> 
> 

### <a name="ad-marker-source"></a>AD-markering bron
U kunt de bron voor ad markeringen signalen opgeven. Standaardwaarde is **Api**, wat aangeeft dat het live coderingsprogramma binnen het kanaal naar een asynchrone luisteren moet **Advertentiemarkerings-API**.

### <a name="cea-708-closed-captions"></a>CEA-708 ondertiteling
Een optionele vlag waarmee het live coderingsprogramma geen gegevens CEA-708-ondertiteling negeren in de binnenkomende video ingesloten. Als de vlag is ingesteld op false (standaard), de encoder detecteert en opnieuw CEA-708 gegevens invoegen in de video uitvoerstromen.

#### <a name="index"></a>Index
Het verdient aanbeveling om te verzenden in een bepaald programma transportstream (SharePoint Team Services). Als de invoerstroom bevat meerdere programma's, het live coderingsprogramma binnen het kanaal parseert het programma kaart tabel (bet) in de invoer, identificeert de invoer met de naam van een stream van MPEG-2 AAC ADTS of AC-3-A-systeem of AC-3-systeem-B of MPEG-2 persoonlijke PES of MPEG-1 Audio- of Audio-MPEG-2, en zorgt ervoor dat ze in de volgorde die is opgegeven in de vervaldatum De op nul gebaseerde index wordt vervolgens gebruikt om op te halen de n-de vermelding in de overeenkomst.

#### <a name="language"></a>Taal
De taal-id van de stroom van de audio, die voldoet aan ISO 639-2, zoals eng Als dat niet aanwezig is, is de standaardinstelling en (ongedefinieerd).

### <a id="preset"></a>Voorinstelling voor het systeem
Hiermee geeft u de vooraf gedefinieerde instellingen kunnen worden gebruikt door het live coderingsprogramma binnen dit kanaal. Op dit moment de enige toegestane waarde is **Default720p** (standaard).

Houd er rekening mee dat als u aangepaste voorinstellingen wilt, moet u contact opnemen amslived@microsoft.com.

**Default720p** wordt de video coderen in de volgende 6 lagen.

#### <a name="output-video-stream"></a>Uitvoer Video Stream
| BitRate | Breedte | Hoogte | MaxFPS | Profiel | Naam van de uitvoer-Stream |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hoog |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Hoog |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Hoog |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Hoog |Video_512x288_850kbps |
| 550 |384 |216 |30 |Hoog |Video_384x216_550kbps |
| 200 |340 |192 |30 |Hoog |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Uitvoer Audio Stream

Audio is naar stereo AAC-LC op 128 k, samplefrequentie van 48 kHz gecodeerd.

## <a name="signaling-advertisements"></a>Advertenties-signalering
Wanneer uw kanaal Live Encoding ingeschakeld heeft, u hebt een onderdeel in de pijplijn die verwerking video en kunt bewerken. U kunt een signaal verzenden naar het kanaal slates en/of advertenties in de uitgaande adaptieve bitrate stream invoegen. Slates zijn stilstaande beelden die u gebruiken kunt om te kunnen krijgen van de invoer live feed in bepaalde gevallen (bijvoorbeeld bij een reclameonderbreking). Reclame signalen, zijn tijd gesynchroniseerd signalen die u in de uitgaande stroom om te zien van de video speler insluiten te doen – zoals overschakelen naar een aankondiging op het juiste moment. Raadpleeg deze [blog](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) voor een overzicht van de SCTE 35 signalering mechanisme dat wordt gebruikt voor dit doel. Hieronder volgt een typisch scenario dat u in uw live-gebeurtenis kan implementeren.

1. Zijn uw viewers een installatiekopie van het pre-gebeurtenissen ophalen voordat de gebeurtenis wordt gestart.
2. De installatiekopie van een post-gebeurtenissen ophalen na afloop van de gebeurtenis kijkers hebben.
3. Uw viewers een installatiekopie van het foutbericht-GEBEURTENIS ophalen als er een probleem tijdens de gebeurtenis (bijvoorbeeld stroomstoring in het stadion) hebben.
4. Een installatiekopie van een AD-BREAK verbergen van de live-gebeurtenis bij een reclameonderbreking-kanaal verzenden.

Hieronder vindt u de eigenschappen die u instellen kunt wanneer signalering aankondigingen. 

### <a name="duration"></a>Duur
De duur, in seconden van de reclameonderbreking. De sleutel moet een positieve waarde dan nul om te beginnen de reclameonderbreking. Wanneer een reclameonderbreking wordt uitgevoerd en de duur is ingesteld op nul met de CueId die overeenkomt met de reclameonderbreking continue die pauze wordt geannuleerd.

### <a name="cueid"></a>CueId
Een unieke ID voor de reclameonderbreking moet worden gebruikt door downstream-toepassing voor het nemen van de juiste actie (s). Moet een positief geheel getal zijn. U kunt deze waarde instelt op een willekeurige positief geheel getal of een upstream-systeem gebruiken voor het bijhouden van de hint-ID's. Zorg dat alle id's positieve gehele getallen normaliseren indient via de API.

### <a name="show-slate"></a>Show slate
Optioneel. Geeft het live coderingsprogramma om over te schakelen naar de [standaard slate](media-services-manage-live-encoder-enabled-channels.md#default_slate) installatiekopie bij een reclameonderbreking en verbergen van de binnenkomende video feed. Audio gedempt is ook tijdens slate. De standaardwaarde is **false**. 

De installatiekopie die wordt gebruikt is opgegeven via de standaard slate asset Id-eigenschap op het moment dat het kanaal wordt gemaakt. De status wordt aangepast aan de grootte van de installatiekopie van weergeven. 

## <a name="insert-slate--images"></a>Afbeeldingen van Slate invoegen
Het live coderingsprogramma binnen het kanaal kunt overschakelen naar een slateafbeelding een signaal ontvangen. Het kan ook een signaal ontvangen een continue slate beëindigen. 

Het live coderingsprogramma kan worden geconfigureerd als u wilt overschakelen naar een slateafbeelding en het binnenkomende video signaal in bepaalde situaties – bijvoorbeeld verbergen tijdens een ad-einde. Als deze een slate niet is geconfigureerd, wordt niet invoervideo gemaskeerd tijdens die ad-einde.

### <a name="duration"></a>Duur
De duur van de slate in seconden. Dit heeft moet een positieve waarde dan nul om te starten van de status. Als er een continue slate, en een duur van nul is opgegeven, wordt die continue slate worden beëindigd.

### <a name="insert-slate-on-ad-marker"></a>Slate invoegen op advertentiemarkering
Ingesteld op true, deze instelling configureert als het live coderingsprogramma om in te voegen een slateafbeelding tijdens een ad-einde. De standaardwaarde is true. 

### <a id="default_slate"></a>Standaard slate Asset-Id

Optioneel. Hiermee geeft u de activa-Id van de Media Services-activa waarin de slateafbeelding. De standaardwaarde is null. 


> [!NOTE] 
> Voordat u het kanaal maakt, moet de slateafbeelding met de volgende beperkingen worden geüpload als een toegewezen asset (er worden geen andere bestanden moeten zich in deze asset). Deze installatiekopie wordt alleen gebruikt als het live coderingsprogramma een slate vanwege een ad-einde wordt ingevoegd of wordt expliciet naar een slate invoegen is aangegeven. Er is momenteel geen optie voor het gebruik van een aangepaste installatiekopie wanneer het live coderingsprogramma die een 'invoer verloren' staat invoert. U kunt stemmen voor deze functie [hier](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel).


* Maximaal 1920 x 1080 resolutie.
* Maximaal 3 MB groot.
* Naam van het bestand moet een *.jpg-extensie hebben.
* De afbeelding moet worden geüpload naar een Asset als de enige AssetFile in die actief en deze AssetFile moet worden gemarkeerd als het primaire bestand. De Asset kan niet worden versleuteld opslag.

Als de **standaard leistenen Asset-Id** niet is opgegeven, en **slate invoegen op advertentiemarkering** is ingesteld op **waar**, een standaardinstallatiekopie van Azure Media Services wordt gebruikt om de invoervideo verbergen de stroom. Audio gedempt is ook tijdens slate. 

## <a name="channels-programs"></a>Programma's van kanaal
Een kanaal is gekoppeld aan programma's waarmee u het publiceren en opslaan van segmenten in een live stream kunt beheren. Kanalen beheren programma's. De kanaal- / relatie is vergelijkbaar met traditionele media waarbij een kanaal een constante stream met inhoud heeft en een programma is afgestemd op bepaalde getimede gebeurtenis op dat kanaal.

U kunt het aantal uren opgeven dat u de opgenomen inhoud voor het programma wilt behouden door de lengte voor **Archiefvenster** in te stellen. Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur. Voor het archiefvenster bepaalt ook dat het maximum aantal wanneer clients terug in tijd kan zoeken vanaf de huidige live positie. Programma's kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elk programma dat is gekoppeld aan een Asset die de gestreamde inhoud opslaat. Een asset is toegewezen aan een blok-blob-container in de Azure Storage-account en de bestanden in de asset worden opgeslagen als blobs in deze container. Voor het publiceren van het programma, zodat uw klanten in de stroom weergeven kunnen moet u een OnDemand-locator voor de gekoppelde asset maken. Met deze locator kunt u een streaming-URL maken die u aan uw clients kunt leveren.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Zo kan het voor uw bedrijf nodig zijn zes uur van een programma te archiveren, maar alleen de laatste tien minuten uit te zenden. Hiervoor moet u twee gelijktijdig actieve programma's maken. Het ene programma wordt ingesteld om zes uur van de gebeurtenis te archiveren, maar het programma wordt niet gepubliceerd. Het andere programma wordt ingesteld om tien minuten te archiveren en dit programma wordt wel gepubliceerd.

Gebruik bestaande programma's niet voor nieuwe gebeurtenissen. In plaats daarvan maakt en start een nieuw programma voor elke gebeurtenis, zoals beschreven in de sectie van het programmeren van Live Streaming-toepassingen.

Start het programma wanneer u klaar bent om te streamen en te archiveren. Stop het programma als u het streamen wilt stoppen en de gebeurtenis wilt archiveren. 

Als u de gearchiveerde inhoud wilt verwijderen, stopt en verwijdert u het programma en verwijdert u vervolgens de gekoppelde asset. Een asset kan niet worden verwijderd als deze wordt gebruikt door een programma. U moet eerst het programma verwijderen. 

Zelfs na het stoppen en verwijderen van het programma kunnen gebruikers de gearchiveerde inhoud als video op aanvraag streamen, mits u de asset niet hebt verwijderd.

Als u de gearchiveerde inhoud wilt behouden maar deze niet langer voor streaming beschikbaar mag zijn, verwijdert u de streaming-locator.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Een voorbeeld van een live feed ophalen
Als Live Encoding is ingeschakeld, kunt u nu een Preview-versie van de live-feed toegang als het kanaal is bereikt. Dit is een waardevol hulpmiddel om te controleren of het kanaal daadwerkelijk wordt uw live-feed is bereikt. 

## <a id="states"></a>Kanaal Staten en hoe Staten toegewezen en de facturering
De huidige status van een Kanaal. Mogelijke waarden:

* **Gestopt**. Dit is de beginstatus van het Kanaal nadat het is gemaakt. In deze status kunnen de eigenschappen van het Kanaal worden bijgewerkt, maar is streaming niet toegestaan.
* **Vanaf**. Het Kanaal wordt gestart. In deze status zijn streaming en updates niet toegestaan. Als er een fout optreedt, wordt het Kanaal teruggezet naar de status Stopped.
* **Met**. Het Kanaal kan nu live streams verwerken.
* **Stoppen**. Het Kanaal wordt gestopt. In deze status zijn streaming en updates niet toegestaan.
* **Verwijderen van**. Het Kanaal wordt verwijderd. In deze status zijn streaming en updates niet toegestaan.

In de volgende tabel wordt het verband tussen de verschillende Kanaalstatussen en de facturering weergegeven. 

| Kanaalstatus | Portal UI-indicatoren | In rekening gebracht? |
| --- | --- | --- |
| Starten |Starten |Nee (overgangsstatus) |
| In uitvoering |Ready (er worden geen programma's uitgevoerd)<br/>of<br/>Streaming (er wordt ten minste een programma uitgevoerd) |Ja |
| Stoppen |Stoppen |Nee (overgangsstatus) |
| Gestopt |Gestopt |Nee |

> [!NOTE]
> Op dit moment het kanaal start gemiddelde ongeveer 2 minuten, maar soms kan maximaal 20 + minuten duren. Opnieuw instellen van kanaal kunnen tot vijf minuten duren.
> 
> 

## <a id="Considerations"></a>Overwegingen met betrekking tot
* Wanneer een kanaal van **Standard** type codering optreedt in een verlies van invoer bron/bijdrage-feed, compenseert deze voor het door de bron-video/audio vervangen door een fout slate en stilte. Het kanaal blijft een slate verzenden totdat de invoer-/ bijdrage feed wordt hervat. U wordt aangeraden een livekanaal niet ingesteld blijven in deze een status voor meer dan 2 uur. Het gedrag van het kanaal op opnieuw verbinden met invoer is niet noodzakelijkerwijs tot voorbij dat punt, is het gedrag in reactie op een opdracht voor opnieuw instellen. U moet het kanaal stopt, verwijderen en een nieuwe maken.
* U kunt het invoerprotocol niet wijzigen terwijl het kanaal of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* Telkens wanneer u de configuratie van het live coderingsprogramma, Roep de **opnieuw** methode voor het kanaal. Voordat u het kanaal opnieuw instelt, moet u het programma stoppen. Nadat u het kanaal opnieuw ingesteld, moet u het programma opnieuw starten.
* Een kanaal kan worden gestopt als deze in de status die wordt uitgevoerd is en alle programma's op het kanaal zijn gestopt.
* Standaard kunt u alleen 5 kanalen toevoegen aan uw Media Services-account. Dit is een dynamische quota voor alle nieuwe accounts. Zie voor meer informatie, [quota en beperkingen](media-services-quotas-and-limitations.md).
* U kunt het invoerprotocol niet wijzigen terwijl het kanaal of de gekoppelde programma's worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke kanalen voor elk invoerprotocol.
* U alleen een rekening ontvangt als het kanaal in de **met** staat. Raadpleeg voor meer informatie, [dit](media-services-manage-live-encoder-enabled-channels.md#states) sectie.
* De maximum aanbevolen duur van een live gebeurtenis is momenteel acht uur. Neem contact op met amslived@microsoft.com als u een kanaal voor langere tijd wilt uitvoeren.
* Zorg ervoor dat u hebt het streaming-eindpunt van waaruit u inhoud wilt streamen in de **met** staat.
* De vooraf ingestelde standaardcodering maakt gebruik van het begrip 'max framesnelheid"van 30 fps. Dus als de invoer 60fps is / 59.94i, de invoer frames zijn verwijderd/de-interlaced op 30/29,97 fps. Als de invoer 50fps/50i is, zijn de invoer frames verwijderd/de-interlaced op 25 fps. Als de invoer 25 fps is, blijft de uitvoer op 25 fps.
* Vergeet niet te stoppen uw kanalen wanneer u klaar bent. Als u dit niet doet, blijven facturering.

## <a name="known-issues"></a>Bekende problemen
* Uptime kanaal wordt gestart, is verbeterd met een gemiddelde van 2 minuten, maar soms met een grotere vraag kan nog steeds maximaal 20 + minuten duren.
* Slate afbeeldingen moeten voldoen aan de beperkingen beschreven [hier](media-services-manage-live-encoder-enabled-channels.md#default_slate). Als u probeert te maken van een kanaal met een standaard-slate die groter is dan 1920 x 1080, wordt de aanvraag wordt uiteindelijk fout.
* Nog een keer... Vergeet niet om te stoppen uw kanalen wanneer u klaar bent streaming. Als u dit niet doet, blijven facturering.

## <a name="next-step"></a>Volgende stap
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen
[Live Streaming-gebeurtenissen met Azure mediaservices leveren](media-services-overview.md)

[Maak kanalen waarmee live encoding uitvoeren van een afzonderlijke bitsnelheid naar adaptieve bitrate stream met Portal](media-services-portal-creating-live-encoder-enabled-channel.md)

[Maak kanalen waarmee live encoding uitvoeren van een afzonderlijke bitsnelheid naar adaptieve bitrate stream met .NET SDK](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[Beheren van kanalen met REST-API](https://docs.microsoft.com/rest/api/media/operations/channel)
 
[Media Services-concepten](media-services-concepts.md)

[Specificatie van Azure mediaservices Live gefragmenteerde MP4-Liveopname](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

