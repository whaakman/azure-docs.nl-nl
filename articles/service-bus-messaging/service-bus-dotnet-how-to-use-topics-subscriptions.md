---
title: Aan de slag met Azure Service Bus-onderwerpen en -abonnementen | Microsoft Docs
description: Een C#-consoletoepassing schrijven die Service Bus Messaging-onderwerpen en -abonnementen gebruikt.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 9401ada519f600b0d2817f06a396e16607a24129
ms.contentlocale: nl-nl
ms.lasthandoff: 07/01/2017


---
<a id="get-started-with-service-bus-topics" class="xliff"></a>

# Aan de slag met Service Bus-onderwerpen

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<a id="what-will-be-accomplished" class="xliff"></a>

## Wat wordt bereikt

Deze zelfstudie bestaat uit de volgende stappen:

1. Een Service Bus-naamruimte maken met de Azure-portal.
2. Een Service Bus-onderwerp maken met de Azure-portal.
3. Een Service Bus-abonnement maken met de Azure-portal.
4. Een consoletoepassing schrijven om een bericht te verzenden naar het onderwerp.
5. Een consoletoepassing schrijven om dit bericht te ontvangen met het abonnement.

<a id="prerequisites" class="xliff"></a>

## Vereisten

1. [Visual Studio 2015 of hoger](http://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2017 gebruikt.
2. Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<a id="1-create-a-namespace-using-the-azure-portal" class="xliff"></a>

## 1. Een naamruimte maken met de Azure-portal

Als u al een Service Bus Messaging-naamruimte hebt gemaakt, gaat u naar het gedeelte [Een onderwerp maken met de Azure-portal](#2-create-a-topic-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

<a id="2-create-a-topic-using-the-azure-portal" class="xliff"></a>

## 2. Een onderwerp maken met de Azure-portal

1. Meld u aan bij [Azure Portal][azure-portal].
2. Klik in het navigatiedeelvenster links van de portal op **Service Bus** (als u geen **Service Bus** ziet, klik dan op **Meer services**).
3. Klik op de naamruimte waarin u het onderwerp wilt maken. De overzichtsblade voor de naamruimte wordt weergegeven:
   
    ![Een onderwerp maken][createtopic1]
4. Klik in de blade **Service Bus-naamruimte** op **Onderwerpen** en klik vervolgens op **Onderwerp toevoegen**.
   
    ![Onderwerpen selecteren][createtopic2]
5. Voer een naam voor het onderwerp in en schakel de optie **Partitionering inschakelen** in. Houd voor de overige opties de standaardwaarden aan.
   
    ![Selecteer Nieuw][createtopic3]
6. Klik onder aan de blade op **Maken**.

<a id="3-create-a-subscription-to-the-topic" class="xliff"></a>

## 3. Een abonnement op het onderwerp maken

1. In het resourcesdeelvenster van de portal klik u op de naamruimte die u in stap 1 hebt gemaakt en klikt u op de naam van het onderwerp dat u in stap 2 hebt gemaakt.
2. Klik aan de bovenkant van het overzichtsvenster op het plusteken (+) naast **Abonnement** om een abonnement toe te voegen aan dit onderwerp.

    ![Abonnement maken][createtopic4]

3. Voer een naam in voor het abonnement. Houd voor de overige opties de standaardwaarden aan.

<a id="4-send-messages-to-the-topic" class="xliff"></a>

## 4. Berichten naar het onderwerp verzenden

We maken een C#-consoletoepassing met Visual Studio om berichten naar het onderwerp te verzenden.

<a id="create-a-console-application" class="xliff"></a>

### Een consoletoepassing maken

Start Visual Studio en maak een nieuwe **consoletoepassing (.NET Framework)**.

<a id="add-the-service-bus-nuget-package" class="xliff"></a>

### Het Service Bus NuGet-pakket toevoegen

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op het tabblad **Bladeren**, zoek naar **Microsoft Azure Service Bus** en selecteer het item **WindowsAzure.ServiceBus**. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.
   
    ![Een NuGet-pakket selecteren][nuget-pkg]

<a id="write-some-code-to-send-a-message-to-the-topic" class="xliff"></a>

### Code schrijven om een bericht naar het onderwerp te verzenden

1. Voeg boven in het bestand Program.cs de volgende `using`-instructie toe.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Voeg de volgende code toe aan de methode `Main`. Stel de variabele `connectionString` in als de verbindingstekenreeks die is verkregen tijdens het maken van de naamruimte en stel `topicName` in als de naam die u hebt gebruikt bij het maken van het onderwerp.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Zo zou het bestand Program.cs er moeten uitzien.
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Voer het programma uit en controleer de Azure-portal: klik op de naam van uw onderwerp in de blade **Overzicht** voor de naamruimte. De blade **Essentials** voor het onderwerp wordt weergegeven. In de lijst met abonnementen onderaan de blade ziet u dat de waarde voor **Aantal berichten** voor elk abonnement nu 1 is worden. Telkens wanneer die u de zendtoepassing uitvoert zonder de berichten op te halen (zoals in het volgende gedeelte wordt beschreven), wordt deze waarde verhoogd met 1. U zult ook zien dat de huidige grootte voor het onderwerp de waarde voor **Huidige** op de blade **Essentials** vergroot telkens wanneer de app een bericht toevoegt aan het onderwerp/abonnement.
   
      ![Berichtgrootte][topic-message]

<a id="5-receive-messages-from-the-subscription" class="xliff"></a>

## 5. Berichten ontvangen van het abonnement

1. Als u de berichten wilt ontvangen die u zojuist hebt verzonden, maakt u een nieuwe consoletoepassing en voegt u een verwijzing toe naar het Service Bus-pakket NuGet, zoals ook met de voorgaande zendtoepassing is gedaan.
2. Voeg boven in het bestand Program.cs de volgende `using`-instructie toe.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Voeg de volgende code toe aan de methode `Main`. Stel de variabele `connectionString` in als de verbindingstekenreeks die is verkregen tijdens het maken van de naamruimte en stel `topicName` in als de naam die u hebt gebruikt bij het maken van het onderwerp.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Zo zou het bestand Program.cs er moeten uitzien:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Voer het programma uit en controleer de portal opnieuw. De waarden voor **Aantal berichten** en **Huidige** moeten nu 0 zijn.
   
    ![Lengte van het onderwerp][topic-message-receive]

Gefeliciteerd. U hebt nu een onderwerp en een abonnement gemaakt, een bericht verzonden en dat bericht ontvangen.

<a id="next-steps" class="xliff"></a>

## Volgende stappen

Bekijk onze [GitHub-opslagplaats met voorbeelden](https://github.com/Azure/azure-service-bus/tree/master/samples) die enkele van de meer geavanceerde functies van Service Bus Messaging laten zien.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com

