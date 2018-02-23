---
title: Uploaden van bestanden van apparaten naar Azure IoT Hub met behulp van Python | Microsoft Docs
description: "Het uploaden van bestanden van een apparaat naar de cloud met Azure IoT-device SDK voor Python. Geüploade bestanden worden opgeslagen in een Azure storage-blob-container."
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2018
ms.author: v-masebo
ms.openlocfilehash: ac871a03ebb93dac3b91f7df2220cd5f4506498b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Uploaden van bestanden op uw apparaat naar de cloud met IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Deze zelfstudie volgt het gebruik van de [bestand uploaden mogelijkheden van IoT Hub](iot-hub-devguide-file-upload.md) voor het uploaden van een bestand naar [Azure blob-opslag](../storage/index.yml). De zelfstudie leert u hoe aan:

- Geef een opslagcontainer veilig voor het uploaden van een bestand.
- De client voor Python gebruiken voor het uploaden van een bestand via uw IoT-hub.

De [aan de slag met IoT Hub](iot-hub-node-node-getstarted.md) en [Cloud naar apparaat verzenden met IoT Hub](iot-hub-node-node-c2d.md) zelfstudies ziet de apparaat-naar-cloud- en cloud-naar-apparaat messaging basisfunctionaliteit van IoT-Hub. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens verzenden van uw apparaten in de relatief klein apparaat-naar-cloud-berichten die IoT Hub accepteert. Wanneer u upland bestanden van een apparaat moet, kunt u de beveiliging en betrouwbaarheid van IoT Hub.

> [!NOTE]
> IoT Hub Python SDK ondersteunt momenteel alleen zoals uploaden van bestanden op basis van het teken **.txt** bestanden.

Aan het einde van deze zelfstudie kunt u de Python-console-app uitvoeren:

* **FileUpload.py**, die een bestand geüpload naar de opslag met de apparaat-SDK voor Python.

> [!NOTE]
> IoT Hub biedt ondersteuning voor veel apparaatplatforms en talen (inclusief C, .NET, Python, Javascript en Java) via Azure IoT-apparaat-SDK's. Raadpleeg de [Azure IoT Developer Center] voor stapsgewijze instructies voor het koppelen van uw apparaat met Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)


[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]


## <a name="upload-a-file-from-a-device-app"></a>Een bestand vanaf een apparaat-app uploaden

In deze sectie maakt u de apparaat-app voor het uploaden van een bestand met iothub.

1. Voer bij de opdrachtprompt de volgende opdracht voor het installeren van de **azure-iothub-apparaat-client** pakket:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Maak met een teksteditor, een **FileUpload.py** bestand in uw werkmap.

1. Voeg de volgende `import` -instructies en variabelen aan het begin van de **FileUpload.py** bestand. Vervang `deviceConnectionString` met de verbindingsreeks van uw IoT hub-apparaat:

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.HTTP

    FILENAME = 'sample.txt'
    ```

1. Maak een callback voor de **upload_blob** functie:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

1. Voeg de volgende code om de client verbinden en upload het bestand. Ook de `main` routine:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )
        
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            client.upload_blob_async(FILENAME, FILENAME, os.path.getsize(FILENAME), blob_upload_conf_callback, 0)
        
            print ( "" )
            print ( "File upload initiated..." )
        
            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )
        
    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

1. Sla op en sluit de **UploadFile.py** bestand.

1. Een voorbeeldtekstbestand kopiëren naar de map en wijzig deze `sample.txt`.

    > [!NOTE]
    > IoT Hub Python SDK ondersteunt momenteel alleen zoals uploaden van bestanden op basis van het teken **.txt** bestanden.


## <a name="run-the-application"></a>De toepassing uitvoeren

U bent nu klaar om uit te voeren van de toepassing.

1. Bij een opdrachtprompt in uw werkmap, voer de volgende opdracht:

    ```cmd/sh
    python FileUpload.py
    ```

1. De volgende schermafbeelding ziet u de uitvoer van de **FileUpload** app:

    ![De uitvoer van de gesimuleerde apparaattoepassing](./media/iot-hub-python-python-file-upload/1.png)

1. U kunt de portal gebruiken om het geüploade bestand in de storage-container die u hebt geconfigureerd:

    ![Geüploade bestand](./media/iot-hub-python-python-file-upload/2.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe de mogelijkheden voor het uploaden van bestand van IoT Hub gebruiken voor het vereenvoudigen van bestandsuploads van apparaten. U kunt doorgaan met het verkennen van IoT hub functies en scenario's met de volgende artikelen:

* [Een iothub via een programma maken][lnk-create-hub]
* [Inleiding tot C-SDK][lnk-c-sdk]
* [Azure IoT SDKs][lnk-sdks]

<!-- Links -->
[Azure IoT Developer Center]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/