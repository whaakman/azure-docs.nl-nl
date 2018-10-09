---
title: Azure Service Bus-end-to-end tracering en diagnostische gegevens | Microsoft Docs
description: Overzicht van Service Bus-client diagnostische gegevens en end-to-end tracering
services: service-bus-messaging
documentationcenter: ''
author: lmolkova
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: lmolkova
ms.openlocfilehash: 2e4ff84c957540aa6863cd9836b1744e73c5b2f5
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854878"
---
# <a name="distributed-tracing-and-correlation-through-service-bus-messaging"></a>Gedistribueerde tracering en correlatie via Service Bus-berichten

Een van de bekende problemen in de ontwikkeling van microservices is de mogelijkheid voor tracering bewerking van een client via de services die bij de verwerking betrokken zijn. Dit is handig voor foutopsporing, analyse van prestaties, A / B-tests en andere diagnostische gegevens van typische scenario's.
Een deel van dit probleem is het bijhouden van werk logische onderdelen. Het bevat resultaat en latentie en externe afhankelijkheidsaanroepen het verwerken van berichten. Een ander deel is correlatie van deze gebeurtenissen diagnostische gegevens buiten de procesgrenzen van het.

Wanneer een producent een bericht via een wachtrij verzendt, gebeurt het meestal binnen het bereik van een andere logische bewerking, gestart door een andere client- of -service. Dezelfde bewerking wordt voortgezet door consumenten wanneer deze een bericht ontvangt. Zowel producent en consument (en andere services die de bewerking verwerken), telemetriegebeurtenissen wilt traceren van de bewerking stroom en het resultaat waarschijnlijk te verzenden. Om te kunnen correleren van dergelijke gebeurtenissen en tracering bewerking end-to-end, heeft elke service die telemetrie-rapporten voor elke gebeurtenis met een trace-context.

Microsoft Azure Service Bus-berichten, is de nettolading van eigenschappen die producenten en consumenten gebruiken moeten om door te geven van dergelijke trace-context gedefinieerd.
Het protocol is gebaseerd op de [correlatie van HTTP-protocol](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).

