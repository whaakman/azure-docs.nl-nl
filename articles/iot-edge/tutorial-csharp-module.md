---
title: Azure IoT Edge C#-module| Microsoft Docs
description: Een IoT Edge-module met C#-code make en implementeren op een Edge-apparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1da3a246a2ad33a4563f491058f5d4d115f3954d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631068"
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Een C# IoT Edge-module maken en implementeren op uw gesimuleerd apparaat - preview

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. U gebruikt het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de zelfstudie Azure IoT Edge implementeren op een gesimuleerd apparaat in [Windows][lnk-tutorial1-win] of [Linux][lnk-tutorial1-lin]. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge-module te maken op basis van .NET core 2.0
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register 
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheidgegevens die worden gecommuniceerd naar en opgeslagen in de cloud te reduceren. 

## <a name="prerequisites"></a>Vereisten

* Het Azure IoT Edge-apparaat dat u hebt gemaakt in de snelstartgids of de vorige zelfstudie.
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
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in deze zelfstudie wanneer u de Docker-installatiekopie publiceert naar uw register en wanneer u de registerreferenties toevoegt aan de Edge-runtime. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
De volgende stappen laten zien hoe u een IoT-Edge-module op basis van .NET core 2.0 maakt met behulp van Visual Studio Code en de Azure IoT Edge-extensie.
1. Selecteer in Visual Studio Code **View** > **Integrated Terminal** om de met VS code geïntegreerde terminal te openen.
2. Voer de volgende opdracht uit in de geïntegreerde terminal om de **AzureIoTEdgeModule**-sjabloon in dotnet te installeren (of bij te werken):

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

