---
title: Voorbeelden van de integratie van Azure Service Bus met Event Grid | Microsoft Docs
description: In dit artikel vindt u voorbeelden van de integratie van Service Bus-berichten met Event Grid.
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: spelluru
ms.openlocfilehash: b29798bb87b7c5c677e7d80e552e45e8d1290541
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65754693"
---
# <a name="respond-to-azure-service-bus-events-received-via-azure-event-grid-by-using-azure-functions-and-azure-logic-apps"></a>Reageren op Azure Service Bus-gebeurtenissen ontvangen via de Azure Event Grid met behulp van Azure Functions en Azure Logic Apps
In deze zelfstudie leert u hoe u om te reageren op Azure Service Bus-gebeurtenissen die worden ontvangen via de Azure Event Grid met behulp van Azure Functions en Azure Logic Apps. U voert de volgende stappen uit:
 
- Maak een test Azure-functie voor foutopsporing en het weergeven van de eerste stroom gebeurtenissen uit Event Grid.
- Maak een Azure-functie waarmee u Service Bus-berichten kunt ontvangen en verwerken op basis van Event Grid-gebeurtenissen.
- Een logische app maken om te reageren op gebeurtenissen van Event Grid

Nadat u de Service Bus, Event Grid, Azure Functions en Logic Apps-artefacten gemaakt, kunt u de volgende acties uitvoeren: 

1. Berichten verzenden naar een Service Bus-onderwerp. 
2. Controleer of de abonnementen op het onderwerp die berichten ontvangen
3. Controleer of de functie of de logische app die van de gebeurtenis geabonneerd de gebeurtenis is ontvangen. 

## <a name="create-a-service-bus-namespace"></a>Een Service Bus-naamruimte maken
Volg de instructies in deze zelfstudie: [Snelstart: De Azure portal gebruiken voor het maken van een Service Bus-onderwerp en -abonnementen naar het onderwerp](service-bus-quickstart-topics-subscriptions-portal.md) naar de volgende taken uitvoeren:

- Maak een **premium** Service Bus-naamruimte. 
- De verbindingsreeks ophalen. 
- Een Service Bus-onderwerp maken.
- Maak twee abonnementen naar het onderwerp. 

## <a name="prepare-a-sample-application-to-send-messages"></a>Voorbereiden van een voorbeeldtoepassing om berichten te verzenden
U kunt een willekeurige methode gebruiken om een bericht naar uw Service Bus-onderwerp te verzenden. De voorbeeldcode aan het einde van deze procedure wordt ervan uitgegaan dat u Visual Studio 2017.

