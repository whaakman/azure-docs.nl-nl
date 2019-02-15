---
title: Azure Event Grid-schema's voor Media Services-gebeurtenissen
description: Beschrijft de eigenschappen die beschikbaar zijn voor Media Services-gebeurtenissen met Azure Event Grid
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: reference
ms.date: 02/13/2019
ms.author: juliako
ms.openlocfilehash: 8ad0efffc89a3c11f412d94b922401c23e84a3e5
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56268784"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Azure Event Grid-schema's voor Media Services-gebeurtenissen

Dit artikel bevat de eigenschappen en schema's voor Media Services-gebeurtenissen.

Zie voor een lijst met zelfstudies en voorbeelden van scripts, [Media Services-gebeurtenisbron](../../event-grid/event-sources.md#azure-subscriptions).

## <a name="job-related-event-types"></a>Gerelateerde gebeurtenistypen

Media Services verzendt de **taak** gerelateerde typen gebeurtenissen die hieronder worden beschreven. Er zijn twee categorieën voor de **taak** gerelateerde gebeurtenissen: "Monitoring taakstatus verandert" en 'Verandert de toestand van de uitvoer controleren taak'. 

U kunt registreren voor alle gebeurtenissen met een abonnement op de gebeurtenis JobStateChange. Of u kunt zich abonneren op specifieke gebeurtenissen alleen (bijvoorbeeld laatste Staten zoals JobErrored, JobFinished en JobCanceled). 

### <a name="monitoring-job-state-changes"></a>Bewaking van de taak de status verandert

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| Een gebeurtenis voor alle wijzigingen van de taakstatus ophalen. |
| Microsoft.Media.JobScheduled| Een gebeurtenis wanneer de taak verandert de status van de geplande ophalen. |
| Microsoft.Media.JobProcessing| Een gebeurtenis wanneer de taak verandert in verwerking van de status ophalen. |
| Microsoft.Media.JobCanceling| Ophalen van een gebeurtenis wanneer de taak verandert in staat te annuleren. |
| Microsoft.Media.JobFinished| Een gebeurtenis wanneer de taak is voltooid de status verandert in ophalen. Dit is een eindtoestand die uitvoer van de taak bevat.|
| Microsoft.Media.JobCanceled| Een gebeurtenis wanneer de taak geannuleerd de status verandert in ophalen. Dit is een eindtoestand die uitvoer van de taak bevat.|
| Microsoft.Media.JobErrored| Ophalen van een gebeurtenis wanneer de taak verandert in de foutstatus. Dit is een eindtoestand die uitvoer van de taak bevat.|

Zie [Schema voorbeelden](#event-schema-examples) onderstaande.

### <a name="monitoring-job-output-state-changes"></a>Bewakingstaak uitvoeren statuswijzigingen

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Media.JobOutputStateChange| Een gebeurtenis ophalen voor alle taakuitvoer die status verandert. |
| Microsoft.Media.JobOutputScheduled| Een gebeurtenis wanneer de taak uitvoer overgangen naar geplande status ophalen. |
| Microsoft.Media.JobOutputProcessing| Een gebeurtenis wanneer de taak uitvoer overgangen naar de verwerking van de status ophalen. |
| Microsoft.Media.JobOutputCanceling| Een gebeurtenis wanneer de taak uitvoer overgang naar het annuleren van de status ophalen.|
| Microsoft.Media.JobOutputFinished| Ophalen van een gebeurtenis wanneer de taak uitvoerovergangen naar de status voltooid.|
| Microsoft.Media.JobOutputCanceled| Een gebeurtenis wanneer de taak uitvoerovergangen naar geannuleerd status ophalen.|
| Microsoft.Media.JobOutputErrored| Ophalen van een gebeurtenis wanneer de taak uitvoer overgangen naar de foutstatus.|

Zie [Schema voorbeelden](#event-schema-examples) onderstaande.

### <a name="monitoring-job-output-progress"></a>Bewakingstaak uitvoeren wordt uitgevoerd

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Media.JobOutputProgress| Deze gebeurtenis geeft de voortgang van 0% tot 100% verwerken taak. De service probeert een gebeurtenis wordt verzonden als er van 5 is sprake % of meer toename van de waarde die wordt uitgevoerd of deze meer dan 30 seconden sinds de laatste gebeurtenis (heartbeat is). De waarde van de voortgang is niet noodzakelijkerwijs om te beginnen bij 0%, of als u wilt bereiken 100%, noch deze gegarandeerd te verhogen met een constante snelheid na verloop van tijd. Deze gebeurtenis mag niet worden gebruikt om te bepalen dat de verwerking is voltooid, moet u in plaats daarvan de statuswijzigingsgebeurtenissen gebruiken.|

Zie [Schema voorbeelden](#event-schema-examples) onderstaande.

## <a name="live-event-types"></a>Live gebeurtenistypen

Media Services verzendt ook de **Live** gebeurtenistypen, zoals hieronder wordt beschreven. Er zijn twee categorieën voor de **Live** gebeurtenissen: stream-niveau en gebeurtenissen bijhouden op serverniveau. 

### <a name="stream-level-events"></a>Gebeurtenissen op toepassingsniveau Stream

Stream-niveau gebeurtenissen worden gegenereerd per stream of de verbinding. Elke gebeurtenis is een `StreamId` parameter waarmee de verbinding of stream. Elke stroom of de verbinding heeft een of meer nummers van verschillende typen. Een verbinding van een coderingsprogramma kan bijvoorbeeld één audiotrack en vier video sporen te wissen. De stream-event-typen zijn:

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Media.LiveEventConnectionRejected | Codering van is verbinding geweigerd. |
| Microsoft.Media.LiveEventEncoderConnected | Coderingsprogramma maakt verbinding met live-gebeurtenis. |
| Microsoft.Media.LiveEventEncoderDisconnected | Coderingsprogramma wordt verbroken. |

Zie [Schema voorbeelden](#event-schema-examples) onderstaande.

### <a name="track-level-events"></a>Gebeurtenissen bijhouden op toepassingsniveau

Gebeurtenissen bijhouden op toepassingsniveau worden gegenereerd per spoor. De track event-typen zijn:

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Gegevenssegment van Media-server wordt geweigerd omdat het te laat is of een tijdstempel in de overlappende (tijdstempel van nieuwe gegevenssegment is kleiner dan de eindtijd van het vorige gegevenssegment). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Media-server ontvangt de eerste gegevenssegment voor elk nummer in de stroom of de verbinding. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Media-server detecteert audio en video-streams, is niet gesynchroniseerd. Als een waarschuwing gebruiken, omdat de gebruikerservaring wordt mogelijk niet beïnvloed. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Media-server detecteert dat een van de twee video stromen die afkomstig zijn van externe encoders zijn niet synchroon. Als een waarschuwing gebruiken, omdat de gebruikerservaring wordt mogelijk niet beïnvloed. |
| Microsoft.Media.LiveEventIngestHeartbeat | Elke 20 seconden voor elk nummer gepubliceerd bij het uitvoeren van live-gebeurtenis. Biedt statussamenvatting opnemen. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | Media-server detecteert onderbreking in de binnenkomende bijhouden. |

Zie [Schema voorbeelden](#event-schema-examples) onderstaande.

## <a name="event-schema-examples"></a>Gebeurtenis schema-voorbeelden

### <a name="jobstatechange"></a>JobStateChange

Het volgende voorbeeld ziet u het schema van de **JobStateChange** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| previousState | string | De status van de taak voor de gebeurtenis. |
| state | string | De nieuwe status van de taak wordt een melding in deze gebeurtenis. Bijvoorbeeld, "geplande: De taak is klaar om te beginnen"of" voltooid: De taak is voltooid'.|

De taakstatus kan waar een van de waarden zijn: *In de wachtrij geplaatst*, *gepland*, *verwerking*, *voltooid*, *fout*, *geannuleerd*, *Annuleren*

> [!NOTE]
> *In de wachtrij geplaatst* zal alleen aanwezig zijn in de **previousState** eigenschap, maar niet in de **status** eigenschap.

### <a name="jobscheduled-jobprocessing-jobcanceling"></a>JobScheduled, JobProcessing, JobCanceling

Voor elke niet-definitieve taak statuswijziging (zoals JobScheduled, JobProcessing, JobCanceling) lijkt het voorbeeldschema op het volgende:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobProcessing",
  "eventTime": "2018-10-12T16:12:18.0839935",
  "id": "a0a6efc8-f647-4fc2-be73-861fa25ba2db",
  "data": {
    "previousState": "Scheduled",
    "state": "Processing",
    "correlationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="jobfinished-jobcanceled-joberrored"></a>JobFinished, JobCanceled, JobErrored

Voor elke laatste taak statuswijziging (zoals JobFinished, JobCanceled, JobErrored) lijkt het voorbeeldschema op het volgende:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobFinished",
  "eventTime": "2018-10-12T16:25:56.4115495",
  "id": "9e07e83a-dd6e-466b-a62f-27521b216f2a",
  "data": {
    "outputs": [
      {
        "@odata.type": "#Microsoft.Media.JobOutputAsset",
        "assetName": "output-7640689F",
        "error": null,
        "label": "VideoAnalyzerPreset_0",
        "progress": 100,
        "state": "Finished"
      }
    ],
    "previousState": "Processing",
    "state": "Finished",
    "correlationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| Uitvoer | Matrix | Hiermee haalt u de taak uitvoer.|

### <a name="joboutputstatechange"></a>JobOutputStateChange

Het volgende voorbeeld ziet u het schema van de **JobOutputStateChange** gebeurtenis:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobOutputStateChange",
  "eventTime": "2018-10-12T16:25:56.0242854",
  "id": "dde85f46-b459-4775-b5c7-befe8e32cf90",
  "data": {
    "previousState": "Processing",
    "output": {
      "@odata.type": "#Microsoft.Media.JobOutputAsset",
      "assetName": "output-7640689F",
      "error": null,
      "label": "VideoAnalyzerPreset_0",
      "progress": 100,
      "state": "Finished"
    },
    "jobCorrelationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="joboutputscheduled-joboutputprocessing-joboutputfinished-joboutputcanceling-joboutputcanceled-joboutputerrored"></a>JobOutputScheduled, JobOutputProcessing, JobOutputFinished, JobOutputCanceling, JobOutputCanceled, JobOutputErrored

Voor elke wijziging van de status JobOutput lijkt het voorbeeldschema op het volgende:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobOutputProcessing",
  "eventTime": "2018-10-12T16:12:18.0061141",
  "id": "f1fd5338-1b6c-4e31-83c9-cd7c88d2aedb",
  "data": {
    "previousState": "Scheduled",
    "output": {
      "@odata.type": "#Microsoft.Media.JobOutputAsset",
      "assetName": "output-7640689F",
      "error": null,
      "label": "VideoAnalyzerPreset_0",
      "progress": 0,
      "state": "Processing"
    },
    "jobCorrelationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```
### <a name="joboutputprogress"></a>JobOutputProgress

De voorbeeldschema ziet er ongeveer als volgt:

 ```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/belohGroup/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/job-5AB6DE32",
  "eventType": "Microsoft.Media.JobOutputProgress",
  "eventTime": "2018-12-10T18:20:12.1514867",
  "id": "00000000-0000-0000-0000-000000000000",
  "data": {
    "jobCorrelationData": {
      "TestKey1": "TestValue1",
      "testKey2": "testValue2"
    },
    "label": "VideoAnalyzerPreset_0",
    "progress": 86
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected

Het volgende voorbeeld ziet u het schema van de **LiveEventConnectionRejected** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventConnectionRejected",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "b303db59-d5c1-47eb-927a-3650875fded1",
    "data": { 
      "streamId":"Mystream1",
      "ingestUrl": "http://abc.ingest.isml",
      "encoderIp": "118.238.251.xxx",
      "encoderPort": 52859,
      "resultCode": "MPE_INGEST_CODEC_NOT_SUPPORTED"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| streamId | string | Id van de stroom of de verbinding. Coderingsprogramma of de klant is verantwoordelijk voor het toevoegen van deze ID in de URL voor opnemen. |  
| ingestUrl | string | Opname-URL opgegeven door de live-gebeurtenis. |  
| encoderIp | string | IP-adres van het coderingsprogramma. |
| encoderPort | string | Poort van het coderingsprogramma van waar deze stroom afkomstig is. |
| resultCode | string | De reden dat de verbinding is geweigerd. De resultaatcodes worden in de volgende tabel weergegeven. |

De resultaatcodes zijn:

| Resultaatcode | Description |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | Onjuiste URL voor opnemen |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | Coderingsprogramma IP niet aanwezig is in IP acceptatielijst geconfigureerd |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | Coderingsprogramma verzenden niet van metagegevens over de stroom. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Codec opgegeven wordt niet ondersteund. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED | Een fragment ontvangen vóór ontvangen en -header voor die stroom. |
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED | Number of qualities specified exceeds allowed max limit. |
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED | Samengevoegde bitrate overschrijdt de maximale toegestane limiet. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | De tijdstempel voor video- of audio FLVTag is ongeldig RTMP-codering. |

### <a name="liveeventencoderconnected"></a>LiveEventEncoderConnected

Het volgende voorbeeld ziet u het schema van de **LiveEventEncoderConnected** gebeurtenis: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderConnected",
    "eventTime": "2018-08-07T23:08:09.1710643",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| streamId | string | Id van de stroom of de verbinding. Coderingsprogramma of de klant is verantwoordelijk voor de levering van deze ID in de URL voor opnemen. |
| ingestUrl | string | Opname-URL opgegeven door de live-gebeurtenis. |
| encoderIp | string | IP-adres van het coderingsprogramma. |
| encoderPort | string | Poort van het coderingsprogramma van waar deze stroom afkomstig is. |

### <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDisconnected

Het volgende voorbeeld ziet u het schema van de **LiveEventEncoderDisconnected** gebeurtenis: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderDisconnected",
    "eventTime": "2018-08-07T23:08:09.1710872",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "resultCode": "S_OK"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| streamId | string | Id van de stroom of de verbinding. Coderingsprogramma of de klant is verantwoordelijk voor het toevoegen van deze ID in de URL voor opnemen. |  
| ingestUrl | string | Opname-URL opgegeven door de live-gebeurtenis. |  
| encoderIp | string | IP-adres van het coderingsprogramma. |
| encoderPort | string | Poort van het coderingsprogramma van waar deze stroom afkomstig is. |
| resultCode | string | De reden voor het coderingsprogramma verbreken. Mogelijk correcte verbinding verbreken of een fout. De resultaatcodes worden in de volgende tabel weergegeven. |

Het resultaat foutcodes zijn:

| Resultaatcode | Description |
| ----------- | ----------- |
| MPE_RTMP_SESSION_IDLE_TIMEOUT | RTMP-sessie is een time-out na een inactiviteit voor toegestane tijdslimiet. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | De tijdstempel voor video- of audio FLVTag is ongeldig RTMP-codering. |
| MPE_CAPACITY_LIMIT_REACHED | Coderingsprogramma te snel gegevens te verzenden. |
| Onbekende foutcodes | Deze foutcodes kunnen variëren van geheugenfout op dubbele vermeldingen in de hash-map. |

De correcte verbinding verbreken resultaatcodes zijn:

| Resultaatcode | Description |
| ----------- | ----------- |
| S_OK | Het coderingsprogramma is verbroken. |
| MPE_CLIENT_TERMINATED_SESSION | Coderingsprogramma verbroken (RTMP). |
| MPE_CLIENT_DISCONNECTED | Coderingsprogramma verbroken (FMP4). |
| MPI_REST_API_CHANNEL_RESET | De opdracht reset kanaal wordt ontvangen. |
| MPI_REST_API_CHANNEL_STOP | Kanaal opdracht voor stoppen ontvangen. |
| MPI_REST_API_CHANNEL_STOP | Het kanaal is momenteel onderhoudswerkzaamheden uitgevoerd. |
| MPI_STREAM_HIT_EOF | EOF-stream wordt verzonden door het coderingsprogramma. |

### <a name="liveeventincomingdatachunkdropped"></a>LiveEventIncomingDataChunkDropped

Het volgende voorbeeld ziet u het schema van de **LiveEventIncomingDataChunkDropped** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingDataChunkDropped",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "03da9c10-fde7-48e1-80d8-49936f2c3e7d",
    "data": { 
      "trackType": "Video",
      "trackName": "Video",
      "bitrate": 300000,
      "timestamp": 36656620000,
      "timescale": 10000000,
      "resultCode": "FragmentDrop_OverlapTimestamp"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| trackType | string | Type van het spoor (Audio / Video). |
| trackName | string | De naam van het spoor. |
| Bitrate | geheel getal | Bitrate van het spoor. |
| tijdstempel | string | Timestamp van het gegevenssegment is verwijderd. |
| tijdschaal | string | Tijdschaal van de tijdstempel. |
| resultCode | string | De reden van de gegevens chunk vervolgkeuzelijst. **FragmentDrop_OverlapTimestamp** or **FragmentDrop_NonIncreasingTimestamp**. |

### <a name="liveeventincomingstreamreceived"></a>LiveEventIncomingStreamReceived

Het volgende voorbeeld ziet u het schema van de **LiveEventIncomingStreamReceived** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamReceived",
    "eventTime": "2018-08-07T23:08:10.5069288Z",
    "id": "7f939a08-320c-47e7-8250-43dcfc04ab4d",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml/Streams(15864-stream0)15864-stream0",
      "trackType": "video",
      "trackName": "video",
      "bitrate": 2962000,
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "timestamp": "15336831655032322",
      "duration": "20000000",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| trackType | string | Type van het spoor (Audio / Video). |
| trackName | string | Naam van het nummer (beide opgegeven door het coderingsprogramma of, in het geval van RTMP, genereert *TrackType_Bitrate* indeling). |
| Bitrate | geheel getal | Bitrate van het spoor. |
| ingestUrl | string | Opname-URL opgegeven door de live-gebeurtenis. |
| encoderIp | string  | IP-adres van het coderingsprogramma. |
| encoderPort | string | Poort van het coderingsprogramma van waar deze stroom afkomstig is. |
| tijdstempel | string | Eerste timestamp van het gegevenssegment dat is ontvangen. |
| tijdschaal | string | Tijdschaal waarin tijdstempel wordt weergegeven. |

### <a name="liveeventincomingstreamsoutofsync"></a>LiveEventIncomingStreamsOutOfSync

Het volgende voorbeeld ziet u het schema van de **LiveEventIncomingStreamsOutOfSync** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamsOutOfSync",
    "eventTime": "2018-08-10T02:26:20.6269183Z",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "minLastTimestamp": "319996",
      "typeOfStreamWithMinLastTimestamp": "Audio",
      "maxLastTimestamp": "366000",
      "typeOfStreamWithMaxLastTimestamp": "Video",
      "timescaleOfMinLastTimestamp": "10000000", 
      "timescaleOfMaxLastTimestamp": "10000000"       
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| minLastTimestamp | string | Minimum van laatste tijdstempels tussen alle sporen (audio of video). |
| typeOfTrackWithMinLastTimestamp | string | Het type van het spoor met minimale laatste tijdstempel (audio of video). |
| maxLastTimestamp | string | Maximum van alle tijdstempels tussen alle sporen (audio of video). |
| typeOfTrackWithMaxLastTimestamp | string | Het type van het spoor met maximale laatste tijdstempel (audio of video). |
| timescaleOfMinLastTimestamp| string | Hiermee haalt u de tijdschaal waarin 'MinLastTimestamp' wordt weergegeven.|
| timescaleOfMaxLastTimestamp| string | Hiermee haalt u de tijdschaal waarin 'MaxLastTimestamp' wordt weergegeven.|

### <a name="liveeventincomingvideostreamsoutofsync"></a>LiveEventIncomingVideoStreamsOutOfSync

Het volgende voorbeeld ziet u het schema van de **LiveEventIncomingVideoStreamsOutOfSync** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/LiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "6dd4d862-d442-40a0-b9f3-fc14bcf6d750",
    "data": {
      "firstTimestamp": "2162058216",
      "firstDuration": "2000",
      "secondTimestamp": "2162057216",
      "secondDuration": "2000",
      "timescale": "10000000"      
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| firstTimestamp | string | Tijdstempel voor een van de sporen te wissen/kwaliteitsniveaus van het type video ontvangen. |
| FirstDuration | string | De duur van het gegevenssegment met eerste tijdstempel. |
| secondTimestamp | string  | Tijdstempel voor een ander nummer/kwaliteit-niveau van het type video ontvangen. |
| SecondDuration | string | De duur van het gegevenssegment met tweede tijdstempel. |
| tijdschaal | string | Tijdschaal van tijdstempels en de duur.|

### <a name="liveeventingestheartbeat"></a>LiveEventIngestHeartbeat

Het volgende voorbeeld ziet u het schema van de **LiveEventIngestHeartbeat** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIngestHeartbeat",
    "eventTime": "2018-08-07T23:17:57.4610506",
    "id": "7f450938-491f-41e1-b06f-c6cd3965d786",
    "data": {
      "trackType": "audio",
      "trackName": "audio",
      "bitrate": 160000,
      "incomingBitrate": 155903,
      "lastTimestamp": "15336837535253637",
      "timescale": "10000000",
      "overlapCount": 0,
      "discontinuityCount": 0,
      "nonincreasingCount": 0,
      "unexpectedBitrate": false,
      "state": "Running",
      "healthy": true
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| trackType | string | Type van het spoor (Audio / Video). |
| trackName | string | Naam van het nummer (beide opgegeven door het coderingsprogramma of, in het geval van RTMP, genereert *TrackType_Bitrate* indeling). |
| Bitrate | geheel getal | Bitrate van het spoor. |
| IncomingBitrate | geheel getal | Berekende bitrate op basis van die afkomstig zijn van het coderingsprogramma gegevenssegmenten. |
| lastTimestamp | string | Meest recente tijdstempel ontvangen voor een nummer in de afgelopen 20 seconden. |
| tijdschaal | string | Tijdschaal waarin tijdstempels worden uitgedrukt. |
| overlapCount | geheel getal | Aantal gegevenssegmenten had tijdstempels in de afgelopen 20 seconden overlapt. |
| discontinuityCount | geheel getal | Het aantal wijzigingen in de afgelopen 20 seconden waargenomen. |
| nonIncreasingCount | geheel getal | Aantal gegevenssegmenten met tijdstempels in het verleden zijn ontvangen in de afgelopen 20 seconden. |
| unexpectedBitrate | bool | Als de verwachte en de daadwerkelijke bitsnelheden verschillen door meer dan de toegestane limiet afgelopen 20 seconden. Dit is waar als en alleen als, incomingBitrate > = 2 * bitrate of incomingBitrate < = bitrate/2 of IncomingBitrate = 0. |
| state | string | Status van de live-gebeurtenis. |
| in orde | bool | Geeft aan of opnemen in orde is gebaseerd op het aantal en de vlaggen. Goed is ingesteld op true als overlapCount = 0 & & discontinuityCount = 0 & & nonIncreasingCount = 0 & & unexpectedBitrate = false. |

### <a name="liveeventtrackdiscontinuitydetected"></a>LiveEventTrackDiscontinuityDetected

Het volgende voorbeeld ziet u het schema van de **LiveEventTrackDiscontinuityDetected** gebeurtenis: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventTrackDiscontinuityDetected",
    "eventTime": "2018-08-07T23:18:06.1270405Z",
    "id": "5f4c510d-5be7-4bef-baf0-64b828be9c9b",
    "data": {
      "trackName": "video",
      "previousTimestamp": "15336837615032322",
      "trackType": "video",
      "bitrate": 2962000,
      "newTimestamp": "15336837619774273",
      "discontinuityGap": "575284",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| trackType | string | Type van het spoor (Audio / Video). |
| trackName | string | Naam van het nummer (beide opgegeven door het coderingsprogramma of, in het geval van RTMP, genereert *TrackType_Bitrate* indeling). |
| Bitrate | geheel getal | Bitrate van het spoor. |
| previousTimestamp | string | Timestamp van het vorige fragment. |
| NewTimestamp | string | Tijdstempel van de huidige fragment. |
| discontinuityGap | string | De kloof tussen boven twee tijdstempels. |
| tijdschaal | string | Tijdschaal in welke tussenruimte zowel tijdstempel en onderbreking worden weergegeven. |

### <a name="common-event-properties"></a>Algemene eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| onderwerp | string | Het onderwerp EventGrid. Deze eigenschap heeft de resource-ID voor de Media Services-account. |
| onderwerp | string | Het bronpad voor het kanaal in Media Services onder het Media Services-account. Samenvoegen van het onderwerp en het onderwerp kunt u de resource-ID voor de taak. |
| eventType | string | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. Bijvoorbeeld: 'Microsoft.Media.JobStateChange'. |
| eventTime | string | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | string | De unieke id voor de gebeurtenis. |
| gegevens | object | Gebeurtenisgegevens voor Media Services. |
| dataVersion | string | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | string | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

## <a name="next-steps"></a>Volgende stappen

[Meld u aan voor de taak statuswijzigingsgebeurtenissen](job-state-events-cli-how-to.md)

## <a name="see-also"></a>Zie ook

- [EventGrid .NET-SDK met Media Service-gebeurtenissen](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/)
- [Definities van Media Services-gebeurtenissen](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/eventgrid/data-plane/Microsoft.Media/stable/2018-01-01/MediaServices.json)
