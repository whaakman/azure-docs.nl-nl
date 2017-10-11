---
title: Azure IoT Hub-bestand uploaden begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - gebruik de functie voor het laden van bestand van IoT Hub voor het beheren van uploaden van van een apparaat aan een Azure-opslag-blob-container bestanden.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a0427925-3e40-4fcd-96c1-2a31d1ddc14b
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 75a6b9bc3ecfe6d6901bb38e312d62333f38daf1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="upload-files-with-iot-hub"></a>Uploaden van bestanden met IoT Hub

Zoals beschreven in de [IoT-hubeindpunten] [ lnk-endpoints] artikel, een apparaat kunt starten uploaden van een bestand met het verzenden van een melding via een apparaat gerichte-eindpunt (**/devices/ {deviceId} /bestanden**). Wanneer een apparaat een melding van IoT Hub een upload is voltooid, IoT Hub verzendt een bericht op bestand uploaden via de **/messages/servicebound/filenotifications** gerichte service-eindpunt.

In plaats van het uitgeven van de berichten via IoT Hub zelf fungeert IoT-Hub in plaats daarvan als een dispatcher op een gekoppelde Azure Storage-account. Een apparaat vraagt een token voor de opslag van IoT-Hub die specifiek is voor het bestand dat het apparaat wenst te uploaden. Het apparaat gebruikmaakt van de SAS-URI van het bestand te uploaden naar de opslag en wanneer het uploaden voltooid is het apparaat een melding van voltooiing naar IoT Hub verzendt. IoT Hub controleert het bestand te uploaden is voltooid en vervolgens een melding van bestand uploaden toegevoegd aan het meldingseindpunt service gerichte-bestand.

Voordat u een bestand naar IoT Hub vanaf een apparaat uploadt, moet u uw hub door configureren [koppelen van een Azure Storage] [ lnk-associate-storage] account toe aan het.

Uw apparaat kunt vervolgens [initialiseren van een upload] [ lnk-initialize] en vervolgens [IoT-hub in kennis] [ lnk-notify] wanneer het uploaden is voltooid. Eventueel, wanneer een apparaat een melding van IoT Hub het uploaden is voltooid, de service kan genereren een [Meldingsbericht][lnk-service-notification].

### <a name="when-to-use"></a>Wanneer gebruikt u dit?

Uploaden bestand gebruiken voor het verzenden van media-bestanden en grote telemetrie batches geüpload door afwisselend verbonden apparaten of gecomprimeerd om bandbreedte besparen.

Raadpleeg [apparaat-naar-cloud communicatie richtlijnen] [ lnk-d2c-guidance] indien in waarover twijfel bestaat tussen het gebruik van de gerapporteerde eigenschappen, apparaat-naar-cloud-berichten of bestand uploaden.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Een Azure Storage-account koppelen met IoT Hub

