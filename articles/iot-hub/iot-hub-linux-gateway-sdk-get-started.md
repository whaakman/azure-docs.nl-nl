---
title: Aan de slag met de SDK-Gateway in IoT Hub | Microsoft Docs
description: In dit overzicht van de Azure IoT Gateway SDK wordt Linux gebruikt om de belangrijkste concepten te illustreren die u moet kennen wanneer u de Azure IoT ateway SDK gebruikt.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/23/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: a76320718f0cefa015728cb79df944e0d34bbf74
ms.openlocfilehash: cbb909adc2d29f9b80a4c97d06176fe74b64a75a


---
# <a name="azure-iot-gateway-sdk---get-started-using-linux"></a>Azure IoT Gateway-SDK - Aan de slag met Linux
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Het voorbeeld maken
Voordat u begint, moet u [uw ontwikkelomgeving instellen][lnk-setupdevbox] om met de SDK in Linux te werken.

1. Open een shell.
2. Navigeer naar de hoofdmap van uw lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk**.
3. Voer het script **tools/build.sh** uit. Dit script gebruikt het hulpprogramma **cmake** om een map **build** te maken in de hoofdmap van uw lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk** en een makefile te genereren. Het script bouwt vervolgens de oplossing op en voert de tests uit.

> [!NOTE]
> Telkens wanneer u het script **build.sh** uitvoert, wordt de map **build** in de hoofdmap van het lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk** verwijderd en weer gemaakt.
> 
> 

## <a name="how-to-run-the-sample"></a>Het voorbeeld uitvoeren
1. De uitvoer van het script **build.sh** wordt gegenereerd in de map **build** in de lokale kopie van de opslagplaats **azure-iot-gateway-sdk**. Dit omvat de twee modules die in dit voorbeeld worden gebruikt.
   
    Het bouwscript plaatst **liblogger.so** in de map **build/modules/logger/** en **libhello_world.so** in de map **build/modules/hello_world/**. Gebruik deze paden voor de waarde van **modulepad** zoals weergegeven in het bestand met JSON-instellingen hieronder.
2. Het proces hello_world_sample gebruikt het pad naar een JSON-configuratiebestand als een argument in de opdrachtregel. Er is een voorbeeld-JSON-bestand geleverd als onderdeel van de repo op **azure-iot-gateway-sdk/samples/hello_world/src/hello_world_win.json**. Dit is hieronder gekopieerd. Het bestand werkt in de huidige staat tenzij u het bouwscript hebt aangepast om modules of uitvoerbare voorbeeldbestanden op niet-standaardlocaties te plaatsen.

   > [!NOTE]
   > De modulepaden zijn gekoppeld aan de huidige werkmap vanwaaruit het uitvoerbare bestand hello_world_sample wordt gestart, niet aan de map waarin het uitvoerbare bestand zich bevindt. Het voorbeeld-JSON-configuratiebestand schrijft standaard 'log.txt' in uw huidige werkmap.
   
    ```
    {
        "modules" :
        [
            {
              "name" : "logger",
              "loader": {
                "name": "native",
                "entrypoint": {
                  "module.path": "./modules/logger/liblogger.so"
                }
              },
              "args" : {"filename":"log.txt"}
            },
            {
                "name" : "hello_world",
              "loader": {
                "name": "native",
                "entrypoint": {
                  "module.path": "./modules/hello_world/libhello_world.so"
                }
              },
                "args" : null
            }
        ],
        "links": 
        [
            {
                "source": "hello_world",
                "sink": "logger"
            }
        ]
    }
    ```
3. Navigeer naar de map **azure-iot-gateway-sdk**.
4. Voer de volgende opdracht uit:
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO4-->


