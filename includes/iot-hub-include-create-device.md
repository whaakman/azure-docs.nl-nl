---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d70544866b9e321d98acd3978da145276e036025
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58671706"
---
<!-- put the ## header in the file that includes this file -->

In deze sectie maakt u een apparaat-id in het id-register in uw IoT-hub. Een apparaat kan geen verbinding maken met de IoT-hub, tenzij het vermeld staat in het id-register. Zie voor meer informatie de sectie 'id-register van de [Ontwikkelaarshandleiding voor IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md) 

1. Open in uw IoT hub-navigatiemenu **IoT-apparaten**en selecteer vervolgens **toevoegen** een nieuw apparaat registreren in uw IoT-hub.

    ![Apparaat-id in de portal maken](./media/iot-hub-include-create-device/create-identity-portal.png)

1. Geef een naam voor het nieuwe apparaat, zoals **myDeviceId**, en selecteer **opslaan**. Deze actie wordt een nieuwe apparaat-id voor uw IoT-hub gemaakt.

   ![Een nieuw apparaat toevoegen](./media/iot-hub-include-create-device/create-a-device.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]


1. Nadat het apparaat is gemaakt, opent u het apparaat uit de lijst in de **IoT-apparaten** deelvenster. Kopieer de **Connection string---primaire sleutel** voor later gebruik.

    ![Apparaat-verbindingsreeks](./media/iot-hub-include-create-device/device-details.png)

> [!NOTE]
> In het id-register van IoT Hub worden alleen apparaat-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. De apparaat-id’s en sleutels worden opgeslagen en gebruikt als beveiligingsreferenties. Met de vlag voor ingeschakeld/uitgeschakeld kunt u toegang tot een afzonderlijk apparaat uitschakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Zie voor meer informatie, [Ontwikkelaarshandleiding voor IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
