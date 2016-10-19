<properties
    pageTitle="Aan de slag met de SDK-Gateway in IoT-Hub | Microsoft Azure"
    description="In dit overzicht van de Azure IoT Hub Gateway SDK wordt Linux gebruikt om de belangrijkste concepten te illustreren die u moet kennen wanneer u de Azure IoT Hub Gateway SDK gebruikt."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>



# IoT Gateway-SDK (bèta) - Aan de slag met Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## Het voorbeeld maken

Voordat u begint, moet u [uw ontwikkelomgeving instellen][lnk-setupdevbox] om met de SDK in Linux te werken.

1. Open een shell.
2. Navigeer naar de hoofdmap van uw lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk**.
3. Voer het script **tools/build.sh** uit. Dit script gebruikt het hulpprogramma **cmake** om een map **build** te maken in de hoofdmap van uw lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk** en een makefile te genereren. Het script bouwt vervolgens de oplossing op en voert de tests uit.

> [AZURE.NOTE]  Telkens wanneer u het script **build.sh** uitvoert, wordt de map **build** in de hoofdmap van het lokale exemplaar van de opslagplaats **azure-iot-gateway-sdk** verwijderd en weer gemaakt.

## Het voorbeeld uitvoeren

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
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
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

3. Navigeer in uw shell naar de map **azure-iot-gateway-sdk**.
4. Voer de volgende opdracht uit:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Oct16_HO1-->


