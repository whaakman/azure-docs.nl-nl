---
title: Aan de slag met de Azure IoT-gateway-SDK (Linux) | Microsoft Docs
description: Kom meer te weten over het bouwen van een gateway op een Linux-computer en over belangrijke concepten in de Azure IoT-gateway-SDK, zoals modules en JSON-configuratiebestanden.
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
ms.sourcegitcommit: 6b41567f7b43fd6a56da1f571e007d31cef97b92
ms.openlocfilehash: 68965a1157b31d75595e546b2b227844ddff2eb9


---
# <a name="get-started-with-the-azure-iot-gateway-sdk-linux"></a>Aan de slag met de Azure IoT-gateway-SDK (Linux)
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Het voorbeeld maken
Voordat u begint, moet u [uw ontwikkelomgeving instellen][lnk-setupdevbox] om met de SDK in Linux te werken.

1. Open een shell.
2. Navigeer naar de hoofdmap van uw lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk**.
3. Voer het script **tools/build.sh --skip-unittests** uit. Dit script gebruikt het hulpprogramma **cmake** om een map **build** te maken in de hoofdmap van uw lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk** en een makefile te genereren. Het script bouwt vervolgens de oplossing. De eenheidstest worden overgeslagen. Verwijder de parameter **--skip-unittests** als u de eenheidstests wilt bouwen en uitvoeren.

> [!NOTE]
> Telkens wanneer u het script **build.sh** uitvoert, wordt de map **build** in de hoofdmap van het lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk** verwijderd en weer gemaakt.
> 
> 

## <a name="how-to-run-the-sample"></a>Het voorbeeld uitvoeren
1. De uitvoer van het script **build.sh** wordt gegenereerd in de map **build** in de lokale kopie van de opslagplaats **azure-iot-gateway-sdk**. Dit omvat de twee modules die in dit voorbeeld worden gebruikt.
   
    Het bouwscript plaatst **liblogger.so** in de map **build/modules/logger/** en **libhello_world.so** in de map **build/modules/hello_world/**. Gebruik deze paden voor de waarde van **modulepad** zoals weergegeven in het bestand met JSON-instellingen hieronder.
2. In het proces hello_world_sample wordt het pad naar een JSON-configuratiebestand gebruikt als een argument in de opdrachtregel. Er is een voorbeeld-JSON-bestand geleverd als onderdeel van de repo op **azure-iot-gateway-sdk/samples/hello_world/src/hello_world_win.json**. Dit is hieronder gekopieerd. Het bestand werkt in de huidige staat tenzij u het bouwscript hebt aangepast om modules of uitvoerbare voorbeeldbestanden op niet-standaardlocaties te plaatsen.

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
3. Navigeer naar de map **azure-iot-gateway-sdk/build**.
4. Voer de volgende opdracht uit:
   
   ```
   ./samples/hello_world/hello_world_sample ./../samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Dec16_HO1-->


