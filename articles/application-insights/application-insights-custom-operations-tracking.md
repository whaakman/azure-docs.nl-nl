---
title: Aangepaste bewerkingen met Azure Application Insights-SDK voor .NET bijhouden | Microsoft Docs
description: Bijhouden van aangepaste bewerkingen met Azure Application Insights-SDK voor .NET
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/30/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 37932dc9d4e96ee31ee0b8adf1e19fc779b5c94e
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722867"
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Aangepaste bewerkingen met Application Insights .NET-SDK bijhouden

Azure Application Insights-SDK's automatisch worden bijgehouden binnenkomende HTTP-aanvragen en aanroepen van afhankelijke services, zoals HTTP-aanvragen en SQL-query's. Wijzigingen bijhouden en correlatie van aanvragen en de afhankelijkheden geven u inzicht in de reactiesnelheid en betrouwbaarheid van de hele toepassing in alle microservices met een combinatie van deze toepassing. 

Er is een klasse toepassingspatronen die algemeen kan niet worden ondersteund. Goede bewaking van deze patronen vereist instrumentation handmatige code. In dit artikel bevat informatie over een paar patronen die mogelijk handmatige instrumentation, zoals aangepaste wachtrij verwerken en uitvoeren van langlopende achtergrondtaken.

Dit document bevat richtlijnen over het bijhouden van aangepaste bewerkingen met de Application Insights-SDK. Deze documentatie is relevant voor:

- Application Insights voor .NET (ook wel bekend als Base SDK) versie 2.4 +.
- Application Insights voor web-toepassingen (ASP.NET uitvoert) versie 2.4 +.
- Application Insights voor ASP.NET Core versie 2.1 +.

## <a name="overview"></a>Overzicht
Een bewerking is een logische werk uitvoeren door een toepassing. Een naam, de starttijd, duur, resultaat en een context van de uitvoering, zoals gebruikersnaam, eigenschappen en resultaat heeft. Als een bewerking is gestart door de bewerking B, wordt bewerking B ingesteld als een bovenliggende voor A. Een bewerking kan slechts één bovenliggende hebben, maar het kan veel onderliggende bewerkingen hebben. Zie voor meer informatie over bewerkingen en telemetriecorrelatie [Azure Application Insights-telemetriecorrelatie](application-insights-correlation.md).

In de Application Insights .NET SDK, de bewerking wordt beschreven door de abstracte klasse [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Extensibility/Implementation/OperationTelemetry.cs) en bijbehorende descendants [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/RequestTelemetry.cs) en [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/DependencyTelemetry.cs).

## <a name="incoming-operations-tracking"></a>Binnenkomende bewerkingen bijhouden 
De Application Insights web SDK worden automatisch verzameld voor HTTP-aanvragen voor ASP.NET-toepassingen die worden uitgevoerd in een pijplijn IIS en alle ASP.NET Core-toepassingen. Er zijn community-ondersteunde oplossingen voor andere platforms en frameworks. Echter, als de toepassing wordt niet ondersteund door een van de standaard of door de community-ondersteunde oplossingen, u kunt instrumenteren deze handmatig.

Een ander voorbeeld waarvoor aangepaste volgschema is de werknemer die items uit de wachtrij ontvangt. Voor bepaalde wachtrijen, wordt de aanroep van een bericht toevoegen aan deze wachtrij bijgehouden als een afhankelijkheid. De op hoog niveau bewerking die wordt beschreven berichtverwerking is echter niet automatisch verzameld.

We gaan nu kijken hoe deze bewerkingen kunnen worden bijgehouden.

Op hoog niveau, de taak is het maken van `RequestTelemetry` en bekende eigenschappen instellen. Nadat de bewerking is voltooid, kunt u de telemetrie bijhouden. Het volgende voorbeeld ziet u deze taak.

### <a name="http-request-in-owin-self-hosted-app"></a>HTTP-aanvraag in de zelf-hostend Owin-app
In dit voorbeeld trace-context wordt doorgegeven volgens de [HTTP-Protocol voor correlatie](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). U moet verwacht te ontvangen van headers die er worden beschreven.

