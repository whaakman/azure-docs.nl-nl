---
title: Communiceren met een apparaat-app in Node.js via IoT Hub apparaat-streams (preview) | Microsoft Docs
description: In deze quickstart voert u een Node.js-toepassing aan de servicezijde uit die via een apparaatstream communiceert met een IoT-apparaat.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 74a9e5e6be326bf9cef3dc6d26594b80491deb3e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446043"
---
# <a name="quickstart-communicate-to-a-device-application-in-nodejs-via-iot-hub-device-streams-preview"></a>Quickstart: Communiceren met een apparaattoepassing in Node.js via IoT Hub-apparaatstreams (preview)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](./iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. Gedurende de openbare preview biedt de Node.js SDK alleen ondersteuning voor apparaatstreams aan de servicezijde. Daarom bevat deze quickstart alleen instructies voor het uitvoeren van de toepassing aan de servicezijde. U moet een bijbehorende apparaat-side '-toepassing uitvoeren vanaf een van de volgende Quick starts:

* [Communiceren met de apparaat-apps in C via IoT Hub apparaat-streams](./quickstart-device-streams-echo-c.md)

* [Communiceren met apps op apparaten in C# via IoT Hub apparaat-streams](./quickstart-device-streams-echo-csharp.md).

De Node.js-toepassing aan de servicezijde in deze quickstart heeft de volgende functionaliteit:

* Maakt een apparaatstream naar een IoT-apparaat.

* Leest invoer vanaf de opdrachtregel en verzendt deze naar de apparaattoepassing, die de invoer op zijn beurt terugstuurt.

De code laat zien het proces voor het initiëren van een apparaat-stream, evenals hoe u kunt gebruiken om te verzenden en ontvangen van gegevens.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-Hubs die zijn gemaakt in de volgende regio's:

*  **US - centraal**

*  **VS-midden EUAP**

Voor het uitvoeren van de toepassing aan de serverkant in deze snelstartgids moet u Node.js v10.x.x of hoger op uw ontwikkelcomputer.

U kunt Node.js downloaden voor meerdere platforms van [Nodejs.org](https://nodejs.org).

Gebruik de volgende opdracht om de huidige versie van Node.js op uw ontwikkelcomputer te controleren:

```
node --version
```

Voer de volgende opdracht om toe te voegen van de Microsoft Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie worden IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS) specifieke opdrachten toegevoegd aan Azure CLI.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Als u dit nog niet hebt gedaan, downloadt u het voorbeeldproject met Node.js van https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip en pakt u het ZIP-archief uit.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Als u [Snelstart: Als u telemetrie vanaf een apparaat wilt verzenden naar een IoT-hub](quickstart-send-telemetry-node.md), kunt u deze stap overslaan.

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Als u [Snelstart: Als u telemetrie vanaf een apparaat wilt verzenden naar een IoT-hub](quickstart-send-telemetry-node.md), kunt u deze stap overslaan.

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Azure Cloud Shell te maken van de apparaat-id.

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyDevice**: dit is de naam van het geregistreerde apparaat. Gebruik MyDevice, zoals wordt weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. U hebt ook een *service-verbindingsreeks* nodig, zodat de back-end-toepassing verbinding kan maken met de IoT-hub en de berichten kan ophalen. Met de volgende opdracht haalt u de serviceverbindingsreeks voor uw IoT-hub op:

    **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Noteer de geretourneerde waarde, die er als volgt uitziet:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>Communicatie tussen apparaat en service via apparaatstreams

In deze sectie maakt u de apparaat-side '-toepassing en de toepassing aan de serverkant uitvoeren en communicatie tussen de twee.

### <a name="run-the-device-side-application"></a>De toepassing aan de apparaatzijde uitvoeren

Zoals eerder vermeld, biedt de IoT Hub Node.js SDK alleen ondersteuning voor apparaatstreams aan de servicezijde. Gebruik de bijbehorende apparaatprogramma beschikbaar in een van deze Quick starts voor apparaat-side '-toepassing:

   * [Communiceren met de apparaat-apps in C via IoT Hub apparaat-streams](./quickstart-device-streams-echo-c.md)

   * [Communiceren met apps op apparaten in C# via IoT Hub apparaat-streams](./quickstart-device-streams-echo-csharp.md)

Controleer of de toepassing aan apparaatzijde wordt uitgevoerd voordat u naar de volgende stap gaat.

### <a name="run-the-service-side-application"></a>De toepassing aan de servicezijde uitvoeren

Ervan uitgaande dat de toepassing aan de apparaatzijde wordt uitgevoerd, volgt u de volgende stappen voor het uitvoeren van de toepassing aan de serverzijde in Node.js:

* Geef de referenties van uw service en de apparaat-id op als omgevingsvariabelen.
 
   ```
   # In Linux
   export IOTHUB_CONNECTION_STRING="<provide_your_service_connection_string>"
   export STREAMING_TARGET_DEVICE="MyDevice"

   # In Windows
   SET IOTHUB_CONNECTION_STRING=<provide_your_service_connection_string>
   SET STREAMING_TARGET_DEVICE=MyDevice
   ```
  
   Wijzig `MyDevice` in de apparaat-id die u voor uw apparaat hebt gekozen.

* Navigeer naar `Quickstarts/device-streams-service` in de uitgepakte projectmap en voer het voorbeeld uit met behulp van het knooppunt.

   ```
   cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service
    
   # Install the preview service SDK, and other dependencies
   npm install azure-iothub@streams-preview
   npm install

   node echo.js
   ```

Aan het einde van de laatste stap initieert de toepassing aan de servicezijde een stream naar uw apparaat die vervolgens via de stroom een tekenreeksbuffer naar de service verzendt. In dit voorbeeld leest het programma aan de serverkant gewoon de `stdin` op de terminal en verzendt dit naar het apparaat en deze wordt vervolgens echo deze terug. Hiermee wordt een geslaagde bidirectionele communicatie tussen de twee toepassingen aangegeven.

![Aan de serverkant console-uitvoer](./media/quickstart-device-streams-echo-nodejs/service-console-output.png)

Vervolgens kunt u het programma beëindigen door nogmaals op Enter te drukken.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een IoT-hub ingesteld, een apparaat geregistreerd, een apparaatstroom tussen toepassingen aan de apparaat- en servicezijde tot stand gebracht en de stroom gebruikt om gegevens tussen de toepassingen heen en weer te verzenden.

Gebruik de onderstaande koppelingen voor meer informatie over apparaatstreams:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md) 