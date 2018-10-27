---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 13eddced155eab6dedfbce77330e7a178ecfb3cb
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165568"
---
> [!div class="op_single_selector"]
> * [Apparaat: Node.js-Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Het apparaat: C# Service:C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Het apparaat: Java-Service: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Apparaat: Python-Service: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Back-end-apps kunnen gebruiken primitieven van Azure IoT Hub, zoals [apparaatdubbel] [ lnk-devtwin] en [directe methoden][lnk-c2dmethod], op afstand starten en bewaken van apparaat van de beheeracties op apparaten. In deze zelfstudie leert u hoe een back-end-app en een apparaat-app samenwerken kunnen om te starten en te bewaken van een extern apparaat opnieuw opstarten met behulp van IoT-Hub.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Gebruik een rechtstreekse methode apparaat management om acties te starten (zoals opnieuw opstarten, Fabrieksinstellingen terugzetten en firmware-update) vanuit een back-end-app in de cloud. Het apparaat is verantwoordelijk voor:

* Verwerken van de methode-aanvraag verzonden vanuit IoT Hub.
* De overeenkomstige apparaat-specifieke actie op het apparaat wordt gestart.
* Van statusupdates via bieden *gerapporteerde eigenschappen* naar IoT Hub.

U kunt een back-end-app in de cloud gebruiken om uit te voeren van de apparaatdubbel-query's om te rapporteren over de voortgang van de acties voor uw apparaat.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
