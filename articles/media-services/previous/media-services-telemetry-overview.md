---
title: Telemetrie voor Azure Media Services | Microsoft Docs
description: In dit artikel biedt een overzicht van telemetrie van Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: juliako
ms.openlocfilehash: 48b88aed833b0cd15f47195c67be80fe75fe153f
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005179"
---
# <a name="azure-media-services-telemetry"></a>Azure Media Services-telemetrie  

Azure Media Services (AMS) kunt u toegang tot telemetrie/metrische gegevens voor de services. De huidige versie van AMS kunt u live verzamelen van telemetriegegevens voor **kanaal**, **streamingendpoint zo**, en live **archief** entiteiten. 

Telemetrie wordt geschreven naar een storage-tabel in een Azure Storage-account dat u opgeeft (meestal het geval is, zou u het opslagaccount dat is gekoppeld aan uw AMS-account gebruikt). 

Bewaren van gegevens niet door het telemetriesysteem wordt beheerd. U kunt de oude telemetriegegevens verwijderen door de opslagtabellen verwijderen.

In dit onderwerp wordt beschreven hoe configureren en gebruiken van de AMS-telemetrie.

## <a name="configuring-telemetry"></a>Telemetrie configureren

U kunt de telemetrie configureren op een niveau granulatie onderdeel. Er zijn twee detailniveaus "Normaal" en "Uitgebreide". Op dit moment retourneert beide niveaus dezelfde informatie. Het verdient aanbeveling om te gebruiken "normaal. 

De volgende onderwerpen laten zien hoe u telemetrie inschakelen:

[Telemetrie met .NET inschakelen](media-services-dotnet-telemetry.md) 

