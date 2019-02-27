---
title: Met gateways - Azure IoT Edge modbus-protocollen vertalen | Microsoft Docs
description: Toestaan dat apparaten die Modbus TCP gebruiken, kunnen communiceren met Azure IoT Hub door een IoT-gatewayapparaat te maken
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 1c9855f982b888e8e1d68bfe5233983db8c826ad
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56873760"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Modbus TCP-apparaten verbinden via de gateway van een IoT Edge-apparaat

Als u IoT-apparaten die gebruikmaken van Modbus TCP- of RTU-protocollen wilt verbinden met een Azure IoT-hub, gebruik dan een IoT Edge-apparaat als een gateway. Het gatewayapparaat leest de gegevens van uw Modbus-apparaten en geeft deze gegevens met behulp van een ondersteund protocol door aan de cloud.

![Modbus-apparaten verbinden met IoT Hub via IoT Edge-gateway](./media/deploy-modbus-gateway/diagram.png)

Dit artikel gaat over hoe u uw eigen containerinstallatiekopie maakt voor een Modbus-module (u kunt ook een kant- en-klaar voorbeeld gebruiken) en deze vervolgens implementeert op het IoT Edge-apparaat dat als uw gateway zal fungeren.

In dit artikel wordt ervan uitgegaan dat u het Modbus-protocol TCP gebruikt. Zie voor meer informatie over hoe u de module configureert ter ondersteuning van Modbus RTU, de [Azure IoT Edge Modbus-module](https://github.com/Azure/iot-edge-modbus) project op GitHub.

## <a name="prerequisites"></a>Vereisten
* Een Azure IoT Edge-apparaat. Zie voor een overzicht over het instellen van een [Azure IoT-Edge implementeren op Windows](quickstart.md) of [Linux](quickstart-linux.md).
* De verbindingsreeks van de primaire sleutel voor het IoT Edge-apparaat.
* Een fysiek of gesimuleerd Modbus-apparaat dat Modbus TCP ondersteunt.

## <a name="prepare-a-modbus-container"></a>Een Modbus-container voorbereiden

Als u de functionaliteit van de Modbus-gateway wilt testen, heeft Microsoft een voorbeeldmodule die u kunt gebruiken. U kunt de module openen vanuit de Azure Marketplace, [Modbus](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview), of met de installatiekopie URI, **mcr.microsoft.com/azureiotedge/modbus:1.0**.

Als u wilt uw eigen module maken en aanpassen voor uw omgeving, er is een open-source [Azure IoT Edge Modbus-module](https://github.com/Azure/iot-edge-modbus) project op GitHub. Volg de instructies in dit project om uw eigen containerinstallatiekopie te maken. Als u uw eigen containerinstallatiekopie hebt gemaakt, raadpleegt u [ontwikkelen C# modules in Visual Studio](how-to-visual-studio-develop-csharp-module.md) of [modules in Visual Studio Code ontwikkelen](how-to-vs-code-develop-module.md). Deze artikelen bevatten instructies over het maken van nieuwe modules en hoe u containerinstallatiekopieën publiceert naar een register.

## <a name="try-the-solution"></a>Probeer de oplossing

In deze sectie helpt bij het implementeren van Microsoft voorbeeld Modbus-module op uw IoT Edge-apparaat.

1. Ga in de [Azure-portal](https://portal.azure.com/) naar uw IoT-hub.

2. Ga naar **IoT Edge** en klikt u op uw IoT Edge-apparaat.

3. Selecteer **Modules instellen**.

4. Voeg als volgt de Modbus-module toe:

   1. Klik op **toevoegen** en selecteer **IoT Edge-module**.

   2. Typ 'modbus' in het veld **Naam**.

   3. Voer in het veld **Installatiekopie** de URI in naar de installatiekopie van de voorbeeldcontainer: `mcr.microsoft.com/azureiotedge/modbus:1.0`.

   4. Schakel het selectievakje **Inschakelen** in om de gewenst eigenschappen van de moduledubbel bij te werken.

   5. Kopieer de volgende JSON naar het tekstvak. Wijzig de waarde van **SlaveConnection** in het IPv4-adres van uw Modbus-apparaat.

      ```JSON
      {
        "properties.desired":{
          "PublishInterval":"2000",
          "SlaveConfigs":{
            "Slave01":{
              "SlaveConnection":"<IPV4 address>","HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{
                "Op01":{
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"400001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. Selecteer **Opslaan**.

5. Terug in de stap **Modules toevoegen** selecteert u **Volgende**.

7. In de stap **Routes opgeven** kopieert u de volgende JSON naar het tekstvak. Deze route verzendt alle berichten die door de Modbus-module worden verzameld naar IoT Hub. In deze route 'modbusOutput' is het eindpunt dat Modbus-module gebruikt om uit te voeren van gegevens en 'upstream' is een speciale bestemming die IoT Edge hub berichten verzenden naar IoT Hub opdracht geeft.
   ```JSON
   {
    "routes": {
      "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
    }
   }
   ```

8. Selecteer **Volgende**.

9. Selecteer in de stap **Implementatie beoordelen** de optie **Verzenden**.

10. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**. U ziet nu de nieuwe **modbus** module samen met de IoT Edge-runtime wordt uitgevoerd.

## <a name="view-data"></a>Gegevens weergeven
U bekijkt als volgt de gegevens die via de modbus-module lopen:
```cmd/sh
iotedge logs modbus
```

U kunt ook de telemetrie met behulp van het apparaat verzendt bekijken de [Azure IoT Hub Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (voorheen Azure IoT Toolkit-extensie).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over hoe IoT Edge-apparaten als gateways kunnen fungeren, [maken van een IoT Edge-apparaat die als een transparante gateway fungeert](./how-to-create-transparent-gateway.md).
- Zie voor meer informatie over de werking van IoT Edge-modules [inzicht in Azure IoT Edge-modules](iot-edge-modules.md).
