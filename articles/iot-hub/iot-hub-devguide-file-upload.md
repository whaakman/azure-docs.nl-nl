---
title: Informatie over Azure IoT Hub-bestand uploaden | Microsoft Docs
description: Handleiding voor ontwikkelaars - gebruik de functionaliteit voor het uploaden van IoT-Hub voor het beheren van uploaden van vanaf een apparaat naar een Azure storage blob-container bestanden.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 8fee8dd727623e81140656a070e6855547693154
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451151"
---
# <a name="upload-files-with-iot-hub"></a>Bestanden uploaden met IoT Hub

Zoals beschreven in de [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md) artikel, een apparaat kunt uploaden van een bestand opnieuw door te sturen van een melding via een apparaat gerichte-eindpunt (**/devices/ {apparaat-id} / bestanden**). Wanneer een apparaat aan IoT-Hub gecommuniceerd dat het uploaden voltooid is, IoT-Hub verzendt een melding van bestand uploaden via de **/messages/servicebound/filenotifications** gerichte service-eindpunt.

In plaats van berichten via IoT Hub zelf uitgeven van de fungeert IoT Hub in plaats daarvan als een functie voor berichtverzending naar een gekoppeld Azure Storage-account. Een apparaat vraagt een token van het opslagaccount van IoT-Hub die specifiek is voor het bestand dat het apparaat wenst te uploaden. Het apparaat de SAS-URI gebruikt om het bestand uploaden naar opslag, en wanneer het uploaden voltooid is het apparaat een melding van voltooiing naar IoT Hub verzendt. IoT Hub controleert of het bestand te uploaden is voltooid en een bericht over een upload vervolgens toegevoegd aan het meldingseindpunt service gerichte-bestand.

Voordat u een bestand naar IoT Hub vanaf een apparaat uploaden, moet u uw hub door configureren [koppelen van een Azure Storage](iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub) account toe.

