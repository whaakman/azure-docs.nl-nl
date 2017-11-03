---
title: Azure Media Services telemetrie | Microsoft Docs
description: In dit artikel biedt een overzicht van telemetrie van Azure Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 1b26d7925fe5bd39905d9f51d22433b1eea43af6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-media-services-telemetry"></a>Azure Media Services-telemetrie

Azure Media Services (AMS) kunt u toegang tot gegevens telemetrie/metrische gegevens voor de services. De huidige versie van het AMS kunt u het verzamelen van telemetriegegevens voor live **kanaal**, **StreamingEndpoint**, en live **archief** entiteiten. 

Telemetrie is geschreven naar een tabel opslag in een Azure Storage-account dat u opgeeft (normaal gesproken zou u het opslagaccount dat is gekoppeld aan uw AMS-account gebruiken). 

Bewaren van gegevens wordt niet beheerd door het telemetriesysteem. U kunt de oude telemetriegegevens verwijderen door de storage-tabellen te verwijderen.

In dit onderwerp wordt beschreven hoe configureren en gebruiken de AMS-telemetrie.

## <a name="configuring-telemetry"></a>Telemetrie configureren

U kunt telemetrie configureren op een niveau granulatie onderdeel. Er zijn twee detailniveaus 'Standaard' en 'Verbose'. Op dit moment kunnen beide niveaus hetzelfde resultaat geven. Het is raadzaam om gebruiken 'normaal. 

De volgende onderwerpen bevatten telemetrie inschakelen:

[Inschakelen van telemetrie met .NET](media-services-dotnet-telemetry.md) 

