---
title: 'Zelfstudie: werken met Azure storage-wachtrijen - Azure Storage'
description: Voor meer informatie over de Azure Queue-service gebruiken om te maken van wachtrijen en invoegen, ophalen en verwijderen van berichten.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.reviewer: cbrooks
ms.service: storage
ms.subservice: queues
ms.topic: tutorial
ms.date: 04/24/2019
ms.openlocfilehash: 8d108e1683be03a79e87990b983f2eda3eadba90
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65797524"
---
# <a name="tutorial-work-with-azure-storage-queues"></a>Zelfstudie: Werken met Azure-opslagwachtrijen

Azure Queue storage implementeert cloud-gebaseerde wachtrijen voor het inschakelen van communicatie tussen onderdelen van een gedistribueerde toepassing. Elke wachtrij houdt een lijst met berichten die kunnen worden toegevoegd door een onderdeel van de afzender en verwerkt door een onderdeel van de ontvanger. Uw toepassing kunt met een wachtrij, onmiddellijk schalen om te voldoen aan de vraag. In dit artikel bevat de basisstappen voor het werken met een Azure storage-wachtrij.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
>
> - Een Azure-opslagaccount maken
> - De app maken
> - Ondersteuning voor asynchrone code toevoegen
> - Een wachtrij maken
> - Berichten in een wachtrij invoegen
> - Berichten in wachtrij
> - Een lege wachtrij verwijderen
> - Controle van opdrachtregelargumenten
> - De app bouwen en uitvoeren

## <a name="prerequisites"></a>Vereisten

