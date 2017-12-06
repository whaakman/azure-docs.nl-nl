---
title: Bijhouden van aangepaste bewerkingen met Azure Application Insights-SDK voor .NET | Microsoft Docs
description: Het bijhouden van aangepaste bewerkingen met Azure Application Insights-SDK voor .NET
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/30/2017
ms.author: sergkanz
ms.openlocfilehash: 18712b1c19fc81e290ead62f73a177874ebe86cd
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Bijhouden van aangepaste bewerkingen met Application Insights-SDK voor .NET

Azure Application Insights-SDK's automatisch worden bijgehouden binnenkomende HTTP-aanvragen en aanroepen naar afhankelijke services, zoals HTTP-aanvragen en SQL-query's. Bijhouden en correlatie van aanvragen en afhankelijkheden bieden u inzicht in de reactietijd en betrouwbaarheid van de hele toepassing op alle microservices met een combinatie van deze toepassing. 

Er is een klasse die van toepassing patronen die algemeen kan niet worden ondersteund. Goede bewaking van deze patronen vereist handmatige code instrumentation. In dit artikel bevat informatie over een paar patronen die mogelijk handmatige instrumentation, zoals aangepaste wachtrij verwerken en langlopende achtergrondtaken uitvoeren.

Dit document biedt richtlijnen voor het bijhouden van aangepaste bewerkingen met Application Insights-SDK. Deze documentatie is relevant voor:

- Application Insights voor .NET (ook wel bekend als Base SDK) versie 2.4 +.
- Application Insights voor web-toepassingen (met ASP.NET) versie 2.4 +.
- Application Insights voor ASP.NET Core versie 2.1 +.

## <a name="overview"></a>Overzicht
Een bewerking is een logische werk uitgevoerd door een toepassing. Een naam, de starttijd, duur, resultaat en een context van de uitvoering zoals gebruikersnaam, eigenschappen en resultaat heeft. Als een bewerking is gestart door de bewerking B, wordt bewerking B ingesteld als een bovenliggend element van A. Een bewerking kan slechts één bovenliggend hebben, maar er veel onderliggende bewerkingen. Zie voor meer informatie over bewerkingen en telemetrie correlatie [Azure Application Insights telemetrie correlatie](application-insights-correlation.md).

De bewerking wordt in de Application Insights .NET SDK beschreven door de abstracte klasse [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Extensibility/Implementation/OperationTelemetry.cs) en afgeleiden [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/RequestTelemetry.cs) en [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/DependencyTelemetry.cs).

## <a name="incoming-operations-tracking"></a>Binnenkomende operations bijhouden 
De Application Insights web SDK verzamelt automatisch HTTP-aanvragen voor ASP.NET-toepassingen die worden uitgevoerd in een IIS-pijplijn en alle toepassingen die ASP.NET Core. Er zijn oplossingen voor andere platforms en frameworks community ondersteund. Echter, als de toepassing wordt niet ondersteund door een van de standaard of community ondersteund oplossingen, u kunt softwareontwikkelaars deze handmatig.

Een ander voorbeeld waarvoor aangepaste bijhouden is de werknemer die items uit de wachtrij ontvangt. Voor sommige wachtrijen, wordt de aanroep van een bericht toevoegen aan deze wachtrij bijgehouden als een afhankelijkheid. De bewerking voor op hoog niveau die worden beschreven berichtverwerking is echter niet automatisch verzameld.

Laten we zien hoe deze bewerkingen kunnen worden bijgehouden.

Op hoog niveau, wordt de taak is het maken van `RequestTelemetry` en bekende eigenschappen instellen. Nadat de bewerking is voltooid, kunt u de telemetrie bijhouden. Het volgende voorbeeld wordt met deze taak.

### <a name="http-request-in-owin-self-hosted-app"></a>HTTP-aanvraag in host zichzelf Owin-app
In dit voorbeeld volgt we de [HTTP-Protocol voor correlatie](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). U moet verwacht te ontvangen van de headers die er worden beschreven.

``` C#
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

Het HTTP-Protocol voor correlatie declareert ook de `Correlation-Context` header. Echter, dit wordt weggelaten hier voor eenvoud.

## <a name="queue-instrumentation"></a>Wachtrij instrumentation
Wij hebben een protocol om te slagen correlatie details gemaakt voor HTTP-communicatie. Met de sommige wachtrijen protocollen, kunt u aanvullende metagegevens samen met het bericht en met anderen die u niet doorgeven.

### <a name="service-bus-queue"></a>Service Bus-wachtrij
Met de Azure [Service Bus-wachtrij](../service-bus-messaging/index.md), kunt u een eigenschappenverzameling samen met het bericht doorgeven. We gebruiken deze om door te geven van de correlatie-ID.

De Service Bus-wachtrij maakt gebruik van TCP-protocollen. Application Insights niet automatisch bijgehouden voor bewerkingen van de wachtrij, zodat we ze handmatig bijhouden. De wachtrij halen-bewerking is een push-stijl-API en kan niet worden bijhouden.

#### <a name="enqueue"></a>In de wachtrij plaatsen

```C#
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

