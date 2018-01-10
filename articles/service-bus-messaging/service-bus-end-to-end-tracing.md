---
title: Azure Service Bus-end-to-end-tracering en diagnostische gegevens | Microsoft Docs
description: Overzicht van Service Bus-clientdiagnose en end-to-end-tracering
services: service-bus-messaging
documentationcenter: 
author: lmolkova
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova
ms.openlocfilehash: 847056acd2d97391782dcac1874a2739b7f5825c
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="distributed-tracing-and-correlation-through-service-bus-messaging"></a>Gedistribueerde tracering en correlatie via Service Bus-berichtenservice

Een van de algemene problemen bij het microservices ontwikkeling is de mogelijkheid voor trace-bewerking van een client via de services die bij de verwerking betrokken zijn. Dit is handig voor foutopsporing, analyse van prestaties, A / B-tests en andere diagnostische gegevens van typische scenario's.
Een deel van dit probleem is het bijhouden van logische schijven werk. Het bevat bericht verwerking resultaat en latentie en het aanroepen van externe afhankelijkheden. Een ander deel is de correlatie van deze gebeurtenissen diagnostische gegevens buiten de procesgrenzen van het.

Wanneer een producent een bericht via een wachtrij verzendt, gebeurt dit gewoonlijk in het bereik van een andere logische bewerking, geïnitieerd door een andere client of service. Dezelfde bewerking wordt voortgezet consument zodra er een bericht wordt ontvangen. Zowel producenten en consumenten (en andere services die de verwerking van de bewerking) waarschijnlijk telemetrische gebeurtenissen wilt traceren voor de bewerking stroom en het resultaat te verzenden. Om te kunnen correleren dergelijke gebeurtenissen en tracering bewerking end-to-end, heeft elke service die telemetrie rapporteert voor elke gebeurtenis met een trace-context.

Microsoft Azure Service Bus-berichtenservice is nettolading eigenschappen die producenten en consumenten gebruiken moeten voor het doorgeven van dergelijke trace-context gedefinieerd.
Het protocol is gebaseerd op de [correlatie van HTTP-protocol](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).

