---
title: Uploaden van bestanden vanaf apparaten met Azure IoT Hub met Python | Microsoft Docs
description: Klik hier voor meer informatie over het uploaden van bestanden vanaf een apparaat naar de cloud met behulp van Azure IoT device-SDK voor Python. Geüploade bestanden worden opgeslagen in een Azure storage blob-container.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: kgremban
ms.openlocfilehash: 0fe2b33bc5f9a0b599934c4cabb065d4c97ea61b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549881"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Uploaden van bestanden van uw apparaat naar de cloud met IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

In dit artikel ziet u hoe u de [bestand uploaden mogelijkheden van IoT-Hub](iot-hub-devguide-file-upload.md) naar een bestand uploadt naar [Azure blob-opslag](../storage/index.yml). In deze zelfstudie leert u het volgende:

- Geef een opslagcontainer veilig voor het uploaden van een bestand.
- De Python-client gebruiken voor het uploaden van een bestand via uw IoT-hub.

De [telemetrie verzenden naar IoT Hub](quickstart-send-telemetry-python.md) Quick Start ziet u de basisfunctionaliteit apparaat-naar-cloud berichten zijn van IoT-Hub. Echter, in sommige scenario's kan niet eenvoudig koppelt u de gegevens die uw apparaten verzenden naar de relatief klein aantal apparaat-naar-cloud-berichten die IoT Hub worden geaccepteerd. Wanneer u upland bestanden vanaf een apparaat wilt, kunt u de beveiliging en betrouwbaarheid van IoT Hub nog steeds gebruiken.

> [!NOTE]
> Python-SDK voor IoT-Hub ondersteunt momenteel alleen tekens gebaseerde bestanden zoals uploadt **.txt** bestanden.

Aan het einde van deze zelfstudie moet u de Python-console-app uitvoeren:

* **FileUpload.py**, die een bestand wordt geüpload naar storage met behulp van de apparaat-SDK voor Python.

> [!NOTE]
> IoT Hub biedt ondersteuning voor vele platformen voor apparaten en talen (waaronder C, .NET, Javascript, Python en Java) via Azure IoT device-SDK's. Raadpleeg de [Azure IoT-ontwikkelaarscentrum] voor stapsgewijze instructies over hoe u uw apparaat aansluiten op Azure IoT Hub.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.
* Een IoT-hub in uw Azure-account met een apparaat-id voor het testen van de functionaliteit voor het uploaden. 

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]


## <a name="upload-a-file-from-a-device-app"></a>Upload een bestand van een apparaat-app

In deze sectie maakt u de apparaat-app voor een bestand uploaden naar IoT hub.

1. Bij de opdrachtprompt, voer de volgende opdracht voor het installeren van de **azure-iothub-apparaat-client** pakket:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Met een teksteditor, maak een testbestand dat u naar blob-opslag uploaden zult. 

    > [!NOTE]
    > Python-SDK voor IoT-Hub ondersteunt momenteel alleen tekens gebaseerde bestanden zoals uploadt **.txt** bestanden.

1. Maak met een teksteditor een **FileUpload.py** bestand in de werkmap.

1. Voeg de volgende `import` instructies en -variabelen aan het begin van de **FileUpload.py** bestand. 

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name for storage]"
    ```

1. Vervang in het bestand `[Device Connection String]` door de verbindingsreeks van uw IoT hub-apparaat. Vervang `[Full path to file]` met het pad naar de testbestand dat u hebt gemaakt, of een bestand op uw apparaat dat u wilt uploaden. Vervang `[File name for storage]` met de naam die u geven tot uw bestand wilt nadat deze is geüpload naar blob-opslag. 

1. Maak een callback voor de **upload_blob** functie:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

1. Voeg de volgende code voor de verbinding van de client en upload het bestand. Ook de `main` routine:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            f = open(PATHTOFILE, "r")
            content = f.read()

            client.upload_blob_async(FILENAME, content, len(content), blob_upload_conf_callback, 0)

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

1. Opslaan en sluiten de **UploadFile.py** bestand.


## <a name="run-the-application"></a>De toepassing uitvoeren

U bent nu klaar om uit te voeren van de toepassing.

1. Bij een opdrachtprompt in de werkmap, voer de volgende opdracht uit:

    ```cmd/sh
    python FileUpload.py
    ```

1. De volgende schermafbeelding ziet u de uitvoer van de **FileUpload** app:

    ![Uitvoer van het gesimuleerde apparaat-app](./media/iot-hub-python-python-file-upload/1.png)

1. U kunt de portal gebruiken om het geüploade bestand in de storage-container die u hebt geconfigureerd:

    ![Het geüploade bestand](./media/iot-hub-python-python-file-upload/2.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de mogelijkheden voor het uploaden van bestand van IoT Hub gebruikt voor het vereenvoudigen van het uploaden van bestanden vanaf apparaten. U kunt doorgaan met het verkennen van IoT hub-functies en scenario's met de volgende artikelen:

* [Een IoT hub via een programma maken][lnk-create-hub]
* [Inleiding tot C SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

<!-- Links -->
[Azure IoT-ontwikkelaarscentrum]: https://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: https://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
