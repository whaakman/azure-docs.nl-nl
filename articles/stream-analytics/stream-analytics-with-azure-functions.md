---
title: 'Zelfstudie: Azure Functions uitvoeren met Azure Stream Analytics-taken | Microsoft Docs'
description: In deze zelfstudie leert u hoe u Azure Functions configureert als een uitvoerlocatie voor Stream Analytics-taken.
services: stream-analytics
author: jasonwhowell
manager: kfile
ms.service: stream-analytics
ms.topic: tutorial
ms.custom: mvc
ms.workload: data-services
ms.date: 04/09/2018
ms.author: jasonh
ms.reviewer: jasonh
ms.openlocfilehash: 50ea4dafe7edfdeb851ad6d9cc42a7bca262e970
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985808"
---
# <a name="run-azure-functions-from-azure-stream-analytics-jobs"></a>Azure Functions uitvoeren vanuit Azure Stream Analytics-taken 

U kunt Azure Functions uitvoeren vanuit Azure Stream Analytics door Azure Functions te configureren als een van de uitvoerlocaties naar de Stream Analytics-taak. Azure Functions is een gebeurtenisafhankelijke, compute-on-demand-ervaring waarmee u code kunt implementeren die wordt geactiveerd door gebeurtenissen in Azure of services van derden. Door deze mogelijkheid van Azure Functions om op triggers te reageren, is het een natuurlijke uitvoer naar Stream Analytics-taken.

Stream Analytics activeert Azure Functions via HTTP-triggers. De uitvoeradapter van Azure Functions biedt gebruikers de mogelijkheid om Azure Functions te verbinden met Stream Analytics. Gebeurtenissen kunnen dan worden geactiveerd op basis van Stream Analytics-query's. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Stream Analytics-taak maken
> * Een Azure-functie maken
> * Azure-functie configureren als uitvoer naar uw taak

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>Een Stream Analytics-taak configureren om een functie uit te voeren 

In deze sectie ziet u hoe u een Stream Analytics-taak configureert om een functie uit te voeren die gegevens naar Azure Redis Cache schrijft. De Stream Analytics-taak leest gebeurtenissen uit Azure Event Hubs en voert een query uit die de functie activeert. Deze functie leest gegevens uit de Stream Analytics-taak en schrijft deze naar Azure Redis Cache.

![Diagram van de relaties tussen de Azure-services](./media/stream-analytics-with-azure-functions/image1.png)

