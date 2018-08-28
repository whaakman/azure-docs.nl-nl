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
ms.openlocfilehash: 935e2b9e861a889bef48c1d7ba119ab694cddfb3
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094010"
---
## <a name="create-a-device-identity"></a>Een apparaat-id maken

In deze sectie maakt u de [Azure-portal](https://portal.azure.com) te maken van een apparaat-id in het identiteitenregister van uw IoT-hub. Een apparaat kan geen verbinding maken met de IoT-hub, tenzij het vermeld staat in het id-register. Zie voor meer informatie de sectie 'id-register van de [Ontwikkelaarshandleiding voor IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md) gebruiken de **IoT-apparaten** deelvenster in de portal voor het genereren van een unieke apparaat-ID en de sleutel voor uw apparaat gebruiken om te identificeren zelf aan IoT-Hub. Apparaat-id's zijn hoofdlettergevoelig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com)

1. Selecteer **alle resources** en zoek de resource van uw IoT hub.

1. Wanneer uw IoT hub-resource wordt geopend, klikt u op de **IoT-apparaten** hulpprogramma, en klik vervolgens op **toevoegen** aan de bovenkant. 

    ![Apparaat-id in de portal maken](./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png)

1. Geef een naam voor het nieuwe apparaat, zoals **myDeviceId**, en klikt u op **opslaan**. Deze actie wordt een nieuwe apparaat-id voor uw IoT-hub gemaakt.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Een nieuw apparaat toevoegen](./media/iot-hub-get-started-create-device-identity-portal/add-device.png)

1. In de lijst, klikt u op het zojuist gemaakte apparaat en kopieer de **Connection string---primaire sleutel** voor later gebruik.

    ![Apparaat-verbindingsreeks](./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png)

> [!NOTE]
> In het id-register van IoT Hub worden alleen apparaat-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. De apparaat-id’s en sleutels worden opgeslagen en gebruikt als beveiligingsreferenties. Met de vlag voor ingeschakeld/uitgeschakeld kunt u toegang tot een afzonderlijk apparaat uitschakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Zie voor meer informatie, [Ontwikkelaarshandleiding voor IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