3. Maak een project voor de nieuwe module. Met de volgende opdracht maakt u de projectmap, **FilterModule**, met de containeropslagplaats. De tweede parameter moet een indeling vergelijkbaar met `<your container registry name>.azurecr.io` hebben als u Azure Container Registry gebruikt. Voer de volgende opdracht in de huidige werkmap in:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule -r <your container registry address>/filtermodule
    ```
 
4. Selecteer **Bestand** > **Map openen**.
5. Blader naar de map **FilterModule** en klik op **Map selecteren** om het project in VS Code te openen.
6. Klik in VS Code-verkenner op **Program.cs** om het te openen.

   ![Open Program.cs][1]

7. Voeg drie `using`-instructies toe bovenaan de naamruimte **FilterModule** voor typen die later worden gebruikt:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

8. Voeg de variabele `temperatureThreshold` toe aan de klasse **Programma**. Deze variabele bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

9. Voeg de klassen `MessageBody`, `Machine` en `Ambient` toe aan de klasse **Programma**. Deze klassen bepalen het verwachte schema voor de hoofdtekst van inkomende berichten.

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

10. In de methode **Init** maakt en configureert de code een object **DeviceClient**. Met dit object kan de module verbinding maken met de lokale Azure IoT Edge-runtime om berichten te verzenden en te ontvangen. De verbindingsreeks die in de methode **Init** wordt gebruikt wordt door IoT Edge-runtime aan de module geleverd. Na het maken van de **DeviceClient** leest de code de TemperatureThreshold uit de gewenste eigenschappen van Moduledubbel en registreert een terugbelfunctie voor ontvangen berichten van de IoT Edge-hub via het **input1**-eindpunt. Vervang de methode `SetInputMessageHandlerAsync` door een nieuwe en voeg een methode `SetDesiredPropertyUpdateCallbackAsync` toe voor updates van gewenste eigenschappen. Om deze wijziging aan te brengen, moet u de laatste regel van de methode **Init** vervangen door de volgende code:

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

11. Voeg de methode `onDesiredPropertiesUpdate`toe aan de klasse **Programma**. Deze methode ontvangt updates van de gewenste eigenschappen van de moduledubbel en updatet de variabele **temperatureThreshold** naar dezelfde waarde. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

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

12. Vervang de methode `PipeMessage` door de methode `FilterMessages`. Deze methode wordt aangeroepen wanneer de module een bericht ontvangt van de IoT Edge-hub. Berichten die temperaturen onder de drempelwaarde die via de moduledubbel is ingesteld, worden hierdoor gefilterd. Ook wordt de eigenschap **BerichtType** toegevoegd aan het bericht met de waarde ingesteld op **Alarm**. 

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

13. Dit bestand opslaan.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde VS Code-terminal in te voeren: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Ga naar de [Azure Portal] https://portal.azure.com) om de gebruikersnaam, het wachtwoord en de aanmeldingsserver op te zoeken die in deze opdracht moeten worden gebruikt. Klik vanuit **Alle resources** op de tegel voor uw Azure-containerregister om de eigenschappen te openen en klik vervolgens op **Toegangssleutels**. Kopieer de waarden in de velden **Gebruikersnaam**, **Wachtwoord** en **Aanmeldingsserver**. 

2. Klik in VS Code explorer met de rechtermuisknop op het bestand **module.json** en klik op **Build and Push IoT Edge module Docker image** (Docker-installatiekopie voor IoT Edge-module maken en pushen). Selecteer in de vervolgkeuzelijst bovenaan het venster VS Code uw containerplatform: **amd64** voor een Linux-container of **windows-amd64** voor een Windows-container. VS Code bouwt uw code, plaatst de `FilterModule.dll` in een container en pusht deze naar het containerregister dat u hebt opgegeven.


3. U kunt het volledige adres van de containerinstallatiekopie, inclusief tag, in de geïntegreerde terminal van VS Code krijgen. Bekijk het `module.json`-bestand als u meer wilt weten over de build- en push-definitie.

## <a name="add-registry-credentials-to-edge-runtime"></a>Registerreferenties toevoegen aan uw Edge-runtime
Voeg de referenties voor uw register toe aan de Edge-runtime op de computer waarop u het Edge-apparaat uitvoert. Deze referenties geven de runtime toegang voor het ophalen van de container. 

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
4. Controleer of de **tempSensor**-module automatisch wordt gevuld. Zo niet, gebruik dan de volgende stappen om die toe te voegen:
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `tempSensor` in.
    3. Voer in het veld **URI installatiekopie** `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` in.
    4. Laat de overige instellingen ongewijzigd en klik op **Opslaan**.
5. Voeg de module **filterModule** toe die u hebt gemaakt in de vorige sectie. 
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `filterModule` in.
    3. Voer in het veld **URI installatiekopie** het adres van uw installatiekopie in, bijvoorbeeld `<your container registry address>/filtermodule:0.0.1-amd64`. Het volledige adres van de installatiekopie vindt u in de vorige sectie.
    4. Schakel het vak **Inschakelen** in, zodat u de moduledubbel kunt bewerken. 
    5. Vervang de JSON in het tekstvak voor de moduledubbel met de volgende JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Klik op **Opslaan**.
6. Klik op **Volgende**.
7. Kopieer in de stap **Routes opgeven** de onderstaande JSON in het tekstvak. Modules publiceren alle berichten naar de Edge-runtime. Declaratieve regels in de runtime bepalen waar de berichten naartoe stromen. In deze zelfstudie heb je twee routes nodig. De eerste route vervoert berichten van de temperatuursensor naar de filtermodule via het eindpunt ‘input1’, het eindpunt dat u hebt geconfigureerd met de handler **FilterMessages**. Met de tweede route worden berichten van de filtermodule naar IoT Hub getransporteerd. In deze route is `upstream` een speciale bestemming die Edge Hub opdracht geeft berichten te verzenden naar IoT Hub. 

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
10. Ga terug naar de detailpagina van het IoT Edge-apparaat en klik op **Vernieuwen**. U ziet nu dat de nieuwe **filtermodule** samen met de **tempSensor**-module en de **IoT Edge-runtime** wordt uitgevoerd. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Ga als volgt te werk om het apparaat te controleren op cloudberichten die vanaf uw IoT Edge-apparaat worden verzonden naar uw IoT Hub:
1. Configureer de Azure IoT Toolkit-extensie met de verbindingsreeks voor uw IoT Hub: 
    1. Open de VS Code Explorer door **Verkenner** > **bekijken** te selecteren. 
    2. Klik in de verkenner op **IOT HUB DEVICES** en klik vervolgens op **...** . Klik op **IoT Hub-verbindingsreeks instellen** en voer in het pop-upvenster de verbindingsreeks in voor de IoT Hub waarnaar uw IoT Edge-apparaat verbindt. 

        Om de verbindingsreeks te vinden, klikt u op de tegel voor uw IoT Hub in Azure Portal en vervolgens op **Gedeeld toegangsbeleid**. Klik in **Gedeeld toegangsbeleid** op het beleid **iothubowner** en kopieer de IoT Hub-verbindingsreeks in het venster **iothubowner**.   

2. Om gegevens die binnenkomen in de IoT Hub te controleren, selecteert u **View** > **Command Palette** en zoekt u naar de menuopdracht **IoT: Start monitoring D2C message**. 
3. Als u wilt stoppen met het controleren van gegevens, gebruikt u de menuopdracht **IoT: Stop monitoring D2C message**. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. U kunt doorgaan met en van de volgende zelfstudies om te leren waarmee Azure IoT Edge u nog meer kan helpen uw gegevens om te zetten in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Azure-functies als module implementeren](tutorial-deploy-function.md)
> [Azure Stream Analytics als module implementeren](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
