---
title: Azure-functies implementeren met Azure IoT Edge | Microsoft Docs
description: In deze zelfstudie implementeert u een Azure-functie als een module op een Edge-apparaat.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 66dcb3347741c4826fc62cc258c67b85f3939ff7
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47422759"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules-preview"></a>Zelfstudie: Azure-functies implementeren als IoT Edge-modules (preview)

U kunt Azure-functies gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw Azure IoT Edge-apparaten implementeert. In deze zelfstudie leert u hoe u een Azure-functie maakt en implementeert die sensorgegevens op het gesimuleerde IoT Edge-apparaat filtert. U gebruikt het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de quickstarts over het implementeren van Azure IoT Edge op een gesimuleerd apparaat in [Windows][lnk-tutorial1-win] of [Linux][lnk-tutorial1-lin]. In deze zelfstudie leert u het volgende:     

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een Azure-functie te maken.
> * VS Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar een containerregister.
> * De module vanuit het containerregister implementeren op uw IoT Edge-apparaat.
> * Gefilterde gegevens weergeven.

<center>
![Architectuurdiagram voor zelfstudie](./media/tutorial-deploy-function/FunctionsTutDiagram.png)
</center>

>[!NOTE]
>Azure Functions-modules in Azure IoT Edge zijn in [openbare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

De Azure-functie die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. De functie verzendt gegevens alleen upstream naar Azure IoT Hub wanneer de temperatuur boven een opgegeven drempelwaarde komt. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).

Cloudresources:

