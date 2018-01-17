---
title: Modbus implementeren op Azure IoT Edge | Microsoft Docs
description: Toestaan dat apparaten die Modbus TCP gebruiken, kunnen communiceren met Azure IoT Hub door een IoT-gatewayapparaat te maken
services: iot-Edge
documentationcenter: 
author: kgremban
manager: timlt
editor: chrisgmsft
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: kgremban
ms.custom: 
ms.openlocfilehash: e239bde48c3da0d899e3c78bdd39f520c4128b95
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway---preview"></a>Modbus TCP-apparaten verbinden via een IoT Edge-apparaatgateway - Preview

Als u IoT-apparaten die gebruikmaken van Modbus TCP- of RTU-protocollen wilt verbinden met een Azure IoT-hub, gebruik dan een IoT Edge-apparaat als een gateway. Het gatewayapparaat leest de gegevens van uw Modbus-apparaten en geeft deze gegevens met behulp van een ondersteund protocol door aan de cloud. 

![Modbus-apparaten maken verbinding met IoT Hub via Edge-gateway](./media/deploy-modbus-gateway/diagram.png)

Dit artikel gaat over hoe u uw eigen containerinstallatiekopie maakt voor een Modbus-module (u kunt ook een kant- en-klaar voorbeeld gebruiken) en deze vervolgens implementeert op het IoT Edge-apparaat dat als uw gateway zal fungeren. 

In dit artikel wordt ervan uitgegaan dat u het Modbus-protocol TCP gebruikt. Voor meer informatie over hoe u de module configureert ter ondersteuning van Modbus RTU raadpleegt u het project [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) op Github. 

## <a name="prerequisites"></a>Vereisten
* Een Azure IoT Edge-apparaat. Voor een overzicht van hoe u er een instelt, raadpleegt u [Deploy Azure IoT Edge on a simulated device in Windows](tutorial-simulate-device-windows.md) (Azure IoT Edge implementeren op een gesimuleerd apparaat in Windows) of [Linux](tutorial-simulate-device-linux.md). 
* De verbindingsreeks van de primaire sleutel voor het IoT Edge-apparaat.
* Een fysiek of gesimuleerd Modbus-apparaat dat Modbus TCP ondersteunt.

## <a name="prepare-a-modbus-container"></a>Een Modbus-container voorbereiden

Als u de functionaliteit van de Modbus-gateway wilt testen, heeft Microsoft een voorbeeldmodule die u kunt gebruiken. Als u de voorbeeldmodule wilt gebruiken, gaat u naar de sectie [De oplossing uitvoeren](#run-the-solution) en voert u de volgende URI van de installatiekopie in: 

```URL
microsoft/azureiotedge-modbus-tcp:1.0-preview
```

Als u uw eigen module wilt maken en deze wilt aanpassen voor uw omgeving, bestaat er een open-source project [Azure IoT Edge Modbus-module](https://github.com/Azure/iot-edge-modbus) op Github. Volg de instructies in dit project om uw eigen containerinstallatiekopie te maken. Als u uw eigen containerinstallatiekopie hebt gemaakt, raadpleegt u [Develop and deploy a C# IoT Edge module](tutorial-csharp-module.md) (Een C# IoT Edge-module ontwikkelen en implementeren) voor instructies over hoe u containerinstallatiekopieën publiceert naar een register en een aangepaste module implementeert op uw apparaat. 


## <a name="run-the-solution"></a>De oplossing uitvoeren
1. Ga in de [Azure-portal](https://portal.azure.com/) naar uw IoT-hub.
2. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
3. Selecteer **Modules instellen**.
4. Voeg als volgt de Modbus-module toe:
   1. Select **Toevoegen aan IoT Edge-module**.
   2. Typ 'modbus' in het veld **Naam**.
   3. Voer in het veld **Installatiekopie** de URI in naar de installatiekopie van de voorbeeldcontainer: `microsoft/azureiotedge-modbus-tcp:1.0-preview`.
   4. Schakel het selectievakje **Inschakelen** in om de gewenst eigenschappen van de moduledubbel bij te werken.
   5. Kopieer de volgende JSON naar het tekstvak. Wijzig de waarde van **SlaveConnection** in het IPv4-adres van uw Modbus-apparaat.

      ```JSON
      {  
        "properties.desired":{  
          "PublishInterval":"2000",
          "SlaveConfigs":{  
            "Slave01":{  
              "SlaveConnection":"<IPV4 address>",
              "HwId":"PowerMeter-0a:01:01:01:01:01",
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
9. Selecteer in de stap **Sjabloon controleren** de optie **Indienen**. 
10. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**. U ziet nu de nieuwe **modbus** die samen met de runtime van IoT Edge wordt uitgevoerd.

## <a name="view-data"></a>Gegevens weergeven
U bekijkt als volgt de gegevens die via de modbus-module lopen:
```cmd/sh
docker logs -f modbus
```

Met behulp van de [IoT Hub-verkenner](https://github.com/azure/iothub-explorer) kunt u ook de telemetrie bekijken die het apparaat verzendt. 

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over hoe IoT Edge-apparaten als gateways kunnen fungeren, raadpleegt u [Create an IoT Edge device that acts as a transparent gateway](how-to-create-transparent-gateway.md) (Een IoT Edge-apparaat maken dat als een transparante gateway fungeert)
- Voor meer informatie over de werking van IoT Edge-modules raadpleegt u [Azure IoT Edge-modules](iot-edge-modules.md)