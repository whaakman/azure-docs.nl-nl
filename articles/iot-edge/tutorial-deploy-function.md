---
title: Azure-functie met Azure IoT-rand implementeren | Microsoft Docs
description: Azure-functie als een module die u wilt een edge-apparaat implementeren
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1dfe46d307a076ae02362c4bba292602001ed915
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Implementeren van Azure-functie als een module van de rand van de IoT - voorbeeld
U kunt Azure Functions gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks aan de rand van de IoT-apparaten implementeert. In deze zelfstudie wordt u begeleid bij het maken en implementeren van een Azure-functie waarmee sensorgegevens op het gesimuleerde rand van de IoT-apparaat dat u hebt gemaakt in de implementatie van Azure IoT-Edge van een gesimuleerd apparaat op gefilterd [Windows] [ lnk-tutorial1-win]of [Linux] [ lnk-tutorial1-lin] zelfstudies. In deze zelfstudie leert u het volgende:     

> [!div class="checklist"]
> * Visual Studio Code gebruiken om te maken van een Azure-functie
> * VS-Code en Docker gebruiken voor het maken van een installatiekopie van een Docker en deze publiceren naar het register 
> * De module implementeren voor uw IoT-randapparaat
> * Gegevens weergeven die zijn gegenereerd


De Azure-functie die u in deze zelfstudie maakt de temperatuur-gegevens die worden gegenereerd door het apparaat worden gefilterd en upstream-berichten worden alleen naar Azure IoT Hub verzendt wanneer de temperatuur hoger dan een opgegeven drempelwaarde is. 

## <a name="prerequisites"></a>Vereisten

