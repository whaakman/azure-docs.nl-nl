---
title: Azure IoT rand C#-module | Microsoft Docs
description: Een IoT-Edge-module maken met C#-code en deze implementeren in een edge-apparaat
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4fd84904fb264fc61d0059d389347e05839162d2
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Opstellen en implementeren van een Edge van C#-IoT-module voor uw gesimuleerde apparaat - voorbeeld

Rand van de IoT-modules kunt u code waarmee u uw bedrijfslogica rechtstreeks aan de rand van de IoT-apparaten implementeren. Deze zelfstudie wordt u begeleid bij het maken en implementeren van een module IoT rand waarmee sensorgegevens wordt gefilterd. U gebruikt het gesimuleerde rand van de IoT-apparaat die u hebt gemaakt in de implementatie van Azure IoT-Edge van een gesimuleerd apparaat in [Windows] [ lnk-tutorial1-win] of [Linux] [ lnk-tutorial1-lin]zelfstudies. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om te maken van een IoT-Edge-module op basis van .NET core 2.0
> * Gebruik Visual Studio Code en Docker voor een docker-installatiekopie maken en deze publiceren naar het register 
> * De module implementeren voor uw IoT-randapparaat
> * Gegevens weergeven die zijn gegenereerd


De rand van de IoT-module die u in deze zelfstudie maakt filtert de temperatuur gegevens die zijn gegenereerd door uw apparaat. Alleen wordt verzonden berichten stroomopwaarts als de temperatuur hoger dan een opgegeven drempelwaarde is. Dit type analyse aan de rand is nuttig voor het beperken van de hoeveelheid gegevens wordt gecommuniceerd aan en opgeslagen in de cloud. 

## <a name="prerequisites"></a>Vereisten

