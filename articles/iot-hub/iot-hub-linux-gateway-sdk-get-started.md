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
ms.date: 08/25/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 23176a9251a90a985a5d2fbce23ceeb9d0925234


---
# <a name="azure-iot-gateway-sdk-beta-get-started-using-linux"></a>Azure IoT Gateway SDK (bèta) - Aan de slag met Linux
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
   
    Het bouwscript plaatst **liblogger_hl.so** in de map **build/modules/logger/** en **libhello_world_hl.so** in de map **build/modules/hello_world/**. Gebruik deze paden voor de waarde van **modulepad** zoals weergegeven in het bestand met JSON-instellingen hieronder.
2. Het bestand **hello_world_lin.json** in de map **samples/hello_world/src** is een voorbeeld van het bestand met JSON-instellingen voor Linux dat u kunt gebruiken om het voorbeeld uit te voeren. Bij de voorbeeld-JSON-instellingen die hieronder zijn weergegeven, wordt ervan uitgegaan dat u het voorbeeld uitvoert vanuit de hoofdmap van uw lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk**.
3. Voor de module **logger_hl** in de sectie **args**, stelt u de waarde van **filename** in op de naam en het pad van het bestand dat de logboekgegevens zal bevatten.
   
    Dit is een voorbeeld van een bestand met JSON-instellingen voor Linux waarbij wordt geschreven naar het bestand **log.txt** van de map waarin u het voorbeeld uitvoert.
   
    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "loading args": {
            "module path" : "./build/modules/logger/liblogger_hl.so"
          },
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "loading args": {
            "module path" : "./build/modules/hello_world/libhello_world_hl.so"
          },
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```
4. Navigeer in uw shell naar de map **azure-iot-gateway-sdk**.
5. Voer de volgende opdracht uit:
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO2-->