* Azure IoT apparaat aan de rand die u hebt gemaakt in de Quick Start of de vorige zelfstudie.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C# voor Visual Studio Code (via OmniSharp)-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). De Community Edition (CE) voor uw platform is voldoende voor deze zelfstudie. 
* [.NET core SDK 2.0](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie maakt u de extensie Azure IoT rand voor VS-Code kunt gebruiken om te bouwen van een module en maak een **container installatiekopie** van de bestanden. En u deze installatiekopie push een **register** die worden opgeslagen en beheerd van uw afbeeldingen. U tot slot implementeren de installatiekopie uit het register uit te voeren op uw IoT-randapparaat.  

Voor deze zelfstudie kunt u een register Docker-compatibel. Twee populaire Docker Registerservices beschikbaar in de cloud zijn [Azure Container register](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Deze zelfstudie wordt Azure Container register. 

1. In de [Azure-portal](https://portal.azure.com), selecteer **maken van een resource** > **Containers** > **Azure Container register** .
2. Geef een naam op voor het register, kies een abonnement, kiest u een resourcegroep en de SKU ingesteld op **Basic**. 
3. Selecteer **Maken**.
4. Zodra het register van de container is gemaakt, gaat u naar het en selecteer **toegangssleutels**. 
5. Wisselknop **gebruiker met beheerdersrechten** naar **inschakelen**.
6. Kopieer de waarden voor **Login-server**, **gebruikersnaam**, en **wachtwoord**. Verderop in de zelfstudie gebruikt u deze waarden. 

## <a name="create-a-function-project"></a>Een functie-project maken
De volgende stappen laten zien hoe een IoT-Edge-functie met behulp van Visual Studio Code en de extensie Azure IoT rand maken.
1. Open Visual Studio Code.
2. Als de Code van de VS geïntegreerde terminal, schakelt **weergave** > **geïntegreerde Terminal**.
3. Om te installeren (of werk) de **AzureIoTEdgeFunction** sjabloon in dotnet, voer de volgende opdracht in de geïntegreerde terminal:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Maak een project voor de nieuwe module. De volgende opdracht maakt u de projectmap **FilterFunction**, in de huidige map:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. Selecteer **bestand** > **map openen**, bladert u naar de **FilterFunction** map en open het project in VS-Code.
4. Vouw in VS-Code explorer de **EdgeHubTrigger Csharp** map, open vervolgens de **run.csx** bestand.
5. Vervang de inhoud van het bestand met de volgende code:

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

11. Sla het bestand op.

## <a name="publish-a-docker-image"></a>Een installatiekopie van een Docker publiceren

1. De Docker-installatiekopie bouwen.
    1. Vouw in VS-Code explorer de **Docker** map. Vouw vervolgens ofwel de map voor uw platform container **linux x64** of **windows nano**. 
    2. Met de rechtermuisknop op de **Dockerfile** -bestand en klik op **bouwen IoT rand module Docker installatiekopie**. 
    3. Navigeer naar de **FilterFunction** projectmap en klik op **map selecteren als EXE_DIR**. 
    4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<your container registry address>/filterfunction:latest`. De Register-adres van de container is hetzelfde als de aanmeldingsserver die u hebt gekopieerd uit het register. Deze moet de vorm van `<your container registry name>.azurecr.io`.
 
4. Aanmelden bij Docker. In de geïntegreerde terminal en voer de volgende opdracht: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Als u de gebruikersnaam, wachtwoord en meld u aan server moet worden gebruikt in deze opdracht zoekt, gaat u naar de [Azure portal] (https://portal.azure.com). Van **alle resources**, klikt u op de tegel voor uw Azure-container register open de eigenschappen en klik vervolgens op **toegangssleutels**. Kopieer de waarden in de **gebruikersnaam**, **wachtwoord**, en **Login-server** velden. 

3. De installatiekopie naar uw opslagplaats Docker forceren. Selecteer **weergave** > **opdracht palet...**  zoek vervolgens naar **rand: Push IoT rand module Docker installatiekopie**.
4. Typ in het pop-tekst dezelfde naam van de installatiekopie die u hebt gebruikt in stap 1.d.

## <a name="add-registry-credentials-to-your-edge-device"></a>Voeg register referenties toe aan het apparaat van rand
De referenties voor uw register toevoegen aan de rand runtime op de computer waarop u het apparaat aan de rand worden uitgevoerd. Dit geeft de runtime toegang voor het ophalen van de container. 

- Voor Windows, moet u de volgende opdracht uitvoeren:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Voer de volgende opdracht voor Linux:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Uitvoeren van de oplossing

1. In de **Azure-portal**, gaat u naar uw IoT-hub.
2. Ga naar **IoT rand (preview)** en selecteer het apparaat IoT rand.
1. Selecteer **Modules ingesteld**. 
2. Als u al hebt geïmplementeerd de **tempSensor** module voor dit apparaat, wordt mogelijk automatisch ingevuld. Als dat niet het geval is, moet u als volgt te werk om dit te voegen:
    1. Selecteer **IoT rand Module toevoegen**.
    2. In de **naam** veld `tempSensor`.
    3. In de **installatiekopie URI** veld `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Laat de overige instellingen ongewijzigd en klik op **opslaan**.
1. Voeg de **filterFunction** module.
    1. Selecteer **IoT rand Module toevoegen** opnieuw.
    2. In de **naam** veld `filterFunction`.
    3. In de **installatiekopie** en voer het adres klopt; bijvoorbeeld `<docker registry address>/filterfunction:latest`.
    74. Klik op **Opslaan**.
2. Klik op **Volgende**.
3. In de **Routes opgeven** stap, kopieert u de JSON onder in het tekstvak. De eerste route transporten berichten van de temperatuursensor naar de filtermodule via het eindpunt 'input1'. De tweede route transporten berichten van de module expressiefilter IoT-hub. In deze route `$upstream` is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klik op **Volgende**.
5. In de **Template bekijken** stap, klikt u op **indienen**. 
6. Ga terug naar de detailpagina voor rand van de IoT-apparaat en klikt u op **vernieuwen**. U ziet nu de nieuwe **filterfunction** module uitgevoerd samen met de **tempSensor** module en de **IoT rand runtime**. 

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

Voor het bewaken van het apparaat verzonden van uw IoT-randapparaat naar uw IoT-hub voor cloud-berichten:
1. De Azure IoT Toolkit-uitbreiding configureren met de verbindingsreeks voor uw IoT-hub: 
    1. Navigeer naar uw IoT-hub in de Azure portal en selecteer **gedeeld toegangsbeleid**. 
    2. Selecteer **iothubowner** kopieert u de waarde van **Connection string-primaire sleutel**.
    1. Klik in de Verkenner VS-Code op **IOT HUB-apparaten** en klik vervolgens op **...** . 
    1. Selecteer **verbindingsreeks van de IoT-Hub ingesteld** en voert u de verbindingsreeks van de Iot-Hub in het pop-upvenster. 

1. Voor het bewaken van gegevens die binnenkomen in de IoT-hub, selecteer de **weergave** > **opdracht palet...**  en zoek naar **IoT: D2C bericht bewaking starten**. 
2. Als u wilt stoppen met het bewaken van gegevens, gebruikt u de **IoT: D2C bericht bewaking stopt** opdracht in het palet opdracht. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Azure-functie met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT-randapparaat gemaakt. Informatie over het gebruik van een IoT-randapparaat als een gateway om te blijven verkennen van Azure IoT rand. 

> [!div class="nextstepaction"]
> [Een IoT-Edge gateway-apparaat maken](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
