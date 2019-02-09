---
title: Azure mediaservices - signalering getimede metagegevens in de Live Streaming | Microsoft Docs
description: Deze specificatie bevat een overzicht van de twee modi die worden ondersteund door Media Services voor signalering getimede metagegevens in live streamen. Dit biedt ondersteuning voor algemene getimede metagegevens signalen, evenals SCTE 35 signalering voor ad splice invoegen.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: johndeu;
ms.openlocfilehash: 3c51b5fd5bf577479aaeb316d5c1b5f8704f2d19
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55980428"
---
# <a name="signaling-timed-metadata-in-live-streaming-legacy"></a>Metagegevens is een time-out opgetreden in de Live Streaming (verouderde)-signalering


## <a name="1-introduction"></a>1-Inleiding 
Om te faciliteren van het invoegen van advertenties of aangepaste gebeurtenissen op een clientspeler, tv-zenders vaak maken gebruik van de getimede metagegevens van ingesloten in de video. Om in te schakelen deze scenario's, biedt Media Services ondersteuning voor het transport van getimede metagegevens samen met de media, vanaf het moment van de opname van het kanaal voor live streaming naar de clienttoepassing.
Deze specificatie bevat twee modi die worden ondersteund door Media Services voor getimede metagegevens in live streaming signalen:

1. [SCTE-35] dat de aanbevolen procedures beschreven door [SCTE-67] heeds-signalering

2. Een algemene is een time-out opgetreden van de modus voor berichten die niet-signalering metagegevens [SCTE-35]

### <a name="12-conformance-notation"></a>1.2-conformiteit notatie
De sleutel woorden 'moet', zijn 'Mag niet', 'Vereist', "Worden", "Wordt niet", 'SHOULD', 'Mag geen', 'Aanbevolen', "Mei" en 'Optioneel' in dit document om te worden geïnterpreteerd zoals beschreven in RFC 2119

### <a name="13-terms-used"></a>1.3-termen die worden gebruikt

| Termijn              | Definitie                                                                                                                                                                                                                       |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Presentatietijd | De tijd die een gebeurtenis wordt weergegeven aan een viewer. De tijd vertegenwoordigt het moment dat op de tijdlijn van de media die een viewer u de gebeurtenis ziet. De presentatietijd van een bericht SCTE 35 splice_info() opdracht wordt bijvoorbeeld de splice_time(). |
| Ontvangsttijd      | De tijd die een bericht wordt ontvangen. De tijd is meestal hetzelfde als de presentatietijd van de gebeurtenis, aangezien gebeurtenisberichten vooraf de presentatie van de gebeurtenis worden verzonden.                                     |
| Sparse bijhouden      | Media bijhouden die niet continu en is gesynchroniseerd met een bovenliggende of het besturingselement bijhouden.                                                                                                                                    |
| Oorsprong            | De Service Azure Media streamen                                                                                                                                                                                                |
| Channel Sink      | De Azure Media Live-Streamingservice                                                                                                                                                                                           |
| HLS               | Apple HTTP Live Streaming-protocol                                                                                                                                                                                               |
| DASH              | Dynamic Adaptive Streaming via HTTP                                                                                                                                                                                             |
| Vloeiend            | Smooth Streaming-Protocol                                                                                                                                                                                                        |
| MPEG2-TS          | Stromen voor MPEG-2-Transport                                                                                                                                                                                                         |
| RTMP              | Realtime Multimedia-Protocol                                                                                                                                                                                                    |
| uimsbf            | Geheel getal zonder teken, de meest significante bits eerst.                                                                                                                                                                                    |

-----------------------

## <a name="2-timed-metadata-ingest"></a>2 getimede metagegevens opnemen
## <a name="21-rtmp-ingest"></a>2.1 RTMP opnemen
RTMP ondersteunt getimede metagegevens signalen verzonden als AMF hint berichten die zijn ingesloten in het RTMP-stream. De hint berichten kunnen enige tijd voordat u de werkelijke gebeurtenis worden gezonden, of advertentie-invoeging splice moet plaatsvinden. Ter ondersteuning van dit scenario, wordt de werkelijke tijd van de verbinding of het segment in het bericht genezen verzonden. Zie [AMF0] voor meer informatie.

De volgende tabel beschrijft de indeling van de nettolading van de AMF bericht die Media Services wordt opnemen.

De naam van het bericht AMF kan worden gebruikt om te onderscheiden van meerdere gebeurtenisstromen van hetzelfde type.  Voor [SCTE-35] berichten moet de naam van het bericht AMF "onAdCue" zoals aanbevolen in [SCTE-67].  De velden hieronder niet wordt vermeld moeten worden genegeerd, zodat de innovatie van dit ontwerp is niet in de toekomst verboden.

