---
title: 'Snelstartgids: Een apparaat beheren vanuit Azure IoT Hub (Java) | Microsoft Docs'
description: In deze snelstartgids gaan we twee in Java geschreven voorbeeldtoepassingen uitvoeren. De ene toepassing is een back-endtoepassing waarmee u op afstand apparaten kunt beheren die zijn verbonden met uw hub. De andere toepassing simuleert een apparaat dat is verbonden met uw hub en dat op afstand kan worden beheerd.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/22/2018
ms.author: dobett
ms.openlocfilehash: 5da4248f0b0a72c3614b4c3e5ea042c4341f4e03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36330540"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-java"></a>Snelstartgids: Een apparaat beheren dat is verbonden met een IoT-hub (Java)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub is een Azure-service waarmee u grote hoeveelheden telemetrie van uw IoT-apparaten kunt overbrengen naar de cloud en uw apparaten kunt beheren vanuit de cloud. In deze snelstartgids gebruikt u een *directe methode* om een gesimuleerd apparaat te beheren dat met uw IoT-hub is verbonden. U kunt directe methoden gebruiken om het gedrag van een apparaat dat is verbonden met uw IoT-hub, op afstand te wijzigen.

In de snelstartgids worden twee vooraf geschreven Java-toepassingen gebruikt:

* Een toepassing voor een gesimuleerd apparaat die reageert op de directe methoden die worden aangeroepen vanuit een back-endtoepassing. Om de aanroepen van de directe methoden te kunnen ontvangen, maakt deze toepassing verbinding met een apparaatspecifiek eindpunt op uw IoT-hub.
* Een back-endtoepassing die de directe methoden op het gesimuleerde apparaat aanroept. Als u een directe methode op een apparaat wilt aanroepen, maakt u met deze toepassing verbinding met een eindpunt aan de servicezijde van uw IoT-hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De twee voorbeeldtoepassingen die u uitvoert in deze snelstartgids zijn geschreven in Java. Java SE 8 of hoger moet zijn geïnstalleerd op uw ontwikkelcomputer.

