---
title: Voorbeelden van de integratie van Azure Service Bus met Event Grid | Microsoft Docs
description: Voorbeelden van de integratie van Service Bus-berichten met Event Grid
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 2a4d17673340d145de9a3514f920c74f7eebf6b6
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="azure-service-bus-to-azure-event-grid-examples"></a>Voorbeelden van Azure Service Bus naar Azure Event Grid

In dit document vindt u informatie over het instellen van Azure-functies en een logische app die op basis van een gebeurtenis van Event Grid berichten ontvangen. In het voorbeeld wordt uitgegaan van een Service Bus-onderwerp met twee abonnementen, en dat het Event Grid-abonnement is gemaakt op een manier dat er gebeurtenissen voor slechts één Service Bus-abonnement worden verzonden. Vervolgens verzendt u berichten naar het Service Bus-onderwerp en controleert u of de gebeurtenis is gegenereerd voor dit Service Bus-abonnement, en of de functie of de logische app de berichten ontvangt van dat Service Bus-abonnement, en ze voltooit.

* Zorg eerst dat u aan alle [vereisten](#prerequisites) voldoet.
* Maak een [eenvoudige testversie van Azure Functions](#test-function-setup) voor het opsporen van fouten en om te zien of er een eerste stroom van gebeurtenissen uit Event Grid op gang is gekomen.  **Deze stap moet worden gedaan ongeacht of u 3. of 4. uitvoert**
* Maak een [Azure-functie waarmee u Service Bus-berichten kunt ontvangen en verwerken ](#receive-messages-using-azure-function) op basis van Event Grid-gebeurtenissen.
* [Logic Apps](#receive-messages-using-azure-logic-app) gebruiken.

## <a name="prerequisites"></a>Vereisten

### <a name="service-bus-namespace"></a>Service Bus-naamruimte

Een Service Bus Premium-naamruimte maken. Een Service Bus-onderwerp met twee abonnementen maken.

### <a name="code-to-send-message-to-the-service-bus-topic"></a>Code voor het verzenden van een bericht naar het Service Bus-onderwerp

U kunt op alle mogelijke manieren een bericht naar uw Service Bus-onderwerp verzenden. U kunt ook het voorbeeld hieronder gebruiken. Voor de voorbeeldcode wordt ervan uitgegaan dat u Visual Studio 2017 gebruikt.

Kloon deze [GitHub-opslagplaats](https://github.com/Azure/azure-service-bus/).

Navigeer naar de volgende map:

\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration en open het bestand SBEventGridIntegration.sln.

Vervolgens gaat u naar het project MessageSender en opent u Program.cs.

![8][]

Vul de onderwerpnaam in, evenals de verbindingsreeks en voer de consoletoepassing uit:

```CSharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
const string TopicName = "YOUR TOPIC NAME";
```

## <a name="test-function-setup"></a>Instellingen voor de testfunctie

Voordat u het gehele end-to-end-scenario gaat doorwerken, wilt u waarschijnlijk eerst een summiere testfunctie tot uw beschikking hebben, die u kunt gebruiken voor het opsporen van fouten en om te zien welke gebeurtenissen er stromen.

Maak in de portal een nieuwe Azure Functions-toepassing. Volg deze [koppeling](https://docs.microsoft.com/en-us/azure/azure-functions/) voor meer informatie over de basisprincipes van Azure Functions.

Klik in de functie die u net hebt gemaakt op het kleine plusteken om een HTTP-triggerfunctie toe te voegen:

![2][]

![3][]

Kopieer vervolgens de volgende code in de functie:

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

Klik op Opslaan en uitvoeren.

## <a name="connect-function-and-namespace-via-event-grid"></a>Functie en naamruimte met elkaar verbinden via Event Grid

De volgende stap is om de functie en de Service Bus-naamruimte aan elkaar te koppelen. Gebruik voor dit voorbeeld Azure. Zie de pagina [concepts](service-bus-to-event-grid-integration-concept.md als u wilt weten hoe u hiervoor ook PowerShell of Azure CLI kunt gebruiken.

Als u een nieuw Azure Event Grid-abonnement wilt maken navigeert u naar uw naamruimte in Azure Portal en selecteert u de blade Event Grid. Klik op + Event Grid-abonnement.

In de volgende schermafbeelding ziet u een naamruimte waarvoor al een aantal Event Grid-abonnementen bestaan.

![20][]

In de volgende schermafbeelding ziet u hoe u zich kunt abonneren op een Azure-functie of een webhook, zonder dat er een specifiek filter nodig is. Vergeet niet dat u het betreffende filter voor uw Service Bus-abonnement als 'achtervoegselfilter' toevoegt:

![21][]

Verzend een bericht naar uw Service Bus-onderwerp, zoals vermeld in de vereisten, en controleer met behulp van de monitorfunctie van Azure Functions of de gebeurtenissen stromen.

![9][]

### <a name="receive-messages-using-azure-function"></a>Berichten ontvangen met behulp van Azure Functions

In de vorige sectie hebt u een eenvoudig test- en foutopsporingsscenario doorlopen, en gecontroleerd of de gebeurtenissen stromen. In dit gedeelte van de documentatie bekijkt u hoe u berichten kunt ontvangen en verwerken nadat u een gebeurtenis hebt ontvangen.

De reden waarom een Azure-functie op de volgende manier wordt toegevoegd, is dat de Service Bus-functies binnen Azure Functions zelf nog niet op een systeemeigen manier ondersteuning bieden voor de nieuwe Event Grid-integratie.

SelecteerReceiveMessagesOnEvent.cs in de dezelfde Visual Studio-oplossing die u als onderdeel van de vereisten hebt geopend. Voer uw verbindingsreeks in de code in:

![10][]

```Csharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
```

Ga vervolgens naar Azure Portal en download het publicatieprofiel voor de Azure-functie die u voorheen in [2. Instellingen voor de testfunctie](#2-test-function-setup) hebt gemaakt.

![11][]

Klik vervolgens in Visual Studio met de rechtermuisknop op SBEventGridIntegration selecteer Publiceren. Gebruik het publicatieprofiel dat u eerder hebt gedownload, selecteer Profiel importeren en klik op Publiceren.

![12][]

Nadat u de nieuwe Azure-functie hebt gepubliceerd, maakt u een nieuw Azure Event Grid-abonnement dat verwijst naar de nieuwe Azure-functie. Zorg ervoor dat u het juiste 'eindigt op'-filter toepast, wat zou moeten overeenkomen met de naam van uw Service Bus-abonnement.

Verzendt vervolgens een bericht naar het Azure Service- onderwerp dat u eerder hebt gemaakt en bestudeer in de portal het logboek van Azure Functions om te zien of de gebeurtenissen stromen en of berichten worden ontvangen.

![12-1][]

### <a name="receive-messages-using-azure-logic-app"></a>Berichten ontvangen met behulp van een logische Azure-app

De volgende instructies laten zien hoe u verbinding kunt maken tussen een logische Azure-app en Azure Service Bus en Azure Event Grid:

Maak eerst een nieuwe logische app in Azure Portal en selecteer Event Grid als beginactie.

![13][]

De eerste weergave in de ontwerpfunctie voor Logic Apps zou er moeten uitzien zoals in de volgende schermafbeelding, waarbij u 'Resourcenaam' vervangt door de naam van uw eigen naamruimte. Zorg er ook voor dat u de geavanceerde opties uitvouwt, en voeg het achtervoegselfilter toe voor uw abonnement:

![14][]

Voeg vervolgens een ontvangstactie van Service Bus toe om iets van een onderwerpabonnement te kunnen ontvangen. De laatste actie moet er ongeveer uitzien zoals in de volgende schermafbeelding.

![15][]

Voeg vervolgens een volledige gebeurtenis toe, die er als volgt zou moeten uitzien.

![16][]

Sla de logische app op en verzend een bericht naar uw Service Bus-onderwerp, zoals vermeld in de vereisten. Kijk vervolgens hoe de logische app wordt uitgevoerd. Als u op 'Overzicht' klikt, en daarin vervolgens weer op 'Geschiedenis van uitvoeringen' klikt, kunt u ook meer gegevens over de uitvoering krijgen.

![17][]

![18][]

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/).
* Meer informatie over [Azure Functions](https://docs.microsoft.com/azure/azure-functions/).
* Meer informatie over [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/).
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
