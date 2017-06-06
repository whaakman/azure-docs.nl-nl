---
title: Een programma schrijven dat gebruikmaakt van Azure Service Bus-wachtrijen | Microsoft Docs
description: Een C#-consoletoepassing schrijven voor Service Bus-berichten
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/23/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: 83649bdad1d369cdfe4edf3c2bdaa67180db8668
ms.contentlocale: nl-nl
ms.lasthandoff: 03/01/2017


---
# <a name="get-started-with-service-bus-queues"></a>Aan de slag met Service Bus-wachtrijen
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Wat wordt bereikt
In deze zelfstudie voltooien we het volgende:

1. Een Service Bus-naamruimte maken met de Azure-portal.
2. Een Service Bus-berichtenwachtrij maken met de Azure-portal.
3. Een consoletoepassing schrijven om een bericht te verzenden.
4. Een consoletoepassing schrijven om berichten te ontvangen.

## <a name="prerequisites"></a>Vereisten
1. [Visual Studio 2015 of hoger](http://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2015 gebruikt.
2. Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Een naamruimte maken met de Azure-portal
Als u al een Service Bus-naamruimte hebt gemaakt, gaat u naar het gedeelte [Een wachtrij maken met de Azure Portal](#2-create-a-queue-using-the-azure-portal)

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Een wachtrij maken met de Azure-portal
Als u al een Service Bus-wachtrij hebt gemaakt, gaat u naar het gedeelte [Berichten naar de wachtrij verzenden](#3-send-messages-to-the-queue).

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Berichten naar de wachtrij verzenden
We maken een C#-consoletoepassing met Visual Studio om berichten naar de wachtrij te verzenden.

### <a name="create-a-console-application"></a>Een consoletoepassing maken

- Start Visual Studio en maak een nieuwe consoletoepassing.

### <a name="add-the-service-bus-nuget-package"></a>Het Service Bus NuGet-pakket toevoegen
1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op het tabblad **Bladeren**, zoek naar Microsoft Azure Service Bus en selecteer het item **Microsoft Azure Service Bus**. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.
   
    ![Een NuGet-pakket selecteren][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Code schrijven om een bericht naar de wachtrij te verzenden
1. Voeg de volgende using-instructie toe aan het begin van het bestand Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Voeg de volgende code toe aan de methode `Main`, stel de variabele **connectionString** in als de verbindingstekenreeks die is verkregen tijdens het maken van de naamruimte en stel **queueName** in als de wachtrijnaam die is gebruikt bij het maken van de wachtrij.
   
    ```csharp
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```
   
    Zo zou het bestand Program.cs er moeten uitzien.
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";
   
                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");
   
                client.Send(message);
            }
        }
    }
    ```
3. Voer het programma uit en ga naar Azure Portal. Klik op de naam van uw wachtrij in de blade **Overzicht** voor de naamruimte. De waarde voor **Aantal actieve berichten** moet nu 1 zijn.
   
      ![Aantal berichten][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Berichten ontvangen uit de wachtrij
1. Maak een nieuwe consoletoepassing en voeg een verwijzing toe naar het Service Bus-pakket NuGet, zoals ook met de voorgaande verzendtoepassing is gedaan.
2. Voeg boven in het bestand Program.cs de volgende `using`-instructie toe.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Voeg de volgende code toe aan de methode `Main`, stel de variable **connectionString** in als de verbindingstekenreeks die is verkregen tijdens het maken van de naamruimte en stel **queueName** in als de wachtrijnaam die u hebt gebruikt bij het maken van de wachtrij.
   
    ```csharp
    var connectionString = "";
    var queueName = "samplequeue";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.ReadLine();
    ```
   
    Zo zou het bestand Program.cs er moeten uitzien:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "";
          var queueName = "samplequeue";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });
   
          Console.ReadLine();
        }
      }
    }
    ```
4. Voer het programma uit en controleer de portal. Merk op dat de waarde voor **Wachtrijlengte** nu 0 moet zijn.
   
    ![Wachtrijlengte][queue-message-receive]

Gefeliciteerd. U hebt nu een wachtrij gemaakt, een bericht verzonden en een bericht ontvangen.

## <a name="next-steps"></a>Volgende stappen
Bekijk onze [GitHub-opslagplaats met voorbeelden](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) die enkele van de meer geavanceerde functies van Azure Service Bus-berichten laten zien.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

