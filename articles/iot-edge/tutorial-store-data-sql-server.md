---
title: Gegevens opslaan met Azure IoT Edge SQL-module | Microsoft Docs
description: Leer hoe u lokaal gegevens kunt opslaan op uw IoT-Edge-apparaat met een SQL Server-module
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/22/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 7e02caf9706a5127d3729256fcc238f467eb2991
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143497"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Zelfstudie: gegevens opslaan aan de rand met SQL Server-databases

Gebruik Azure IoT Edge en SQL Server voor het opslaan en opvragen van gegevens aan de rand. Azure IoT Edge bevat ingebouwde basisopslagfuncties die berichten cachen als een apparaat offline gaat en deze doorsturen wanneer de verbinding weer tot stand wordt gebracht. Mogelijk wilt u echter meer geavanceerde opslagmogelijkheden, zoals het lokaal kunnen opvragen van gegevens. Door lokale databases op te nemen, kunnen uw IoT Edge-apparaten complexere computing uitvoeren zonder een verbinding met IoT Hub in stand te hoeven houden. Een buitendiensttechnicus kan bijvoorbeeld de laatste dagen aan sensorgegevens op een machine lokaal weergeven, ook al worden die gegevens slechts één keer per maand geüpload naar de cloud om een machine learning-model te helpen verbeteren.

Dit artikel bevat instructies voor het implementeren van een SQL Server-database op een IoT Edge-apparaat. Azure Functions, dat op het IoT Edge-apparaat wordt uitgevoerd, structureert de binnenkomende gegevens en verzendt deze naar de database. De stappen in dit artikel kunnen ook worden toegepast op andere databases die in containers werken, zoals MySQL of PostgreSQL.

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een Azure-functie te maken
> * Een SQL-database op uw IoT Edge-apparaat implementeren
> * Gebruik Visual Studio Code om modules te bouwen en deze op uw IoT Edge-apparaat te implementeren
> * Gegenereerde gegevens weergeven

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).

Cloudresources:

* Een standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C#-extensie voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) voor Visual Studio Code. 
* [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)-extensie voor Visual Studio Code. 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Container Registry**.

    ![containerregister maken](./media/tutorial-deploy-function/create-container-registry.png)

2. Geef uw register een naam en kies een abonnement.
3. Voor de resourcegroep is het raadzaam de naam te gebruiken van dezelfde resourcegroep die uw IoT Hub bevat. Door alle resources bij elkaar te houden in dezelfde groep, kunt u ze samen beheren. Als u bijvoorbeeld de resourcegroep verwijdert die u hebt gebruikt om te testen, worden alle testresources verwijderd die deze groep bevat. 
4. Stel de SKU in op **Basic**en stel **Gebruiker met beheerdersrechten** in op **Inschakelen**. 
5. Klik op **Create**.
6. Zodra het containerregister is gemaakt, gaat u er naartoe en selecteert u **Toegangssleutels**. 
7. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie. 

## <a name="create-a-function-project"></a>Een functieproject maken

Om gegevens naar een database te verzenden hebt u een module nodig die de gegevens correct kan structureren en vervolgens in een tabel opslaat. 

De volgende stappen laten zien hoe u een IoT-Edge-functie maakt met behulp van Visual Studio Code en de Azure IoT Edge-extensie.

1. Open Visual Studio Code.
2. Open de met VS Code geïntegreerde terminal door **View** > **Terminal** te selecteren.
3. Selecteer **View** > **Command Palette** en open het VS Code-opdrachtpalet.
4. Typ in het opdrachtpalet de opdracht **Azure: Sign in** en voer deze uit. Volg vervolgens de instructies om u aan te melden bij uw Azure-account. Als u zich al hebt aangemeld, kunt u deze stap overslaan.
3. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** en voer deze uit. Geef in het opdrachtpalet de volgende informatie op om de oplossing te maken: 
   1. Selecteer de map waarin u de oplossing wilt maken. 
   2. Geef een naam op voor de oplossing of houd de standaardnaam **EdgeSolution** aan.
   3. Kies **Azure Functions - C#** als de module-sjabloon. 
   4. Noem uw module **sqlFunction**. 
   5. Geef het Azure Container Registry dat u in de vorige sectie hebt gemaakt, op als de opslagplaats voor installatiekopieën voor de eerste module. Vervang **localhost:5000** door de gekopieerde waarde voor de aanmeldingsserver. De uiteindelijke tekenreeks ziet er ongeveer als volgt uit: **\<registernaam\>.azurecr.io/sqlFunction**.

