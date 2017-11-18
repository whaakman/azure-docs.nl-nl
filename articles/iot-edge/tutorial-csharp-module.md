---
title: Azure IoT rand C#-module | Microsoft Docs
description: Een IoT-Edge-module maken met C#-code en deze implementeren in een edge-apparaat
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: c778c412bf6d65c5b6ee92d603aac7acfa6139eb
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Opstellen en implementeren van een Edge van C#-IoT-module voor uw gesimuleerde apparaat - voorbeeld

Rand van de IoT-modules kunt u code waarmee u uw bedrijfslogica rechtstreeks aan de rand van de IoT-apparaten implementeren. In deze zelfstudie wordt u begeleid bij het maken en implementeren van een IoT-Edge-module die sensorgegevens op het gesimuleerde rand van de IoT-apparaat dat u hebt gemaakt in de implementatie van Azure IoT-Edge van een gesimuleerd apparaat filtert op [Windows] [ lnk-tutorial1-win]of [Linux] [ lnk-tutorial1-lin] zelfstudies. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om te maken van een IoT-Edge-module op basis van .NET core 2.0
> * Gebruik Visual Studio Code en Docker voor een docker-installatiekopie maken en deze publiceren naar het register 
> * De module implementeren voor uw IoT-randapparaat
> * Gegevens weergeven die zijn gegenereerd


De rand van de IoT-module die u in deze zelfstudie maakt de temperatuur-gegevens die worden gegenereerd door het apparaat worden gefilterd en berichten alleen stroomopwaarts wordt verzonden wanneer de temperatuur hoger dan een opgegeven drempelwaarde is. 

## <a name="prerequisites"></a>Vereisten

