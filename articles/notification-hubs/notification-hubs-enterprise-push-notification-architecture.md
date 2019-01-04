---
title: Notification Hubs - architectuur van de Enterprise-Push
description: Informatie over het gebruik van Azure Notification Hubs in een bedrijfsomgeving
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 1c9161f6d31a3fcff8f8926c8bf188f1bdc14799
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725850"
---
# <a name="enterprise-push-architectural-guidance"></a>Hulp voor architectuur via pushmeldingen van het bedrijf
Ondernemingen zijn tegenwoordig geleidelijk verplaatsen naar het maken van mobiele toepassingen voor hun eindgebruikers (extern) of voor de werknemers (intern). Ze hebben bestaande back-endsystemen erin mainframes of bepaalde LoB-toepassingen moeten worden geïntegreerd in de architectuur van de mobiele toepassing. Deze handleiding wordt besproken hoe best kunt uitvoeren van deze integratie aanbeveelt van mogelijke oplossing voor algemene scenario's.

Een frequente vereiste is voor het verzenden van pushmeldingen aan de gebruikers via hun mobiele toepassing wanneer een bepaalde gebeurtenis in de back-endsystemen plaatsvindt. bijvoorbeeld, wil een klant van de bank die van de bank bankieren app op een iPhone heeft worden gewaarschuwd wanneer er een betaalpas boven een bepaalde hoeveelheid van het account of een intranet-scenario waarbij een werknemer van de afdeling Financiën met een budget goedkeuring-app op een Windows Phone wil wordt gedaan  om te worden geïnformeerd wanneer de goedkeuringsaanvraag wordt ontvangen.

De bank-account of de verwerking van de goedkeuring is waarschijnlijk worden uitgevoerd in sommige endsysteem back-voor een push naar de gebruiker moet starten. Mogelijk zijn er meerdere dergelijke back endsystemen, moeten alle dezelfde soort logica voor het pushen wanneer een gebeurtenis een melding activeert maken. De complexiteit hier ligt in verschillende back-endsystemen, samen met een één push-systeem waar de eindgebruikers mogelijk andere meldingen bent geabonneerd en er kan zelfs meerdere mobiele toepassingen integreren. Bijvoorbeeld intranet mobiele apps waarbij een mobiele toepassing kan meldingen wilt ontvangen uit meerdere endsystemen die dergelijke back. De back-endsystemen niet kent of wilt weten van push-semantiek/technologie, zodat een gangbare oplossing hier traditioneel is geweest om te introduceren een onderdeel, dat de back-endsystemen voor alle gebeurtenissen van belang zijn worden opgevraagd en is verantwoordelijk voor het verzenden van de pushberichten naar de -client.

Een betere oplossing is het gebruik van Azure Service Bus - onderwerp/abonnement model, waardoor de complexiteit bij het maken van de oplossing schaalbaar.

Hier volgt de algemene architectuur van de oplossing (gegeneraliseerde met meerdere mobiele apps maar evenveel van toepassing wanneer er slechts één mobiele app)

## <a name="architecture"></a>Architectuur
![][1]

