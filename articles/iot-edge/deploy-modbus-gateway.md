---
title: Modbus implementeren op Azure IoT Edge | Microsoft Docs
description: Toestaan dat apparaten die Modbus TCP gebruiken, kunnen communiceren met Azure IoT Hub door een IoT-gatewayapparaat te maken
author: kgremban
manager: timlt
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: kgremban
ms.openlocfilehash: e9db20a1458c398913387e40d247400ee933578b
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912302"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Modbus TCP-apparaten verbinden via de gateway van een IoT Edge-apparaat

Als u IoT-apparaten die gebruikmaken van Modbus TCP- of RTU-protocollen wilt verbinden met een Azure IoT-hub, gebruik dan een IoT Edge-apparaat als een gateway. Het gatewayapparaat leest de gegevens van uw Modbus-apparaten en geeft deze gegevens met behulp van een ondersteund protocol door aan de cloud. 

![Modbus-apparaten maken verbinding met IoT Hub via Edge-gateway](./media/deploy-modbus-gateway/diagram.png)

Dit artikel gaat over hoe u uw eigen containerinstallatiekopie maakt voor een Modbus-module (u kunt ook een kant- en-klaar voorbeeld gebruiken) en deze vervolgens implementeert op het IoT Edge-apparaat dat als uw gateway zal fungeren. 

In dit artikel wordt ervan uitgegaan dat u het Modbus-protocol TCP gebruikt. Zie voor meer informatie over hoe u de module configureert ter ondersteuning van Modbus RTU, de [Azure IoT Edge Modbus-module](https://github.com/Azure/iot-edge-modbus) project op Github. 

## <a name="prerequisites"></a>Vereisten
* Een Azure IoT Edge-apparaat. Voor een overzicht van hoe u er een instelt, raadpleegt u [Deploy Azure IoT Edge on a simulated device in Windows](quickstart.md) (Azure IoT Edge implementeren op een gesimuleerd apparaat in Windows) of [Linux](quickstart-linux.md). 
* De verbindingsreeks van de primaire sleutel voor het IoT Edge-apparaat.
* Een fysiek of gesimuleerd Modbus-apparaat dat Modbus TCP ondersteunt.

## <a name="prepare-a-modbus-container"></a>Een Modbus-container voorbereiden

Als u de functionaliteit van de Modbus-gateway wilt testen, heeft Microsoft een voorbeeldmodule die u kunt gebruiken. Als u de voorbeeldmodule wilt gebruiken, gaat u naar de sectie [De oplossing uitvoeren](#run-the-solution) en voert u de volgende URI van de installatiekopie in: 

```URL
mcr.microsoft.com/azureiotedge/modbus:1.0
```

Als u wilt uw eigen module maken en aanpassen voor uw omgeving, er is een open-source [Azure IoT Edge Modbus-module](https://github.com/Azure/iot-edge-modbus) project op Github. Volg de instructies in dit project om uw eigen containerinstallatiekopie te maken. Als u uw eigen containerinstallatiekopie hebt gemaakt, raadpleegt u [Develop and deploy a C# IoT Edge module](tutorial-csharp-module.md) (Een C# IoT Edge-module ontwikkelen en implementeren) voor instructies over hoe u containerinstallatiekopieën publiceert naar een register en een aangepaste module implementeert op uw apparaat. 


## <a name="run-the-solution"></a>De oplossing uitvoeren
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
7. In de stap **Routes opgeven** kopieert u de volgende JSON naar het tekstvak. Deze route verzendt alle berichten die door de Modbus-module worden verzameld naar IoT Hub. In deze route is 'modbusOutput' het eindpunt dat de Modbus-module gebruikt om gegevens uit te voeren, en is 'upstream' een speciale bestemming die Edge Hub vertelt dat berichten naar IoT Hub moeten worden verzonden. 
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
docker logs -f modbus
```

U kunt ook de telemetriegegevens bekijken die het apparaat verzendt door de [Azure IoT Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) te gebruiken. 

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over hoe IoT Edge-apparaten als gateways kunnen fungeren, raadpleegt u [Create an IoT Edge device that acts as a transparent gateway](./how-to-create-transparent-gateway.md) (Een IoT Edge-apparaat maken dat als een transparante gateway fungeert)
- Voor meer informatie over de werking van IoT Edge-modules raadpleegt u [Azure IoT Edge-modules](iot-edge-modules.md)
