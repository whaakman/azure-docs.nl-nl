---
title: Optimaliseren van uw Azure-code in Visual Studio | Microsoft Docs
description: Meer informatie over hoe Azure code optimalisatie hulpprogramma's in Visual Studio zorgt u ervoor dat uw code krachtigere en meer.
services: visual-studio-online
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: cawa
ms.openlocfilehash: a606c8e7d8b730b67bd8481656e099900eb39fbc
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="optimizing-your-azure-code"></a>Optimaliseren van uw Azure-Code
Wanneer u apps die gebruikmaken van Microsoft Azure programmeren, zijn er enkele codering procedures die u volgen moet om te voorkomen dat u problemen met app-schaalbaarheid, werking en prestaties in een cloudomgeving. Microsoft biedt een analyseprogramma Azure Code die wordt herkend en identificeert diverse van deze problemen meestal aangetroffen en kunt u deze kunt oplossen. U kunt het hulpprogramma in Visual Studio via NuGet downloaden.

## <a name="azure-code-analysis-rules"></a>Azure Code Analysis-regels
Het hulpmiddel Azure Code maakt gebruik van de volgende regels automatisch uw Azure code markeren wanneer er bekende problemen met prestaties beïnvloeden. Gevonden problemen worden weergegeven als een waarschuwingen of compilatiefouten. Code-oplossingen of suggesties om op te lossen waarschuwingen of fouten zijn vaak door middel van een gloeilampje opgegeven.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Vermijd het gebruik van standaard (in-process) sessiestatusmodus gebruiken
### <a name="id"></a>Id
AP0000

### <a name="description"></a>Beschrijving
Als u de sessiestatusmodus (in-process) standaard voor cloud-toepassingen gebruikt, kunt u de sessiestatus kunt verliezen.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
De sessiestatusmodus in het web.config-bestand opgegeven is in-process standaard. Als u geen vermelding in het configuratiebestand is opgegeven, moet de modus voor sessiestatus standaard ook in-process. De modus in-process sessiestatus in geheugen opgeslagen op de webserver. Wanneer een exemplaar opnieuw is opgestart of een nieuw exemplaar wordt gebruikt voor de load balancer of failover-ondersteuning, wordt de sessiestatus opgeslagen in het geheugen op de webserver is niet opgeslagen. Deze situatie wordt voorkomen dat de toepassing wordt schaalbare in de cloud.

