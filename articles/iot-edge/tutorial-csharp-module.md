---
title: Zelfstudie voor Azure IoT Edge C# | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met C#-code maakt en deze implementeert op een Edge-apparaat.
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: c94479ca523f0097c8fbf94729f3a255ffc0c2bf
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413218"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Zelfstudie: Een C# IoT Edge-module ontwikkelen en implementeren op een gesimuleerd apparaat

U kunt Azure IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. U gebruikt het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de quickstarts over het implementeren van Azure IoT Edge op een gesimuleerd apparaat in [Windows][lnk-tutorial1-win] of [Linux][lnk-tutorial1-lin]. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge-module te maken op basis van de .NET Core 2.0 SDK.
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register.
> * De module implementeren op uw IoT Edge-apparaat.
> * Gegenereerde gegevens weergeven.


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheid gegevens te reduceren die worden gecommuniceerd naar en worden opgeslagen in de cloud. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).

Cloudresources:

* Een standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [De extensie C# voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code. 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/)


## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Azure Container Registry**.
2. Geef een naam op voor het register, kies een abonnement, kies een resourcegroep en stel de SKU in op **Basis**. 
3. Selecteer **Maken**.
4. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u **Toegangssleutels**. 
5. Stel de **Gebruiker met beheerdersrechten** in op **Inschakelen**.
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in deze zelfstudie om de Docker-installatiekopie naar uw register te publiceren en de registerreferenties aan de Azure IoT Edge-runtime toe te voegen. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
Met de volgende stappen maakt u een IoT Edge-moduleproject op basis van de .NET Core 2.0 SDK met behulp van Visual Studio Code en de Azure IoT Edge-extensie.

1. Selecteer in Visual Studio Code **View** > **Command Palette** om het VS Code-opdrachtpalet te openen. 
2. Voer in het opdrachtpalet de opdracht **Azure: Sign in** in en voer deze uit. Volg vervolgens de instructies om u aan te melden bij uw Azure-account. Als u al bent aangemeld, kunt u deze stap overslaan.
3. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Geef in het opdrachtpalet de volgende informatie op om de oplossing te maken: 

   1. Selecteer de map waarin u de oplossing wilt maken. 
   2. Geef een naam op voor de oplossing of houd de standaardnaam **EdgeSolution** aan.
   3. Kies **C# Module** als de modulesjabloon. 
   4. Vervang de standaardnaam van de module door **CSharpModule**. 
   5. Geef het Azure-containerregister dat u in de vorige sectie hebt gemaakt, op als de opslagplaats voor installatiekopieën voor de eerste module. Vervang **localhost:5000** door de gekopieerde waarde voor de aanmeldingsserver. De uiteindelijke tekenreeks ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/csharpmodule.

4.  In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen: de map modules, een map \.vscode, een sjabloonbestand voor het distributiemanifest en een \.env-bestand. Open in VS Code Explorer **modules** > **CSharpModule** > **Program.cs**.

5. Voeg bovenaan de naamruimte **CSharpModule** drie **using**-instructies toe voor typen die later worden gebruikt:

    ```csharp
    using System.Collections.Generic;     // For KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // For TwinCollection
    using Newtonsoft.Json;                // For JsonConvert
    ```

6. Voeg de variabele **temperatureThreshold** toe aan de klasse **Program**. Deze variabele bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar de IoT-hub worden verzonden. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

7. Voeg de klassen **MessageBody**, **Machine** en **Ambient** toe aan de klasse **Program**. Deze klassen bepalen het verwachte schema voor de hoofdtekst van inkomende berichten.

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

8. In de methode **Init** wordt met de code een object **ModuleClient** gemaakt en geconfigureerd. Met dit object kan de module verbinding maken met de lokale Azure IoT Edge-runtime om berichten te verzenden en te ontvangen. De verbindingsreeks die in de methode **Init** wordt gebruikt, wordt door de IoT Edge-runtime aan de module geleverd. Nadat de **ModuleClient** is gemaakt, leest de code de waarde van **temperatureThreshold** uit de gewenste eigenschappen van de moduledubbel. De code registreert een aanroep om berichten van een IoT Edge-hub te ontvangen via het eindpunt **input1**. Vervang de methode **SetInputMessageHandlerAsync** door een nieuwe en voeg een methode **SetDesiredPropertyUpdateCallbackAsync** toe voor updates van de gewenste eigenschappen. Om deze wijziging aan te brengen, moet u de laatste regel van de methode **Init** vervangen door de volgende code:

    ```csharp
    // Register a callback for messages that are received by the module.
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read the TemperatureThreshold value from the module twin's desired properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine($"Property TemperatureThreshold not exist: {e.Message}"); 
    }

    // Attach a callback for updates to the module twin's desired properties.
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);

    // Register a callback for messages that are received by the module.
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Voeg de methode **onDesiredPropertiesUpdate** toe aan de klasse **Program**. Deze methode ontvangt updates van de gewenste eigenschappen van de moduledubbel en updatet de variabele **temperatureThreshold** naar dezelfde waarde. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

    ```csharp
    static Task OnDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
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

10. Vervang de methode **PipeMessage** door de methode **FilterMessages**. Deze methode wordt aangeroepen wanneer de module een bericht ontvangt van de IoT Edge-hub. Berichten die temperaturen onder de drempelwaarde die via de moduledubbel is ingesteld, worden hierdoor gefilterd. Ook wordt de eigenschap **BerichtType** toegevoegd aan het bericht met de waarde ingesteld op **Alarm**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);
        try
        {
            ModuleClient moduleClient = (ModuleClient)userContext;
            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get the message body.
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
                await moduleClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed.
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed.
            var moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed.
            ModuleClient moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Sla dit bestand op.

## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In de vorige sectie hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de **CSharpModule** om berichten te filteren waarin de gemelde temperatuur van de machine onder de aanvaardbare drempelwaarde is. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen. 

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde Visual Studio Code-terminal in te voeren. Vervolgens kunt u de module-installatiekopie naar het Azure-containerregister pushen.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Gebruik de gebruikersnaam, het wachtwoord en de aanmeldingsserver die u in de eerste sectie hebt gekopieerd uit het Azure-containerregister. U kunt deze waarden ook ophalen in de sectie **Toegangssleutels** van het register in de Azure-portal.

2. Open in VS Code Explorer het bestand deployment.template.json in de werkruimte van de IoT Edge-oplossing. Dit bestand zorgt ervoor dat deze twee modules worden geïmplementeerd in **$edgeAgent**: **tempSensor** en **CSharpModule**. De waarde **CSharpModule.image** is ingesteld op een installatiekopie met de versie Linux amd64. 

   Controleer of de sjabloon de juiste modulenaam heeft, niet de **SampleModule**-standaardnaam die u hebt gewijzigd tijdens het maken van de IoT Edge-oplossing.

   Zie [Informatie over het gebruiken, configureren en hergebruiken van IoT Edge-modules](module-composition.md) voor meer informatie over distributiemanifesten.

3. Het bestand deployment.template.json heeft een sectie **registryCredentials** waarin de referenties van het Docker-register zijn opgeslagen. De actuele gebruikersnaam en het bijbehorende wachtwoord zijn opgeslagen in het .env-bestand, dat wordt genegeerd door Git.  

4. Voeg de moduledubbel **CSharpModule** toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onderaan de sectie **moduleContent** in, na de moduledubbel **$edgeHub**: 
    ```json
        "CSharpModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Sla dit bestand op.

5. Klik in VS Code Explorer met de rechtermuisknop op het bestand deployment.template.json en selecteer **IoT Edge-oplossing bouwen**. 

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een bestand deployment.json gemaakt in een nieuwe map genaamd **config** op basis van de informatie in de distributiesjabloon. Vervolgens worden twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt uw code gebouwd, wordt de CSharpModule.dll in een container opgeslagen en wordt de code vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde. 

U kunt het volledige adres van de containerinstallatiekopie, inclusief de tag, zien in de geïntegreerde terminal van VS Code. Het adres van de installatiekopie is opgebouwd uit informatie uit het bestand module.json, in de indeling \<opslagplaats\>:\<versie\>-\<platform\>. Voor deze zelfstudie ziet dit adres er ongeveer als volgt uit: registryname.azurecr.io/csharpmodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>De oplossing implementeren en uitvoeren

1. Configureer de Azure IoT Toolkit-extensie met de verbindingsreeks voor uw IoT-hub: 

    1. Open de VS Code Explorer door **Verkenner** > **bekijken** te selecteren.

    1. Selecteer in de verkenner **Azure IoT Hub-apparaten**, klik op het weglatingsteken (**...**) en kies vervolgens **IoT Hub selecteren**. Volg de instructies om u aan te melden bij uw Azure-account en kies uw IoT-hub. 

       > [!Note]
       > U kunt de installatie ook voltooien door **IoT Hub-verbindingsreeks instellen** te kiezen. Voer in het pop-upvenster de verbindingsreeks voor de IoT-hub in waarmee het IoT Edge-apparaat verbinding maakt.

2. Klik in de verkenner Azure IoT Hub-apparaten met de rechtermuisknop op uw IoT Edge-apparaat. Selecteer vervolgens **Implementatie maken voor IoT Edge-apparaat**. Selecteer het bestand deployment.json in de map config en kies vervolgens **Edge-distributiemanifest selecteren**.

3. Vernieuw de sectie **Azure IoT Hub-apparaten**. U ziet nu dat de nieuwe **CSharpModule** wordt uitgevoerd, samen met de module **TempSensor** en de **$edgeAgent** en **$edgeHub**. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

1. Klik op het weglatingsteken (**...**) en selecteer **Controle D2C-berichten starten** om de gegevens te controleren die binnenkomen bij de IoT-hub.
2. Als u de D2C-berichten voor een specifiek apparaat wilt controleren, klikt u met de rechtermuisknop op dit apparaat in de lijst en selecteert u **Controle D2C-berichten starten**.
3. Voer de opdracht **Azure IoT Hub: Stop monitoring D2C message** uit in het opdrachtpalet om het controleren van berichten te stoppen. 
4. Als u de moduledubbel wilt weergeven of bewerken, klikt u met de rechtermuisknop op deze moduledubbel in de lijst en selecteert u **Moduledubbel bewerken**. Als u de moduledubbel wilt bijwerken, slaat u het dubbele JSON-bestand op, klikt u met de rechtermuisknop in de editor en selecteert u **Moduledubbel bijwerken**.
5. Als u Docker-logboeken wilt bekijken, installeert u [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) voor VS Code. U kunt de actieve modules lokaal zoeken in Docker Explorer. Selecteer in het contextmenu **Logboeken weergeven** om ze te bekijken in de geïntegreerde terminal.
 
## <a name="clean-up-resources"></a>Resources opschonen 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken.

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

> [!IMPORTANT]
> Het verwijderen van de Azure-resources en resourcegroepen kan niet ongedaan worden gemaakt. Wanneer deze items worden verwijderd, worden de resourcegroep en alle bijbehorende resources permanent verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de IoT-hub in een bestaande resourcegroep hebt gemaakt met resources die u wilt behouden, moet u alleen de IoT-hubresource zelf verwijderen in plaats van de resourcegroep te verwijderen.
>

Als u alleen de IoT-hub wilt verwijderen, voert u de volgende opdracht uit met behulp van de naam van de hub en van de resourcegroep:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Ga als volgt te werk om de hele resourcegroep te verwijderen op naam:

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en selecteer **Resourcegroepen**.

2. Voer in het tekstvak **Filteren op naam** de naam van de resourcegroep in die uw IoT-hub bevat. 

3. Selecteer rechts van de resourcegroep, in de resultatenlijst, het weglatingsteken (**...**) en selecteer vervolgens **Resourcegroep verwijderen**.

4. U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Voer ter bevestiging de naam van de resourcegroep nogmaals in en selecteer **Verwijderen**. Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Als u klaar bent om uw eigen modules te bouwen, kunt u meer informatie krijgen over het [ontwikkelen van een C#-module met Azure IoT Edge voor Visual Studio Code](how-to-develop-csharp-module.md). U kunt verdergaan met de volgende zelfstudies om te leren hoe Azure IoT Edge u nog meer kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Gegevens opslaan met SQL Server-databases](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
