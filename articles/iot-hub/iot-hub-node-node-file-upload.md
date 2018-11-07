---
title: Uploaden van bestanden vanaf apparaten met Azure IoT Hub met Node | Microsoft Docs
description: Klik hier voor meer informatie over het uploaden van bestanden vanaf een apparaat naar de cloud met Azure IoT device-SDK voor Node.js. Geüploade bestanden worden opgeslagen in een Azure storage blob-container.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 12ff4fef5e04819e967a39fe65845b89790e22d6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234442"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Uploaden van bestanden van uw apparaat naar de cloud met IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

In deze zelfstudie bouwt voort op de code in de [Cloud-naar-apparaat-berichten verzenden met IoT Hub](iot-hub-node-node-c2d.md) zelfstudie leert u hoe u gebruik van de [bestand uploaden mogelijkheden van IoT-Hub](iot-hub-devguide-file-upload.md) naar een bestand uploadt naar [Azure blob opslag](../storage/index.yml). In deze zelfstudie leert u het volgende:

- Veilig een apparaat voorzien van een Azure blob-URI voor het uploaden van een bestand.
- Gebruik de IoT Hub-bestand uploaden meldingen voor het activeren van het bestand in de back-end van uw app wordt verwerkt.

De [aan de slag met IoT Hub](quickstart-send-telemetry-node.md) de basisfunctionaliteit apparaat-naar-cloud berichten zijn van IoT Hub wordt gedemonstreerd. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens die uw apparaten verzenden naar de relatief klein aantal apparaat-naar-cloud-berichten die IoT Hub worden geaccepteerd. Bijvoorbeeld:

* Grote bestanden met afbeeldingen
* Video's
* Trillingen gegevens verzameld met hoge frequentie
* Een vorm van voorverwerkte gegevens.

Deze bestanden zijn meestal batch verwerkt in de cloud met behulp van hulpprogramma's zoals [Azure Data Factory](../data-factory/introduction.md) of de [Hadoop](../hdinsight/index.yml) stack. Wanneer u upland bestanden vanaf een apparaat wilt, kunt u de beveiliging en betrouwbaarheid van IoT Hub nog steeds gebruiken.

Aan het einde van deze zelfstudie moet u twee Node.js-consoletoepassingen uitvoeren:

* **SimulatedDevice.js**, die een bestand wordt geüpload naar storage met behulp van een SAS-URI geleverd door uw IoT-hub.
* **ReadFileUploadNotification.js**, dat bestand uploaden meldingen ontvangt van uw IoT-hub.

> [!NOTE]
> IoT Hub biedt ondersteuning voor vele platformen voor apparaten en talen (waaronder C, .NET, Javascript, Python en Java) via Azure IoT device-SDK's. Raadpleeg de [Azure IoT-ontwikkelaarscentrum] voor stapsgewijze instructies over hoe u uw apparaat aansluiten op Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Upload een bestand van een apparaat-app

In deze sectie maakt u de apparaat-app voor een bestand uploaden naar IoT hub.

1. Maak een lege map met de naam ```simulateddevice```.  Maak in de map ```simulateddevice``` een package.json-bestand door achter de opdrachtprompt de volgende opdracht op te geven.  Accepteer alle standaardwaarden:

    ```cmd/sh
    npm init
    ```

1. In de opdrachtprompt in de map ```simulateddevice``` voert u de volgende opdracht uit om het **azure-iot-device** Device SDK-pakket en het **azure-iot-device-mqtt**-pakket te installeren:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Maak met een teksteditor een **SimulatedDevice.js**-bestand in de map ```simulateddevice```.

1. Voeg de volgende ```require``` instructies toe aan het begin van het bestand **SimulatedDevice.js**:

    ```nodejs
    'use strict';
    
    var fs = require('fs');
    var mqtt = require('azure-iot-device-mqtt').Mqtt;
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    ```

1. Voeg een ```deviceconnectionstring```-variabele toe en gebruik deze om een **client**exemplaar te maken.  Vervang ```{deviceconnectionstring}``` met de naam van het apparaat dat u hebt gemaakt in de _maken van een IoT-Hub_ sectie:

    ```nodejs
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > Om het eenvoudig te de verbindingsreeks is opgenomen in de code: dit is geen aanbevolen en afhankelijk van uw use-case en -architectuur kunt u rekening houden met veiliger manieren om dit geheim op te slaan.

1. Voeg de volgende code om de client verbinding te maken:

    ```nodejs
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