U kunt Java voor meerdere platforms downloaden van [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

Gebruik de volgende opdracht om de huidige versie van Java op uw ontwikkelcomputer te controleren:

```cmd/sh
java --version
```

U moet Maven 3 installeren om de voorbeelden te kunnen compileren. U kunt Maven voor meerdere platforms downloaden van [Apache Maven](https://maven.apache.org/download.cgi).

Gebruik de volgende opdracht om de huidige versie van Maven op uw ontwikkelcomputer te controleren:

```cmd/sh
mvn --version
```

Als u dit nog niet hebt gedaan, downloadt u het voorbeeldproject met Java van https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip en pakt u het ZIP-archief uit.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

U kunt deze stap overslaan na het afronden van de voorgaande [ snelstartgids: Telemetriegegevens vanaf een apparaat verzenden naar een IoT-hub](quickstart-send-telemetry-java.md).

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

U kunt deze stap overslaan na het afronden van de voorgaande [ snelstartgids: Telemetriegegevens vanaf een apparaat verzenden naar een IoT-hub](quickstart-send-telemetry-java.md).

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u de Azure-CLI om een gesimuleerd apparaat te registreren.

1. Voeg de IoT Hub CLI-extensie toe en maak de apparaatidentiteit. Vervang `{YourIoTHubName}` door de naam die u hebt gekozen voor uw IoT-hub:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyJavaDevice
    ```

    Als u een andere naam voor het apparaat kiest, werkt u de apparaatnaam bij in de voorbeeldtoepassingen voordat u ze uitvoert.

2. Voer de volgende opdracht uit om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyJavaDevice --output table
    ```

    Noteer de apparaatverbindingsreeks, die er ongeveer zo uitziet: `Hostname=...=`. U gebruikt deze waarde verderop in de snelstartgids.

## <a name="retrieve-the-service-connection-string"></a>De verbindingsreeks voor de service ophalen

U hebt ook de _serviceverbindingsreeks_ voor de IoT-hub nodig, zodat de back-endtoepassing verbinding kan maken met de hub en de berichten kan ophalen. Met de volgende opdracht haalt u de serviceverbindingsreeks voor uw IoT-hub op:

```azurecli-interactive
az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
```

Noteer de serviceverbindingsreeks, die er ongeveer zo uitziet: `Hostname=...=`. U gebruikt deze waarde verderop in de snelstartgids.

## <a name="listen-for-direct-method-calls"></a>Luisteren naar aanroepen van directe methoden

De toepassing voor het gesimuleerde apparaat maakt verbinding met een apparaatspecifiek eindpunt op uw IoT-hub, verstuurt gesimuleerde telemetrie en luistert naar aanroepen van directe methoden vanuit de hub. In deze snelstartgids geeft de aanroep van de directe methode vanuit de hub het apparaat opdracht om het interval voor het verzenden van telemetrie te wijzigen. Het gesimuleerde apparaat stuurt een bevestiging terug naar de hub nadat de directe methode is uitgevoerd.

1. Navigeer in een terminalvenster naar de hoofdmap van het voorbeeldproject in Java. Navigeer vervolgens naar de map **iot-hub\Quickstarts\simulated-device-2**.

2. Open het bestand **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `connString` door de apparaatverbindingsreeks die u eerder hebt genoteerd. Sla daarna de wijzigingen in het bestand **SimulatedDevice.java** op.

3. Voer in het terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren en de toepassing voor het gesimuleerde apparaat te compileren:

    ```cmd/sh
    mvn clean package
    ```

4. Voer in het terminalvenster de volgende opdrachten uit om de toepassing voor het gesimuleerde apparaat uit te voeren:

    ```cmd/sh
    java -jar target/simulated-device-2-1.0.0-with-deps.jar
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de toepassing voor het gesimuleerde apparaat telemetriegegevens naar uw IoT-hub verzendt:

    ![Het gesimuleerde apparaat uitvoeren](media/quickstart-control-device-java/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>De directe methode aanroepen

De back-endtoepassing maakt verbinding met een eindpunt aan de servicezijde van uw IoT-hub. De toepassing verstuurt via uw IoT-hub aanroepen naar directe methoden op een apparaat en luistert naar bevestigingen. Een back-endtoepassing van IoT Hub wordt meestal in de cloud uitgevoerd.

1. Navigeer in een ander terminalvenster naar de hoofdmap van het voorbeeldproject in Java. Navigeer vervolgens naar de map **iot-hub\Quickstarts\back-end-application**.

2. Open het bestand **src/main/java/com/microsoft/docs/iothub/samples/BackEndApplication.java** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `iotHubConnectionString` door de serviceverbindingsreeks die u eerder hebt genoteerd. Sla daarna de wijzigingen in het bestand **BackEndApplication.java** op.

3. Voer in het terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren en de back-endtoepassing te compileren:

    ```cmd/sh
    mvn clean package
    ```

4. Voer in het terminalvenster de volgende opdrachten uit om de back-endtoepassing uit te voeren:

    ```cmd/sh
    java -jar target/back-end-application-1.0.0-with-deps.jar
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de toepassing een directe methode op het apparaat aanroept en een bevestiging ontvangt:

    ![De back-endtoepassing uitvoeren](media/quickstart-control-device-java/BackEndApplication.png)

    Nadat u de back-endtoepassing hebt uitgevoerd, ziet u een bericht in het consolevenster dat het gesimuleerde apparaat wordt uitgevoerd, en dat het interval voor het verzenden van berichten is gewijzigd:

    ![Wijziging in gesimuleerde client](media/quickstart-control-device-java/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u vanuit een back-endtoepassing een directe methode op een apparaat aangeroepen en op de aanroep van de directe methode gereageerd in een toepassing voor een gesimuleerd apparaat.

Ga verder met de volgende zelfstudie als u wilt leren hoe u berichten van een apparaat naar andere bestemmingen in de cloud kunt routeren.

> [!div class="nextstepaction"]
> [Zelfstudie: Routeren van telemetriegegevens naar verschillende eindpunten voor verwerking](tutorial-routing.md)