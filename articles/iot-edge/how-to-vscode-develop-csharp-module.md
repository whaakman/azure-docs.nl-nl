---
title: Visual Studio Code gebruiken voor het ontwikkelen van een C#-module met Azure IoT rand | Microsoft Docs
description: Ontwikkelen en implementeren van een C#-module met Azure IoT rand in de Visual Studio Code zonder context overschakelen.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 48c6cacebdeb7505c8dc2bcaed099c33862589ac
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2018
---
# <a name="use-visual-studio-code-to-develop-a-c-module-with-azure-iot-edge"></a>Visual Studio Code gebruiken voor het ontwikkelen van een C#-module met Azure IoT rand
In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio Code](https://code.visualstudio.com/) als de belangrijkste ontwikkelprogramma te ontwikkelen en implementeren van uw Azure-IoT-Edge-modules. 

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat, of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

Voordat u deze richtlijnen, voert u de volgende zelfstudies:
- Azure IoT rand implementeren op een gesimuleerd apparaat in [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) of [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Ontwikkelen en implementeren van een Edge van C#-IoT-module voor uw gesimuleerde apparaat](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Hier volgt een controlelijst waarin de items dat moet u nadat u de voorgaande zelfstudies hebben:

- [Visual Studio Code](https://code.visualstudio.com/) 
- [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [C# voor uitbreiding van de Visual Studio Code (via OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core SDK 2.0](https://www.microsoft.com/net/core#windowscmd) 
- [Python 2.7](https://www.python.org/downloads/)
- [IoT-rand besturingselement script](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- Sjabloon AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Een actieve iothub met ten minste een Edge van de IoT-apparaat

Het is ook nuttig voor het installeren van [Docker-ondersteuning voor VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) beter uw module installatiekopieën en containers te beheren.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>Een Azure-IoT-Edge-module in VS-Code implementeren

### <a name="list-your-iot-hub-devices"></a>Lijst van uw IoT hub-apparaten
Er zijn twee manieren om weer te geven van uw IoT hub-apparaten in VS-Code. U kunt in beide gevallen om door te gaan.

#### <a name="sign-in-to-your-azure-account-in-vs-code-and-choose-your-iot-hub"></a>Aanmelden bij uw Azure-account in VS-Code en kies uw IoT-hub
1. Typ in het palet opdracht (F1 of Ctrl + Shift + P) en selecteer **Azure: aanmelden**. Selecteer vervolgens **Open & kopiëren**. (Ctrl + V) de code in uw browser plakken en selecteer **doorgaan**. Vervolgens kunt u aanmelden met uw Azure-account. Hier ziet u de accountgegevens van uw op de statusbalk VS-Code.
2. Typ in het palet opdracht en selecteer **IoT: Selecteer IoT Hub**. Selecteer eerst het abonnement waar u uw IoT-hub in de vorige zelfstudie gemaakt. Kies de IoT-hub die het apparaat aan de rand van de IoT bevat.

    ![Schermafbeelding van de lijst met apparaten](./media/how-to-vscode-develop-csharp-module/device-list.png)

#### <a name="set-the-iot-hub-connection-string"></a>De verbindingsreeks van de IoT-hub instellen
Typ in het palet opdracht en selecteer **IoT: verbindingsreeks van de IoT-Hub ingesteld**. Zorg ervoor dat u de verbindende tekenreeks onder beleid plakken **iothubowner**. (U kunt deze vinden in de beleidsregels voor gedeelde toegang van uw IoT-hub in de Azure portal.)
 
Hier ziet u de lijst met apparaten in IoT Hub-apparaten Explorer in de zijmarge aan de linkerkant.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>Start uw IoT-Edge-runtime en implementeren van een module
Installeren en starten van de rand van Azure IoT-runtime op uw apparaat. Implementeer een gesimuleerde sensor-module die telemetriegegevens naar Azure IoT Hub verzendt.
1. Selecteer in de opdracht palet **rand: Setup rand** en kies uw IoT-rand apparaat-ID. U kunt ook met de rechtermuisknop op de rand van de IoT-apparaat-ID in **lijst met apparaten**, en selecteer **Setup rand**.

    ![Schermafbeelding van de rand van de Setup-runtime](./media/how-to-vscode-develop-csharp-module/setup-edge.png)

2. Selecteer in het palet opdracht **rand: Start rand** starten van uw IoT-Edge-runtime. Hier ziet u bijbehorende uitvoer in de geïntegreerde terminal.

    ![Schermopname van Start rand runtime](./media/how-to-vscode-develop-csharp-module/start-edge.png)

3. Controleer de status van de runtime IoT rand in de Docker-Explorer. Groen betekent dat deze wordt uitgevoerd en uw IoT-Edge-runtime is gestart. Uw computer wordt nu een IoT-randapparaat simuleert.

    ![Schermopname van rand runtimestatus](./media/how-to-vscode-develop-csharp-module/edge-runtime.png)

4. Een sensor die van berichten naar uw IoT-randapparaat verzenden houdt simuleren. Typ in het palet opdracht en selecteer **rand: configuratiebestand genereren rand**. Selecteer een map voor dit bestand maken. Vervang de inhoud in de deployment.json-bestand dat wordt gegenereerd, `<registry>/<image>:<tag>` met `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`, en sla het bestand.

    ![Schermopname van sensor-module](./media/how-to-vscode-develop-csharp-module/sensor-module.png)

5. Selecteer **rand: implementatie voor randapparaat maken**, en kies de IoT-rand apparaat-ID voor het maken van een nieuwe implementatie. U kunt ook u kunt met de rechtermuisknop op de rand van de IoT-apparaat-ID in de lijst met apparaten en selecteer **implementatie creëert voor randapparaat**. 

6. U ziet uw IoT-rand uitgevoerd in de Docker-Explorer met de gesimuleerde sensor. Met de rechtermuisknop op de container in Docker Explorer. U kunt Docker-logboeken voor elke module bekijken. U kunt ook de modulelijst weergeven in de lijst met apparaten.

    ![Schermopname van Modulelijst](./media/how-to-vscode-develop-csharp-module/module-list.png)

7. Met de rechtermuisknop op uw apparaat-ID van IoT-rand en kunt u berichten in de VS Code D2C bewaken.
8. Typ om te stoppen uw IoT-Edge-runtime en de module sensor, en selecteer **rand: stoppen rand** in het palet opdracht.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Ontwikkelen en implementeren van een C#-module in VS-Code
In de zelfstudie [ontwikkelen van een C#-module](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), bijwerken, maken en publiceren van uw installatiekopie module in VS-Code. Vervolgens gaat u naar de Azure-portal voor het implementeren van uw C#-module. Deze sectie wordt het gebruik van de VS Code implementeren en controleren van uw C#-module.

### <a name="start-a-local-docker-registry"></a>Start een lokale Docker-register
Voor dit artikel kunt u een register Docker-compatibel. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Deze sectie wordt een [lokale Docker-register](https://docs.docker.com/registry/deploying/), die voor het testen van tijdens het ontwikkelen van uw eerdere eenvoudiger is.
In de VS-Code **geïntegreerde terminal** (Ctrl + '), voer de volgende opdracht om te starten van een lokale register:  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Dit voorbeeld toont de Register-configuraties die alleen geschikt voor het testen zijn. Een register gereed is voor productie moet worden beveiligd door TLS, en een mechanisme voor toegangsbeheer in het ideale geval moet gebruiken. We raden u aan [Azure Container register](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) voor het implementeren van de rand van de IoT-modules gereed is voor productie.

### <a name="create-an-iot-edge-module-project"></a>Een IoT-rand module-project maken
De volgende stappen ziet u het maken van een IoT-Edge-module op basis van .NET Core 2.0 met behulp van Visual Studio Code en de extensie Azure IoT rand. Als u deze sectie hebt voltooid in de vorige zelfstudie, kunt u veilig in deze sectie overslaan.
1. Selecteer in Visual Studio Code **weergave** > **geïntegreerde Terminal** openen van de VS Code geïntegreerde terminal.
3. Voer de volgende opdracht om te installeren (of werk) in de geïntegreerde terminal de **AzureIoTEdgeModule** sjabloon in dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Maak een project voor de nieuwe module. De volgende opdracht maakt u de projectmap **FilterModule**, in de huidige map:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```
 
3. Selecteer **bestand** > **Open map**.
4. Blader naar de **FilterModule** map en klik op **map selecteren** openen van het project in VS-Code.
5. Selecteer in Verkenner Code tegenover **Program.cs** om dit te openen. Aan de bovenkant van **program.cs**, omvatten de volgende naamruimten:
   ```csharp
   using Microsoft.Azure.Devices.Shared;
   using System.Collections.Generic;  
   using Newtonsoft.Json;
   ```

6. Voeg de `temperatureThreshold` variabele de **programma** klasse. Deze variabele bepaalt de waarde die de gemeten temperatuur moet groter zijn dan om de gegevens worden verzonden naar IoT Hub. 

   ```csharp
   static int temperatureThreshold { get; set; } = 25;
   ```

7. Voeg de `MessageBody`, `Machine`, en `Ambient` klassen voor de **programma** klasse. Deze klassen definiëren het verwachte schema voor de hoofdtekst van binnenkomende berichten.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
    ```

8. In de **Init** methode, de code maakt en configureert u een **DeviceClient** object. Dit object kunt de module die u wilt verbinding maken met de lokale runtime IoT rand berichten te verzenden en ontvangen. De rand van de IoT-runtime wordt verstrekt aan de module voor de verbindingsreeks wordt gebruikt in de **Init** methode. Na het maken van de **DeviceClient** object, de code registreert een retouraanroep voor het ontvangen van berichten van de rand van de IoT-hub via de **input1** eindpunt. Vervang de `SetInputMessageHandlerAsync` methode met een nieuwe, en voeg een `SetDesiredPropertyUpdateCallbackAsync` methode voor het gewenste eigenschappen updates. Als u deze wijziging, vervangt u de laatste regel van de **Init** methode met de volgende code:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Voeg de `onDesiredPropertiesUpdate` methode voor de **programma** klasse. Deze methode ontvangt updates op de gewenste eigenschappen van de module-twin en werkt de **temperatureThreshold** variabele moet worden gezocht. Alle modules hebben hun eigen module twin, kunt u de code die wordt uitgevoerd in een module rechtstreeks vanuit de cloud.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

10. Vervang de `PipeMessage` methode met de `FilterMessages` methode. Deze methode wordt aangeroepen wanneer de module een bericht van de rand IoT-Hub ontvangt. Deze berichten waarin wordt aangegeven temperaturen lager dan de drempelwaarde temperatuur is ingesteld via de module-twin gefilterd. Voegt ook de **MessageType** eigenschap aan het bericht met de waarde die is ingesteld op **waarschuwing**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        int counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            byte[] messageBytes = message.GetBytes();
            string messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get message body
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await deviceClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Het project bouwen, met de rechtermuisknop op de **FilterModule.csproj** bestand in Verkenner en selecteer **bouwen IoT rand module**. Dit proces compileert de module en exporteert het binaire bestand en de bijbehorende afhankelijkheden in een map die wordt gebruikt om een Docker-installatiekopie te maken. 

    ![Schermafbeelding van de VS Code Explorer](./media/how-to-vscode-develop-csharp-module/build-module.png)

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Vouw in VS Code Explorer de **Docker** map. Vouw vervolgens de map voor uw containerplatform uit, **linux-x64** of **windows-nano**.
2. Met de rechtermuisknop op de **Dockerfile** bestand en selecteer **bouwen IoT rand module Docker installatiekopie**. 

    ![Schermafbeelding van de VS Code Explorer](./media/how-to-vscode-develop-csharp-module/build-docker-image.png)

3. In de **map selecteren** venster ofwel om te zoeken of invoeren `./bin/Debug/netcoreapp2.0/publish`. Selecteer **map selecteren als EXE_DIR**.
4. Typ in het pop-uptekstvak boven aan het VS Code-venster de naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filtermodule:latest`. Als u naar het lokale register implementeert, moet dit worden `localhost:5000/filtermodule:latest`.
5. Push de installatiekopie naar uw Docker-opslagplaats. Gebruik de **rand: Push IoT rand module Docker installatiekopie** opdracht en de afbeeldings-URL in het pop-tekst boven aan het venster VS-Code invoeren. Gebruik dezelfde afbeeldings-URL die u hebt gebruikt in de vorige stap. Controleer het consolelogboek om te controleren of dat de installatiekopie van het met succes is gepusht.

    ![Schermafbeelding van de installatiekopie van het Docker pushen](./media/how-to-vscode-develop-csharp-module/push-image.png) ![schermafbeelding van de consolelogboek](./media/how-to-vscode-develop-csharp-module/pushed-image.png)

### <a name="deploy-your-iot-edge-modules"></a>Implementeren van uw IoT-Edge-modules

1. Open de `deployment.json` bestand en vervang de **modules** sectie met de volgende stappen:
    ```json
    "tempSensor": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
            "createOptions": ""
        }
    },
    "filtermodule": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "localhost:5000/filtermodule:latest",
            "createOptions": ""
        }
    }
    ```

2. Vervang de **routes** sectie met de volgende stappen:
    ```json
    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
    ```
   > [!NOTE]
   > Declaratieve regels in de runtime definiëren waar deze berichten stromen. In dit artikel moet u twee routes. Met de eerste route worden berichten van de temperatuursensor naar de filtermodule getransporteerd via het eindpunt 'input1'. Dit is het eindpunt dat u met de handler FilterMessages geconfigureerd. Met de tweede route worden berichten van de filtermodule naar IoT Hub getransporteerd. In deze route upstream is een speciale bestemming waarin wordt uitgelegd IoT-Hub rand om berichten te verzenden naar IoT Hub.

3. Sla dit bestand.
4. Selecteer in het palet opdracht **rand: implementatie voor randapparaat maken**. Selecteer vervolgens uw IoT-rand apparaat-ID voor het maken van een implementatie. Of, met de rechtermuisknop op het apparaat-ID in de lijst met apparaten en selecteer **implementatie creëert voor randapparaat**.

    ![Implementatieoptie schermopname maken](./media/how-to-vscode-develop-csharp-module/create-deployment.png)

5. Selecteer de `deployment.json` u bijgewerkt. In het venster output ziet u bijbehorende uitvoer voor uw implementatie.

    ![Schermopname van het venster output](./media/how-to-vscode-develop-csharp-module/deployment-succeeded.png)

6. Start uw IoT-Edge-runtime in het palet opdracht (Selecteer **rand: Start rand**).
7. U kunt uw IoT-rand runtime start uitgevoerd in de Docker-Explorer met de gesimuleerde sensor en filtermodule zien.

    ![Schermopname van Docker Explorer](./media/how-to-vscode-develop-csharp-module/solution-running.png)

8. Met de rechtermuisknop op uw apparaat-ID van IoT-rand en kunt u berichten in de VS Code D2C bewaken.


## <a name="next-steps"></a>Volgende stappen

[Fouten opsporen in C#-module in VS-Code](how-to-vscode-debug-csharp-module.md)
