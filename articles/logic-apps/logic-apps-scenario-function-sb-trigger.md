---
title: Scenario - Trigger logische apps met Azure Functions en Azure Service Bus | Microsoft Docs
description: Functies maken die logische apps activeren met behulp van Azure Functions en Azure Service Bus
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 08/25/2018
ms.openlocfilehash: 8d6f2ecaec7bf7ae7e4415ccd60fc6ec3ff487f3
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126169"
---
# <a name="scenario-trigger-logic-apps-with-azure-functions-and-azure-service-bus"></a>Scenario: Trigger logische apps met Azure Functions en Azure Service Bus

Azure Functions kunt u een trigger voor een logische app maken wanneer u nodig hebt om een langlopende listener of taak te implementeren. U kunt bijvoorbeeld een functie maakt die een wachtrij luistert en een logische app als een push-trigger wordt onmiddellijk gestart.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Voordat u een Azure-functie kunt maken [maken van een functie-app](../azure-functions/functions-create-function-app-portal.md).

## <a name="create-logic-app"></a>Logische app maken

In dit voorbeeld hebt u een functie die wordt uitgevoerd voor elke logische app die moet worden geactiveerd. Maak eerst een logische app met een HTTP-aanvraag als trigger. De functie-aanroepen dat eindpunt wanneer er een wachtrijbericht is ontvangen.  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en maken van lege, logische app. 

   Als u geen ervaring met logische apps, raadpleegt u [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Voer in het zoekvak, "http-aanvraag. Selecteer deze trigger onder de lijst met triggers: **wanneer een HTTP-aanvraag wordt ontvangen**

   ![Trigger selecteren](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

1. Voor de **aanvragen** trigger, kunt u eventueel een JSON-schema voor gebruik met het wachtrijbericht invoeren. JSON-schema's helpen Logic App Designer inzicht in de structuur van de ingevoerde gegevens en maakt uitvoer eenvoudiger voor u om te selecteren in de hele werkstroom. 

   Geef een schema op het schema in te voeren de **hoofdtekst van de JSON-Schema aanvragen** vak, bijvoorbeeld: 

   ![Geef de JSON-schema](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   Als u een schema hebt, maar u een voorbeeld-nettolading in JSON-indeling hebt, kunt u een schema genereren uit deze nettolading.

   1. Kies in de aanvraagtrigger **voorbeeldnettolading gebruiken voor het genereren van schema**.

   1. Onder **typt of plakt u een voorbeeld-JSON-nettolading**, voer de voorbeeldnettolading en kies vervolgens **gedaan**.
      
      ![Voorbeeld van payload invoeren](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   In dit voorbeeld van payload genereert dit schema dat wordt weergegeven in de trigger:

   ```json
   {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "number": {
                  "type": "integer"
               },
               "street": {
                  "type": "string"
               },
               "city": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "integer"
               },
               "country": {
                  "type": "string"
               }
            }
         }
      }
   }
   ```

1. Eventuele andere vereiste acties die moeten worden uitgevoerd na de ontvangst van bericht uit de wachtrij toevoegen. 

   U kunt bijvoorbeeld een e-mailbericht met de Office 365 Outlook-connector verzenden.

1. Sla uw logische app, die de callback-URL voor de trigger in deze logische app wordt gegenereerd. Deze URL wordt weergegeven in de **URL voor HTTP POST** eigenschap.

   ![Gegenereerde URL voor terugbellen voor trigger](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure-functie maken

Maak vervolgens de functie die fungeert als de trigger en luistert naar de wachtrij. 

1. In de Azure-portal openen en vouw uw functie-app, als dat niet is geopend. 

1. Vouw onder de naam van uw functie-app **functies**. Op de **functies** deelvenster Kies **nieuwe functie**. Selecteer deze sjabloon: **Service Bus-wachtrij-trigger - C#**
   
   ![Selecteer Azure Functions-portal](./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png)

1. Geef een naam op voor de trigger en configureer vervolgens de verbinding met de Service Bus-wachtrij, die gebruikmaakt van de SDK van Azure Service Bus `OnMessageReceive()` listener.

1. Schrijf een basisfunctie die voor het aanroepen van het eindpunt van de eerder gemaakte logische app met behulp van het wachtrijbericht als een trigger, bijvoorbeeld: 
   
   ```CSharp
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

   In dit voorbeeld wordt de `application/json` inhoudstype bericht, maar u kunt dit type zo nodig wijzigen.

1. De functie te testen, kunt u een wachtrijbericht toevoegen met behulp van een hulpprogramma zoals de [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). 

   De triggers voor logische app onmiddellijk nadat de functie het bericht ontvangt.

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

