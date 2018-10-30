---
title: De connectiviteit en telemetriegegevens verzamelt apparaat met digitale dubbels Azure | Microsoft Docs
description: Overzicht van de onboarding van een apparaat met digitale dubbels van Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 7eddea7e0d57b89318232da6f086bbe2f649ee77
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211924"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Apparaatconnectiviteit en inkomende telemetriegegevens

De telemetriegegevens die zijn verzonden door apparaten en sensoren vormen de ruggengraat van een IoT-oplossing. Hierdoor is chief probleem in de ontwikkeling van IoT-Apps die deze verschillende resources en het beheer binnen de context van een locatie. Azure van digitale dubbels vereenvoudigt het ontwikkelen van IoT-oplossingen door samenvoeging van apparaten en sensoren met een ruimtelijke intelligence-grafiek.

Als u wilt beginnen, moet een IoT Hub-resources worden gemaakt in de hoofdmap van de ruimtelijke grafiek, zodat alle apparaten onder de hoofdmap ruimte om berichten te verzenden. Zodra de IoT-Hub is gemaakt en apparaten met sensoren zijn geregistreerd in de digitale Twins-instantie, de apparaten kunnen beginnen met het verzenden van gegevens aan een digitale dubbels service de [Azure IoT Device SDK](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks#azure-iot-device-sdks).

Een stapsgewijze handleiding voor apparaten toevoegen kunt u vinden in de [zelfstudie om te implementeren en configureren van digitale dubbels](tutorial-facilities-setup.md). In een oogopslag zijn de stappen:

- Implementeer een exemplaar van Azure digitale dubbels uit de [Azure-portal](https://portal.azure.com)
- Opslagruimten maken in de grafiek
- De resource van een IoT-Hub maken en deze toewijzen aan een spatie in de grafiek
- Apparaten en sensoren in uw grafiek maken en deze toewijzen aan de spaties in de bovenstaande stappen hebt gemaakt
- Maken van een matcher om te filteren op basis van voorwaarden berichten over telemetrie
- Maak een [de gebruiker gedefinieerde functie](concepts-user-defined-functions.md) en deze toewijzen aan een spatie in de grafiek voor aangepaste verwerking van uw berichten over telemetrie
- Een rol toewijzen aan de gebruiker gedefinieerde functie voor toegang tot de grafiekgegevens toestaan
- De IoT Hub-verbindingsreeks ophalen uit de digitale dubbels Management API 's
- De verbindingsreeks van het apparaat op het apparaat met de Azure IoT Device-SDK configureren

Hierna leert u over het verkrijgen van de IoT Hub-verbindingsreeks van de API voor beheer van digitale dubbels en het gebruik van de IoT Hub telemetrie-berichtindeling om op basis van een sensor telemetrie te verzenden. Digitale dubbels is vereist voor elk stukje telemetrie die wordt ontvangen om te worden gekoppeld aan een sensor binnen de ruimtelijke grafiek om te controleren of de gegevens wordt verwerkt en gerouteerd binnen het juiste ruimtelijke context.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>De IoT Hub-verbindingsreeks ophalen uit de beheer-API

Voer een GET-aanroep van de apparaat-API met een `includes=ConnectionString` parameter om op te halen van de verbindingsreeks van de IoT Hub-apparaat. U kunt filteren op apparaat GUID of hardware-ID te vinden van het opgegeven apparaat:

```plaintext
https://yourManagementApiUrl/api/v1.0/devices/yourDeviceGuid?includes=ConnectionString
```

```plaintext
https://yourManagementApiUrl/api/v1.0/devices?hardwareIds=yourDeviceHardwareId&includes=ConnectionString
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *yourManagementApiUrl* | De volledige URL-pad voor uw API Management |
| *yourDeviceGuid* | De apparaat-ID |
| *yourDeviceHardwareId* | De hardware-ID van het apparaat |

Kopieer in de nettolading van de reactie van het apparaat *connectionString* eigenschap, die u bij het aanroepen van de Azure IoT Device SDK gebruikt om gegevens te verzenden naar Azure digitale dubbels.

## <a name="device-to-cloud-message"></a>Apparaat naar cloudbericht

U kunt aanpassen van uw apparaat berichtindeling en -nettolading aanpassen aan de behoeften van uw oplossing. U kunt een gegevensovereenkomst die kan worden geserialiseerd in een bytematrix of een stroom die wordt ondersteund door de [bericht (byteArray byte []) van Azure IoT Device Client Message klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___). Het bericht is een aangepaste binaire indeling van uw keuze, zolang u de opdracht gegevens in een bijbehorende door de gebruiker gedefinieerde functie decoderen. De enige vereiste voor een apparaat-naar-Cloud-bericht is voor het onderhouden van een set eigenschappen (Zie hieronder) om te controleren of dat uw bericht op de juiste wijze wordt doorgestuurd naar de engine voor gebeurtenisverwerking.

### <a name="telemetry-properties"></a>Telemetrie-eigenschappen

Terwijl de nettolading van de inhoud van een **bericht** mag willekeurige gegevens van 256 kB groot, er zijn enkele vereisten op verwacht [ `Message.Properties` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet). De onderstaande stappen uit weerspiegelen de vereiste en optionele eigenschappen die worden ondersteund door het systeem:

| De naam van eigenschap | Waarde | Vereist | Beschrijving |
|---|---|---|---|
| *DigitalTwins-telemetrie* | 1.0 | Ja | Een constante waarde die een op het systeem bericht |
| *DigitalTwins SensorHardwareId* | `string(72)` | Ja | Een unieke id van het verzenden van de sensor de **bericht**. Deze waarde moet overeenkomen met een object **HardwareId** eigenschap voor het systeem te verwerken. Bijvoorbeeld: `00FF0643BE88-CO2` |
| *CreationTimeUtc* | `string` | Nee | Een [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) identificeren van de tijd steekproef nemen van de nettolading van de tekenreeks met datum opgemaakt. Bijvoorbeeld: `2018-09-20T07:35:00.8587882-07:00` |
| *correlationId* | `string` | Nee | Een UUID die kan worden gebruikt om traceringsgebeurtenissen in het systeem. Bijvoorbeeld: `cec16751-ab27-405d-8fe6-c68e1412ce1f`

### <a name="sending-your-message-to-digital-twins"></a>Uw bericht verzenden naar digitale dubbels

Gebruik de DeviceClient [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) of [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) aanroep van uw bericht te verzenden naar uw digitale Twins-service.

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over de gegevensverwerking en de mogelijkheden van de door de gebruiker gedefinieerde functies Azure digitale dubbels [Azure digitale dubbele gegevens verwerken en de gebruiker gedefinieerde functies](concepts-user-defined-functions.md).

