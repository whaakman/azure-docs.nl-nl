---
title: Aan de slag met Azure Service Bus-wachtrijen | Microsoft Docs
description: Een C#-consoletoepassing schrijven voor Service Bus Messaging-wachtrijen.
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
ms.date: 12/7/2017
ms.author: sethm
ms.openlocfilehash: 6af7e4d238c10c0fed3443db58644e3557525993
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-service-bus-queues"></a>Aan de slag met Service Bus-wachtrijen

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze zelfstudie bestaat uit de volgende stappen:

1. Een Service Bus-naamruimte maken met de Azure-portal.
2. Een Service Bus-wachtrij maken met de Azure-portal.
3. Schrijf een .NET Core-consoletoepassing om een set berichten naar de wachtrij te verzenden.
4. Schrijf een .NET Core-consoletoepassing om deze berichten van de wachtrij te ontvangen.

## <a name="prerequisites"></a>Vereisten

1. [Visual Studio 2017 update 3 (versie 15.3, 26730.01)](http://www.visualstudio.com/vs) of later.
2. [NET Core SDK](https://www.microsoft.com/net/download/windows), versie 2.0 of later.
2. Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Een naamruimte maken met de Azure-portal

> [!NOTE] 
> U kunt ook een Service Bus-naamruimte en berichtentiteiten maken met [PowerShell](/powershell/azure/get-started-azureps). Zie voor meer informatie [PowerShell gebruiken om Service Bus-resources te beheren](service-bus-manage-with-ps.md).

Als u al een Service Bus Messaging-naamruimte hebt gemaakt, gaat u naar het gedeelte [Een wachtrij maken met de Azure-portal](#2-create-a-queue-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Een wachtrij maken met de Azure-portal

Als u al een Service Bus-wachtrij hebt gemaakt, gaat u naar het gedeelte [Berichten naar de wachtrij verzenden](#3-send-messages-to-the-queue).

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Berichten naar de wachtrij verzenden

Maak een C#-consoletoepassing met Visual Studio om berichten naar de wachtrij te verzenden.

### <a name="create-a-console-application"></a>Een consoletoepassing maken

Start Visual Studio en maak een nieuwe **consoletoepassing (.NET Core)**.

### <a name="add-the-service-bus-nuget-package"></a>Het Service Bus NuGet-pakket toevoegen

1. Klik met de rechtermuisknop op het nieuwe project en selecteer **NuGet-pakketten beheren**.
2. Klik op het tabblad **Bladeren**, zoek naar **[Microsoft.Azure.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)** en selecteer het item **Microsoft.Azure.ServiceBus**. Klik op **Installeren** om de installatie te voltooien en sluit vervolgens dit dialoogvenster.
   
    ![Een NuGet-pakket selecteren][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-queue"></a>Code schrijven om berichten naar de wachtrij te verzenden

1. Voeg in Program.cs de volgende `using` instructies aan het begin van de naamruimtedefinitie toe voor de klassedeclaratie:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Declareer binnen de klasse `Program` de volgende variabelen. Stel de variabele `ServiceBusConnectionString` in op de verbindingstekenreeks die u hebt verkregen tijdens het maken van de naamruimte en stel `QueueName` in op de naam die u hebt gebruikt bij het maken van de wachtrij:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ``` 

3. Vervang de standaardinhoud van `Main()` door de volgende coderegel:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Voeg direct na `Main()` de volgende asynchrone `MainAsync()` methode toe die de methode berichten verzenden aanroept:

    ```csharp
    static async Task MainAsync()
    {
        const int numberOfMessages = 10;
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

5. Voeg direct na de `MainAsync()` methode de volgende `SendMessagesAsync()` methode toe voor het uitvoeren van het werk van het verzenden van het aantal berichten dat is opgegeven door `numberOfMessagesToSend` (momenteel ingesteld op 10):

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the queue.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the queue.
                await queueClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```
   
6. Zo zou het bestand Program.cs er moeten uitzien.
   
    ```csharp
    namespace CoreSenderApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            // Connection String for the namespace can be obtained from the Azure portal under the 
            // 'Shared Access policies' section.
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string QueueName = "<your_queue_name>";
            static IQueueClient queueClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                const int numberOfMessages = 10;
                queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
                Console.WriteLine("======================================================");

                // Send Messages
                await SendMessagesAsync(numberOfMessages);
                        
                Console.ReadKey();

                await queueClient.CloseAsync();
            }

            static async Task SendMessagesAsync(int numberOfMessagesToSend)
            {
                try
                {
                    for (var i = 0; i < numberOfMessagesToSend; i++)
                    {
                        // Create a new message to send to the queue
                        string messageBody = $"Message {i}";
                        var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                        // Write the body of the message to the console
                        Console.WriteLine($"Sending message: {messageBody}");

                        // Send the message to the queue
                        await queueClient.SendAsync(message);
                    }
                }
                catch (Exception exception)
                {
                    Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
                }
            }
        }
    }
    ```

7. Voer het programma uit en controleer dit in Azure Portal: klik op de naam van uw wachtrij in het venster **Overzicht** voor de naamruimte. Het scherm **Essentials** voor de wachtrij wordt weergegeven. De waarde voor **Aantal actieve berichten** voor de wachtrij is nu **10**. Telkens wanneer die u de zendtoepassing uitvoert zonder de berichten op te halen (zoals in het volgende gedeelte wordt beschreven), wordt deze waarde verhoogd met 10. U zult ook zien dat de huidige grootte voor de wachtrij de waarde voor **Huidige** op het venster **Essentials** vergroot telkens wanneer de app een bericht toevoegt aan de wachtrij.
   
      ![Berichtgrootte][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Berichten ontvangen uit de wachtrij

Als u berichten wilt ontvangen die u zojuist hebt verzonden, maakt u een andere .NET Core-consoletoepassing en installeert u het **Microsoft.Azure.ServiceBus** NuGet-pakket, zoals ook met de voorgaande verzendtoepassing is gedaan.

### <a name="write-code-to-receive-messages-from-the-queue"></a>Schrijven van code voor het ontvangen van berichten van de wachtrij

1. Voeg in Program.cs de volgende `using` instructies aan het begin van de naamruimtedefinitie toe voor de klassedeclaratie:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Declareer binnen de klasse `Program` de volgende variabelen. Stel de variabele `ServiceBusConnectionString` in op de verbindingstekenreeks die u hebt verkregen tijdens het maken van de naamruimte en stel `QueueName` in op de naam die u hebt gebruikt bij het maken van de wachtrij:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

3. Vervang de standaardinhoud van `Main()` door de volgende coderegel:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Voeg direct na `Main()` de volgende asynchrone `MainAsync()` methode toe die de methode `RegisterOnMessageHandlerAndReceiveMessages()` aanroept:

    ```csharp
    static async Task MainAsync()
    {
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register the queue message handler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

5. Voeg direct na de methode `MainAsync()` de volgende methode toe die de berichtenhandler registreert en de berichten ontvangt die zijn verzonden door de zendtoepassing:

    ```csharp
    static void RegisterOnMessageHandlerAndReceiveMessages()
    {
        // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
        var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
        {
            // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
            // Set it according to how many messages the application wants to process in parallel.
            MaxConcurrentCalls = 1,

            // Indicates whether the message pump should automatically complete the messages after returning from user callback.
            // False below indicates the complete operation is handled by the user callback as in ProcessMessagesAsync().
            AutoComplete = false
        };

        // Register the function that processes messages.
        queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```

6. Voeg direct na de vorige methode de volgende methode `ProcessMessagesAsync()` toe voor het verwerken van de ontvangen berichten:
 
    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the queue Client is created in ReceiveMode.PeekLock mode (which is the default).
        await queueClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
        // If queueClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

7. Voeg tot slot de volgende methode toe om eventuele uitzonderingen af te handelen:
 
    ```csharp
    // Use this handler to examine the exceptions received on the message pump.
    static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
    {
        Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
        var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
        Console.WriteLine("Exception context for troubleshooting:");
        Console.WriteLine($"- Endpoint: {context.Endpoint}");
        Console.WriteLine($"- Entity Path: {context.EntityPath}");
        Console.WriteLine($"- Executing Action: {context.Action}");
        return Task.CompletedTask;
    }    
    ```    
   
8. Zo zou het bestand Program.cs er moeten uitzien:
   
    ```csharp
    namespace CoreReceiverApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            // Connection String for the namespace can be obtained from the Azure portal under the 
            // 'Shared Access policies' section.
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string QueueName = "<your_queue_name>";
            static IQueueClient queueClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
                Console.WriteLine("======================================================");

                // Register QueueClient's MessageHandler and receive messages in a loop
                RegisterOnMessageHandlerAndReceiveMessages();
                    
                Console.ReadKey();

                await queueClient.CloseAsync();
            }

            static void RegisterOnMessageHandlerAndReceiveMessages()
            {
                // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
                var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
                {
                    // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
                    // Set it according to how many messages the application wants to process in parallel.
                    MaxConcurrentCalls = 1,

                    // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                    // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                    AutoComplete = false
                };

                // Register the function that will process messages
                queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
            }

            static async Task ProcessMessagesAsync(Message message, CancellationToken token)
            {
                // Process the message
                Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

                // Complete the message so that it is not received again.
                // This can be done only if the queueClient is created in ReceiveMode.PeekLock mode (which is default).
                await queueClient.CompleteAsync(message.SystemProperties.LockToken);

                // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
                // If queueClient has already been Closed, you may chose to not call CompleteAsync() or AbandonAsync() etc. calls 
               // to avoid unnecessary exceptions.
            }

            static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
            {
                Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
                var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
                Console.WriteLine("Exception context for troubleshooting:");
                Console.WriteLine($"- Endpoint: {context.Endpoint}");
                Console.WriteLine($"- Entity Path: {context.EntityPath}");
                Console.WriteLine($"- Executing Action: {context.Action}");
                return Task.CompletedTask;
            }
        }
    }
    ```
4. Voer het programma uit en controleer de portal opnieuw. De waarden voor **Aantal actieve berichten** en **Huidige** moeten nu **0** zijn.
   
    ![Wachtrijlengte][queue-message-receive]

Gefeliciteerd. U hebt nu een wachtrij gemaakt, een set berichten verzonden naar die wachtrij en deze berichten ontvangen van dezelfde wachtrij.

## <a name="next-steps"></a>Volgende stappen

Bekijk onze [GitHub-opslagplaats met voorbeelden](https://github.com/Azure/azure-service-bus/tree/master/samples) die enkele van de meer geavanceerde functies van Service Bus Messaging laten zien.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png

