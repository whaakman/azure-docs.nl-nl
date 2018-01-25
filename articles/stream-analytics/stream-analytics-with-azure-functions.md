---
title: Azure Functions uitvoeren met Azure Stream Analytics-taken | Microsoft Docs
description: Informatie over het configureren van Azure Functions als uitvoerlocatie aan Stream Analytics-taken.
keywords: gegevens uitvoert, streamen van gegevens van Azure-functie
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/19/2017
ms.author: sngun
ms.openlocfilehash: f2f4a8d8cda752dc6ed197b8402119f7cbcaf58f
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="run-azure-functions-with-azure-stream-analytics-jobs"></a>Azure Functions uitvoeren met Azure Stream Analytics-taken 

U kunt Azure Functions uitvoeren met Azure Stream Analytics door functies te configureren als een van de PUT uitvoer naar de Stream Analytics-taak. Functies is een gebeurtenisafhankelijke, compute-on-demand-ervaring waarmee u de code die wordt geactiveerd door gebeurtenissen in Azure of services van derden implementeren. Deze mogelijkheid van functies om te reageren op triggers wordt het een natuurlijke uitvoer voor Stream Analytics-taken.

Stream Analytics roept functies via HTTP-triggers. De uitvoeradapter van de functies kan gebruikers verbinding maken functies met Stream Analytics, zodat de gebeurtenissen kunnen worden geactiveerd op basis van de Stream Analytics query's. 

Deze zelfstudie laat zien hoe u verbinding maken met Stream Analytics voor [Azure Redis-Cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md), met behulp van [Azure Functions](../azure-functions/functions-overview.md). 

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>Configureren van een Stream Analytics-taak voor het uitvoeren van een functie 

Deze sectie wordt gedemonstreerd hoe u een Stream Analytics-taak voor uitvoering van een functie die u gegevens naar Azure Redis-Cache schrijft configureren. De Stream Analytics-taak gebeurtenissen worden gelezen uit Azure Event Hubs en voert een query die de functie activeert. Deze functie gegevens van de Stream Analytics-taak leest en schrijft deze naar Azure Redis-Cache.

![Diagram van de relaties tussen de Azure-services](./media/stream-analytics-with-azure-functions/image1.png)

