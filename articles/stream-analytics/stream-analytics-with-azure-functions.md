---
title: Azure Functions uitvoeren met Azure Stream Analytics-taken | Microsoft Docs
description: Informatie over het configureren van Azure-functie als uitvoerlocatie naar de stroom analytische taken.
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
ms.openlocfilehash: adc147fc9f47e78cc0a2fcaf53f064bcfa5eee2c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="run-azure-functions-with-azure-stream-analytics-jobs"></a>Azure Functions uitvoeren met Azure Stream Analytics-taken 
 
> [!IMPORTANT]
> Deze functionaliteit is in preview.

U kunt Azure Functions uitvoeren met Azure Stream Analytics met Azure Functions configureren als een van de PUT uitvoer naar de Stream Analytics-taak. Azure-functie is een gebeurtenissen, compute-on-demand-ervaring waarmee u de code die wordt geactiveerd door gebeurtenissen in Azure of services van derden implementeren. Deze mogelijkheid van Azure-functie om te reageren op triggers maakt het een natuurlijke uitvoer naar Azure Stream Analytics-taak.

Azure Stream Analytics roept Azure-functie met HTTP-triggers. De uitvoeradapter van de Azure-functie kan gebruikers verbinding maken met de Azure Functions Stream Analytics zodanig dat de gebeurtenissen kunnen worden geactiveerd op basis van de Stream Analytics query's. 

Deze zelfstudie laat zien hoe u verbinding maken met Azure Stream Analytics voor [Azure Redis-Cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) met [Azure Functions](../azure-functions/functions-overview.md). 

## <a name="configure-stream-analytics-job-to-run-an-azure-function"></a>Stream Analytics-taak voor uitvoering van een Azure-functie configureren 

Deze sectie wordt gedemonstreerd hoe u een Stream Analytics-taak voor uitvoering van een Azure-functie die u gegevens naar Azure Redis-Cache schrijft configureren. De Stream Analytics-taak gebeurtenissen van de Event Hub leest, voert een query die de Azure-functie activeert. Deze functie Azure gegevens van de Stream Analytics-taak leest en schrijft deze naar de Azure Redis-Cache.

![Afbeelding ter illustratie van de zelfstudie](./media/stream-analytics-with-azure-functions/image1.png)

