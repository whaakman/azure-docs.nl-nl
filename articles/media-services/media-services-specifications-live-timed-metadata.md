---
title: Azure mediaservices - signalering is een time-out opgetreden metagegevens in Live streamen | Microsoft Docs
description: Deze specificatie bevat een overzicht van de twee modi die worden ondersteund door Media Services voor signalering getimede metagegevens binnen live streamen. Dit biedt ondersteuning voor algemene getimede metagegevens signalen, evenals SCTE 35-signalering voor het invoegen van de ad-verbinding.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2018
ms.author: johndeu;
ms.openlocfilehash: cf4541aebe0c735d66f42532c74e97bf9bbc4a5f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>-Signalering is een time-out opgetreden metagegevens in Live streamen


## <a name="1-introduction"></a>1 introduction 
Om te maken voor het invoegen van advertenties of aangepaste gebeurtenissen op een clientspeler, vaak omroeporganisaties faciliteren ingesloten gebruik van metagegevens is een time-out opgetreden in de video. Om deze scenario's, biedt Media Services ondersteuning voor het transport van getimede metagegevens samen met de media, vanaf het punt opnemen van het live streaming kanaal aan de clienttoepassing.
Deze specificatie overzichten twee modi die worden ondersteund door Media Services voor getimede metagegevens binnen live streaming signalen:

1. [SCTE 35]-signalering die heeds de aanbevolen procedures beschreven door [SCTE 67]

2. Een algemeen is een time-out opgetreden van de modus voor berichten die niet-signalering metagegevens [SCTE 35]

### <a name="12-conformance-notation"></a>1.2-conformiteit notatie
De woorden key 'moet', zijn 'Mogen niet', 'REQUIRED', 'Vermelde', "Wordt niet", 'SHOULD', 'Mag geen', 'Aanbevolen', 'Mogelijk' en 'Optioneel' in dit document moeten worden geïnterpreteerd zoals beschreven in RFC 2119

### <a name="13-terms-used"></a>1.3-termen die worden gebruikt

| Termijn              | Definitie                                                                                                                                                                                                                       |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Presentatietijd | De tijd waarop een gebeurtenis op een viewer wordt weergegeven. De tijd vertegenwoordigt het moment dat op de media-tijdlijn een viewer ziet de gebeurtenis. Bijvoorbeeld, is de presentatietijd van een bericht SCTE 35 splice_info() opdracht de splice_time(). |
| Aankomsttijd      | De tijd dat een bericht binnenkomt. De tijd is doorgaans hetzelfde als de presentatietijd van de gebeurtenis omdat event-berichten worden verzonden presentatie tevoren van de gebeurtenis.                                     |
| Sparse bijhouden      | Media die geen continue bijhouden en tijd gesynchroniseerd is met een bovenliggende of besturingselement bijhouden.                                                                                                                                    |
| Oorsprong            | De Service Azure Media Streaming                                                                                                                                                                                                |
| Kanaalfilter      | De Azure Media Live Streaming Service                                                                                                                                                                                           |
| HLS               | Apple HTTP Live Streaming-protocol                                                                                                                                                                                               |
| DASH              | Dynamische adaptieve Streaming via HTTP                                                                                                                                                                                             |
| Smooth            | Smooth Streaming-Protocol                                                                                                                                                                                                        |
| MPEG2-TS          | MPEG-2 Transport-stromen                                                                                                                                                                                                         |
| RTMP              | Realtime Multimedia Protocol                                                                                                                                                                                                    |
| uimsbf            | Niet-ondertekend geheel getal, de meest significante bits eerst.                                                                                                                                                                                    |

-----------------------

## <a name="2-timed-metadata-ingest"></a>Metagegevens van 2 is een time-out opgetreden voor opnemen
## <a name="21-rtmp-ingest"></a>2.1 RTMP opnemen
RTMP ondersteunt getimede metagegevens signalen als AMF hint berichten die zijn ingesloten in de stroom RTMP verzonden. De hint berichten kunnen enige tijd voordat de werkelijke gebeurtenis worden gezonden, of invoegen van ad-verbinding moet worden uitgevoerd. Ter ondersteuning van dit scenario wordt het daadwerkelijke tijdstip van de verbinding of een segment in het bericht twee verzonden. Zie voor meer informatie [AMF0].

