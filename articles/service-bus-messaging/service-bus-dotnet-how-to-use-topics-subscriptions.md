---
title: Service Bus-onderwerpen gebruiken met .NET | Microsoft Docs
description: Meer informatie over het gebruik van Service Bus-onderwerpen en -abonnementen met .NET in Azure. Codevoorbeelden zijn geschreven voor .NET-toepassingen.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 31d0bc29-6524-4b1b-9c7f-aa15d5a9d3b4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 12/21/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: add228c8a24fbd36ab05f55570abf1374f519822
ms.openlocfilehash: 9927de3bba251a2cc135657f00b789c7522fc05c


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Service Bus-onderwerpen en -abonnementen gebruiken
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe u Service Bus-onderwerpen en -abonnementen gebruikt. De voorbeelden zijn geschreven in C# en gebruiken de .NET API's. De behandelde scenario's hebben betrekking op het maken van onderwerpen en abonnementen, het maken van abonnementsfilters, het verzenden van berichten naar een onderwerp, het ontvangen van berichten van een abonnement en het verwijderen van onderwerpen en abonnementen. Zie de sectie [Volgende stappen](#next-steps) voor meer informatie over onderwerpen en abonnementen.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>De toepassing configureren voor het gebruik van Service Bus
Wanneer u een toepassing maakt die gebruikmaakt van Service Bus, moet u een verwijzing naar de Service Bus-assembly toevoegen en de bijbehorende naamruimten opnemen. De eenvoudigste manier om dit te doen, is het juiste [NuGet](https://www.nuget.org)-pakket te downloaden.

## <a name="get-the-service-bus-nuget-package"></a>Het Service Bus NuGet-pakket ophalen
Het [Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is de eenvoudigste manier om de Service Bus-API op te halen en om de toepassing te configureren met alle benodigde Service Bus-afhankelijkheden. Ga als volgt te werk om het Service Bus NuGet-pakket in uw project te installeren:

1. Klik in Solution Explorer met de rechtermuisknop op **Verwijzingen** en klik vervolgens op **NuGet-pakketten beheren**.
2. Zoek ‘Service Bus’ en selecteer het item **Microsoft Azure Service Bus**. Klik op **Installeren** om de installatie te voltooien en sluit het volgende dialoogvenster:
   
   ![][7]

U bent nu klaar om code te schrijven voor Service Bus.

## <a name="create-a-service-bus-connection-string"></a>Een Service Bus-verbindingsreeks maken
Service Bus maakt gebruik van een verbindingsreeks om eindpunten en referenties op te slaan. U kunt de verbindingsreeks in een configuratiebestand plaatsen in plaats van hiervoor hard-coding te gebruiken:

* Wanneer u Azure-services gebruikt, wordt het aanbevolen om de verbindingsreeks met het Azure-serviceconfiguratiesysteem (.csdef- en .cscfg-bestanden) op te slaan.
* Wanneer u Azure Websites of Azure Virtual Machines gebruikt, wordt het aanbevolen om de verbindingsreeks met het .NET-configuratiesysteem (bijvoorbeeld het Web.config-bestand) op te slaan.

In beide gevallen kunt u de verbindingsreeks ophalen met de `CloudConfigurationManager.GetSetting`-methode (zie verderop in dit artikel).

### <a name="configure-your-connection-string"></a>De verbindingsreeks configureren
U kunt met het serviceconfiguratiemechanisme configuratie-instellingen dynamisch wijzigen vanuit [Azure Portal][Azure portal] zonder dat u de toepassing opnieuw hoeft te implementeren. Voeg bijvoorbeeld een `Setting`-label toe aan het **.csdef**-bestand (het bestand met de servicedefinitie), zoals wordt weergegeven in het volgende voorbeeld.

```xml
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Vervolgens geeft u waarden op in het bestand met de serviceconfiguratie (.cscfg).

```xml
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Gebruik de SAS-sleutelnaam (Shared Access Signature) en sleutelwaarden die zijn opgehaald via de portal zoals eerder beschreven.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>De verbindingsreeks configureren bij gebruik van Azure Websites of Azure Virtual Machines
Bij gebruik van Azure Websites of Azure Virtual Machines wordt het aanbevolen het .NET-configuratiesysteem te gebruiken (bijvoorbeeld Web.config). U slaat de verbindingsreeks op met het `<appSettings>`-element.

```xml
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Gebruik de SAS-naam en sleutelwaarden die u hebt opgehaald via [Azure Portal][Azure portal], zoals eerder beschreven.

## <a name="create-a-topic"></a>Een onderwerp maken
U kunt voor Service Bus-onderwerpen en -abonnementen beheerbewerkingen uitvoeren met de klasse [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager). Deze klasse biedt methoden voor het maken, opsommen en verwijderen van onderwerpen.

In het volgende voorbeeld wordt een `NamespaceManager`-object gemaakt met de klasse `CloudConfigurationManager` van Azure met een verbindingsreeks die bestaat uit het basisadres van een Service Bus-naamruimte en de juiste SAS-referenties met machtigingen voor beheer. Deze verbindingsreeks heeft de volgende vorm:

```xml
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Gebruik het volgende voorbeeld met de configuratie-instellingen uit de vorige sectie.

```csharp
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Er zijn overloads van de [CreateTopic](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager)-methode waarmee u eigenschappen van het onderwerp kunt instellen, bijvoorbeeld om de standaard TTL-waarde (time-to-live) in te stellen die moet worden toegepast op berichten die naar het onderwerp worden verzonden. Deze instellingen worden toegepast met de klasse [TopicDescription](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicdescription). Het volgende voorbeeld laat zien hoe u een onderwerp maakt met de naam **TestTopic** en een maximale grootte van 5 GB, en een standaardbericht-TTL van 1 minuut.

```csharp
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [!NOTE]
> U kunt de [TopicExists](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_TopicExists_System_String_)-methode voor [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager)-objecten gebruiken om te controleren of een onderwerp met een opgegeven naam al in een naamruimte bestaat.
> 
> 

## <a name="create-a-subscription"></a>Een abonnement maken
U kunt ook onderwerpabonnementen maken met de klasse [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager). Abonnementen hebben een naam en kunnen een optioneel filter hebben waarmee de verzameling berichten wordt beperkt die aan de virtuele wachtrij van het abonnement wordt doorgegeven.

> [!IMPORTANT]
> Om de berichten door een abonnement te kunnen laten ontvangen, moet u dat abonnement maken voordat u berichten naar het onderwerp verzendt. Als er geen abonnementen voor een onderwerp zijn, worden deze berichten door het onderwerp verwijderd.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)
Het **MatchAll**-filter is het standaardfilter dat wordt gebruikt als er bij het maken van een nieuw abonnement geen filter is opgegeven. Bij gebruik van het **MatchAll**-filter worden alle berichten die naar het onderwerp worden gepubliceerd, in de virtuele wachtrij van het abonnement geplaatst. Met het volgende voorbeeld maakt u een abonnement met de naam AllMessages en wordt het standaardfilter **MatchAll** gebruikt.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken
U kunt ook filters instellen waarmee u kunt opgeven welke berichten die naar een onderwerp worden verzonden, moeten worden weergegeven in een bepaald onderwerpabonnement.

Het meest flexibele type filter dat wordt ondersteund door abonnementen, is de klasse [SqlFilter][SqlFilter] waarmee een subset van SQL92 wordt geïmplementeerd. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Zie de [SqlFilter.SqlExpression][SqlFilter.SqlExpression]-syntaxis voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter.

In het volgende voorbeeld wordt een abonnement genaamd **HighMessages** gemaakt met een [SqlFilter][SqlFilter]-object dat alleen berichten selecteert die een aangepaste **MessageNumber**-eigenschap groter dan 3 hebben.

```csharp
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Op dezelfde manier wordt in het volgende voorbeeld een abonnement genaamd **LowMessages** gemaakt met een [SqlFilter][SqlFilter] dat alleen berichten selecteert die een **MessageNumber**-eigenschap minder dan of gelijk aan 3 hebben.

```csharp
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Wanneer er nu een bericht naar `TestTopic` wordt verzonden, wordt het altijd bezorgd bij ontvangers die zijn geabonneerd op het **AllMessages**-onderwerpabonnement en selectief bezorgd bij ontvangers die zijn geabonneerd op het **HighMessages**- en **LowMessages**-onderwerpabonnement (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp
Voor het verzenden van een bericht naar een Service Bus-onderwerp maakt een toepassing een [TopicClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicclient)-object met de verbindingsreeks.

De volgende code laat zien hoe een [TopicClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicclient)-object wordt gemaakt voor het **TestTopic**-onderwerp dat eerder is gemaakt met de API [CreateFromConnectionString](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.topicclient#Microsoft_ServiceBus_Messaging_TopicClient_CreateFromConnectionString_System_String_System_String_).

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

De berichten die worden verzonden naar Service Bus-onderwerpen, zijn exemplaren van de klasse [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). **BrokeredMessage**-objecten hebben een aantal standaardeigenschappen (zoals [Label](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) en [TimeToLive](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen te bewaren en de hoofdtekst met willekeurige toepassingsgegevens. De hoofdtekst van het bericht kan met een toepassing worden ingesteld door elk serialiseerbaar object door te geven aan de constructor van het **BrokeredMessage**-object waarna de juiste **DataContractSerializer** wordt gebruikt om het object te serialiseren. Ook kan een **System.IO.Stream**-object worden geleverd.

Het volgende voorbeeld toont hoe vijf testberichten naar het **TestTopic** [TopicClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicclient)-object worden verzonden dat is verkregen in het vorige codevoorbeeld. Merk op dat de [MessageId](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId)-eigenschapswaarde van elk bericht varieert afhankelijk van de herhaling van de lus (deze bepaalt welke abonnementen het bericht ontvangen).

```csharp
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageId"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten in een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB. Als partitioneren is ingeschakeld, is de bovengrens hoger. Zie [Partitioned messaging entities](service-bus-partitioning.md) (Gepartitioneerde berichtentiteiten) voor meer informatie.

## <a name="how-to-receive-messages-from-a-subscription"></a>Berichten van een abonnement ontvangen
De aanbevolen manier om berichten te ontvangen van een abonnement is het gebruik van een [SubscriptionClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient)-object. **SubscriptionClient**-objecten kunnen werken in twee verschillende modi: [*ReceiveAndDelete* en *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** is de standaardmodus.

Wanneer u de **ReceiveAndDelete**-modus gebruikt, wordt het ontvangen in één bewerking uitgevoerd; dat wil zeggen als Service Bus een leesaanvraag voor een bericht in een abonnement ontvangt, wordt voor het bericht aangegeven dat het is verbruikt en wordt het bericht naar de toepassing geretourneerd. De **ReceiveAndDelete**-modus is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing het niet verwerken van een bericht bij een fout kan tolereren. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, ontbreekt het bericht dat voor het vastlopen is verbruikt wanneer de toepassing opnieuw wordt gestart en het verbruik van berichten opnieuw begint.

In de **PeekLock**-modus (de standaardmodus) wordt het ontvangstproces een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met de verwerking van het bericht (of het bericht veilig heeft opgeslagen voor toekomstige verwerking), wordt de tweede fase van het ontvangstproces voltooid door [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) aan te roepen voor het ontvangen bericht. Wanneer Service Bus de aanroep **Complete** waarneemt, wordt het bericht gemarkeerd als verbruikt en wordt het uit het abonnement verwijderd.

Het volgende voorbeeld laat zien hoe berichten kunnen worden ontvangen en verwerkt met de standaardmodus **PeekLock**. Als u een andere [ReceiveMode](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode)-waarde wilt opgeven, kunt u een andere overload voor [CreateFromConnectionString](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_CreateFromConnectionString_System_String_System_String_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) gebruiken. In dit voorbeeld wordt de [OnMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_)-callback gebruikt om berichten te verwerken wanneer ze bij het **HighMessages**-abonnement aankomen.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

In dit voorbeeld wordt de [OnMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_)-callback geconfigureerd met een [OnMessageOptions](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions)-object. [AutoComplete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoComplete) wordt ingesteld op **false** om handmatig beheer van het aanroepen van [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) in te schakelen voor het ontvangen bericht. [AutoRenewTimeout](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoRenewTimeout) is ingesteld op 1 minuut, zodat de client maximaal één minuut wacht voordat de functie voor automatisch verlengen wordt beëindigd en de client een nieuwe aanroep maakt om op berichten te controleren. Met deze eigenschapswaarde wordt het aantal keren gereduceerd dat de client toerekenbare aanroepen maakt waarmee geen berichten worden opgehaald.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing het bericht om de een of andere reden niet kan verwerken, kan de methode [Abandon](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon_System_Collections_Generic_IDictionary_System_String_System_Object__) voor het ontvangen bericht worden aangeroepen (in plaats van de methode [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Dit zorgt ervoor dat Service Bus het bericht in het abonnement ontgrendelt en het beschikbaar maakt om opnieuw te worden ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Daarnaast is er een time-out gekoppeld aan een bericht dat in het abonnement is vergrendeld. Als de toepassing het bericht niet kan verwerken voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), ontgrendelt Service Bus het bericht automatisch en wordt het beschikbaar gemaakt om opnieuw te worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de aanvraag [Complete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) is uitgegeven, wordt het bericht opnieuw bij de toepassing bezorgd wanneer de toepassing opnieuw wordt gestart. Dit wordt vaak *Ten minste eenmaal verwerken* genoemd; dat wil zeggen dat elk bericht ten minste één keer wordt verwerkt maar dat hetzelfde bericht in sommige situaties opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met de [MessageId](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId)-eigenschap van het bericht dat gelijk blijft bij meerdere bezorgingspogingen.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen
Het volgende voorbeeld laat zien hoe u het onderwerp **TestTopic** uit de **HowToSample**-servicenaamruimte verwijdert.

```csharp
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Als een onderwerp wordt verwijderd, worden ook alle abonnementen verwijderd die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code laat zien hoe u een abonnement met de naam **HighMessages** uit het **TestTopic**-onderwerp verwijdert.

```csharp
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Service Bus-onderwerpen en -abonnementen hebt geleerd, volgt u deze koppelingen voor meer informatie.

* [Wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions].
* [Voorbeeld onderwerpfilters][Topic filters sample]
* API-naslaginformatie voor [SqlFilter][SqlFilter].
* Bouw een werkende toepassing waarmee berichten naar en van een Service Bus-wachtrij worden verzonden en ontvangen: [zelfstudie Service Bus Brokered Messaging .NET][Service Bus brokered messaging .NET tutorial].
* Service Bus-voorbeelden: download van [Azure-voorbeelden][Azure samples] of raadpleeg het [overzicht](service-bus-samples.md).

[Azure portal]: https://portal.azure.com

[7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Topic filters sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
[SqlFilter]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sqlfilter
[SqlFilter.SqlExpression]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[Service Bus brokered messaging .NET tutorial]: service-bus-brokered-tutorial-dotnet.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2



<!--HONumber=Jan17_HO3-->