### <a name="signal-syntax"></a>Signaal syntaxis
Voor eenvoudige RTMP-modus ondersteunt Media Services een enkele AMF hint-bericht met de naam 'onAdCue' met de volgende indeling:

### <a name="simple-mode"></a>Eenvoudige modus

| Veldnaam | Veldtype | Vereist? | Beschrijvingen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| hint        | String     | Vereist | Het gebeurtenisbericht.  Worden moet 'SpliceOut' om aan te geven van een eenvoudige modus genereren.                                              |
| id         | String     | Vereist | Een unieke id die met een beschrijving van de verbinding of het segment. Dit exemplaar van het bericht aangeeft                            |
| duur   | Aantal     | Vereist | De duur van de verbinding. Eenheden zijn precisiewaarde.                                                                |
| elapsed    | Aantal     | Optioneel | Wanneer het signaal wordt herhaald ter ondersteuning van afstemmen op, in dit veld moet de hoeveelheid presentatietijd die is verstreken sinds het begin van de verbinding. Eenheden zijn precisiewaarde. Als u eenvoudige gebruikt, moet deze waarde niet langer zijn dan de oorspronkelijke duur van de verbinding.                                                  |
| time       | Aantal     | Vereist | De tijd van de verbinding in de presentatietijd zijn. Eenheden zijn precisiewaarde.                                     |

---------------------------

### <a name="scte-35-mode"></a>SCTE-35 Mode

| Veldnaam | Veldtype | Vereist? | Beschrijvingen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| hint        | String     | Vereist | Het gebeurtenisbericht.  Voor [SCTE-35] berichten, moet dit de met base64 (IETF RFC 4648) binaire codering splice_info_section() in volgorde van berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE-67].                                              |
| type       | String     | Vereist | Een URN of een URL voor het identificeren van het bericht-schema. Voor [SCTE-35] berichten moet dit "urn: scte:scte35:2013a:bin" in volgorde van berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE-67].  |
| id         | String     | Vereist | Een unieke id die met een beschrijving van de verbinding of het segment. Hiermee geeft u dit exemplaar van het bericht.  Berichten met gelijkwaardige semantiek moeten dezelfde waarde hebben.|
| duur   | Aantal     | Vereist | De duur van de gebeurtenis of ad splice-segment, indien bekend. Als onbekend, moet de waarde 0 zijn.                                                                 |
| elapsed    | Aantal     | Optioneel | Wanneer het signaal van de ad [SCTE-35] wordt herhaald om te stemmen, bevat dit veld moet de hoeveelheid presentatietijd die is verstreken sinds het begin van de verbinding. Eenheden zijn precisiewaarde. In de modus [SCTE-35] deze waarde kan groter zijn dan de oorspronkelijke opgegeven duur van de verbinding of het segment.                                                  |
| time       | Aantal     | Vereist | De presentatietijd van de gebeurtenis of ad genereren.  De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I. Tijdstip en de duur moeten voor uitgaand verkeer HLS uitgelijnd met de grenzen van het segment. De presentatietijd en de duur van verschillende gebeurtenisberichten binnen hetzelfde gebeurtenisstroom mogen elkaar niet overlappen. Eenheden zijn precisiewaarde.

---------------------------

#### <a name="211-cancellation-and-updates"></a>2.1.1 annulering en Updates

Berichten kunnen worden geannuleerd of door het verzenden van meerdere berichten met dezelfde id en presentatietijd bijgewerkt De presentatietijd en de ID unieke identificatie van de gebeurtenis en het laatste bericht ontvangen voor een specifieke presentatie die voldoet aan de implementatie van de pre-beperkingen is het bericht dat wordt gereageerd. De bijgewerkte gebeurtenis vervangt alle eerder ontvangen berichten. De implementatie van de pre-beperking is vier seconden. Berichten ontvangen van ten minste vier seconden voorafgaand aan het moment van de presentatie zal worden gereageerd.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 opname van gefragmenteerde MP4 (Smooth Streaming)
Raadpleeg [LIVE-FMP4] voor opname van vereisten op live stream. De volgende secties vindt u informatie met betrekking tot de opname van getimede presentatie metagegevens.  Getimede presentatie metagegevens wordt opgenomen als een sparse spoor, die is gedefinieerd in zowel het Live Server Manifest (Zie MS-SSTR) en in het vak film (moov).  Elke sparse fragment bestaat uit een film Fragment vak (moof) en Media gegevens ('mdat'), waarbij het selectievakje 'mdat' in het binaire bericht is.

