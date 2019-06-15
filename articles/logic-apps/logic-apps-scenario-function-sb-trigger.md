---
title: Logische apps met Azure Functions en Azure Service Bus-trigger of aanroepen
description: Maken van Azure-functies die worden aangeroepen of een logische apps activeren met behulp van Azure Service Bus
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 06/04/2019
ms.openlocfilehash: 3d4f642ae25a179ea2c3241240996da774cd8c23
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66494986"
---
# <a name="call-or-trigger-logic-apps-by-using-azure-functions-and-azure-service-bus"></a>Worden aangeroepen of een logische apps activeren met behulp van Azure Functions en Azure Service Bus

U kunt [Azure Functions](../azure-functions/functions-overview.md) voor het activeren van een logische app wanneer u nodig hebt om een langlopende listener of taak te implementeren. Bijvoorbeeld, kunt u een Azure-functie die luistert in een [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) in de wachtrij en een logische app als een push-trigger onmiddellijk gestart.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een Azure Service Bus-naamruimte. Als u een naamruimte, geen [maakt u eerst uw naamruimte](../service-bus-messaging/service-bus-create-namespace-portal.md).

* Een Azure-functie-app, dat een container voor Azure functions is. Als u een functie-app geen [maakt u eerst uw functie-app](../azure-functions/functions-create-first-azure-function.md), en zorg ervoor dat u .NET als de runtimestack selecteert.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-logic-app"></a>Logische app maken

Voor dit scenario hebt u een functie die wordt uitgevoerd elke logische app die u wilt activeren. Maak eerst een logische app die met een HTTP-aanvraag als trigger begint. De functie-aanroepen dat eindpunt wanneer er een wachtrijbericht is ontvangen.  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en maken van lege, logische app.

   Als u geen ervaring met logische apps, raadpleegt u [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Voer in het zoekvak, "http-aanvraag. Selecteer deze trigger uit de lijst met triggers: **Wanneer een HTTP-aanvraag wordt ontvangen**

   ![Trigger selecteren](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

   U kunt eventueel een JSON-schema moet worden gebruikt met het wachtrijbericht met de aanvraagtrigger invoeren. JSON-schema's helpen Logic App Designer inzicht in de structuur van de ingevoerde gegevens en de uitvoer eenvoudiger te gebruiken in uw werkstroom.

1. Geef een schema op het schema in te voeren de **hoofdtekst van de JSON-Schema aanvragen** vak, bijvoorbeeld:

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

1. Eventuele andere vereiste acties uit voeren te na de ontvangst van bericht uit de wachtrij toevoegen.

   U kunt bijvoorbeeld een e-mailbericht met de Office 365 Outlook-connector verzenden.

1. Sla uw logische app, die de callback-URL voor de trigger in deze logische app wordt gegenereerd. Later, gebruikt u deze URL voor terugbellen voor in de code voor de trigger voor Azure Service Bus-wachtrij.

   De callback URL wordt weergegeven in de **URL voor HTTP POST** eigenschap.

   ![Gegenereerde URL voor terugbellen voor trigger](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure-functie maken

Maak vervolgens de functie die fungeert als de trigger en luistert naar de wachtrij.

1. In de Azure-portal openen en vouw uw functie-app, als dat niet is geopend. 

1. Vouw onder de naam van uw functie-app **functies**. Op de **functies** deelvenster Kies **nieuwe functie**.

   ![Vouw 'Functies' en 'Nieuwe functie' kiezen](./media/logic-apps-scenario-function-sb-trigger/create-new-function.png)

1. Selecteer deze sjabloon op basis van of u een nieuwe functie-app waarbij u .NET geselecteerd als de runtimestack of u een bestaande functie-app hebt gemaakt.

   * Voor de nieuwe functie-apps, selecteert u deze sjabloon: **Trigger voor Service Bus-wachtrij**

     ![Selecteer een sjabloon voor de nieuwe functie-app](./media/logic-apps-scenario-function-sb-trigger/current-add-queue-trigger-template.png)

   * Voor een bestaande functie-app, selecteert u deze sjabloon: **Service Bus Queue trigger - C#**

     ![Selecteer een sjabloon voor bestaande functie-app](./media/logic-apps-scenario-function-sb-trigger/legacy-add-queue-trigger-template.png)

1. Op de **Azure Service Bus-wachtrij trigger** deelvenster Geef een naam op voor de trigger en stel de **Service Bus-verbinding** voor de wachtrij waarin de SDK van Azure Service Bus `OnMessageReceive()` listener, en kies **Maken**.

1. Schrijven van een basisfunctie die voor het aanroepen van het eindpunt van de eerder gemaakte logische app met behulp van het wachtrijbericht als een trigger. In dit voorbeeld wordt de `application/json` inhoudstype bericht, maar u kunt dit type zo nodig wijzigen. Gebruik indien mogelijk opnieuw het exemplaar van de HTTP-clients. Zie voor meer informatie, [verbindingen beheren in Azure Functions](../azure-functions/manage-connections.md).

   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   // Callback URL for previously created Request trigger
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/workflows/<remaining-callback-URL>";

   // Reuse the instance of HTTP clients if possible
   private static HttpClient httpClient = new HttpClient();

   public static void Run(string myQueueItem, ILogger log)
   {
       log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       var response = httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
   }
   ```

1. De functie te testen, kunt u een wachtrijbericht toevoegen met behulp van een hulpprogramma zoals de [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).

   De triggers voor logische app onmiddellijk nadat de functie het bericht ontvangt.

## <a name="next-steps"></a>Volgende stappen

[Aanroepen, triggeren of nesten van werkstromen met behulp van HTTP-eindpunten](../logic-apps/logic-apps-http-endpoint.md)