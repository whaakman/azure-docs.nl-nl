---
title: Een Azure-functie implementeren met Azure IoT Edge | Microsoft Docs
description: Een Azure-functie als een module voor een Edge-apparaat implementeren
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 04/02/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 6102a28ec92f841fe32652e4dac36848d69e389c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631697"
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Een Azure-functie als een IoT Edge-module implementeren - preview
U kunt Azure Functions gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie doorloopt u het maken en implementeren van een Azure-functie waarmee sensorgegevens worden gefilterd op het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de zelfstudie Azure IoT-Edge implementeren op een gesimuleerd apparaat in [Windows][lnk-tutorial1-win]of [Linux][lnk-tutorial1-lin]. In deze zelfstudie leert u het volgende:     

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een Azure-functie te maken
> * VS Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register 
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De Azure-functie die u in deze zelfstudie maakt, filtert de temperatuurgegevens die worden gegenereerd door uw apparaat en stuurt alleen upstreamberichten naar Azure IoT Hub wanneer de temperatuur hoger is dan een opgegeven drempelwaarde. 

## <a name="prerequisites"></a>Vereisten

* Het Azure IoT Edge-apparaat dat hebt gemaakt in de quickstart of de vorige zelfstudie.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [De extensie C# voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). De Community Edition (CE) voor uw platform is voldoende voor deze zelfstudie. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Azure Container Registry**.
2. Geef een naam op voor het register, kies een abonnement, kies een resourcegroep en stel de SKU in op **Basis**. 
3. Selecteer **Maken**.
4. Zodra het containerregister is gemaakt, gaat u er naartoe en selecteert u **Toegangssleutels**. 
5. Stel de **Gebruiker met beheerdersrechten** in op **Inschakelen**.
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie. 

## <a name="create-a-function-project"></a>Een functieproject maken
De volgende stappen laten zien hoe u een IoT-Edge-functie maakt met behulp van Visual Studio Code en de Azure IoT Edge-extensie.
1. Open Visual Studio Code.
2. Om de met VS Code geïntegreerde terminal te openen, selecteert u **View** > **Integrated Terminal**.
3. Voer de volgende opdracht in de geïntegreerde terminal uit om de **AzureIoTEdgeFunction**-sjabloon in dotnet te installeren (of bij te werken):

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Maak een project voor de nieuwe module. Met de volgende opdracht maakt u de projectmap, **FilterFunction**, met de containeropslagplaats. De tweede parameter moet een indeling vergelijkbaar met `<your container registry name>.azurecr.io` hebben als u Azure Container Registry gebruikt. Voer de volgende opdracht in de huidige werkmap in:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction -r <your container registry address>/filterfunction
    ```

3. Selecteer **File** > **Open Folder**, blader naar de map **FilterFunction** en open het project in VS Code.
4. Vouw in VS Code Explorer de map **EdgeHubTrigger-Csharp** uit en open het bestand **run.csx**.
5. Vervang de inhoud van het bestand door de volgende code:

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
                    filteredMessage.Properties.Add(prop.Key, prop.Value);                }
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

11. Sla het bestand op.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde VS Code-terminal in te voeren: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Ga naar de [Azure Portal] https://portal.azure.com) om de gebruikersnaam, het wachtwoord en de aanmeldingsserver op te zoeken die in deze opdracht moeten worden gebruikt. Klik vanuit **Alle resources** op de tegel voor uw Azure-containerregister om de eigenschappen te openen en klik vervolgens op **Toegangssleutels**. Kopieer de waarden in de velden **Gebruikersnaam**, **Wachtwoord** en **Aanmeldingsserver**. 

2. Open **module.json**. U kunt de `"version"` ook bijwerken naar bijvoorbeeld **"1.0"**. U ziet ook de naam van de opslagplaats die u hebt opgegeven in de `-r`-parameter van `dotnet new aziotedgefunction`.

3. Sla het bestand **module.json** op.

4. Klik in VS Code explorer met de rechtermuisknop op het bestand **module.json** en klik op **Build and Push IoT Edge module Docker image** (Docker-installatiekopie voor IoT Edge-module maken en pushen). Selecteer in de vervolgkeuzelijst bovenaan het venster VS Code uw containerplatform: **amd64** voor een Linux-container of **windows-amd64** voor een Windows-container. VS Code plaatst vervolgens uw functiecodes in een container en pusht deze naar het containerregister dat u hebt opgegeven.

5. U kunt het volledige adres van de containerinstallatiekopie, inclusief tag, in de geïntegreerde terminal van VS Code krijgen. Bekijk het `module.json`-bestand als u meer wilt weten over de build- en push-definitie.

## <a name="add-registry-credentials-to-your-edge-device"></a>Registerreferenties toevoegen aan uw Edge-apparaat
Voeg de referenties voor uw register toe aan de Edge-runtime op de computer waarop u het Edge-apparaat uitvoert. Dit geeft de runtime toegang voor het ophalen van de container. 

- Voer voor Windows de volgende opdracht uit:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Voer voor Linux de volgende opdracht uit:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>De oplossing uitvoeren

1. Navigeer in **Azure Portal** naar uw IoT-hub.
2. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
1. Selecteer **Modules instellen**. 
2. Als u de **tempSensor**-module al hebt geïmplementeerd op dit apparaat, is deze mogelijk automatisch ingevuld. Als dat niet het geval is, gaat u als volgt te werk om deze toe te voegen:
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `tempSensor` in.
    3. Voer in het veld **URI installatiekopie** `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` in.
    4. Laat de overige instellingen ongewijzigd en klik op **Opslaan**.
1. Voeg de **filterFunction**-module toe.
    1. Selecteer opnieuw **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `filterFunction` in.
    3. Voer in het veld **URI installatiekopie** het adres van uw installatiekopie in, bijvoorbeeld `<your container registry address>/filterfunction:0.0.1-amd64`. Het volledige adres van de installatiekopie vindt u in de vorige sectie.
    74. Klik op **Opslaan**.
2. Klik op **Volgende**.
3. Kopieer in de stap **Routes opgeven** de onderstaande JSON in het tekstvak. Met de eerste route worden berichten van de temperatuursensor naar de filtermodule getransporteerd via het eindpunt 'input1'. Met de tweede route worden berichten van de filtermodule naar IoT Hub getransporteerd. In deze route is `$upstream` een speciale bestemming die Edge Hub opdracht geeft berichten te verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klik op **Volgende**.
5. Klik in de stap **Sjabloon controleren** op **Indienen**. 
6. Ga terug naar de detailpagina van het IoT Edge-apparaat en klik op **Vernieuwen**. U ziet nu dat de nieuwe **filterfunction**-module samen met de **tempSensor**-module en de **IoT Edge-runtime** wordt uitgevoerd. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Ga als volgt te werk om het apparaat te controleren op cloudberichten die vanaf uw IoT Edge-apparaat worden verzonden naar uw IoT Hub:
1. Configureer de Azure IoT Toolkit-extensie met de verbindingsreeks voor uw IoT Hub: 
    1. Navigeer in Azure Portal naar uw IoT Hub en selecteer **Beleid voor gedeelde toegang**. 
    2. Selecteer **iothubowner** en kopieer de waarde van **Verbindingsreeks - primaire sleutel**.
    3. Klik in de VS Code Explorer op **IOT HUB DEVICES** en klik vervolgens op **...** . 
    4. Selecteer **Set IoT Hub Connection String** en voer de verbindingsreeks van de Iot Hub in het pop-upvenster in. 

2. Om gegevens die binnenkomen in de IoT Hub te controleren, selecteert u **View** > **Command Palette...** en zoekt u naar **IoT: Start monitoring D2C message**. 
3. Als u wilt stoppen met het controleren van gegevens, gebruikt u de opdracht **IoT: Stop monitoring D2C message** in de Command Palette. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure-functie gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Als u meer wilt weten over Azure IoT Edge, kunt u lezen over het gebruik van een IoT Edge-apparaat als een gateway. 

> [!div class="nextstepaction"]
> [Een IoT Edge-gatewayapparaat maken](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