#### <a name="process"></a>Proces
```C#
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
Het volgende voorbeeld ziet u het bijhouden van de [Azure Storage-wachtrij](../storage/queues/storage-dotnet-how-to-use-queues.md) bewerkingen en vergelijken telemetrie tussen de producent, de consumer en Azure Storage. 

De opslagwachtrij heeft een HTTP-API. Alle aanroepen naar de wachtrij worden bijgehouden door de Application Insights afhankelijkheid Collector voor HTTP-aanvragen.
Zorg ervoor dat u hebt `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config`. Als u dit niet hebt u deze toevoegen via een programma zoals beschreven in [filteren en in de Application Insights-SDK van Azure voorverwerking](app-insights-api-filtering-sampling.md).

Als u de Application Insights handmatig configureert, moet u maken en initialiseren `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` als:
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection to some domains by adding it to the excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on the Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget to dispose of the module during application shutdown.
```

Ook kunt u correlaties zichtbaar maken tussen de Application Insights bewerkings-ID met de opslag-aanvraag-ID. Zie voor meer informatie over het instellen en ophalen van een client van de aanvraag opslag en de aanvraag-ID van een server [Monitor, vaststellen en oplossen van Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>In de wachtrij plaatsen
Omdat opslagwachtrijen de HTTP-API ondersteunt, worden alle bewerkingen met de wachtrij automatisch bijgehouden door de Application Insights. In veel gevallen kan moeten deze instrumentation voldoende. Echter voor de bijbehorende traceringen aan de kant van de consument met producent traceringen, moet u doorgeven context correlatie op dezelfde manier aan hoe we in het HTTP-Protocol voor correlatie doen. 

In dit voorbeeld wordt de optionele bijhouden `Enqueue` bewerking. U kunt:

 - **Nieuwe pogingen correleren (indien aanwezig)**: alle hebben een gemeenschappelijke bovenliggende die heeft de `Enqueue` bewerking. Ze zijn anders bijgehouden als onderliggende elementen van de binnenkomende aanvraag. Als er meerdere logische aanvragen naar de wachtrij, kan het lastig zijn om te zoeken welke aanroep heeft nieuwe pogingen.
 - **Opslag logboeken correleren (of en wanneer nodig)**: ze zijn gecorreleerd met Application Insights telemetrie.

De `Enqueue` bewerking is het onderliggende lid van een bovenliggende-bewerking (bijvoorbeeld, een binnenkomende HTTP-aanvraag). De HTTP-aanroep voor afhankelijkheid is het onderliggende lid van de `Enqueue` bewerking en de onderliggend van de inkomende aanvraag:

```C#
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

Ter verlaging van de hoeveelheid telemetrie uw toepassing rapporten of als u niet wilt bijhouden de `Enqueue` bewerking om andere redenen, gebruik de `Activity` rechtstreeks API:

- Maak (en start) een nieuwe `Activity` in plaats van het begin van de Application Insights-bewerking. U doet *niet* eventuele eigenschappen, behalve de naam van de bewerking toe te wijzen.
- Serialiseren `yourActivity.Id` in de nettolading van het bericht in plaats van `operation.Telemetry.Id`. U kunt ook `Activity.Current.Id`.


#### <a name="dequeue"></a>In wachtrij
Als `Enqueue`, een werkelijke HTTP-aanvraag naar de wachtrij opslag automatisch wordt gevolgd door Application Insights. Echter, de `Enqueue` -bewerking wordt waarschijnlijk uitgevoerd in de context van de bovenliggende, zoals de aanvraagcontext van een binnenkomende. Application Insights-SDK's correleren automatisch dergelijke bewerking (en het onderdeel HTTP) met de bovenliggende aanvraag en andere telemetrie die zijn gerapporteerd in hetzelfde bereik.

De `Dequeue` bewerking is lastig. HTTP-aanvragen automatisch worden bijgehouden in de Application Insights-SDK. Maar weet niet de correlatie-context totdat het bericht wordt geparseerd. Het is niet mogelijk is de HTTP-aanvraag voor het ophalen van het bericht met de rest van de telemetrie correleren.

In veel gevallen is het mogelijk dat het nuttig om de HTTP-aanvraag naar de wachtrij correleren met andere traceringen ook. Het volgende voorbeeld laat zien hoe om dat te doen:

``` C#
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

#### <a name="process"></a>Proces

In het volgende voorbeeld wordt een binnenkomend bericht op een manier om hoe we een binnenkomende HTTP-aanvraag wilt traceren op dezelfde manier te traceren:

```C#
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

Op dezelfde manier kunnen andere bewerkingen wachtrij worden geïmplementeerd. Een bewerking peek moet geïnstrumenteerd op een vergelijkbare manier als een bewerking van de wachtrij halen. Wachtrij-beheerbewerkingen instrumenteren is niet nodig. Application Insights houdt bewerkingen zoals HTTP en in de meeste gevallen is voldoende.

Wanneer u softwareontwikkelaars verwijdering van berichten, zorg er dan voor dat u de bewerking-id's (correlatie) instellen. U kunt ook de `Activity` API. U hoeft niet vervolgens bewerking-id's op de telemetrie-items niet instellen omdat Application Insights dit gebeurt:

- Maak een nieuwe `Activity` nadat u hebt een item uit de wachtrij.
- Gebruik `Activity.SetParentId(message.ParentId)` als Logboeken correleren.
- Start de `Activity`.
- Bijhouden in wachtrij, verwerken en verwijderen van bewerkingen met behulp van `Start/StopOperation` helpers. Dit doen via de dezelfde asynchrone Controlestroom (uitvoeringscontext). Op deze manier bent ze gecorreleerde goed.
- Stop de `Activity`.
- Gebruik `Start/StopOperation`, of bel `Track` telemetrie handmatig.

### <a name="batch-processing"></a>Batchverwerking
Sommige wachtrijen, kunt u meerdere berichten met één aanvraag in wachtrij. Dergelijke berichten worden verwerkt, is waarschijnlijk onafhankelijk en behoort tot de verschillende logische bewerkingen. In dit geval wordt het is niet mogelijk met elkaar correleren de `Dequeue` bewerking op de verwerking van een bepaald bericht.

Elk bericht moet worden verwerkt in de eigen asynchrone Controlestroom. Zie voor meer informatie de [uitgaande afhankelijkheden bijhouden](#outgoing-dependencies-tracking) sectie.

## <a name="long-running-background-tasks"></a>Achtergrondtaken langlopende
Sommige toepassingen starten langlopende bewerkingen die kunnen worden veroorzaakt door het aanvragen van gebruikers. Vanuit het perspectief tracering/instrumentation verschilt niet van de aanvraag of een afhankelijkheid instrumentation: 

``` C#
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

In dit voorbeeld gebruiken we `telemetryClient.StartOperation` maken `RequestTelemetry` en vult u de context van de correlatie. Stel dat u hebt een bovenliggende-bewerking die is gemaakt door de inkomende aanvragen dat de bewerking gepland. Zolang `BackgroundTask` begint in dezelfde asynchrone transportbesturing als een inkomende aanvraag, deze worden gecorreleerd met die bovenliggende voor deze bewerking. `BackgroundTask`en alle geneste telemetrie-items automatisch worden gecorreleerd met de aanvraag, waardoor er, zelfs nadat de aanvraag wordt beëindigd.

Wanneer de taak wordt gestart vanuit de achtergrondthread die geen bewerkingen (`Activity`) gekoppeld, `BackgroundTask` heeft een bovenliggende geen. Echter, het kan hebben genest bewerkingen. Alle telemetrie-items gerapporteerd door de taak zijn gecorreleerd met de `RequestTelemetry` gemaakt in `BackgroundTask`.

## <a name="outgoing-dependencies-tracking"></a>Uitgaande afhankelijkheden bijhouden
U kunt uw eigen soort afhankelijkheid of een bewerking die niet wordt ondersteund door de Application Insights bijhouden.

De `Enqueue` methode in de Service Bus-wachtrij of de opslagwachtrij kan fungeren als voorbeelden van dergelijke aangepaste bijhouden.

De algemene benadering voor het bijhouden van aangepaste afhankelijkheid is:

- Roep de `TelemetryClient.StartOperation` (extensie)-methode die vult de `DependencyTelemetry` eigenschappen die nodig zijn voor correlatie en andere eigenschappen (begintijd stempel, duur).
- Andere aangepaste eigenschappen worden ingesteld op de `DependencyTelemetry`, zoals de naam en een andere context die u nodig.
- Controleer een afhankelijkheid bellen en een ogenblik geduld.
- Stoppen van de bewerking met `StopOperation` wanneer deze voltooid.
- Uitzonderingen worden verwerkt.

`StopOperation`alleen stopt de bewerking die is gestart. Als de huidige actieve bewerking, komt niet overeen met de versie die u stoppen wilt, `StopOperation` , gebeurt er niets. Deze situatie kan gebeuren als u meerdere bewerkingen parallel in dezelfde uitvoeringscontext starten:

```C#
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// This will do nothing and will not report telemetry for the first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

Zorg ervoor dat u altijd aanroepen `StartOperation` en voert u de taak in een eigen context:
```C#
public async Task RunMyTaskAsync()
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
    try 
    {
        var myTask = await StartMyTaskAsync();
        // Update status code and success as appropriate.
    }
    catch(...) 
    {
        // Update status code and success as appropriate.
    }
    finally 
    {
        telemetryClient.StopOperation(operation);
    }
}
```

## <a name="next-steps"></a>Volgende stappen

- Leer de basisbeginselen van [telemetrie correlatie](application-insights-correlation.md) in Application Insights.
- Zie de [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.
- Aangepast rapport [gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md) naar Application Insights.
- Bekijk standaard [configuratie](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) voor context eigenschappen verzameling.
- Controleer de [System.Diagnostics.Activity gebruikershandleiding](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) om te zien hoe we telemetrie correleren.