[Telemetrie met REST inschakelen](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Telemetriegegevens gebruiken

Telemetrie wordt geschreven naar een Azure Storage-tabel in de storage-account dat u hebt opgegeven tijdens het configureren van telemetrie voor de Media Services-account. Deze sectie beschrijft de opslagtabellen voor de metrische gegevens.

U kunt de telemetrische gegevens gebruiken in een van de volgende manieren:

- Gegevens rechtstreeks vanuit Azure Table Storage (bijvoorbeeld met behulp van de Storage-SDK) lezen. Zie voor de beschrijving van de opslagtabellen telemetrie, het **verbruikt telemetriegegevens** in [dit](https://msdn.microsoft.com/library/mt742089.aspx) onderwerp.

of

- De ondersteuning voor gebruik in de Media Services .NET SDK voor het lezen van opslag-gegevens, zoals beschreven in [dit](media-services-dotnet-telemetry.md) onderwerp. 


De telemetrie-schema die hieronder worden beschreven, is ontworpen om goede prestaties binnen de grenzen van Azure Table Storage:

- Account-ID en -service-ID waarmee de telemetrie van elke service moet worden gezocht onafhankelijk van de gegevens zijn gepartitioneerd.
- Partities bevatten de datum moet een redelijke bovengrens geven over de grootte van de partitie.
- Rij-sleutels worden in volgorde van de omgekeerde tijd om toe te staan de meest recente telemetrie-items moet worden gezocht voor een bepaalde service.

Hierdoor moeten veel van de algemene query's om efficiënt zijn:

- Parallelle, ongeacht het downloaden van gegevens voor afzonderlijke services.
- Het ophalen van alle gegevens voor een bepaalde service in een bepaalde periode.
- Het ophalen van de meest recente gegevens voor een service.

### <a name="telemetry-table-storage-output-schema"></a>Uitvoerschema van telemetrie table storage

Telemetrische gegevens worden opgeslagen in een statistische functie in een tabel, "TelemetryMetrics20160321" waar "20160321" de datum van de gemaakte tabel is. Een afzonderlijke tabel telemetriesysteem gemaakt voor elke nieuwe per dag om 00:00 UTC. De tabel wordt gebruikt voor het opslaan van terugkerende waarden, zoals opnemen bitrate binnen een bepaalde tijd, aantal verzonden bytes, enzovoort. 

Eigenschap|Value|Voorbeelden/opmerkingen
---|---|---
PartitionKey|{account-ID} _ {entiteit-ID}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66<br/<br/>De account-ID is opgenomen in de partitiesleutel voor het vereenvoudigen van werkstromen waar meerdere Media Services-accounts naar hetzelfde opslagaccount schrijft.
RowKey|{seconds bij middernacht} _ {willekeurige waarde}|01688_00199<br/><br/>De rijsleutel begint met het aantal seconden tot middernacht om toe te staan van top n-query's binnen een partitie. Raadpleeg [dit artikel](../../cosmos-db/table-storage-design-guide.md#log-tail-pattern) voor meer informatie. 
Tijdstempel|Datum/tijd|Automatische-timestamp van de Azure-tabel 2016-09-09T22:43:42.241Z
Type|Het type van de entiteit die levert telemetrische gegevens|Channel/StreamingEndpoint/Archive<br/><br/>Gebeurtenistype is een string-waarde.
Name|De naam van de telemetrie-gebeurtenis|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|De tijd die de telemetrie-gebeurtenis heeft plaatsgevonden (UTC)|2016-09-09T22:42:36.924Z<br/><br/>De waargenomen tijd wordt geleverd door de entiteit die de telemetrie (bijvoorbeeld een kanaal) te verzenden. Kunnen er problemen met de synchronisatie tussen onderdelen, zodat deze waarde is geschatte tijd
ServiceID|{service-ID}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Entiteit-specifieke eigenschappen|Zoals gedefinieerd door de gebeurtenis|StreamName: stream1, Bitrate 10123...<br/><br/>De overige eigenschappen zijn gedefinieerd voor het opgegeven type. Inhoud van Azure is de sleutel-waardeparen.  (dat wil zeggen, hebben verschillende rijen in de tabel verschillende sets van eigenschappen).

### <a name="entity-specific-schema"></a>Entiteit-specifieke schema

Er zijn drie typen van de entiteit-specifieke telemetrische gegevens vermeldingen doorgegeven met de volgende frequentie:

- Streaming-eindpunten: Elke 30 seconden
- Live kanalen: Elke minuut
- Livearchief: Elke minuut

**Streaming-eindpunt**

Eigenschap|Value|Voorbeelden
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Tijdstempel|Tijdstempel|Automatische-timestamp van Azure Table 2016-09-09T22:43:42.241Z
Type|Type|StreamingEndpoint
Name|Name|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-id|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Hostnaam|Hostnaam van het eindpunt|builddemoserver.origin.mediaservices.windows.net
StatusCode|Records HTTP-status|200
ResultCode|Details van de resultaat-code|S_OK
RequestCount|Totaal aantal aanvragen in de samenstelling|3
BytesSent|Samengevoegde verzonden bytes|2987358
ServerLatency|Gemiddelde serverlatentie (inclusief opslag)|129
E2ELatency|Gemiddelde end-to-end-latentie|250

**Live kanalen**

Eigenschap|Value|Voorbeelden/opmerkingen
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Tijdstempel|Tijdstempel|Automatische-timestamp van de Azure-tabel 2016-09-09T22:43:42.241Z
Type|Type|Kanaal
Name|Name|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-id|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Type van de video, audio/tekst van bijhouden|video/audio
TrackName|Naam van het spoor|video/audio_1
Bitrate|Bitrate bijhouden|785000
CustomAttributes||   
IncomingBitrate|Werkelijke binnenkomende bitrate|784548
OverlapCount|Elkaar overlappen in de opname|0
DiscontinuityCount|Onderbreking voor bijhouden|0
LastTimestamp|Tijdstempel voor laatste opgenomen gegevens|1800488800
NonincreasingCount|Aantal fragmenten verwijderd als gevolg van niet-toenemende timestamp|2
UnalignedKeyFrames|Of er fragment(s) (via kwaliteitsniveaus ontvangen) waar frames niet uitgelijnd sleutel |True
UnalignedPresentationTime|(Alle kwaliteit niveaus/nummers) fragment(s) of we ontvangen waar presentatietijd is niet uitgelijnd|True
UnexpectedBitrate|Waar, als berekend/werkelijke bitrate voor audio/video > 40.000 bps en IncomingBitrate bijhouden == of IncomingBitrate en actualBitrate met 50 verschillen % 0 |True
In orde|Waar, als <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> alle 0 zijn|True<br/><br/>In orde is een samengestelde functie ' false ' retourneert als een van de volgende voorwaarden:<br/><br/>-OverlapCount > 0<br/>- DiscontinuityCount > 0<br/>-NonincreasingCount > 0<br/>-UnalignedKeyFrames == True<br/>-UnalignedPresentationTime == True<br/>-UnexpectedBitrate == True

**Livearchief**

Eigenschap|Value|Voorbeelden/opmerkingen
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Tijdstempel|Tijdstempel|Automatische-timestamp van de Azure-tabel 2016-09-09T22:43:42.241Z
Type|Type|Archiveren
Name|Name|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Service-id|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Programma-url|asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4bd2-8c01-a92a2b38c9ba.ism
TrackName|Naam van het spoor|audio_1
TrackType|Type van het spoor|Audio/video
CustomAttribute|Hex-tekenreeks is die onderscheid tussen de verschillende bijhouden met dezelfde naam en de bitsnelheid (meerdere camerahoek gemaakt)|
Bitrate|Bitrate bijhouden|785000
In orde|Waar, als FragmentDiscardedCount == 0 & & ArchiveAcquisitionError == False|True (deze twee waarden zijn niet aanwezig in de metrische gegevens, maar ze zijn aanwezig in de bron-gebeurtenis)<br/><br/>In orde is een samengestelde functie ' false ' retourneert als een van de volgende voorwaarden:<br/><br/>- FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError == True

## <a name="general-qa"></a>Algemene Q & A

### <a name="how-to-consume-metrics-data"></a>Het gebruik van metrische gegevens?

Metrische gegevens worden opgeslagen als een reeks Azure-tabellen in de storage-account van de klant. Deze gegevens kan worden gebruikt met behulp van de volgende hulpprogramma's:

- AMS SDK
- Microsoft Azure Storage Explorer (ondersteunt exporteren naar een indeling met door komma's gescheiden waarden en verwerkt in Excel)
- REST-API

### <a name="how-to-find-average-bandwidth-consumption"></a>Hoe kunt u het vinden van gemiddelde bandbreedte verbruikt?

Het gemiddelde bandbreedtegebruik is het gemiddelde van BytesSent gedurende een bepaalde periode.

### <a name="how-to-define-streaming-unit-count"></a>Het aantal streaming definiëren?

Het aantal streaming kan worden gedefinieerd als de maximale doorvoer van streaming van de service-eindpunten gedeeld door de maximale doorvoer van een streaming-eindpunt. De piek bruikbaar doorvoer van een streaming-eindpunt is 160 Mbps.
Stel bijvoorbeeld dat de piek-doorvoer van een klant-service is 40 MBps (de maximale waarde van BytesSent gedurende een bepaalde periode). Klik, het aantal streaming is gelijk aan (40 MBps) * (8-bits/byte) /(160 Mbps) = 2 streaming-eenheden.

### <a name="how-to-find-average-requestssecond"></a>Hoe kan ik aanvragen/seconde voor gemiddelde berekenen?

Als u zoekt het gemiddelde aantal aanvragen per seconde, compute-het gemiddelde aantal aanvragen (RequestCount) gedurende een bepaalde periode.

### <a name="how-to-define-channel-health"></a>Het definiëren van kanalen health?

Kanaal status kan worden gedefinieerd als een samengestelde Booleaanse functie dat is de waarde false als een van de volgende voorwaarden:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames == True 
- UnalignedPresentationTime == True 
- UnexpectedBitrate == True


### <a name="how-to-detect-discontinuities"></a>Hoe wijzigingen detecteren?

Voor het detecteren van wijzigingen, vindt u alle gegevens van kanaal waar DiscontinuityCount > 0. De bijbehorende ObservedTime tijdstempel geeft de tijden waarop de wijzigingen heeft plaatsgevonden.

### <a name="how-to-detect-timestamp-overlaps"></a>Het vaststellen van timestamp overlapt?

Voor het detecteren van timestamp overlapt, vindt u alle gegevens van kanaal waar OverlapCount > 0. De tijdstempel van de bijbehorende ObservedTime geeft aan dat de tijden waarop overlapt met de tijdstempel is opgetreden.

### <a name="how-to-find-streaming-request-failures-and-reasons"></a>Hoe kan ik mislukte streaming aanvragen en de redenen vinden?

Zoeken om te zoeken mislukte streaming aanvragen en de redenen, alle gegevens van Streaming-eindpunt waar ResultCode is niet gelijk aan S_OK. In het bijbehorende StatusCode veld wordt de reden voor de aanvraag-fout.

### <a name="how-to-consume-data-with-external-tools"></a>Hoe kan ik gegevens met externe hulpprogramma's gebruiken?

Telemetrische gegevens kunnen worden verwerkt en gevisualiseerd met de volgende hulpprogramma's:

- PowerBI
- Application Insights
- Azure Monitor (voorheen Shoebox)
- Dashboard voor AMS Live
- Azure-Portal (in behandeling release)

### <a name="how-to-manage-data-retention"></a>Hoe werkt het bewaren van gegevens beheren?

Het telemetriesysteem biedt geen bewaarperiode gegevensbeheer of automatische verwijdering van de oude records. U moet dus, beheren en het handmatig verwijderen van oude records uit de opslagtabel. U kunt verwijzen naar de opslag-SDK voor hoe u dit doet.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
