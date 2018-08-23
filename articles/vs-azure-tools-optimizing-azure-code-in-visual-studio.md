---
title: Optimaliseren van uw Azure-code in Visual Studio | Microsoft Docs
description: Meer informatie over hoe Azure-code optimalisatie-hulpprogramma's in Visual Studio te maken van uw code krachtigere en meer.
services: visual-studio-online
documentationcenter: na
author: cawa
manager: paulyuk
editor: ''
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/11/2016
ms.author: cawa
ms.openlocfilehash: 0497ac628d7882a0b722796493c10c0d8b04e759
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42443665"
---
# <a name="optimizing-your-azure-code"></a>Optimaliseren van uw Azure-Code
Wanneer u apps die gebruikmaken van Microsoft Azure programmeren, zijn er enkele codering procedures die u volgen moet om te voorkomen van problemen met app-schaalbaarheid, werking en prestaties in een cloudomgeving. Microsoft biedt een analyse van de Azure-Code-hulpprogramma dat herkent en geeft diverse van deze problemen meestal aangetroffen en helpt u bij het op te lossen. U kunt het hulpprogramma in Visual Studio via NuGet te downloaden.

## <a name="azure-code-analysis-rules"></a>Azure-Code Analysis regels
Het hulpmiddel Azure Code maakt gebruik van de volgende regels automatisch uw Azure-code markeren wanneer er bekende problemen met prestaties beïnvloeden. Gevonden problemen worden weergegeven als een waarschuwingen of compilatiefouten. Code voor problemen of suggesties voor het oplossen van de waarschuwing of fout worden vaak geleverd via het pictogram van een gloeilampje.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Vermijd het gebruik van standaard (in-process) sessiestatusmodus
### <a name="id"></a>Id
AP0000

### <a name="description"></a>Beschrijving
Als u de standaardmodus voor de status van de sessie (in-process) voor cloud-toepassingen gebruikt, kunt u de sessiestatus kunt verliezen.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Standaard is de sessiestatusmodus die is opgegeven in het web.config-bestand in-process. Ook als er geen vermelding die is opgegeven in het configuratiebestand, standaard de modus voor sessiestatus in-process. De modus in-process sessiestatus in geheugen opgeslagen op de webserver. Wanneer een exemplaar opnieuw wordt opgestart of een nieuw exemplaar wordt gebruikt voor taakverdeling of failover-ondersteuning, wordt de sessiestatus opgeslagen in het geheugen op de webserver wordt niet opgeslagen. Deze situatie wordt voorkomen dat de toepassing wordt schaalbare in de cloud.

