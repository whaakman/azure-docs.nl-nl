---
title: Gebruik Visual Studio Code te ontwikkelen en implementeren van Azure Functions voor Azure IoT rand | Microsoft Docs
description: Ontwikkelen en implementeren van een C# Azure werkt in combinatie met Azure IoT rand in de VS Code zonder context overschakelen
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 219474a4577a76f5ceb9a9efaa3c349d633de047
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-visual-studio-code-to-develop-and-deploy-azure-functions-to-azure-iot-edge"></a>Visual Studio Code te ontwikkelen en implementeren van Azure Functions voor Azure IoT Edge gebruiken

In dit artikel vindt u gedetailleerde instructies voor het gebruik van [Visual Studio Code](https://code.visualstudio.com/) als de belangrijkste ontwikkelprogramma te ontwikkelen en implementeren van Azure Functions op IoT rand. 

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u gebruikmaakt van een computer of virtuele machine met Windows of Linux als uw ontwikkelcomputer. Uw IoT-randapparaat kan een andere fysieke apparaat of kunt u uw IoT-randapparaat simuleren op uw ontwikkelcomputer.

Zorg ervoor dat u hebt voltooid volgende zelfstudies voordat u deze richtlijnen.
- Azure IoT rand implementeren op een gesimuleerd apparaat in [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) of [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Azure Functions implementeren](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function)

Hier volgt een controlelijst waarin de items dat moet u nadat u klaar bent met het voorgaande zelfstudies hebben.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# voor Visual Studio Code (via OmniSharp)-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [IoT-rand besturingselement script](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- Sjabloon AzureIoTEdgeFunction (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
- Een actieve iothub met ten minste een Edge van de IoT-apparaat.

Ook het beste installeren [Docker-ondersteuning voor VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) beter uw module installatiekopieën en containers te beheren.

> [!NOTE]
> Azure Functions op IoT rand ondersteunt momenteel alleen C#.

## <a name="deploy-azure-iot-functions-in-vs-code"></a>Azure IoT-functies in VS-Code implementeren
In de zelfstudie [implementeren Azure Functions](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function), u bijwerken, maken, en de functie module afbeeldingen publiceren in VS-Code en gaat vervolgens naar Azure-portal voor het implementeren van Azure Functions. Deze sectie wordt het gebruik van de VS Code implementeren en controleren van uw Azure-functies.

### <a name="start-a-local-docker-registry"></a>Start een lokale docker-register
Voor deze zelfstudie kunt u een register Docker-compatibel. Twee populaire Docker Registerservices beschikbaar in de cloud zijn [Azure Container register](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Deze sectie wordt een [lokale Docker-register](https://docs.docker.com/registry/deploying/), die is het eenvoudiger voor doel tijdens het ontwikkelen van uw vroege testdoeleinden.
In de VS Code **geïntegreerde terminal**(Ctrl + '), de volgende opdrachten voor het starten van een lokale register uitvoeren.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Hierboven voorbeeld toont register-configuraties die alleen geschikt voor het testen zijn. Een register gereed is voor productie moet worden beveiligd door TLS en moet in het ideale geval een access control-mechanisme gebruiken. We raden u aan [Azure Container register](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) voor het implementeren van de rand van de IoT-modules gereed is voor productie.

### <a name="create-a-function-project"></a>Een functie-project maken
De volgende stappen laten zien u hoe u een IoT-Edge-module maken die is gebaseerd op .NET core 2.0 met behulp van Visual Studio Code en de extensie Azure IoT rand. Als u deze sectie hebt voltooid in de vorige zelfstudie, kunt u veilig in deze sectie overslaan.

1. Selecteer in Visual Studio Code **weergave** > **geïntegreerde Terminal** openen van de VS Code geïntegreerde terminal.
2. Om te installeren (of werk) de **AzureIoTEdgeFunction** sjabloon in dotnet, voer de volgende opdracht in de geïntegreerde terminal:

   ```cmd/sh
   dotnet new -i Microsoft.Azure.IoT.Edge.Function
   ```
3. Maak een project voor de nieuwe module. De volgende opdracht maakt u de projectmap **FilterFunction**, in de huidige map:

   ```cmd/sh
   dotnet new aziotedgefunction -n FilterFunction
   ```
 
4. Selecteer **bestand > openen map**, bladert u naar de **FilterFunction** map en open het project in VS-Code.
5. Blader naar de **FilterFunction** map en klik op **map selecteren** openen van het project in VS-Code.
6. Vouw in VS-Code explorer de **EdgeHubTrigger Csharp** map, open vervolgens de **run.csx** bestand.
7. Vervang de inhoud van het bestand met de volgende code:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }
                // Add a new property to the message to indicate it is an alert
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message        
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages
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

8. Sla het bestand op.

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Vouw in VS-Code explorer de **Docker** map. Vouw vervolgens ofwel de map voor uw platform container **linux x64** of **windows nano**.
2. Met de rechtermuisknop op de **Dockerfile** -bestand en klik op **bouwen IoT rand module Docker installatiekopie**. 

    ![Een docker-installatiekopie maken](./media/how-to-vscode-develop-csharp-function/build-docker-image.png)

3. Navigeer naar de **FilterFunction** projectmap en klik op **map selecteren als EXE_DIR**. 
4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filterfunction:latest`. Als u naar het lokale register implementeert, moet dit worden `localhost:5000/filterfunction:latest`.
5. De installatiekopie naar uw opslagplaats Docker forceren. Gebruik de **rand: Push IoT rand module Docker installatiekopie** opdracht en de afbeeldings-URL in het pop-tekst boven aan het venster VS-Code invoeren. Gebruik dezelfde afbeeldings-URL die u in bovenstaande stap gebruikt.
    ![Push docker-afbeelding](./media/how-to-vscode-develop-csharp-function/push-image.png)

### <a name="deploy-your-function-to-iot-edge"></a>De functie naar IoT rand implementeren

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
   "filterfunction": {
      "version": "1.0",
      "type": "docker",
      "status": "running",
      "restartPolicy": "always",
      "settings": {
         "image": "localhost:5000/filterfunction:latest",
         "createOptions": ""
      }
   }
   ```

2. Vervang de **routes** sectie met de onderstaande inhoud:
   ```json
       "routes":{
           "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
           "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
   ```
   > [!NOTE]
   > Declaratieve regels in de runtime definiëren waar deze berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor aan de filterfunctie via het eindpunt 'input1', dat het eindpunt dat u met de handler FilterMessages geconfigureerd is. De tweede route transporten berichten van de filterfunctie IoT-hub. In deze route upstream is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub.

3. Sla dit bestand.
4. Selecteer in de opdracht palet **rand: implementatie voor randapparaat maken**. Selecteer vervolgens uw IoT-rand apparaat-ID voor het maken van een implementatie. Of met de rechtermuisknop op het apparaat-ID in de lijst met apparaten en selecteer **implementatie creëert voor randapparaat**.

    ![Implementatie maken](./media/how-to-vscode-develop-csharp-function/create-deployment.png)

5. Selecteer de `deployment.json` u bijgewerkt. In het venster output ziet u bijbehorende uitvoer voor uw implementatie.
6. Start uw runtime rand in de opdracht palet. **Rand: Begin rand**
7. U ziet uw IoT-rand runtime starten in de Docker-explorer met de gesimuleerde sensoren en filter-functie wordt uitgevoerd.

    ![Oplossing met](./media/how-to-vscode-develop-csharp-function/solution-running.png)

8. Met de rechtermuisknop op uw apparaat-ID van rand en kunt u berichten in de VS Code D2C bewaken.

    ![Monitor-berichten](./media/how-to-vscode-develop-csharp-function/monitor-d2c-messages.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt van een Azure-functie op IoT rand en geïnstalleerd op de rand van de IoT-apparaat in VS-Code. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere scenario's bij het ontwikkelen van Azure IoT rand in de VS-Code.

> [!div class="nextstepaction"]
> [Fouten opsporen in Azure Functions in VS-Code](how-to-vscode-debug-azure-function.md)