De volgende stappen zijn vereist voor deze taak te bereiken:
* [Stream Analytics-taak maken met Event Hub als invoer.](#create-stream-analytics-job-with-event-hub-as-input)  
* [Maak een Azure Redis-Cache.](#create-an-azure-redis-cache)  
* [Maak een Azure-functie die gegevens naar de Redis-Cache kan schrijven.](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Werk de stroom analytische taak met Azure-functie als uitvoer.](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Controleer de Redis-Cache voor de resultaten.](#check-redis-cache-for-results)  

## <a name="create-stream-analytics-job-with-event-hub-as-input"></a>Stream Analytics-taak maken met Event Hub als invoer

Ga als volgt de [realtime-fraudedetectie](stream-analytics-real-time-fraud-detection.md) zelfstudie voor het maken van een event hub, de toepassing van de generator gebeurtenis wordt gestart en maken van een Azure Stream Analytics (overslaan bij de stappen voor het maken van de query en de uitvoer, in plaats daarvan stelt een Azure Functions uitvoer in de volgende sectie.)

## <a name="create-an-azure-redis-cache"></a>Maak een Azure Redis Cache

1. Maken van een Azure Redis-Cache met behulp van de stappen in [een cache maken](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) sectie van het artikel Redis-Cache.  

2. Nadat de Redis-Cache is gemaakt, gaat u naar de gemaakte cache > **toegangssleutels** > en noteer de **primaire verbindingsreeks**.

   ![Redis-Cache-verbindingsreeks](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-an-azure-function-that-can-write-data-to-the-redis-cache"></a>Een Azure-functie die u gegevens naar de Redis-Cache schrijven kunt maken

1. Gebruik de [maken van een functie-app](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) gedeelte van de Azure Functions-documentatie voor het maken van een Azure-functie-App en een [Azure-functie voor HTTP-geactiveerde](../azure-functions/functions-create-first-azure-function.md#create-function) (aka Webhook) met behulp van **CSharp** taal.  

2. Navigeer naar de **run.csx** functioneren en bijwerken met de volgende code (Zorg ervoor dat u de '\<de redis-cache-verbindingsreeks hier\>' tekst met primaire verbindingsreeks voor de Redis-Cache die u hebt opgehaald in de vorige sectie):  

   ```c#
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

   Wanneer Azure Stream Analytics 413 (HTTP-aanvragen entiteit te groot) uitzondering van de functie Azure ontvangt, vermindert het de grootte van de batches naar Azure Functions verzendt. Gebruik de volgende code om te controleren dat de Azure Stream Analytics te grote batches niet verzenden in uw Azure-functie. Zorg ervoor dat de maximale batch-aantal en grootte waarden zijn gebruikt in de functie consistent met de waarden die zijn opgegeven in de Stream Analytics-portal zijn.

   ```c#
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. Maak een JSON-bestand met de naam in een teksteditor van uw keuze, **project.json** met de volgende code en sla deze op uw lokale computer. Dit bestand bevat de NuGet-pakketafhankelijkheden vereist door de c#-functie.  
   
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
 
4. Ga terug naar de Azure portal > navigeren naar uw Azure-functie > van de **platformfuncties** tabblad > Klik op **App Service-Editor** die zich bevindt onder **ontwikkelingsprogramma's**. 
 
   ![Scherm App service-editor](./media/stream-analytics-with-azure-functions/image3.png)

5. In de App Service-Editor, klik met de rechtermuisknop op uw hoofdmap en upload de **project.json** bestand. Nadat het uploaden is voltooid, wordt de pagina vernieuwt, wordt u ziet nu een automatisch gegenereerde-bestand met de naam **project.lock.json**.  Het bestand automatisch gegenereerde bevat verwijzingen naar het dll-bestanden die zijn opgegeven in het bestand Project.json.  

   ![Project.json-bestand uploaden](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytic-job-with-azure-function-as-output"></a>Update de stroom analytische taak met de Azure-functie als uitvoer

1. Open uw Azure Stream Analytics-taak op de Azure-portal.  

2. Navigeer naar uw Azure-functie > **overzicht** > **uitvoer** > **toevoegen** een nieuwe uitvoer > Selecteer **Azure-functie** voor de optie Sink. De uitvoeradapter van de nieuwe Azure-functie is beschikbaar met de volgende eigenschappen:  

   |**De naam van eigenschap**|**Beschrijving**|
   |---|---|
   |Uitvoeraliassen| Een gebruiksvriendelijke naam die u in query om te verwijzen naar de uitvoer van de taak gebruikt. |
   |Optie importeren| U kunt azure functie uit het huidige abonnement gebruiken of de instellingen handmatig opgeven als de functie in een ander abonnement bevindt zich. |
   |Functie-app| Naam van uw Azure-functie-App. |
   |Functie| De naam van de functie in uw App functie (naam van de functie run.csx).|
   |Maximale batchgrootte|Deze eigenschap wordt gebruikt voor de maximale grootte voor elke batch uitvoer, die is verzonden naar uw Azure-functie. Deze waarde is standaard ingesteld op 256 KB.|
   |Maximale Batch-aantal|Deze eigenschap kunt u het maximum aantal gebeurtenissen in elke batch die wordt verzonden naar de Azure-functie. De standaardwaarde voor de maximale batch-aantal is 100. Deze eigenschap is optioneel.|
   |Sleutel|Deze eigenschap kunt u een Azure-functie van een ander abonnement gebruiken. Geef de waarde van de sleutel voor toegang tot uw functie. Deze eigenschap is optioneel.|

3. Geef een naam voor de uitvoeralias. In deze zelfstudie we deze naam **saop1** (u kunt een andere naam van uw keuze) en andere details invullen.  

4. Open uw Stream Analytics-taak en bijwerken van de query voor de volgende (Zorg ervoor dat de tekst "saop1" wordt vervangen als u hebt de naam van de uitvoerlocatie anders):  

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

5. De toepassing telcodatagen.exe starten met de volgende opdracht op de opdrachtregel (de opdracht wordt de indeling - `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`)  
   
   **telcodatagen.exe 1000.2 2**
    
6.  Start de Stream Analytics-taak.

## <a name="check-redis-cache-for-results"></a>Redis-Cache voor de resultaten controleren

1. Ga naar de Azure-portal en zoek uw Redis-Cache > Selecteer **Console**.  

2. Gebruik [Redis-cache opdrachten](https://redis.io/commands) om te controleren of uw gegevens in Redis-cache. (De opdracht wordt de indeling-Get {sleutel}). Bijvoorbeeld:

   **Ophalen van ' 12/19/2017 21:32:24-123414732 '**

   Met deze opdracht moet de waarde voor de opgegeven sleutel afdrukken:

   ![Redis-Cache-uitvoer](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Bekende problemen

* In de Azure portal, wanneer u probeert te herstellen van de grootte van de maximale Batch / maximale Batch-aantal van waarde naar empty(default), verandert deze terug naar de eerder ingevoerde waarde bij het opslaan. Opzettelijk de standaardwaarden invoeren voor deze velden in dit geval.

## <a name="next-steps"></a>Volgende stappen