1. Maak een callback en gebruik de **uploadToBlob** functie om het bestand te uploaden.

    ```nodejs
    fs.stat(filename, function (err, stats) {
        const rr = fs.createReadStream(filename);
    
        client.uploadToBlob(filename, rr, stats.size, function (err) {
            if (err) {
                console.error('Error uploading file: ' + err.toString());
            } else {
                console.log('File uploaded');
            }
        });
    });
    ```

1. Sla het bestand **SimulatedDevice.js** op en sluit het.

1. Een van installatiekopiebestand kopiëren naar de `simulateddevice` map en wijzig de naam `myimage.png`.

## <a name="receive-a-file-upload-notification"></a>Een bestand uploaden melding ontvangen

In deze sectie maakt u een Node.js-consoletoepassing die bestand uploaden kennisgeving berichten uit IoT Hub ontvangt.

U kunt de **iothubowner** connection string vanuit uw IoT-Hub in deze sectie voltooid. U vindt de verbindingsreeks in de [Azure-portal](https://portal.azure.com/) op de **gedeeld toegangsbeleid** blade.

1. Maak een lege map met de naam ```fileuploadnotification```.  Maak in de map ```fileuploadnotification``` een package.json-bestand door achter de opdrachtprompt de volgende opdracht op te geven.  Accepteer alle standaardwaarden:

    ```cmd/sh
    npm init
    ```

1. In het opdrachtprompt in de ```fileuploadnotification``` map, voer de volgende opdracht voor het installeren van de **azure-iothub** SDK-pakket:

    ```cmd/sh
    npm install azure-iothub --save
    ```

1. Maak met een teksteditor een **FileUploadNotification.js** -bestand in de ```fileuploadnotification``` map.

1. Voeg de volgende ```require``` instructies toe aan het begin van de **FileUploadNotification.js** bestand:

    ```nodejs
    'use strict';
    
    var Client = require('azure-iothub').Client;
    ```

1. Voeg een ```iothubconnectionstring```-variabele toe en gebruik deze om een **client**exemplaar te maken.  Vervang ```{iothubconnectionstring}``` door de verbindingsreeks naar de IoT-hub die u hebt gemaakt in de _maken van een IoT-Hub_ sectie:

    ```nodejs
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Om het eenvoudig te de verbindingsreeks is opgenomen in de code: dit is geen aanbevolen en afhankelijk van uw use-case en -architectuur kunt u rekening houden met veiliger manieren om dit geheim op te slaan.

1. Voeg de volgende code om de client verbinding te maken:

    ```nodejs
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

1. Open de client en gebruik de **getFileNotificationReceiver** functie om de statusupdates te ontvangen.

    ```nodejs
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFileNotificationReceiver(function receiveFileUploadNotification(err, receiver){
          if (err) {
            console.error('error getting the file notification receiver: ' + err.toString());
          } else {
            receiver.on('message', function (msg) {
              console.log('File upload from device:')
              console.log(msg.getData().toString('utf-8'));
            });
          }
        });
      }
    });
    ```

1. Opslaan en sluiten de **FileUploadNotification.js** bestand.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

Bij een opdrachtprompt in de `fileuploadnotification` map, voer de volgende opdracht uit:

```cmd/sh
node FileUploadNotification.js
```

Bij een opdrachtprompt in de `simulateddevice` map, voer de volgende opdracht uit:

```cmd/sh
node SimulatedDevice.js
```

De volgende schermafbeelding ziet u de uitvoer van de **SimulatedDevice** app:

![Uitvoer van het gesimuleerde apparaat-app](./media/iot-hub-node-node-file-upload/simulated-device.png)

De volgende schermafbeelding ziet u de uitvoer van de **FileUploadNotification** app:

![Uitvoer van de app lezen-bestand-upload-melding](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

U kunt de portal gebruiken om het geüploade bestand in de storage-container die u hebt geconfigureerd:

![Het geüploade bestand](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de mogelijkheden voor het uploaden van bestand van IoT Hub gebruikt voor het vereenvoudigen van het uploaden van bestanden vanaf apparaten. U kunt doorgaan met het verkennen van IoT hub-functies en scenario's met de volgende artikelen:

* [Een IoT hub via een programma maken][lnk-create-hub]
* [Inleiding tot C SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

<!-- Links -->
[Azure IoT-ontwikkelaarscentrum]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
