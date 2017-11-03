---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van standaard .NET | Microsoft Docs
description: Gebeurtenissen verzenden naar Event Hubs in .NET standaard aan de slag
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: sethm
ms.openlocfilehash: 5cf01580b53b551064a46282b9005ade6afe9604
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Verzenden van berichten naar Azure Event Hubs in .NET standaard aan de slag

> [!NOTE]
> Dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Deze zelfstudie laat zien hoe u schrijft een consoletoepassing .NET Core dat een set berichten naar een event hub verzendt. U kunt uitvoeren de [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) oplossing-, vervangt de `EhConnectionString` en `EhEntityPath` tekenreeksen met de waarden van uw event hub. Of u kunt de stappen in deze zelfstudie voor het maken van uw eigen.

## <a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio 2015 of 2017](http://www.visualstudio.com). De voorbeelden in deze zelfstudie Gebruik Visual Studio 2017, maar de Visual Studio 2015 wordt ook ondersteund.
* [.NET core Visual Studio 2015 of hulpprogramma's voor 2017](http://www.microsoft.com/net/core).
* Een Azure-abonnement.
* Een event hub-naamruimte.

Om berichten te verzenden naar een event hub, zullen Visual Studio gebruikt voor het schrijven van een C#-consoletoepassing.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken

De eerste stap is het gebruik van de [Azure-portal](https://portal.azure.com) voor het maken van een naamruimte voor het gebeurtenistype hub en beheerreferenties die uw toepassing moet communiceren met de event hub. Volg de procedure in voor het maken van een naamruimte en een event hub, [in dit artikel](event-hubs-create.md), en vervolgens doorgaan met de volgende stappen uit.

## <a name="create-a-console-application"></a>Een consoletoepassing maken

Start Visual Studio. Klik in het menu **File** op **New** en klik vervolgens op **Project**. Maak een consoletoepassing .NET Core.

![Nieuw project][1]

## <a name="add-the-event-hubs-nuget-package"></a>Toevoegen van het Event Hubs NuGet-pakket

Voeg de [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) standaard .NET-bibliotheek NuGet-pakket aan uw project met de volgende stappen: 

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op de **Bladeren** tabblad, zoekt u naar 'Microsoft.Azure.EventHubs' en selecteer de **Microsoft.Azure.EventHubs** pakket. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Code schrijven om berichten te verzenden naar de event hub

1. Voeg aan het begin van het bestand Program.cs de volgende `using`-instructies toe.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Constanten toevoegen de `Program` klasse voor het Event Hubs tekenreeks en entiteit verbindingspad (afzonderlijke event hub-naam). Vervang de tijdelijke aanduidingen haakjes met de juiste waarden die zijn verkregen tijdens het maken van de event hub. Zorg ervoor dat de `{Event Hubs connection string}` is de verbindingsreeks op naamruimteniveau en niet de event hub-tekenreeks. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Voeg een nieuwe methode met de naam `MainAsync` naar de `Program` klasse als volgt:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
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

4. Voeg een nieuwe methode met de naam `SendMessagesToEventHub` naar de `Program` klasse als volgt:

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

5. Voeg de volgende code naar de `Main` methode in de `Program` klasse.

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
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
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

* [Ontvangen van gebeurtenissen van Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