ASP.NET-sessiestatus ondersteunt verschillende verschillende opslagopties voor sessiestatusgegevens: InProc, StateServer, SQL Server, aangepast, en uitgeschakeld. Het verdient aanbeveling dat u aangepaste modus als host voor gegevens op een externe sessiestatus-winkel, zoals gebruiken [Azure Session State-provider voor Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Oplossing
Een aanbevolen methode is dat de sessiestatus wordt opgeslagen op een beheerde cacheservice. Informatie over het gebruik [Azure Session State-provider voor Redis](http://go.microsoft.com/fwlink/?LinkId=401521) voor het opslaan van de sessiestatus. U kunt de sessiestatus ook opslaan in andere locaties om te controleren of uw toepassing schaalbare in de cloud. Voor meer informatie over alternatieve oplossingen Lees [sessie status modi](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Voer methode mag geen asynchrone
### <a name="id"></a>Id
AP1000

### <a name="description"></a>Beschrijving
Maken van asynchrone methoden (zoals [await](https://msdn.microsoft.com/library/hh156528.aspx)) buiten de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode en roept u de async-methoden van [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Het declareren van de [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode als asynchroon zorgt ervoor dat de werkrol een lus opnieuw invoeren.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Het aanroepen van asynchrone methoden in de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode zorgt ervoor dat de runtime cloud service wilt recyclen van de werkrol. Wanneer een werkrol wordt gestart, alle programma-uitvoering vindt plaats in de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode. Afsluiten van de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode zorgt ervoor dat de werkrol om opnieuw te starten. Wanneer de runtime worker-rol bij de async-methode aankomt, verzendt alle bewerkingen na de async-methode en vervolgens terugkeert. Dit zorgt ervoor dat de werkrol om af te sluiten van de [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode en start deze opnieuw. In de volgende herhaling van de uitvoering van de werkrol de async-methode opnieuw treffers en opnieuw is opgestart, waardoor de werkrol wilt recyclen ook opnieuw.

### <a name="solution"></a>Oplossing
Plaats alle asynchrone bewerkingen buiten de [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode. Roep vervolgens de geherstructureerd async-methode van binnen de [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode, zoals RunAsync () .wait. Het hulpmiddel Azure Code kunt u kunt dit probleem oplossen.

Het volgende codefragment wordt getoond hoe de code-oplossing voor dit probleem:

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
Shared Access Signature (SAS) gebruiken voor verificatie. Access Control Service (ACS) wordt voor service bus-verificatie afgeschaft.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Voor een betere beveiliging vervangt Azure Active Directory authentication ACS door de SAS-verificatie. Zie [Azure Active Directory is de toekomst van ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) voor meer informatie over het plan overgang.

### <a name="solution"></a>Oplossing
De SAS-verificatie gebruiken in uw apps. Het volgende voorbeeld ziet hoe u een bestaande SAS-token gebruikt voor toegang tot een service bus-naamruimte of entiteit.

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
Om te voorkomen dat momenteel in een 'ontvangen lus,' aanroepen van de **OnMessage** methode is een betere oplossing voor het ontvangen van berichten dan aanroepen de **ontvangen** methode. Echter, als u moet de **ontvangen** methode en u geeft de wachttijd van een niet-standaard-server, controleert u de wachttijd van de server is meer dan één minuut.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Bij het aanroepen van **OnMessage**, de client wordt gestart van een interne bericht pomp die voortdurend de wachtrij of abonnement peilt. Deze pomp bericht bevat een oneindige lus die problemen met een aanroep om berichten te ontvangen. Als de aanroep van een optreedt time-out, moet deze een nieuwe aanroep uitgeeft. De time-outinterval wordt bepaald door de waarde van de [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) eigenschap van de [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)die wordt gebruikt.

Het voordeel van **OnMessage** vergeleken met **ontvangen** is dat gebruikers niet handmatig poll-frequentie voor berichten, uitzonderingen, meerdere berichten parallel verwerken en de berichten te voltooien.

Als u aanroept **ontvangen** zonder de standaardwaarde wordt gebruikt, moet u de *ServerWaitTime* waarde is meer dan één minuut. Instelling *ServerWaitTime* met meer dan één minuut voorkomt u dat de server een time-out opgetreden voordat het bericht volledig is ontvangen.

### <a name="solution"></a>Oplossing
Zie de volgende codevoorbeelden voor het gebruik van aanbevolen. Zie voor meer informatie [QueueClient.OnMessage methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)en [QueueClient.Receive methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Om te verbeteren de prestaties van de Azure messaging-infrastructuur, Zie het ontwerppatroon [asynchrone Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).

Hieronder volgt een voorbeeld van het gebruik van **OnMessage** om berichten te ontvangen.

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

Hieronder volgt een voorbeeld van het gebruik van **ontvangen** met de standaardserver wachttijd.

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

Hieronder volgt een voorbeeld van het gebruik van **ontvangen** met een niet-standaard server wachttijd.

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
Asynchrone Service Bus-methoden gebruiken voor het verbeteren de prestaties met brokered messaging.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Met asynchrone methoden kunt toepassing programma gelijktijdigheid omdat de hoofdthread uitvoeren van elke aanroep niet blokkeert. Wanneer u Service Bus messaging-methoden, uitvoeren van een bewerking (verzenden, ontvangen, verwijderen, enz.) tijd in beslag neemt. Deze tijd bevat de verwerking van de bewerking door de Service Bus-service naast de latentie van de aanvraag en het antwoord. Voor een verhoging van het aantal bewerkingen per keer moeten worden operations gelijktijdig worden uitgevoerd. Raadpleeg voor meer informatie [aanbevolen procedures voor prestaties verbeteringen met behulp van Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Oplossing
Zie [QueueClient klasse (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) voor informatie over het gebruik van de aanbevolen asynchrone methode.

Om te verbeteren de prestaties van de Azure messaging-infrastructuur, Zie het ontwerppatroon [asynchrone Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>U kunt partitionering Service Bus-wachtrijen en onderwerpen
### <a name="id"></a>Id
AP2004

### <a name="description"></a>Beschrijving
Partitie Service Bus-wachtrijen en onderwerpen voor betere prestaties bij het Service Bus-berichtenservice.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Service Bus-wachtrijen en onderwerpen partitioneren verhoogt de doorvoer en de service de beschikbaarheid van prestaties omdat de totale doorvoer van een gepartitioneerde wachtrij of onderwerp niet langer door de prestaties van een enkel bericht broker of berichten-store beperkt is. Bovendien een tijdelijke onderbreking van berichten-store betekent niet dat een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Zie voor meer informatie [Messaging Entities partitioneren](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Oplossing
Het volgende codefragment laat zien hoe voor het partitioneren van berichtentiteiten.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Zie voor meer informatie [gepartitioneerd Service Bus-wachtrijen en onderwerpen | Microsoft Azure-Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) en bekijk de [Microsoft Azure Service Bus gepartitioneerd wachtrij](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) voorbeeld.

## <a name="do-not-set-sharedaccessstarttime"></a>Stel geen SharedAccessStartTime
### <a name="id"></a>Id
AP3001

### <a name="description"></a>Beschrijving
Vermijd het gebruik van SharedAccessStartTimeset aan de huidige tijd om direct te starten het beleid voor gedeelde toegang. Alleen moet u deze eigenschap instellen als u wilt het beleid voor gedeelde toegang op een later tijdstip te starten.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Synchronisatie van computerklok zorgt ervoor dat een lichte tijdsverschil tussen datacenters. Bijvoorbeeld, logisch daar de begintijd van een SAS-beveiligingsbeleid van de opslag aan het huidige tijdstip instellen met behulp van DateTime.Now of een vergelijkbare manier, zal het SAS-beleid wordt onmiddellijk van kracht. Echter, het lichte tijdsverschil tussen datacentra kunnen problemen veroorzaken met dit omdat een aantal keren datacenter mogelijk enigszins hoger is dan de begintijd, terwijl andere voor deze. Als gevolg hiervan het SAS-beleid kunt verloopt snel (of zelfs onmiddellijk) als de levensduur van het beleid te kort is ingesteld.

Zie voor meer informatie over het gebruik van Shared Access Signature op Azure-opslag [introductie van tabel SAS (Shared Access Signature), wachtrij SAS en update voor Blob-SAS - Team Blog van Microsoft Azure Storage - Site-MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Oplossing
Verwijder de instructie die de begintijd van het beleid voor gedeelde toegang ingesteld. Het hulpmiddel Azure Code biedt een oplossing voor dit probleem. Zie voor meer informatie over beveiligingsbeheer het ontwerppatroon [Valet sleutel patroon](https://msdn.microsoft.com/library/dn568102.aspx).

Het volgende codefragment bevat de code-oplossing voor dit probleem.

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
Er kunnen zich als een vijf minuten verschil in klokken tussen datacenters op verschillende locaties als gevolg van een voorwaarde bekend als "tijdsverschil." Om te voorkomen dat de SAS instellen-token verloopt beleid eerdere versies dan gepland, het verlooptijdstip moet meer dan vijf minuten.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Datacenters op verschillende locaties wereldwijd synchroniseren met een kloksignaal. Omdat het duurt voor kloksignaal naar verschillende locaties reizen, kan er een verschil tijd tussen datacenters op verschillende geografische locaties Hoewel alles zogenaamd is gesynchroniseerd. Deze tijdsverschil kan invloed hebben op de toegang tot gedeelde policy start tijd en verlooptijd-interval. Daarom om ervoor te zorgen gedeeld toegangsbeleid wordt direct van kracht, geeft u de begintijd. Bovendien moet dat de vervaltijd is langer dan vijf minuten om te voorkomen dat eerdere time-out.

Zie voor meer informatie over het gebruik van Shared Access Signature op Azure-opslag [introductie van tabel SAS (Shared Access Signature), wachtrij SAS en update voor Blob-SAS - Team Blog van Microsoft Azure Storage - Site-MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Oplossing
Zie voor meer informatie over beveiligingsbeheer het ontwerppatroon [Valet sleutel patroon](https://msdn.microsoft.com/library/dn568102.aspx).

Hier volgt een voorbeeld van een begintijd voor toegang tot gedeelde beleid niet op te geven.

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

Hier volgt een voorbeeld van het opgeven van een begintijd voor toegang tot gedeelde beleid met een duur van de beleid-vervaldatum langer dan vijf minuten.

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

Zie voor meer informatie [maken en gebruiken van een Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Gebruik CloudConfigurationManager
### <a name="id"></a>Id
AP4000

### <a name="description"></a>Beschrijving
Met behulp van de [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) klasse voor projecten, zoals Azure-Website en Azure mobile services won't introduceren runtime-problemen. Als een best practice, het is echter een goed idee om te gebruiken Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) uniforme wijze van het beheren van configuraties voor alle Azure-Cloud-toepassingen.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
CloudConfigurationManager leest het configuratiebestand dat geschikt is voor de omgeving van toepassing.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Oplossing
Opsplitsen van uw code te gebruiken de [klasse CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Een code-oplossing voor dit probleem wordt geleverd door het hulpmiddel Azure Code.

Het volgende codefragment bevat de code-oplossing voor dit probleem. Vervangen

`var settings = ConfigurationManager.AppSettings["mySettings"];`

met

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Hier volgt een voorbeeld van het opslaan van de configuratie-instelling in een App.config- of Web.config-bestand. De instellingen toevoegen aan de sectie appSettings van het configuratiebestand. Hieronder volgt het bestand Web.config voor het vorige codevoorbeeld.

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
Als u vastgelegde verbindingsreeksen en moet u deze later bijwerken, hebt u wijzigingen aanbrengen in de broncode en de toepassing opnieuw te compileren. Echter, als u de verbindingstekenreeksen in een configuratiebestand opslaat, u kunt ze later wijzigen door simpelweg bijwerken van het configuratiebestand.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Hard-coding van verbindingsreeksen is een ongeldige procedure omdat hierdoor problemen wanneer verbindingsreeksen moeten snel worden gewijzigd. Bovendien, als het project worden gecontroleerd moet om verbinding met broncodebeheer, vastgelegde verbindingsreeksen leiden tot beveiligingsproblemen omdat de tekenreeksen kunnen worden weergegeven in de broncode.

### <a name="solution"></a>Oplossing
Verbindingsreeksen worden opgeslagen in configuratiebestanden of Azure-omgevingen.

* Voor zelfstandige toepassingen app.config te gebruiken voor het opslaan van verbindingstekenreeksinstellingen.
* Gebruik web.config voor het opslaan van verbindingsreeksen voor IIS gehoste webtoepassingen.
* Gebruik voor ASP.NET-toepassingen vNext configuration.json voor het opslaan van verbindingsreeksen.

Zie voor meer informatie over het gebruik van configuraties bestanden zoals web.config of app.config [ASP.NET Web configuratie-instructies](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx). Zie voor informatie over hoe Azure-omgeving variabelen werken, [Azure websites: tekenreeksen van toepassingen en verbinding tekenreeksen werken](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Zie voor meer informatie over het opslaan van de verbindingsreeks in broncodebeheer [te voorkomen dat gevoelige informatie zoals verbindingsreeksen in bestanden die zijn opgeslagen in de bron code opslagplaats](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Diagnostische gegevens configuratiebestand gebruiken
### <a name="id"></a>Id
AP5000

### <a name="description"></a>Beschrijving
In plaats van de diagnostische instellingen configureren in uw code zoals met behulp van de API programming Microsoft.WindowsAzure.Diagnostics, moet u de diagnostische instellingen configureren in het bestand diagnostics.wadcfg. (Of diagnostics.wadcfgx als u gebruikmaakt van Azure SDK 2.5). Op deze manier kunt u de diagnostische instellingen kunt wijzigen zonder de code compileren.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
Voordat u Azure SDK 2,5 (die gebruikmaakt van Azure diagnostics 1.3), Azure Diagnostics (af) kan worden geconfigureerd met behulp van verschillende methoden gebruiken: toe te voegen aan de configuratie-blob in de opslag, via imperatieve code, declaratieve configuratie of de standaardwaarde de configuratie. De beste manier voor het configureren van diagnostische gegevens is echter een XML-configuratiebestand (diagnostics.wadcfg of diagnositcs.wadcfgx voor SDK 2.5 en hoger) in het toepassingsproject gebruiken. In deze benadering, kan het bestand diagnostics.wadcfg volledig definieert de configuratie en worden bijgewerkt en geïmplementeerd op wordt. Het gebruik van het configuratiebestand diagnostics.wadcfg combineren met de programmatische methoden voor het instellen van configuraties met behulp van de [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)of [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)klassen kunnen tot verwarring leiden. Zie [initialiseren of Azure Diagnostics-configuratie wijzigen](https://msdn.microsoft.com/library/azure/hh411537.aspx) voor meer informatie.

Beginnend met af 1.3 (opgenomen in de Azure SDK 2.5), is het niet meer code gebruiken voor het configureren van diagnostische gegevens. Als gevolg hiervan kunt u alleen de configuratie tijdens het toepassen van of bijwerken van de extensie voor diagnostische gegevens opgeven.

### <a name="solution"></a>Oplossing
Gebruik de ontwerpfunctie van de configuratie van diagnostische gegevens te verplaatsen van diagnostische instellingen voor het configuratiebestand van diagnostische gegevens (diagnositcs.wadcfg of diagnositcs.wadcfgx voor SDK 2.5 en hoger). Ook wordt aanbevolen dat u installeert [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) en de meest recente diagnostics-functie gebruiken.

1. In het snelmenu voor de rol die u wilt configureren, kiest u eigenschappen en kies vervolgens het tabblad configuratie.
2. In de **Diagnostics** sectie, zorg ervoor dat de **diagnostische gegevens inschakelen** selectievakje is ingeschakeld.
3. Kies de **configureren** knop.

   ![Toegang tot de optie diagnostische gegevens inschakelen](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   Zie [diagnostische gegevens configureren voor Azure Cloud Services en virtuele Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) voor meer informatie.

## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vermijd DbContext-objecten als statische declareren
### <a name="id"></a>Id
AP6000

### <a name="description"></a>Beschrijving
Vermijd DBContext-objecten als statische declareren voor het opslaan van geheugen.

Deel uw ideeën en feedback op [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden
DBContext-objecten bevatten van elke aanroep van de queryresultaten. Statische DBContext-objecten zijn niet verwijderd totdat het toepassingsdomein verwijderd is. Een statisch DBContext-object kan daarom grote hoeveelheden geheugen gebruiken.

### <a name="solution"></a>Oplossing
DBContext declareren als een lokale variabele of een van de niet-statisch exemplaarveld, gebruiken voor een taak en vervolgens laat u dit na gebruik worden verwijderd.

Het volgende voorbeeld MVC-controllerklasse laat zien hoe de DBContext-object te gebruiken.

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
Zie voor meer informatie over het optimaliseren van en probleemoplossing van apps van Azure [een web-app in Azure App Service met behulp van Visual Studio oplossen](app-service/web-sites-dotnet-troubleshoot-visual-studio.md).
