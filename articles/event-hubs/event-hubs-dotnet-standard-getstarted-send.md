---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van .NET Standard | Microsoft Docs
description: Aan de slag met het verzenden van gebeurtenissen naar Event Hubs in .NET Standard
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 4cd2fdb2bd8b6a15bc8dc3e4594971a61e1889e7
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "41918619"
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Aan de slag met het verzenden van berichten naar Azure Event Hubs in .NET Standard

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Deze zelfstudie laat zien hoe u een .NET Core-consoletoepassing schrijft die een set berichten naar een event hub verzendt. U kunt de [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender)-oplossing ongewijzigd uitvoeren en de tekenreeksen `EhConnectionString` en `EhEntityPath` vervangen door uw event hubwaarden. Of u kunt de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio 2015 of 2017](http://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt gebruikgemaakt van Visual Studio 2017, maar Visual Studio 2015 wordt ook ondersteund.
* [.NET core Visual Studio 2015- of 2017-hulpprogramma's](http://www.microsoft.com/net/core).
* Een Azure-abonnement.
* [Een Event Hub-naamruimte en Event Hub](event-hubs-quickstart-portal.md).

In deze zelfstudie wordt Visual Studio gebruikt voor het schrijven van een C#-consoletoepassing om berichten naar een event hub te verzenden.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken

Volg de procedure in [dit artikel](event-hubs-quickstart-portal.md) en ga daarna verder met deze zelfstudie om een naamruimte en een Event Hub te maken en ga daarna verder met de volgende stappen.

## <a name="create-a-console-application"></a>Een consoletoepassing maken

Start Visual Studio. Klik in het menu **File** op **New** en klik vervolgens op **Project**. Maak een .NET Core-consoletoepassing.

![Nieuw project][1]

## <a name="add-the-event-hubs-nuget-package"></a>Het Event Hubs NuGet-pakket toevoegen

Voeg de [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard-bibliotheek NuGet-pakket toe aan uw project via de volgende stappen: 

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op het tabblad **Bladeren**, zoek naar Microsoft.Azure.EventHubs en selecteer het pakket **Microsoft.Azure.EventHubs**. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Code schrijven om berichten te verzenden naar de event hub

1. Voeg aan het begin van het bestand Program.cs de volgende `using`-instructies toe:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Voeg constanten toe aan de `Program`-klasse voor de Event Hubs-verbindingsreeks en entiteitspad (afzonderlijke event hub-naam). Vervang de tijdelijke aanduidingen tussen punthaken door de juiste waarden die zijn verkregen bij het maken van de event hub. Zorg ervoor dat `{Event Hubs connection string}` de verbindingsreeks op naamruimteniveau is en niet de event hub-tekenreeks. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Voeg als volgt een nieuwe methode met de naam `MainAsync` toe aan de klasse `Program`:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Voeg als volgt een nieuwe methode met de naam `SendMessagesToEventHub` toe aan de klasse `Program`:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Voeg de volgende code toe aan de methode `Main` in de klasse `Program`:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Zo zou het bestand Program.cs er moeten uitzien.

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
                {
                    EntityPath = EventHubName
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Voer het programma uit en controleer of er geen fouten zijn.

Gefeliciteerd. U hebt nu berichten verzonden naar een Event Hub.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Gebeurtenissen ontvangen van Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png