[Telemetrie met REST inschakelen](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Telemetrie-informatie gebruiken

Telemetrie naar een Azure Storage-tabel in het opslagaccount dat u hebt opgegeven tijdens het configureren van telemetrie voor de Media Services-account geschreven. Deze sectie beschrijft de storage-tabellen voor de metrische gegevens.

U kunt telemetrische gegevens in een van de volgende manieren gebruiken:

- Gegevens lezen rechtstreeks vanuit Azure Table Storage (bijvoorbeeld met de opslag-SDK). Zie voor de beschrijving van telemetrie storage-tabellen, de **verbruikt telemetrie informatie** in [dit](https://msdn.microsoft.com/library/mt742089.aspx) onderwerp.

of

- De ondersteuning voor gebruik in de Media Services .NET SDK voor het lezen van gegevens van opslag, zoals beschreven in [dit](media-services-dotnet-telemetry.md) onderwerp. 


De hieronder beschreven telemetrie-schema is ontworpen om goede prestaties binnen de grenzen van Azure Table Storage:

- Account-ID en service-ID om toe te staan telemetrie van elke service moeten worden opgevraagd onafhankelijk van de gegevens zijn gepartitioneerd.
- Partities bevatten de datum een redelijke bovengrens geven op de grootte van de partitie.
- Rij sleutels worden in volgorde van de omgekeerde tijd om toe te staan de meest recente telemetrie-items moeten worden opgevraagd voor een bepaalde service.

Op die manier kunnen veel van de algemene query's efficiënt:

- Parallelle, ongeacht het downloaden van gegevens voor afzonderlijke services.
- Bij het ophalen van alle gegevens voor een bepaalde service in een datumbereik.
- Ophalen van de meest recente gegevens voor een service.

### <a name="telemetry-table-storage-output-schema"></a>Opslag telemetrie-uitvoer tabelschema

Telemetriegegevens wordt opgeslagen in een statistische functie in een tabel, 'TelemetryMetrics20160321' waar '20160321' de datum van de tabel is. Telemetriesysteem maakt een afzonderlijke tabel voor elke nieuwe dag gebaseerd op 00:00 UTC. De tabel wordt gebruikt voor het opslaan van terugkerende waarden zoals opnemen bitrate binnen een bepaalde tijd, aantal verzonden bytes, enzovoort. 

Eigenschap|Waarde|Voorbeelden/opmerkingen
---|---|---
PartitionKey|{account-ID} _ {entiteit-ID}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66 < br /<br/>De account-ID is opgenomen in de partitiesleutel vereenvoudigen werkstromen waar meerdere Media Services-accounts zijn schrijven naar hetzelfde opslagaccount.
RowKey|{seconden op middernacht} _ {willekeurige waarde}|01688_00199<br/><br/>De rijsleutel begint met het aantal seconden op middernacht waarmee query's de bovenste n stijl binnen een partitie. Zie voor meer informatie [dit](../cosmos-db/table-storage-design-guide.md#log-tail-pattern) artikel. 
tijdstempel|Datum/tijd|Tijdstempel van de Azure-tabel 2016 auto-09-09T22:43:42.241Z
Type|Het type van de partij die levert telemetriegegevens|StreamingEndpoint-kanaal-archief<br/><br/>Gebeurtenistype is een string-waarde.
Naam|De naam van de telemetrie-gebeurtenis|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|De tijd die de telemetrie-gebeurtenis heeft plaatsgevonden (UTC)|2016-09-09T22:42:36.924Z<br/><br/>De waargenomen tijd wordt verstrekt door de entiteit die de telemetrie (bijvoorbeeld een kanaal) te verzenden. Er kunnen zich synchronisatieproblemen met de tussen onderdelen, zodat deze waarde is geschatte tijd
ServiceID|{service ID}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Entiteit-specifieke eigenschappen|Zoals gedefinieerd door de gebeurtenis|StreamName: stream1, Bitrate 10123...<br/><br/>De overige eigenschappen worden gedefinieerd voor de gegeven gebeurtenis-type. Azure Table-inhoud is sleutelwaarde-paren.  (dat wil zeggen, hebben verschillende rijen in de tabel verschillende sets eigenschappen).

### <a name="entity-specific-schema"></a>Entiteit-specifieke schema

Er zijn drie soorten entiteit-specifieke telemetrische gegevens vermeldingen gepusht met de volgende frequentie:

- Streaming-eindpunten: elke 30 seconden
- Live kanalen: elke minuut
- Archief Live: elke minuut

**Streaming-eindpunt**

Eigenschap|Waarde|Voorbeelden
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
tijdstempel|tijdstempel|Tijdstempel van Azure Table 2016 auto-09-09T22:43:42.241Z
Type|Type|StreamingEndpoint
Naam|Naam|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Hostnaam|Hostnaam van het eindpunt|builddemoserver.Origin.mediaservices.Windows.NET
statusCode|Registreert HTTP-status|200
resultCode|Details van resultaat code|S_OK
requestCount|Totaal aantal aanvragen in de samenvoeging|3
BytesSent|Cumulatief aantal verzonden bytes|2987358
ServerLatency|Server gemiddelde latentie (inclusief opslag)|129
E2ELatency|Gemiddelde latentie van end-to-end|250

**Live kanaal**

Eigenschap|Waarde|Voorbeelden/opmerkingen
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
tijdstempel|tijdstempel|Tijdstempel van de Azure-tabel 2016 auto-09-09T22:43:42.241Z
Type|Type|Kanaal
Naam|Naam|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Type bijhouden audio-video/tekst|video en audio
TrackName|Naam van het nummer|video/audio_1
Bitrate|Bitrate bijhouden|785000
CustomAttributes||   
IncomingBitrate|Werkelijke binnenkomende bitrate|784548
OverlapCount|Elkaar overlappen in de opnemen|0
DiscontinuityCount|Onderbreking voor bijhouden|0
LastTimestamp|Tijdstempel voor laatste opgenomen gegevens|1800488800
NonincreasingCount|Telling van fragmenten verwijderd als gevolg van niet-toenemende tijdstempel|2
UnalignedKeyFrames|Of er fragment(s) (in verschillende kwaliteitsniveaus ontvangen) waar sleutel frames niet uitgelijnd |True
UnalignedPresentationTime|Hiermee wordt aangegeven of er ontvangen fragment(s) (alle kwaliteit niveaus/nummers) waar presentatietijd is niet uitgelijnd|True
UnexpectedBitrate|Als berekend/werkelijke bitrate voor audio/video > 40.000 bps en IncomingBitrate bijhouden == True, of IncomingBitrate en actualBitrate met 50 verschillen % 0 |True
In orde|True is, als <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> alle 0 zijn|True<br/><br/>In orde is een samengestelde functie die wordt false geretourneerd bij een van de volgende voorwaarden houdt:<br/><br/>-OverlapCount > 0<br/>-DiscontinuityCount > 0<br/>-NonincreasingCount > 0<br/>-UnalignedKeyFrames == True<br/>-UnalignedPresentationTime == True<br/>-UnexpectedBitrate == True

**Live archief**

Eigenschap|Waarde|Voorbeelden/opmerkingen
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
tijdstempel|tijdstempel|Tijdstempel van de Azure-tabel 2016 auto-09-09T22:43:42.241Z
Type|Type|Archiveren
Naam|Naam|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Programma-url|Asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4bd2-8c01-a92a2b38c9ba.ISM
TrackName|Naam van het nummer|Audio_1
TrackType|Type van het nummer|Audio-video
CustomAttribute|Hexadecimale tekenreeks die onderscheidt van andere bijhouden met dezelfde naam en bitrate (meerdere camerahoek)|
Bitrate|Bitrate bijhouden|785000
In orde|True is, als FragmentDiscardedCount == 0 & & ArchiveAcquisitionError == False|True (deze twee waarden zijn niet aanwezig in de metriek maar ze zijn aanwezig in de bron-gebeurtenis)<br/><br/>In orde is een samengestelde functie die wordt false geretourneerd bij een van de volgende voorwaarden houdt:<br/><br/>-FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError == True

## <a name="general-qa"></a>Algemene Q & A

### <a name="how-to-consume-metrics-data"></a>Hoe de gegevens van de metrische gegevens gebruiken?

Metrische gegevens wordt opgeslagen als een reeks met Azure-tabellen in de storage-account van de klant. Deze gegevens kan worden gebruikt met de volgende hulpprogramma's:

- AMS SDK
- Microsoft Azure Storage Explorer (ondersteunt exporteren naar CSV-indeling en verwerkt in Excel)
- REST API

### <a name="how-to-find-average-bandwidth-consumption"></a>Het gemiddelde bandbreedteverbruik zoeken?

Het gemiddelde bandbreedtegebruik is het gemiddelde van BytesSent gedurende een bepaalde tijd.

### <a name="how-to-define-streaming-unit-count"></a>Het aantal streaming eenheden definiëren?

De streaming aantal eenheden kan worden gedefinieerd als de piek-doorvoer van de service streaming-eindpunten gedeeld door de piek-doorvoer van een streaming-eindpunt. De bruikbare piek-doorvoer van een streaming-eindpunt is 160 Mbps.
Stel bijvoorbeeld dat de piek-doorvoer van een klant-service is 40 MBps (de maximumwaarde van BytesSent gedurende een bepaalde tijd). Vervolgens de streaming eenheid telling is gelijk aan (40 MBps) * (8 bits/byte) /(160 Mbps) = 2 streaming-eenheden.

### <a name="how-to-find-average-requestssecond"></a>Het gemiddelde zoeken aanvragen/seconde?

Berekent het gemiddelde aantal aanvragen/seconde vindt het gemiddelde aantal aanvragen (RequestCount) gedurende een bepaalde tijd.

### <a name="how-to-define-channel-health"></a>Het kanaal health definiëren?

Kanaal health kan worden gedefinieerd als een samengestelde Booleaanse functie die false als een van de volgende voorwaarden houdt:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames == True 
- UnalignedPresentationTime == True 
- UnexpectedBitrate == True


### <a name="how-to-detect-discontinuities"></a>Hoe wijzigingen detecteren?

Vinden om te detecteren wijzigingen, alle gegevens van kanaal waar DiscontinuityCount > 0. De bijbehorende ObservedTime tijdstempel geeft de tijdstippen waarop de wijzigingen heeft plaatsgevonden.

### <a name="how-to-detect-timestamp-overlaps"></a>Het vaststellen van tijdstempel overlapt?

Vinden om te detecteren tijdstempel overlappingen, alle gegevens van kanaal waar OverlapCount > 0. De bijbehorende ObservedTime tijdstempel geeft aan de tijdstippen waarop de tijdstempel overlapt opgetreden.

### <a name="how-to-find-streaming-request-failures-and-reasons"></a>Het zoeken van streaming aanvraag fouten en redenen?

Vinden om te zoeken mislukte streaming aanvragen en redenen, alle gegevens van Streaming-eindpunt waar ResultCode is niet gelijk aan S_OK. Het overeenkomende veld van de StatusCode aangeeft waarom de aanvraag is mislukt.

### <a name="how-to-consume-data-with-external-tools"></a>Hoe kan ik gegevens met externe hulpprogramma's gebruiken?

Telemetrische gegevens kunnen worden verwerkt en worden weergegeven in de volgende hulpprogramma's:

- PowerBI
- Application Insights
- Monitor voor Azure (voorheen schoenendoos)
- Dashboard voor AMS Live
- Azure-Portal (in behandeling release)

### <a name="how-to-manage-data-retention"></a>Het bewaren van gegevens beheren?

Het telemetriesysteem biedt geen bewaarperiode gegevensbeheer of automatische verwijdering van de oude records. Daarom hoeft te beheren en oude records handmatig verwijderen uit de tabel opslag. U kunt verwijzen naar de opslag-SDK voor hoe u dit doet.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
