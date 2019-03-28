---
title: 'Snelstartgids: Een apparaat beheren vanuit Azure IoT Hub (Python) | Microsoft Docs'
description: In deze snelstartgids gaan we twee voorbeeldtoepassingen geschreven in Python uitvoeren. De ene toepassing is een back-endtoepassing waarmee u op afstand apparaten kunt beheren die zijn verbonden met uw hub. De andere toepassing simuleert een apparaat dat is verbonden met uw hub en dat op afstand kan worden beheerd.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/26/2019
ms.openlocfilehash: ce3bf98a5f31f18c6759b202d53d8a1ced46296e
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519660"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-python"></a>Quickstart: Een apparaat dat is verbonden met een IoT-hub (Python) beheren

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub is een Azure-service waarmee u grote hoeveelheden telemetrie van uw IoT-apparaten kunt overbrengen naar de cloud en uw apparaten kunt beheren vanuit de cloud. In deze snelstartgids gebruikt u een *directe methode* om een gesimuleerd apparaat te beheren dat met uw IoT-hub is verbonden. U kunt directe methoden gebruiken om het gedrag van een apparaat dat is verbonden met uw IoT-hub, op afstand te wijzigen.

In de snelstartgids worden twee vooraf geschreven Python-toepassingen gebruikt:

* Een toepassing voor een gesimuleerd apparaat die reageert op de directe methoden die worden aangeroepen vanuit een back-endtoepassing. Om de aanroepen van de directe methoden te kunnen ontvangen, maakt deze toepassing verbinding met een apparaatspecifiek eindpunt op uw IoT-hub.

* Een back-endtoepassing die de directe methoden op het gesimuleerde apparaat aanroept. Als u een directe methode op een apparaat wilt aanroepen, maakt u met deze toepassing verbinding met een eindpunt aan de servicezijde van uw IoT-hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De twee voorbeeldtoepassingen die u uitvoert in deze snelstartgids zijn geschreven in Python. De Microsoft Azure IoT SDK's voor Python momenteel alleen specifieke versies van Python voor elk platform. Zie voor meer informatie, de [Python SDK Leesmij](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).

