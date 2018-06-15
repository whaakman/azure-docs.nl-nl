---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: f8cd78e63099f864c5fc54b6268f6e558d738626
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371368"
---
## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Nu dat u een IoT-hub hebt gemaakt, zoek de belangrijke informatie waarmee u verbinding maken met apparaten en toepassingen naar uw IoT-hub. 

Open in uw IoT hub-navigatiemenu **gedeeld toegangsbeleid**.
Selecteer de **iothubowner** beleid en kopieer de **verbindingsreeks---primaire sleutel** van uw iothub. Zie voor meer informatie [Toegang tot IoT Hub regelen](../articles/iot-hub/iot-hub-devguide-security.md).

   > [!NOTE] 
   > U hoeft deze verbindingsreeks iothubowner niet voor deze zelfstudie installeren. Echter, moet u mogelijk het voor een aantal van de zelfstudies of andere IoT-scenario's na het voltooien van deze configuratie.

   ![Uw IoT-hub-verbindingsreeks ophalen](./media/iot-hub-get-started-create-hub-and-device/create-iot-hub5.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Een apparaat in de IoT-hub voor uw apparaat registreren

1. Open in uw IoT hub-navigatiemenu **IoT-apparaten**, klikt u vervolgens op **toevoegen** registreren van een apparaat in uw IoT-hub.

   ![Een apparaat toevoegt in de IoT-apparaten van uw IoT-hub](./media/iot-hub-get-started-create-hub-and-device/create-identity-portal.png)

2. Voer een **apparaat-ID** voor het nieuwe apparaat. Apparaat-id's zijn hoofdlettergevoelig.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klik op **Opslaan**.
5. Nadat het apparaat is gemaakt, opent u het apparaat uit de lijst in de **IoT-apparaten** deelvenster.
6. Kopieer de **verbindingsreeks---primaire sleutel** voor later gebruik.

   ![De verbindingsreeks ophalen](./media/iot-hub-get-started-create-hub-and-device/device-connection-string.png)
