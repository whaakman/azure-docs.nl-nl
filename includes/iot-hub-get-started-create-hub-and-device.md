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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724902"
---
## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Nu u een IoT-hub hebt gemaakt, kunt u de belangrijke informatie zoeken die u gebruikt om apparaten en toepassingen te verbinden met uw IoT-hub. 

Open in uw IoT hub-navigatiemenu **beleid voor gedeelde toegang**.
Selecteer de **iothubowner** beleid en kopieer de **Connection string---primaire sleutel** van uw IoT-hub. Zie voor meer informatie [Toegang tot IoT Hub regelen](../articles/iot-hub/iot-hub-devguide-security.md).

   > [!NOTE] 
   > U hoeft deze iothubowner-verbindingsreeks niet voor deze zelfstudie. Echter, moet u mogelijk het voor sommige van de zelfstudies of andere IoT-scenario's na het voltooien van deze configuratie.

   ![Uw IoT-hub-verbindingsreeks ophalen](./media/iot-hub-get-started-create-hub-and-device/create-iot-hub5.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Een apparaat in de IoT-hub voor uw apparaat registreren

1. Open in uw IoT hub-navigatiemenu **IoT-apparaten**, klikt u vervolgens op **toevoegen** voor het registreren van een apparaat in uw IoT-hub.

   ![Een apparaat in de IoT-apparaten van uw IoT-hub toevoegen](./media/iot-hub-get-started-create-hub-and-device/create-identity-portal.png)

2. Voer een **apparaat-ID** voor het nieuwe apparaat. Apparaat-id's zijn hoofdlettergevoelig.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klik op **Opslaan**.
5. Nadat het apparaat is gemaakt, opent u het apparaat uit de lijst in de **IoT-apparaten** deelvenster.
6. Kopieer de **Connection string---primaire sleutel** voor later gebruik.

   ![De verbindingsreeks ophalen](./media/iot-hub-get-started-create-hub-and-device/device-connection-string.png)
