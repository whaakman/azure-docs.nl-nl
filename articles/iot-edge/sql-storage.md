---
title: Azure IoT rand SQL-module | Microsoft Docs
description: Gegevens aan de rand met Microsoft SQL-modules met Azure Functions opmaken van de gegevens worden opgeslagen.
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban, ebertrams
ms.date: 02/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 792e754b84f1dc03a32780ed94d274c833be68f5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="store-data-at-the-edge-with-sql-server-databases"></a>Opslaan van gegevens aan de rand met SQL Server-databases

Gebruik Azure IoT-randapparaten voor het opslaan van de gegevens die worden gegenereerd aan de rand. Apparaten met onregelmatige internet-verbindingen kunnen onderhouden hun eigen databases en het rapport wijzigingen terug naar de cloud alleen wanneer verbonden. Apparaten die geprogrammeerd verzenden alleen kritieke gegevens naar de cloud de rest van de gegevens voor reguliere bulksgewijs kunt opslaan uploadt. Wanneer in de cloud, de gestructureerde gegevens kunnen worden gedeeld met andere Azure-services voor instantie voor het bouwen van een machine learning-model. 

In dit artikel bevat instructies voor het implementeren van een SQL Server-database op een Edge van de IoT-apparaat. Azure Functions, uitgevoerd op het apparaat IoT rand structuren van de inkomende gegevens en verzendt het naar de database. De stappen in dit artikel kunnen ook worden toegepast op andere databases die in containers, zoals MySQL of PostgreSQL werken. 

## <a name="prerequisites"></a>Vereisten 

Voordat u de instructies in dit artikel, moet u de volgende zelfstudies uitvoeren:
* Azure IoT rand implementeren op een gesimuleerd apparaat in [Windows](tutorial-simulate-device-windows.md) of [Linux](tutorial-simulate-device-linux.md)
* [Azure-functie implementeren als een Edge van de IoT-module](tutorial-deploy-function.md)

De volgende artikelen voor het voltooien van deze zelfstudie zijn niet vereist, maar kunnen handig zijn context bieden:
* [Uitvoeren van de installatiekopie van het SQL Server 2017-container met Docker](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker)
* [Visual Studio Code te ontwikkelen en implementeren van Azure Functions voor Azure IoT Edge gebruiken](how-to-vscode-develop-azure-function.md)