* Een gratis [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [De extensie C# voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code. 
* [De .NET Core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in de [Azure-portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Container Registry**.

    ![Een containerregister maken](./media/tutorial-deploy-function/create-container-registry.png)

2. Geef de volgende waarden op om uw containerregister te maken:

   | Veld | Waarde | 
   | ----- | ----- |
   | Registernaam | Geef hier een unieke naam op. |
   | Abonnement | Selecteer een abonnement in de vervolgkeuzelijst. |
   | Resourcegroep | Het wordt aangeraden om dezelfde resourcegroep te gebruiken voor alle test-resources die u maakt met de snelstartgidsen en zelfstudies voor IoT Edge, zoals **IoTEdgeResources**. |
   | Locatie | Kies een locatie dicht bij u in de buurt. |
   | Beheerder | Stel deze optie in op **Inschakelen**. |
   | SKU | Selecteer **Basic**. | 

5. Selecteer **Maken**.

6. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u vervolgens **Toegangssleutels**. 

7. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie. 

## <a name="create-a-function-project"></a>Een functieproject maken

De Azure IoT Edge-extensie voor Visual Studio Code die u hebt geïnstalleerd als een van de vereiste onderdelen biedt niet alleen beheermogelijkheden, maar ook enkele codesjablonen. In deze sectie gebruikt u Visual Studio Code om een IoT Edge-oplossing te maken waarin een Azure-functie wordt gebruikt. 

1. Open Visual Studio Code op uw ontwikkelcomputer.

2. Selecteer **View** > **Command Palette** en open het VS Code-opdrachtpalet.

3. Voer in het opdrachtpalet de opdracht **Azure: Sign in** in en voer deze uit. Volg de instructies om u aan te melden bij uw Azure-account.

4. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   1. Selecteer de map waarin u de oplossing wilt maken. 
   2. Geef een naam op voor de oplossing of houd de standaardnaam **EdgeSolution** aan.
   3. Kies **Azure Functions - C#** als de modulesjabloon. 
   4. Geef de module de naam **CSharpFunction**. 
   5. Geef het Azure-containerregister dat u in de vorige sectie hebt gemaakt, op als de opslagplaats voor installatiekopieën voor de eerste module. Vervang **localhost:5000** door de gekopieerde waarde voor de aanmeldingsserver. De uiteindelijke tekenreeks ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/csharpfunction.

   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-deploy-function/repository.png)

4. In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen: een map \.vscode, een map modules, een sjabloonbestand voor het distributiemanifest en een \.env-bestand. Open in VS Code Explorer **modules** > **CSharpFunction** > **EdgeHubTrigger-Csharp** > **run.csx**.

5. Vervang de inhoud van het bestand **run.csx** door de volgende code:

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
            // Get the body of the message and deserialize it.
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold.
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object.
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);                }
                // Add a new property to the message to indicate it is an alert.
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message.       
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages.
    class MessageBody
    {
        public Machine machine {get; set;}
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

6. Sla het bestand op.

## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In de vorige sectie hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de **CSharpFunction** om berichten te filteren waarin de gemelde temperatuur van de machine onder de aanvaardbare drempelwaarde is. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen.

In deze sectie geeft u tweemaal de referenties voor het containerregister op. De eerste keer om u lokaal aan te melden vanaf de ontwikkelcomputer zodat Visual Studio Code installatiekopieën naar het register kan pushen. De tweede keer in het **ENV**-bestand van uw IoT Edge-oplossing, waardoor uw IoT-Edge-apparaat bevoegd is voor het ophalen van installatiekopieën uit het register. 

1. Open de met VS Code geïntegreerde terminal door **View** > **Terminal** te selecteren. 

1. Meld u aan bij uw containerregister door de volgende opdracht in te voeren in de geïntegreerde terminal. Vervolgens kunt u de module-installatiekopie naar het Azure-containerregister pushen: 
     
    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Gebruik de gebruikersnaam en de aanmeldingsserver die u eerder hebt gekopieerd uit het Azure-containerregister. Wanneer u wordt gevraagd om het wachtwoord, plakt u het wachtwoord voor uw containerregister en drukt u op **Enter**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

2. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing. Dit bestand geeft aan welke modules de IoT Edge-runtime moet implementeren op een apparaat. U ziet dat uw functiemodule **CSharpFunction** samen wordt vermeld met de module **tempSensor**, die testgegevens aanlevert. Zie [Informatie over het gebruiken, configureren en hergebruiken van IoT Edge-modules](module-composition.md) voor meer informatie over distributiemanifesten.

   ![De module weergeven in het implementatiemanifest](./media/tutorial-deploy-function/deployment-template.png)

3. Open het **ENV**-bestand in de werkruimte van uw IoT Edge-oplossing. In dit door git genegeerde bestand worden de referenties van uw containerregister opgeslagen, zodat u deze niet hoeft op te nemen in de sjabloon voor het implementatiemanifest. Geef de **gebruikersnaam** en het **wachtwoord** voor uw containerregister op. 

5. Sla dit bestand op.

6. Klik in VS Code Explorer met de rechtermuisknop op het bestand deployment.template.json en selecteer **Build and Push IoT Edge solution**. 

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een bestand deployment.json gemaakt in een nieuwe map genaamd **config** op basis van de informatie in de distributiesjabloon. Vervolgens worden twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt uw code gebouwd, worden de functies in een container opgeslagen en wordt de code vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde. 

## <a name="view-your-container-image"></a>De containerinstallatiekopie weergeven

In Visual Studio Code wordt een succesbericht weergegeven wanneer de containerinstallatiekopie naar het containerregister is doorgezet. Als u zelf wilt bevestigen dat de bewerking is geslaagd, kunt u de installatiekopie in het register bekijken. 

1. Blader in de Azure-portal naar het Azure-containerregister. 
2. Selecteer **Opslagplaatsen**.
3. U zou nu de opslagplaats **csharpfunction** in de lijst moeten zien. Selecteer deze opslagplaats om meer details te bekijken.
4. In de sectie **Tags** zou u de tag **0.0.1-amd64** moeten zien. Deze tag bevat de versie en het platform van de installatiekopie die u hebt gemaakt. Deze waarden zijn ingesteld in het bestand module.json in de map CSharpFunction. 

## <a name="deploy-and-run-the-solution"></a>De oplossing implementeren en uitvoeren

U kunt de Azure-portal gebruiken om uw functiemodule op een IoT Edge-apparaat te implementeren, zoals u hebt gedaan in de quickstarts. U kunt modules ook vanuit Visual Studio Code implementeren en bewaken. In de volgende secties wordt de Azure IoT Edge-extensie voor VS Code gebruikt die wordt vermeld in de vereisten. Installeer de extensie nu, als u dat nog niet hebt gedaan. 

1. Selecteer **View** > **Command Palette** en open het VS Code-opdrachtpalet.

2. Zoek de opdracht **Azure: aanmelden** en voer deze uit. Volg de instructies om u aan te melden bij uw Azure-account. 

3. Zoek in het opdrachtpalet de opdracht **Azure IoT Hub: IoT-hub selecteren** en voer deze uit. 

4. Selecteer het abonnement dat de IoT-hub bevat. Selecteer vervolgens de IoT-hub waartoe u toegang wilt.

5. Vouw in VS Code Explorer de sectie **Azure IoT Hub Devices** uit. 

6. Klik met de rechtermuisknop op de naam van het IoT Edge-apparaat en selecteer **Implementatie voor één apparaat maken**. 

7. Blader naar de oplossingsmap die de **CSharpFunction** bevat. Open de map config, selecteer het bestand deployment.json en kies vervolgens **Edge-distributiemanifest selecteren**.

8. Vernieuw de sectie **Azure IoT Hub-apparaten**. U ziet nu dat de nieuwe **CSharpFunction** wordt uitgevoerd, samen met de module **TempSensor** en de **$edgeAgent** en **$edgeHub**. 

   ![Geïmplementeerde modules in VS Code weergeven](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

U kunt alle berichten zien die bij de IoT-hub binnenkomen door in het opdrachtpalet de opdracht **Azure IoT Hub: Start Monitoring D2C Message** uit te voeren.

U kunt de weergave ook filteren om alle berichten te zien die bij de IoT-hub binnenkomen vanuit een specifiek apparaat. Klik met de rechtermuisknop op het apparaat in de sectie **Azure IoT Hub Devices** en selecteer **Start Monitoring D2C Messages**.

Als u het controleren van berichten wilt stoppen, voert u de opdracht **Azure IoT Hub: Stop monitoring D2C message** in het opdrachtpalet uit. 


## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure-functiemodule gemaakt met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u klaar bent om uw eigen modules te bouwen, kunt u meer informatie krijgen over het [ontwikkelen van Azure-functies met Azure IoT Edge voor Visual Studio Code](how-to-develop-csharp-function.md). 

Ga verder met de volgende zelfstudies om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Gemiddelden zoeken met een zwevend venster in Azure Stream Analytics](tutorial-deploy-stream-analytics.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
