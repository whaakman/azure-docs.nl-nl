---
title: Zelfstudie ontwikkelen C# -module voor Windows - Azure IoT Edge | Microsoft Docs
description: Deze zelfstudie leert u over het maken van een IoT Edge-module met C# code en deze implementeren in een Windows IoT Edge-apparaat.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 046398af8678e708784614dfdc231778454ed945
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576598"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-windows-devices"></a>Zelfstudie: Ontwikkel een C# IoT Edge-module voor Windows-apparaten

Visual Studio gebruiken voor het ontwikkelen van C# code en deze implementeren in een Windows-apparaat met Azure IoT Edge. 

U kunt Azure IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio gebruiken voor het maken van een IoT Edge-module die gebaseerd op de C# SDK.
> * Gebruik Visual Studio en Docker om te maken van een Docker-installatiekopie en deze publiceren naar uw register.
> * De module implementeren op uw IoT Edge-apparaat.
> * Gegenereerde gegevens weergeven.

De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheid gegevens te reduceren die worden gecommuniceerd naar en worden opgeslagen in de cloud. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Bereik van de oplossing

In deze zelfstudie laat zien hoe u voor het ontwikkelen van een module in **C#** met behulp van **Visual Studio 2017**, en hoe u implementeert een **Windows-apparaat**. Als u modules voor Linux-apparaten ontwikkelt, gaat u naar [ontwikkelen een C# IoT Edge-module voor Linux-apparaten](tutorial-csharp-module.md) in plaats daarvan. 

Gebruik de volgende tabel om te begrijpen van de opties voor het ontwikkelen en implementeren van modules in C op Windows-apparaten: 