Nadat u de vereiste zelfstudies hebt voltooid, hebt u nu alle vereiste onderdelen op uw computer: 
* Een actief Azure-IoT-hub.
* Een IoT randapparaat met ten minste 2 GB RAM-geheugen en een schijfstation 2 GB.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [C# voor Visual Studio Code (via OmniSharp)-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
* [Python 2.7](https://www.python.org/downloads/)
* [IoT-rand besturingselement script](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
* Sjabloon AzureIoTEdgeFunction (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
* Een actieve iothub met ten minste een Edge van de IoT-apparaat.

Windows- en Linux-containers op x64 processorarchitecturen voor deze zelfstudie werken. SQL Server biedt geen ondersteuning voor ARM-processors.

## <a name="deploy-a-sql-server-container"></a>Een SQL Server-container implementeren

In deze sectie kunt u een MS SQL-database toevoegen aan uw gesimuleerde rand van de IoT-apparaat. De SQL Server 2017 docker-container afbeelding gebruiken, beschikbaar als een [Windows](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) container en als een [Linux](https://hub.docker.com/r/microsoft/mssql-server-linux/) container. 

### <a name="deploy-sql-server-2017"></a>Deploy SQL Server 2017

De code in deze sectie maakt standaard een container met de gratis Developer-editie van SQL Server 2017. Als u wilt een productie-versies in plaats daarvan uitvoeren, Zie [uitvoeren productie container installatiekopieën](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#production) voor gedetailleerde informatie. 

In stap 3, kunt u het maken van opties voor de SQL Server-container die belangrijk zijn voor het tot stand brengen van omgevingsvariabelen en persistant opslag toevoegen. De geconfigureerde [omgevingsvariabelen](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-environment-variables) accepteer de gebruiksrechtovereenkomst en definiëren van een wachtwoord. De [persistant opslag](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#persist) is geconfigureerd met [koppelt](https://docs.docker.com/storage/volumes/). Koppelingen maken van de SQL Server 2017-container met een *sqlvolume* volumecontainer is gekoppeld, zodat uw gegevens zich blijft voordoen zelfs als de container wordt verwijderd. 

1. Open de `deployment.json` bestand in Visual Studio Code. 
2. Vervang de **modules** gedeelte van het bestand met de volgende code: 

   ```json
   "modules": {
          "filterFunction": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "<docker registry address>/filterfunction:latest",
              "createOptions": "{}"
            }
          },
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
              "createOptions": "{}"
            }
          },
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
        }
   ```

3. Vervang de `<docker registry address>` met het adres op de voltooide zelfstudie ingevuld [Azure-functie implementeren als een module van de rand van de IoT - voorbeeld](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-deploy-function)

   >[!NOTE]
   >De Register-adres van de container is hetzelfde als de aanmeldingsserver die u hebt gekopieerd uit het register. Deze moet de vorm van `<your container registry name>.azurecr.io`

4. Afhankelijk van het besturingssysteem die u gebruikt, moet u de instellingen voor de SQL-module bijwerken met de volgende code: 

   * Windows:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}"
      ```

   * Linux:

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

5. Sla het bestand op. 
6. Selecteer in het palet tegenover Code opdracht **rand: implementatie voor randapparaat maken**. 
7. Selecteer uw IoT-Edge-apparaat-ID.
8. Selecteer de `deployment.json` bestand dat u hebt bijgewerkt. In het venster output ziet u bijbehorende uitvoer voor uw implementatie. 
9. Voor het starten van de runtime van de rand, selecteer **rand: Start rand** in het palet opdracht.

>[!TIP]
>Elke keer dat u een SQL Server-container in een productieomgeving maken, moet u [wijzigen wachtwoord van de systeembeheerder standaard](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

## <a name="create-the-sql-database"></a>De SQL-database maken

Deze sectie helpt u bij het instellen van de SQL-database voor het opslaan van de temperatuur gegevens ontvangen van de sensoren die is verbonden met de IoT-randapparaat. Als u een gesimuleerd apparaat gebruikt, deze gegevens zijn afkomstig van de *tempSensor* container. 

Verbinding maken met uw database in een opdrachtregelprogramma: 

* Windows-container
   ```cmd
   docker exec -it sql cmd
   ```

* Linux-container
   ```bash
   docker exec -it sql bash
   ```

Open het hulpprogramma SQL-opdracht: 

* Windows-container
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Linux-container
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Uw database maken: 

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

Definieer de tabel: 

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

U kunt uw SQL Server docker-bestand naar uw SQL-Server automatisch ingesteld om te worden geïmplementeerd op meerdere rand van de IoT-apparaten. Zie voor meer informatie de [Microsoft SQL Server-container demo project](https://github.com/twright-msft/mssql-node-docker-demo-app).

## <a name="understand-the-sql-connection"></a>Inzicht in de SQL-verbinding

In andere zelfstudies gebruiken we routes om toe te staan van containers om te communiceren en tegelijk van elkaar geïsoleerd. Wanneer u met een SQL Server-database werkt, is een dichter relatie echter nodig zijn. 

Rand IoT maakt automatisch een brug (Linux) of een NAT (Windows) netwerk wanneer deze wordt gestart. Het netwerk wordt aangeroepen **azure-iot-edge**. Als u ooit moet fouten opsporen in deze verbinding, kunt u de eigenschappen ervan op de opdrachtregel opzoeken:

* Windows

   ```cmd
   docker network inspect azure-iot-edge
   ```

* Linux

   ```bash
   sudo docker network inspect azure-iot-edge
   ```

IoT-rand kunt ook oplossen door de DNS-server van een containernaam via docker, dus u hoeft niet te verwijzen naar de SQL Server-database door het IP-adres. 

Een voorbeeld: dit is de verbindingsreeks die we in de volgende sectie gebruiken: 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

U kunt zien dat de verbindingsreeks verwijst naar voor de container met de naam **sql**. Als u de naam van de module iets anders worden gewijzigd, bijwerken en deze verbindingsreeks. Anders gaat u verder met de volgende sectie. 

## <a name="update-your-azure-function"></a>Bijwerken van uw Azure-functie
Werk de om gegevens te verzenden naar de database, de FilterFunction Azure-functie die u hebt aangebracht in de vorige zelfstudie. Dit bestand wijzigen zodat de gegevens die zijn ontvangen door de sensoren structuren vervolgens in een SQL-tabel opgeslagen. 

1. Open de map FilterFunction in Visual Studio Code. 
2. Het bestand run.csx vervangen door de volgende code met de SQL-verbindingsreeks uit het vorige gedeelte: 

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
           const string str = "Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;";
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

## <a name="update-your-container-image"></a>Bijwerken van uw installatiekopie container

Om de wijzigingen die u hebt aangebracht, bijwerken van uw installatiekopie container, publiceren en opnieuw opstarten IoT rand.

1. Vouw in de Visual Studio Code, de **Docker** map. 
2. Op basis van het platform dat u gebruikt, vouw ofwel de **windows nano** of **linux x64** map. 
3. Met de rechtermuisknop op de **Dockerfile** bestand en selecteer **bouwen IoT rand module Docker installatiekopie**.
4. Navigeer naar de **FilterFunction** projectmap en klik op **Selecteer map als EXE_DIR**.
5. Typ in het pop-tekst boven aan het venster tegenover Code, naam van de installatiekopie. Bijvoorbeeld `<your container registry address>/filterfunction:latest`. Als u met een lokale register implementeert, de naam moet `<localhost:5000/filterfunction:latest>`.
6. Selecteer in het palet van de opdracht tegenover Code **rand: Push IoT rand module Docker installatiekopie**. 
7. Voer de naam van de dezelfde installatiekopie in het pop-tekstvak. 
8. Selecteer in het palet van de opdracht tegenover Code **rand: opnieuw opstarten rand**.

## <a name="view-the-local-data"></a>De lokale gegevens weergeven

Zodra de containers opnieuw hebt gestart, worden de gegevens ontvangen van de temperatuursensors wordt opgeslagen in een lokale 2017 van SQL Server-database op het apparaat aan uw IoT-rand. 

Verbinding maken met uw database in een opdrachtregelprogramma: 

* Windows-container
   ```cmd
   docker exec -it sql cmd
   ```

* Linux-container
   ```bash
   docker exec -it sql bash
   ```

Open het hulpprogramma SQL-opdracht: 

* Windows-container
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Linux-container
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Uw gegevens bekijken: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [configureren van SQL Server 2017 container installatiekopieën op Docker](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker).

* Ga naar de [mssql docker GitHub-opslagplaats](https://github.com/Microsoft/mssql-docker) voor resources, feedback en bekende problemen.