De volgende tabel beschrijft de indeling van de nettolading van de AMF bericht die Media Services wordt opnemen.

De naam van het bericht AMF kan worden gebruikt om te onderscheiden van meerdere gebeurtenisstromen van hetzelfde type.  Voor [SCTE 35] berichten moet de naam van het bericht AMF 'onAdCue' zoals aanbevolen in [SCTE 67].  De velden hieronder niet wordt vermeld moeten worden genegeerd, zodat de vernieuwing van dit ontwerp is niet in de toekomst verboden.

### <a name="signal-syntax"></a>Signaal syntaxis
Media Services ondersteunt voor eenvoudige RTMP-modus is één AMF hint bericht aangeroepen 'onAdCue' met de volgende indeling:

### <a name="simple-mode"></a>Eenvoudige modus

| Veldnaam | Veldtype | Vereist? | Beschrijvingen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| hint        | Tekenreeks     | Vereist | Het gebeurtenisbericht.  Worden moeten 'SpliceOut' om aan te wijzen een vereenvoudigde weergave splitsen.                                              |
| id         | Tekenreeks     | Vereist | Een unieke identificatie met een beschrijving van de verbinding of een segment. Dit exemplaar van het bericht identificeert                            |
| Duur   | Aantal     | Vereist | De duur van de verbinding. Eenheden zijn breukdeel seconden.                                                                |
| Verstreken    | Aantal     | Optioneel | Wanneer het signaal wordt herhaald ter ondersteuning van afstemmen, dit veld moet de hoeveelheid presentatietijd die is verstreken sinds het begin van de verbinding. Eenheden zijn breukdeel seconden. Wanneer u eenvoudige modus gebruikt, moet deze waarde niet meer dan de oorspronkelijke duur van de verbinding.                                                  |
| tijd       | Aantal     | Vereist | De tijd van de verbinding in de presentatietijd zijn. Eenheden zijn breukdeel seconden.                                     |

---------------------------

### <a name="scte-35-mode"></a>SCTE-35 Mode

| Veldnaam | Veldtype | Vereist? | Beschrijvingen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| hint        | Tekenreeks     | Vereist | Het gebeurtenisbericht.  Voor [SCTE 35] berichten, moet dit de base64 (IETF RFC 4648) binaire codering splice_info_section() zodat berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE 67].                                              |
| type       | Tekenreeks     | Vereist | Een URN of een URL naar het schema bericht; bijvoorbeeld ' urn: voorbeeld:-signalering: 1.0 ".  Voor [SCTE 35] berichten moet dit 'urn: scte:scte35:2013a:bin' zodat berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE 67].  |
| id         | Tekenreeks     | Vereist | Een unieke identificatie met een beschrijving van de verbinding of een segment. Dit exemplaar van het bericht identificeert.  Berichten met gelijkwaardige semantiek moeten dezelfde waarde hebben.|
| Duur   | Aantal     | Vereist | De duur van de gebeurtenis of ad verbinding-segment, indien bekend. Als onbekend, is de waarde moet 0 zijn.                                                                 |
| Verstreken    | Aantal     | Optioneel | Wanneer het signaal van de ad [SCTE 35] wordt wordt herhaald om afstemmen, bevat dit veld moet de hoeveelheid presentatietijd die is verstreken sinds het begin van de verbinding. Eenheden zijn breukdeel seconden. In de modus [SCTE 35] deze waarde hoger zijn dan de oorspronkelijke opgegeven duur van de verbinding of een segment.                                                  |
| tijd       | Aantal     | Vereist | De presentatietijd van de gebeurtenis of ad splitsen.  De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO 14496 12] bijlage I. Voor uitgaande HLS, tijd en de duur moeten worden uitgelijnd met de grenzen van het segment. De presentatietijd en de duur van verschillende gebeurtenisberichten in hetzelfde gebeurtenisstroom mogen elkaar niet overlappen. Eenheden zijn breukdeel seconden.

---------------------------