| De naam van eigenschap        | Beschrijving                                                 |
|----------------------|-------------------------------------------------------------|
|  Diagnose-Id       | De unieke id van een externe aanroep van producent naar de wachtrij. Raadpleeg [Request-Id in de HTTP-protocol](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#request-id) voor logica, overwegingen en indeling |
|  Correlatie-Context | De bewerkingscontext, die wordt doorgegeven in alle services die betrokken zijn bij de bewerking wordt verwerkt. Zie voor meer informatie, [correlatie-Context in HTTP-protocol](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#correlation-context) |

## <a name="service-bus-net-client-auto-tracing"></a>Service Bus .NET Client auto-tracering

Vanaf versie 3.0.0 [Microsoft Azure Service Bus-Client voor .NET](/dotnet/api/microsoft.azure.servicebus.queueclient) instrumentatiepunten tracering die kunnen worden aangesloten op tracering-systemen of stukje clientcode bevat.
De instrumentatie kunt bijhouden van alle aanroepen naar de Service Bus messaging-service vanaf clientzijde. Als het bericht verwerken is voltooid met de [bericht handler patroon](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler), berichtverwerking ook is geïnstrumenteerd

### <a name="tracking-with-azure-application-insights"></a>Wijzigingen bijhouden met Azure Application Insights

[Microsoft Application Insights](https://azure.microsoft.com/services/application-insights/) biedt uitgebreide mogelijkheden, waaronder automagical aanvraag en bijhouden van afhankelijkheid voor prestatiebewaking.

Afhankelijk van het projecttype van uw, Application Insights SDK te installeren:
- [ASP.NET](../application-insights/app-insights-asp-net.md) -versie 2.5 beta2 installeren of hoger
- [ASP.NET Core](../application-insights/app-insights-asp-net-core.md) -installatie van versie 2.2.0-beta2 of hoger.
Deze koppelingen vindt u informatie over het installeren van de SDK, het maken van resources en SDK configureren (indien nodig). Raadpleeg voor niet-ASP.NET-toepassingen, [Azure Application Insights voor consoletoepassingen](../application-insights/application-insights-console.md) artikel.

Als u [bericht handler patroon](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) voor het verwerken van berichten, bent u klaar: alle Service Bus-aanroepen uitgevoerd door uw service automatisch worden bijgehouden en verband houden met de andere telemetrie-items. Anders raadpleegt u het volgende voorbeeld voor handmatige berichtverwerking bijhouden.

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

In dit voorbeeld `RequestTelemetry` voor elk verwerkte bericht wordt een tijdstempel, duur en het resultaat (geslaagd) gerapporteerd. De telemetrische gegevens is ook een set eigenschappen correlatie.
Geneste traces en uitzonderingen gemeld tijdens de verwerking van berichten zijn ook een factureringslabel voor correlatie-eigenschappen die ze als 'kinderen' van de `RequestTelemetry`.

In het geval u aanroepen van ondersteunde externe onderdelen tijdens de verwerking van berichten aanbrengt, worden ze ook automatisch bijgehouden en gecorreleerde. Raadpleeg [aangepaste bewerkingen met Application Insights .NET-SDK bijhouden](../application-insights/application-insights-custom-operations-tracking.md) voor het handmatig bijhouden en correlatie.

### <a name="tracking-without-tracing-system"></a>Bijhouden zonder traceringssysteem
Als uw traceringssysteem biedt geen ondersteuning voor automatische Service Bus aanroepen bijhouden wordt u op zoek naar dergelijke ondersteuning in een traceringssysteem of in uw toepassing toe te voegen. Deze sectie beschrijft diagnostische gebeurtenissen die door Service Bus .NET-client worden verzonden.  

Service Bus .NET-Client is geïnstrumenteerd met behulp van .NET tracering primitieven [System.Diagnostics.Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) en [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).

`Activity` fungeert als een trace-context tijdens `DiagnosticSource` is een meldingsmechanisme voor. 

Als er geen listener voor de gebeurtenissen DiagnosticSource, heeft instrumentation is uitgeschakeld, de kosten van nul instrumentation. Alle controle geeft DiagnosticSource aan de listener van:
- listener bepaalt welke gegevensbronnen en gebeurtenissen te luisteren naar
- snelheid van besturingselementen listener en steekproeven
- gebeurtenissen worden verzonden met een nettolading die volledige context biedt, zodat u kunt benaderen en berichtobject tijdens de gebeurtenis wijzigen

Maak uzelf vertrouwd met [DiagnosticSource gebruikershandleiding](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) voordat u doorgaat met de implementatie.

We maken een listener voor Service Bus-gebeurtenissen in ASP.NET Core-app die u schrijft de logboeken met Microsoft.Extension.Logger.
Hierbij [System.Reactive.Core](https://www.nuget.org/packages/System.Reactive.Core) bibliotheek moet worden geabonneerd DiagnosticSource (dit is ook eenvoudig te abonneren op DiagnosticSource zonder)

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

Voor elke bewerking, twee gebeurtenissen worden verzonden: 'Start' en 'Stop'. U bent waarschijnlijk, alleen geïnteresseerd in 'Stop'-gebeurtenissen. Ze geven het resultaat van bewerking, evenals begintijd en duur als de activiteitseigenschappen van een.

Nettolading biedt een listener met de context van de bewerking, het binnenkomende API-parameters worden gerepliceerd en waarde retourneren. Nettolading 'Stop' heeft de eigenschappen van nettolading 'Start', zodat u kunt de gebeurtenis 'Start' volledig negeren.

Alle gebeurtenissen hebben ook eigenschappen 'Entiteit' en 'Eindpunt' voorvoegsels worden weggelaten onderstaande tabel
  * `string Entity` --De naam van de entiteit (wachtrij, onderwerp, enz.)
  * `Uri Endpoint` -Service Bus-eindpunt-URL

Elke gebeurtenis 'Stop' heeft een `Status` eigenschap met de `TaskStatus` asynchrone bewerking is voltooid, die ook in de volgende tabel voor het gemak wordt weggelaten.

Dit is de volledige lijst met providers bewerkingen:

| Naam van bewerking | Bijgehouden API | De nettolading van de specifieke eigenschappen|
|----------------|-------------|---------|
| Microsoft.Azure.ServiceBus.Send | [MessageSender.SendAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.sendasync) | IList<Message> berichten - lijst met berichten worden verzonden. |
| Microsoft.Azure.ServiceBus.ScheduleMessage | [MessageSender.ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.schedulemessageasync) | Bericht - bericht dat wordt verwerkt<br/>DateTimeOffset ScheduleEnqueueTimeUtc - verschuiving van de geplande berichten<br/>lange SequenceNumber - volgnummer van gepland bericht (nettolading 'Stop') |
| Microsoft.Azure.ServiceBus.Cancel | [MessageSender.CancelScheduledMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.cancelscheduledmessageasync) | lange SequenceNumber - volgnummer van het bericht te worden geannuleerd | 
| Microsoft.Azure.ServiceBus.Receive | [MessageReceiver.ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) |int RequestedMessageCount - het maximum aantal berichten dat kan worden ontvangen.<br/>IList<Message> berichten - lijst van de ontvangen berichten (nettolading 'Stop') |
| Microsoft.Azure.ServiceBus.Peek | [MessageReceiver.PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync) | int FromSequenceNumber - het beginpunt van waaruit berichten batchgewijs bladeren.<br/>int RequestedMessageCount - het aantal berichten om op te halen.<br/>IList<Message> berichten - lijst van de ontvangen berichten (nettolading 'Stop') |
| Microsoft.Azure.ServiceBus.ReceiveDeferred | [MessageReceiver.ReceiveDeferredMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receivedeferredmessageasync) | IEnumerable<long> SequenceNumbers - de lijst met de volgnummers te ontvangen.<br/>IList<Message> berichten - lijst van de ontvangen berichten (nettolading 'Stop') |
| Microsoft.Azure.ServiceBus.Complete | [MessageReceiver.CompleteAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.completeasync) | IList<string> LockTokens - de lijst met het lock-tokens van de bijbehorende berichten om te voltooien.|
| Microsoft.Azure.ServiceBus.Abandon | [MessageReceiver.AbandonAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.abandonasync) | de tekenreeks LockToken - het lock-token van het bijbehorende bericht te breken. |
| Microsoft.Azure.ServiceBus.Defer | [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) | de tekenreeks LockToken - het lock-token van het bijbehorende bericht om uit te stellen. | 
| Microsoft.Azure.ServiceBus.DeadLetter | [MessageReceiver.DeadLetterAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deadletterasync) | de tekenreeks LockToken - het lock-token van het bijbehorende bericht onbestelbare berichten. | 
| Microsoft.Azure.ServiceBus.RenewLock | [MessageReceiver.RenewLockAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync) | de tekenreeks LockToken - het lock-token van het bijbehorende bericht vergrendeling op vernieuwen.<br/>DateTime LockedUntilUtc - nieuwe vergrendeling vervaldatum van token datum en tijd in UTC-notatie. (Nettolading 'Stop')|
| Microsoft.Azure.ServiceBus.Process | Lambda-functie voor bericht Handler is opgegeven in [IReceiverClient.RegisterMessageHandler](/dotnet/api/microsoft.azure.servicebus.core.ireceiverclient.registermessagehandler) | Bericht - bericht dat wordt verwerkt. |
| Microsoft.Azure.ServiceBus.ProcessSession | Lambda-functie voor bericht-sessie-Handler is opgegeven in [IQueueClient.RegisterSessionHandler](/dotnet/api/microsoft.azure.servicebus.iqueueclient.registersessionhandler) | Bericht - bericht dat wordt verwerkt.<br/>IMessageSession-sessie - sessie wordt verwerkt |
| Microsoft.Azure.ServiceBus.AddRule | [SubscriptionClient.AddRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.addruleasync) | Regel voor RuleDescription - beschrijving van de regel waarmee de regel toe te voegen. |
| Microsoft.Azure.ServiceBus.RemoveRule | [SubscriptionClient.RemoveRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.removeruleasync) | de tekenreeks die RuleName - naam van de regel te verwijderen. |
| Microsoft.Azure.ServiceBus.GetRules | [SubscriptionClient.GetRulesAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.getrulesasync) | IEnumerable<RuleDescription> regels - alle regels die zijn gekoppeld aan het abonnement. (Alleen voor 'Stop' payload) |
| Microsoft.Azure.ServiceBus.AcceptMessageSession | [ISessionClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.azure.servicebus.isessionclient.acceptmessagesessionasync) | sessie-id - sessie-id aanwezig zijn in de berichten de tekenreeks. |
| Microsoft.Azure.ServiceBus.GetSessionState | [IMessageSession.GetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.getstateasync) | sessie-id - sessie-id aanwezig zijn in de berichten de tekenreeks.<br/>byte [] staat - sessiestatus (nettolading 'Stop') |
| Microsoft.Azure.ServiceBus.SetSessionState | [IMessageSession.SetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.setstateasync) | sessie-id - sessie-id aanwezig zijn in de berichten de tekenreeks.<br/>byte [] staat - sessiestatus |
| Microsoft.Azure.ServiceBus.RenewSessionLock | [IMessageSession.RenewSessionLockAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.renewsessionlockasync) | sessie-id - sessie-id aanwezig zijn in de berichten de tekenreeks. |
| Microsoft.Azure.ServiceBus.Exception | een API hebt geïnstrumenteerd| Uitzondering opgetreden uitzondering - uitzondering exemplaar |

U kunt in elke gebeurtenis openen `Activity.Current` dat de huidige bewerkingscontext bevat.

#### <a name="logging-additional-properties"></a>Extra eigenschappen voor logboekregistratie

`Activty.Current` biedt gedetailleerde context van de huidige bewerking en de bijbehorende bovenliggende klassen. Zie voor meer informatie, [activiteit documentatie](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) voor meer informatie.
Service Bus instrumentation bevat aanvullende informatie in de `Activity.Current.Tags` -ze hebben `MessageId` en `SessionId` wanneer deze beschikbaar zijn.

Activiteiten die bijhouden 'Ontvangen', 'Peek' en 'ReceiveDeferred' gebeurtenis ook mogelijk `RelatedTo` tag. Deze unieke lijst bevat `Diagnostic-Id`(s) van berichten die als gevolg hiervan zijn ontvangen.
Deze bewerking kan leiden tot meerdere niet-gerelateerde berichten te ontvangen. Ook de `Diagnostic-Id` is niet bekend bij de bewerking wordt gestart, zodat ' ontvangen ' aan 'Process'-bewerkingen met dit label alleen kunnen worden gecorreleerd. Dit is handig bij het analyseren van prestatieproblemen om te controleren hoe lang het heeft geduurd om het bericht te ontvangen.

Efficiënte manier om aan te melden Tags is om te herhalen, zodat de labels toe te voegen aan het voorgaande voorbeeld ziet eruit als 

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

In sommige gevallen is het wenselijk om aan te melden slechts een deel van de gebeurtenissen worden verkleind performance overhead of opslag. U kunt gebeurtenissen 'Stop' alleen (zoals in het voorgaande voorbeeld) of percentage van de steekproef van de gebeurtenissen kan aanmelden. 
`DiagnosticSource` Geef manier om te realiseren met `IsEnabled` predicaat. Zie voor meer informatie, [filteren op basis van een Context in DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#context-based-filtering).

`IsEnabled` mogelijk worden meerdere keren aangeroepen voor een enkele bewerking om te beperken van invloed op de prestaties.

`IsEnabled` wordt aangeroepen in de volgende volgorde:

1. `IsEnabled(<OperationName>, string entity, null)` bijvoorbeeld, `IsEnabled("Microsoft.Azure.ServiceBus.Send", "MyQueue1")`. Let op: Er is geen 'Start' of 'Stop' aan het einde. Gebruik deze om te filteren om bepaalde bewerkingen of wachtrijen. Als de callback retourneert `false`, gebeurtenissen voor de bewerking niet worden verzonden

  * Voor de 'Process' en 'ProcessSession', ontvangt u ook `IsEnabled(<OperationName>, string entity, Activity activity)` retouraanroep. Gebruiken om te filteren op basis van gebeurtenissen `activity.Id` of Tags eigenschappen.
  
2. `IsEnabled(<OperationName>.Start)` bijvoorbeeld, `IsEnabled("Microsoft.Azure.ServiceBus.Send.Start")`. Hiermee wordt gecontroleerd of de gebeurtenis 'Start' moet worden geactiveerd. Het resultaat heeft alleen gevolgen voor de gebeurtenis 'Start', maar verdere instrumentation is niet afhankelijk van het.

Er is geen `IsEnabled` voor gebeurtenis 'Stop'.

Als sommige bewerkingsresultaat uitzondering, `IsEnabled("Microsoft.Azure.ServiceBus.Exception")` wordt genoemd. U kunt alleen abonneren op gebeurtenissen 'Uitzondering' en te voorkomen dat de rest van de instrumentatie. In dit geval hebt u nog steeds voor het afhandelen van zulke uitzonderingen. Aangezien andere instrumentatie is uitgeschakeld, moet u tracering context stromen met de berichten van consumenten producent niet verwacht.

U kunt `IsEnabled` ook steekproeven strategieën implementeren. Steekproef nemen op basis van de `Activity.Id` of `Activity.RootId` zorgt ervoor dat consistent sampling voor alle te testen (zolang het is doorgegeven door de tracering of door uw eigen code).

In de aanwezigheid van meerdere `DiagnosticSource` listeners voor dezelfde bron, is voldoende voor slechts één listener om te accepteren van de gebeurtenis, dus `IsEnabled` is niet noodzakelijkerwijs worden aangeroepen,

## <a name="next-steps"></a>Volgende stappen

* [Application Insights correlatie](../application-insights/application-insights-correlation.md)
* [Application Insights-afhankelijkheden controleren](../application-insights/app-insights-asp-net-dependencies.md) om te zien als REST, SQL of andere externe resources zorgen voor vertraging.
* [Aangepaste bewerkingen met Application Insights .NET-SDK bijhouden](../application-insights/application-insights-custom-operations-tracking.md)