#### <a name="221-live-server-manifest-box"></a>2.2.1 Manifest vak live-Server
De track voor sparse moet worden gedeclareerd in het Manifest van Live-Server met een \<textstream\> invoeren en deze moet de volgende kenmerken instellen:

| **De naam van kenmerk** | **Veldtype** | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Aantal         | Vereist      | Moet '0', die wijzen op een bijhouden met onbekende, variabele bitrate.                                                                                                                                                                                                 |
| parentTrackName    | String         | Vereist      | Moet u de naam van de bovenliggende bijhouden, waarbij de tijdcodes sparse bijhouden tijdschaal uitgelijnd zijn. De track bovenliggende mag niet een sparse bijhouden.                                                                                                                    |
| manifestOutput     | Booleaans        | Vereist      | Moet 'true', om aan te geven dat de track voor sparse worden ingesloten in het manifest Smooth client.                                                                                                                                                               |
| Subtype            | String         | Vereist      | MOET worden de vier tekens van de code "Gegevens".                                                                                                                                                                                                                         |
| Schema             | String         | Vereist      | Moet een URN of een URL voor het identificeren van het bericht-schema. Voor [SCTE-35] berichten moet dit "urn: scte:scte35:2013a:bin" in volgorde van berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE-67]. |
| trackName          | String         | Vereist      | Moet u de naam van de sparse bijhouden. De trackName kan worden gebruikt om onderscheid maken tussen meerdere gebeurtenisstreams met hetzelfde schema. Elke unieke gebeurtenisstroom moet de naam van een uniek nummer hebben.                                                                           |
| tijdschaal          | Aantal         | Optioneel      | Moet u de tijdschaal van de bovenliggende bijhouden.                                                                                                                                                                                                                      |

-------------------------------------

### <a name="222-movie-box"></a>2.2.2 vak film

Het vak film (moov) volgt Live Manifest van het Server als onderdeel van de stream-header voor een sparse bijhouden.

Het vak 'moov' moet bevatten een **TrackHeaderBox (tkhd)** vak zoals gedefinieerd in [ISO-14496-12] met de volgende beperkingen:

| **Veldnaam** | **Veldtype**          | **Vereist?** | **Beschrijving**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| duur       | niet-ondertekende 64-bits geheel getal | Vereist      | Moet 0, omdat het nummer nul voorbeelden is en de totale duur van de voorbeelden in het spoor 0 is. |
-------------------------------------

Het vak 'moov' moet bevatten een **HandlerBox (hdlr)** zoals gedefinieerd in [ISO-14496-12] met de volgende beperkingen:

| **Veldnaam** | **Veldtype**          | **Vereist?** | **Beschrijving**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | niet-ondertekende 32-bits geheel getal | Vereist      | Moet 'meta'. |
-------------------------------------

Het vak 'stsd' moet een MetaDataSampleEntry vak met de naam van een code bevatten, zoals gedefinieerd in [ISO-14496-12].  De naam van de codering moet bijvoorbeeld voor SCTE 35 berichten zijn 'scte'.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 film Fragment vak en Media gegevens

Sparse bijhouden fragmenten bestaan uit het Fragment film (moof) en een Data Box voor Media (mdat).

Het vak MovieFragmentBox (moof) moet bevatten een **TrackFragmentExtendedHeaderBox (uuid)** vak zoals gedefinieerd in [MS-SSTR] met de volgende velden:

| **Veldnaam**         | **Veldtype**          | **Vereist?** | **Beschrijving**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | niet-ondertekende 64-bits geheel getal | Vereist      | Moet u de aankomsttijd van de gebeurtenis. Deze waarde wordt het bericht met de bovenliggende track uitgelijnd.   |
| fragment_duration      | niet-ondertekende 64-bits geheel getal | Vereist      | Moet de duur van de gebeurtenis. De duur is nul om aan te geven dat de duur onbekend is. |

------------------------------------


Het vak MediaDataBox (mdat) moet de volgende indeling hebben:

| **Veldnaam**          | **Veldtype**                   | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| versie                 | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | Bepaalt de indeling van de inhoud van het vak 'mdat'. Niet-herkende versies worden genegeerd. Op dit moment is de enige ondersteunde versie 1.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | Hiermee geeft u dit exemplaar van het bericht. Berichten met gelijkwaardige semantiek hebben dezelfde waarde; dat wil zeggen, is het voldoende verwerken van een berichtvenster één gebeurtenis met dezelfde id.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | De som van de fragment_absolute_time, opgegeven in de TrackFragmentExtendedHeaderBox en de presentation_time_delta moet de presentatietijd van de gebeurtenis. De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I. Tijdstip en de duur moeten voor uitgaand verkeer HLS uitgelijnd met de grenzen van het segment. De presentatietijd en de duur van verschillende gebeurtenisberichten binnen hetzelfde gebeurtenisstroom mogen elkaar niet overlappen. |
| message                 | bytematrix                       | Vereist      | Het gebeurtenisbericht. Het bericht is de binaire splice_info_section() voor [SCTE-35] berichten, hoewel [SCTE-67] Hiermee wordt aanbevolen iets anders. Voor [SCTE-35] berichten moet dit de splice_info_section() in volgorde van berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE-67]. Voor [SCTE-35] berichten, het binaire splice_info_section() is de nettolading van het vak 'mdat' en het is niet base64-gecodeerd.                                                            |

------------------------------


### <a name="224-cancellation-and-updates"></a>2.2.4 annulering en Updates
Berichten kunnen worden geannuleerd of door het verzenden van meerdere berichten met dezelfde id en presentatietijd bijgewerkt  De presentatietijd en de ID is een unieke identificatie van de gebeurtenis. Het laatste bericht ontvangen voor een specifieke presentatie, die voldoet aan de implementatie van de pre-beperkingen, is het bericht dat wordt gereageerd. Het bijgewerkte bericht vervangt alle eerder ontvangen berichten.  De implementatie van de pre-beperking is vier seconden. Berichten ontvangen van ten minste vier seconden voorafgaand aan het moment van de presentatie zal worden gereageerd. 


## <a name="3-timed-metadata-delivery"></a>3 is een time-out opgetreden van de levering van metagegevens

Gebeurtenisgegevens stream is niet transparant voor Media Services. Media Services geeft slechts drie soorten informatie tussen de opname-eindpunt en het clienteindpunt van de. De volgende eigenschappen worden geleverd aan de client, in overeenstemming met [SCTE-67] en/of de client de streaming-protocol:

1.  Schema: een URN of de URL van het schema van het bericht te identificeren.

2.  Presentatietijd: de presentatie van de gebeurtenis op de tijdlijn van de media.

3.  Duur: de duur van de gebeurtenis.

4.  -ID: een optionele unieke id voor de gebeurtenis.

5.  Bericht: gegevens van de gebeurtenis.


## <a name="31-smooth-streaming-delivery"></a>3.1 naadloze levering van Streaming

Raadpleeg de sparse volgen de details in [MS-SSTR] verwerken.

#### <a name="smooth-client-manifest-example"></a>Manifest Smooth Client-voorbeeld
~~~ xml
<?xml version=”1.0” encoding=”utf-8”?>
<SmoothStreamingMedia MajorVersion=”2” MinorVersion=”0” TimeScale=”10000000” IsLive=”true” Duration=”0”
  LookAheadFragmentCount=”2” DVRWindowLength=”6000000000”>

  <StreamIndex Type=”video” Name=”video” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(video={start time})”>
    <QualityLevel Index=”0” Bitrate=”230000”
      CodecPrivateData=”250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <QualityLevel Index=”1” Bitrate=”305000”
      CodecPrivateData=”250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”audio” Name=”audio” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(audio={start time})”>
    <QualityLevel Index=”0” Bitrate=”96000” CodecPrivateData=”1000030000000000000000000000E00042C0”
      FourCC=”WMAP” AudioTag=”354” Channels=”2” SamplingRate=”44100” BitsPerSample=”16” PacketSize=”4459”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”text” Name=”scte35-event-stream” Subtype=”DATA” Chunks=”0” TimeScale=”10000000”
    ParentStreamIndex=”video” ManifestOutput=”true” 
    Url=”QualityLevels({bitrate})/Fragments(captions={start time})”>
    <QualityLevel Index=”0” Bitrate=”0” CodecPrivateData=”” FourCC=””>
      <CustomAttributes>
        <Attribute Name=”Scheme” Value=”urn:scte:scte35:2013a:bin”/>
      </CustomAttributes>
    </QualityLevel>
    <!-- <f> contains base-64 encoded splice_info_section() message 
    <c t=”600000000” d=”300000000”>
<f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t=”1200000000” d=”400000000”>      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia> 
~~~ 

## <a name="32--apple-hls-delivery"></a>3.2 Apple HLS levering
Getimede metagegevens voor Apple HTTP Live Streaming (HLS) kan worden ingesloten in de afspeellijst segment in een aangepaste M3U-tag.  Niveau van de toepassing kan de afspeellijst M3U parseren en M3U tags worden verwerkt. Azure Media Services wordt getimede metagegevens insluiten in de tag EXT-X-hint is gedefinieerd in [HLS].  De tag EXT-X-hint wordt momenteel gebruikt door DynaMux voor berichten van het type ADI3.  Ter ondersteuning van SCTE 35 en niet SCTE 35 berichten, stelt u de kenmerken van de tag EXT-X-hint zoals hieronder gedefinieerd:

| **De naam van kenmerk** | **Type**                      | **Vereist?**                             | **Beschrijving**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HINT                | tekenreeks tussen aanhalingstekens                 | Vereist                                  | Het bericht als een tekenreeks met Base 64 gecodeerde zoals beschreven in [IETF RFC 4648](http://tools.ietf.org/html/rfc4648). Voor [SCTE-35] berichten is dit de splice_info_section() base64-gecodeerd.                                                                                                |
| TYPE               | tekenreeks tussen aanhalingstekens                 | Vereist                                  | Een URN of een URL voor het identificeren van het bericht-schema. Voor [SCTE-35] berichten duurt het type de speciale waarde 'scte35'.                                                                                                                                |
| Id                 | tekenreeks tussen aanhalingstekens                 | Vereist                                  | Een unieke id voor de gebeurtenis. Als de ID niet opgegeven is wanneer het bericht wordt opgenomen, wordt een unieke id gegenereerd in Azure Media Services.                                                                                                                                          |
| DUUR           | decimaal getal met drijvende komma | Vereist                                  | De duur van de gebeurtenis. Als onbekend, moet de waarde 0 zijn. Eenheden zijn factional seconden.                                                                                                                                                                                           |
| VERSTREKEN            | decimaal getal met drijvende komma | Optioneel, maar vereist voor het venster schuiven | Wanneer het signaal wordt herhaald voor de ondersteuning van een sliding window van presentatie, bevat dit veld moet de hoeveelheid presentatietijd die is verstreken sinds het begin van de gebeurtenis. Eenheden zijn precisiewaarde. Deze waarde kan groter zijn dan de oorspronkelijke opgegeven duur van de verbinding of het segment. |
| TIME               | decimaal getal met drijvende komma | Vereist                                  | De presentatietijd van de gebeurtenis. Eenheden zijn precisiewaarde.                                                                                                                                                                                                                    |


Het niveau van de HLS player-toepassing wordt het TYPE gebruiken om te identificeren van de indeling van het bericht, de-bericht decoderen, toepassen van de nodige conversies en verwerken van de gebeurtenis.  De gebeurtenissen worden gesynchroniseerd in de afspeellijst segment van het spoor bovenliggende, op basis van de tijdstempel van de gebeurtenis.  Ze worden ingevoegd voordat het dichtstbijzijnde segment (#EXTINF code).

#### <a name="hls-segment-playlist-example"></a>HLS Segment afspeellijst voorbeeld
~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:346
#EXT-X-TARGETDURATION:6
#EXT-X-I-FRAMES-ONLY
#EXT-X-PROGRAM-DATE-TIME:2018-12-13T15:54:19.462Z
#EXTINF:4.000000,no-desc
KeyFrames(video_track=15447164594627600,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447164634627600,format=m3u8-aapl)
#EXT-X-CUE:ID="1026",TYPE="scte35",DURATION=30.000000,TIME=1544716520.022760,CUE="/DAlAAAAAAAAAP/wFAUAAAQCf+//KRjAfP4AKTLgAAAAAAAAVYsh2w=="
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447165474627600,format=m3u8-aapl)
~~~

#### <a name="hls-message-handling"></a>HLS bericht verwerken

Gebeurtenissen worden gesignaleerd in de afspeellijst segment van de video en audio-nummers. De positie van de tag EXT-X-hint moet altijd zijn onmiddellijk voordat de eerste HLS-segment (splice out of segment start) of onmiddellijk nadat de laatste HLS-segment (splice in of segment einde) op die de tijd en de duur van de kenmerken verwijzen, zoals wordt vereist door [ HLS].

Wanneer een sliding window van de presentatie is ingeschakeld, moet de tag EXT-X-hint vaak genoeg worden herhaald die de verbinding of het segment altijd volledig wordt beschreven in de afspeellijst segment is en het kenmerk VERSTREKEN moet worden gebruikt om aan te geven van de hoeveelheid tijd de splice of segment heeft is actief, zoals vereist door [HLS].

Wanneer een sliding window van de presentatie is ingeschakeld, worden de EXT-X-hint-codes uit de afspeellijst segment verwijderd wanneer de media keer dat ze naar verwijzen buiten de sliding window van de presentatie is hersteld.

## <a name="33--dash-delivery"></a>3.3 DASH levering
[DASH] biedt drie manieren voor het signaal gebeurtenissen:

1.  Gebeurtenissen in de MPD een signaal ontvangen
2.  In-band in de weergave (gebeurtenis-berichtvenster (emsg) met een signaal ontvangen gebeurtenissen
3.  Een combinatie van beide 1 en 2

Gebeurtenissen in de MPD een signaal ontvangen zijn nuttig voor het streamen van VOD omdat clients toegang tot alle gebeurtenissen hebben, onmiddellijk nadat de MPD is gedownload. De in-band-oplossing is handig voor live streamen omdat clients niet hoeft de MPD opnieuw downloaden. Voor de segmentering op basis van tijd bepaalt de client de URL voor het volgende segment door toe te voegen, de duur en de tijdstempel van het huidige segment. Als die aanvraag mislukt, de client wordt ervan uitgegaan dat een onderbreking en downloadt de MPD echter anders wordt voortgezet zonder te downloaden van de MPD streaming.

Azure Media Services doen beide signalering in de MPD en in-band-signalering met behulp van het bericht gebeurtenis.

#### <a name="mpd-signaling"></a>MPD-signalering

Gebeurtenissen worden gesignaleerd in de MPD met behulp van de EventStream-element, dat wordt weergegeven in de periode-element.

Het EventStream-element heeft de volgende kenmerken:

| **De naam van kenmerk** | **Type**                | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | string                  | Vereist      | Hiermee geeft u het schema van het bericht. Het schema is ingesteld op de waarde van de schema-kenmerk in het Manifest van Live-Server. De waarde moet een URN of een URL voor het identificeren van het schema message; bijvoorbeeld: 'urn: scte:scte35:2013a:bin'.                                                                |
| waarde              | string                  | Optioneel      | Een extra string-waarde die wordt gebruikt door de eigenaren van het schema voor het aanpassen van de semantiek van het bericht. Om te onderscheiden van meerdere gebeurtenisstreams met hetzelfde schema, moet de waarde worden ingesteld op de naam van de gebeurtenisstroom (trackName voor Smooth opnemen of AMF berichtnaam voor RTMP opnemen). |
| Tijdschaal          | niet-ondertekende 32-bits geheel getal | Vereist      | De tijdschaal, in tikken per seconde van de van de duurvelden in het vak 'emsg' en tijden.                                                                                                                                                                                                       |


Nul of meer elementen van de gebeurtenis zijn opgenomen in het element EventStream en hebben de volgende kenmerken:

| **De naam van kenmerk**  | **Type**                | **Vereist?** | **Beschrijving**                                                                                                                                                                                                             |
|---------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| presentation_time   | niet-ondertekende 64-bits geheel getal | Optioneel      | Moet de media presentatietijd van de gebeurtenis ten opzichte van het begin van de periode. De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I. |
| duur            | niet-ondertekende 32-bits geheel getal | Optioneel      | De duur van de gebeurtenis. Dit moet worden weggelaten als de duur onbekend is.                                                                                                                                                 |
| id                  | niet-ondertekende 32-bits geheel getal | Optioneel      | Hiermee geeft u dit exemplaar van het bericht. Berichten met gelijkwaardige semantiek moeten dezelfde waarde hebben. Als de ID niet opgegeven is wanneer het bericht wordt opgenomen, wordt een unieke id gegenereerd in Azure Media Services.             |
| De waarde van de gebeurtenis-element | string                  | Vereist      | Het gebeurtenisbericht weergegeven als een tekenreeks met Base 64 zoals beschreven in [IETF RFC 4648](http://tools.ietf.org/html/rfc4648).                                                                                                                   |

#### <a name="xml-syntax-and-example-for-dash-manifest-mpd-signaling"></a>Voorbeeld voor DASH en de syntaxis van de XML-manifest (MPD)-signalering

~~~ xml
<!-- XML Syntax -->
<xs:complexType name=”EventStreamType”>
  <xs:sequence>
    <xs:element name=”Event” type=”EventType” minOccurs=”0” maxOccurs=”unbounded”/>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute ref=”xlink:href”/>
  <xs:attribute ref=”xlink:actuate” default=”onRequest”/>
  <xs:attribute name=”schemeIdUri” type=”xs:anyURI” use=”required”/>
  <xs:attribute name=”value” type=”xs:string”/>
  <xs:attribute name=”timescale” type=”xs:unsignedInt”/>
</xs:complexType>
<!-- Event -->
<xs:complexType name=”EventType”>
  <xs:sequence>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute name=”presentationTime” type=”xs:unsignedLong” default=”0”/>
  <xs:attribute name=”duration” type=”xs:unsignedLong”/>
  <xs:attribute name=”id” type=”xs:unsignedInt”/>
  <xs:anyAttribute namespace=”##other” processContents=”lax”/>
</xs:complexType><?xml version=”1.0” encoding=”utf-8”?>


<!-- Example Section in MPD -->
  <EventStream schemeIdUri="urn:scte:scte35:2013a:bin" value="scte35_track_001_000" timescale="10000000">
        <Event presentationTime="15447165200227600" duration="300000000" id="1026">/DAlAAAAAAAAAP/wFAUAAAQCf+//KRjAfP4AKTLgAAAAAAAAVYsh2w==</Event>
        <Event presentationTime="15447166250227600" duration="300000000" id="1027">/DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==</Event>
        <Event presentationTime="15447167300227600" duration="600000000" id="1028">/DAlAAAAAAAAAP/wFAUAAAQEf+//KjkknP4AUmXAAAAAAAAAWcEldA==</Event>
        <Event presentationTime="15447168350227600" duration="600000000" id="1029">/DAlAAAAAAAAAP/wFAUAAAQFf+//KslyqP4AUmXAAAAAAAAAvKNt0w==</Event>
        <Event presentationTime="15447169400227600" duration="300000000" id="1030">/DAlAAAAAAAAAP/wFAUAAAQGf+//K1mIvP4AKTLgAAAAAAAAt2zEbw==</Event>
        <Event presentationTime="15447170450227600" duration="600000000" id="1031">/DAlAAAAAAAAAP/wFAUAAAQHf+//K+hc/v4AUmXAAAAAAAAANNRzVw==</Event>
    </EventStream>
~~~

>[!NOTE]
>Houd er rekening mee dat presentationTime is de presentatietijd van de gebeurtenis, niet de aankomsttijd van het bericht.
>

### <a name="431-in-band-event-message-box-signaling"></a>4.3.1 in-band gebeurtenis bericht Box-signalering
Een in-band-gebeurtenisstroom vereist de MPD hebben een InbandEventStream-element op het niveau van de aanpassing van woordenlijsten instellen.  Dit element heeft een verplichte schemeIdUri kenmerk en de optionele tijdschaal kenmerk, die ook worden weergegeven in de gebeurtenis berichtvenster (emsg).  Berichtvensters gebeurtenis met de schema-id's die niet zijn gedefinieerd in de MPD mag niet aanwezig zijn. Als een client DASH een berichtvenster gebeurtenis met een schema dat is niet gedefinieerd in de MPD detecteert, wordt de client wordt verwacht om dit te negeren.
Het gebeurtenisbericht-vak (emsg) biedt signalering voor algemene gebeurtenissen met betrekking tot het moment van de presentatie media. Indien aanwezig, wordt een willekeurige box 'emsg' vóór een willekeurige box 'moof' worden geplaatst.

### <a name="dash-event-message-box-emsg"></a>DASH gebeurtenis berichtvenster 'emsg'
~~~
Box Type: `emsg’
Container: Segment
Mandatory: No
Quantity: Zero or more
~~~

~~~ c
aligned(8) class DASHEventMessageBox extends FullBox(‘emsg’, version = 0, flags =0) 
{
    string scheme_id_uri;
    string value;
    unsigned int(32) timescale;
    unsigned int(32) presentation_time_delta;
    unsigned int(32) event_duration;
    unsigned int(32) id;
    unsigned int(8) message_data[];
}
~~~

De velden van de DASHEventMessageBox worden hieronder gedefinieerd:

| **Veldnaam**          | **Veldtype**          | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | string                  | Vereist      | Hiermee geeft u het schema van het bericht. Het schema is ingesteld op de waarde van de schema-kenmerk in het Manifest van Live-Server. De waarde moet een URN of een URL voor het identificeren van het bericht-schema. Voor [SCTE-35] berichten duurt dit de speciale waarde "urn: scte:scte35:2013a:bin", hoewel [SCTE-67] Hiermee wordt aanbevolen iets anders. |
| Value                   | string                  | Vereist      | Een extra string-waarde die wordt gebruikt door de eigenaren van het schema voor het aanpassen van de semantiek van het bericht. Om te onderscheiden van meerdere gebeurtenisstreams met hetzelfde schema, wordt de waarde worden ingesteld op de naam van de gebeurtenisstroom (trackName voor Smooth opnemen of AMF berichtnaam voor RTMP opnemen).                                                                  |
| Tijdschaal               | niet-ondertekende 32-bits geheel getal | Vereist      | De tijdschaal, in tikken per seconde van de van de duurvelden in het vak 'emsg' en tijden.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | niet-ondertekende 32-bits geheel getal | Vereist      | De media presentatie tijdsdelta van de presentatietijd van de gebeurtenis en de vroegste presentatietijd in dit segment. De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I.                                                                                            |
| event_duration          | niet-ondertekende 32-bits geheel getal | Vereist      | De duur van de gebeurtenis of 0xFFFFFFFF om aan te geven van een onbekende duur.                                                                                                                                                                                                                                                                                          |
| Id                      | niet-ondertekende 32-bits geheel getal | Vereist      | Hiermee geeft u dit exemplaar van het bericht. Berichten met gelijkwaardige semantiek moeten dezelfde waarde hebben. Als de ID niet opgegeven is wanneer het bericht wordt opgenomen, wordt een unieke id gegenereerd in Azure Media Services.                                                                                                                                                    |
| Message_data            | bytematrix              | Vereist      | Het gebeurtenisbericht. Voor [SCTE-35] berichten is de berichtgegevens de binaire splice_info_section(), hoewel [SCTE-67] Hiermee wordt aanbevolen iets anders.                                                                                                                                                                                                                                 |

### <a name="332-dash-message-handling"></a>3.3.2 streepje afhandeling van berichten

Gebeurtenissen worden gesignaleerd in-band, in het vak 'emsg' voor video en audio sporen te wissen.  De signalering doet zich voor alle aanvragen waarvoor de presentation_time_delta 15 seconden is segment of minder. Wanneer een sliding window van de presentatie is ingeschakeld, worden berichten voor gebeurtenissen verwijderd uit de MPD wanneer de som van de tijd en de duur van het gebeurtenisbericht kleiner is dan de tijd van de mediagegevens in het manifest.  Met andere woorden, worden de gebeurtenisberichten verwijderd uit het manifest, wanneer de media-tijd waarop ze betrekking hebben buiten de sliding window van de presentatie is hersteld.

## <a name="4-scte-35-ingest"></a>4 SCTE-35 opnemen

[SCTE-35] berichten zijn opgenomen in binaire indeling met behulp van het schema **"urn: scte:scte35:2013a:bin"** voor Smooth opnemen en het type **"scte35"** voor RTMP opnemen. De conversie van de timing [SCTE-35], die is gebaseerd op MPEG-2 transport stream presentatie tijdstempels (punten), een toewijzing tussen punten te vereenvoudigen (pts_time + pts_adjustment van de splice_time()) en de tijdlijn van de media wordt geleverd door de gebeurtenis presentatie tijd ( het veld fragment_absolute_time voor Smooth opnemen en de tijd-veld voor RTMP opnemen). De toewijzing is nodig omdat de waarde van 33-bits punten wordt getotaliseerd via gebeurt ongeveer elke 26,5 uur.

Smooth Streaming opnemen is vereist dat de Media Data Box (mdat) moet bevatten de **splice_info_section()** in tabel 8-1 [SCTE-35] gedefinieerd. Voor RTMP opnemen, de hint-kenmerk van het bericht AMF is ingesteld op de base64encoded **splice_info_section()**. Wanneer de berichten de indeling die hebben hierboven worden beschreven, zijn ze verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE-67].


## <a name="5-references"></a>5-verwijzingen

**[SCTE-35]** ANSI/SCTE 35 2013a – Digital Program Insertion Cueing Message for Cable, 2013a

**[SCTE-67]**  ANSI/SCTE 67 2014 – aanbevolen procedures voor SCTE 35: Programma voor digitale invoegen in wachtrij zetten... bericht voor de kabel

**[DASH]**  ISO/IEC 23009-1 2014 – Information technology – Dynamic adaptive streaming via HTTP (DASH) – deel 1: Media-presentatie beschrijving en segment indelingen, 2e edition

**[HLS]**  ["HTTP Live Streaming", draft-pantos-http-live-streaming-14, 14 oktober 2014](http://tools.ietf.org/html/draft-pantos-http-live-streaming-14)

**[MS-SSTR]**  ["Microsoft Smooth Streaming-Protocol', 15 mei 2014](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

**[AMF0]**  ["Actie bericht indeling AMF0"](http://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf)

**[LIVE-FMP4]** [Azure Media Services Fragmented MP4 Live Ingest Specification](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview)

**[ISO-14496-12]** ISO/IEC 14496-12: Deel 12 ISO base media bestandsindeling, vierde editie 2012-07-15.

**[RTMP]**  ["Adobe realtime berichtenprotocol', 21 December 2012](https://www.adobe.com/devnet/rtmp.html) 

------------------------------------------

## <a name="next-steps"></a>Volgende stappen
Weergave Media Services-leertrajecten.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