```csharp
public class ApplicationInsightsMiddleware : OwinMiddleware
{
    private readonly TelemetryClient telemetryClient = new TelemetryClient(TelemetryConfiguration.Active);
    
    public ApplicationInsightsMiddleware(OwinMiddleware next) : base(next) {}

    public override async Task Invoke(IOwinContext context)
    {
        // Let's create and start RequestTelemetry.
        var requestTelemetry = new RequestTelemetry
        {
            Name = $"{context.Request.Method} {context.Request.Uri.GetLeftPart(UriPartial.Path)}"
        };

        // If there is a Request-Id received from the upstream service, set the telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process the request.
        try
        {
            await Next.Invoke(context);
        }
        catch (Exception e)
        {
            requestTelemetry.Success = false;
            telemetryClient.TrackException(e);
            throw;
        }
        finally
        {
            // Update status code and success as appropriate.
            if (context.Response != null)
            {
                requestTelemetry.ResponseCode = context.Response.StatusCode.ToString();
                requestTelemetry.Success = context.Response.StatusCode >= 200 && context.Response.StatusCode <= 299;
            }
            else
            {
                requestTelemetry.Success = false;
            }

            // Now it's time to stop the operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns the root ID from the '|' to the first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

De HTTP-Protocol voor correlatie declareert ook de `Correlation-Context` header. Maar wordt deze weggelaten hier voor het gemak.

## <a name="queue-instrumentation"></a>Wachtrij instrumentation
Hoewel er [HTTP-Protocol voor correlatie](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md) om door te geven correlatiedetails van de met HTTP-aanvraag, elke wachtrij-protocol is om te definiëren hoe de details van hetzelfde bericht in de wachtrij worden doorgegeven. Sommige protocollen wachtrij (zoals AMQP) kunnen aanvullende metagegevens te geven en andere (zoals Azure Storage-wachtrij) moeten de context om te worden gecodeerd in de berichtnettolading van het.

### <a name="service-bus-queue"></a>Service Bus-wachtrij
Application Insights worden bijgehouden voor Service Bus Messaging-aanroepen met de nieuwe [Microsoft Azure Service Bus-Client voor .NET](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/) versie 3.0.0 en hoger.
Als u [bericht handler patroon](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) voor het verwerken van berichten, bent u klaar: alle Service Bus-aanroepen uitgevoerd door uw service automatisch worden bijgehouden en verband houden met de andere telemetrie-items. Raadpleeg de [Service Bus-clienttoepassing traceren met Microsoft Application Insights](../service-bus-messaging/service-bus-end-to-end-tracing.md) als u handmatig-berichten verwerken.

Als u [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) pakket, lees de aanvullende - volgen voorbeelden laten zien hoe u te houden (en correleren) aanroepen naar de Service Bus AMQP-protocol maakt gebruik van Service Bus-wachtrij en Application Insights niet wachtrijbewerkingen automatisch worden bijgehouden.
Correlatie-id's worden doorgegeven in de eigenschappen van berichten.

#### <a name="enqueue"></a>In de wachtrij plaatsen

```csharp
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes the telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows the property bag to pass along with the message.
    // We will use them to pass our correlation identifiers (and other context)
    // to the consumer.
    message.Properties.Add("ParentId", operation.Telemetry.Id);
    message.Properties.Add("RootId", operation.Telemetry.Context.Operation.Id);

    try
    {
        await queue.SendAsync(message);
        
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = true;
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = false;
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

#### <a name="process"></a>Verwerken
```csharp
public async Task Process(BrokeredMessage message)
{
    // After the message is taken from the queue, create RequestTelemetry to track its processing.
    // It might also make sense to get the name from the message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
    requestTelemetry.Context.Operation.Id = rootId;
    requestTelemetry.Context.Operation.ParentId = parentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

### <a name="azure-storage-queue"></a>Azure Storage-wachtrij
Het volgende voorbeeld laat zien hoe voor het bijhouden van de [Azure Storage-wachtrij](../storage/queues/storage-dotnet-how-to-use-queues.md) bewerkingen en correleren telemetrie tussen de producent, de consument- en Azure Storage. 

De opslagwachtrij heeft een HTTP-API. Alle aanroepen naar de wachtrij worden bijgehouden door de Collector Application Insights-afhankelijkheid voor HTTP-aanvragen.
Zorg ervoor dat u hebt `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config`. Als u deze niet hebt, deze toevoegen via een programma zoals beschreven in [voor filteren en voorverwerking in de Azure Application Insights SDK](app-insights-api-filtering-sampling.md).

Als u Application Insights handmatig configureren, moet u maken en initialiseren `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` op dezelfde manier naar:
 
```csharp
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection to some domains by adding it to the excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on the Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget to dispose of the module during application shutdown.
```

Ook kunt u correlaties zichtbaar maken tussen de bewerkings-ID van de Application Insights met de opslag-aanvraag-ID. Zie voor meer informatie over het instellen en ophalen van een client van de aanvraag voor opslag en de aanvraag-ID van een server [bewaken, problemen vaststellen en oplossen van Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>In de wachtrij plaatsen
Omdat Storage-wachtrijen de HTTP-API ondersteunen, worden alle bewerkingen aan de wachtrij automatisch bijgehouden door Application Insights. In veel gevallen moet deze instrumentatie voldoende. Echter, voor het correleren van traceringen consumers met producent traceringen, u moet bepaalde context kan doorgeven correlatie op dezelfde manier hoe we doen in het HTTP-Protocol voor correlatie. 

In dit voorbeeld laat zien hoe voor het bijhouden van de `Enqueue` bewerking. U kunt:

 - **Nieuwe pogingen correleren (indien aanwezig)**: hebben ze een gemeenschappelijke bovenliggende die ook is de `Enqueue` bewerking. Ze zijn anders als onderliggende items van de inkomende aanvraag bijgehouden. Als er meerdere logische aanvragen naar de wachtrij worden, is het mogelijk dat het moeilijk te vinden waarvan het gesprek leidde tot nieuwe pogingen.
 - **Opslaglogboeken correleren (of en wanneer nodig)**: ze zijn gecorreleerd met Application Insights telemetry.

De `Enqueue` bewerking is het onderliggende lid van een bovenliggende bewerking (bijvoorbeeld, een binnenkomende HTTP-aanvraag). De HTTP-afhankelijkheidsaanroep is het onderliggende lid van de `Enqueue` bewerking en de onderliggend van de inkomende aanvraag:

```csharp
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose to pass payload serialized to JSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message to process'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id to the OperationContext to correlate Storage logs and Application Insights telemetry.
    OperationContext context = new OperationContext { ClientRequestID = operation.Telemetry.Id};

    try
    {
        await queue.AddMessageAsync(queueMessage, null, null, new QueueRequestOptions(), context);
    }
    catch (StorageException e)
    {
        operation.Telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.Telemetry.Success = false;
        operation.Telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}  
```

Te verminderen de hoeveelheid telemetrie van uw toepassing rapporten of als u niet wilt dat voor het bijhouden van de `Enqueue` bewerking om andere redenen, gebruik de `Activity` API nu rechtstreeks:

- Maken (en starten) een nieuwe `Activity` in plaats van de Application Insights-bewerking starten. U doet *niet* alle eigenschappen, behalve de naam van de bewerking toe te wijzen.
- Serialiseren `yourActivity.Id` in de berichtnettolading van het in plaats van `operation.Telemetry.Id`. U kunt ook `Activity.Current.Id`.


#### <a name="dequeue"></a>Uit de wachtrij verwijderen
Op dezelfde manier naar `Enqueue`, een HTTP-aanvraag naar de opslagwachtrij is automatisch bijgehouden door Application Insights. Echter, de `Enqueue` bewerking waarschijnlijk in de context van de bovenliggende, zoals de aanvraagcontext van een binnenkomende gebeurt. Application Insights-SDK's correleren automatisch dergelijke een bewerking (en de HTTP-onderdeel) met de bovenliggende aanvraag en andere telemetrie die zijn gerapporteerd in hetzelfde bereik.

De `Dequeue` bewerking is moeilijk. HTTP-aanvragen automatisch worden bijgehouden in de Application Insights-SDK. Maar weet niet de correlatie-context totdat het bericht wordt geparseerd. Het is niet mogelijk om te correleren van de HTTP-aanvraag om op te halen van het bericht met de rest van de telemetrie.

In veel gevallen kan het nuttig zijn de HTTP-aanvraag naar de wachtrij correleren met andere traceringen ook zijn. Het volgende voorbeeld ziet u hoe u om dit te doen:

```csharp
public async Task<MessagePayload> Dequeue(CloudQueue queue)
{
    var telemetry = new DependencyTelemetry
    {
        Type = "Queue",
        Name = "Dequeue " + queue.Name
    };

    telemetry.Start();

    try
    {
        var message = await queue.GetMessageAsync();

        if (message != null)
        {
            var payload = JsonConvert.DeserializeObject<MessagePayload>(message.AsString);

            // If there is a message, we want to correlate the Dequeue operation with processing.
            // However, we will only know what correlation ID to use after we get it from the message,
            // so we will report telemetry after we know the IDs.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // Delete the message.
            return payload;
        }
    }
    catch (StorageException e)
    {
        telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        telemetry.Success = false;
        telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetry.Stop();
        telemetryClient.Track(telemetry);
    }

    return null;
}
```

#### <a name="process"></a>Verwerken

In het volgende voorbeeld wordt wordt een binnenkomend bericht bijgehouden in een manier op dezelfde manier naar binnenkomende HTTP-aanvraag:

```csharp
public async Task Process(MessagePayload message)
{
    // After the message is dequeued from the queue, create RequestTelemetry to track its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // It might also make sense to get the name from the message.
    requestTelemetry.Context.Operation.Id = message.RootId;
    requestTelemetry.Context.Operation.ParentId = message.ParentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

Op deze manier kunnen andere wachtrijbewerkingen worden geïmplementeerd. Een peek-bewerking moet worden geïmplementeerd op een soortgelijke manier als een bewerking uit de wachtrij verwijderen. Wachtrij-beheerbewerkingen instrumenteren is niet nodig. Application Insights bewerkingen, zoals HTTP worden bijgehouden en in de meeste gevallen is voldoende.

Tijdens het instrumenteren van verwijdering van berichten, zorg ervoor dat u de bewerking-id's (correlatie) instelt. U kunt ook kunt u de `Activity` API. Vervolgens moet u geen bewerking-id's op de telemetrie-items instellen omdat Application Insights-SDK het allemaal voor u doet:

- Maak een nieuwe `Activity` nadat u hebt een item uit de wachtrij.
- Gebruik `Activity.SetParentId(message.ParentId)` consumenten en producent logboeken correleren.
- Start de `Activity`.
- Spoor uit de wachtrij verwijderen, verwerken en verwijderbewerkingen met behulp van `Start/StopOperation` hulpprogramma's. Dit doen vanuit de dezelfde asynchrone Controlestroom (uitvoeringscontext). Op deze manier bent ze gecorreleerd naar behoren.
- Stop de `Activity`.
- Gebruik `Start/StopOperation`, of neem contact op `Track` telemetrie handmatig.

### <a name="batch-processing"></a>Batchverwerking
Sommige wachtrijen, kunt u meerdere berichten met één aanvraag in wachtrij. Deze berichten worden verwerkt, is waarschijnlijk onafhankelijk en behoort tot de verschillende logische bewerkingen. In dit geval het is niet mogelijk om te correleren de `Dequeue` bewerking bepaalde berichtverwerking.

Elk bericht dat moet worden verwerkt in een eigen asynchrone Controlestroom. Zie voor meer informatie de [uitgaande afhankelijkheden bijhouden](#outgoing-dependencies-tracking) sectie.

## <a name="long-running-background-tasks"></a>Langlopende achtergrondtaken
Sommige toepassingen starten langlopende bewerkingen die kunnen worden veroorzaakt door aanvragen van gebruikers. Vanuit het perspectief tracering/instrumentation verschilt niet van de aanvraag of een afhankelijkheid instrumentation: 

```csharp
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<RequestTelemetry>(taskName);
    operation.Telemetry.Type = "Background";
    try
    {
        int progress = 0;
        while (progress < 100)
        {
            // Process the task.
            telemetryClient.TrackTrace($"done {progress++}%");
        }
        // Update status code and success as appropriate.
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Update status code and success as appropriate.
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

In dit voorbeeld `telemetryClient.StartOperation` maakt `RequestTelemetry` en vult u de correlatie-context. Stel dat u hebt een bovenliggende bewerking die is gemaakt door binnenkomende aanvragen die de bewerking gepland. Zolang `BackgroundTask` wordt gestart in dezelfde asynchrone Controlestroom als een binnenkomende aanvraag, deze worden gecorreleerd met die bovenliggende voor deze bewerking. `BackgroundTask` en alle geneste telemetrie-items automatisch worden gecorreleerd met de aanvraag, waardoor er, zelfs nadat de aanvraag is beëindigd.

Wanneer de taak wordt gestart vanuit de achtergrondthread die geen elke bewerking (`Activity`) die zijn gekoppeld aan deze `BackgroundTask` beschikt niet over een bovenliggende. Echter, het kunt geneste bewerkingen. Alle telemetrie-items gerapporteerd door de taak worden gecorreleerd met de `RequestTelemetry` gemaakt in `BackgroundTask`.

## <a name="outgoing-dependencies-tracking"></a>Uitgaande afhankelijkheden bijhouden
U kunt uw eigen soort afhankelijkheid of een bewerking die niet wordt ondersteund door Application Insights bijhouden.

De `Enqueue` methode in de Service Bus-wachtrij of de Storage-wachtrij kan fungeren als voorbeelden van dergelijke aangepaste bijhouden.

De algemene benadering voor het bijhouden van aangepaste afhankelijkheid is:

- Roep de `TelemetryClient.StartOperation` (extensie)-methode die wordt ingevuld de `DependencyTelemetry` eigenschappen die nodig zijn voor correlatie- en andere eigenschappen (en-tijd tijdstempel, duur).
- Andere aangepaste eigenschappen worden ingesteld op de `DependencyTelemetry`, zoals de naam en een andere context die u nodig hebt.
- Een afhankelijkheid aanroepen en wacht totdat deze maken.
- Stoppen van de bewerking opnieuw uit met `StopOperation` wanneer deze voltooid.
- Het verwerken van uitzonderingen.

```csharp
public async Task RunMyTaskAsync()
{
    using (var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1"))
    {
        try 
        {
            var myTask = await StartMyTaskAsync();
            // Update status code and success as appropriate.
        }
        catch(...) 
        {
            // Update status code and success as appropriate.
        }
    }
}
```

Verwijdering van de bewerking zorgt ervoor dat bewerking moet worden gestopt, zodat u deze in plaats van aanroepen doen kunt `StopOperation`.

*Waarschuwing*: in sommige gevallen unhanded uitzondering kan [te voorkomen dat](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/try-finally) `finally` om te worden aangeroepen, zodat bewerkingen kunnen niet worden gevolgd.

### <a name="parallel-operations-processing-and-tracking"></a>Parallelle bewerkingen kunnen worden verwerkt en bijhouden

`StopOperation` alleen stopt de bewerking die is gestart. Als de huidige actieve bewerking komt niet overeen met de versie die u stoppen wilt, `StopOperation` , gebeurt er niets. Deze situatie kan gebeuren als u meerdere bewerkingen gelijktijdig in de dezelfde uitvoeringscontext starten:

```csharp
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// FAILURE!!! This will do nothing and will not report telemetry for the first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

Zorg ervoor dat u altijd aanroepen `StartOperation` en verwerken van de bewerking in dezelfde **asynchrone** methode voor het isoleren van bewerkingen gelijktijdig uitgevoerd. Als de bewerking is synchroon (of niet asynchrone) proces verpakken en bij te houden met `Task.Run`:

```csharp
public void RunMyTask(string name)
{
    using (var operation = telemetryClient.StartOperation<DependencyTelemetry>(name))
    {
        Process();
        // Update status code and success as appropriate.
    }
}

public async Task RunAllTasks()
{
    var task1 = Task.Run(() => RunMyTask("task 1"));
    var task2 = Task.Run(() => RunMyTask("task 2"));
    
    await Task.WhenAll(task1, task2);
}
```

## <a name="next-steps"></a>Volgende stappen

- Leer de basisprincipes van [telemetriecorrelatie](application-insights-correlation.md) in Application Insights.
- Zie de [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en -gegevensmodel.
- Aangepaste rapport [gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md) naar Application Insights.
- Bekijk standard [configuratie](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) voor context eigenschappen verzameling.
- Controleer de [System.Diagnostics.Activity gebruikershandleiding](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) om te zien hoe we correleren van telemetrie.
