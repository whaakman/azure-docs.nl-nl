---
title: Notification Hubs - Push ondernemingsstructuur
description: Richtlijnen over het gebruik van Azure Notification Hubs in een bedrijfsomgeving
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c626d6415a27f8495304eeaab480ab62606102ea
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="enterprise-push-architectural-guidance"></a>Hulp voor architectuur via pushmeldingen van het bedrijf
Ondernemingen zijn tegenwoordig geleidelijk verplaatsen naar het maken van mobiele toepassingen voor hun eindgebruikers (extern) of voor de werknemers (intern). Ze hebben bestaande back-endsystemen mainframes of sommige LoB-toepassingen die moeten worden geïntegreerd in de architectuur van de mobiele toepassing. Deze handleiding wordt hebben over de beste manier om u te doen van deze integratie mogelijke oplossing aanbevelen voor het algemene scenario's.

Een frequente vereiste is voor het verzenden van pushmeldingen aan de gebruikers via hun mobiele toepassing als een gebeurtenis van belang zijn in de back endsystemen. Bijvoorbeeld een klant bank die de bank-app voor bankieren op haar iPhone wil worden gewaarschuwd wanneer er een incasso wordt gedaan boven een bepaald bedrag van haar account of een intranet-scenario waarin een medewerker van de afdeling Financiën die een budget goedkeuring app op zijn Windows Phone wil worden gewaarschuwd wanneer hij een goedkeuringsaanvraag opgehaald.

De bank- of goedkeuring verwerking is het waarschijnlijk worden uitgevoerd in sommige back endsysteem dat een push naar de gebruiker moet starten. Er zijn meerdere dergelijke back voor endsystemen die u moeten alle dezelfde soort logica voor het implementeren van push wanneer een gebeurtenis wordt geactiveerd een melding maken. De complexiteit hier ligt in het integreren van verschillende back endsystemen samen met een enkele push-systeem waar de eindgebruikers mogelijk andere meldingen bent geabonneerd en er kan zelfs meerdere mobiele toepassingen bijvoorbeeld in het geval van mobiele apps intranet waar een mobiele toepassing kan meldingen wilt ontvangen van meerdere dergelijke back endsystemen. De back endsystemen niet kent of moet weten van push-semantiek/technologie zodat een gangbare oplossing hier is normaal gesproken in te voeren van een onderdeel dat de back endsystemen voor gebeurtenissen die van belang worden opgevraagd en is verantwoordelijk voor de pushberichten verzenden naar de client.
Er wordt hier hebben over een nog beter oplossing met behulp van Azure Service Bus - onderwerp/abonnement model waarmee de complexiteit tijdens het maken van de oplossing schaalbaar wordt beperkt.

Hier volgt de algemene architectuur van de oplossing (gegeneraliseerde met meerdere mobiele apps maar evenveel van toepassing wanneer er slechts één van de mobiele app)

## <a name="architecture"></a>Architectuur
![][1]

De sleutel in deze Architectuurdiagram is Azure Service Bus waarmee een onderwerpen/de abonnementen programmeermodel (meer informatie over op het [Service Bus Pub subitems programmering]). De ontvanger, die in dit geval de mobiele back-end wordt (meestal [Azure Mobile Service], gang een push naar de mobiele apps) niet ontvangen berichten rechtstreeks vanuit de back endsystemen, maar in plaats daarvan hebben we een tussenliggende abstractielaag geleverd door [Azure Service Bus] waardoor mobiele back-end om berichten te ontvangen van een of meer back endsystemen. Een Service Bus-onderwerp moet worden gemaakt voor elk van de back-endsystemen bijvoorbeeld Account HR, financiële die eigenlijk 'onderwerpen' van belang dat berichten worden verzonden als de pushmelding wordt geïnitieerd. De back endsystemen zal berichten verzenden naar deze onderwerpen. Een back-end van Mobile kunnen zich abonneren op een of meer van deze onderwerpen door het maken van een Service Bus-abonnement. Dit recht geven de mobiele back-end een melding te ontvangen van de bijbehorende back endsysteem. Back-end voor mobiele blijft om te luisteren naar berichten op hun abonnementen en zodra er een bericht binnenkomt, schakelt terug en verzendt het als melding met de notification hub. Notification hubs uiteindelijk bezorgt het bericht vervolgens naar de mobiele app. Om samen te vatten de hoofdonderdelen, hebben we dus:

1. Back-end-systemen (LoB/oudere systemen)
   * Service Bus-onderwerp maakt
   * Bericht
2. Mobiele back-end
   * Service-abonnement wordt gemaakt
   * Ontvangen van bericht (van de back-end-systeem)
   * Wordt een melding verzonden naar clients (via Azure Notification Hub)
3. Mobile Application
   * Ontvangt en meldingen weergeven

### <a name="benefits"></a>Voordelen:
1. Het ontkoppelen tussen de ontvanger (mobiele app/service via Notification Hub) en de afzender (back endsystemen), kunt aanvullende back endsystemen worden geïntegreerd met minimale wijzigen.
2. Hierdoor wordt het scenario met meerdere mobiele apps kunnen worden ontvangen van gebeurtenissen van een of meer back endsystemen.  

## <a name="sample"></a>Voorbeeld:
### <a name="prerequisites"></a>Vereisten
U moet de volgende zelfstudies om vertrouwd met de concepten, evenals het maken van algemene & configuratiestappen te voltooien:

1. [Service Bus Pub subitems programmering] -Hier wordt uitgelegd hoe de details van het werken met Service Bus onderwerpen/abonnementen, het maken van een naamruimte onderwerpen/abonnementen bevatten, hoe u berichten ontvangen van deze & verzenden.
2. [Notification Hubs - universele Windows-zelfstudie] -procedure voor het instellen van een Windows Store-app en Notification Hubs gebruiken om te registreren en vervolgens meldingen ontvangen.

### <a name="sample-code"></a>Voorbeeldcode
De code voor het volledige voorbeeld is beschikbaar op [Notification Hub voorbeelden]. Deze is opgesplitst in drie onderdelen:

1. **EnterprisePushBackendSystem**
   
    a. Dit project gebruikt de *WindowsAzure.ServiceBus* Nuget-pakket en is gebaseerd op [Service Bus Pub subitems programmering].
   
    b. Dit is een eenvoudige C#-consoletoepassing te simuleren van een LoB-systeem waarvan het bericht moet worden geleverd aan de mobiele app initieert.
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    c. `CreateTopic`wordt gebruikt voor het maken van de Service Bus-onderwerp waarin berichten worden verzonden.
   
        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    d. `SendMessage`wordt gebruikt om de berichten verzenden naar deze Service Bus-onderwerp. We zijn een reeks willekeurige berichten hier alleen het verzenden naar het onderwerp periodiek omwille van de steekproef. Normaal gesproken zal er een back endsysteem dat berichten verzenden zal wanneer een gebeurtenis plaatsvindt.
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };
   
            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);
   
                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);
   
                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);
   
                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }
2. **ReceiveAndSendNotification**
   
    a. Dit project gebruikt de *WindowsAzure.ServiceBus* en *Microsoft.Web.WebJobs.Publish* Nuget-pakketten en is gebaseerd op [Service Bus Pub subitems programmering].
   
    b. Dit is een andere C#-consoletoepassing die wordt uitgevoerd als een [Azure webtaak] omdat er continu uitvoeren om te luisteren naar berichten van de LoB-/ back-end-systemen. Dit zal onderdeel zijn van uw mobiele back-end.
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    c. `CreateSubscription`wordt gebruikt voor het maken van een Service Bus-abonnement voor het onderwerp waarin de back endsysteem verzenden. Dit onderdeel maakt, afhankelijk van het bedrijfsscenario een of meer abonnementen op de bijbehorende onderwerpen (bv. Sommige kan worden ontvangen van berichten van HR-systeem, sommige vanuit Financiën system, enzovoort)
   
        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    d. ReceiveMessageAndSendNotification wordt gebruikt voor het lezen van het bericht van het onderwerp met behulp van het abonnement en als het lezen slaagt vervolgens een melding (in het voorbeeldscenario voor een Windows-systeemeigen pop-upmelding) stellen om te worden verzonden naar de mobiele toepassingen met Azure Notification Hubs.
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";
   
                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");
   
                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);
   
                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }
   
    e. Voor het publiceren van dit als een **webtaak**, klik met de rechtermuisknop op de oplossing in Visual Studio en selecteer **publiceren als webtaak**
   
    ![][2]
   
    f. Selecteer uw publicatieprofiel en als deze niet al host die deze webtaak en zodra u de WebSite klikt u vervolgens een nieuwe Azure-WebSite maken **publiceren**.
   
    ![][3]
   
    g. Configureren van de taak voor het 'Continu uitvoeren' zo dat wanneer u zich aanmelden bij de [Azure-portal] ziet er ongeveer als volgt:
   
    ![][4]
3. **EnterprisePushMobileApp**
   
    a. Dit is een Windows Store-toepassing die wordt ontvangen pop-upmeldingen uit de webtaak uitgevoerd als onderdeel van uw mobiele back-end en weer te geven. Dit is gebaseerd op [Notification Hubs - universele Windows-zelfstudie].  
   
    b. Zorg ervoor dat uw toepassing is ingeschakeld voor het pop-upmeldingen te ontvangen.
   
    c. Zorg ervoor dat de volgende Notification Hubs registratiecode wordt aangeroepen op de App gestart (na het vervangen van de *HubName* en *DefaultListenSharedAccessSignature*:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Voorbeeld uitgevoerd:
1. Zorg ervoor dat uw webtaak correct wordt uitgevoerd en wordt 'Continu uitvoeren'.
2. Voer de **EnterprisePushMobileApp** die de Windows Store-app wordt gestart.
3. Voer de **EnterprisePushBackendSystem** consoletoepassing die de LoB-back-end simuleert en wordt gestart met het verzenden van berichten en ziet u pop-upmeldingen verschijnen als volgt:
   
    ![][5]
4. De berichten werden oorspronkelijk is verzonden naar Service Bus-onderwerpen die werd bewaakt door Service Bus-abonnementen in uw Web-taak. Wanneer een bericht is ontvangen, is een melding gemaakt en verzonden naar de mobiele app. U kunt zoeken via de webtaak Logboeken om te bevestigen dat de verwerking wanneer u gaat de koppeling om de logboeken in [Azure-portal] voor uw Web-taak:
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Notification Hub voorbeelden]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub subitems programmering]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure webtaak]: ../app-service/web-sites-create-web-jobs.md
[Notification Hubs - universele Windows-zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure-portal]: https://portal.azure.com/
