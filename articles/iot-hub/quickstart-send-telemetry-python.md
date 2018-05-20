---
title: 'Snelstartgids: Telemetrie verzenden naar Azure IoT Hub (Python) | Microsoft Docs'
description: In deze snelstartgids voert u een Python-voorbeeldtoepassing uit om gesimuleerde telemetrie te verzenden naar een IoT-hub en gebruikt u vervolgens een hulpprogramma om telemetrie uit de IoT-hub te lezen.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: 2abc978d6f40808a1bea46a01647444bb79b1211
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-python"></a>Snelstartgids: Telemetrie vanaf een apparaat verzenden naar een IoT-hub en de telemetrie lezen uit de hub met een back-endtoepassing (Python)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub is een Azure-service waarmee grote hoeveelheden telemetrie van uw IoT-apparaten naar de cloud kunt opnemen voor opslag of verwerking. In deze snelstartgids verzendt u telemetrie vanuit een toepassing voor een gesimuleerd apparaat via IoT Hub naar een back-endtoepassing voor verwerking.

In dit artikel wordt een vooraf geschreven Python-toepassing gebruikt om de telemetrie te verzenden en een CLI-hulpprogramma om de telemetrie uit IoT Hub te lezen. Voordat u deze twee toepassingen kunt uitvoeren, moet u een IoT-hub maken en een apparaat registreren bij de hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De twee voorbeeldtoepassingen die u uitvoert in deze snelstartgids zijn geschreven in Python. Python 2.7.x of 3.5.x moet beschikbaar zijn op uw ontwikkelcomputer.

U kunt Python voor meerdere platforms downloaden van [Python.org](https://www.python.org/downloads/).

Gebruik een van de volgende opdrachten om te controleren wat de huidige versie van Python op uw ontwikkelcomputer is:

```python
python --version
```

```python
python3 --version
```

Download het Python-voorbeeldproject van https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip en pak het ZIP-archief uit.

Om het CLI-hulpprogramma te installeren dat de telemetriegegevens van de IoT-hub moet lezen, moet u eerst Node.js v4.x.x of hoger installeren op uw ontwikkelcomputer. U kunt Node.js voor meerdere platforms downloaden van [nodejs.org](https://nodejs.org).

Gebruik de volgende opdracht om de huidige versie van Node.js op uw ontwikkelcomputer te controleren:

```cmd/sh
node --version
```

Voer de volgende opdracht uit om het CLI-hulpprogramma `iothub-explorer` te installeren:

```cmd/sh
npm install -g iothub-explorer
```

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u de Azure-CLI om een gesimuleerd apparaat te registreren.

1. Voeg de IoT Hub CLI-extensie toe en maak de apparaatidentiteit. Vervang `{YourIoTHubName}` door de naam die u hebt gekozen voor uw IoT-hub:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyPythonDevice
    ```

    Als u een andere naam voor het apparaat kiest, werkt u de apparaatnaam bij in de voorbeeldtoepassing voordat u deze uitvoert.

1. Voer de volgende opdracht uit om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyPythonDevice --output table
    ```

    Noteer de apparaatverbindingsreeks, die er ongeveer zo uitziet: `Hostname=...=`. U gebruikt deze waarde verderop in de snelstartgids.

1. U hebt ook een _service-verbindingsreeks_ nodig, zodat het CLI-hulpprogramma `iothub-explorer` verbinding kan maken met de IoT-hub en de berichten kan ophalen. Met de volgende opdracht haalt u de serviceverbindingsreeks voor uw IoT-hub op:

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

    Noteer de serviceverbindingsreeks, die er ongeveer zo uitziet: `Hostname=...=`. U gebruikt deze waarde verderop in de snelstartgids. De verbindingsreeks voor de service is iets anders dan de verbindingsreeks voor het apparaat.

## <a name="send-simulated-telemetry"></a>Gesimuleerde telemetrie verzenden

De toepassing voor het gesimuleerde apparaat maakt verbinding met een apparaatspecifiek eindpunt op uw IoT-hub en verstuurt gesimuleerde telemetrie over temperatuur en luchtvochtigheid.

1. Navigeer in een terminalvenster naar de hoofdmap van het voorbeeldproject in Python. Navigeer vervolgens naar de map **Quickstarts\simulated-device**.

1. Open het bestand **SimulatedDevice.py** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `CONNECTION_STRING` door de apparaatverbindingsreeks die u eerder hebt genoteerd. Sla ten slotte de wijzigingen in **SimulatedDevice.py** op.

1. Voer in het terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren voor de toepassing voor het gesimuleerde apparaat:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Voer in het terminalvenster de volgende opdrachten uit om de toepassing voor het gesimuleerde apparaat uit te voeren:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de toepassing voor het gesimuleerde apparaat telemetriegegevens naar uw IoT-hub verzendt:

    ![Het gesimuleerde apparaat uitvoeren](media/quickstart-send-telemetry-python/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>De telemetrie van uw hub lezen

Het `iothub-explorer` CLI-hulpprogramma maakt verbinding met het servicezijde-eindpunt **Gebeurtenissen** op uw IoT-hub. Het hulpprogramma ontvangt de berichten die van het gesimuleerde apparaat naar de cloud worden verzonden. Een back-endtoepassing van IoT Hub wordt meestal uitgevoerd in de cloud om berichten van apparaat naar cloud te ontvangen en verwerken.

Voer in een ander terminalvenster de volgende opdrachten uit, waarbij u `{your hub service connection string}` vervangt door de verbindingsreeks van de service die u eerder hebt genoteerd:

```cmd/sh
iothub-explorer monitor-events MyPythonDevice --login {your hub service connection string}
```

In de volgende schermafbeelding ziet u de uitvoer op het moment dat het hulpprogramma telemetriegegevens ontvangt die door het gesimuleerde apparaat naar de hub zijn verzonden:

![De back-endtoepassing uitvoeren](media/quickstart-send-telemetry-python/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent om de volgende snelstartgids uit te voeren, hebt u de resourcegroep en IoT-hub nog nodig.

Als u de IoT-hub niet langer nodig hebt, verwijdert u deze en de resourcegroep in de portal. Dit doet u door de resourcegroep **qs-iot-hub-rg** met de IoT-hub te selecteren en op **Verwijderen** te klikken.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een IoT-hub geconfigureerd, een apparaat geregistreerd, gesimuleerde telemetrie verzonden naar de hub met behulp van een Python-toepassing en de telemetrie weer gelezen van de hub met behulp van een eenvoudig hulpprogramma.

Ga verder met de volgende snelstartgids als u wilt weten hoe u een gesimuleerd apparaat beheert vanuit een back-endtoepassing.

> [!div class="nextstepaction"]
> [Snelstartgids: Een apparaat beheren dat is verbonden met een IoT-hub](quickstart-control-device-python.md)