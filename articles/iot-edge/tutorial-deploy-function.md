---
title: Azure-functie met Azure IoT-rand implementeren | Microsoft Docs
description: Azure-functie als een module die u wilt een edge-apparaat implementeren
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 34ed5083b952c42d4ed119b6986db965eb9eb67a
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
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
* [C# voor Visual Studio Code (via OmniSharp)-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (U kunt de uitbreiding installeren vanuit het deelvenster uitbreidingen in Visual Studio Code.)
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (U kunt de uitbreiding installeren vanuit het deelvenster uitbreidingen in Visual Studio Code.)
* [Docker](https://docs.docker.com/engine/installation/). De Community Edition (CE) voor uw platform is voldoende voor deze zelfstudie. 
* [.NET core SDK 2.0](https://www.microsoft.com/net/core#windowscmd). 

## <a name="set-up-a-docker-registry"></a>Instellen van een Docker-register
In deze zelfstudie maakt u de extensie Azure IoT rand voor VS Code gebruiken voor het maken van een Azure-functie en het bouwen een [Docker-afbeelding](https://docs.docker.com/glossary/?term=image) aan. En u deze installatiekopie Docker push een [Docker-opslagplaats](https://docs.docker.com/glossary/?term=repository) gehost door een [Docker-register](https://docs.docker.com/glossary/?term=registry). Ten slotte het implementeren van uw Docker-installatiekopie geleverd als een [Docker-container](https://docs.docker.com/glossary/?term=container) uit het register naar uw IoT-Edge-apparaat.  

Voor deze zelfstudie kunt u een register Docker-compatibel. Twee populaire Docker Registerservices beschikbaar in de cloud zijn **Azure Container register** en **Docker Hub**:

- [Azure Container register](https://docs.microsoft.com/en-us/azure/container-registry/) is beschikbaar met een [betaald abonnement](https://azure.microsoft.com/en-us/pricing/details/container-registry/). Voor deze zelfstudie de **Basic** abonnement is voldoende. 

- [Docker-Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) biedt een gratis persoonlijke opslagplaats als u zich aanmeldt voor een (gratis) Docker-ID. 
    1. Als u wilt aanmelden voor een Docker-ID, volg de instructies in [registreren voor een Docker-ID](https://docs.docker.com/docker-id/#register-for-a-docker-id) op de Docker-site. 

    2. Volg de instructies in voor het maken van een persoonlijke Docker-opslagplaats [maken van een nieuwe opslagplaats op Docker Hub](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) op de Docker-site.

In deze zelfstudie dat eventueel opdrachten zijn beschikbaar voor zowel de Azure Container register en de Docker-Hub.

## <a name="create-a-function-project"></a>Een functie-project maken
De volgende stappen laten zien hoe een IoT-Edge-functie met behulp van Visual Studio Code en de extensie Azure IoT rand maken.
1. Open VS-Code.
2. Gebruik de **weergave | Geïntegreerde Terminal** menuopdracht openen van de VS Code geïntegreerde terminal.
3. Voer de volgende opdracht om te installeren (of werk) in de geïntegreerde terminal de **AzureIoTEdgeFunction** sjabloon in dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Voer de volgende opdracht voor het maken van een project voor de nieuwe module in de geïntegreerde terminal:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

    >[!NOTE]
    > Deze opdracht maakt u de projectmap **FilterFunction**, in de huidige map. Als u maken in een andere locatie wilt, wijzig de mappen voordat de opdracht uit te voeren.

3. Gebruik de **bestand | Open de map** menuopdracht, blader naar de **FilterFunction** map en klik op **map selecteren** openen van het project in VS-Code.
4. Klik in Verkenner VS-Code op de **EdgeHubTrigger Csharp** map, klikt u vervolgens op de **run.csx** bestand om dit te openen.
5. Voeg de volgende instructie na de `#r "Microsoft.Azure.Devices.Client"` instructie:

    ```csharp
    #r "Newtonsoft.Json"
    ```

5. Voeg de volgende using-instructies na de bestaande `using` instructies:

    ```csharp
    using Newtonsoft.Json;
    ```

1. De volgende klassen toevoegen. Deze klassen definiëren het verwachte schema voor de hoofdtekst van binnenkomende berichten.

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

1. Vervang de hoofdtekst van de **uitvoeren** methode met de volgende code. Deze filtert berichten op basis van de temperatuur-waarde in de hoofdtekst van het bericht en de drempelwaarde temperatuur.

    ```csharp
    const int temperatureThreshold = 25;

    byte[] messageBytes = messageReceived.GetBytes();
    var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

    if (!string.IsNullOrEmpty(messageString))
    {
        // Get the body of the message and deserialize it
        var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);
        
        if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
        {
            // We will send the message to the output as the temperature value is greater than the threashold
            var filteredMessage = new Message(messageBytes);
            // We need to copy the properties of the original message into the new Message object
            foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
            {
                filteredMessage.Properties.Add(prop.Key, prop.Value);
            }
            // We are adding a new property to the message to indicate it is an alert
            filteredMessage.Properties.Add("MessageType", "Alert");
            // Send the message        
            await output.AddAsync(filteredMessage);
            log.Info("Received and transfered a message with temperature above the threshold");
        }
    }
    ```

11. Sla het bestand op.

## <a name="publish-a-docker-image"></a>Een installatiekopie van een Docker publiceren

1. De Docker-installatiekopie bouwen.
    1. Klik in Verkenner VS-Code op de **Docker** map om dit te openen. Selecteer de map voor uw platform container ofwel **linux x64** of **windows nano**. 
    2. Met de rechtermuisknop op de **Dockerfile** -bestand en klik op **bouwen IoT rand module Docker installatiekopie**. 
    3. In de **map selecteren** vak, gaat u naar de **linux-Docker-x64** map en klik op **map selecteren als EXE_DIR**. 
    4. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld: `<docker registry address>/filterfunction:latest`, waarbij *docker register adres* uw Docker-ID als u met behulp van Docker-Hub of is vergelijkbaar met `<your registry name>.azurecr.io`, als u van Azure Container register gebruikmaakt.
 
4. Aanmelden bij Docker. Voer de volgende opdracht in de geïntegreerde terminal: 

    - Docker-Hub (Voer uw referenties als u wordt gevraagd):
        
        ```csh/sh
        docker login
        ```

    - Azure-Container register:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Als u de gebruikersnaam, wachtwoord en meld u aan server moet worden gebruikt in deze opdracht zoekt, gaat u naar de [Azure portal] (https://portal.azure.com). Van **alle resources**, klikt u op de tegel voor uw Azure-container register open de eigenschappen en klik vervolgens op **toegangssleutels**. Kopieer de waarden in de **gebruikersnaam**, **wachtwoord**, en **Login-server** velden. De aanmelding server sould worden van het formulier: `<your registry name>.azurecr.io`.

3. De installatiekopie naar uw opslagplaats Docker forceren. Gebruik de **weergave | Opdracht palet... | Rand: Push IoT rand module Docker installatiekopie** menu opdracht in en voer de naam van de installatiekopie in het pop-tekst boven aan het venster VS-Code. Gebruik dezelfde naam van de installatiekopie u in stap gebruikt 1.c.

## <a name="add-registry-credentials-to-your-edge-device"></a>Voeg register referenties toe aan het apparaat van rand
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

1. In de **Azure-portal**, gaat u naar uw IoT-hub.
2. Ga naar **IoT rand (preview)** en selecteer het apparaat IoT rand.
1. Selecteer **Modules ingesteld**. 
2. Voeg de **tempSensor** module. Deze stap is alleen vereist als u nog niet eerder hebt geïmplementeerd de **tempSensor** module die u wilt uw IoT-randapparaat.
    1. Selecteer **IoT rand Module toevoegen**.
    2. In de **naam** veld `tempSensor`.
    3. In de **installatiekopie URI** veld `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Laat de overige instellingen ongewijzigd en klik op **opslaan**.
1. Voeg de **filterfunction** module.
    1. Selecteer **IoT rand Module toevoegen** opnieuw.
    2. In de **naam** veld `filterfunction`.
    3. In de **installatiekopie** en voer het adres klopt; bijvoorbeeld `<docker registry address>/filterfunction:latest`.
    74. Klik op **Opslaan**.
2. Klik op **Volgende**.
3. In de **Routes opgeven** stap, kopieert u de JSON onder in het tekstvak. Alle berichten publiceren modules naar de Edge-runtime. Declaratieve regels in de runtime definiëren waar deze berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor naar de filtermodule via het eindpunt 'input1', dat het eindpunt dat u hebt geconfigureerd met de **FilterMessages** handler. De tweede route transporten berichten van de module expressiefilter IoT-hub. In deze route `upstream` is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klik op **Volgende**.
5. In de **Template bekijken** stap, klikt u op **indienen**. 
6. Ga terug naar de detailpagina voor rand van de IoT-apparaat en klikt u op **vernieuwen**. U ziet nu de nieuwe **filterfunction** module uitgevoerd samen met de **tempSensor** module en de **IoT rand runtime**. 

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

Voor het bewaken van het apparaat verzonden van uw IoT-randapparaat naar uw IoT-hub voor cloud-berichten:
1. De Azure IoT Toolkit-uitbreiding configureren met de verbindingsreeks voor uw IoT-hub: 
    1. Gebruik de **weergave | Explorer** menuopdracht om de Code van de VS explorer te openen. 
    3. Klik in de Verkenner op **IOT HUB-apparaten** en klik vervolgens op **...** . Klik op **verbindingsreeks van de IoT-Hub ingesteld** en geef de verbindingsreeks voor de IoT-hub die uw IoT-Edge-apparaat met in het pop-upvenster verbindt. 

        Als u wilt zoeken in de verbindingsreeks, klik op de tegel voor uw IoT-hub in de Azure portal en klik vervolgens op **gedeeld toegangsbeleid**. In **gedeeld toegangsbeleid**, klikt u op de **iothubowner** beleid en kopieert u de verbinding met IoT Hub tekenreeks in de **iothubowner** venster.   

1. Voor het bewaken van gegevens die binnenkomen in de IoT-hub, gebruiken de **weergave | Opdracht palet... | IoT: Starten van de controle van D2C bericht** menuopdracht. 
2. Als u wilt stoppen met het bewaken van gegevens, gebruikt u de **weergave | Opdracht palet... | IoT: Stopt met bewaken D2C bericht** menuopdracht. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Azure-functie met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT-randapparaat gemaakt. Informatie over het gebruik van een IoT-randapparaat als een gateway om te blijven verkennen van Azure IoT rand. 

> [!div class="nextstepaction"]
> [Een IoT-Edge gateway-apparaat maken](how-to-create-gateway-device.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