ASP.NET-sessiestatus ondersteunt verschillende opties voor andere opslag voor gegevens over de sessiestatus: InProc, StateServer, SQL Server, aangepast, en Off. Het wordt aanbevolen dat u aangepaste modus waarnaar moet worden hostgegevens op een externe sessiestatus-store, zoals [Azure State-provider voor Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Oplossing
Een aanbevolen oplossing is voor het opslaan van de status van de sessie op een beheerde cacheservice. Meer informatie over het gebruik van [Azure State-provider voor Redis](http://go.microsoft.com/fwlink/?LinkId=401521) voor het opslaan van de sessiestatus. U kunt ook de sessiestatus opslaan op andere plaatsen om te controleren of dat uw toepassing schaalbaar is in de cloud. Voor meer informatie over alternatieve oplossingen Lees [sessie status modi](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Voer methode mag geen asynchrone
### <a name="id"></a>Id
AP1000

### <a name="description"></a>Beschrijving
Asynchrone methoden maken (zoals [await](https://msdn.microsoft.com/library/hh156528.aspx)) buiten de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode en roept u vervolgens het async-methoden van [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Declareren de [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) asynchrone methode zorgt ervoor dat de werkrol in te voeren een lus voor opnieuw opstarten.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Aanroepen van asynchrone methoden in de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode zorgt ervoor dat de cloud service runtime te recyclen van de werkrol. Wanneer een werkrol wordt gestart, alle programma-uitvoering vindt plaats binnen de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode. Afsluiten van de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode zorgt ervoor dat de werkrol te starten. Wanneer de runtime worker-rol bij het async-methode aankomt, verzendt alle bewerkingen na de methode async en retourneert vervolgens. Dit zorgt ervoor dat de werkrol om af te sluiten van de [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode en start deze opnieuw. In de volgende iteratie van de uitvoering van de werkrol treffers in de asynchrone methode opnieuw en opnieuw is opgestart, waardoor de werkrol worden ook opnieuw worden gerecycled.

### <a name="solution"></a>Oplossing
Plaats alle asynchrone bewerkingen buiten de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode. Roep vervolgens de geherstructureerd async-methode uit binnen de [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode, zoals RunAsync () .wait. Het hulpmiddel Azure Code kunt u kunt dit probleem oplossen.

Het volgende codefragment ziet u de code-oplossing voor dit probleem:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Service Bus Shared Access Signature-verificatie gebruiken
### <a name="id"></a>Id
AP2000

### <a name="description"></a>Beschrijving
Shared Access Signature (SAS) gebruiken voor verificatie. Access Control Service (ACS) is afgeschaft voor service bus-verificatie.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Voor een betere beveiliging is Azure Active Directory vervangen van ACS-verificatie met SAS-verificatie. Zie [Azure Active Directory is de toekomst van ACS](https://cloudblogs.microsoft.com/enterprisemobility/2013/06/22/azure-active-directory-is-the-future-of-acs/) voor meer informatie over het overgangsplan.

### <a name="solution"></a>Oplossing
SAS-verificatie gebruiken in uw apps. Het volgende voorbeeld ziet hoe u een bestaande SAS-token gebruikt voor toegang tot een service bus-naamruimte of de entiteit.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Zie de volgende onderwerpen voor meer informatie.

* Zie voor een overzicht [Shared Access Signature-verificatie met Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)
* [Het gebruik van Shared Access Signature-verificatie met Service Bus](https://msdn.microsoft.com/library/dn205161.aspx)
* Zie voor een voorbeeldproject [met behulp van Shared Access Signature (SAS)-verificatie met Service Bus-abonnementen](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Overweeg het gebruik van OnMessage methode om te voorkomen dat 'ontvangen lus'
### <a name="id"></a>Id
AP2002

### <a name="description"></a>Beschrijving
Om te voorkomen in een 'ontvangen lus' aanroepen van de **OnMessage** methode is een betere oplossing voor het ontvangen van berichten dan aanroepen de **ontvangen** methode. Echter, als u moet gebruiken de **ontvangen** methode en kunt u opgeven de wachttijd voor een niet-standaard-server, zorg ervoor dat de wachttijd voor de server is meer dan één minuut.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Bij het aanroepen van **OnMessage**, de client een interne bericht pomp die voortdurend de wachtrij of abonnement peilt wordt gestart. Dit bericht pomp bevat een oneindige lus uit die problemen met een aanroep om berichten te ontvangen. Als de aanroep van een optreedt time-out, is deze service een nieuwe aanroep. De time-outinterval wordt bepaald door de waarde van de [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) eigenschap van de [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)die wordt gebruikt.

Het voordeel van het gebruik van **OnMessage** in vergelijking met **ontvangen** is dat gebruikers hoeft te handmatig of er berichten, het verwerken van uitzonderingen, meerdere berichten parallel verwerken en voltooien van de berichten.

Als u aanroepen **ontvangen** zonder met behulp van de standaardwaarde, zorg ervoor dat de *ServerWaitTime* waarde is meer dan één minuut. Instellen van *ServerWaitTime* met meer dan één minuut voorkomt dat de server een time-out opgetreden voordat het bericht volledig is ontvangen.

### <a name="solution"></a>Oplossing
Raadpleeg de volgende codevoorbeelden voor het gebruik van aanbevolen. Zie voor meer informatie, [QueueClient.OnMessage methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)en [QueueClient.Receive methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Ter verbetering van de prestaties van de Azure messaging-infrastructuur, Zie het ontwerppatroon [Inleiding in asynchrone berichten](https://msdn.microsoft.com/library/dn589781.aspx).

Hier volgt een voorbeeld van het gebruik van **OnMessage** om berichten te ontvangen.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Hier volgt een voorbeeld van het gebruik van **ontvangen** tijd wachten met de standaard-server.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Hier volgt een voorbeeld van het gebruik van **ontvangen** met een niet-standaard-server wachttijd.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Overweeg het gebruik van asynchrone Service Bus-methoden
### <a name="id"></a>Id
AP2003

### <a name="description"></a>Beschrijving
Asynchrone Service Bus-methoden gebruiken om prestaties met brokered berichten te verbeteren.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Asynchrone methoden kunt toepassing programma gelijktijdigheid omdat elke aanroep wordt uitgevoerd op de hoofdthread niet blokkeert. Wanneer u Service Bus messaging-methoden, door een bewerking uitgevoerd (verzenden, ontvangen, verwijderen, enz.) kost tijd. Deze keer bevat de verwerking van de bewerking door de Service Bus-service naast de latentie van de aanvraag en het antwoord. Als u wilt het aantal bewerkingen per keer verhogen, moeten gelijktijdig bewerkingen uitvoeren. Raadpleeg voor meer informatie [aanbevolen procedures voor prestaties verbeteringen met behulp van Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Oplossing
Zie [QueueClient klasse (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) voor informatie over het gebruik van de aanbevolen asynchrone methode.

Ter verbetering van de prestaties van de Azure messaging-infrastructuur, Zie het ontwerppatroon [Inleiding in asynchrone berichten](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Houd rekening met het partitioneren van Service Bus-wachtrijen en onderwerpen
### <a name="id"></a>Id
AP2004

### <a name="description"></a>Beschrijving
Partitie Service Bus-wachtrijen en onderwerpen voor betere prestaties met Service Bus-berichten.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Service Bus-wachtrijen en onderwerpen partitioneren verhoogt de doorvoer en de service de beschikbaarheid van prestaties omdat de algehele doorvoer van een gepartitioneerde wachtrij of onderwerp niet langer door de prestaties van één bericht-broker of berichtenarchief beperkt is. Bovendien een tijdelijke onderbreking van een berichtenarchief betekent niet dat een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Zie voor meer informatie, [Messaging-entiteiten partitioneren](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Oplossing
Het volgende codefragment laat zien hoe voor het partitioneren van berichtentiteiten.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Zie voor meer informatie, [gepartitioneerd Service Bus-wachtrijen en onderwerpen | Microsoft Azure-Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) en bekijk de [Microsoft Azure Service Bus gepartitioneerde wachtrij](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) voorbeeld.

## <a name="do-not-set-sharedaccessstarttime"></a>Stel geen SharedAccessStartTime
### <a name="id"></a>Id
AP3001

### <a name="description"></a>Beschrijving
Vermijd het gebruik van SharedAccessStartTimeset op de huidige tijd om direct te starten het beleid voor gedeelde toegang. U hoeft deze eigenschap instellen als u wilt het beleid voor gedeelde toegang op een later tijdstip te starten.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Synchronisatie van computerklokken zorgt ervoor dat een lichte tijdsverschil tussen datacenters. Bijvoorbeeld, u logisch denkt de begintijd van een SAS-beleid voor opslag als de huidige tijd instellen met behulp van DateTime.Now of een vergelijkbare manier zorgt ervoor dat het SAS-beleid wordt onmiddellijk van kracht. Echter, het lichte tijdsverschil tussen datacenters kunnen problemen veroorzaken met dit omdat enkele keren datacenter mogelijk iets hoger dan de begintijd, terwijl andere vooraf het. Als gevolg hiervan de SAS-beleid kan verlopen snel (of zelfs onmiddellijk) als de levensduur van het beleid te kort is ingesteld.

Zie voor meer informatie over het gebruik van Shared Access Signature op Azure-opslag, [Introducing Table SAS (Shared Access Signature), Queue SAS bij te werken naar Blob-SAS - Team Blog van Microsoft Azure Storage - Site Start - MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Oplossing
De instructie die Hiermee stelt u de begintijd van het beleid voor gedeelde toegang verwijderen. De Azure Code-analyseprogramma biedt een oplossing voor dit probleem. Zie voor meer informatie over het beveiligingsbeheer het ontwerppatroon [Valet-sleutelpatroon](https://msdn.microsoft.com/library/dn568102.aspx).

Het volgende codefragment ziet u de code-oplossing voor dit probleem.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Gedeeld toegangsbeleid verlooptijdstip meer dan vijf minuten moet
### <a name="id"></a>Id
AP3002

### <a name="description"></a>Beschrijving
Kunnen er maar liefst vijf even verschil in klokken tussen datacenters op verschillende locaties vanwege een voorwaarde bekend als "tijdsverschil." Om te voorkomen dat de SAS ingesteld beleid token verlopen ouder dan gepland, het verlooptijdstip moet meer dan vijf minuten.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Datacenters op verschillende locaties over de hele wereld synchroniseren met een kloksignaal. Omdat het enige tijd om kloksignaal om te reizen naar verschillende locaties vergt, kan er een afwijking van de tijd tussen datacenters op verschillende geografische locaties Hoewel alles zogenaamd wordt gesynchroniseerd. Dit verschil in tijd kan invloed hebben op de toegang tot gedeelde beleid start tijd en de vervaldatum interval. Daarom om ervoor te zorgen gedeeld toegangsbeleid wordt direct van kracht, geeft u de begintijd. Bovendien moet u ervoor dat de verlooptijd is meer dan vijf minuten om te voorkomen dat eerdere time-out.

Zie voor meer informatie over het gebruik van Shared Access Signature op Azure storage [Introducing Table SAS (Shared Access Signature), Queue SAS bij te werken naar Blob-SAS - Team Blog van Microsoft Azure Storage - Site Start - MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Oplossing
Zie voor meer informatie over het beveiligingsbeheer, het ontwerppatroon [Valet-sleutelpatroon](https://msdn.microsoft.com/library/dn568102.aspx).

Hier volgt een voorbeeld van een begintijd voor gedeelde toegang-beleid niet op te geven.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Hier volgt een voorbeeld van een begintijd voor gedeelde toegang-beleid met een beleid Vervaltijd van de meer dan vijf minuten op te geven.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Zie voor meer informatie, [maken en gebruiken van een Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Gebruik CloudConfigurationManager
### <a name="id"></a>Id
AP4000

### <a name="description"></a>Beschrijving
Met behulp van de [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) klasse voor projecten zoals Azure-Website en Azure mobile services wordt niet runtimeproblemen introduceren. Als een best practice, het is echter een goed idee om het gebruik van Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) als een uniforme manier van het beheer van configuraties voor alle Azure-Cloud-toepassingen.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
CloudConfigurationManager leest het configuratiebestand dat geschikt is voor de omgeving.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Oplossing
Herstructureren van uw code te gebruiken de [klasse CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Een code-oplossing voor dit probleem wordt geleverd door het hulpmiddel voor Azure-Code.

Het volgende codefragment ziet u de code-oplossing voor dit probleem. Vervangen

`var settings = ConfigurationManager.AppSettings["mySettings"];`

met

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Hier volgt een voorbeeld van hoe u voor het opslaan van de configuratie-instelling in een App.config- of Web.config-bestand. Instellingen toevoegt aan de sectie appSettings van het configuratiebestand. Hier volgt het Web.config-bestand voor het vorige codevoorbeeld.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Vermijd het gebruik van vastgelegde verbindingsreeksen
### <a name="id"></a>Id
AP4001

### <a name="description"></a>Beschrijving
Als u verbindingsreeksen vastgelegde en moet u ze later bijwerken, hebt u wijzigingen aanbrengen aan uw broncode en de toepassing opnieuw te compileren. Echter, als u uw verbindingsreeksen in een configuratiebestand opslaat, u kunt ze later wijzigen door bij te werken het configuratiebestand.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Codeert verbindingsreeksen is een ongeldige procedure omdat deze problemen worden geïntroduceerd als tekenreeksen voor databaseverbindingen moeten snel worden gewijzigd. Bovendien, als het project worden ingecheckt met resourcebeheer moet, tekenreeksen voor databaseverbindingen vastgelegde leiden tot beveiligingsproblemen omdat de tekenreeksen kunnen worden weergegeven in de broncode.

### <a name="solution"></a>Oplossing
Tekenreeksen voor databaseverbindingen Store in de configuratiebestanden of een Azure-omgevingen.

* Gebruik voor zelfstandige toepassingen, app.config voor het opslaan van instellingen voor connection strings.
* Gebruik web.config voor het opslaan van tekenreeksen voor databaseverbindingen voor IIS gehoste webtoepassingen.
* Gebruik configuration.json voor het opslaan van tekenreeksen voor databaseverbindingen voor ASP.NET vNext-toepassingen.

Zie voor meer informatie over het gebruik van configuraties bestanden zoals web.config of app.config [richtlijnen voor ASP.NET Web](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx). Zie voor informatie over hoe Azure omgeving variabelen werken, [Azure websites: tekenreeksen van toepassingen en tekenreeksen werkt verbinding](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Zie voor meer informatie over het opslaan van de verbindingsreeks in broncodebeheer [te voorkomen dat gevoelige informatie, zoals verbindingsreeksen plaatsen in de bestanden die zijn opgeslagen in de opslagplaats broncode](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Diagnostics-configuratiebestand gebruiken
### <a name="id"></a>Id
AP5000

### <a name="description"></a>Beschrijving
In plaats van de instellingen voor diagnostische gegevens in uw code zoals configureren met behulp van de Microsoft.WindowsAzure.Diagnostics API programmeren, moet u de diagnostische instellingen configureren in het bestand diagnostics.wadcfg. (Of diagnostics.wadcfgx als u gebruikmaakt van Azure SDK 2.5). Op deze manier kunt kunt u de diagnostische instellingen wijzigen zonder de code compileren.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Voordat u Azure SDK 2.5 (die gebruikmaakt van Azure diagnostics 1.3), Azure Diagnostics (WAD) kan worden geconfigureerd met behulp van verschillende methoden: toe te voegen aan de gatewayconfiguratie-blob in de opslag, met behulp van imperatieve code, declaratieve configuratie of de standaardwaarde de configuratie. De voorkeursmethode voor het configureren van diagnostische gegevens is echter een XML-configuratiebestand (diagnostics.wadcfg of diagnositcs.wadcfgx voor SDK 2.5 of hoger) in het toepassingsproject gebruiken. In deze benadering het diagnostics.wadcfg bestand kan volledig definieert de configuratie en worden bijgewerkt en opnieuw op zal worden geïmplementeerd. Het gebruik van het configuratiebestand diagnostics.wadcfg combineren met de programmatische methoden van het instellen van configuraties met behulp van de [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)of [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)klassen kunnen tot verwarring leiden. Zie [initialiseren of Azure Diagnostics-configuratie wijzigen](https://msdn.microsoft.com/library/azure/hh411537.aspx) voor meer informatie.

Beginnen met WAD 1.3 (opgenomen in de Azure SDK 2.5), is het niet meer mogelijk code gebruikt om te configureren van diagnostische gegevens. Als gevolg hiervan kunt u alleen de configuratie bij het toepassen of bijwerken van de extensie voor diagnostische gegevens opgeven.

### <a name="solution"></a>Oplossing
Diagnostische gegevens configuration designer gebruiken om te verplaatsen van diagnostische instellingen voor het configuratiebestand voor diagnostische gegevens (diagnositcs.wadcfg of diagnositcs.wadcfgx voor SDK 2.5 of hoger). Het verdient ook aanbeveling dat u installeert [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) en de meest recente functie voor diagnostische gegevens gebruiken.

1. In het snelmenu voor de rol die u wilt configureren, kiest u eigenschappen en kies vervolgens het tabblad configuratie.
2. In de **Diagnostics** sectie, zorg ervoor dat de **diagnostische gegevens inschakelen** selectievakje is ingeschakeld.
3. Kies de **configureren** knop.

   ![Toegang tot de optie diagnostische gegevens inschakelen](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   Zie [diagnostische gegevens configureren voor Azure Cloud Services en Virtual Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) voor meer informatie.

## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vermijd DbContext-objecten als statisch declareren
### <a name="id"></a>Id
AP6000

### <a name="description"></a>Beschrijving
Om geheugen te besparen, te voorkomen dat DBContext-objecten als statisch declareren.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
DBContext-objecten bevatten de resultaten van de query uit elke aanroep. Statische DBContext-objecten worden niet verwijderd totdat het toepassingsdomein verwijderd is. Een statisch DBContext-object kan daarom gebruikmaken van grote hoeveelheden geheugen.

### <a name="solution"></a>Oplossing
DBContext declareren als een lokale variabele of een niet-statische exemplaarveld, gebruiken voor een taak en vervolgens laat u dit na gebruik worden verwijderd.

Het volgende voorbeeld MVC-controllerklasse ziet over het gebruik van het object DBContext.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het optimaliseren van en het oplossen van Azure-apps, [oplossen van een web-app in Azure App Service met behulp van Visual Studio](app-service/web-sites-dotnet-troubleshoot-visual-studio.md).