#### <a name="211-cancelation-and-updates"></a>2.1.1 annulering en Updates

Berichten kunnen worden geannuleerd of bijgewerkt door meerdere berichten met dezelfde id en presentatietijd te verzenden De presentatietijd en de ID unieke identificatie van de gebeurtenis en het laatste bericht ontvangen voor een specifieke presentatietijd die voldoet aan vooraf roll beperkingen is het bericht dat wordt gereageerd. De bijgewerkte gebeurtenis vervangt alle eerder ontvangen berichten. De beperking vooraf vooruitrollen is vier seconden. Ten minste vier seconden voordat de presentatietijd ontvangen berichten worden gereageerd.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 gefragmenteerde MP4 (Smooth Streaming) voor opnemen
Raadpleeg [LIVE-FMP4] voor de vereisten voor live stream opnemen. De volgende secties bevatten informatie over het opnemen van getimede presentatie metagegevens.  Getimede presentatie metagegevens wordt ingenomen als een sparse spoor, die is gedefinieerd in zowel het Live Server Manifest (Zie MS SSTR) en het vak film (moov).  Elke sparse fragment bestaat uit een film Fragment vakken (moof) en Media gegevens ('mdat'), waarbij het selectievakje 'mdat' in het binaire bericht is.

#### <a name="221-live-server-manifest-box"></a>2.2.1 het Manifest vak live Server
De sparse bijhouden moet worden gedeclareerd in het Manifest van Live-Server met een \<textstream\> vermelding en deze hebben de volgende kenmerken instellen:

| **Naam van kenmerk** | **Veldtype** | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Aantal         | Vereist      | Moet '0', die wijzen op een nummer met bitrate onbekend, variabele.                                                                                                                                                                                                 |
| parentTrackName    | Tekenreeks         | Vereist      | Moet de naam van de bovenliggende baan, waarbij de sparse bijhouden tijdcodes tijdschaal uitgelijnd zijn. Het bijhouden van de bovenliggende mag niet een sparse bijhouden.                                                                                                                    |
| manifestOutput     | Boole-waarde        | Vereist      | Moet 'true', om aan te geven dat de sparse bijhouden worden ingesloten in het manifest Smooth client.                                                                                                                                                               |
| Subtype            | Tekenreeks         | Vereist      | Worden moet de vier tekencode "Gegevens".                                                                                                                                                                                                                         |
| Schema             | Tekenreeks         | Vereist      | Moet een URN of -URL voor het identificeren van het bericht schema; bijvoorbeeld ' urn: voorbeeld:-signalering: 1.0 ". Voor [SCTE 35] berichten moet dit 'urn: scte:scte35:2013a:bin' zodat berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE 67]. |
| trackName          | Tekenreeks         | Vereist      | Moet u de naam van de sparse bijhouden. De trackName kan worden gebruikt om te onderscheiden van meerdere gebeurtenisstromen met hetzelfde schema. Elke unieke gebeurtenisstroom moet de naam van een uniek nummer hebben.                                                                           |
| Tijdschaal          | Aantal         | Optioneel      | Moet u de tijdschaal van de bovenliggende bijhouden.                                                                                                                                                                                                                      |

-------------------------------------

### <a name="222-movie-box"></a>2.2.2 vak film

Het vak film (moov) volgt Live Manifest het Server als onderdeel van de stroom-header voor een sparse bijhouden.

Het vak 'moov' moet bevatten een **TrackHeaderBox (tkhd)** vak zoals gedefinieerd in [ISO 14496 12] met de volgende beperkingen:

| **Veldnaam** | **Veldtype**          | **Vereist?** | **Beschrijving**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| Duur       | 64-bits geheel getal zonder teken | Vereist      | Moet 0, omdat het vak bijhouden nul-voorbeelden heeft en de totale duur van de voorbeelden in het vak bijhouden 0 is. |
-------------------------------------

Het vak 'moov' moet bevatten een **HandlerBox (hdlr)** zoals gedefinieerd in [ISO 14496 12] met de volgende beperkingen:

| **Veldnaam** | **Veldtype**          | **Vereist?** | **Beschrijving**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | 32-bits geheel getal zonder teken | Vereist      | Moet "meta". |
-------------------------------------