Voor het gebruik van de functionaliteit voor het uploaden van bestanden, moet u eerst een Azure Storage-account koppelen aan de IoT-Hub. U kunt deze taak uitvoeren via de [Azure-portal][lnk-management-portal], of via een programma via de [resourceprovider IoT Hub REST-API's] [ lnk-resource-provider-apis]. Zodra u hebt een Azure Storage-account gekoppeld aan uw IoT-Hub, stuurt de service een SAS-URI op een apparaat wanneer het apparaat een aanvraag voor het uploaden van bestand initieert.

> [!NOTE]
> De [Azure IoT SDK's] [ lnk-sdks] automatisch bij het ophalen van de SAS-URI, het bestand uploadt en IoT-Hub van het uploaden van een voltooide verwittigen verwerken.


## <a name="initialize-a-file-upload"></a>Initialiseren van een bestand is geüpload
IoT Hub heeft een eindpunt specifiek voor apparaten om aan te vragen van een SAS-URI voor de opslag van een bestand te uploaden. Voor het initiëren van het uploadproces bestand, het apparaat verzendt een POST-aanvraag voor `{iot hub}.azure-devices.net/devices/{deviceId}/files` met de volgende JSON-hoofdtekst:

```json
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub retourneert de volgende gegevens, het apparaat gebruikmaakt van het bestand te uploaden:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Afgeschafte: het uploaden van een bestand met een GET initialiseren

> [!NOTE]
> Deze sectie beschrijft afgeschafte functionaliteit voor het ontvangen van een SAS-URI van IoT-Hub. Gebruik de POST-methode die eerder zijn beschreven.

IoT Hub heeft twee REST-eindpunten voor de ondersteuning van bestand is geüpload, een voor de SAS-URI voor opslag en de andere op de hoogte van de IoT-hub van het uploaden van een voltooide wilt ophalen. Het apparaat het uploadproces bestand door een GET te sturen naar de IoT-hub aan initieert `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. De IoT-hub als resultaat:

* Een SAS-URI specifiek zijn voor het bestand te uploaden.
* Een correlatie-ID moet worden gebruikt wanneer het uploaden is voltooid.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Melding van IoT Hub een voltooide bestand is geüpload

Het apparaat is verantwoordelijk voor het uploaden van het bestand naar de opslag met behulp van de Azure-opslag-SDK's. Wanneer het uploaden voltooid is, het apparaat verzendt een POST-aanvraag voor `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` met de volgende JSON-hoofdtekst:

```json
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

De waarde van `isSuccess` wordt een Boolean die aangeeft of het bestand is geüpload. De statuscode voor `statusCode` is de status voor het uploaden van het bestand naar de opslag, en de `statusDescription` komt overeen met de `statusCode`.

## <a name="reference-topics"></a>De onderwerpen waarnaar wordt verwezen:

De volgende onderwerpen met naslaginformatie bieden u meer informatie over het uploaden van bestanden van een apparaat.

## <a name="file-upload-notifications"></a>Bestand uploaden meldingen

Wanneer een apparaat een melding van IoT Hub een upload is voltooid, kunt IoT Hub eventueel genereren in een bericht op dat de locatie van de naam en de opslag van het bestand bevat.

Zoals uitgelegd in [eindpunten][lnk-endpoints], IoT Hub biedt bestand uploaden meldingen via een gerichte service-eindpunt (**/messages/servicebound/fileuploadnotifications**) Als berichten. De receive-semantiek voor het bestand uploaden meldingen zijn hetzelfde als voor cloud-naar-apparaat-berichten en hebben dezelfde [bericht lifecycle][lnk-lifecycle]. Elk bericht opgehaald uit het bestand uploaden-meldingseindpunt is een JSON-record met de volgende eigenschappen:

| Eigenschap | Beschrijving |
| --- | --- |
| EnqueuedTimeUtc |De tijdstempel die aangeeft wanneer de melding is gemaakt. |
| Apparaat-id |**DeviceId** van het apparaat waarop het bestand geüpload. |
| BlobUri |De URI van het geüploade bestand. |
| BlobName |Naam van het geüploade bestand. |
| LastUpdatedTime |De tijdstempel die aangeeft wanneer het bestand het laatst is bijgewerkt. |
| BlobSizeInBytes |Grootte van het geüploade bestand. |

**Voorbeeld**. Dit voorbeeld toont de hoofdtekst van een bestand uploaden een melding.

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Bestand uploaden meldingsopties configuratie

Elke IoT-hub toont de volgende configuratieopties voor bestand uploaden meldingen:

| Eigenschap | Beschrijving | Bereik en standaard |
| --- | --- | --- |
| **enableFileUploadNotifications** |Bepaalt of meldingen van bestand uploaden naar het eindpunt van de meldingen bestand worden geschreven. |BOOL. Standaard: True. |
| **fileNotifications.ttlAsIso8601** |Standaard-TTL voor meldingen van bestand uploaden. |ISO_8601 interval maximaal 48 uur (minimaal 1 minuut). Standaardwaarde: 1 uur. |
| **fileNotifications.lockDuration** |De duur van de vergrendeling voor de wachtrij bestand uploaden meldingen. |5-300 seconden (minimaal 5 seconden). Standaard: 60 seconden. |
| **fileNotifications.maxDeliveryCount** |Levering van het maximum aantal voor het bestand uploaden meldingswachtrij. |1 tot 100. Standaard: 100. |

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Er zijn andere onderwerpen waarnaar wordt verwezen in de IoT Hub developer guide:

* [IoT-hubeindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.
* [Bandbreedtebeperking en quota's] [ lnk-quotas] beschrijving van de quota en beperking gedrag die betrekking hebben op de service IoT Hub.
* [Azure IoT-SDKs voor apparaat en de service] [ lnk-sdks] geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apps voor het apparaat en de service die communiceren met IoT Hub-taal.
* [IoT Hub-querytaal] [ lnk-query] beschrijft de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat horende en taken.
* [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u het uploaden van bestanden van apparaten met behulp van IoT Hub hebt geleerd, kunt u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Apparaat-id's van IoT-Hub beheren][lnk-devguide-identities]
* [Toegang beheren met IoT Hub][lnk-devguide-security]
* [Horende apparaten gebruiken om de status en configuraties te synchroniseren][lnk-devguide-device-twins]
* [Een directe methode aangeroepen voor een apparaat][lnk-devguide-directmethods]
* [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u wilt uitproberen enkele concepten die in dit artikel wordt beschreven, kunt u mogelijk geïnteresseerd in de volgende IoT Hub-zelfstudie:

* [Het uploaden van bestanden van apparaten naar de cloud met IoT Hub][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