1. Kloon de [GitHub azure-service-bus repository](https://github.com/Azure/azure-service-bus/) (opslagplaats van GitHub-azure-service-bus).
2. Ga in Visual Studio naar de map *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration* en open het bestand *SBEventGridIntegration.sln*.
3. Ga naar het project **MessageSender** en selecteer **Program.cs**.
4. Vul de naam van uw Service Bus-onderwerp en de verbindingsreeks die u hebt verkregen in de vorige stap:

    ```CSharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```
5. Ontwikkel en voer het programma voor het verzenden van testberichten naar het Service Bus-onderwerp. 

## <a name="set-up-a-test-function-on-azure"></a>Instellen van een testfunctie op Azure 
Voordat u het volledige scenario doorlopen, stelt u ten minste een kleine testfunctie, kunt u fouten opsporen en observeert welke gebeurtenissen stromen. Volg de instructies in de [uw eerste functie maken in Azure portal](../azure-functions/functions-create-first-azure-function.md) artikel voor de volgende taken uitvoeren: 

1. Een functie-app maken.
2. Een door HTTP geactiveerde functie maken. 

Voer de volgende stappen uit: 


# <a name="azure-functions-v2tabv2"></a>[Azure Functions V2](#tab/v2)

1. Vouw **functies** in de structuur weergeven en selecteer uw functie. Vervang de code voor de functie door de volgende code: 

    ```CSharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
        var content = req.Body;
        string jsonContent = await new StreamReader(content).ReadToEndAsync();
        log.LogInformation($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        headerValues = req.Headers.GetCommaSeparatedValues("Aeg-Event-Type");
    
        if (headerValues.Count() != 0)
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
                var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                var code = events[0].Data["validationCode"];
                log.LogInformation("Validation code: {code}");
                return (ActionResult) new OkObjectResult(new { validationResponse = code });
            }
        }
    
        return jsonContent == null
            ? new BadRequestObjectResult("Please pass a name on the query string or in the request body")
            : (ActionResult)new OkObjectResult($"Hello, {jsonContent}");
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }
    
    ```
2. Selecteer **Opslaan en uitvoeren**.

    ![Functie-app-uitvoer](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
3. Selecteer **functie-URL ophalen** en noteert u de URL. 

    ![Functie-URL ophalen](./media/service-bus-to-event-grid-integration-example/get-function-url.png)

# <a name="azure-functions-v1tabv1"></a>[Azure Functions V1](#tab/v1)

1. Configureren van de functie **V1** versie: 
    1. Selecteer uw functie-app in de structuurweergave wordt weergegeven en selecteer **functie app-instellingen**. 

        ![Instellingen voor functie-apps](). / media/service-bus-to-event-grid-integration-example/function-app-settings.png)
    2. Selecteer **~ 1** voor **runtimeversie**. 
2. Vouw **functies** in de structuur weergeven en selecteer uw functie. Vervang de code voor de functie door de volgende code: 

    ```CSharp
    #r "Newtonsoft.Json"
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
        // parse query parameter
        var content = req.Content;
    
        string jsonContent = await content.ReadAsStringAsync(); 
        log.Info($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
            var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                 var code = events[0].Data["validationCode"];
                 return req.CreateResponse(HttpStatusCode.OK,
                 new { validationResponse = code });
            }
        }
    
        return jsonContent == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }
    ```
4. Selecteer **Opslaan en uitvoeren**.

    ![Functie-app-uitvoer](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
4. Selecteer **functie-URL ophalen** en noteert u de URL. 

    ![Functie-URL ophalen](./media/service-bus-to-event-grid-integration-example/get-function-url.png)

---

## <a name="connect-the-function-and-namespace-via-event-grid"></a>Functie en naamruimte met elkaar verbinden via Event Grid
In deze sectie koppelt u de functie en de Service Bus-naamruimte met behulp van de Azure-portal. 

Volg deze stappen voor het maken van een Azure Event Grid-abonnement:

1. In de Azure-portal, gaat u naar uw naamruimte en selecteer vervolgens in het linkerdeelvenster **gebeurtenissen**. Het venster met de naamruimte wordt geopend, waarbij twee Event Grid-abonnementen in het rechterdeelvenster worden weergegeven. 
    
    ![Servicebus - pagina gebeurtenissen](./media/service-bus-to-event-grid-integration-example/service-bus-events-page.png)
2. Selecteer **+ gebeurtenisabonnement** op de werkbalk. 
3. Op de **gebeurtenisabonnement maken** pagina, de volgende stappen uit:
    1. Voer een **naam** voor het abonnement. 
    2. Selecteer **Webhook** voor **Eindpunttype**. 

        ![Servicebus - Event Grid-abonnement](./media/service-bus-to-event-grid-integration-example/event-grid-subscription-page.png)
    3. Gekozen **selecteert u een eindpunt**, plak de URL van de functie en selecteer vervolgens **Bevestig de selectie van**. 

        ![Functie - het-eindpunt selecteren](./media/service-bus-to-event-grid-integration-example/function-select-endpoint.png)
    4. Schakel over naar de **Filters** tabblad, voer de naam van de **eerste abonnement** naar de Service Bus-onderwerp u gemaakt eerdere, en selecteer vervolgens de **maken** knop. 

        ![Gebeurtenisfilter voor abonnement](./media/service-bus-to-event-grid-integration-example/event-subscription-filter.png)
4. Bevestig dat u ziet dat het gebeurtenisabonnement in de lijst.

    ![Gebeurtenisabonnement in de lijst](./media/service-bus-to-event-grid-integration-example/event-subscription-in-list.png)

## <a name="send-messages-to-the-service-bus-topic"></a>Berichten verzenden naar de Service Bus-onderwerp
1. Uitvoeren van de .NET C# toepassing waarmee berichten worden verzonden naar de Service Bus-onderwerp. 

    ![De uitvoer van de console-app](./media/service-bus-to-event-grid-integration-example/console-app-output.png)
1. Vouw op de pagina voor uw Azure-functie-app, **functies**, vouw uw **functie**, en selecteer **Monitor**. 

    ![Monitor-functie](./media/service-bus-to-event-grid-integration-example/function-monitor.png)

## <a name="receive-messages-by-using-azure-functions"></a>Berichten ontvangen met behulp van Azure Functions
In de vorige sectie hebt u een eenvoudig test- en foutopsporingsscenario gezien, en gecontroleerd of de gebeurtenissen stromen. 

In deze sectie leert u hoe u berichten ontvangt en verwerkt nadat u een gebeurtenis hebt ontvangen.

### <a name="publish-a-function-from-visual-studio"></a>Publiceren van een functie vanuit Visual Studio
1. In dezelfde Visual Studio-oplossing (**SBEventGridIntegration**) die u hebt geopend, selecteert u **ReceiveMessagesOnEvent.cs** in de **SBEventGridIntegration** project. 
2. Voer uw Service Bus-verbindingsreeks in de volgende code:

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```
3. Download de **publicatieprofiel** voor de functie:
    1. Selecteer uw functie-app. 
    2. Selecteer de **overzicht** tabblad als dit nog niet is geselecteerd. 
    3. Selecteer **publicatieprofiel ophalen** op de werkbalk. 

        ![Publicatieprofiel ophalen voor de functie](./media/service-bus-to-event-grid-integration-example/function-download-publish-profile.png)
    4. Sla het bestand naar de map van uw project. 
4. Klik vervolgens in Visual Studio met de rechtermuisknop op **SBEventGridIntegration** selecteer **Publish**. 
5. Selecteer *Start** op de **publiceren** pagina. 
6. Op de **kiezen een publicatiedoel** pagina, de volgende stappen uit, selecteer **-profiel importeren**. 

    ![Visual Studio - knop-profiel importeren](./media/service-bus-to-event-grid-integration-example/visual-studio-import-profile-button.png)
7. Selecteer de **publiceren profielbestand** u eerder hebt gedownload. 
8. Selecteer **publiceren** op de **publiceren** pagina. 

    ![Visual Studio - publiceren](./media/service-bus-to-event-grid-integration-example/select-publish.png)
9. Bevestig dat u de nieuwe Azure-functie **ReceiveMessagesOnEvent**. Vernieuw de pagina, indien nodig. 

    ![Controleer of de nieuwe functie is gemaakt](./media/service-bus-to-event-grid-integration-example/function-receive-messages.png)
10. De URL naar de nieuwe functie ophalen en noteer deze. 

### <a name="event-grid-subscription"></a>Event Grid-abonnement

1. Verwijder de bestaande Event Grid-abonnement:
    1. Op de **Service Bus Namespace** weergeeft, schakelt **gebeurtenissen** in het menu links. 
    2. Selecteer het bestaande gebeurtenisabonnement. 
    3. Op de **gebeurtenisabonnement** weergeeft, schakelt **verwijderen**.
2. Volg de instructies in de [verbinding maken met de functie en naamruimte via Event Grid](#connect-the-function-and-namespace-via-event-grid) om te maken van een Event Grid-abonnement met de nieuwe functie-URL.
3. Volg de instructies in de [berichten verzenden naar de Service Bus-onderwerp](#send-messages-to-the-service-bus-topic) sectie voor het verzenden van berichten naar het onderwerp en controleren van de functie. 

## <a name="receive-messages-by-using-logic-apps"></a>Berichten ontvangen met behulp van Logic Apps
Verbinding maken met een logische app met Azure Service Bus en Azure Event Grid door de volgende stappen:

1. Een logische app maken in Azure portal.
    1. Selecteer **+ een resource maken**, selecteer **integratie**, en selecteer vervolgens **logische App**. 
    2. Op de **logische App - maken** pagina een **naam** voor de logische app.
    3. Selecteer uw Azure-**abonnement**. 
    4. Selecteer **Use existing** voor de **resourcegroep**, en selecteer de resourcegroep die u hebt gebruikt voor andere bronnen (zoals Azure-functie, Service Bus-naamruimte) die u eerder hebt gemaakt. 
    5. Selecteer de **locatie** voor de logische app. 
    6. Selecteer **maken** te maken van de logische app. 
2. Op de **ontwerper van logische Apps** weergeeft, schakelt **lege logische App** onder **sjablonen**. 
3. Voer op de designer, de volgende stappen uit:
    1. Zoeken naar **Event Grid**. 
    2. Selecteer **wanneer een resourcegebeurtenis optreedt (preview) - Azure Event Grid**. 

        ![Ontwerper van logische Apps - Selecteer Event Grid-trigger](./media/service-bus-to-event-grid-integration-example/logic-apps-event-grid-trigger.png)
4. Selecteer **aanmelden**, Voer uw Azure-referenties in en selecteer **toegang toestaan**. 
5. Op de **wanneer een resourcegebeurtenis optreedt** pagina, de volgende stappen uit:
    1. Selecteer uw Azure-abonnement. 
    2. Voor **resourcetype**, selecteer **Microsoft.ServiceBus.Namespaces**. 
    3. Voor **resourcenaam**, selecteert u uw Service Bus-naamruimte. 
    4. Selecteer **toevoegen van nieuwe parameter**, en selecteer **Achtervoegselfilter**. 
    5. Voor **Achtervoegselfilter**, voer de naam van de tweede Service Bus-onderwerp-abonnement. 

        ![Ontwerper van logische Apps - gebeurtenissen configureren](./media/service-bus-to-event-grid-integration-example/logic-app-configure-event.png)
6. Selecteer **+ nieuwe stap** in de ontwerpfunctie en voer de volgende stappen uit:
    1. Zoeken naar **Service Bus**.
    2. Selecteer **Service Bus** in de lijst. 
    3. Selecteer voor **berichten ophalen** in de **acties** lijst. 
    4. Selecteer **berichten ophalen van een onderwerpabonnement (peek-lock)**. 

        ![Logic Apps Designer - actie voor get-berichten](./media/service-bus-to-event-grid-integration-example/service-bus-get-messages-step.png)
    5. Voer een **naam voor de verbinding**. Bijvoorbeeld: **Berichten van het onderwerpabonnement ontvangen**, en selecteer de Service Bus-naamruimte. 

        ![Ontwerper van logische Apps - de Service Bus-naamruimte selecteren](./media/service-bus-to-event-grid-integration-example/logic-apps-select-namespace.png) 
    6. Selecteer **RootManageSharedAccessKey**.

        ![Ontwerper van logische Apps - selecteert u de gedeelde toegangssleutel](./media/service-bus-to-event-grid-integration-example/logic-app-shared-access-key.png) 
    7. Selecteer **Maken**. 
    8. Selecteer uw onderwerp en abonnement. 
    
        ![Ontwerper van logische Apps - uw Service Bus-onderwerp en abonnement selecteren](./media/service-bus-to-event-grid-integration-example/logic-app-select-topic-subscription.png)
7. Selecteer **+ nieuwe stap**, en voer de volgende stappen uit: 
    1. Selecteer **Service Bus**.
    2. Selecteer **het bericht in een onderwerpabonnement voltooien** uit de lijst met acties. 
    3. Selecteer uw Service Bus **onderwerp**.
    4. Selecteer het tweede **abonnement** naar het onderwerp.
    5. Voor **Lock-token van het bericht**, selecteer **Lock-Token** uit de **dynamische inhoud**. 

        ![Ontwerper van logische Apps - uw Service Bus-onderwerp en abonnement selecteren](./media/service-bus-to-event-grid-integration-example/logic-app-complete-message.png)
8. Selecteer **opslaan** op de werkbalk op de ontwerper van logische Apps om op te slaan van de logische app. 
9. Volg de instructies in de [berichten verzenden naar de Service Bus-onderwerp](#send-messages-to-the-service-bus-topic) sectie berichten te verzenden naar het onderwerp. 
10. Schakel over naar de **overzicht** pagina van uw logische app. U ziet dat de logische app wordt uitgevoerd in de **geschiedenis van uitvoeringen** voor de berichten die worden verzonden.

    ![Ontwerper van logische Apps - runs voor logic Apps](./media/service-bus-to-event-grid-integration-example/logic-app-runs.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/).
* Meer informatie over [Azure Functions](https://docs.microsoft.com/azure/azure-functions/).
* Meer informatie over de [Logic Apps-functie van Azure App Service](https://docs.microsoft.com/azure/logic-apps/).
* Meer informatie over [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).


[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
