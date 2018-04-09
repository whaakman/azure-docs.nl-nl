---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/05/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b08bfcd4cb9e85f9e682efe0f599b6dd88897962
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
## <a name="create-a-device-identity"></a>Een apparaat-id maken

In deze sectie gebruikt u de [Azure-portal] [ lnk-azure-portal] maken van een apparaat-id in het identiteitenregister van uw IoT-hub. Een apparaat kan geen verbinding maken met de IoT-hub, tenzij het vermeld staat in het id-register. Zie de sectie Id-register in de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie. Gebruik de **IoT-apparaten** deelvenster in de portal voor het genereren van een unieke apparaat-ID en sleutel voor het apparaat moet worden gebruikt om zichzelf te identificeren met IoT Hub. Apparaat-id's zijn hoofdlettergevoelig.

1. Zorg ervoor dat u bent aangemeld bij de [Azure-portal][lnk-azure-portal].

1. Klik in de Snelbalk op **alle resources** en uw IoT hub bron vinden.

    ![Navigeer naar uw Iot-hub][img-find-iothub]

1. Wanneer uw IoT hub resource wordt geopend, klikt u op de **IoT-apparaten** hulpprogramma en klik vervolgens op **toevoegen** aan de bovenkant. Geef de naam voor het nieuwe apparaat, zoals **myDeviceId**, en klik op **opslaan**.

    ![Apparaat-id in de portal maken][img-create-device]

   Deze actie wordt een nieuw apparaat-id voor uw IoT-hub gemaakt.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. In de **IoT-apparaten**van lijst met apparaten, klikt u op het nieuwe apparaat en noteer de **verbindingsreeks---primaire sleutel**.

    ![Apparaat-verbindingsreeks][img-connection-string]

> [!NOTE]
> In het id-register van IoT Hub worden alleen apparaat-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. De apparaat-id’s en sleutels worden opgeslagen en gebruikt als beveiligingsreferenties. Met de vlag voor ingeschakeld/uitgeschakeld kunt u toegang tot een afzonderlijk apparaat uitschakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Zie de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie.

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