Het selectievakje 'stsd' moet een MetaDataSampleEntry vak met de naam van een code bevatten, zoals gedefinieerd in [ISO 14496 12].  Bijvoorbeeld: voor SCTE 35 berichten de naam van de codering moet 'scte'.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 film Fragment vakken en Media gegevens

Sparse bijhouden fragmenten bestaan uit een film Fragment vakken (moof) en een Media gegevens (mdat).

Het vak MovieFragmentBox (moof) moet bevatten een **TrackFragmentExtendedHeaderBox (uuid)** vak zoals gedefinieerd in [MS-SSTR] met de volgende velden:

| **Veldnaam**         | **Veldtype**          | **Vereist?** | **Beschrijving**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | 64-bits geheel getal zonder teken | Vereist      | Moet de aankomsttijd van de gebeurtenis. Deze waarde wordt het bericht met de bovenliggende bijhouden uitgelijnd.   |
| fragment_duration      | 64-bits geheel getal zonder teken | Vereist      | Moet u de duur van de gebeurtenis. De duur mag nul geeft aan dat de duur onbekend is. |

------------------------------------


Het vak MediaDataBox (mdat) moet de volgende indeling hebben:

| **Veldnaam**          | **Veldtype**                   | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| versie                 | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | Bepaalt de indeling van de inhoud van het vak 'mdat'. Niet-herkende versies worden genegeerd. De enige ondersteunde versie is momenteel 1.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | Dit exemplaar van het bericht identificeert. Berichten met gelijkwaardige semantiek heeft dezelfde waarde; verwerking van een berichtvenster een gebeurtenis met dezelfde id is voldoende.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | De som van de fragment_absolute_time, opgegeven in de TrackFragmentExtendedHeaderBox en de presentation_time_delta moet de presentatietijd van de gebeurtenis. De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO 14496 12] bijlage I. Voor uitgaande HLS, tijd en de duur moeten worden uitgelijnd met de grenzen van het segment. De presentatietijd en de duur van verschillende gebeurtenisberichten in hetzelfde gebeurtenisstroom mogen elkaar niet overlappen. |
| bericht                 | bytematrix                       | Vereist      | Het gebeurtenisbericht. Het bericht is het binaire splice_info_section() voor [SCTE 35] berichten, hoewel [SCTE 67] raadt aan om iets anders. Voor [SCTE 35] berichten moet dit de splice_info_section() zodat berichten worden verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE 67]. Voor [SCTE 35] berichten, de binaire splice_info_section() is de nettolading van het vak 'mdat' en het is niet met base64 gecodeerd.                                                            |

------------------------------


### <a name="224-cancelation-and-updates"></a>2.2.4 annulering en Updates
Berichten kunnen worden geannuleerd of bijgewerkt door meerdere berichten met dezelfde id en presentatietijd te verzenden  De presentatietijd en de ID is een unieke identificatie van de gebeurtenis. Het laatste bericht ontvangen voor een specifieke presentatietijd die voldoet aan vooraf roll beperkingen, is het bericht dat wordt gereageerd. Het bijgewerkte bericht vervangt alle eerder ontvangen berichten.  De beperking vooraf vooruitrollen is vier seconden. Ten minste vier seconden voordat de presentatietijd ontvangen berichten worden gereageerd. 


## <a name="3-timed-metadata-delivery"></a>3 is een time-out opgetreden van de levering van metagegevens

Gebeurtenisgegevens van de stroom is ondoorzichtige met Media Services. Media Services geeft slechts drie soorten informatie tussen het eindpunt opnemen en het eindpunt van de client. De volgende eigenschappen worden geleverd aan de client, in overeenstemming met [SCTE 67] en/of de client de streaming-protocol:

1.  Schema een URN of een URL naar het schema van het bericht.

2.  Presentatietijd – de presentatietijd van de gebeurtenis op de tijdlijn van media.

3.  Duur: de duur van de gebeurtenis.

4.  -ID: een optionele unieke id voor de gebeurtenis.

5.  Bericht – de gebeurtenisgegevens worden opgeslagen.