In deze snelstartgids wordt ervan uitgegaan dat u een Windows-ontwikkelcomputer. Voor Windows-systemen, alleen [Python 3.6.x](https://www.python.org/downloads/release/python-368/) wordt ondersteund. Welk Python-installatieprogramma u moet kiezen, hangt af van de architectuur van het systeem waarmee u werkt. Als uw systeem-CPU-architectuur 32-bits en download de x86 installatieprogramma is; voor de 64-bits architectuur, het x86-64-installatieprogramma te downloaden. Controleer ook of de [Microsoft Visual C++ Redistributable voor Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) voor uw architectuur (x86 of x64) is geïnstalleerd.

U kunt Python downloaden voor andere platforms van [Python.org](https://www.python.org/downloads/).

Gebruik een van de volgende opdrachten om te controleren wat de huidige versie van Python op uw ontwikkelcomputer is:

```python
python --version
```

```python
python3 --version
```

Als u dit nog niet hebt gedaan, downloadt u het voorbeeldproject met Python van https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip en pakt u het ZIP-archief uit.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Als u [Snelstart: Als u telemetrie vanaf een apparaat wilt verzenden naar een IoT-hub](quickstart-send-telemetry-python.md), kunt u deze stap overslaan.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Als u [Snelstart: Als u telemetrie vanaf een apparaat wilt verzenden naar een IoT-hub](quickstart-send-telemetry-python.md), kunt u deze stap overslaan.

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdrachten uit in Azure Cloud Shell om de IoT Hub CLI-extensie toe te voegen en de apparaat-id te maken. 

    **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    **MyPythonDevice** : dit is de naam van het geregistreerde apparaat. Gebruik MyPythonDevice zoals weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyPythonDevice
    ```

2. Voer de volgende opdrachten uit in Azure Cloud Shell om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

    **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyPythonDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet er ongeveer als volgt uit:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de snelstartgids.

3. U hebt ook een _service-verbindingsreeks_ nodig, zodat de back-end-toepassing verbinding kan maken met de IoT-hub en de berichten kan ophalen. Met de volgende opdracht haalt u de serviceverbindingsreeks voor uw IoT-hub op:

    **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub show-connection-string \
      --name YourIoTHubName \
      --output table
    ```

    Noteer de serviceverbindingsreeks. Deze ziet er ongeveer als volgt uit:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de snelstartgids. De verbindingsreeks voor de service is iets anders dan de verbindingsreeks voor het apparaat.

## <a name="listen-for-direct-method-calls"></a>Luisteren naar aanroepen van directe methoden

De toepassing voor het gesimuleerde apparaat maakt verbinding met een apparaatspecifiek eindpunt op uw IoT-hub, verstuurt gesimuleerde telemetrie en luistert naar aanroepen van directe methoden vanuit de hub. In deze snelstartgids geeft de aanroep van de directe methode vanuit de hub het apparaat opdracht om het interval voor het verzenden van telemetrie te wijzigen. Het gesimuleerde apparaat stuurt een bevestiging terug naar de hub nadat de directe methode is uitgevoerd.

1. Navigeer in een lokaal terminalvenster naar de hoofdmap van het voorbeeldproject in Python. Navigeer vervolgens naar de map **iot-hub\Quickstarts\simulated-device-2**.

1. Open het bestand **SimulatedDevice.py** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `CONNECTION_STRING` door de apparaatverbindingsreeks die u eerder hebt genoteerd. Sla ten slotte de wijzigingen in **SimulatedDevice.py** op.

1. Voer in het lokale terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren voor de toepassing voor het gesimuleerde apparaat:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Voer in het lokale terminalvenster de volgende opdrachten uit om de toepassing voor het gesimuleerde apparaat uit te voeren:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de toepassing voor het gesimuleerde apparaat telemetriegegevens naar uw IoT-hub verzendt:

    ![Het gesimuleerde apparaat uitvoeren](./media/quickstart-control-device-python/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>De directe methode aanroepen

De back-endtoepassing maakt verbinding met een eindpunt aan de servicezijde van uw IoT-hub. De toepassing verstuurt via uw IoT-hub aanroepen naar directe methoden op een apparaat en luistert naar bevestigingen. Een back-endtoepassing van IoT Hub wordt meestal in de cloud uitgevoerd.

1. Navigeer in een ander lokaal terminalvenster naar de hoofdmap van het voorbeeldproject in Python. Navigeer vervolgens naar de map **iot-hub\Quickstarts\back-end-application**.

1. Open het bestand **BackEndApplication.py** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `CONNECTION_STRING` door de service-verbindingsreeks die u eerder hebt genoteerd. Sla de wijzigingen in het bestand **BackEndApplication.py** ten slotte op.

1. Voer in het lokale terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren voor de toepassing voor het gesimuleerde apparaat:

    ```cmd/sh
    pip install azure-iothub-service-client future
    ```

1. Voer in het lokale terminalvenster de volgende opdrachten uit om de back-endtoepassing uit te voeren:

    ```cmd/sh
    python BackEndApplication.py
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de toepassing een directe methode op het apparaat aanroept en een bevestiging ontvangt:

    ![De back-endtoepassing uitvoeren](./media/quickstart-control-device-python/BackEndApplication.png)

    Nadat u de back-endtoepassing hebt uitgevoerd, ziet u een bericht in het consolevenster dat het gesimuleerde apparaat wordt uitgevoerd, en dat het interval voor het verzenden van berichten is gewijzigd:

    ![Wijziging in gesimuleerde client](./media/quickstart-control-device-python/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u vanuit een back-endtoepassing een directe methode op een apparaat aangeroepen en op de aanroep van de directe methode gereageerd in een toepassing voor een gesimuleerd apparaat.

Ga verder met de volgende zelfstudie als u wilt leren hoe u berichten van een apparaat naar andere bestemmingen in de cloud kunt routeren.

> [!div class="nextstepaction"]
> [Zelfstudie: Routeren van telemetriegegevens naar verschillende eindpunten voor verwerking](tutorial-routing.md)