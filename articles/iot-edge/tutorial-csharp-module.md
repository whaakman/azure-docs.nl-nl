---
title: Azure IoT rand C#-module | Microsoft Docs
description: Een IoT-Edge-module maken met C#-code en deze implementeren in een edge-apparaat
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1caa887a13453ce2b2b07e83b74f0ed57535b026
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Opstellen en implementeren van een Edge van C#-IoT-module voor uw gesimuleerde apparaat - voorbeeld

Rand van de IoT-modules kunt u code waarmee u uw bedrijfslogica rechtstreeks aan de rand van de IoT-apparaten implementeren. Deze zelfstudie wordt u begeleid bij het maken en implementeren van een module IoT rand waarmee sensorgegevens wordt gefilterd. U gebruikt het gesimuleerde rand van de IoT-apparaat die u hebt gemaakt in de implementatie van Azure IoT-Edge van een gesimuleerd apparaat in [Windows] [ lnk-tutorial1-win] of [Linux] [ lnk-tutorial1-lin]zelfstudies. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om te maken van een IoT-Edge-module op basis van .NET core 2.0
> * Gebruik Visual Studio Code en Docker voor een docker-installatiekopie maken en deze publiceren naar het register 
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De rand van de IoT-module die u in deze zelfstudie maakt filtert de temperatuur gegevens die zijn gegenereerd door uw apparaat. Alleen wordt verzonden berichten stroomopwaarts als de temperatuur hoger dan een opgegeven drempelwaarde is. Dit type analyse aan de rand is nuttig voor het beperken van de hoeveelheid gegevens wordt gecommuniceerd aan en opgeslagen in de cloud. 

## <a name="prerequisites"></a>Vereisten

