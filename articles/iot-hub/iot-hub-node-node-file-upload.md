---
title: Uploaden van bestanden van apparaten naar Azure IoT Hub met knooppunt | Microsoft Docs
description: "Het uploaden van bestanden van een apparaat naar de cloud met Azure IoT-device SDK voor Node.js. Geüploade bestanden worden opgeslagen in een Azure storage-blob-container."
services: iot-hub
documentationcenter: nodejs
author: msebolt
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: v-masebo
ms.openlocfilehash: cff0f2fc664e0c09bfa1f8f0e0d488a049a6f448
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Uploaden van bestanden op uw apparaat naar de cloud met IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Deze zelfstudie bouwt voort op de code in de [Cloud naar apparaat verzenden met IoT Hub](iot-hub-node-node-c2d.md) zelfstudie leert u gebruikt de [bestand uploaden mogelijkheden van IoT Hub](iot-hub-devguide-file-upload.md) voor het uploaden van een bestand naar [Azure blob-opslag](../storage/index.yml). De zelfstudie leert u hoe aan:

- Veilig een apparaat voorzien van een Azure blob-URI voor het uploaden van een bestand.
- Gebruik de IoT Hub-bestand uploaden meldingen voor het activeren van het bestand in de back-end van uw app wordt verwerkt.

De [aan de slag met IoT Hub](iot-hub-node-node-getstarted.md) en [Cloud naar apparaat verzenden met IoT Hub](iot-hub-node-node-c2d.md) zelfstudies ziet de apparaat-naar-cloud- en cloud-naar-apparaat messaging basisfunctionaliteit van IoT-Hub. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens verzenden van uw apparaten in de relatief klein apparaat-naar-cloud-berichten die IoT Hub accepteert. Bijvoorbeeld:

* Grote bestanden met afbeeldingen
* Video's
* Trillingen gegevens dat met hoge frequentie
* Een vorm van voorverwerkte gegevens.

Deze bestanden zijn meestal batch verwerkt in de cloud met hulpprogramma's zoals [Azure Data Factory](../data-factory/introduction.md) of de [Hadoop](../hdinsight/index.md) stack. Wanneer u upland bestanden van een apparaat moet, kunt u de beveiliging en betrouwbaarheid van IoT Hub.

Aan het einde van deze zelfstudie kunt u twee console Node.js-apps uitvoeren:

* **SimulatedDevice.js**, die een bestand geüpload naar de opslag met een SAS-URI geleverd door uw IoT-hub.
* **ReadFileUploadNotification.js**, dat bestand uploaden meldingen ontvangt van uw IoT-hub.

> [!NOTE]
> IoT Hub biedt ondersteuning voor veel apparaatplatforms en talen (inclusief C, .NET, Python, Javascript en Java) via Azure IoT-apparaat-SDK's. Raadpleeg de [Azure IoT Developer Center] voor stapsgewijze instructies voor het koppelen van uw apparaat met Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Een bestand vanaf een apparaat-app uploaden

In deze sectie maakt u de apparaat-app voor het uploaden van een bestand met iothub.

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

1. Voeg een ```deviceconnectionstring```-variabele toe en gebruik deze om een **client**exemplaar te maken.  Vervang ```{deviceconnectionstring}``` met de naam van het apparaat dat u hebt gemaakt in de _een IoT Hub maken_ sectie:

    ```nodejs
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > Omdat dit eenvoudiger de verbindingsreeks is opgenomen in de code: dit is niet aanbevolen en afhankelijk van uw use case en architectuur mogelijk wilt u veiliger manieren voor het opslaan van dit geheim.

1. Voeg de volgende code om de client verbinding te maken:

    ```nodejs
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

1. Maak een callback en gebruik de **uploadToBlob** functie het bestand te uploaden.

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

## <a name="receive-a-file-upload-notification"></a>Ontvangt een melding van bestand uploaden

In deze sectie maakt u een Node.js-consoletoepassing die bestand uploaden meldingsberichten uit IoT Hub ontvangt.

U kunt de **iothubowner** verbindingsreeks uit uw IoT-Hub in deze sectie voltooid. Vindt u de verbindingsreeks in de [Azure-portal](https://portal.azure.com/) op de **beleid voor gedeelde toegang** blade.

1. Maak een lege map met de naam ```fileuploadnotification```.  Maak in de map ```fileuploadnotification``` een package.json-bestand door achter de opdrachtprompt de volgende opdracht op te geven.  Accepteer alle standaardwaarden:

    ```cmd/sh
    npm init
    ```

1. Bij de opdrachtprompt in de ```fileuploadnotification``` map, voer de volgende opdracht voor het installeren van de **azure-iothub** SDK-pakket:

    ```cmd/sh
    npm install azure-iothub --save
    ```

1. Maak met een teksteditor, een **FileUploadNotification.js** bestand de ```fileuploadnotification``` map.

1. Voeg de volgende ```require``` instructies aan het begin van de **FileUploadNotification.js** bestand:

    ```nodejs
    'use strict';
    
    var Client = require('azure-iothub').Client;
    ```

1. Voeg een ```iothubconnectionstring```-variabele toe en gebruik deze om een **client**exemplaar te maken.  Vervang ```{iothubconnectionstring}``` met de verbindingsreeks met de IoT-hub die u hebt gemaakt in de _een IoT Hub maken_ sectie:

    ```nodejs
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Omdat dit eenvoudiger de verbindingsreeks is opgenomen in de code: dit is niet aanbevolen en afhankelijk van uw use case en architectuur mogelijk wilt u veiliger manieren voor het opslaan van dit geheim.

1. Voeg de volgende code om de client verbinding te maken:

    ```nodejs
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

1. De client openen en gebruiken de **getFileNotificationReceiver** functie statusupdates moet ontvangen.

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

1. Sla op en sluit de **FileUploadNotification.js** bestand.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

Bij een opdrachtprompt in de `fileuploadnotification` map, voer de volgende opdracht:

```cmd/sh
node FileUploadNotification.js
```

Bij een opdrachtprompt in de `simulateddevice` map, voer de volgende opdracht:

```cmd/sh
node SimulatedDevice.js
```

De volgende schermafbeelding ziet u de uitvoer van de **SimulatedDevice** app:

![De uitvoer van de gesimuleerde apparaattoepassing](./media/iot-hub-node-node-file-upload/simulated-device.png)

De volgende schermafbeelding ziet u de uitvoer van de **FileUploadNotification** app:

![De uitvoer van de lezen-bestand-upload-melding app](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

U kunt de portal gebruiken om het geüploade bestand in de storage-container die u hebt geconfigureerd:

![Geüploade bestand](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe de mogelijkheden voor het uploaden van bestand van IoT Hub gebruiken voor het vereenvoudigen van bestandsuploads van apparaten. U kunt doorgaan met het verkennen van IoT hub functies en scenario's met de volgende artikelen:

* [Een iothub via een programma maken][lnk-create-hub]
* [Inleiding tot C-SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

<!-- Links -->
[Azure IoT Developer Center]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