| C# | Visual Studio Code | Visual Studio 2017 | 
| -- | ------------------ | ------------------ |
| **Windows AMD64 develop** | ![Ontwikkel C# modules voor WinAMD64 in VS Code](./media/tutorial-c-module/green-check.png) | ![Ontwikkel C# modules voor WinAMD64 in Visual Studio](./media/tutorial-c-module/green-check.png) |
| **Windows-AMD64-foutopsporing** |   | ![Fouten opsporen in C# modules voor WinAMD64 in Visual Studio](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u zijn gebleven door middel van de vorige zelfstudie over het instellen van uw ontwikkelomgeving [ontwikkelen van een IoT Edge-module voor een Windows-apparaat](tutorial-develop-for-windows.md). Na het voltooien van deze zelfstudie, moet u al de volgende vereisten zijn: 

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.
* Een [Windows-apparaat met Azure IoT Edge](quickstart.md).
* Een containerregister, bijvoorbeeld [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio?view=vs-2017), versie 15.7 of hoger, geconfigureerd met de [hulpprogramma's voor Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) extensie.
* [Docker CE](https://docs.docker.com/install/) geconfigureerd voor het uitvoeren van Windows-containers.

## <a name="create-a-module-project"></a>Een module-project maken

De volgende stappen maakt u een IoT Edge-module-project met behulp van Visual Studio en de hulpprogramma's voor Azure IoT Edge-extensie. Zodra u een projectsjabloon gemaakt hebt, voegt u nieuwe code toe zodat de module berichten op basis van de gerapporteerde eigenschappen filtert. 

### <a name="create-a-new-project"></a>Een nieuw project maken

De hulpprogramma's voor Azure IoT-extensie biedt projectsjablonen voor alle ondersteunde IoT Edge module talen in Visual Studio 2017. Deze sjablonen zijn alle bestanden en code die u nodig hebt voor het implementeren van een module werken als u wilt testen van IoT Edge of geeft u een beginpunt voor het aanpassen van de sjabloon met uw eigen bedrijfslogica. 

1. Visual Studio als beheerder uitvoeren.

2. Selecteer **Bestand** > **Nieuw** > **Project**. 

3. Selecteer in het venster Nieuw project, de **Azure IoT** type toepassingsproject en kies de **Azure IoT Edge** project. Wijzig de naam van het project en de oplossing iets beschrijvende als volgt aan bij **CSharpTutorialApp**. Selecteer **OK** om het project te maken. 

   ![Een nieuw Azure IoT Edge-project maken](./media/tutorial-csharp-module-windows/new-project.png)

4. In de IoT Edge-toepassing en het modulevenster, configureert u uw project met de volgende waarden: 

   | Veld | Value |
   | ----- | ----- |
   | Platform voor toepassingen | Schakel het selectievakje **Linux Amd64**, en Controleer **WindowsAmd64**. |
   | Selecteer een sjabloon | Selecteer  **C# Module**. | 
   | Naam van de module-project | Geef de module de naam **CSharpModule**. | 
   | Opslagplaats voor docker-installatiekopieën | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. Uw containerinstallatiekopie wordt vooraf ingevuld vanuit de waarde van de naam van de module project. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br> De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/csharpmodule. |

   ![Configureren van uw project voor het doelapparaat en module-type containerregister](./media/tutorial-csharp-module-windows/add-application-and-module.png)

5. Selecteer **OK** de wijzigingen worden toegepast. 

### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

Het manifest implementatie deelt de referenties voor uw container registry met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen. Gebruik de referenties van de **toegangssleutels** sectie van uw Azure container registry. 

1. Open in Visual Studio solution explorer, de **deployment.template.json** bestand. 

2. Zoek de **registryCredentials** eigenschap in de $edgeAgent gewenste eigenschappen. 

3. Werk de eigenschap met de referenties, deze indeling te volgen: 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }

4. Save the deployment.template.json file. 

### Update the module with custom code

The default module code receives messages on an input queue and passes them along through an output queue. Let's add some additional code so that the module processes the messages at the edge before forwarding them to IoT Hub. Update the module so that it analyzes the temperature data in each message, and only sends the message to IoT Hub if the temperature exceeds a certain threshold. 

1. In Visual Studio, open **CSharpModule** > **Program.cs**.

2. At the top of the **CSharpModule** namespace, add three **using** statements for types that are used later:

    ```csharp
    using System.Collections.Generic;     // For KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // For TwinCollection
    using Newtonsoft.Json;                // For JsonConvert
    ```

3. Voeg de **temperatureThreshold** variabele de **programma** klasse na de tellervariabele. De variabele temperatureThreshold Hiermee stelt u de waarde die de gemeten temperatuur moet groter zijn dan voor de gegevens worden verzonden naar de IoT-hub. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

4. Toevoegen de **MessageBody**, **Machine**, en **Ambient** klassen op de **programma** klasse na de variabelendeclaraties. Deze klassen bepalen het verwachte schema voor de hoofdtekst van inkomende berichten.

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

5. Zoek de **Init** methode. Deze methode maakt en configureert een **ModuleClient** object, waardoor de module verbinding maken met de lokale Azure IoT Edge-runtime voor het verzenden en ontvangen van berichten. De code registreert ook een retouraanroep voor het ontvangen van berichten van een IoT Edge hub via de **input1** eindpunt.

   Vervang de gehele Init-methode door de volgende code:
   
   ```csharp
   static async Task Init()
   {
       AmqpTransportSettings amqpSetting = new AmqpTransportSettings(TransportType.Amqp_Tcp_Only);
       ITransportSettings[] settings = { amqpSetting };

       // Open a connection to the Edge runtime
       ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
       await ioTHubModuleClient.OpenAsync();
       Console.WriteLine("IoT Hub module client initialized.");

       // Read the TemperatureThreshold value from the module twin's desired properties
       var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
       await OnDesiredPropertiesUpdate(moduleTwin.Properties.Desired, ioTHubModuleClient);

       // Attach a callback for updates to the module twin's desired properties.
       await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);

       // Register a callback for messages that are received by the module.
       await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
   }
   ```
   
   Deze bijgewerkte Init-methode nog steeds stelt u de verbinding met de IoT Edge-runtime met de ModuleClient, maar ook nieuwe functionaliteit wordt toegevoegd. Deze leest de module de gewenste eigenschappen van de apparaatdubbel om op te halen de **temperatureThreshold** waarde. Vervolgens wordt een callbackfunctie die naar alle toekomstige updates voor de gewenste eigenschappen van de moduledubbel luistert. Met deze terugbellen, kunt u de drempelwaarde temperatuur in de moduledubbel op afstand bijwerken en de wijzigingen worden opgenomen in de module. 

   De bijgewerkte Init-methode verandert ook de bestaande **SetInputMessageHandlerAsync** methode. In de voorbeeldcode voor inkomende berichten op *input1* worden verwerkt met de *PipeMessage* functie, maar we wilt wijzigen die u wilt gebruiken de *FilterMessages* die functie We maken in de volgende stappen. 

6. Toevoegen van een nieuwe **onDesiredPropertiesUpdate** methode de **programma** klasse. Deze methode ontvangt updates van de gewenste eigenschappen van de moduledubbel en updatet de variabele **temperatureThreshold** naar dezelfde waarde. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

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

7. Verwijderen van het voorbeeld **PipeMessage** methode en vervang deze door een nieuwe **FilterMessages** methode. Deze methode wordt aangeroepen wanneer de module een bericht ontvangt van de IoT Edge-hub. Berichten die temperaturen onder de drempelwaarde die via de moduledubbel is ingesteld, worden hierdoor gefilterd. Ook wordt de eigenschap **BerichtType** toegevoegd aan het bericht met de waarde ingesteld op **Alarm**. 

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

8. Sla het bestand Program.cs op.

9. Open de **deployment.template.json** bestand in uw IoT Edge-oplossing. Dit bestand laat aan de IoT Edge-agent weten welke modules moeten worden geïmplementeerd. In dit geval gaat het om **tempSensor** en **CSharpModule**. Het bestand laat de IoT Edge-hub ook weten hoe berichten tussen de modules moeten worden gerouteerd.

10. Voeg de moduledubbel **CSharpModule** toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onder aan de sectie **modulesContent** in, na de moduledubbel **$edgeHub**: 

    ```json
       "CSharpModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
    ```

    ![Moduledubbel toevoegen aan implementatiesjabloon](./media/tutorial-csharp-module-windows/module-twin.png)

11. Sla het bestand deployment.template.json op.


## <a name="build-and-push-your-module"></a>Bouw en stuur uw module

In de vorige sectie hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de **CSharpModule** om berichten te filteren waarin de gemelde temperatuur van de machine onder de aanvaardbare drempelwaarde is. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen. 

1. Gebruik de volgende opdracht op uw ontwikkelcomputer aanmelden bij Docker. Gebruik de gebruikersnaam, wachtwoord en aanmeldingsserver van het Azure container registry. U vindt deze waarden uit de **toegangssleutels** gedeelte van uw register in Azure portal.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   U ontvangt mogelijk een beveiligingswaarschuwing voor het gebruik van aanbevelen `--password-stdin`. Hoewel deze aanbevolen procedure wordt aanbevolen voor productiescenario's, valt buiten het bereik van deze zelfstudie. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) verwijzing.

2. In Visual Studio solution explorer met de rechtermuisknop op de naam van het project dat u wilt maken. De standaardnaam is **AzureIotEdgeApp1** en omdat u een Windows-module bouwt, de uitbreiding moet **Windows.Amd64**. 

3. Selecteer **maken en pushen van IoT Edge-Modules**. 

   De opdracht build- en push start drie bewerkingen. Eerst een nieuwe map wordt gemaakt in de oplossing met de naam **config** die de volledige implementatie oplossing voor het manifest, gebouwd om van gegevens in de sjabloon voor de implementatie en andere bestanden bevat. Ten tweede, deze wordt uitgevoerd `docker build` om de containerinstallatiekopie op basis van het dockerfile dat geschikt is voor uw doel-architectuur te bouwen. Vervolgens wordt deze uitgevoerd `docker push` naar de opslagplaats voor installatiekopieën pushen naar uw containerregister. 

## <a name="deploy-modules-to-device"></a>Modules op apparaat implementeren

De Visual Studio cloud explorer en de hulpprogramma's voor Azure IoT Edge-extensie gebruiken aan het project module implementeren in uw IoT Edge-apparaat. U hebt al een implementatie-manifest voorbereid voor uw scenario, de **deployment.json** bestand in de config-map. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

Zorg ervoor dat uw IoT Edge-apparaat actief en werkend is. 

1. Vouw in de Visual Studio cloud explorer, de resources voor uw overzicht van IoT-apparaten. 

2. Met de rechtermuisknop op de naam van de IoT Edge-apparaat dat u wilt ontvangen van de implementatie. 

3. Selecteer **implementatie maken-**.

4. Selecteer in de bestandenverkenner de **deployment.windows amd64** bestand in de map van de configuratie van uw oplossing. 

5. Vernieuwen van de cloud explorer om te zien van de geïmplementeerde modules vermeld in uw apparaat. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart. 

U kunt de hulpprogramma's voor IoT Edge-extensie gebruiken om berichten te bekijken wanneer ze op uw IoT-Hub binnenkomen. 

1. Selecteer in de Visual Studio cloud explorer, de naam van uw IoT Edge-apparaat. 

2. In de **acties** in de lijst met **Start Monitoring D2C Message**. 

3. Bekijk de berichten die binnenkomen in uw IoT-Hub. Het duurt even voor de berichten binnenkomen, omdat we de wijzigingen in de code CSharpModule wachten totdat de temperatuur machine 25 graden is bereikt voordat het verzenden van berichten. Het berichttype ook toegevoegd **waarschuwing** in de berichten die deze temperatuur drempelwaarde bereikt. 

   ![Berichten weergeven die binnenkomen in IoT Hub](./media/tutorial-csharp-module-windows/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>De moduledubbel bewerken

We de moduledubbel CSharpModule gebruikt voor het instellen van de drempelwaarde voor de temperatuur op 25 graden. De moduledubbel kunt u de functionaliteit wijzigen zonder de code van de module bijwerken.

1. Open in Visual Studio, de **deployment.windows amd64.json** bestand. (Niet in het bestand deployment.template. Als er geen de implementatie in het configuratiebestand in solution explorer het manifest, selecteert u de **weergeven van alle bestanden** pictogram in de Verkenner.)

2. Het dubbele CSharpModule vinden en wijzig de waarde van de **temperatureThreshold** parameter voor een nieuwe temperatuur 5 graden tot 10 graden hoger is dan de meest recente gemelde temperatuur. 

3. Sla de **deployment.windows amd64.json** bestand.

4. Volg de implementatiestappen opnieuw uit om te het manifest van de bijgewerkte implementatie toepassen op uw apparaat. 

5. De binnenkomende berichten voor apparaat-naar-cloud bewaken. U ziet de berichten die niet meer totdat de nieuwe temperatuur drempelwaarde is bereikt. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en de Azure-resources dat u in dit artikel gebruikt om kosten te vermijden verwijderen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u gereed voor het bouwen van uw eigen modules bent, vindt u meer informatie over [ontwikkelen van uw eigen IoT Edge-modules](module-development.md) of hoe u [modules met Visual Studio ontwikkelen](how-to-visual-studio-develop-module.md). U kunt doorgaan naar de volgende zelfstudies voor meer informatie over hoe Azure IoT Edge kunnen helpen bij het implementeren van Azure cloudservices om te verwerken en analyseren van gegevens aan de rand.

> [!div class="nextstepaction"]
> [Functies](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Machine Learning](tutorial-deploy-machine-learning.md)
> [Custom Vision Service](tutorial-deploy-custom-vision.md)