Het belangrijkste gedeelte in dit Architectuurdiagram is Azure Service Bus, waarmee u een onderwerpen/abonnementen programmeermodel (meer hierover op [Service Bus Pub/Sub-programmering]). De ontvanger, die in dit geval de mobiele back-end is (meestal [Mobiele Service van Azure], die een push naar de mobiele apps initieert) ontvangt geen berichten rechtstreeks vanuit de back-endsystemen, maar in plaats daarvan een tussenliggende abstractie laag geleverd door de [Azure Service Bus, waarmee mobiele back-end om berichten te ontvangen van een of meer back-endsystemen. Een Service Bus-onderwerp moet worden gemaakt voor elk van de back endsystemen, bijvoorbeeld-Account, HR, financiën, dit is in feite 'onderwerpen"van belang, waarmee berichten worden verzonden als pushmelding wordt geïnitieerd. De back-endsystemen versturen berichten naar deze onderwerpen. Een mobiele back-end kunt abonneren op een of meer deze onderwerpen door een Service Bus-abonnement te maken. Het recht geeft de mobiele back-end die een melding ontvangen van de bijbehorende back endsysteem. Mobiele back-end blijft om te luisteren naar berichten op hun abonnementen en zodra een bericht binnenkomt, weer wordt ingeschakeld en verzendt deze melding met de notification hub. Notification hubs vervolgens uiteindelijk het bericht verzenden naar de mobiele app. Hier volgt de lijst met belangrijke onderdelen:

1. Back-endsystemen (LoB/oudere systemen)
   * Hiermee maakt u Service Bus-onderwerp
   * Bericht verzenden
1. Mobiele back-end
   * Hiermee maakt u serviceabonnement
   * Bericht ontvangt (van back-end-systeem)
   * Wordt een melding verzonden naar clients (via Azure Notification Hub)
1. Mobile Application
   * Ontvangt en een melding weergeven

### <a name="benefits"></a>Voordelen:
1. De ontkoppeling tussen de ontvanger (mobiele app/service via Notification Hub) en de afzender (back-endsystemen), kunt aanvullende back endsystemen wordt geïntegreerd met minimale wijzigen.
1. Ook kan het scenario van meerdere mobiele apps geen gebeurtenissen ontvangen van een of meer back-endsystemen.  

## <a name="sample"></a>Voorbeeld:
### <a name="prerequisites"></a>Vereisten
De volgende zelfstudies om vertrouwd met de concepten, evenals de algemene stappen voor het maken en de configuratie te voltooien:

1. [Service Bus Pub/Sub-programmering] -in deze zelfstudie wordt uitgelegd dat de details van het werken met Service Bus-onderwerpen/abonnementen, het maken van een naamruimte voor het bevatten van onderwerpen/abonnementen, het verzenden en ontvangen van berichten van deze.
1. [Notification Hubs - zelfstudie voor Windows Universal] -in deze zelfstudie wordt uitgelegd hoe u instellen van een Windows Store-app en Notification Hubs gebruiken om te registreren en vervolgens ontvangen van meldingen.

### <a name="sample-code"></a>Voorbeeldcode
De volledige voorbeeldcode is beschikbaar op [Notification Hub-voorbeelden]. Dit wordt verdeeld in drie onderdelen:

1. **EnterprisePushBackendSystem**
   
    a. Dit project gebruikt de *WindowsAzure.ServiceBus* NuGet-pakket en zijn gebaseerd op [Service Bus Pub/Sub-programmering].
   
    b. Deze toepassing is een eenvoudig C#-consoletoepassing voor het simuleren van een LoB-systeem, die het bericht moet worden geleverd aan de mobiele app initieert.
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the topic
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    c. `CreateTopic` wordt gebruikt voor het maken van de Service Bus-onderwerp.
   
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
   
    d. `SendMessage` wordt gebruikt om de berichten verzenden naar deze Service Bus-onderwerp. Deze code verzendt gewoon een set willekeurige berichten naar het onderwerp periodiek ten behoeve van het voorbeeld. Normaal gesproken is er een back endsysteem, die berichten worden verzonden wanneer een gebeurtenis plaatsvindt.
   
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
1. **ReceiveAndSendNotification**
   
    a. Dit project gebruikt de *WindowsAzure.ServiceBus* en *Microsoft.Web.WebJobs.Publish* NuGet-pakketten en zijn gebaseerd op [Service Bus Pub/Sub-programmering].
   
    b. De volgende console-app wordt uitgevoerd als een [Azure-webtaak] omdat er continu worden uitgevoerd om te luisteren naar berichten van de LoB/back-endsystemen. Deze toepassing maakt deel uit van uw mobiele back-end.
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the subscription that receives messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    c. `CreateSubscription` wordt gebruikt om te maken van een Service Bus-abonnement voor het onderwerp waarin de back-end verzendt het afdruksysteem van berichten. Afhankelijk van het bedrijfsscenario dit onderdeel maakt u een of meer abonnementen op de bijbehorende onderwerpen (bijvoorbeeld enkele kunnen worden ontvangen van berichten van HR-systeem, sommige vanuit system financiën, enzovoort)
   
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
   
    d. ReceiveMessageAndSendNotification wordt gebruikt voor het lezen van het bericht van het onderwerp met behulp van het abonnement en als de lezen slaagt vervolgens een melding (in het voorbeeldscenario een systeemeigen pop-upmelding voor Windows) beschikbaar stellen om te worden verzonden naar de mobiele toepassing met behulp van Azure Notification Hubs.
   
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
   
    e. Voor het publiceren van deze app als een **webtaak**, klik met de rechtermuisknop op de oplossing in Visual Studio en selecteer **publiceren als WebJob**
   
    ![][2]
   
    f. Selecteer uw publicatieprofiel en een nieuwe Azure-WebSite maken als deze nog niet bestaat, die als host fungeert voor deze WebJob en beschikt u over de WebSite vervolgens **publiceren**.
   
    ![][3]
   
    g. Configureren van de taak voor het "Continu uitvoeren" zo dat wanneer u zich aanmeldt bij de [Azure Portal] ziet dit er ongeveer als volgt:
   
    ![][4]
1. **EnterprisePushMobileApp**
   
    a. Deze toepassing is een Windows Store-toepassing die pop-upmeldingen uit de webtaak uitgevoerd als onderdeel van uw mobiele back-end ontvangt en weer te geven. Deze code is gebaseerd op [Notification Hubs - zelfstudie voor Windows Universal].  
   
    b. Zorg ervoor dat uw toepassing is ingeschakeld voor het ontvangen van pop-upmeldingen.
   
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

### <a name="running-sample"></a>Voorbeeld uitvoert:
1. Zorg ervoor dat de webtaak correct wordt uitgevoerd en gepland om te worden continu worden uitgevoerd.
1. Voer de ** EnterprisePushMobileApp, waarmee de Windows Store-app wordt gestart.
1. Voer de **EnterprisePushBackendSystem** consoletoepassing, deze simuleert de LoB back-end en begint met het verzenden van berichten en ziet u pop-upmeldingen weergegeven zoals in de volgende afbeelding:
   
    ![][5]
1. De berichten zijn oorspronkelijk verzonden naar Service Bus-onderwerpen die werd bewaakt door Service Bus-abonnementen in uw Web-Job. Zodra een bericht is ontvangen, is een melding gemaakt en verzonden naar de mobiele app. U kunt zoeken via de WebJob-Logboeken om te bevestigen van de verwerking wanneer u gaat u naar de Logs-koppeling in [Azure Portal] voor uw Web-Project:
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Notification Hub-voorbeelden]: https://github.com/Azure/azure-notificationhubs-samples
[Mobiele Service van Azure]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub-programmering]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure-webtaak]: ../app-service/webjobs-create.md
[Notification Hubs - zelfstudie voor Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure Portal]: https://portal.azure.com/