* Azure IoT apparaat aan de rand die u hebt gemaakt in de Quick Start of de eerste zelfstudie.
* De verbindingsreeks van de primaire sleutel voor het IoT Edge-apparaat.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [C# voor Visual Studio Code (via OmniSharp)-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/) op dezelfde computer met Visual Studio Code. De Community Edition (CE) is voldoende voor deze zelfstudie. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie maakt u de extensie Azure IoT rand voor VS-Code kunt gebruiken om te bouwen van een module en maak een **container installatiekopie** van de bestanden. En u deze installatiekopie push een **register** die worden opgeslagen en beheerd van uw afbeeldingen. U tot slot implementeren de installatiekopie uit het register uit te voeren op uw IoT-randapparaat.  

Voor deze zelfstudie kunt u een register Docker-compatibel. Twee populaire Docker Registerservices beschikbaar in de cloud zijn [Azure Container register](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Deze zelfstudie wordt Azure Container register. 

1. In de [Azure-portal](https://portal.azure.com), selecteer **maken van een resource** > **Containers** > **Azure Container register** .
2. Geef een naam op voor het register, kies een abonnement, kiest u een resourcegroep en de SKU ingesteld op **Basic**. 
3. Selecteer **Maken**.
4. Zodra het register van de container is gemaakt, gaat u naar het en selecteer **toegangssleutels**. 
5. Wisselknop **gebruiker met beheerdersrechten** naar **inschakelen**.
6. Kopieer de waarden voor **Login-server**, **gebruikersnaam**, en **wachtwoord**. Verderop in de zelfstudie gebruikt u deze waarden. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT-rand module-project maken
De volgende stappen laten zien u hoe u een IoT-Edge-module maken die is gebaseerd op .NET core 2.0 met behulp van Visual Studio Code en de extensie Azure IoT rand.
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
5. Klik in Verkenner VS-Code op **Program.cs** om dit te openen.

   ![Open Program.cs][1]

6. Aan de bovenkant van de **FilterModule** naamruimte toevoegen drie `using` -instructies voor typen die later worden gebruikt:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
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

8. In de **Init** methode, de code maakt en configureert u een **DeviceClient** object. Dit object kunt de module die u wilt verbinding maken met de lokale Azure IoT Edge-runtime voor het verzenden en ontvangen van berichten. De verbindingsreeks die wordt gebruikt in de **Init** methode voor de module wordt verstrekt door de rand van de IoT-runtime. Na het maken van de **DeviceClient**, de code leest de TemperatureThreshold uit de Module-Twin gewenste eigenschappen en registreert een retouraanroep voor het ontvangen van berichten van de rand van de IoT-hub via de **input1**eindpunt. Vervang de `SetInputMessageHandlerAsync` methode met een nieuwe, en voeg een `SetDesiredPropertyUpdateCallbackAsync` methode voor het gewenste eigenschappen updates. Als u deze wijziging, vervangt u de laatste regel van de **Init** methode met de volgende code:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    if (moduleTwinCollection["TemperatureThreshold"] != null)
    {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    }

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

   ![Rand van de IoT-module maken][2]

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Vouw in VS-Code explorer de **Docker** map. Vouw vervolgens ofwel de map voor uw platform container **linux x64** of **windows nano**.

   ![Selecteer Docker-container platform][3]

2. Met de rechtermuisknop op de **Dockerfile** -bestand en klik op **bouwen IoT rand module Docker installatiekopie**. 
3. In de **map selecteren** venster ofwel om te zoeken of invoeren `./bin/Debug/netcoreapp2.0/publish`. Klik op **map selecteren als EXE_DIR**.
4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filtermodule:latest`. De Register-adres van de container is hetzelfde als de aanmeldingsserver die u hebt gekopieerd uit het register. Deze moet de vorm van `<your container registry name>.azurecr.io`.
5. Aanmelden bij Docker hiertoe de volgende opdracht in de geïntegreerde terminal VS-Code in: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Gebruik de gebruikersnaam, wachtwoord en login-server die u hebt gekopieerd uit uw Azure-container register wanneer u het hebt gemaakt.

3. De installatiekopie naar uw opslagplaats Docker forceren. Selecteer **weergave** > **opdracht palet** en zoek naar de **rand: Push IoT rand module Docker installatiekopie** menuopdracht. Voer de naam van de afbeelding in het pop-tekst boven aan het venster VS-Code. Gebruik dezelfde naam van de installatiekopie die u in stap 4 wordt gebruikt.

## <a name="add-registry-credentials-to-edge-runtime"></a>Register-referenties aan de rand runtime toevoegen
De referenties voor uw register toevoegen aan de rand runtime op de computer waarop u het apparaat aan de rand worden uitgevoerd. Deze referenties geven de runtime toegang tot het ophalen van de container. 

- Voor Windows, moet u de volgende opdracht uitvoeren:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Voer de volgende opdracht voor Linux:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>De oplossing uitvoeren

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub.
2. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
3. Selecteer **Modules ingesteld**. 
2. Controleer of de **tempSensor** module wordt automatisch ingevuld. Als dit niet het geval is, gebruikt u de volgende stappen toe te voegen:
    1. Selecteer **IoT rand Module toevoegen**.
    2. In de **naam** veld `tempSensor`.
    3. In de **installatiekopie URI** veld `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Laat de overige instellingen ongewijzigd en klik op **opslaan**.
9. Voeg de **filterModule** module die u hebt gemaakt in de vorige secties. 
    1. Selecteer **IoT rand Module toevoegen**.
    2. In de **naam** veld `filterModule`.
    3. In de **installatiekopie URI** en voer het adres klopt; bijvoorbeeld `<your container registry address>/filtermodule:latest`.
    4. Controleer de **inschakelen** vak zodat u de twin module kunt bewerken. 
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
13. In de **Routes opgeven** stap, kopieert u de JSON onder in het tekstvak. Alle berichten publiceren modules naar de Edge-runtime. Declaratieve regels in de runtime definiëren waar de berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor naar de filtermodule via het eindpunt 'input1', dat het eindpunt dat u hebt geconfigureerd met de **FilterMessages** handler. De tweede route transporten berichten van de module expressiefilter IoT-hub. In deze route `upstream` is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Klik op **Volgende**.
5. In de **Template bekijken** stap, klikt u op **indienen**. 
6. Ga terug naar de detailpagina voor rand van de IoT-apparaat en klikt u op **vernieuwen**. U ziet nu de nieuwe **filtermodule** uitgevoerd samen met de **tempSensor** module en de **IoT rand runtime**. 

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

Voor het bewaken van het apparaat verzonden van uw IoT-randapparaat naar uw IoT-hub voor cloud-berichten:
1. De Azure IoT Toolkit-uitbreiding configureren met de verbindingsreeks voor uw IoT-hub: 
    1. De Code van de VS explorer openen door te selecteren **weergave** > **Explorer**. 
    3. Klik in de Verkenner op **IOT HUB-apparaten** en klik vervolgens op **...** . Klik op **verbindingsreeks van de IoT-Hub ingesteld** en geef de verbindingsreeks voor de IoT-hub die uw IoT-Edge-apparaat met in het pop-upvenster verbindt. 

        Als u wilt zoeken in de verbindingsreeks, klik op de tegel voor uw IoT-hub in de Azure portal en klik vervolgens op **gedeeld toegangsbeleid**. In **gedeeld toegangsbeleid**, klikt u op de **iothubowner** beleid en kopieert u de verbinding met IoT Hub tekenreeks in de **iothubowner** venster.   

1. Selecteer voor het bewaken van gegevens die binnenkomen in de IoT-hub **weergave** > **opdracht palet** en zoek naar de **IoT: D2C bericht bewaking starten** menuopdracht. 
2. Als u wilt stoppen met het bewaken van gegevens, gebruikt u de **IoT: D2C bericht bewaking stopt** menuopdracht. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een IoT-rand module met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT-randapparaat gemaakt. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere manieren Azure IoT rand kunt u de gegevens in zakelijke inzichten aan de rand.

> [!div class="nextstepaction"]
> [Azure-functie als een module implementeren](tutorial-deploy-function.md)
> [Azure Stream Analytics implementeren als een module](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
