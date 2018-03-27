---
title: Voorbeelden van de integratie van Azure Service Bus met Event Grid | Microsoft Docs
description: In dit artikel vindt u voorbeelden van de integratie van Service Bus-berichten met Event Grid.
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: fd30a8eb5149647a24ff04e099bf5c3e187459ef
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-service-bus-to-azure-event-grid-integration-examples"></a>Voorbeelden van de integratie van Azure Service Bus met Event Grid

In dit artikel leert u hoe u een Azure-functie en een logische app instelt die beide berichten ontvangen op basis van het ontvangen van een gebeurtenis van Azure Event Grid. U voert het volgende uit:
 
* Maak een eenvoudige [Azure-testfunctie](#test-function-setup) voor het opsporen van fouten en het weergeven van de eerste stroom gebeurtenissen uit Event Grid. Voer deze stap in elk geval uit, ongeacht of u de andere stappen uitvoert.
* Maak een [Azure-functie waarmee u Service Bus-berichten kunt ontvangen en verwerken](#receive-messages-using-azure-function) op basis van Event Grid-gebeurtenissen.
* Gebruik de [Logic Apps-functie van Azure App Service](#receive-messages-using-azure-logic-app).

Voor het voorbeeld dat u maakt, wordt ervanuit gegaan dat het Service Bus-onderwerp twee abonnementen kent. Er wordt ook vanuit gegaan dat het Event Grid-abonnement is gemaakt voor het verzenden van gebeurtenissen voor slechts één Service Bus-abonnement. 

In het voorbeeld verzendt u berichten naar het Service Bus-onderwerp en verifieert u of de gebeurtenis is gegenereerd voor dit Service Bus-abonnement. De functie of logische app ontvangt de berichten van het Service Bus-abonnement en voltooit dit vervolgens.

## <a name="prerequisites"></a>Vereisten
Voor u begint dient u de stappen in de volgende twee secties te hebben voltooid.

### <a name="create-a-service-bus-namespace"></a>Een Service Bus-naamruimte maken

Maak een Service Bus Premium-abonnement en een Service Bus-onderwerp met twee abonnementen.

### <a name="send-a-message-to-the-service-bus-topic"></a>Een bericht verzenden naar het Service Bus-onderwerp

U kunt een willekeurige methode gebruiken om een bericht naar uw Service Bus-onderwerp te verzenden. In de voorbeeldcode aan het einde van deze procedure wordt ervan uitgegaan dat u van Visual Studio 2017 gebruikmaakt.

1. Kloon de [GitHub azure-service-bus repository](https://github.com/Azure/azure-service-bus/) (opslagplaats van GitHub-azure-service-bus).

2. Ga in Visual Studio naar de map *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration* en open het bestand *SBEventGridIntegration.sln*.

3. Ga naar het project **MessageSender** en selecteer **Program.cs**.

   ![8][]

4. Vul de naam van het onderwerp en de verbindingsreeks in en voer de volgende consoletoepassingscode uit:

    ```CSharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```

## <a name="set-up-a-test-function"></a>Testfunctie instellen

Voordat u het hele scenario doorwerkt, dient u in elk geval een kleine testfunctie in te stellen. Deze kunt u gebruiken om te zien welke gebeurtenissen er worden gestroomd en om de fouten erin op te sporen.

1. Maak in de Azure Portal een nieuwe Azure Functions-toepassing. Zie [Documentatie van Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/) voor de basisbeginselen van Azure Functions.

2. Selecteer in de functie die u net hebt gemaakt het plusteken (+) om een HTTP-triggerfunctie toe te voegen:

    ![2][]
    
    Het venster **Ga snel aan de slag met een vooraf gemaakte functie** wordt geopend.

    ![3][]

3. Selecteer de knop **Webhook + API**, selecteer **CSharp** en vervolgens **Deze functie maken**.
 
4. Plak de volgende code in de functie:

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

5. Selecteer **Opslaan en uitvoeren**.

## <a name="connect-the-function-and-namespace-via-event-grid"></a>Functie en naamruimte met elkaar verbinden via Event Grid

In deze sectie koppelt u de functie en de Service Bus-naamruimte. Gebruik voor dit voorbeeld Azure. Zie [Overzicht integratie Azure Service Bus en Event Grid](service-bus-to-event-grid-integration-concept.md) om te begrijpen hoe u PowerShell of Azure CLI gebruikt om deze procedure uit te voeren.

Ga als volgt te werk als u een Azure Event Grid-abonnement wilt maken:
1. Ga in Azure Portal naar uw naamruimte en selecteer **Event Grid** in het linkerdeelvenster.  
    Het venster met de naamruimte wordt geopend, waarbij twee Event Grid-abonnementen in het rechterdeelvenster worden weergegeven.

    ![20][]

2. Selecteer **Gebeurtenisabonnement**.  
    Het venster **Gebeurtenisabonnement** wordt geopend. In de volgende afbeelding wordt een formulier weergegeven waarmee u zich kunt abonneren op een Azure-functie of webhook zonder filters toe te passen.

    ![21][]

3. Vul het formulier in zoals weergegeven en voer in het vak **Achtervoegselfilter** het relevante filter in.

4. Selecteer **Maken**.

5. Verzend een bericht naar uw Service Bus-onderwerp, zoals vermeld in de Sectie Vereisten, en controleer met behulp van de bewakingsfunctie van Azure Functions of de gebeurtenissen stromen.

De volgende stap bestaat uit het koppelen van de functie en de Service Bus-naamruimte. Gebruik voor dit voorbeeld Azure. Zie [Overzicht integratie Azure Service Bus en Event Grid](service-bus-to-event-grid-integration-concept.md) om te begrijpen hoe u PowerShell of Azure CLI gebruikt om deze stap uit te voeren.

![9][]

### <a name="receive-messages-by-using-azure-functions"></a>Berichten ontvangen met behulp van Azure Functions

In de vorige sectie hebt u een eenvoudig test- en foutopsporingsscenario gezien, en gecontroleerd of de gebeurtenissen stromen. 

In deze sectie leert u hoe u berichten ontvangt en verwerkt nadat u een gebeurtenis hebt ontvangen.

U voegt een Azure-functie toe, zoals in het volgende voorbeeld wordt getoond, omdat de Service Bus-functies in Azure Functions de nieuwe Event Grid-integratie nog niet vanuit het systeem zelf ondersteunen.

1. Selecteer **ReceiveMessagesOnEvent.cs** in dezelfde Visual Studio-oplossing die u als onderdeel van de vereisten hebt geopend. 

    ![10][]

2. Voer uw verbindingsreeks in de volgende code in:

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```

3. Download in Azure Portal het publicatieprofiel voor de Azure-functie die u hebt gemaakt in de sectie Testfunctie instellen.

    ![11][]

4. Klik vervolgens in Visual Studio met de rechtermuisknop op **SBEventGridIntegration** selecteer **Publish**. 

5. In het deelvenster **Publish** voor het publicatieprofiel dat u eerder hebt gedownload, selecteert u **Import profile** en vervolgens **Publish**.

    ![12][]

6. Nadat u de nieuwe Azure-functie hebt gepubliceerd, maakt u een nieuw Azure Event Grid-abonnement dat verwijst naar de nieuwe Azure-functie.  
    Zorg ervoor dat u in het vak **Eindigt op** het juiste filter toepast, wat zou moeten overeenkomen met de naam van uw Service Bus-abonnement.

7. Verzend een bericht naar het Azure Service Bus-onderwerp dat u eerder hebt gemaakt en controleer vervolgens het Azure Functions-logboek in Azure Portal om er zeker van te zijn dat er gebeurtenissen stromen en berichten worden ontvangen.

    ![12-1][]

### <a name="receive-messages-by-using-logic-apps"></a>Berichten ontvangen met behulp van Logic Apps

Verbind als volgt een logische app met Azure Service Bus en Azure Event Grid:

1. Maak een nieuwe logische app in Azure Portal en selecteer **Event Grid** als eerste actie.

    ![13][]

    Het venster Ontwerper van logische apps wordt geopend.

    ![14][]

2. Voeg als volgt uw gegevens toe:

    a. Voer in het vak **Resourcenaam** de naam in van uw eigen naamruimte. 

    b. Voer onder **Geavanceerde opties**, in het vak **Achtervoegselfilter**, het filter in voor uw abonnement.

3. Voeg vervolgens een ontvangstactie van Service Bus toe om berichten van een onderwerpabonnement te ontvangen.  
    De laatste actie wordt in de volgende afbeelding weergegeven:

    ![15][]

4. Voeg een voltooide gebeurtenis toe, zoals in de volgende afbeelding weergegeven:

    ![16][]

5. Sla de logische app op en verzend een bericht naar uw Service Bus-onderwerp, zoals vermeld in de sectie Vereisten.  
    Bekijk hoe de logische app wordt uitgevoerd. Als u meer gegevens voor het uitvoeren wilt bekijken, selecteert u **Overzicht** en bekijkt u de gegevens onder  **	Geschiedenis van uitvoeringen**.

    ![17][]

    ![18][]

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