Uw apparaat kunt vervolgens [initialiseren uploaden](iot-hub-devguide-file-upload.md#initialize-a-file-upload) en vervolgens [kennis van IoT-hub](iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload) wanneer het uploaden is voltooid. (Optioneel) wanneer een apparaat aan IoT-Hub gecommuniceerd dat het uploaden voltooid is, de service kan genereren een [Meldingsbericht](iot-hub-devguide-file-upload.md#file-upload-notifications).

### <a name="when-to-use"></a>Wanneer gebruikt u dit?

Uploaden van het bestand gebruiken voor het verzenden van media-bestanden en grote telemetrie batches geüpload door tijdelijk verbonden apparaten of gecomprimeerd om bandbreedte te sparen.

Raadpleeg [richtlijnen voor communicatie van apparaat-naar-cloud](iot-hub-devguide-d2c-guidance.md) als bij twijfel tussen het gebruik van de gerapporteerde eigenschappen, apparaat-naar-cloud-berichten of bestand uploaden.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Een Azure Storage-account met IoT Hub koppelen

Voor het gebruik van de functionaliteit voor het uploaden van bestand, moet u eerst een Azure Storage-account koppelen aan de IoT-Hub. U kunt deze taak uitvoeren via de [Azure-portal](https://portal.azure.com), of programmatisch via de [IoT-Hub resourceprovider REST-API's](/rest/api/iothub/iothubresource). Nadat u hebt een Azure Storage-account gekoppeld aan uw IoT-Hub, stuurt de service een SAS-URI naar een apparaat, wanneer een bestand uploaden-aanvraag indient voor het apparaat.

> [!NOTE]
> De [Azure IoT SDK's](iot-hub-devguide-sdks.md) verwerkt automatisch bij het ophalen van de SAS-URI, het bestand te uploaden en IoT-Hub van een voltooide upload de hoogte te brengen.


## <a name="initialize-a-file-upload"></a>Initialiseren van een bestand is geüpload
IoT-Hub heeft een eindpunt specifiek voor apparaten om aan te vragen van een SAS-URI voor de opslag om een bestand te uploaden. Voor het starten van het proces voor het uploaden van bestand, het apparaat verzendt een POST-aanvraag voor `{iot hub}.azure-devices.net/devices/{deviceId}/files` met de volgende JSON-hoofdtekst:

```json
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT-Hub geeft de volgende gegevens, die het apparaat maakt gebruik van het bestand te uploaden:

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
> Deze sectie beschrijft afgeschafte functionaliteit voor het ontvangen van een SAS-URI van IoT Hub. Gebruik de POST-methode die eerder zijn beschreven.

IoT-Hub heeft twee REST-eindpunten voor de ondersteuning van bestand uploaden, een om op te halen van de SAS-URI voor de opslag en de andere aan op de hoogte stellen van een voltooide upload de IoT-hub. Het apparaat start het proces voor het uploaden van bestand door een GET te sturen naar de IoT-hub op `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Geretourneerd door de IoT-hub:

* Een SAS-URI specifiek zijn voor het bestand moet worden geüpload.

* Een correlatie-ID moet worden gebruikt nadat het uploaden is voltooid.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Kennis van IoT Hub een voltooide bestand is geüpload

Het apparaat is verantwoordelijk voor het bestand te uploaden naar storage met behulp van de Azure Storage-SDK's. Wanneer het uploaden voltooid is, het apparaat verzendt een POST-aanvraag voor `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` met de volgende JSON-hoofdtekst:

```json
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

De waarde van `isSuccess` is een Booleaanse waarde die aangeeft of het bestand is geüpload. De statuscode voor `statusCode` is de status voor het uploaden van het bestand naar de opslag, en de `statusDescription` komt overeen met de `statusCode`.

## <a name="reference-topics"></a>Onderwerpen met naslaginformatie:

De volgende onderwerpen met naslaginformatie bieden u meer informatie over het uploaden van bestanden vanaf een apparaat.

## <a name="file-upload-notifications"></a>Bestand uploaden meldingen

Wanneer een apparaat aan IoT-Hub gecommuniceerd dat het uploaden voltooid is, genereert IoT Hub (optioneel) een bericht op dat de locatie van de naam en de opslag van het bestand bevat.

Zoals uitgelegd in [eindpunten](iot-hub-devguide-endpoints.md), IoT-Hub biedt een bestand uploaden meldingen via een gerichte service-eindpunt (**/messages/servicebound/fileuploadnotifications**) als berichten. De semantiek ontvangen voor het bestand uploaden meldingen zijn dezelfde als die voor cloud-naar-apparaat-berichten en hebben hetzelfde [bericht lifecycle](iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle). Elk bericht dat wordt opgehaald uit het bestand uploaden-meldingseindpunt is een JSON-record met de volgende eigenschappen:

| Eigenschap | Beschrijving |
| --- | --- |
| EnqueuedTimeUtc |De tijdstempel die aangeeft wanneer de melding is gemaakt. |
| DeviceId |**Apparaat-id** van het apparaat waarop het bestand wordt geüpload. |
| BlobUri |De URI van het geüploade bestand. |
| BlobName |De naam van het geüploade bestand. |
| lastUpdatedTime |De tijdstempel die aangeeft wanneer het bestand voor het laatst is bijgewerkt. |
| BlobSizeInBytes |Grootte van het geüploade bestand. |

**Voorbeeld**. In dit voorbeeld ziet de hoofdtekst van een bestand uploaden een melding.

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

## <a name="file-upload-notification-configuration-options"></a>Bestand uploaden melding configuratie-opties

Elke IoT hub bevat de volgende configuratieopties voor bestandsmeldingen uploaden:

| Eigenschap | Beschrijving | Bereik en de standaard |
| --- | --- | --- |
| **enableFileUploadNotifications** |Hiermee bepaalt u of bestand uploaden meldingen worden geschreven naar het bestand meldingen-eindpunt. |BOOL. Standaard: True. |
| **fileNotifications.ttlAsIso8601** |Standaard-TTL voor bestandsmeldingen uploaden. |ISO_8601 interval tot 48 uur (minimaal 1 minuut). Standaard: 1 uur. |
| **fileNotifications.lockDuration** |De vergrendelingsduur van de voor de wachtrij met bestand uploaden meldingen. |5 tot en met 300 seconden (minimaal 5 seconden). Standaard: 60 seconden. |
| **fileNotifications.maxDeliveryCount** |Maximumaantal leveringen voor het bestand uploaden meldingswachtrij. |1 tot en met 100. Standaard: 100. |

## <a name="additional-reference-material"></a>Meer referentiemateriaal dat beschikbaar is

Er zijn andere onderwerpen met naslaginformatie in de IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md) beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.

* [Bandbreedtebeperking en quota's](iot-hub-devguide-quotas-throttling.md) beschrijving van de quota en beperkingen van problemen die betrekking hebben op de IoT Hub-service.

* [Azure IoT-apparaat en service-SDK's](iot-hub-devguide-sdks.md) geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apparaat- en service-apps die communiceren met IoT Hub-taal.

* [IoT Hub-querytaal](iot-hub-devguide-query-language.md) beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaatdubbels en taken.

* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md) vindt u meer informatie over IoT Hub-ondersteuning voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd over het uploaden van bestanden vanaf apparaten met behulp van IoT Hub, kunt u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Apparaat-id's in IoT Hub beheren](iot-hub-devguide-identity-registry.md)

* [Toegang tot IoT Hub regelen](iot-hub-devguide-security.md)

* [Apparaatdubbels gebruiken voor het synchroniseren van de status en -configuraties](iot-hub-devguide-device-twins.md)

* [Een rechtstreekse methode aanroepen op een apparaat](iot-hub-devguide-direct-methods.md)

* [Taken op meerdere apparaten plannen](iot-hub-devguide-jobs.md)

Als u wilt uitproberen enkele concepten die worden beschreven in dit artikel, raadpleegt u de volgende zelfstudie voor IoT-Hub:

* [Over het uploaden van bestanden vanaf apparaten naar de cloud met IoT Hub](iot-hub-csharp-csharp-file-upload.md)