- Uw gratis kopie van de platformoverschrijdende [Visual Studio Code](https://code.visualstudio.com/download) editor.
- Download en installeer de [.NET Core SDK](https://dotnet.microsoft.com/download).
- Als u geen huidige Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

Maak eerst een Azure storage-account. Zie voor een stapsgewijze handleiding voor het maken van een opslagaccount, de [een opslagaccount maken](../common/storage-quickstart-create-account.md?toc=%2Fazure%2Fstorage%2Fqueues%2Ftoc.json) Quick Start.

## <a name="create-the-app"></a>De app maken

Maak een .NET Core-toepassing met de naam **QueueApp**. Ter verduidelijking, wordt deze app zowel berichten verzenden en ontvangen via de wachtrij.

1. In een consolevenster (zoals CMD, PowerShell of Azure CLI), gebruikt u de `dotnet new` opdracht voor het maken van een nieuwe console-app met de naam van de **QueueApp**. Deze opdracht maakt u een eenvoudige 'Hallo wereld" C# project met een één-bronbestand: **Program.cs**.

   ```console
   dotnet new console -n QueueApp
   ```

2. Schakel over naar de zojuist gemaakte **QueueApp** map en bouwen de app om te controleren of alles goed.

   ```console
   cd QueueApp
   ```

   ```console
   dotnet build
   ```

   Hier ziet u het resultaat is vergelijkbaar met het volgende:

   ```output
   C:\Tutorials>dotnet new console -n QueueApp
   The template "Console Application" was created successfully.

   Processing post-creation actions...
   Running 'dotnet restore' on QueueApp\QueueApp.csproj...
     Restore completed in 155.62 ms for C:\Tutorials\QueueApp\QueueApp.csproj.

   Restore succeeded.

   C:\Tutorials>cd QueueApp

   C:\Tutorials\QueueApp>dotnet build
   Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

     Restore completed in 40.87 ms for C:\Tutorials\QueueApp\QueueApp.csproj.
     QueueApp -> C:\Tutorials\QueueApp\bin\Debug\netcoreapp2.1\QueueApp.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:02.40

   C:\Tutorials\QueueApp>_
   ```

## <a name="add-support-for-asynchronous-code"></a>Ondersteuning voor asynchrone code toevoegen

De code wordt asynchroon uitgevoerd omdat de app gebruikmaakt van cloudresources. Echter, C#van **asynchrone** en **await** zijn niet geldig in de trefwoorden **Main** methoden tot C# 7.1. U kunt gemakkelijk overschakelen naar die compiler via een vlag in de **csproj** bestand.

1. Typ vanaf de opdrachtregel in de projectmap, `code .` Visual Studio Code openen in de huidige map. Houd het opdrachtregelvenster openen. Er zijn meer opdrachten uit te voeren het later opnieuw. Als u wordt gevraagd om toe te voegen C# activa die zijn vereist voor het bouwen en fouten opsporen in, klik op de **Ja** knop.

2. Open de **QueueApp.csproj** bestand in de editor.

3. Voeg `<LangVersion>7.1</LangVersion>` voor de eerste **PropertyGroup** in het buildbestand. Zorg ervoor dat u alleen toevoegen de **LangVersion** markeren als uw **TargetFramework** kunnen mogelijk verschillen, afhankelijk van welke versie van .NET die u hebt geïnstalleerd.

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">

     <PropertyGroup>
       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp2.1</TargetFramework>
       <LangVersion>7.1</LangVersion>
     </PropertyGroup>

   ...

   ```

4. Sla de **QueueApp.csproj** bestand.

5. Open de **Program.cs** bron bestands- en update de **Main** methode asynchroon te laten uitvoeren. Vervang **void** met een **asynchrone taak** waarde retourneren.

   ```csharp
   static async Task Main(string[] args)
   ```

6. Sla de **Program.cs** bestand.

## <a name="create-a-queue"></a>Een wachtrij maken

1. Installeer de **Microsoft.Azure.Storage.Common** en **Microsoft.Azure.Storage.Queue** pakketten aan het project met de `dotnet add package` opdracht. De volgende dotnet-opdrachten uitvoeren vanuit de projectmap in het consolevenster.

   ```console
   dotnet add package Microsoft.Azure.Storage.Common
   dotnet add package Microsoft.Azure.Storage.Queue
   ```

2. Aan de bovenkant van de **Program.cs** bestand, de volgende naamruimten toevoegen direct na de `using System;` instructie. Deze app maakt gebruik van typen uit deze naamruimten verbinding maken met Azure Storage en werken met wachtrijen.

   ```csharp
   using System.Threading.Tasks;
   using Microsoft.Azure.Storage;
   using Microsoft.Azure.Storage.Queue;
   ```

3. Sla de **Program.cs** bestand.

### <a name="get-your-connection-string"></a>Verbindingsreeks ophalen

De clientbibliotheek maakt gebruik van een verbindingsreeks om uw verbinding te maken. De verbindingsreeks is beschikbaar in de **instellingen** sectie van uw opslagaccount in Azure portal.

1. In uw webbrowser, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com/).

2. Ga in Azure Portal naar uw opslagaccount.

3. Selecteer **toegangssleutels**.

4. Klik op de **kopie** knop aan de rechterkant van de **Connection string** veld.

![Verbindingsreeks](media/storage-tutorial-queues/get-connection-string.png)

De verbindingsreeks heeft de volgende indeling:

   ```
   "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
   ```

### <a name="add-the-connection-string-to-the-app"></a>De verbindingsreeks toevoegen aan de app

De verbindingsreeks in de app toevoegen, zodat deze toegang heeft tot het opslagaccount.

1. Ga terug naar Visual Studio Code.

2. In de **programma** klasse, voegt u een `private const string connectionString =` lid voor het opslaan van de verbindingsreeks.

3. Na het gelijkteken, plak de tekenreeks-waarde die u eerder hebt in uw Azure-portal hebt gekopieerd. De **connectionString** waarde is uniek voor uw account.

4. Verwijderen van de code van de "Hello World" **Main**. Uw code moet eruitzien als in het volgende, maar met de waarde van de unieke verbindingsreeks.

   ```csharp
   namespace QueueApp
   {
       class Program
       {
           private const string connectionString = "DefaultEndpointsProtocol=https; ...";

           static async Task Main(string[] args)
           {
           }
       }
   }
   ```

5. Update **Main** maken een **CloudQueue** object, dat later wordt doorgegeven aan het verzenden en ontvangen van methoden.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");
        }
   ```

6. Sla het bestand op.

## <a name="insert-messages-into-the-queue"></a>Plaats berichten in de wachtrij

Maak een nieuwe methode voor het verzenden van een bericht in de wachtrij wordt geplaatst. Toevoegen van de volgende methode aan uw **programma** klasse. Deze methode een verwijzing naar een wachtrij opgehaald en vervolgens maakt u een nieuwe wachtrij als deze nog niet bestaat door het aanroepen van [CreateIfNotExistsAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.createifnotexistsasync). Vervolgens, wordt het bericht naar de wachtrij door het aanroepen van [AddMessageAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessageasync).

1. Voeg de volgende **SendMessageAsync** methode voor uw **programma** klasse.

   ```csharp
   static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
   {
       bool createdQueue = await theQueue.CreateIfNotExistsAsync();

       if (createdQueue)
       {
           Console.WriteLine("The queue was created.");
       }

       CloudQueueMessage message = new CloudQueueMessage(newMessage);
       await theQueue.AddMessageAsync(message);
   }
   ```

2. Sla het bestand op.

## <a name="dequeue-messages"></a>Berichten in wachtrij

Maak een nieuwe methode met de naam **ReceiveMessageAsync**. Deze methode kunt u een bericht uit de wachtrij ontvangt door het aanroepen van [GetMessageAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessageasync). Zodra het bericht is ontvangen, is het belangrijk om te verwijderen uit de wachtrij, zodat deze meer dan één keer wordt niet verwerkt. Nadat het bericht is ontvangen, verwijderen uit de wachtrij door het aanroepen van [DeleteMessageAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessageasync).

1. Voeg de volgende **ReceiveMessageAsync** methode voor uw **programma** klasse.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
       }
   }
   ```

2. Sla het bestand op.

## <a name="delete-an-empty-queue"></a>Een lege wachtrij verwijderen

Het is een best practice aan het einde van een project om te bepalen of u moet nog steeds de resources die u hebt gemaakt. Resources naar links wordt uitgevoerd kan kosten u geld. Als de wachtrij bestaat, maar leeg is, vraagt de gebruiker als ze zouden graag om deze te verwijderen.

1. Vouw de **ReceiveMessageAsync** methode om op te nemen van een prompt om de wachtrij leeg te verwijderen.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
           else
           {
               Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
               string response = Console.ReadLine();

               if (response == "Y" || response == "y")
               {
                   await theQueue.DeleteIfExistsAsync();
                   return "The queue was deleted.";
               }
               else
               {
                   return "The queue was not deleted.";
               }
           }
       }
       else
       {
           return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
       }
   }
   ```

