---
title: Scenario - Trigger logische apps met Azure Functions en Azure Service Bus | Microsoft Docs
description: Maken van een functie in om een logische app activeren met behulp van Azure Functions en Azure Service Bus
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 088f10bc32dd492f82f0a10a7e5829e76f588758
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>Scenario: Een logische app met Azure Functions en Azure Service Bus activeren

U kunt Azure Functions gebruiken voor het maken van een trigger voor een logische app wanneer u moet een langlopende listener of taak implementeren. U kunt bijvoorbeeld een functie die een wachtrij luistert maken en een logische app als een push-signaal wordt direct gestart.

## <a name="build-the-logic-app"></a>De logische app bouwen
In dit voorbeeld hebt u een functie die wordt uitgevoerd voor elke logische app die moet worden geactiveerd. Maak eerst een logische app, die een HTTP-verzoek-trigger heeft. De functie aanroept dat eindpunt wanneer er een wachtrijbericht is ontvangen.  

1. Een logische app maken.
2. Selecteer de **handmatig - wanneer er een HTTP-aanvraag wordt ontvangen** trigger.
   Desgewenst kunt u een JSON-schema te gebruiken met het bericht uit de wachtrij met behulp van een hulpprogramma zoals [jsonschema.net](http://jsonschema.net). Het schema in de trigger te plakken. Schema's kunnen de ontwerpfunctie voor inzicht in de vorm van de gegevens en stroom eigenschappen gemakkelijker via de werkstroom.
2. Eventuele extra stappen die u laten uitvoeren wilt na de ontvangst van een wachtrijbericht toevoegen. Bijvoorbeeld, sturen een e-mail via Office 365.  
3. Sla de logische app voor het genereren van de callback-URL voor de trigger aan deze logische app. De URL wordt weergegeven op de trigger-kaart.

![De callback URL wordt weergegeven op de trigger-kaart][1]

## <a name="build-the-function"></a>De functie bouwen
Vervolgens moet u een functie die fungeert als de trigger en luistert naar de wachtrij maken.

1. In de [Azure Functions-portal](https://functions.azure.com/signin), selecteer **nieuwe functie**, en selecteer vervolgens de **ServiceBusQueueTrigger - C#** sjabloon.
   
    ![Azure Functions-portal][2]
2. Configureer de verbinding met de Service Bus-wachtrij die gebruikmaakt van de SDK van Azure Service Bus `OnMessageReceive()` listener.
3. Een eenvoudige functie voor het aanroepen van de logische app-eindpunt (eerder hebt gemaakt) met behulp van bericht in de wachtrij als een trigger schrijven. Hier volgt een voorbeeld van een volledige van een functie. In het voorbeeld wordt een `application/json` inhoud berichttype, maar u kunt dit type zo nodig wijzigen.
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Als u wilt testen, Voeg een wachtrijbericht via een hulpprogramma zoals [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). Zie de logische app gestart onmiddellijk nadat de functie heeft het bericht ontvangen.

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