Voor deze taak moeten de volgende stappen worden gezet:
* [Een Stream Analytics-taak maken met Event Hubs als invoer](#create-a-stream-analytics-job-with-event-hubs-as-input)  
* [Een Azure Redis Cache-exemplaar maken](#create-an-azure-redis-cache-instance)  
* [Een functie in Azure Functions maken die gegevens naar de Azure Redis Cache schrijft](#create-a-function-in-azure-functions-that-can-write-data-to-azure-redis-cache)    
* [De Stream Analytics-taak bijwerken met de functie als uitvoer](#update-the-stream-analytics-job-with-the-function-as-output)  
* [Azure Redis Cache controleren op resultaten](#check-azure-redis-cache-for-results)  

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Een Stream Analytics-taak maken met Event Hubs als invoer

Volg de zelfstudie [Realtime fraudedetectie](stream-analytics-real-time-fraud-detection.md) om een Event Hub te maken, de gebeurtenisgenerator te starten en een Stream Analytics-taak te maken. (Sla de stappen voor het maken van de query en de uitvoer over. Raadpleeg in plaats daarvan de volgende secties voor het instellen van Functions-uitvoer.)

## <a name="create-an-azure-redis-cache-instance"></a>Een Azure Redis Cache-exemplaar maken

1. Een cache in Azure Redis Cache maken met behulp van de stappen die zijn beschreven in [Een cache maken](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).  

2. Nadat u de cache hebt gemaakt, selecteert u onder **Instellingen** de optie **Toegangssleutels**. Noteer de **Primaire sleutel van de verbindingsreeks**.

   ![Schermopname van verbindingsreeks van Azure Redis Cache](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-redis-cache"></a>Een functie in Azure Functions maken die gegevens naar Azure Redis Cache schrijft

1. Zie de sectie [Een functie-app maken](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) van de Azure Functions-documentatie. Hiermee wordt u begeleid bij het maken van een functie-app en een [HTTP-geactiveerde functie in Azure Functions](../azure-functions/functions-create-first-azure-function.md#create-function). Hiervoor wordt de taal C# gebruikt.  

2. Blader naar de functie **run.csx**. Werk deze bij met de volgende code. (Vervang '\<your redis cache connection string goes here\>' door de verbindingsreeks van Azure Redis Cache die u in de vorige sectie hebt opgehaald.)  

   ```csharp
   using System;
   using System.Net;
   using System.Threading.Tasks;
   using StackExchange.Redis;
   using Newtonsoft.Json;
   using System.Configuration;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
      // Get the request body
      dynamic dataArray = await req.Content.ReadAsAsync<object>();

      // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

      if (dataArray.ToString().Length > 262144)
      {        
         return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
      var connection = ConnectionMultiplexer.Connect("<your redis cache connection string goes here>");
      log.Info($"Connection string.. {connection}");
    
      // Connection refers to a property that returns a ConnectionMultiplexer
      IDatabase db = connection.GetDatabase();
      log.Info($"Created database {db}");
    
      log.Info($"Message Count {dataArray.Count}");

      // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
      for (var i = 0; i < dataArray.Count; i++)
      {
        string time = dataArray[i].time;
        string callingnum1 = dataArray[i].callingnum1;
        string key = time + " - " + callingnum1;
        db.StringSet(key, dataArray[i].ToString());
        log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
      // Simple get of data types from the cache
      string value = db.StringGet(key);
      log.Info($"Database got: {value}");
      }

      return req.CreateResponse(HttpStatusCode.OK, "Got");
    }    

   ```

   Wanneer Stream Analytics de uitzondering 'HTTP Request Entity Too Large' (HTTP-aanvraagentiteit te groot) van de functie ontvangt, worden de batches die naar Azure Functions worden gestuurd, verkleind. Gebruik de volgende code in uw functie om te controleren of Stream Analytics geen te grote batches verzendt. Zorg dat het maximum aantal batches en de grootten die in de functie worden gebruikt, consistent zijn met de waarden die zijn ingevoerd in de Stream Analytics-portal.

   ```csharp
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. Maak in een teksteditor een JSON-bestand met de naam **project.json**. Gebruik de volgende code en sla het bestand op uw lokale computer op. Dit bestand bevat de NuGet-pakketafhankelijkheden die de C#-functie nodig heeft.  
   
   ```json
       {
         "frameworks": {
             "net46": {
                 "dependencies": {
                     "StackExchange.Redis":"1.1.603",
                     "Newtonsoft.Json": "9.0.1"
                 }
             }
         }
     }

   ```
 
4. Ga terug naar Azure Portal. Blader op het tabblad **Platformfuncties** naar uw functie. Selecteer onder **Ontwikkelingsprogramma's** de optie **App Service-editor**. 
 
   ![Schermopname van App Service-editor](./media/stream-analytics-with-azure-functions/image3.png)

5. Klik in de App Service-editor met de rechtermuisknop op de hoofdmap en upload het bestand **project.json**. Vernieuw de pagina wanneer de upload is voltooid. Nu wordt een automatisch gegenereerd bestand met de naam **project.lock.json** weergegeven. Het automatisch gegenereerde bestand bevat verwijzingen naar de DLL-bestanden die in het bestand project.json zijn opgegeven.  

   ![Schermopname van App Service-editor](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>De Stream Analytics-taak bijwerken met de functie als uitvoer

1. Open uw Stream Analytics-taak in Azure Portal.  

2. Blader naar uw functie en selecteer **Overzicht** > **Uitvoer** > **Toevoegen**. Selecteer **Azure-functie** als sinkoptie als u een nieuwe uitvoer wilt toevoegen. De nieuwe Azure Functions-uitvoeradapter is beschikbaar en heeft de volgende eigenschappen:  

   |**Eigenschapsnaam**|**Beschrijving**|
   |---|---|
   |Uitvoeralias| Een gebruiksvriendelijke naam die u in query van de taak gebruikt om naar de uitvoer te verwijzen. |
   |Importoptie| U kunt de functie van het huidige abonnement gebruiken of de instellingen handmatig opgeven als de functie zich in een ander abonnement bevindt. |
   |Functie-app| Naam van uw Azure Functions-app. |
   |Functie| Naam van de functie in uw Azure Functions-app (naam van de functie run.csx).|
   |Maximale batchgrootte|Hiermee stelt u de maximale grootte in voor elke uitvoerbatch die naar uw functie wordt verzonden. Deze waarde is standaard ingesteld op 256 kB.|
   |Maximum aantal batches|Hiermee geeft u het maximum aantal gebeurtenissen in elke batch op die naar de functie worden verzonden. De standaardwaarde is 100. Deze eigenschap is optioneel.|
   |Sleutel|Hiermee kunt u een functie uit een ander abonnement gebruiken. Geef de waarde op van de sleutel die toegang geeft tot uw functie. Deze eigenschap is optioneel.|

3. Geef een naam op voor de uitvoeralias. In deze zelfstudie hebben we deze **saop1** genoemd. U kunt elke willekeurige andere naam kiezen. Vul de andere details in.  

4. Open uw Stream Analytics-taak en werk de query als volgt bij. (Verander de tekst 'saop1' als u uw uitvoerlocatie een andere naam hebt gegeven.)  

   ```sql
    SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Start de toepassing telcodatagen.exe door de volgende opdracht uit te voeren op de opdrachtregel (gebruik de notatie `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`):  
   
   **telcodatagen.exe 1000 .2 2**
    
6.  Start de Stream Analytics-taak.

## <a name="check-azure-redis-cache-for-results"></a>Azure Redis Cache controleren op resultaten

1. Blader naar Azure Portal en zoek uw Azure Redis Cache. Selecteer **Console**.  

2. Gebruik [Redis Cache-opdrachten](https://redis.io/commands) om te controleren of uw gegevens zich in Redis Cache bevinden. (De opdracht heeft de notatie Get {sleutel}.) Bijvoorbeeld:

   **Get "12/19/2017 21:32:24 - 123414732"**

   Met deze opdracht wordt de waarde voor de opgegeven sleutel afgedrukt:

   ![Schermopname van uitvoer van Azure Redis Cache](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Bekende problemen

Wanneer u in Azure Portal de waarde van Maximale batchgrootte/Maximum aantal batches probeert leeg te maken (standaard), verandert de waarde weer in de eerder ingevoerde waarde wanneer u de gegevens opslaat. Voer in dat geval de standaardwaarden voor deze velden handmatig in.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u een resourcegroep niet meer nodig hebt, verwijdert u de resourcegroep, de streamingtaak en alle gerelateerde resources. Door de taak te verwijderen, voorkomt u dat de streaming-eenheden die door de taak worden verbruikt, in rekening worden gebracht. Als u denkt dat u de taak in de toekomst nog gaat gebruiken, kunt u deze stoppen en later opnieuw starten wanneer dat nodig is. Als u deze taak niet meer gaat gebruiken, verwijdert u alle resources die in deze snelstart zijn gemaakt. Daarvoor voert u de volgende stappen uit:

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt.  
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een eenvoudige Stream Analytics-taak gemaakt die een Azure-functie uitvoert. Ga verder met de volgende zelfstudie voor meer informatie over Stream Analytics-taken:

> [!div class="nextstepaction"]
> [Door gebruiker gedefinieerde JavaScript-functies uitvoeren in Stream Analytics-taken](stream-analytics-javascript-user-defined-functions.md)