* Azure IoT apparaat aan de rand die u hebt gemaakt in de Quick Start of de vorige zelfstudie.
* De verbindingsreeks voor de IoT Hub voor uw IoT-Edge-apparaat verbinding met maakt IoT-hub.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (U kunt de uitbreiding installeren vanuit het deelvenster uitbreidingen in Visual Studio Code.)
* [C# voor Visual Studio Code (via OmniSharp)-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (U kunt de uitbreiding installeren vanuit het deelvenster uitbreidingen in Visual Studio Code.)
* [Docker](https://docs.docker.com/engine/installation/). De Community Edition (CE) voor uw platform is voldoende voor deze zelfstudie. Zorg ervoor dat u deze installeren op de computer waarop u tegenover Code uitvoeren op.
* [.NET core SDK 2.0](https://www.microsoft.com/net/core#windowscmd). 

## <a name="choose-or-sign-up-for-a-docker-registry"></a>Kies of zich aanmeldt voor een Docker-register
In deze zelfstudie maakt u de extensie Azure IoT rand voor VS-Code kunt gebruiken om te bouwen van een module en maak een [Docker-afbeelding](https://docs.docker.com/glossary/?term=image). En u deze installatiekopie Docker push een [Docker-opslagplaats](https://docs.docker.com/glossary/?term=repository) gehost op een [Docker-register](https://docs.docker.com/glossary/?term=registry). Ten slotte het implementeren van uw Docker-installatiekopie geleverd als een [Docker-container](https://docs.docker.com/glossary/?term=container) uit het register naar uw IoT-Edge-apparaat.  

Voor deze zelfstudie kunt u een register Docker-compatibel. Twee populaire Docker Registerservices beschikbaar in de cloud zijn **Azure Container register** en **Docker Hub**:

- [Azure Container register](https://docs.microsoft.com/en-us/azure/container-registry/) is beschikbaar met een [betaald abonnement](https://azure.microsoft.com/en-us/pricing/details/container-registry/). Voor deze zelfstudie de **Basic** abonnement is voldoende. 

- [Docker-Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) biedt een gratis persoonlijke opslagplaats als u zich aanmeldt voor een (gratis) Docker-ID. 
    1. Als u wilt aanmelden voor een Docker-ID, volg de instructies in [registreren voor een Docker-ID](https://docs.docker.com/docker-id/#register-for-a-docker-id) op de Docker-site. 

    2. Volg de instructies in voor het maken van een persoonlijke Docker-opslagplaats [maken van een nieuwe opslagplaats op Docker Hub](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) op de Docker-site.

In deze zelfstudie, indien van toepassing wordt opdrachten worden opgegeven voor zowel de Azure Container register en de Docker-Hub.

## <a name="create-an-iot-edge-module-project"></a>Een IoT-rand module-project maken
De volgende stappen laten zien u hoe u een IoT-Edge-module maken die is gebaseerd op .NET core 2.0 met behulp van Visual Studio Code en de extensie Azure IoT rand.
1. Open VS-Code.
2. Gebruik de **weergave | Geïntegreerde Terminal** menuopdracht openen van de VS Code geïntegreerde terminal.
3. Voer de volgende opdracht om te installeren (of werk) in de geïntegreerde terminal de **AzureIoTEdgeModule** sjabloon in dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Voer de volgende opdracht voor het maken van een project voor de nieuwe module in de geïntegreerde terminal:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```

    >[!NOTE]
    > Deze opdracht maakt u de projectmap **FilterModule**, in de huidige map. Als u maken in een andere locatie wilt, wijzig de mappen voordat de opdracht uit te voeren.
 
3. Gebruik de **bestand | Open de map** menuopdracht, blader naar de **FilterModule** map en klik op **map selecteren** openen van het project in VS-Code.
4. Klik in Verkenner VS-Code op **Program.cs** om dit te openen.
5. Het volgende veld toevoegen aan de **programma** klasse.

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

1. De volgende klassen toevoegt aan de **programma** klasse. Deze klassen definiëren het verwachte schema voor de hoofdtekst van binnenkomende berichten.

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

1. In de **Init** methode, de code maakt en configureert u een **DeviceClient** object. Dit object kunt de module die u wilt verbinding maken met de lokale Azure IoT Edge-runtime voor het verzenden en ontvangen van berichten. De verbinding tekenreeksparameter gebruikt in de **Init** methode naar de module wordt verstrekt door de runtime IoT rand in de omgevingsvariabele **EdgeHubConnectionString**. Na het maken van de **DeviceClient**, de code registreert een retouraanroep voor het ontvangen van berichten van de rand van de IoT-hub via de **input1** eindpunt. Vervang de methode die wordt gebruikt als retouraanroep voor het verwerken van berichten met een nieuwe, en voeg een retouraanroep voor eigenschappen van de gewenste updates in de module-twin. Als u deze wijziging, vervangt u de laatste regel van de **Init** methode met de volgende code:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

1. Toevoegen van de volgende methode voor de **programma** klasse bijwerken de **temperatureThreshold** veld gebaseerd op de gewenste eigenschappen is verzonden door de back-end-service via de module-twin. Alle modules hebben hun eigen twin module. Een module-twin kunt een back-endservice configureren van de code die wordt uitgevoerd in een module.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"].exists())
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

1. Vervang de **PipeMessage** methode met de volgende methode. Deze methode wordt aangeroepen wanneer de module een bericht van de Edge-Hub verzonden wordt. Deze filtert berichten op basis van de temperatuur-waarde in de hoofdtekst van het bericht en de temperatuur drempelwaarde ingesteld via de module-twin.

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

11. Maak het project. Gebruik de **weergave | Explorer** menuopdracht om de Code van de VS explorer te openen. In de Verkenner met de rechtermuisknop op de **FilterModule.csproj** -bestand en klik op **bouwen IoT rand module** compileren van de module en exporteren van het binaire bestand en de bijbehorende afhankelijkheden in een map of de Docker-installatiekopie is gemaakt uit in de volgende stap.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. De Docker-installatiekopie bouwen.
    1. Klik in Verkenner VS-Code op de **Docker** map om dit te openen. Selecteer de map voor uw platform container ofwel **linux x64** of **windows nano**. 
    2. Met de rechtermuisknop op de **Dockerfile** -bestand en klik op **bouwen IoT rand module Docker installatiekopie**. 
    3. In de **map selecteren** vak beide bladert u naar of voer `./bin/Debug/netcoreapp2.0/publish`. Klik op **map selecteren als EXE_DIR**.
    4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld:`<docker registry address>/filtermodule:latest`, waarbij *docker register adres* uw Docker-ID is als u met behulp van Docker-Hub of vergelijkbaar met `<your registry name>.azurecr.io`, als u van Azure Container register gebruikmaakt.
 
4. Aanmelden bij Docker. Voer de volgende opdracht in de geïntegreerde terminal: 

    - Docker-Hub (Voer uw referenties als u wordt gevraagd):
        
        ```csh/sh
        docker login
        ```

    - Voor Azure-Container registersleutel:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Als u de gebruikersnaam, wachtwoord en meld u aan server moet worden gebruikt in deze opdracht zoekt, gaat u naar de [Azure portal] (https://portal.azure.com). Van **alle resources**, klikt u op de tegel voor uw Azure-container register open de eigenschappen en klik vervolgens op **toegangssleutels**. Kopieer de waarden in de **gebruikersnaam**, **wachtwoord**, en **Login-server** velden. De aanmelding server sould worden van het formulier: `<your registry name>.azurecr.io`.

3. De installatiekopie naar uw opslagplaats Docker forceren. Gebruik de **weergave | Opdracht palet... | Rand: Push IoT rand module Docker installatiekopie** menu opdracht in en voer de naam van de installatiekopie in het pop-tekst boven aan het venster VS-Code. Gebruik dezelfde naam van de installatiekopie u in stap gebruikt 1.c.

## <a name="add-registry-credentials-to-edge-runtime-on-your-edge-device"></a>Voeg register referenties toe aan de rand runtime op het apparaat van rand
De referenties voor uw register toevoegen aan de rand runtime op de computer waarop u het apparaat aan de rand worden uitgevoerd. Dit geeft de runtime toegang voor het ophalen van de container. 

- Voor Windows, moet u de volgende opdracht uitvoeren:
    
    ```cmd/sh
    iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

- Voer de volgende opdracht voor Linux:
    
    ```cmd/sh
    sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

## <a name="run-the-solution"></a>Uitvoeren van de oplossing

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub.
2. Ga naar **IoT rand (preview)** en selecteer het apparaat IoT rand.
3. Selecteer **Modules ingesteld**. 
2. Voeg de **tempSensor** module. Deze stap is alleen vereist als u nog niet eerder hebt geïmplementeerd de **tempSensor** module die u wilt uw IoT-randapparaat.
    1. Selecteer **IoT rand Module toevoegen**.
    2. In de **naam** veld `tempSensor`.
    3. In de **installatiekopie URI** veld `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Laat de overige instellingen ongewijzigd en klik op **opslaan**.
9. Voeg de **filtermodule**
    1. Selecteer **IoT rand Module toevoegen** opnieuw.
    2. In de **naam** veld `filtermodule`.
    3. In de **installatiekopie** en voer het adres klopt; bijvoorbeeld `<docker registry address>/filtermodule:latest`.
    4. Controleer de **bewerken module twin** vak.
    5. Vervang de JSON in het tekstvak voor de module met de volgende JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Klik op **Opslaan**.
12. Klik op **Volgende**.
13. In de **Routes opgeven** stap, kopieert u de JSON onder in het tekstvak. Alle berichten publiceren modules naar de Edge-runtime. Declaratieve regels in de runtime definiëren waar deze berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor naar de filtermodule via het eindpunt 'input1', dat het eindpunt dat u hebt geconfigureerd met de **FilterMessages** handler. De tweede route transporten berichten van de module expressiefilter IoT-hub. In deze route `upstream` is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Klik op **Volgende**.
5. In de **Template bekijken** stap, klikt u op **indienen**. 
6. Ga terug naar de detailpagina voor rand van de IoT-apparaat en klikt u op **vernieuwen**. U ziet nu de nieuwe **filtermodule** uitgevoerd samen met de **tempSensor** module en de **IoT rand runtime**. 

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

Voor het bewaken van het apparaat verzonden van uw IoT-randapparaat naar uw IoT-hub voor cloud-berichten:
1. De Azure IoT Toolkit-uitbreiding configureren met de verbindingsreeks voor uw IoT-hub: 
    1. Gebruik de **weergave | Explorer** menuopdracht om de Code van de VS explorer te openen. 
    3. Klik in de Verkenner op **IOT HUB-apparaten** en klik vervolgens op **...** . Klik op **verbindingsreeks van de IoT-Hub ingesteld** en geef de verbindingsreeks voor de IoT-hub die uw IoT-Edge-apparaat met in het pop-upvenster verbindt. 

        Als u wilt zoeken in de verbindingsreeks, klik op de tegel voor uw IoT-hub in de Azure portal en klik vervolgens op **gedeeld toegangsbeleid**. In **gedeeld toegangsbeleid**, klikt u op de **iothubowner** beleid en kopieert u de verbinding met IoT Hub tekenreeks in de **iothubowner** venster.   

1. Voor het bewaken van gegevens die binnenkomen in de IoT-hub, gebruiken de **weergave | Opdracht palet... | IoT: Starten van de controle van D2C bericht** menuopdracht. 
2. Als u wilt stoppen met het bewaken van gegevens, gebruikt u de **weergave | Opdracht palet... | IoT: Stopt met bewaken D2C bericht** menuopdracht. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een IoT-rand module met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT-randapparaat gemaakt. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere manieren Azure IoT rand kunt u de gegevens in zakelijke inzichten aan de rand.

> [!div class="nextstepaction"]
> [Azure-functie als een module implementeren](tutorial-deploy-function.md)
> [Azure Stream Analytics implementeren als een module](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