2. Sla het bestand op.

## <a name="check-for-command-line-arguments"></a>Controle van opdrachtregelargumenten

Als er eventueel opdrachtregelargumenten in die in de app wordt doorgegeven, wordt ervan uitgegaan dat ze een bericht moeten worden toegevoegd aan de wachtrij. Neem deel aan de argumenten bij elkaar voor het maken van een tekenreeks. Deze tekenreeks naar de berichtenwachtrij toevoegen door het aanroepen van de **SendMessageAsync** methode hebben we eerder hebt toegevoegd.

Als er geen opdrachtregelargumenten, een ophalen-bewerking niet uitvoeren. Roep de **ReceiveMessageAsync** methode voor het ophalen van het eerste bericht in de wachtrij.

Ten slotte wachten op invoer van de gebruiker voordat u afsluit door het aanroepen van **Console.ReadLine**.

1. Vouw de **Main** methode om te controleren voor opdrachtregelargumenten en wachten op invoer van de gebruiker.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received: {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }
   ```

2. Sla het bestand op.

## <a name="complete-code"></a>Volledige code

Hier is de volledige code voor dit project.

   ```csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.Azure.Storage;
   using Microsoft.Azure.Storage.Queue;

   namespace QueueApp
   {
    class Program
    {
        // The string value is broken up for better onscreen formatting
        private const string connectionString = "DefaultEndpointsProtocol=https;" +
                                                "AccountName=<your storage account name>;" +
                                                "AccountKey=<your key>;" +
                                                "EndpointSuffix=core.windows.net";

        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }

        static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
        {
            bool createdQueue = await theQueue.CreateIfNotExistsAsync();

            if (createdQueue)
            {
                Console.WriteLine("The queue was created.");
            }

            CloudQueueMessage message = new CloudQueueMessage(newMessage);
            await theQueue.AddMessageAsync(message);
        }

        static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
        {
            bool exists = await theQueue.ExistsAsync();

            if (exists)
            {
                CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

                if (retrievedMessage != null)
                {
                    string theMessage = retrievedMessage.AsString;
                    await theQueue.DeleteMessageAsync(retrievedMessage);
                    return theMessage;
                }
                else
                {
                    Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
                    string response = Console.ReadLine();

                    if (response == "Y" || response == "y")
                    {
                        await theQueue.DeleteIfExistsAsync();
                        return "The queue was deleted.";
                    }
                    else
                    {
                        return "The queue was not deleted.";
                    }
                }
            }
            else
            {
                return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
            }
        }
    }
   }
   ```

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Vanaf de opdrachtregel in de projectmap, voer de volgende dotnet-opdracht om het project te bouwen.

   ```console
   dotnet build
   ```

2. Nadat het project wordt gemaakt is, voert u de volgende opdracht om het eerste bericht toevoegen aan de wachtrij.

   ```console
   dotnet run First queue message
   ```

U ziet deze uitvoer:

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter..._
   ```

3. De app uitvoeren met geen opdrachtregelargumenten voor het ontvangen en verwijderen van het eerste bericht in de wachtrij.

   ```console
   dotnet run
   ```

4. Doorgaan met de app totdat alle berichten zijn verwijderd. Als u er nog een keer uitvoert, krijgt u een bericht dat de wachtrij is leeg en een prompt om de wachtrij te verwijderen.

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Second queue message
   Sent: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Third queue message
   Sent: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   The queue is empty. Attempt to delete it? (Y/N) Y
   Received: The queue was deleted.
   Press Enter...

   C:\Tutorials\QueueApp>_
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

1. Een wachtrij maken
2. Toevoegen en verwijderen van berichten uit een wachtrij
3. Een Azure storage-wachtrij verwijderen

Bekijk de Snelstartgids voor Azure-wachtrijen voor meer informatie.

> [!div class="nextstepaction"]
> [Wachtrijen-snelstartgids](storage-quickstart-queues-portal.md)
