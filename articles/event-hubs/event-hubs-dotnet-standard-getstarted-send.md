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
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: 633d29d3e2e8a8ab0b746549f126ad45ea781d6e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227889"
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Aan de slag met het verzenden van berichten naar Azure Event Hubs in .NET Standard
Event Hubs is een service die grote hoeveelheden gebeurtenisgegevens (telemetrie) van verbonden apparaten en toepassingen verwerkt. Nadat u gegevens in Event Hubs hebt verzameld, kunt u de gegevens opslaan met behulp van een opslagcluster of transformeren met een provider van realtime-analyses. Deze functie voor grootschalige gebeurtenisverzameling en -verwerking is een belangrijk onderdeel van de architectuur van moderne toepassingen, met inbegrip van het Internet der dingen (IoT). Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

Deze zelfstudie laat zien hoe u met behulp van .NET Core gebeurtenissen naar een event hub verzendt via een in C# geschreven consoletoepassing. 

> [!NOTE]
> U kunt deze snelstart als voorbeeld downloaden van de [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender), de tekenreeksen `EventHubConnectionString` en `EventHubName` vervangen door uw event hub-waarden en deze uitvoeren. U kunt ook de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten
* [Microsoft Visual Studio 2015 of 2017](https://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt gebruikgemaakt van Visual Studio 2017, maar Visual Studio 2015 wordt ook ondersteund.
* [.NET core Visual Studio 2015- of 2017-hulpprogramma's](https://www.microsoft.com/net/core). 

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en een Event Hub te maken en ga daarna verder met de volgende stappen in deze zelfstudie.

## <a name="create-a-console-application"></a>Een consoletoepassing maken

Start Visual Studio. Klik in het menu **File** op **New** en klik vervolgens op **Project**. Maak een .NET Core-consoletoepassing.

![Nieuw project][1]

## <a name="add-the-event-hubs-nuget-package"></a>Het Event Hubs NuGet-pakket toevoegen

Voeg de [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard-bibliotheek NuGet-pakket toe aan uw project via de volgende stappen: 

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op het tabblad **Bladeren**, zoek naar Microsoft.Azure.EventHubs en selecteer het pakket **Microsoft.Azure.EventHubs**. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Code schrijven om berichten te verzenden naar de event hub

1. Voeg aan het begin van het bestand Program.cs de volgende `using`-instructies toe:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Voeg constanten toe aan de `Program`-klasse voor de Event Hubs-verbindingsreeks en entiteitspad (afzonderlijke event hub-naam). Vervang de tijdelijke aanduidingen tussen punthaken door de juiste waarden die zijn verkregen bij het maken van de event hub. Zorg ervoor dat `{Event Hubs connection string}` de verbindingsreeks op naamruimteniveau is en niet de event hub-tekenreeks. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    ```

3. Voeg als volgt een nieuwe methode met de naam `MainAsync` toe aan de klasse `Program`:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
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
In deze snelstart hebt u berichten verzonden naar een event hub met behulp van .NET Standard. Zie [Gebeurtenissen ontvangen uit event hub - .NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md) voor informatie over het ontvangen van gebeurtenissen uit een event hub met behulp van .NET Standard.

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png
