---
title: Visual Studio Code gebruiken voor het ontwikkelen van een C#-module met Azure IoT rand | Microsoft Docs
description: Ontwikkelen en implementeren van een C#-module met Azure IoT rand in de VS Code zonder context overschakelen
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: cad28b4e6d4e46058641da19795cd71efdbd0c92
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-develop-c-module-with-azure-iot-edge"></a>Visual Studio Code gebruiken voor het ontwikkelen van C#-module met Azure IoT rand
In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio Code](https://code.visualstudio.com/) als de belangrijkste ontwikkelprogramma te ontwikkelen en implementeren van uw IoT-Edge-modules. 

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

Zorg ervoor dat u hebt voltooid volgende zelfstudies voordat u deze richtlijnen.
- Azure IoT rand implementeren op een gesimuleerd apparaat in [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) of [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Ontwikkelen en implementeren van een Edge van C#-IoT-module voor uw gesimuleerde apparaat](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Hier volgt een controlelijst waarin de items dat moet u nadat u klaar bent met het voorgaande zelfstudies hebben.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# voor Visual Studio Code (via OmniSharp)-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [IoT-rand besturingselement script](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- Sjabloon AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Een actieve iothub met ten minste een Edge van de IoT-apparaat.

Ook het beste installeren [Docker-ondersteuning voor VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) beter uw module installatiekopieën en containers te beheren.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>Een Azure-IoT-Edge-module in VS-Code implementeren

### <a name="list-your-iot-hub-devices"></a>Lijst van uw IoT hub-apparaten
Er zijn twee manieren om weer te geven van uw IoT hub-apparaten in uw Code VS. U kunt in beide gevallen om door te gaan.

#### <a name="sign-in-your-azure-account-in-vscode-and-choose-your-iot-hub"></a>Meld u aan uw Azure-account in VSCode en kies uw IoT-hub
1. Typ in de opdracht palet (F1 of Ctrl + Shift + P), en selecteer **Azure: aanmelden**. Klik vervolgens op  **kopie* & Open** in het pop-upvenster. (Ctrl + V) de code in uw browser plakken en klik op de knop Doorgaan. Meld u aan met uw Azure-account. Hier ziet u de accountgegevens van uw op de statusbalk VS-Code.
2. Typ in de opdracht palet (F1 of Ctrl + Shift + P), en selecteer **IoT: Selecteer IoT Hub**. Het abonnement waarbij u uw IoT-hub in vorige zelfstudie gemaakt voor het eerst wordt selecteren. Kies vervolgens de IoT-hub die het apparaat aan de rand van de IoT bevat.

    ![Lijst met apparaten](./media/how-to-vscode-develop-csharp-module/device-list.png)

#### <a name="set-iot-hub-connection-string"></a>IoT hub-verbindingsreeks instellen
Typ in de opdracht palet (F1 of Ctrl + Shift + P), en selecteer **IoT: verbindingsreeks van de IoT-Hub ingesteld**. Zorg ervoor dat u de verbindende tekenreeks onder beleid plakken **iothubowner** (u kunt deze vinden in een gedeeld toegangsbeleid van uw IoT-hub in Azure-portal).
 
Hier ziet u de lijst met apparaten in IoT Hub-apparaten Explorer in het linkerdeelvenster zijmarge.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>Start uw IoT-Edge-runtime en implementeren van een module
Installeren en starten van de rand van Azure IoT-runtime op uw apparaat. En implementeren van een gesimuleerde sensor-module die telemetriegegevens naar IoT Hub verzendt.
1. Selecteer in de opdracht palet **rand: Setup rand** en kies uw IoT-rand apparaat-ID. Of met de rechtermuisknop op het apparaat-ID van de rand in de lijst met apparaten en selecteer **Setup rand**.

    ![Setup rand runtime](./media/how-to-vscode-develop-csharp-module/setup-edge.png)

2. Selecteer in de opdracht palet **rand: Start rand** starten van de Edge-runtime. U ziet de bijbehorende uitvoer in de geïntegreerde terminal.

    ![Starten van de rand runtime](./media/how-to-vscode-develop-csharp-module/start-edge.png)

3. Controleer de status van de rand runtime in Docker explorer. Groen betekent dat deze wordt uitgevoerd. Uw IoT-Edge-runtime is gestart.

    ![Rand runtime wordt uitgevoerd](./media/how-to-vscode-develop-csharp-module/edge-runtime.png)

4. Nu uw runtime rand wordt uitgevoerd, simuleert wat betekent dat de PC nu een randapparaat. Volgende stap is om te simuleren een sensorthing houdt berichten verzenden naar het apparaat aan de rand. Typ in het kleurenpalet van de opdracht, en selecteer **rand: configuratiebestand genereren rand**. En selecteer een map voor dit bestand maken. Vervang de inhoud in het bestand gegenereerde deployment.json `<registry>/<image>:<tag>` met `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` en sla het bestand.

    ![Module temperatuursensor](./media/how-to-vscode-develop-csharp-module/sensor-module.png)

5. Selecteer **rand: implementatie voor randapparaat maken** en kies de apparaat-ID van de rand om een nieuwe implementatie te maken. U kunt met de rechtermuisknop op het apparaat-ID van de rand in de lijst met apparaten en selecteer **implementatie creëert voor randapparaat**. 

6. U ziet uw IoT-rand uitgevoerd in de Docker-explorer met de gesimuleerde sensor. Met de rechtermuisknop op de container in Docker explorer. U kunt docker-logboeken voor elke module bekijken. U kunt ook de modulelijst weergeven in de lijst met apparaten.

    ![Modulelijst](./media/how-to-vscode-develop-csharp-module/module-list.png)

7. Met de rechtermuisknop op uw apparaat-ID van rand en kunt u berichten in de VS Code D2C bewaken.
8. Om te stoppen uw IoT-Edge-runtime en de module sensor, kunt u typt en selecteer **rand: stoppen rand** in opdracht palet.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Ontwikkelen en implementeren van een C#-module in VS-Code
In de zelfstudie [ontwikkelen van een C#-module](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), u bijwerkt, maken, en publiceren van uw installatiekopie module in VS-Code en gaat vervolgens naar Azure-portal voor het implementeren van uw C#-module. Deze sectie wordt het gebruik van de VS Code implementeren en controleren van uw C#-module.

### <a name="start-a-local-docker-registry"></a>Start een lokale docker-register
Voor deze zelfstudie kunt u een register Docker-compatibel. Twee populaire Docker Registerservices beschikbaar in de cloud zijn [Azure Container register](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Deze sectie wordt een [lokale Docker-register](https://docs.docker.com/registry/deploying/), die is het eenvoudiger voor doel tijdens het ontwikkelen van uw vroege testdoeleinden.
In de VS Code **geïntegreerde terminal**(Ctrl + '), de volgende opdrachten voor het starten van een lokale register uitvoeren.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Hierboven voorbeeld toont register-configuraties die alleen geschikt voor het testen zijn. Een register gereed is voor productie moet worden beveiligd door TLS en moet in het ideale geval een access control-mechanisme gebruiken. We raden u aan [Azure Container register](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) voor het implementeren van de rand van de IoT-modules gereed is voor productie.

### <a name="create-an-iot-edge-module-project"></a>Een IoT-rand module-project maken
De volgende stappen laten zien u hoe u een IoT-Edge-module maken die is gebaseerd op .NET core 2.0 met behulp van Visual Studio Code en de extensie Azure IoT rand. Als u deze sectie hebt voltooid in de vorige zelfstudie, kunt u veilig in deze sectie overslaan.
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
5. Klik in Verkenner VS-Code op **Program.cs** om dit te openen. Aan de bovenkant van **program.cs**, omvatten onder naamruimten.
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

8. In de **Init** methode, de code maakt en configureert u een **DeviceClient** object. Dit object kunt de module die u wilt verbinding maken met de lokale Azure IoT Edge-runtime voor het verzenden en ontvangen van berichten. De verbindingsreeks die wordt gebruikt in de **Init** methode voor de module wordt verstrekt door de rand van de IoT-runtime. Na het maken van de **DeviceClient**, de code registreert een retouraanroep voor het ontvangen van berichten van de rand van de IoT-hub via de **input1** eindpunt. Vervang de `SetInputMessageHandlerAsync` methode met een nieuwe, en voeg een `SetDesiredPropertyUpdateCallbackAsync` methode voor het gewenste eigenschappen updates. Als u deze wijziging, vervangt u de laatste regel van de **Init** methode met de volgende code:

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

10. Vervang de `PipeMessage` methode met de `FilterMessages` methode. Deze methode wordt aangeroepen wanneer de module een bericht van de rand van de IoT-hub ontvangt. Deze berichten waarin wordt aangegeven temperaturen lager dan de drempelwaarde temperatuur is ingesteld via de module-twin gefilterd. Voegt ook de **MessageType** eigenschap aan het bericht met de waarde die is ingesteld op **waarschuwing**. 

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

11. Het project bouwen, met de rechtermuisknop op de **FilterModule.csproj** bestand in Verkenner en klik op **bouwen IoT rand module**. Dit proces wordt gecompileerd van de module en exporteert het binaire bestand en de bijbehorende afhankelijkheden in een map die wordt gebruikt om een Docker-installatiekopie te maken. 

    ![Module maken](./media/how-to-vscode-develop-csharp-module/build-module.png)

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Vouw in VS-Code explorer de **Docker** map. Vouw vervolgens ofwel de map voor uw platform container **linux x64** of **windows nano**.
2. Met de rechtermuisknop op de **Dockerfile** -bestand en klik op **bouwen IoT rand module Docker installatiekopie**. 

    ![Een docker-installatiekopie maken](./media/how-to-vscode-develop-csharp-module/build-docker-image.png)

3. In de **map selecteren** venster ofwel om te zoeken of invoeren `./bin/Debug/netcoreapp2.0/publish`. Klik op **map selecteren als EXE_DIR**.
4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filtermodule:latest`. Als u naar het lokale register implementeert, moet dit worden `localhost:5000/filtermodule:latest`.
5. De installatiekopie naar uw opslagplaats Docker forceren. Gebruik de **rand: Push IoT rand module Docker installatiekopie** opdracht en de afbeeldings-URL in het pop-tekst boven aan het venster VS-Code invoeren. Gebruik dezelfde afbeeldings-URL die u in bovenstaande stap gebruikt. Raadpleeg het consolelogboek voor de en controleer of dat de installatiekopie van het met succes is gepusht.

    ![Docker-afbeelding push](./media/how-to-vscode-develop-csharp-module/push-image.png) ![Pushed docker-afbeelding](./media/how-to-vscode-develop-csharp-module/pushed-image.png)

### <a name="deploy-your-iot-edge-modules"></a>Implementeren van uw IoT-Edge-modules

1. Open de `deployment.json` bestand, vervangt u **modules** sectie met de onderstaande inhoud:
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

2. Vervang de **routes** sectie met de onderstaande inhoud:
    ```json
    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
    ```
   > [!NOTE]
   > Declaratieve regels in de runtime definiëren waar deze berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor naar de filtermodule via het eindpunt 'input1', dat het eindpunt dat u met de handler FilterMessages geconfigureerd is. De tweede route transporten berichten van de module expressiefilter IoT-hub. In deze route upstream is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub.

3. Sla dit bestand.
4. Selecteer in de opdracht palet **rand: implementatie voor randapparaat maken**. Selecteer vervolgens uw IoT-rand apparaat-ID voor het maken van een implementatie. Of met de rechtermuisknop op het apparaat-ID in de lijst met apparaten en selecteer **implementatie creëert voor randapparaat**.

    ![Implementatie maken](./media/how-to-vscode-develop-csharp-module/create-deployment.png)

5. Selecteer de `deployment.json` u bijgewerkt. In het venster output ziet u bijbehorende uitvoer voor uw implementatie.

    ![Implementatie is voltooid](./media/how-to-vscode-develop-csharp-module/deployment-succeeded.png)

6. Start uw runtime rand in de opdracht palet. **Rand: Begin rand**
7. U kunt uw IoT-rand runtime start uitgevoerd in de Docker-explorer met de gesimuleerde sensor en filtermodule zien.

    ![Rand van de IoT-oplossing wordt uitgevoerd](./media/how-to-vscode-develop-csharp-module/solution-running.png)

8. Met de rechtermuisknop op uw apparaat-ID van rand en kunt u berichten in de VS Code D2C bewaken.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie een IoT-Edge-module gemaakt en geïnstalleerd op de rand van de IoT-apparaat in VS-Code. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere scenario's bij het ontwikkelen van Azure IoT rand in de VS-Code.

> [!div class="nextstepaction"]
> [Fouten opsporen in C#-module in VS-Code](how-to-vscode-debug-csharp-module.md)