## <a name="31-smooth-streaming-delivery"></a>3.1 smooth Streaming levering

Raadpleeg de sparse bijhouden afhandeling van gegevens in [MS-SSTR].

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
Getimede metagegevens voor Apple HTTP Live Streaming (HLS) kan een ingesloten in de afspeellijst segment binnen een aangepaste M3U-tag.  De toepassingslaag kan parseren van de afspeellijst M3U en M3U labels verwerken. Azure Media Services wordt metagegevens is een time-out opgetreden in de tag EXT-X-hint is gedefinieerd in [HLS] insluiten.  De tag EXT-X-hint wordt momenteel gebruikt door DynaMux voor berichten van het type ADI3.  Stel de kenmerken van de tag EXT-X-hint zoals hieronder gedefinieerd ter ondersteuning van SCTE 35 en niet SCTE 35 berichten:

| **Naam van kenmerk** | **Type**                      | **Vereist?**                             | **Beschrijving**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HINT                | tekenreeks tussen aanhalingstekens                 | Vereist                                  | Het bericht gecodeerd als een base64-tekenreeks, zoals beschreven in [IETF RFC 4648](http://tools.ietf.org/html/rfc4648). Voor [SCTE 35] berichten is dit de splice_info_section() base64-gecodeerd.                                                                                                |
| TYPE               | tekenreeks tussen aanhalingstekens                 | Vereist                                  | Een URN of een URL naar het schema bericht; bijvoorbeeld ' urn: voorbeeld:-signalering: 1.0 ". Voor [SCTE 35] berichten duurt het type de speciale waarde 'scte35'.                                                                                                                                |
| Id                 | tekenreeks tussen aanhalingstekens                 | Vereist                                  | Een unieke id voor de gebeurtenis. Als de ID niet opgegeven is wanneer het bericht wordt ingenomen, genereert Azure Media Services een unieke id.                                                                                                                                          |
| DUUR           | decimaal getal met drijvende komma | Vereist                                  | De duur van de gebeurtenis. Als onbekend, is de waarde moet 0 zijn. Eenheden zijn factional seconden.                                                                                                                                                                                           |
| ELAPSED            | decimaal getal met drijvende komma | Optioneel, maar vereist voor het venster Verschuivend | Wanneer het signaal wordt ter ondersteuning van een sliding window van presentatie wordt herhaald, bevat dit veld moet de hoeveelheid presentatietijd die is verstreken sinds het begin van de gebeurtenis. Eenheden zijn breukdeel seconden. Deze waarde overschrijdt misschien de oorspronkelijke opgegeven duur van de verbinding of een segment. |
| TIJD               | decimaal getal met drijvende komma | Vereist                                  | De presentatietijd van de gebeurtenis. Eenheden zijn breukdeel seconden.                                                                                                                                                                                                                    |


De toepassingslaag HLS player gebruiken het TYPE voor het identificeren van de indeling van het bericht, decoderen van het bericht, de benodigde tijd conversies toepassen en verwerken van de gebeurtenis.  Tijd gesynchroniseerd in de afspeellijst segment van de bovenliggende sporen, zijn de gebeurtenissen volgens de tijdstempel van de gebeurtenis.  Ze worden ingevoegd voordat het dichtstbijzijnde segment (#EXTINF tag).

#### <a name="hls-segment-playlist-example"></a>Voorbeeld van de afspeellijst HLS Segment
~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:6
#EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
#EXTINF:6.000000,no-desc
Fragments(video=0,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=60000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
#EXT-X-CUE: ID=”metadata-12.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”12.000000”, DURATION=”18.000000”,CUE=”HrwOi8vYmWVkaWEvhhaWFRlRDa=”
Fragments(video=120000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=180000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=240000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=300000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=360000000,format=m3u8-aapl)
#EXT-X-CUE: ID=”metadata-42.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”42.000000”, DURATION=”60.000000”,CUE=”PD94bWwgdm0iMS4wIiBlbmNvpD4=”
#EXTINF:6.000000,no-desc
Fragments(video=420000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=480000000,format=m3u8-aapl)
…
~~~

#### <a name="hls-message-handling"></a>HLS verwerking bericht

Gebeurtenissen worden gesignaleerd in de afspeellijst segment van elk video en audio-nummer. De positie van de tag EXT-X-hint moet altijd worden ofwel vlak voordat de eerste HLS-segment (verbinding uit of segment start) of onmiddellijk nadat de laatste HLS-segment (verbinding in of segment einde) die de kenmerken voor tijdstip en de duur verwijzen, zoals wordt vereist door [ HLS].

Als een sliding window van presentatie is ingeschakeld, moet de tag EXT-X-hint vaak genoeg worden herhaald dat de verbinding of het segment altijd volledig worden beschreven in de afspeellijst segment is en het kenmerk VERSTREKEN moet worden gebruikt om aan te geven hoe lang de verbinding of segment heeft is actief, zoals vereist door [HLS].

Als een sliding window van presentatie is ingeschakeld, worden de labels EXT-X-hint uit de afspeellijst segment verwijderd wanneer de tijd van de media die ze naar verwijzen buiten het sliding presentatievenster is teruggedraaid.

## <a name="33--dash-delivery"></a>3.3 DASH levering
[DASH] biedt drie manieren signaal gebeurtenissen:

1.  Gebeurtenissen in de MPD gesignaleerd
2.  Gebeurtenissen in-band in de weergave (gebeurtenis berichtvenster (emsg) met een signaal ontvangen
3.  Een combinatie van zowel 1 en 2

Gebeurtenissen in de MPD gesignaleerd zijn handig voor VOD streamen omdat clients toegang tot alle gebeurtenissen hebben, onmiddellijk wanneer de MPD wordt gedownload. De in-band-oplossing is nuttig voor live streamen omdat clients niet hoeven de MPD opnieuw downloaden. Voor de segmentering op basis van tijd bepaalt de client de URL voor het volgende segment door toe te voegen de duur en de tijdstempel van het huidige segment. Als dat mislukt aangevraagd, de client wordt ervan uitgegaan dat een onderbreking en downloadt de MPD, maar anders blijft zonder te downloaden van de MPD streaming.

Azure Media Services komen beide-signalering in de MPD en in-band-signalering met behulp van het bericht gebeurtenis.

#### <a name="mpd-signaling"></a>MPD-signalering

Gebeurtenissen worden in de MPD met behulp van het element EventStream, die wordt weergegeven in het element periode gesignaleerd.

Het element EventStream heeft de volgende kenmerken:

| **Naam van kenmerk** | **Type**                | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | tekenreeks                  | Vereist      | Het schema van het bericht identificeert. Het schema is ingesteld op de waarde van de schema-kenmerk in het Manifest van Live-Server. De waarde moet een URN of een URL naar het schema bericht; bijvoorbeeld ' urn: voorbeeld:-signalering: 1.0 ".                                                                |
| waarde              | tekenreeks                  | Optioneel      | Een extra string-waarde die wordt gebruikt door de eigenaars van het schema voor het aanpassen van de semantiek van het bericht. Om te kunnen onderscheiden meerdere gebeurtenisstromen met hetzelfde schema, moet de waarde worden ingesteld op de naam van de gebeurtenisstroom (trackName voor Smooth opnemen of AMF berichtnaam voor RTMP opnemen). |
| Tijdschaal          | 32-bits geheel getal zonder teken | Vereist      | De tijdschaal in ticks per seconde van de tijdstippen en de van de duurvelden in het vak 'emsg'.                                                                                                                                                                                                       |


Nul of meer elementen van de gebeurtenis zijn opgenomen in het element EventStream en hebben de volgende kenmerken:

| **Naam van kenmerk**  | **Type**                | **Vereist?** | **Beschrijving**                                                                                                                                                                                                             |
|---------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| presentation_time   | 64-bits geheel getal zonder teken | Optioneel      | Moet het medium presentatietijd van de gebeurtenis ten opzichte van het begin van de periode. De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO 14496 12] bijlage I. |
| Duur            | 32-bits geheel getal zonder teken | Optioneel      | De duur van de gebeurtenis. Dit moet worden weggelaten als de duur onbekend is.                                                                                                                                                 |
| id                  | 32-bits geheel getal zonder teken | Optioneel      | Dit exemplaar van het bericht identificeert. Berichten met gelijkwaardige semantiek moeten dezelfde waarde hebben. Als de ID niet opgegeven is wanneer het bericht wordt ingenomen, genereert Azure Media Services een unieke id.             |
| De waarde van de gebeurtenis-element | tekenreeks                  | Vereist      | Een base64-tekenreeks, zoals beschreven in het gebeurtenisbericht [IETF RFC 4648](http://tools.ietf.org/html/rfc4648).                                                                                                                   |

#### <a name="xml-syntax-and-example-for-dash-manifest-mpd-signaling"></a>Voorbeeld voor DASH en syntaxis van de XML-manifest (MPD)-signalering

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

<EventStream schemeIdUri=”urn:example:signaling:1.0” timescale=”1000” value=”player-statistics”>
  <Event presentationTime=”0” duration=”10000” id=”0”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
  <Event presentationTime=”20000” duration=”10000” id=”1”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
</EventStream>
~~~

>[!NOTE]
>Houd er rekening mee dat presentationTime is de presentatietijd van de gebeurtenis niet de aankomsttijd van het bericht.
>

### <a name="431-in-band-event-message-box-signaling"></a>4.3.1 in-band gebeurtenis bericht vak-signalering
Een in-band-gebeurtenisstroom vereist de MPD InbandEventStream element hebben op het niveau van aanpassing ingesteld.  Dit element heeft een verplichte schemeIdUri kenmerk en een optionele tijdschaal kenmerk, die ook worden weergegeven in de gebeurtenisstroom berichtvenster (emsg).  Gebeurtenis berichtvakken met schema-id's die niet zijn gedefinieerd in de MPD mag niet aanwezig zijn. Als een client DASH een berichtvenster gebeurtenis met een schema dat is niet gedefinieerd in de MPD detecteert, wordt de client verwacht wordt genegeerd.
Het gebeurtenisbericht-vak (emsg) biedt-signalering voor algemene gebeurtenissen met betrekking tot de media presentatie-tijd. Indien aanwezig, wordt een vak 'emsg' vóór een willekeurige box 'moof' worden geplaatst.

### <a name="dash-event-message-box-emsg"></a>Gebeurtenis-berichtvenster 'emsg' streepje
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
| scheme_id_uri           | tekenreeks                  | Vereist      | Het schema van het bericht identificeert. Het schema is ingesteld op de waarde van de schema-kenmerk in het Manifest van Live-Server. De waarde moet een URN of een URL naar het schema bericht; bijvoorbeeld ' urn: voorbeeld:-signalering: 1.0 ". Voor [SCTE 35] berichten gaat de speciale waarde 'urn: scte:scte35:2013a:bin', hoewel [SCTE 67] raadt aan om iets anders. |
| Waarde                   | tekenreeks                  | Vereist      | Een extra string-waarde die wordt gebruikt door de eigenaars van het schema voor het aanpassen van de semantiek van het bericht. Om meerdere gebeurtenisstromen met hetzelfde schema onderscheiden, wordt de waarde worden ingesteld op de naam van de gebeurtenisstroom (trackName voor Smooth opnemen of AMF berichtnaam voor RTMP opnemen).                                                                  |
| Tijdschaal               | 32-bits geheel getal zonder teken | Vereist      | De tijdschaal in ticks per seconde van de tijdstippen en de van de duurvelden in het vak 'emsg'.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | 32-bits geheel getal zonder teken | Vereist      | De media presentatie tijdsdelta van de presentatietijd van de gebeurtenis en de vroegste presentatietijd in dit segment. De presentatietijd en de duur moeten worden uitgelijnd met Stream toegang punten (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO 14496 12] bijlage I.                                                                                            |
| event_duration          | 32-bits geheel getal zonder teken | Vereist      | De duur van de gebeurtenis of 0xFFFFFFFF om aan te geven van een onbekende duur.                                                                                                                                                                                                                                                                                          |
| Id                      | 32-bits geheel getal zonder teken | Vereist      | Dit exemplaar van het bericht identificeert. Berichten met gelijkwaardige semantiek moeten dezelfde waarde hebben. Als de ID niet opgegeven is wanneer het bericht wordt ingenomen, genereert Azure Media Services een unieke id.                                                                                                                                                    |
| Message_data            | bytematrix              | Vereist      | Het gebeurtenisbericht. Voor [SCTE 35] berichten is de berichtgegevens de binaire splice_info_section() Hoewel [SCTE 67] raadt aan om iets anders.                                                                                                                                                                                                                                 |

### <a name="332-dash-message-handling"></a>3.3.2 streepje afhandeling van berichten

Gebeurtenissen worden gesignaleerd in-band zijn, in het vak 'emsg' video en audio-nummers.  De signalering doet zich voor alle aanvragen waarvoor de presentation_time_delta 15 seconden is segmenteren of minder. Als een sliding window van presentatie is ingeschakeld, worden event-berichten uit de MPD verwijderd wanneer de som van de tijd en de duur van het gebeurtenisbericht kleiner dan de tijd van de mediagegevens in het manifest is.  Met andere woorden, worden de event-berichten verwijderd uit het manifest, wanneer de media tijd waarnaar ze verwijzen buiten het sliding presentatievenster is teruggedraaid.

## <a name="4-scte-35-ingest"></a>4 SCTE-35 opnemen

[SCTE 35] berichten zijn geconsumeerd in binaire indeling met behulp van het schema **'urn: scte:scte35:2013a:bin'** voor Smooth opnemen en het type **'scte35'** voor RTMP opnemen. De conversie van de timing [SCTE 35], die is gebaseerd op MPEG-2 transport stroom presentatie tijdstempels (punten), een toewijzing tussen punten te vereenvoudigen (pts_time + pts_adjustment van de splice_time()) en de tijdlijn media is geleverd door de gebeurtenis presentatie tijd ( het veld fragment_absolute_time voor Smooth opnemen en het veld time voor RTMP opnemen). De toewijzing is nodig omdat de punten 33-bits waarde over ongeveer elke 26,5 uur rolt.

Smooth Streaming opnemen vereist dat het gegevens Media (mdat) moet bevatten de **splice_info_section()** in tabel 8-1 [SCTE 35] gedefinieerd. Voor RTMP opnemen, de hint-kenmerk van het bericht AMF is ingesteld op de base64encoded **splice_info_section()**. Wanneer de berichten de indeling die hebben hierboven worden beschreven, zijn ze verzonden naar HLS, Smooth en Dash-clients in overeenstemming met [SCTE 67].


## <a name="5-references"></a>5 verwijzingen

**[SCTE 35]**  ANSI/SCTE 35 2013a – digitale programma invoegen in wachtrij zetten... bericht voor 2013a-kabel

**[SCTE 67]**  ANSI/SCTE 67 2014 – aanbevolen beveiligingsprocedure voor SCTE 35: digitale programma invoegen in wachtrij zetten... bericht voor de kabel

**[DASH]**  ISO/IEC 23009 1 2014 – informatietechnologie – dynamische adaptieve streaming via HTTP-(KOPPELTEKEN) – Part 1: Media presentatie beschrijving en segment indelingen, 2e edition

**[HLS]**  ['HTTP Live Streaming', draft-pantos-http-live-streaming-14, 14 oktober 2014](http://tools.ietf.org/html/draft-pantos-http-live-streaming-14)

**[MS-SSTR]**  ["Microsoft Smooth Streaming Protocol", 15 mei 2014](http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

**[AMF0]**  ['Actie bericht indeling AMF0'](http://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf)

**[LIVE FMP4]**  [Azure mediaservices gefragmenteerde MP4 Live specificatie opnemen](https://docs.microsoft.com/en-us/azure/media-services/media-services-fmp4-live-ingest-overview)

**[ISO 14496 12]**  ISO/IEC 14496-12: deel 12 ISO base van mediabestand vierde editie 2012-07-15-indeling.

**[RTMP]** [“Adobe’s Real-Time Messaging Protocol”, December 21, 2012](https://www.adobe.com/devnet/rtmp.html) 

------------------------------------------

## <a name="next-steps"></a>Volgende stappen
Weergave Media Services-leertrajecten.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