| De naam van eigenschap        | Beschrijving                                                 |
|----------------------|-------------------------------------------------------------|
|  Diagnose-Id       | De unieke id van een externe aanroep van producent naar de wachtrij. Raadpleeg [Request-Id in de HTTP-protocol](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#request-id) voor de logica, overwegingen en -indeling |
|  Correlatie-Context | De context bewerking, die is doorgegeven via alle services die betrokken zijn bij het verwerken van de bewerking. Zie voor meer informatie [correlatie-Context in HTTP-protocol](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#correlation-context) |

## <a name="service-bus-net-client-auto-tracing"></a>Service Bus .NET Client auto-tracering

Vanaf versie 3.0.0 [Microsoft Azure Service Bus-Client voor .NET](/dotnet/api/microsoft.azure.servicebus.queueclient) tracering instrumentation punten die kunnen worden aangesloten op tracering systemen of stuk clientcode bevat.
De instrumentatie kunt bijhouden van alle aanroepen voor de Service Bus berichtenservice vanaf clientzijde. Als de berichtverwerking is klaar met het [bericht handler patroon](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler), berichtverwerking ook is geïnstrumenteerd

### <a name="tracking-with-azure-application-insights"></a>Bijhouden met Azure Application Insights

[Microsoft Application Insights](https://azure.microsoft.com/services/application-insights/) biedt uitgebreide mogelijkheden zoals automagical aanvraag en het bijhouden van afhankelijkheid voor prestatiebewaking.

Afhankelijk van het projecttype van uw, Application Insights-SDK te installeren:
- [ASP.NET](../application-insights/app-insights-asp-net.md) versie 2.5 beta2 of hoger
- [ASP.NET Core](../application-insights/app-insights-asp-net-core.md) versie 2.2.0-beta2 of hoger.
Deze koppelingen geven informatie over het installeren van SDK, maken van resources en SDK configureren (indien nodig). Raadpleeg voor niet-ASP.NET-toepassingen [Azure Application Insights voor consoletoepassingen](../application-insights/application-insights-console.md) artikel.

Als u [bericht handler patroon](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) voor het verwerken van berichten, u klaar bent: alle Service Bus-aanroepen doen door de service automatisch worden bijgehouden en gecorreleerd met andere telemetrie-items. Anders raadpleegt u het volgende voorbeeld voor handmatige berichtverwerking bijhouden.

#### <a name="trace-message-processing"></a>Verwerking van berichten traceren

```csharp
private readonly TelemetryClient telemetryClient;

async Task ProcessAsync(Message message)
{
    var activity = message.ExtractActivity();
    
    // If you are using Microsoft.ApplicationInsights package version 2.6-beta or higher, you should call StartOperation<RequestTelemetry>(activity) instead
    using (var operation = telemetryClient.StartOperation<RequestTelemetry>("Process", activity.RootId, activity.ParentId))
    {
        telemetryClient.TrackTrace("Received message");
        try 
        {
           // process message
        }
        catch (Exception ex)
        {
             telemetryClient.TrackException(ex);
             operation.Telemetry.Success = false;
             throw;
        }

        telemetryClient.TrackTrace("Done");
   }
}
```

In dit voorbeeld `RequestTelemetry` voor elk verwerkt bericht met een tijdstempel, de duur en het resultaat (geslaagd) wordt gerapporteerd. De telemetrie heeft ook een set eigenschappen van de correlatie.
Geneste traceringen en uitzonderingen gerapporteerd tijdens de berichtverwerking zijn ook een factureringslabel voor correlatie eigenschappen vertegenwoordigen als 'onderliggende' van de `RequestTelemetry`.

Als u aanroepen naar externe componenten ondersteund tijdens de berichtverwerking aanbrengt, maar ze zijn ook automagically bijgehouden en gecorreleerde. Raadpleeg [bijhouden van aangepaste bewerkingen met Application Insights-SDK voor .NET](../application-insights/application-insights-custom-operations-tracking.md) voor handmatige bijhouden en de correlatie.

### <a name="tracking-without-tracing-system"></a>Zonder de tracering bijhouden
Als uw systeem tracering biedt geen ondersteuning voor Service Bus-aanroepen automagical bijhouden wordt u op zoek naar deze ondersteuning in een tracering systeem of in uw toepassing toe te voegen. Deze sectie beschrijft de diagnostische gegevens van gebeurtenissen die worden verzonden door Service Bus .NET-client.  

Service Bus .NET-Client is geïnstrumenteerd met .NET tracering primitieven [System.Diagnostics.Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) en [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).

`Activity`fungeert als een context trace tijdens `DiagnosticSource` is een meldingsmechanisme. 

Als er geen listener voor de gebeurtenissen DiagnosticSource, wordt instrumentation is uitgeschakeld, de nul instrumentation kosten. Alle controle geeft DiagnosticSource aan de listener:
- Hiermee bepaalt u listener die gegevensbronnen en gebeurtenissen te luisteren naar
- snelheid van gebeurtenissen voor listener-besturingselementen en steekproeven
- gebeurtenissen worden verzonden met een nettolading waarmee volledige context, zodat u kunt toegang tot en berichtobject tijdens de gebeurtenis wijzigen

Vertrouwd raken met [DiagnosticSource gebruikershandleiding](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) voordat u doorgaat met de implementatie.

We gaan een listener voor Service Bus-gebeurtenissen in ASP.NET Core-app die u schrijft de logboeken met Microsoft.Extension.Logger maken.
Hierbij [System.Reactive.Core](https://www.nuget.org/packages/System.Reactive.Core) bibliotheek om u te abonneren op DiagnosticSource (het is heel eenvoudig om u te abonneren op DiagnosticSource zonder deze)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory factory, IApplicationLifetime applicationLifetime)
{
    // configuration...

    var serviceBusLogger = factory.CreateLogger("Microsoft.Azure.ServiceBus");

    IDisposable innerSubscription = null;
    IDisposable outerSubscription = DiagnosticListener.AllListeners.Subscribe(delegate (DiagnosticListener listener)
    {
        // subscribe to the Service Bus DiagnosticSource
        if (listener.Name == "Microsoft.Azure.ServiceBus")
        {
            // receive event from Service Bus DiagnosticSource
            innerSubscription = listener.Subscribe(delegate (KeyValuePair<string, object> evnt)
            {
                // Log operation details once it's done
                if (evnt.Key.EndsWith("Stop"))
                {
                    Activity currentActivity = Activity.Current;
                    TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");
                    serviceBusLogger.LogInformation($"Operation {currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}");
                }
            });
        }
    });

    applicationLifetime.ApplicationStopping.Register(() =>
    {
        outerSubscription?.Dispose();
        innerSubscription?.Dispose();
    });
}
```

In dit voorbeeld registreert listener duur, resultaat, unieke id en de begintijd voor elke Service Bus-bewerking.

#### <a name="events"></a>Gebeurtenissen

Voor elke bewerking, twee gebeurtenissen worden verzonden: 'Start' en 'Stop'. Zeer waarschijnlijk bent u alleen geïnteresseerd in 'Stop'-gebeurtenissen. Ze geven het resultaat van bewerking, evenals starttijd en duur als de activiteitseigenschappen van een.

Nettolading biedt een listener met de context van de bewerking, repliceert API binnenkomende parameters en waarde geretourneerd. Nettolading 'Stop' heeft de eigenschappen van nettolading 'Start', zodat u kunt de gebeurtenis 'Start' volledig negeren.

Alle gebeurtenissen ook eigenschappen 'Entiteit' en 'Eindpunt', hebben ze worden weggelaten onderstaande tabel
  * `string Entity`--De naam van de entiteit (wachtrij, onderwerp, enz.)
  * `Uri Endpoint`-Service Bus-eindpunt-URL

Elke gebeurtenis 'Stop' heeft een `Status` eigenschap met de `TaskStatus` asynchrone bewerking is voltooid, die ook in de volgende tabel voor het gemak wordt weggelaten.

Hier volgt een volledige lijst met providers bewerkingen:

| Naam bewerking | Bijgehouden API | De nettolading van de specifieke eigenschappen|
|----------------|-------------|---------|
| Microsoft.Azure.ServiceBus.Send | [MessageSender.SendAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.sendasync) | IList<Message> berichten - lijst met berichten worden verzonden. |
| Microsoft.Azure.ServiceBus.ScheduleMessage | [MessageSender.ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.schedulemessageasync) | Bericht - bericht dat wordt verwerkt<br/>DateTimeOffset ScheduleEnqueueTimeUtc - offset geplande bericht<br/>lange SequenceNumber - volgnummer van geplande bericht ('Stop' nettolading) |
| Microsoft.Azure.ServiceBus.Cancel | [MessageSender.CancelScheduledMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.cancelscheduledmessageasync) | lange SequenceNumber - volgnummer van het bericht te worden geannuleerd | 
| Microsoft.Azure.ServiceBus.Receive | [MessageReceiver.ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) |int RequestedMessageCount - het maximale aantal berichten dat kan worden ontvangen.<br/>IList<Message> berichten - lijst met ontvangen berichten ('Stop' nettolading) |
| Microsoft.Azure.ServiceBus.Peek | [MessageReceiver.PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync) | int FromSequenceNumber - uitgangspunt van waaruit u berichten batchgewijs bladeren.<br/>int RequestedMessageCount - het aantal berichten ophalen.<br/>IList<Message> berichten - lijst met ontvangen berichten ('Stop' nettolading) |
| Microsoft.Azure.ServiceBus.ReceiveDeferred | [MessageReceiver.ReceiveDeferredMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receivedeferredmessageasync) | IEnumerable<long> SequenceNumbers - de lijst met de volgnummers te ontvangen.<br/>IList<Message> berichten - lijst met ontvangen berichten ('Stop' nettolading) |
| Microsoft.Azure.ServiceBus.Complete | [MessageReceiver.CompleteAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.completeasync) | IList<string> LockTokens - de lijst met de lock-tokens van de bijbehorende berichten te voltooien.|
| Microsoft.Azure.ServiceBus.Abandon | [MessageReceiver.AbandonAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.abandonasync) | tekenreeks LockToken - het Vergrendelingstoken van het bijbehorende bericht om af te breken. |
| Microsoft.Azure.ServiceBus.Defer | [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) | tekenreeks LockToken - het Vergrendelingstoken van het bijbehorende bericht om uit te stellen. | 
| Microsoft.Azure.ServiceBus.DeadLetter | [MessageReceiver.DeadLetterAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deadletterasync) | tekenreeks LockToken - het Vergrendelingstoken van het bijbehorende bericht onbestelbare berichten. | 
| Microsoft.Azure.ServiceBus.RenewLock | [MessageReceiver.RenewLockAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync) | tekenreeks LockToken - het Vergrendelingstoken van het bijbehorende bericht verlengd vergrendeling op.<br/>DateTime LockedUntilUtc - nieuwe vergrendeling toegangstoken datum en tijd in UTC-notatie. ('Stop' nettolading)|
| Microsoft.Azure.ServiceBus.Process | Lambda-functie voor bericht Handler is opgegeven in [IReceiverClient.RegisterMessageHandler](/dotnet/api/microsoft.azure.servicebus.core.ireceiverclient.registermessagehandler) | Bericht - bericht dat wordt verwerkt. |
| Microsoft.Azure.ServiceBus.ProcessSession | Lambda-functie voor bericht-sessie-Handler is opgegeven in [IQueueClient.RegisterSessionHandler](/dotnet/api/microsoft.azure.servicebus.iqueueclient.registersessionhandler) | Bericht - bericht dat wordt verwerkt.<br/>IMessageSession-sessie - sessie wordt verwerkt |
| Microsoft.Azure.ServiceBus.AddRule | [SubscriptionClient.AddRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.addruleasync) | Regel voor RuleDescription - beschrijving van de regel waarmee u de regel wilt toevoegen. |
| Microsoft.Azure.ServiceBus.RemoveRule | [SubscriptionClient.RemoveRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.removeruleasync) | tekenreeks RuleName - naam van de regel te verwijderen. |
| Microsoft.Azure.ServiceBus.GetRules | [SubscriptionClient.GetRulesAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.getrulesasync) | IEnumerable<RuleDescription> regels - alle regels die zijn gekoppeld aan het abonnement. (Alleen voor 'Stop' payload) |
| Microsoft.Azure.ServiceBus.AcceptMessageSession | [ISessionClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.azure.servicebus.isessionclient.acceptmessagesessionasync) | tekenreeks SessionId - sessie-id aanwezig is in de berichten. |
| Microsoft.Azure.ServiceBus.GetSessionState | [IMessageSession.GetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.getstateasync) | tekenreeks SessionId - sessie-id aanwezig is in de berichten.<br/>byte [] Status - sessiestatus ('Stop' nettolading) |
| Microsoft.Azure.ServiceBus.SetSessionState | [IMessageSession.SetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.setstateasync) | tekenreeks SessionId - sessie-id aanwezig is in de berichten.<br/>byte [] Status - sessiestatus |
| Microsoft.Azure.ServiceBus.RenewSessionLock | [IMessageSession.RenewSessionLockAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.renewsessionlockasync) | tekenreeks SessionId - sessie-id aanwezig is in de berichten. |
| Microsoft.Azure.ServiceBus.Exception | een API geïnstrumenteerd| Uitzondering uitzondering opgetreden - exemplaar van de uitzondering |

U kunt in elke gebeurtenis openen `Activity.Current` die de huidige bewerking context bevat.

#### <a name="logging-additional-properties"></a>Aanvullende logboekregistratie-eigenschappen

`Activty.Current`biedt gedetailleerde context van de huidige bewerking en de bijbehorende bovenliggende klassen. Zie voor meer informatie [activiteit documentatie](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) voor meer informatie.
Service Bus instrumentation biedt aanvullende informatie in de `Activity.Current.Tags` -ze hebben `MessageId` en `SessionId` wanneer ze beschikbaar zijn.

Activiteiten die bijhouden 'Ontvangen', 'Peek' en 'ReceiveDeferred' gebeurtenis ook wellicht `RelatedTo` label. Deze bevat verschillende lijst met `Diagnostic-Id`(s) van berichten die zijn ontvangen als gevolg hiervan.
Deze bewerking kan ertoe leiden dat er meerdere niet-gerelateerde berichten te ontvangen. Ook de `Diagnostic-Id` is niet bekend als de bewerking wordt gestart, dus 'Ontvangen' operations kunnen worden gecorreleerd met 'Process'-bewerkingen met behulp van deze Tag alleen. Dit is handig bij het analyseren van prestatieproblemen om te controleren hoe lang geduurd om het bericht te ontvangen.

Efficiënte manier aan te melden Tags is doorlopen, dus labels toe te voegen aan het voorgaande voorbeeld ziet als eruit 

```csharp
Activity currentActivity = Activity.Current;
TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");

var tagsList = new StringBuilder();
foreach (var tags in currentActivity.Tags)
{
    tagsList.Append($", "{tags.Key}={tags.Value}");
}

serviceBusLogger.LogInformation($"{currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}{tagsList}");
```

#### <a name="filtering-and-sampling"></a>Filteren en steekproeven

In sommige gevallen is het wenselijk is om aan te melden slechts een deel van de gebeurtenissen voor prestaties overhead of opslag verminderd. U kunt gebeurtenissen 'Stop' alleen (zoals in vorige voorbeeld) of percentage voorbeeld van de gebeurtenissen kan aanmelden. 
`DiagnosticSource`manier om u te bereiken met `IsEnabled` predikaat. Zie voor meer informatie [filteren op basis van een Context in DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#context-based-filtering).

`IsEnabled`kan worden meerdere keren aangeroepen voor een enkele bewerking invloed op de prestaties te minimaliseren.

`IsEnabled`wordt aangeroepen in de volgende volgorde:

1. `IsEnabled(<OperationName>, string entity, null)`bijvoorbeeld: `IsEnabled("Microsoft.Azure.ServiceBus.Send", "MyQueue1")`. Opmerking: Er is geen 'Start' of 'Stop' aan het einde. Gebruik dit voor bepaalde bewerkingen of wachtrijen uitfilteren. Als de callback retourneert `false`, gebeurtenissen voor de bewerking niet worden verzonden

  * Voor de bewerkingen 'Process' en 'ProcessSession', ontvangt u ook `IsEnabled(<OperationName>, string entity, Activity activity)` retouraanroep. Gebruiken om te filteren op basis van gebeurtenissen `activity.Id` of eigenschappen Tags.
  
2. `IsEnabled(<OperationName>.Start)`bijvoorbeeld: `IsEnabled("Microsoft.Azure.ServiceBus.Send.Start")`. Controleert of de gebeurtenis 'Start' moet worden geactiveerd. Het resultaat zijn alleen van invloed op de gebeurtenis 'Start', maar verdere instrumentation is niet afhankelijk van het.

Er is geen `IsEnabled` voor gebeurtenis 'Stop'.

Als sommige bewerkingsresultaat uitzondering, `IsEnabled("Microsoft.Azure.ServiceBus.Exception")` wordt aangeroepen. U kunt alleen abonneren op ' uitzonderingsgebeurtenissen ' en te voorkomen dat de rest van de instrumentatie. In dit geval hebt u nog steeds voor het afhandelen van zulke uitzonderingen. Aangezien andere instrumentation is uitgeschakeld, moet u tracering context stromen met de berichten van consumer producent niet verwacht.

U kunt `IsEnabled` steekproeven strategieën ook worden geïmplementeerd. Steekproef op basis van de `Activity.Id` of `Activity.RootId` zorgt ervoor dat consistent steekproef nemen over alle functie (als deze is doorgegeven door tracering systeem of door uw eigen code).

In de aanwezigheid van meerdere `DiagnosticSource` listeners voor dezelfde bron is voldoende voor slechts één listener te accepteren van de gebeurtenis, dus `IsEnabled` kan niet worden gegarandeerd moet worden aangeroepen

## <a name="next-steps"></a>Volgende stappen

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Application Insights correlatie](../application-insights/application-insights-correlation.md)
* [Application Insights Monitor afhankelijkheden](../application-insights/app-insights-asp-net-dependencies.md) om te zien als REST, SQL of andere externe bronnen u vertragen.
* [Bijhouden van aangepaste bewerkingen met Application Insights-SDK voor .NET](../application-insights/application-insights-custom-operations-tracking.md)