De volgende stappen zijn vereist voor deze taak te bereiken:
* [Maken van een Stream Analytics-taak met Event Hubs als invoer](#create-stream-analytics-job-with-event-hub-as-input)  
* [Azure Redis-Cache-exemplaar maken](#create-an-azure-redis-cache)  
* [Maak een functie in Azure-functies die gegevens naar de Azure Redis-Cache kan schrijven](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Bijwerken van de Stream Analytics-taak met de functie als uitvoer](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Azure Redis-Cache voor de resultaten controleren](#check-redis-cache-for-results)  

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Maken van een Stream Analytics-taak met Event Hubs als invoer

Ga als volgt de [realtime-fraudedetectie](stream-analytics-real-time-fraud-detection.md) zelfstudie voor het maken van een event hub, start de gebeurtenis generator-toepassing en een Stream Analytics-taak maken. (De stappen voor het maken van de query en de uitvoer overslaan. In plaats daarvan, Zie de volgende secties voor het instellen van de uitvoer van de functies.)

## <a name="create-an-azure-redis-cache-instance"></a>Azure Redis-Cache-exemplaar maken

1. Maken van een cache in Azure Redis-Cache met behulp van de stappen in [een cache maken](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).  

2. Nadat u de cache onder gemaakt **instellingen**, selecteer **toegangstoetsen**. Noteer de **primaire verbindingsreeks**.

   ![Schermopname van Azure Redis-Cache-verbindingsreeks](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-redis-cache"></a>Maak een functie in Azure-functies die gegevens naar Azure Redis-Cache kan schrijven

1. Zie de [maken van een functie-app](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) gedeelte van de documentatie van de functies. Hiermee wordt u begeleid bij het maken van een functie-app en een [HTTP-geactiveerde functie in Azure Functions](../azure-functions/functions-create-first-azure-function.md#create-function), met behulp van de CSharp-taal.  

2. Blader naar de **run.csx** functie. Bijwerken met de volgende code. (Zorg ervoor dat u '\<de redis-cache-verbindingsreeks hier\>"met de verbindingsreeks die u hebt opgehaald in de vorige sectie primaire van Azure Redis-Cache.)  

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

   Wanneer de uitzondering 'HTTP-aanvragen entiteit te groot' Stream Analytics van de functie ontvangt, vermindert het de grootte van de batches naar functies verzendt. In de functie, de volgende code gebruiken om te controleren dat Stream Analytics te grote batches niet verzenden. Zorg ervoor dat de maximale batch-aantal en grootte waarden zijn gebruikt in de functie consistent met de waarden die zijn opgegeven in de Stream Analytics-portal zijn.

   ```csharp
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. Maak een JSON-bestand met de naam in een teksteditor van uw keuze, **project.json**. Gebruik de volgende code en sla deze op uw lokale computer. Dit bestand bevat de NuGet-pakketafhankelijkheden vereist door de C#-functie.  
   
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
 
4. Ga terug naar de Azure-portal. Van de **platformfuncties** tabblad, blader naar de functie. Onder **ontwikkelingsprogramma's**, selecteer **App Service-Editor**. 
 
   ![Schermopname van App Service-Editor](./media/stream-analytics-with-azure-functions/image3.png)

5. In de App Service-Editor met de rechtermuisknop op uw hoofdmap en upload de **project.json** bestand. Nadat het uploaden voltooid is, wordt de pagina vernieuwen. U ziet nu een automatisch gegenereerde-bestand met de naam **project.lock.json**. Het bestand automatisch gegenereerde bevat verwijzingen naar het dll-bestanden die zijn opgegeven in het bestand project.json.  

   ![Schermopname van App Service-Editor](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>Bijwerken van de Stream Analytics-taak met de functie als uitvoer

1. Open uw Stream Analytics-taak op de Azure-portal.  

2. Blader naar de functie en selecteer **overzicht** > **uitvoer** > **toevoegen**. Selecteer om een nieuwe uitvoer toe **Azure-functie** voor de optie sink. De nieuwe functies uitvoeradapter is beschikbaar met de volgende eigenschappen:  

   |**De naam van eigenschap**|**Beschrijving**|
   |---|---|
   |Uitvoeralias| Een gebruiksvriendelijke naam die u in query om te verwijzen naar de uitvoer van de taak gebruikt. |
   |Optie importeren| U kunt de functie van het huidige abonnement gebruiken of de instellingen handmatig opgeven als de functie in een ander abonnement bevindt zich. |
   |Functie-app| Naam van uw app functies. |
   |Functie| De naam van de functie in uw app functies (naam van de functie run.csx).|
   |Maximale batchgrootte|Hiermee stelt u de maximale grootte voor elke batch uitvoer, die is verzonden naar de functie. Deze waarde is standaard ingesteld op 256 KB.|
   |Maximale Batch-aantal|Hiermee geeft u het maximum aantal gebeurtenissen in elke batch die wordt verzonden naar de functie. De standaardwaarde is 100. Deze eigenschap is optioneel.|
   |Sleutel|Hiermee kunt u een functie van een ander abonnement gebruiken. Geef de waarde van de sleutel voor toegang tot uw functie. Deze eigenschap is optioneel.|

3. Geef een naam voor de uitvoeralias. In deze zelfstudie we deze naam **saop1** (u kunt elke willekeurige naam van uw keuze). Vul in andere details.  

4. Open uw Stream Analytics-taak en bijwerken van de query met de volgende. (Zorg ervoor dat de tekst "saop1" wordt vervangen als u hebt de naam van de uitvoerlocatie anders.)  

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

5. De toepassing telcodatagen.exe starten met de volgende opdracht op de opdrachtregel (Gebruik de notatie `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`):  
   
   **telcodatagen.exe 1000 .2 2**
    
6.  De Stream Analytics-taak starten.

## <a name="check-azure-redis-cache-for-results"></a>Azure Redis-Cache voor de resultaten controleren

1. Blader naar de Azure-portal en zoeken van uw Azure Redis-Cache. Selecteer **Console**.  

2. Gebruik [Redis-cache opdrachten](https://redis.io/commands) om te controleren of uw gegevens in Redis-cache. (De opdracht heeft de notatie {sleutel} Get.) Bijvoorbeeld:

   **Ophalen van ' 12/19/2017 21:32:24-123414732 '**

   Met deze opdracht moet de waarde voor de opgegeven sleutel afdrukken:

   ![Schermopname van Azure Redis-Cache-uitvoer](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Bekende problemen

In de Azure portal, wanneer u probeert te herstellen van de grootte van de maximale Batch / maximale Batch-aantal value leegmaken (standaard), de waarde gewijzigd terug naar de eerder ingevoerde waarde bij het opslaan. Typ in dit geval de standaardwaarden voor deze velden.

