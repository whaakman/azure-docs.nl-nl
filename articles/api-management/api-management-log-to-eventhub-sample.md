---
title: API's met Azure API Management, Eventhubs en Moesif bewaken | Microsoft Docs
description: Voorbeeld van een toepassing demonstreren van het logboek naar Event hub-beleid door verbindende Azure API Management, Azure Event Hubs en Moesif voor HTTP-logboekregistratie en bewaking
services: api-management
documentationcenter: ''
author: darrelmiller
manager: erikre
editor: ''
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2018
ms.author: apimpm
ms.openlocfilehash: cdaaf5323543377d9c2b603ad7377d088710cde8
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447740"
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-moesif"></a>Uw API's met Azure API Management, Eventhubs en Moesif controleren
De [API Management-service](api-management-key-concepts.md) biedt veel mogelijkheden voor het verbeteren van de verwerking van HTTP-aanvragen die worden verzonden op uw HTTP-API. Het bestaan van de aanvragen en antwoorden is echter tijdelijk. De aanvraag wordt gedaan en hiervandaan stroomt het via de API Management-service naar uw back-end-API. Uw API verwerkt de aanvraag en een antwoord doorloopt de consument API. De API Management-service houdt u enkele belangrijke statistische gegevens over de API's om weer te geven in de Azure portal-dashboard, maar ook buiten dat de gegevens verwijderd zijn.

Met behulp van het logboek naar Event hub-beleid in de API Management-service, kunt u alle details verzenden vanuit de aanvraag en reactie op een [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md). Er zijn diverse redenen waarom u mogelijk wilt genereren van gebeurtenissen uit de HTTP-berichten worden verzonden naar uw API's. Enkele voorbeelden zijn audittrail van updates, gebruiksanalyse, uitzondering waarschuwingen en integratie van derden.

In dit artikel laat zien hoe u het hele HTTP-aanvraag en respons bericht vastleggen, verzenden naar een Event Hub en vervolgens doorsturen dat bericht met een service van derden waarmee HTTP-logboekregistratie en bewaking van services.

## <a name="why-send-from-api-management-service"></a>Waarom verzenden van API Management-Service?
Het is mogelijk het schrijven van HTTP-middleware die HTTP-API-frameworks voor HTTP-aanvragen en antwoorden vastleggen en ze in logboekregistratie en bewaking van systemen kan aansluiten. Het nadeel deze benadering is de HTTP-middleware moet worden geïntegreerd in de back-end-API en moet overeenkomen met het platform van de API. Als er meerdere API's, moet elk criterium de middleware implementeren. Er zijn vaak redenen waarom de back-end-API's kan niet worden bijgewerkt.

Met behulp van de Azure API Management-service voor integratie met infrastructuur voor logboekregistratie biedt een gecentraliseerd en platformonafhankelijke-oplossing. Het is ook schaalbaar zijn, gedeeltelijk indienen bij de [geo-replicatie](api-management-howto-deploy-multi-region.md) mogelijkheden van Azure API Management.

## <a name="why-send-to-an-azure-event-hub"></a>Waarom verzenden naar een Azure Event Hub?
Het is een redelijk te vragen, waarom maakt u een beleid dat specifiek is voor Azure Event Hubs? Er zijn veel verschillende plaatsen waar ik wil mijn aanvragen vastleggen. Alleen verzenden waarom niet de aanvragen rechtstreeks naar de eindbestemming?  Dat kan worden gebruikt. Wanneer u logboekregistratie van aanvragen van een API management-service, is het echter nodig zijn om te overwegen hoe berichten van de logboekregistratie van invloed zijn op de prestaties van de API. Geleidelijke verhogingen in de belasting kunnen worden verwerkt door het verhogen van de beschikbare exemplaren van de onderdelen van het systeem of door te profiteren van geo-replicatie. Korte pieken in verkeer kunnen echter leiden tot aanvragen als aanvragen voor infrastructuur voor logboekregistratie wordt gestart tot trage onder belasting worden vertraagd.

De Azure Event Hubs is ontworpen voor inkomend verkeer zeer grote hoeveelheden gegevens, met een capaciteit voor het omgaan met een veel groter aantal gebeurtenissen dan het aantal HTTP-verwerken voor de meeste API's aanvragen. De Event Hub fungeert als een soort geavanceerde buffer tussen uw API management-service en de infrastructuur die worden opgeslagen en berichten worden verwerkt. Dit zorgt ervoor dat de prestaties van uw API niet vanwege de infrastructuur voor logboekregistratie afnemen.

