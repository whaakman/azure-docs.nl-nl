---
title: API's met Azure API Management, Event Hubs en Runscope bewaken | Microsoft Docs
description: Voorbeeld van een toepassing demonstreren van het logboek voor eventhub-beleid door het verbindende Azure API Management, Azure Event Hubs en Runscope voor HTTP-logboekregistratie en controle
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 70ee752c5639c90f77dde104ce85eec0a1062300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Uw API's met Azure API Management, Event Hubs en Runscope bewaken
De [API Management-service](api-management-key-concepts.md) biedt veel mogelijkheden voor het verbeteren van de verwerking van HTTP-aanvragen verzonden naar uw HTTP-API. Het bestaan van de aanvragen en antwoorden zijn echter tijdelijke. De aanvraag wordt gedaan en wordt via de API Management-service naar uw back-end API gebruikt. De aanvraag wordt verwerkt door uw API en een antwoord doorloopt aan de verbruiker API. API Management-service houdt een aantal belangrijke statistieken over de API voor weergave in het dashboard van de uitgever-portal, maar ook buiten dat de gegevens verwijderd zijn.

Met behulp van de [logboek voor eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [beleid](api-management-howto-policies.md) in API Management-service verzendt u details van de aanvraag en de reactie op een [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md). Er zijn diverse redenen waarom u mogelijk gebeurtenissen wilt van HTTP-berichten worden verzonden naar uw API's. Voorbeelden zijn audittrail van updates, gebruiksanalyse, uitzondering waarschuwingen en 3e integraties van derden.   

In dit artikel laat zien hoe het gehele HTTP-aanvraag en -antwoord bericht vastleggen, verzenden naar een Event Hub en vervolgens relay-bericht naar een service waarmee HTTP-logboekregistratie en controle van de services van derden.

## <a name="why-send-from-api-management-service"></a>Waarom verzenden van API Management-Service?
Het is mogelijk HTTP-middleware waarmee HTTP-API-frameworks vastleggen HTTP-aanvragen en antwoorden en ze in logboekregistratie en controle van systemen feed kunt aansluiten schrijven. Het nadeel hiervan is de HTTP-middleware moet worden geïntegreerd in de back-end API en moet overeenkomen met het platform van de API. Als er meerdere API's moet elk criterium de middleware implementeren. Er zijn vaak redenen waarom de back-end voor API's kan niet worden bijgewerkt.

De Azure API Management-service gebruikt om te integreren met infrastructuur voor logboekregistratie biedt een gecentraliseerd en platformonafhankelijk-oplossing. Het is ook schaalbare gedeeltelijk indienen bij de [geo-replicatie](api-management-howto-deploy-multi-region.md) mogelijkheden van Azure API Management.

## <a name="why-send-to-an-azure-event-hub"></a>Waarom verzenden naar een Azure Event Hub?
Het is een redelijk te vragen, waarom maakt u een beleid dat specifiek is voor Azure Event Hubs? Er zijn verschillende plaatsen waar mogelijk wil mijn aanvragen moeten worden geregistreerd. Alleen verzenden waarom niet de aanvragen rechtstreeks naar de uiteindelijke bestemming?  Dit is een optie. Wanneer u logboekregistratie van aanvragen van een API management-service, is het echter nodig zijn om te overwegen hoe berichtregistratie invloed op de prestaties van de API. Geleidelijke toename in belasting kunnen worden verwerkt door het verhogen van de beschikbare exemplaren van de onderdelen van het systeem of door gebruik te maken van geo-replicatie. Korte pieken in het verkeer kunnen echter leiden tot aanvragen als aanvragen voor de infrastructuur voor logboekregistratie van beginnen met het vertragen belast aanzienlijk vertraagd.

De Azure Event Hubs is ontworpen voor grote hoeveelheden gegevens, inkomend met capaciteit voor het omgaan met een veel hoger aantal gebeurtenissen dan het aantal HTTP-verwerken voor de meeste API's aanvragen. De Event Hub fungeert als een soort geavanceerde buffer tussen uw API management-service en de infrastructuur die wordt opgeslagen en de berichten worden verwerkt. Dit zorgt ervoor dat de prestaties van uw API niet als gevolg van de infrastructuur voor logboekregistratie afnemen.  

Nadat de gegevens is doorgegeven naar een Event Hub, deze wordt bewaard en wordt gewacht op Event Hub consumenten verwerken. De Event Hub niets uit hoe deze worden verwerkt, het is net belangrijk ervoor te zorgen dat het bericht met succes worden geleverd.     

Event Hubs kunnen de stroom gebeurtenissen aan meerdere consumergroepen. Hiermee worden gebeurtenissen worden verwerkt door andere systemen. Hierdoor kunnen veel integratiescenario's ondersteunen zonder te plaatsen vertragingen bij het toevoegen van de verwerking van de API-aanvraag in de API Management-service als slechts één gebeurtenis moet worden gegenereerd.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Een beleid voor het verzenden van de toepassing/HTTP-berichten
Een Event Hub accepteert gebeurtenisgegevens worden opgeslagen als een eenvoudige tekenreeks. De inhoud van de reeks zijn volledig bepaalt zelf. Om te kunnen verpakken in een HTTP-aanvraag en dit uit te verzenden naar Event Hubs moet de tekenreeks met de aanvraag of antwoord informatie. In dit soort situaties, als er een bestaande opmaak kan opnieuw worden gebruikt en we kunnen geen ons eigen bij het parseren van code schrijven. In eerste instantie als ik beschouwd met behulp van de [HAR](http://www.softwareishard.com/blog/har-12-spec/) voor het verzenden van HTTP-aanvragen en antwoorden. Deze indeling is echter wel geoptimaliseerd voor het opslaan van een reeks van HTTP-aanvragen in een JSON op basis van indeling. Het bevat een aantal verplichte elementen die onnodige complexiteit voor het scenario van het HTTP-bericht wordt doorgegeven via de kabel toegevoegd.  

Een alternatief is om te gebruiken de `application/http` mediatype zoals beschreven in de HTTP-specificatie [RFC 7230](http://tools.ietf.org/html/rfc7230). Dit mediumtype exact dezelfde indeling die wordt gebruikt voor het daadwerkelijk HTTP-berichten verzenden via de kabel gebruikt, maar het volledige bericht kan worden geplaatst in de hoofdtekst van een andere HTTP-aanvraag. In ons geval gaan we net gebruiken de hoofdtekst als ons bericht te verzenden naar Event Hubs. Gemakkelijk, is er een parser dat voorkomt in [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) bibliotheken die u kunnen deze indeling parseren en te converteren naar het native `HttpRequestMessage` en `HttpResponseMessage` objecten.

Om te kunnen maken van dit bericht moeten we om te profiteren van C# [beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management. Hier wordt het beleid dat u een HTTP-bericht naar Azure Event Hubs verzendt.

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a>De declaratie van het beleid
Er enkele bepaalde zaken opgemerkt over dit beleidsexpressie. Het logboek voor eventhub-beleid heeft een kenmerk met de naam van logboek-id die verwijst naar de naam van het logboek dat is gemaakt in de API Management-service. De details van het instellen van een Event Hub-logboek in de API Management-service vindt u in het document [vastleggen van gebeurtenissen voor Azure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md). Het tweede kenmerk is een optionele parameter zodat Event Hubs die partitie voor het opslaan van het bericht in. Event Hubs partities gebruiken voor schaalbaarheid en ten minste twee nodig hebben. De levering van berichten is alleen binnen een partitie gegarandeerd. Als we niet het Event Hub in welke partitie opdragen doen te plaatsen van het bericht, die een round robin-algoritme wordt gebruikt voor de verdelen. Echter, die mogelijk enkele van onze berichten volgorde worden verwerkt.  

### <a name="partitions"></a>Partities
Ik heb gekozen onze berichten naar gebruikers in volgorde worden geleverd en te profiteren van de load-distributie-functionaliteit van partities te garanderen, HTTP-aanvraagberichten verzenden naar één partitie en HTTP-antwoordberichten naar een tweede partitie. Dit zorgt ervoor dat een gelijkmatige verdeling en wij kunnen garanderen dat alle aanvragen worden gebruikt in volgorde en alle antwoorden in volgorde worden gebruikt. Is het mogelijk een reactie op voordat de bijbehorende aanvraag worden verbruikt, maar omdat die niet is een probleem zoals we hebben een ander mechanisme voor het correleren van aanvragen te antwoorden en we weten dat aanvragen altijd voordat antwoorden afkomstig zijn.

### <a name="http-payloads"></a>HTTP-nettoladingen
Na het gebouw de `requestLine` gecontroleerd om te zien als de hoofdtekst van de aanvraag moet worden afgekapt. De aanvraagtekst is afgekapt tot enige 1024. Dit kan worden verhoogd, maar afzonderlijke Event Hub berichten beperkt tot 256KB zijn, dus is het waarschijnlijk dat bepaalde HTTP-bericht wordt niet in een enkel bericht passen instanties. Bij het uitvoeren van logboekregistratie en analyse kan een aanzienlijke hoeveelheid gegevens worden afgeleid van alleen de regel voor HTTP-aanvraag en -koppen. Ook veel API-aanvragen alleen kleine instanties retourneren en dus het verlies van informatie waarde door af te kappen grote instanties vrij minimale in vergelijking met de beperking van overdracht, verwerking en kosten voor opslag te houden van alle inhoud. Een afsluitende opmerking over het verwerken van de hoofdtekst is dat we moet doorgeven `true` naar de As<string>()-methode omdat we bij het lezen van de inhoud van de instantie, maar is ook wilt dat de back-end API om te kunnen worden gelezen van de hoofdtekst. Door door te geven waar deze methode we ervoor zorgen dat de hoofdtekst van de buffer worden opgeslagen zodat deze een tweede keer kan worden gelezen. Dit is belangrijk te weten als er een API die geen zeer grote bestanden worden geüpload of lange polling gebruikt. In deze gevallen zou worden aanbevolen om te voorkomen dat de hoofdtekst helemaal lezen.   

### <a name="http-headers"></a>HTTP-headers
HTTP-Headers kunnen eenvoudig worden overgedragen via in de berichtindeling in een eenvoudige sleutel/waarde-paar-indeling. We hebben ervoor gekozen het verwijderen van bepaalde beveiliging gevoelige velden, om te voorkomen dat onnodig lekken referentie-informatie. Het lijkt onwaarschijnlijk dat API-sleutels en andere referenties zou worden gebruikt voor andere doeleinden analytics. Als we willen analyse van de gebruiker en het product ze gebruiken en we kan ophalen die van de `context` object en voeg die toe aan het bericht.     

### <a name="message-metadata"></a>De metagegevens van het bericht
Wanneer u het volledige bericht te verzenden naar de event hub, de eerste regel is niet daadwerkelijk deel uit van de `application/http` bericht. De eerste regel zijn aanvullende metagegevens die bestaan uit of het bericht is een aanvraag of antwoordbericht en een bericht-id die wordt gebruikt voor aanvragen voor reacties correleren. De bericht-id is gemaakt met behulp van een ander beleid dat op dit lijkt:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Er kan het aanvraagbericht gemaakt, die in een variabele opgeslagen totdat het antwoord is geretourneerd en gewoon verzend de aanvraag en -antwoord als één bericht. Echter, krijgen we verzenden onafhankelijk van de aanvraag en -antwoord en een bericht-id correlaties zichtbaar maken tussen de twee, iets meer flexibiliteit in de berichtgrootte, de mogelijkheid om te profiteren van meerdere partities terwijl het onderhouden van de volgorde van berichten en het verzoek wordt weergegeven in onze logboekregistratie dashboard sneller. Ook kan er een aantal scenario's waarbij een geldig antwoord wordt nooit verzonden naar de event hub, mogelijk vanwege een onherstelbare aanvraag-fout in de API Management-service, maar er wordt nog een record van de aanvraag.

Het beleid voor het verzenden van het HTTP-antwoordbericht lijkt erg op de aanvraag en dus de volledige beleidsconfiguratie ziet er ongeveer als volgt:

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

De `set-variable` beleid maakt een waarde die toegankelijk is voor zowel de `log-to-eventhub` beleid in de `<inbound>` sectie en de `<outbound>` sectie.  

## <a name="receiving-events-from-event-hubs"></a>Ontvangen van gebeurtenissen van Event Hubs
Gebeurtenissen van Azure Event Hub worden ontvangen met behulp van de [AMQP-protocol](http://www.amqp.org/). Het Microsoft Service Bus-team aangebracht client bibliotheken beschikbaar om de verbruikende gebeurtenissen te vereenvoudigen. Er zijn twee verschillende benaderingen ondersteund, een wordt een *directe Consumer* en de andere maakt gebruik van de `EventProcessorHost` klasse. Voorbeelden van deze twee methoden kunnen u vinden in de [Event Hubs-programmeergids](../event-hubs/event-hubs-programming-guide.md). Er is de beknopte versie van de verschillen, `Direct Consumer` hebt u volledige controle en de `EventProcessorHost` biedt u enkele van de Loodgieterswerk voor u maar kunt u bepaalde veronderstellingen over hoe u deze gebeurtenissen worden verwerkt.  

### <a name="eventprocessorhost"></a>EventProcessorHost
In dit voorbeeld gebruiken we de `EventProcessorHost` voor eenvoud, maar het kan niet de beste keuze voor dit scenario. `EventProcessorHost`de vaste werkt waarbij wordt gecontroleerd of er problemen binnen een bepaalde gebeurtenis Processorklasse threading bang geen. In ons scenario zijn we echter gewoon het bericht converteren naar een andere indeling en deze samen met een andere service door middel van een async-methode wordt doorgegeven. Er is niet nodig voor het bijwerken van de gedeelde status en daarom geen risico bestaat dat threading problemen. Voor de meeste scenario `EventProcessorHost` is waarschijnlijk de beste keuze en het is gewoon de gemakkelijker optie.     

### <a name="ieventprocessor"></a>IEventProcessor
Het centrale concept bij gebruik van `EventProcessorHost` is het maken van een implementatie van de `IEventProcessor` interface de methode bevat `ProcessEventAsync`. De essentie van deze methode wordt hier weergegeven:

```c#
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

   foreach (EventData eventData in messages)
   {
       _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

       try
       {
           var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
           await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
       }
       catch (Exception ex)
       {
           _Logger.LogError(ex.Message);
       }
   }
    ... checkpointing code snipped ...
}
```

Een lijst met EventData objecten worden doorgegeven aan de methode en we doorlopen die lijst. Het aantal bytes van elke methode worden geparseerd naar een object HttpMessage en dat object wordt doorgegeven aan een exemplaar van IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
De `HttpMessage` -exemplaar bevat drie soorten gegevens:

```c#
public class HttpMessage
{
   public Guid MessageId { get; set; }
   public bool IsRequest { get; set; }
   public HttpRequestMessage HttpRequestMessage { get; set; }
   public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

De `HttpMessage` -exemplaar bevat een `MessageId` GUID die kan worden uitgevoerd op de verbinding met de HTTP-aanvragen in het bijbehorende HTTP-antwoord en een Boolean die aangeeft of het object een exemplaar van een HttpRequestMessage en HttpResponseMessage bevat. Met behulp van de ingebouwde in HTTP-klassen uit `System.Net.Http`, kon ik om te profiteren van de `application/http` bij het parseren van code die is opgenomen in `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
De `HttpMessage` exemplaar wordt vervolgens doorgestuurd naar de implementatie van `IHttpMessageProcessor` die ik gemaakt voor het loskoppelen van de ontvangen en de interpretatie van de gebeurtenis van Azure Event Hub en de daadwerkelijke verwerking van deze interface is.

## <a name="forwarding-the-http-message"></a>Het HTTP-bericht doorsturen
Voor dit voorbeeld ik besloten deze zou van belang zijn voor de HTTP-aanvraag via push naar [Runscope](http://www.runscope.com). Runscope is een service in de cloud die is gespecialiseerd in HTTP-foutopsporing, logboekregistratie en controle. Ze hebben een gratis laag, zodat het gemakkelijk om te proberen en kunt u ons om te zien van de HTTP-aanvragen in realtime stromend door onze API Management-service.

De `IHttpMessageProcessor` implementatie uitziet,

```c#
public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
{
   private HttpClient _HttpClient;
   private ILogger _Logger;
   private string _BucketKey;
   public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
   {
       _HttpClient = httpClient;
       var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
       _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
       _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
       _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
       _Logger = logger;
   }

   public async Task ProcessHttpMessage(HttpMessage message)
   {
       var runscopeMessage = new RunscopeMessage()
       {
           UniqueIdentifier = message.MessageId
       };

       if (message.IsRequest)
       {
           _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
           runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
       }
       else
       {
           _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
           runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
       }

       var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
       messagesLink.BucketKey = _BucketKey;
       messagesLink.RunscopeMessage = runscopeMessage;
       var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
       _Logger.LogDebug("Request sent to Runscope");
   }
}
```

Ik heb om te profiteren van een [bestaande-clientbibliotheek voor Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) waarmee u gemakkelijk te pushen `HttpRequestMessage` en `HttpResponseMessage` up bij hun service-exemplaren. Om te kunnen krijgen tot de API Runscope moet u een account en een API-sleutel. Instructies voor het ophalen van een API-sleutel kunnen worden gevonden in de [toepassingen maken voor toegang tot Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) screencast.

## <a name="complete-sample"></a>Compleet codevoorbeeld
De [broncode](https://github.com/darrelmiller/ApimEventProcessor) en tests voor het voorbeeld op GitHub. U moet een [API Management-Service](api-management-get-started.md), [een verbonden Event Hub](api-management-howto-log-event-hubs.md), en een [Opslagaccount](../storage/common/storage-create-storage-account.md) om uit te voeren van het voorbeeld voor uzelf.   

Het voorbeeld is alleen een eenvoudige consoletoepassing die luistert naar gebeurtenissen die afkomstig zijn van de Event Hub, converteert u deze in een `HttpRequestMessage` en `HttpResponseMessage` objecten en stuurt deze door u aan bij de Runscope-API.

In de volgende animatie afbeelding ziet u een aanvraag wordt aan een API in de Portal voor ontwikkelaars de consoletoepassing met het bericht wordt ontvangen, verwerkt en doorgestuurd en vervolgens de aanvraag en antwoord weergegeven in de inspector Runscope verkeer.

![Demonstratie van de aanvraag doorgestuurd naar Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Samenvatting
Azure API Management-service biedt een ideaal plaats voor het vastleggen van de HTTP-verkeer op reis naar en van uw API's. Azure Event Hubs is een zeer schaalbaar, voordelige oplossing voor het vastleggen van dat verkeer en die deze naar secundaire verwerkingssystemen voor logboekregistratie, bewaken en andere geavanceerde analyses. Verbinding maken met 3e partij verkeer bewaken Runscope is een eenvoudige als een paar dozijn regels code.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Azure Event Hubs
  * [Aan de slag met Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Berichten ontvangen met EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Programmeerhandleiding voor Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Meer informatie over de integratie van API Management en Event Hubs
  * [Het registreren van gebeurtenissen voor Azure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md)
  * [Entiteitsverwijzing berichtenlogboek](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [documentatie voor logboek-eventhub-beleid](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