4. In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. Er is een map **modules**, een map **.vscode**, en een sjabloonbestand voor het distributiemanifest. Open **modules** > **sqlFunction** > **EdgeHubTrigger-Csharp** > **run.csx**.

5. Vervang de inhoud van het bestand door de volgende code:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"
   #r "System.Data.SqlClient"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;
   using System.Threading.Tasks;

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

           //Store the data in SQL db
           const string str = "<sql connection string>";
           using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
           {
           conn.Open();
           var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
           var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
               using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
               {
               //Execute the command and log the # rows affected.
               var rows = await cmd.ExecuteNonQueryAsync();
               log.Info($"{rows} rows were updated");
               }
           }

           if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
           {
               // Send the message to the output as the temperature value is greater than the threshold
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

6. Vervang in regel 24 de tekenreeks **\<sql-verbindingsreeks\>** door de volgende tekenreeks. De eigenschap **Gegevensbron** verwijst naar de naam van de SQL Server-container, die u in de volgende sectie met de naam **SQL** maakt. 

   ```C#
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Sla het bestand **run.csx** op. 

## <a name="add-a-sql-server-container"></a>Een SQL Server-container toevoegen

Een [distributiemanifest](module-composition.md) declareert welke modules de IoT Edge-runtime op uw IoT Edge-apparaat zal installeren. U hebt de code voor het maken van een ​​aangepaste functiemodule in de vorige sectie toegevoegd, maar de SQL Server-module is al gebouwd. U hoeft alleen aan de IoT Edge-runtime op te geven dat het deze module moet opnemen, en die vervolgens op uw apparaat te configureren. 

1. Open in de Visual Studio Code Explorer het bestand **deployment.template.json**. 
2. Zoek de sectie **moduleContent. $edgeAgent.properties.desired.modules** op. Er zouden twee modules moeten worden vermeld: **tempSensor**, die gesimuleerde gegevens genereert, en uw **sqlFunction**-module.
3. Voeg de volgende code toe om een ​​derde module te declareren:

   ```json
   "sql": {
       "version": "1.0",
       "type": "docker",
       "status": "running",
       "restartPolicy": "always",
       "settings": {
           "image": "",
           "createOptions": ""
       }
   }
   ```

4. Afhankelijk van het besturingssysteem van uw IoT Edge-apparaat, werkt u de parameter in **sql.settings** bij met de volgende code:

   * Windows:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Linux:

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   >[!Tip]
   >Telkens wanneer u een SQL Server-container in een productieomgeving maakt, moet u [het standaardwachtwoord van de systeembeheerder](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password)wijzigen.

5. Sla het bestand **deployment.template.json** op. 

## <a name="build-your-iot-edge-solution"></a>Uw IoT Edge-oplossing bouwen

In de voorgaande secties hebt u een oplossing met één module gemaakt en er vervolgens nog een toegevoegd aan de distributiemanifestsjabloon. Nu moet u de oplossing bouwen, containerinstallatiekopieën voor de modules maken en de installatiekopieën naar uw containerregister pushen. 

1. Geef in het bestand deployment.template.json de IoT Edge runtime uw registerreferenties, zodat deze toegang heeft tot uw module-installatiekopieën. Zoek de sectie **moduleContent.$edgeAgent.properties.desired.runtime.settings** op. 
2. Voeg de volgende JSON-code toe na de **loggingOptions**:

   ```JSON
   "registryCredentials": {
       "myRegistry": {
           "username": "",
           "password": "",
           "address": ""
       }
   }
   ```

3. Voeg uw registratiegegevens in de velden **gebruikersnaam**, **wachtwoord**en **adres** in. Gebruik de waarden die u hebt gekopieerd toen u aan het begin van de zelfstudie uw Azure Container Registry hebt gemaakt.
4. Sla het bestand **deployment.template.json** op.
5. Meld uw containerregister aan in Visual Studio Code, zodat u uw installatiekopieën naar uw register kunt pushen. Gebruik dezelfde referenties die u zojuist aan het distributiemanifest hebt toegevoegd. Voer de volgende opdracht in de geïntegreerde terminal in: 

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    U wordt om het wachtwoord gevraagd. Plak het wachtwoord achter de prompt en druk op **Enter**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

6. Klik in VS Code Explorer met de rechtermuisknop op het bestand **deployment.template.json** en selecteer **IoT Edge-oplossing bouwen**. 

## <a name="deploy-the-solution-to-a-device"></a>De oplossing implementeren op een apparaat

U kunt modules op een apparaat instellen via de IoT Hub, maar u hebt ook toegang tot uw IoT Hub en apparaten via Visual Studio Code. In dit gedeelte stelt u de toegang tot uw IoT Hub in en gebruikt u VS Code om uw oplossing op uw IoT Edge-apparaat te implementeren. 

1. Selecteer in het opdrachtenpalet van VS Code **Azure IoT Hub: IoT Hub selecteren**.
2. Volg de aanwijzingen om u aan te melden bij uw Azure-account. 
3. Selecteer in het opdrachtenpalet uw Azure-abonnement en selecteer vervolgens uw IoT Hub. 
4. Vouw in VS Code Explorer de sectie **Azure IoT Hub Devices** uit. 
5. Klik met de rechtermuisknop op het doelapparaat van uw implementatie en selecteer **Implementatie maken voor één apparaat**. 
6. Ga in de bestandsverkenner naar de map **config** in uw oplossing en kies **deployment.json**. Klik op **Edge-distributiemanifest selecteren**. 

Als de implementatie is geslaagd, wordt er een bevestigingsbericht afgedrukt in de VS Code-uitvoer. U kunt ook controleren of alle modules op uw apparaat actief zijn. 

Voer op uw IoT Edge-apparaat de volgende opdracht uit om de status van de modules te bekijken. Dit kan enkele minuten duren.

   ```PowerShell
   iotedge list
   ```

## <a name="create-the-sql-database"></a>De SQL-database maken

Wanneer u het distributiemanifest op uw apparaat toepast, krijgt u drie modules die worden uitgevoerd. De module tempSensor genereert gesimuleerde omgevingsgegevens. De module sqlFunction neemt de gegevens en deelt deze in voor een database. 

Deze sectie leidt u door het instellen van de SQL-database voor het opslaan van de temperatuurgegevens. 

1. Gebruik een opdrachtregelprogramma om verbinding te maken met uw database. 
   * Windows-container:
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Linux-container: 

      ```bash
      docker exec -it sql bash
      ```

2. Open het SQL-opdrachthulpprogramma.
   * Windows-container:

      ```cmd
      sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

   * Linux-container: 

      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Uw database maken: 

   * Windows-container
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = 'C:\mssql\measurementsdb.mdf')
      GO
      ```

   * Linux-container
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
      GO
      ```

4. Definieer uw tabel.

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

U kunt het dockerbestand van uw SQL Server aanpassen om uw SQL Server automatisch in te stellen om te worden geïmplementeerd op meerdere IoT Edge-apparaten. Zie het [Microsoft SQL Server-containerdemo-project](https://github.com/twright-msft/mssql-node-docker-demo-app)voor meer informatie. 

## <a name="view-the-local-data"></a>De lokale gegevens bekijken

Nadat uw tabel is gemaakt, begint de module sqlFunction gegevens op te slaan in een lokale SQL Server 2017-database op uw IoT Edge-apparaat. 

Voer vanuit het SQL-opdrachthulpprogramma de volgende opdracht uit om uw opgemaakte tabelgegevens te bekijken: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![Lokale gegevens bekijken](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure Functions-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u klaar bent om uw eigen modules te bouwen, kunt u meer informatie krijgen over het [ontwikkelen van Azure Functions met Azure IoT Edge voor Visual Studio Code](how-to-develop-csharp-function.md). 

Ga verder met de volgende zelfstudies om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Gemiddelden zoeken met een zwevend venster in Azure Stream Analytics](tutorial-deploy-stream-analytics.md)