Zodra de gegevens naar een Event Hub is verstreken, worden persistent gemaakt en wordt gewacht op Event Hub-consumenten te verwerken. De Event Hub maakt niet uit hoe deze wordt verwerkt, het zojuist hecht te zorgen dat het bericht met succes worden geleverd.

Eventhubs heeft de mogelijkheid om gebeurtenissen streamen naar meerdere consumentengroepen. Hierdoor kunnen worden verwerkt door verschillende systemen. Hierdoor kan de ondersteuning van veel scenario's voor gegevensintegratie zonder toevoeging vertragingen in de verwerking van de API-aanvraag in de API Management-service plaatsen als er slechts één gebeurtenis moet worden gegenereerd.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Een beleid voor het verzenden van de toepassing/http-berichten
Een Event Hub accepteert gebeurtenisgegevens als een eenvoudige tekenreeks. De inhoud van die tekenreeks wordt aan u. Als u een HTTP-aanvraag verpakken en dit uit te verzenden naar Event Hubs, moeten we de tekenreeks met de aanvraag of antwoord informatie. In situaties als volgt, als er een bestaande indeling kan opnieuw worden gebruikt en we kunnen geen onze eigen parseren code schrijven. In eerste instantie als ik beschouwd met behulp van de [HAR](http://www.softwareishard.com/blog/har-12-spec/) voor het verzenden van HTTP-aanvragen en antwoorden. Deze indeling is echter wel geoptimaliseerd voor het opslaan van een reeks HTTP-aanvragen in een JSON-indeling. Het bevat een aantal verplichte elementen die onnodige complexiteit voor het scenario van het HTTP-bericht doorgegeven via de kabel toegevoegd.

Een alternatief is om te gebruiken de `application/http` mediatype zoals beschreven in de HTTP-specificatie [RFC 7230](https://tools.ietf.org/html/rfc7230). Dit mediatype exact dezelfde indeling die wordt gebruikt voor het verzenden van daadwerkelijk HTTP-berichten via de kabel gebruikt, maar het hele bericht te plaatsen in de hoofdtekst van een andere HTTP-aanvraag. In ons geval gaan alleen we de hoofdtekst als onze bericht gebruiken om te verzenden naar Event Hubs. Eenvoudig, is er een parser die deel uitmaakt van [2.2-Client voor Microsoft ASP.NET Web API](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) bibliotheken die u kunnen deze indeling parseren en te converteren naar de native `HttpRequestMessage` en `HttpResponseMessage` objecten.

Als u dit bericht maken, moeten we om te profiteren van C# [beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management. Hier is het beleid, die vervolgens een HTTP-aanvraagbericht naar Azure Event Hubs doorsturen.

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

### <a name="policy-declaration"></a>De declaratie van beleid
Er een aantal bepaalde dingen opgemerkt over dit beleidsexpressie. Het logboek naar Event hub-beleid heeft een kenmerk met de naam van de logger-id, die verwijst naar de naam van de logger die is gemaakt in de API Management-service. De details van het instellen van een Event Hub-logboek in de API Management-service kunnen u vinden in het document [hoe u gebeurtenissen registreren bij Azure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md). Het tweede kenmerk is een optionele parameter waarmee u Event Hubs dat partitie voor het opslaan van het bericht in. Eventhubs maakt gebruik van partities schaalbaarheid inschakelen en moet ten minste twee. De geordende bezorging van berichten kan alleen worden gegarandeerd binnen een partitie. Als we de opdracht Event Hub in welke partitie te plaatsen van het bericht een round robin-algoritme wordt gebruikt voor het distribueren van de belasting niet geven. Echter, die mogelijk enkele van onze berichten niet de juiste volgorde worden verwerkt.

### <a name="partitions"></a>Partities
Ik heb geselecteerd om te zorgen dat onze berichten voor consumenten in volgorde worden geleverd en te profiteren van de mogelijkheid van de distributie load van partities, HTTP-aanvraagberichten te verzenden naar één partitie en HTTP-antwoordberichten naar een tweede partitie. Dit zorgt ervoor dat de distributie van een evenredig worden belast en we kunnen garanderen dat alle aanvragen worden gebruikt in volgorde en alle antwoorden in volgorde worden verbruikt. Het is mogelijk een reactie op de bijbehorende aanvraag worden verbruikt, maar is dat geen probleem, zoals we hebben een ander mechanisme voor het correleren van aanvragen voor antwoorden en we weten dat aanvragen altijd eerder zijn dan de antwoorden.

### <a name="http-payloads"></a>HTTP-nettoladingen
Na het gebouw de `requestLine`, gecontroleerd om te zien als hoofdtekst van de aanvraag moet worden afgekapt. De aanvraagtekst is afgekapt tot enige 1024. Dit kan worden verhoogd, maar afzonderlijke Event Hub-berichten beperkt tot 256 KB zijn, dus is het waarschijnlijk dat bepaalde HTTP-bericht wordt niet in een enkel bericht passen instanties. Bij het uitvoeren van logboekregistratie en analyse kan een aanzienlijke hoeveelheid gegevens kan worden afgeleid van alleen de HTTP-aanvraagregel en -koppen. Ook allerlei API's alleen geretourneerde kleine instanties aanvragen en het verlies van gegevens waarde door af te breken grote organisaties is dus redelijk minimale in vergelijking met de vermindering van de overdracht, verwerking en kosten voor opslag te houden van alle inhoud. Een definitieve opmerking over het verwerken van de hoofdtekst is dat we nodig om door te geven `true` naar de `As<string>()` methode omdat we bij het lezen van de inhoud, maar is ook wilde de back-end-API om te kunnen lezen van de hoofdtekst. Door te geven waar deze methode we ervoor zorgen dat de instantie worden gebufferd zodat het een tweede keer kan worden gelezen. Dit is belangrijk rekening mee moet houden als u een API die geen grote bestanden worden geüpload of long polling gebruikt. In dergelijke gevallen zou het beste om te voorkomen dat de instantie op alle lezen zijn.

### <a name="http-headers"></a>HTTP-headers
HTTP-Headers kunnen worden overgedragen naar de berichtindeling in een paar eenvoudige sleutel/waarde-indeling. Het verwijderen van bepaalde gevoelige velden van beveiliging, om te voorkomen dat onnodig lekken referentie-informatie hebben we gekozen. Is het onwaarschijnlijk dat API-sleutels en andere referenties zouden worden gebruikt voor analyse. Als we willen analyse uitvoeren op de gebruiker en het product ze gebruiken, dan we vanuit krijgen kan de `context` object en voeg dat toe aan het bericht.

### <a name="message-metadata"></a>De metagegevens van het bericht
Bij het bouwen van de volledige dat moet worden verzonden naar de event hub, de eerste regel is niet daadwerkelijk deel uitmaakt van de `application/http` bericht. De eerste regel is aanvullende metagegevens die bestaan uit of het bericht is een aanvraag of antwoordbericht en een bericht-ID, die wordt gebruikt voor het correleren van naar antwoorden aanvragen. De bericht-ID wordt gemaakt met behulp van een ander beleid er als volgt uitzien:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

We kunnen hebben gemaakt van het aanvraagbericht, die in een variabele opgeslagen totdat de reactie is geretourneerd en verzend de aanvraag en respons als één bericht. Echter, krijgen we door het verzenden van de aanvraag en respons onafhankelijk van elkaar en met behulp van een bericht-id correlaties zichtbaar maken tussen de twee, iets meer flexibiliteit in de grootte van het bericht, de mogelijkheid om te profiteren van meerdere partities tijdens onderhoud van de volgorde van berichten en de aanvraag wordt weergegeven in onze logboekregistratie dashboard eerder. Ook mogelijk zijn er enkele scenario's waarin een geldige reactie wordt nooit verzonden naar de event hub, mogelijk vanwege een fatale fout in de API Management-service, maar hebben we nog een record van de aanvraag.

Het beleid voor het verzenden van het HTTP-antwoordbericht lijkt op de aanvraag en dus de configuratie van een volledige er als volgt uitzien:

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
Gebeurtenissen van Azure Event Hub worden ontvangen met behulp van de [AMQP-protocol](https://www.amqp.org/). De Service Bus van Microsoft-team aangebracht client bibliotheken die beschikbaar zijn voor het vereenvoudigen van de gebeurtenissen in beslag nemen. Er zijn twee verschillende manieren worden ondersteund, een wordt een *directe Consumer* en de andere is met behulp van de `EventProcessorHost` klasse. Voorbeelden van deze twee methoden kunnen u vinden in de [Event Hubs-programmeergids](../event-hubs/event-hubs-programming-guide.md). De verkorte versie van de verschillen is, `Direct Consumer` hebt u volledige controle en de `EventProcessorHost` biedt enkele van de Loodgieterswerk voor u, maar kunt u bepaalde veronderstellingen over hoe u deze gebeurtenissen worden verwerkt.

### <a name="eventprocessorhost"></a>EventProcessorHost
In dit voorbeeld gebruiken we de `EventProcessorHost` voor het gemak, maar wordt deze wellicht niet de beste keuze voor dit specifieke scenario. `EventProcessorHost` komt het harde werk van het te zorgen dat u hoeft te bekommeren threading problemen binnen een bepaalde gebeurtenis processor-klasse. In ons scenario, zijn we echter gewoon converteren van het bericht naar een andere indeling en deze samen met een andere service met behulp van een asynchrone methode door te geven. Er is niet nodig voor het bijwerken van gedeelde status en daarom geen risico threading problemen. Voor de meeste scenario's, `EventProcessorHost` is waarschijnlijk de beste keuze en het is zeker de gemakkelijker optie.

### <a name="ieventprocessor"></a>IEventProcessor
Het centrale concept bij het gebruik van `EventProcessorHost` is het maken van een implementatie van de `IEventProcessor` interface, welke de methode bevat `ProcessEventAsync`. De essentie van deze methode wordt hier weergegeven:

```csharp
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

Een lijst met objecten EventData worden doorgegeven aan de methode en we herhalen die lijst. Het aantal bytes van elke methode worden geparseerd in een object HttpMessage en dat object wordt doorgegeven aan een exemplaar van IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
De `HttpMessage` -exemplaar bevat drie soorten gegevens:

```csharp
public class HttpMessage
{
    public Guid MessageId { get; set; }
    public bool IsRequest { get; set; }
    public HttpRequestMessage HttpRequestMessage { get; set; }
    public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

De `HttpMessage` -exemplaar bevat een `MessageId` GUID waarmee we verbinding maken van de HTTP-aanvraag met de bijbehorende HTTP-antwoord en een Booleaanse waarde die aangeeft of het object een exemplaar van een het HttpRequestMessage en HttpResponseMessage bevat. Met behulp van de ingebouwde HTTP-klassen vanuit `System.Net.Http`, was ik kunnen profiteren van de `application/http` parseren van code die is opgenomen in `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
De `HttpMessage` exemplaar wordt vervolgens doorgestuurd naar de implementatie van `IHttpMessageProcessor`, die een interface die ik heb gemaakt als u wilt loskoppelen van de ontvangen en de interpretatie van de gebeurtenis van Azure Event Hub en de daadwerkelijke verwerking van is.

## <a name="forwarding-the-http-message"></a>Doorsturen van de HTTP-bericht
Voor dit voorbeeld, besloot het normaal zou zijn om de HTTP-aanvraag via het uiterste te interessante [Moesif API Analytics](https://www.moesif.com). Moesif is een cloudgebaseerde service die is gespecialiseerd in HTTP-analyse en foutopsporing. Ze hebben een gratis laag, dus het is gemakkelijk om te proberen en deze maakt het mogelijk om te zien van de HTTP-aanvragen in realtime-stromen via onze API Management-service.

De `IHttpMessageProcessor` implementatie er als volgt uitzien,

```csharp
public class MoesifHttpMessageProcessor : IHttpMessageProcessor
{
    private readonly string RequestTimeName = "MoRequestTime";
    private MoesifApiClient _MoesifClient;
    private ILogger _Logger;
    private string _SessionTokenKey;
    private string _ApiVersion;
    public MoesifHttpMessageProcessor(ILogger logger)
    {
        var appId = Environment.GetEnvironmentVariable("APIMEVENTS-MOESIF-APP-ID", EnvironmentVariableTarget.Process);
        _MoesifClient = new MoesifApiClient(appId);
        _SessionTokenKey = Environment.GetEnvironmentVariable("APIMEVENTS-MOESIF-SESSION-TOKEN", EnvironmentVariableTarget.Process);
        _ApiVersion = Environment.GetEnvironmentVariable("APIMEVENTS-MOESIF-API-VERSION", EnvironmentVariableTarget.Process);
        _Logger = logger;
    }

    public async Task ProcessHttpMessage(HttpMessage message)
    {
        if (message.IsRequest)
        {
            message.HttpRequestMessage.Properties.Add(RequestTimeName, DateTime.UtcNow);
            return;
        }

        EventRequestModel moesifRequest = new EventRequestModel()
        {
            Time = (DateTime) message.HttpRequestMessage.Properties[RequestTimeName],
            Uri = message.HttpRequestMessage.RequestUri.OriginalString,
            Verb = message.HttpRequestMessage.Method.ToString(),
            Headers = ToHeaders(message.HttpRequestMessage.Headers),
            ApiVersion = _ApiVersion,
            IpAddress = null,
            Body = message.HttpRequestMessage.Content != null ? System.Convert.ToBase64String(await message.HttpRequestMessage.Content.ReadAsByteArrayAsync()) : null,
            TransferEncoding = "base64"
        };

        EventResponseModel moesifResponse = new EventResponseModel()
        {
            Time = DateTime.UtcNow,
            Status = (int) message.HttpResponseMessage.StatusCode,
            IpAddress = Environment.MachineName,
            Headers = ToHeaders(message.HttpResponseMessage.Headers),
            Body = message.HttpResponseMessage.Content != null ? System.Convert.ToBase64String(await message.HttpResponseMessage.Content.ReadAsByteArrayAsync()) : null,
            TransferEncoding = "base64"
        };

        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApimMessageId", message.MessageId.ToString());

        EventModel moesifEvent = new EventModel()
        {
            Request = moesifRequest,
            Response = moesifResponse,
            SessionToken = _SessionTokenKey != null ? message.HttpRequestMessage.Headers.GetValues(_SessionTokenKey).FirstOrDefault() : null,
            Tags = null,
            UserId = null,
            Metadata = metadata
        };

        Dictionary<string, string> response = await _MoesifClient.Api.CreateEventAsync(moesifEvent);

        _Logger.LogDebug("Message forwarded to Moesif");
    }

    private static Dictionary<string, string> ToHeaders(HttpHeaders headers)
    {
        IEnumerable<KeyValuePair<string, IEnumerable<string>>> enumerable = headers.GetEnumerator().ToEnumerable();
        return enumerable.ToDictionary(p => p.Key, p => p.Value.GetEnumerator()
                                                         .ToEnumerable()
                                                         .ToList()
                                                         .Aggregate((i, j) => i + ", " + j));
    }
}
```

De `MoesifHttpMessageProcessor` maakt gebruik van een [ C# API-bibliotheek voor Moesif](https://www.moesif.com/docs/api?csharp#events) die kunt u eenvoudig pushmeldingen HTTP-gebeurtenisgegevens naar hun service. Als u wilt gegevens van HTTP naar de Moesif gegevensverzamelaar-API verzendt, moet u een account en een toepassings-id. U een Moesif toepassings-Id ophalen met het maken van een account op [Moesif van website](https://www.moesif.com) en ga vervolgens naar de _Menu rechts van het hoogste_ -> _App-instelling_.

## <a name="complete-sample"></a>Compleet voorbeeld
De [broncode](https://github.com/dgilling/ApimEventProcessor) en tests voor het voorbeeld op GitHub. U moet een [API Management-Service](get-started-create-service-instance.md), [een verbonden Event Hub](api-management-howto-log-event-hubs.md), en een [Opslagaccount](../storage/common/storage-create-storage-account.md) om uit te voeren van het voorbeeld voor uzelf.   

Het voorbeeld is alleen een eenvoudige consoletoepassing die luistert naar gebeurtenissen die afkomstig zijn van de Event Hub, omgezet in een Moesif `EventRequestModel` en `EventResponseModel` objecten en stuurt ze door u aan bij de Moesif gegevensverzamelaar-API.

In de volgende bewegende afbeelding ziet u een aanvraag aan een API in de Portal voor ontwikkelaars, de Console-toepassing met het bericht wordt ontvangen, verwerkt en doorgestuurd en vervolgens de aanvraag en antwoord weergegeven in de gebeurtenis Stream.

![Demonstratie van de aanvraag doorgestuurd naar de Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Samenvatting
Azure API Management-service biedt een ideale locatie voor het vastleggen van de HTTP-verkeer naar en van uw API's onderweg. Azure Event Hubs is een zeer schaalbare, goedkope oplossing voor het vastleggen van dat verkeer en deze in de van secundaire verwerkingssystemen voor logboekregistratie, bewaking en andere geavanceerde analyses uit. Verbinding maken met externe verkeer bewakingssystemen Moesif is net zo eenvoudig als enkele tientallen regels code.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Azure Event Hubs
  * [Aan de slag met Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Berichten ontvangen met EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Programmeerhandleiding voor Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Meer informatie over de integratie van API Management en Event Hubs
  * [Hoe u gebeurtenissen registreren bij Azure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md)
  * [Verwijzing naar de Logger-entiteit](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [documentatie voor log Event hub beleid](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