* Azure IoT apparaat aan de rand die u hebt gemaakt in de Quick Start of de eerste zelfstudie.
* De verbindingsreeks van de primaire sleutel voor het IoT Edge-apparaat.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [De extensie C# voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/) op dezelfde computer met Visual Studio Code. De Community Edition (CE) is voldoende voor deze zelfstudie. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Azure Container Registry**.
2. Geef een naam op voor het register, kies een abonnement, kies een resourcegroep en stel de SKU in op **Basis**. 
3. Selecteer **Maken**.
4. Zodra het containerregister is gemaakt, gaat u er naartoe en selecteert u **Toegangssleutels**. 
5. Stel de **Gebruiker met beheerdersrechten** in op **Inschakelen**.
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden verderop in de zelfstudie wanneer u de installatiekopie van het Docker publiceert naar het register en wanneer u de referenties van het register toevoegt aan de rand-runtime. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT-rand module-project maken
De volgende stappen laten zien u hoe u een IoT-Edge-module maken die is gebaseerd op .NET core 2.0 met behulp van Visual Studio Code en de extensie Azure IoT rand.
1. Selecteer in Visual Studio Code **weergave** > **geïntegreerde Terminal** openen van de VS Code geïntegreerde terminal.
2. Voer de volgende opdracht om te installeren (of werk) in de geïntegreerde terminal de **AzureIoTEdgeModule** sjabloon in dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

3. Maak een project voor de nieuwe module. De volgende opdracht maakt u de projectmap **FilterModule**, met de container-opslagplaats. De tweede parameter moet een indeling vergelijkbaar met `<your container registry name>.azurecr.io` hebben als u Azure Container Registry gebruikt. Voer de volgende opdracht in de huidige werkmap in:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule -r <your container registry address>/filtermodule
    ```
 
4. Selecteer **bestand** > **Open map**.
5. Blader naar de **FilterModule** map en klik op **map selecteren** openen van het project in VS-Code.
6. Klik in Verkenner VS-Code op **Program.cs** om dit te openen.

   ![Open Program.cs][1]

7. Aan de bovenkant van de **FilterModule** naamruimte toevoegen drie `using` -instructies voor typen die later worden gebruikt:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

8. Voeg de `temperatureThreshold` variabele de **programma** klasse. Deze variabele bepaalt de waarde die de gemeten temperatuur moet groter zijn dan om de gegevens worden verzonden naar IoT Hub. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

9. Voeg de `MessageBody`, `Machine`, en `Ambient` klassen voor de **programma** klasse. Deze klassen definiëren het verwachte schema voor de hoofdtekst van binnenkomende berichten.

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

10. In de **Init** methode, de code maakt en configureert u een **DeviceClient** object. Dit object kunt de module die u wilt verbinding maken met de lokale Azure IoT Edge-runtime voor het verzenden en ontvangen van berichten. De verbindingsreeks die wordt gebruikt in de **Init** methode voor de module wordt verstrekt door de rand van de IoT-runtime. Na het maken van de **DeviceClient**, de code leest de TemperatureThreshold uit de Module-Twin gewenste eigenschappen en registreert een retouraanroep voor het ontvangen van berichten van de rand van de IoT-hub via de **input1**eindpunt. Vervang de `SetInputMessageHandlerAsync` methode met een nieuwe, en voeg een `SetDesiredPropertyUpdateCallbackAsync` methode voor het gewenste eigenschappen updates. Als u deze wijziging, vervangt u de laatste regel van de **Init** methode met de volgende code:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine("Property TemperatureThreshold not exist");
    }

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

11. Voeg de `onDesiredPropertiesUpdate` methode voor de **programma** klasse. Deze methode ontvangt updates op de gewenste eigenschappen van de module-twin en werkt de **temperatureThreshold** variabele moet worden gezocht. Alle modules hebben hun eigen module twin, kunt u de code die wordt uitgevoerd in een module rechtstreeks vanuit de cloud.

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

12. Vervang de `PipeMessage` methode met de `FilterMessages` methode. Deze methode wordt aangeroepen wanneer de module een bericht van de rand van de IoT-hub ontvangt. Deze berichten waarin wordt aangegeven temperaturen lager dan de drempelwaarde temperatuur is ingesteld via de module-twin gefilterd. Voegt ook de **MessageType** eigenschap aan het bericht met de waarde die is ingesteld op **waarschuwing**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
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
            var deviceClient = (DeviceClient)userContext;
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

13. Sla dit bestand.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde VS Code-terminal in te voeren: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Ga naar de [Azure Portal] https://portal.azure.com) om de gebruikersnaam, het wachtwoord en de aanmeldingsserver op te zoeken die in deze opdracht moeten worden gebruikt. Klik vanuit **Alle resources** op de tegel voor uw Azure-containerregister om de eigenschappen te openen en klik vervolgens op **Toegangssleutels**. Kopieer de waarden in de velden **Gebruikersnaam**, **Wachtwoord** en **Aanmeldingsserver**. 

2. Klik in VS Code explorer met de rechtermuisknop op het bestand **module.json** en klik op **Build and Push IoT Edge module Docker image** (Docker-installatiekopie voor IoT Edge-module maken en pushen). Selecteer in de vervolgkeuzelijst bovenaan het venster VS Code uw containerplatform: **amd64** voor een Linux-container of **windows-amd64** voor een Windows-container. VS Code vervolgens uw code wordt gemaakt, containerize de `FilterModule.dll` en dit doorgeven aan de container-registersleutel die u hebt opgegeven.


3. U kunt het volledige adres van de containerinstallatiekopie, inclusief tag, in de geïntegreerde terminal van VS Code krijgen. Bekijk het `module.json`-bestand als u meer wilt weten over de build- en push-definitie.

## <a name="add-registry-credentials-to-edge-runtime"></a>Register-referenties aan de rand runtime toevoegen
Voeg de referenties voor uw register toe aan de Edge-runtime op de computer waarop u het Edge-apparaat uitvoert. Deze referenties geven de runtime toegang tot het ophalen van de container. 

- Voer voor Windows de volgende opdracht uit:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Voer voor Linux de volgende opdracht uit:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>De oplossing uitvoeren

1. Navigeer in [Azure Portal](https://portal.azure.com) naar uw IoT-hub.
2. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
3. Selecteer **Modules instellen**. 
4. Controleer of de **tempSensor** module wordt automatisch ingevuld. Als dit niet het geval is, gebruikt u de volgende stappen toe te voegen:
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `tempSensor` in.
    3. Voer in het veld **URI installatiekopie** `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` in.
    4. Laat de overige instellingen ongewijzigd en klik op **Opslaan**.
5. Voeg de **filterModule** module die u hebt gemaakt in de vorige secties. 
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `filterModule` in.
    3. Voer in het veld **URI installatiekopie** het adres van uw installatiekopie in, bijvoorbeeld `<your container registry address>/filtermodule:0.0.1-amd64`. Het volledige adres van de installatiekopie vindt u in de vorige sectie.
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
6. Klik op **Volgende**.
7. Kopieer in de stap **Routes opgeven** de onderstaande JSON in het tekstvak. Alle berichten publiceren modules naar de Edge-runtime. Declaratieve regels in de runtime definiëren waar de berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor naar de filtermodule via het eindpunt 'input1', dat het eindpunt dat u hebt geconfigureerd met de **FilterMessages** handler. Met de tweede route worden berichten van de filtermodule naar IoT Hub getransporteerd. In deze route is `upstream` een speciale bestemming die Edge Hub opdracht geeft berichten te verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

8. Klik op **Volgende**.
9. Klik in de stap **Sjabloon controleren** op **Indienen**. 
10. Ga terug naar de detailpagina van het IoT Edge-apparaat en klik op **Vernieuwen**. U ziet nu de nieuwe **filtermodule** uitgevoerd samen met de **tempSensor** module en de **IoT rand runtime**. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Ga als volgt te werk om het apparaat te controleren op cloudberichten die vanaf uw IoT Edge-apparaat worden verzonden naar uw IoT Hub:
1. Configureer de Azure IoT Toolkit-extensie met de verbindingsreeks voor uw IoT Hub: 
    1. De Code van de VS explorer openen door te selecteren **weergave** > **Explorer**. 
    2. Klik in de Verkenner op **IOT HUB-apparaten** en klik vervolgens op **...** . Klik op **verbindingsreeks van de IoT-Hub ingesteld** en geef de verbindingsreeks voor de IoT-hub die uw IoT-Edge-apparaat met in het pop-upvenster verbindt. 

        Als u wilt zoeken in de verbindingsreeks, klik op de tegel voor uw IoT-hub in de Azure portal en klik vervolgens op **gedeeld toegangsbeleid**. In **gedeeld toegangsbeleid**, klikt u op de **iothubowner** beleid en kopieert u de verbinding met IoT Hub tekenreeks in de **iothubowner** venster.   

2. Selecteer voor het bewaken van gegevens die binnenkomen in de IoT-hub **weergave** > **opdracht palet** en zoek naar de **IoT: D2C bericht bewaking starten** menuopdracht. 
3. Als u wilt stoppen met het bewaken van gegevens, gebruikt u de **IoT: D2C bericht bewaking stopt** menuopdracht. 

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
