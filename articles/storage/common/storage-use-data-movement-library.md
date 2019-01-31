---
title: Gegevensoverdracht met het Microsoft Azure Storage-bibliotheek voor gegevensverplaatsing | Microsoft Docs
description: Gebruik de bibliotheek voor gegevensverplaatsing om te verplaatsen of kopiëren van gegevens naar of van blob en bestand inhoud. Gegevens kopiëren naar Azure Storage van lokale bestanden en gegevens binnen of tussen opslagaccounts kopiëren. Migreer uw gegevens eenvoudig naar Azure Storage.
services: storage
author: seguler
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 09/27/2017
ms.author: seguler
ms.subservice: common
ms.openlocfilehash: fd64376f68a177f12b2105ae0974eeeeefa94f4b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55460077"
---
# <a name="transfer-data-with-the-microsoft-azure-storage-data-movement-library"></a>Gegevensoverdracht met het Microsoft Azure Storage-bibliotheek voor gegevensverplaatsing

## <a name="overview"></a>Overzicht
De Microsoft Azure Storage-bibliotheek voor gegevensverplaatsing is een platformoverschrijdende open-source-bibliotheek die is ontworpen voor hoge prestaties uploaden, downloaden en kopiëren van bestanden en Azure Storage-Blobs. Deze bibliotheek is de data movement basisframework [AzCopy](../storage-use-azcopy.md). De bibliotheek voor gegevensverplaatsing biedt handige methoden die niet beschikbaar zijn in onze traditionele [.NET Azure Storage Client Library](../blobs/storage-dotnet-how-to-use-blobs.md). Dit omvat de mogelijkheid het aantal parallelle bewerkingen instellen, overdracht voortgang bijhouden, Ga eenvoudig weer verder een geannuleerde overdracht en nog veel meer.

Deze bibliotheek maakt ook gebruik van .NET Core, wat betekent dat u deze kunt gebruiken dat bij het bouwen van .NET-apps voor Windows, Linux en macOS. Raadpleeg voor meer informatie over .NET Core, de [.NET Core-documentatie](https://dotnet.github.io/). Deze bibliotheek werkt ook voor traditionele .NET Framework-apps voor Windows.

Dit document laat zien hoe u een .NET Core-consoletoepassing die wordt uitgevoerd op Windows, Linux en Mac OS en voert de volgende scenario's maken:

- Bestanden en mappen uploaden naar Blob Storage.
- Het aantal parallelle bewerkingen opgeven bij de overdracht van gegevens.
- Voortgang van het overdracht bijhouden.
- Overdracht van gegevens hervatten geannuleerd.
- Bestand kopiëren van de URL naar Blob-opslag.
- Kopiëren van Blob Storage naar Blob-opslag.

**Wat u nodig hebt:**

* [Visual Studio Code](https://code.visualstudio.com/)
* Een [Azure Storage-account](storage-quickstart-create-account.md)

> [!NOTE]
> Deze handleiding wordt ervan uitgegaan dat u al bekend met bent [Azure Storage](https://azure.microsoft.com/services/storage/). Als niet zo is, leest de [Inleiding tot Azure Storage](storage-introduction.md) documentatie is het handig. Het belangrijkste is dat u wilt [een opslagaccount maken](storage-quickstart-create-account.md) om te beginnen met behulp van de bibliotheek voor gegevensverplaatsing.
>
>

## <a name="setup"></a>Instellen

1. Ga naar de [.NET Core-installatiehandleiding](https://www.microsoft.com/net/core) .NET Core installeren. Kies de opdrachtregeloptie te gebruiken bij het selecteren van uw omgeving.
2. Maak een map voor uw project vanaf de opdrachtregel. Navigeer in deze map, typt u vervolgens `dotnet new console -o <sample-project-name>` om een C# console-project te maken.
3. Deze map openen in Visual Studio Code. Deze stap kan worden snel uitgevoerd via de opdrachtregel door te typen `code .` in Windows.
4. Installeer de [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) van de Visual Studio Code Marketplace. Start Visual Studio Code opnieuw op.
5. Op dit moment ziet u twee vragen. Een is voor het toevoegen van "vereiste assets om te bouwen en fouten opsporen." Klik op 'Ja'. Een andere vraag is voor het herstellen van niet-omgezette afhankelijkheden. Klik op 'herstellen'.
6. Wijzigen `launch.json` onder `.vscode` externe terminal als een console gebruiken. Deze instelling moet lezen ` "console": "externalTerminal"`
7. Visual Studio Code kunt u fouten opsporen in .NET Core-toepassingen. Raak `F5` uw toepassing uitvoeren en controleren of de installatie werkt. U ziet "Hallo wereld!" weergegeven in de console.

## <a name="add-data-movement-library-to-your-project"></a>-Bibliotheek voor gegevensverplaatsing toevoegen aan uw project

1. Toevoegen van de meest recente versie van de bibliotheek voor gegevensverplaatsing voor de `dependencies` sectie van uw `<project-name>.csproj` bestand. Op het moment van schrijven is deze versie `"Microsoft.Azure.Storage.DataMovement": "0.6.2"`
2. Een prompt moet worden weergegeven voor het herstellen van uw project. Klik op de knop 'herstellen'. U kunt ook uw project herstellen vanaf de opdrachtregel door de opdracht te typen `dotnet restore` in de hoofdmap van de projectmap.

Wijzigen `<project-name>.csproj`:

    <Project Sdk="Microsoft.NET.Sdk">

        <PropertyGroup>
            <OutputType>Exe</OutputType>
            <TargetFramework>netcoreapp2.0</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
            <PackageReference Include="Microsoft.Azure.Storage.DataMovement" Version="0.6.2" />
            </ItemGroup>
        </Project>

## <a name="set-up-the-skeleton-of-your-application"></a>De basis van uw toepassing instellen
Het eerste wat dat we doen is ingesteld om de code 'skeleton' van de toepassing. Met deze code ons gevraagd een naam en opslagaccountsleutel en gebruikt deze referenties voor het maken van een `CloudStorageAccount` object. Dit object wordt gebruikt om te communiceren met onze Storage-account in alle scenario's voor overdracht. De code wordt ook gevraagd ons Kies het type overdrachtbewerking die wij willen graag om uit te voeren.

Wijzigen `Program.cs`:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Diagnostics;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.DataMovement;

namespace DMLibSample
{
    public class Program
    {
        public static void Main()
        {
            Console.WriteLine("Enter Storage account name:");
            string accountName = Console.ReadLine();

            Console.WriteLine("\nEnter Storage account key:");
            string accountKey = Console.ReadLine();

            string storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + accountName + ";AccountKey=" + accountKey;
            CloudStorageAccount account = CloudStorageAccount.Parse(storageConnectionString);

            ExecuteChoice(account);
        }

        public static void ExecuteChoice(CloudStorageAccount account)
        {
            Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
            int choice = int.Parse(Console.ReadLine());

            if(choice == 1)
            {
                TransferLocalFileToAzureBlob(account).Wait();
            }
            else if(choice == 2)
            {
                TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
            }
            else if(choice == 3)
            {
                TransferUrlToAzureBlob(account).Wait();
            }
            else if(choice == 4)
            {
                TransferAzureBlobToAzureBlob(account).Wait();
            }
        }

        public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
        {

        }

        public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
        {

        }
    }
}
```

## <a name="transfer-local-file-to-azure-blob"></a>Lokaal bestand overdragen naar Azure Blob
Voeg de methoden toe `GetSourcePath` en `GetBlob` naar `Program.cs`:

```csharp
public static string GetSourcePath()
{
    Console.WriteLine("\nProvide path for source:");
    string sourcePath = Console.ReadLine();

    return sourcePath;
}

public static CloudBlockBlob GetBlob(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    Console.WriteLine("\nProvide name of new Blob:");
    string blobName = Console.ReadLine();
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    return blob;
}
```

Wijzig de `TransferLocalFileToAzureBlob` methode:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    await TransferManager.UploadAsync(localFilePath, blob);
    Console.WriteLine("\nTransfer operation complete.");
    ExecuteChoice(account);
}
```

Deze code wordt ons gevraagd het pad naar een lokaal bestand, de naam van een nieuwe of bestaande container en de naam van een nieuwe blob. De `TransferManager.UploadAsync` methode voert het uploaden met behulp van deze gegevens.

Raak `F5` uw toepassing uit te voeren. U kunt controleren of dat de upload is opgetreden door te bekijken van uw opslagaccount met de [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="set-number-of-parallel-operations"></a>Aantal parallelle bewerkingen
Een fantastische functie die worden aangeboden door de bibliotheek voor gegevensverplaatsing is de mogelijkheid om in te stellen van het aantal parallelle bewerkingen om de overdracht van gegevensdoorvoer te vergroten. De bibliotheek voor gegevensverplaatsing wordt standaard het aantal parallelle bewerkingen zijn ingesteld op 8 * het aantal kernen op uw computer.

Houd er rekening mee dat veel parallelle bewerkingen in een omgeving met een lage bandbreedte kunnen de netwerkverbinding blijvend wordt overbelast en daadwerkelijk te voorkomen dat bewerkingen volledig is voltooid. U moet om te experimenteren met deze instelling om te bepalen wat het beste op basis van werkt op uw beschikbare netwerkbandbreedte.

Nu code toevoegen waardoor het aantal parallelle bewerkingen instellen. We gaan ook toevoegen code die hoe lang het duurt voor de overdracht tijden te voltooien.

Voeg een `SetNumberOfParallelOperations` methode `Program.cs`:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

Wijzig de `ExecuteChoice` te gebruiken methode `SetNumberOfParallelOperations`:

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
    int choice = int.Parse(Console.ReadLine());

    SetNumberOfParallelOperations();

    if(choice == 1)
    {
        TransferLocalFileToAzureBlob(account).Wait();
    }
    else if(choice == 2)
    {
        TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
    }
    else if(choice == 3)
    {
        TransferUrlToAzureBlob(account).Wait();
    }
    else if(choice == 4)
    {
        TransferAzureBlobToAzureBlob(account).Wait();
    }
}
```

Wijzig de `TransferLocalFileToAzureBlob` methode voor het gebruik van een timer:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="track-transfer-progress"></a>Voortgang van de overdracht bijhouden
Het is heel handig weten hoe lang het heeft geduurd voor onze gegevens om over te dragen. Is het echter kunnen om te zien van de voortgang van onze overdracht *tijdens* bewerking voor het overbrengen nog beter zou zijn. Voor het bereiken van dit scenario, moeten we maken een `TransferContext` object. De `TransferContext` object is beschikbaar in twee vormen: `SingleTransferContext` en `DirectoryTransferContext`. De eerste is voor het overdragen van één bestand (dit is wat we doen nu) en de laatste is voor het overbrengen van een map met bestanden (die we later toevoegen).

Voeg de methoden toe `GetSingleTransferContext` en `GetDirectoryTransferContext` naar `Program.cs`:

```csharp
public static SingleTransferContext GetSingleTransferContext(TransferCheckpoint checkpoint)
{
    SingleTransferContext context = new SingleTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });

    return context;
}

public static DirectoryTransferContext GetDirectoryTransferContext(TransferCheckpoint checkpoint)
{
    DirectoryTransferContext context = new DirectoryTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });

    return context;
}
```

Wijzig de `TransferLocalFileToAzureBlob` te gebruiken methode `GetSingleTransferContext`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    Console.WriteLine("\nTransfer started...\n");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob, null, context);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="resume-a-canceled-transfer"></a>Een geannuleerde overdracht hervatten
Een andere handige functie die worden aangeboden door de bibliotheek voor gegevensverplaatsing is de mogelijkheid om te hervatten van een geannuleerde overdracht. Code waarmee we kunnen tijdelijk de bestandsoverdracht annuleren door te typen toevoegen `c`, en vervolgens de overdracht hervat 3 seconden later opnieuw.

Wijzigen `TransferLocalFileToAzureBlob`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.UploadAsync(localFilePath, blob, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadAsync(localFilePath, blob, null, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Tot nu toe onze `checkpoint` waarde is altijd ingesteld op `null`. Nu, als we de bestandsoverdracht annuleren, we ophalen van het laatste controlepunt van onze overdracht, wordt dit nieuwe controlepunt in de context van onze overdracht gebruiken.

## <a name="transfer-local-directory-to-azure-blob-directory"></a>Lokale directory overbrengen naar Azure Blob-directory
Het zou worden geplaatst als één bestand kan alleen de overdracht van de bibliotheek voor gegevensverplaatsing op een tijdstip. Gelukkig, dit is niet het geval is. De bibliotheek voor gegevensverplaatsing biedt de mogelijkheid om over te dragen van een map met bestanden en alle bijbehorende submappen. Nu code toevoegen waarmee we precies dat doen.

Eerst voegt u de methode `GetBlobDirectory` naar `Program.cs`:

```csharp
public static CloudBlobDirectory GetBlobDirectory(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container. This can be a new or existing Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    CloudBlobDirectory blobDirectory = container.GetDirectoryReference("");

    return blobDirectory;
}
```

Vervolgens wijzigt u `TransferLocalDirectoryToAzureBlobDirectory`:

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account);
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    UploadDirectoryOptions options = new UploadDirectoryOptions()
    {
        Recursive = true
    };

    try
    {
        task = TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetDirectoryTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Er zijn enkele verschillen tussen deze methode en de methode voor het uploaden van een enkel bestand. We maken nu gebruik `TransferManager.UploadDirectoryAsync` en de `getDirectoryTransferContext` methode die we eerder hebben gemaakt. Bovendien we bieden nu een `options` waarde aan onze uploadbewerking, waarmee je om aan te geven dat we willen submappen opnemen in onze uploaden.

## <a name="copy-file-from-url-to-azure-blob"></a>Bestand kopiëren van de URL naar Azure Blob
Nu gaan we toevoegen code waarmee we een bestand kopiëren vanuit een URL naar een Azure-Blob.

Wijzigen `TransferUrlToAzureBlob`:

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Een belangrijke use case voor deze functie zijn wanneer u nodig hebt om gegevens te verplaatsen van een andere cloudservice (bijvoorbeeld AWS) naar Azure. Als u een URL die u toegang tot de resource biedt hebt, kunt u eenvoudig verplaatsen die resource in Azure-Blobs met behulp van de `TransferManager.CopyAsync` methode. Deze methode wordt ook een nieuwe Booleaanse parameter geïntroduceerd. Als deze parameter `true` geeft aan dat we willen een asynchrone serverzijde kopiëren. Als deze parameter `false` geeft aan dat een synchrone kopie - wat betekent dat de resource is eerst naar de lokale computer gedownload en vervolgens geüpload naar Azure-Blob. Synchrone kopiëren is echter momenteel alleen beschikbaar voor het kopiëren van de ene Azure Storage-resource naar een andere.

## <a name="transfer-azure-blob-to-azure-blob"></a>Azure Blob overbrengen naar Azure-Blob
Een andere functie die een unieke wordt geleverd door de bibliotheek voor gegevensverplaatsing is de mogelijkheid om te kopiëren van de ene Azure Storage-resource naar een andere.

Wijzigen `TransferAzureBlobToAzureBlob`:

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(sourceBlob, destinationBlob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(sourceBlob, destinationBlob, false, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

In dit voorbeeld wordt de Booleaanse parameter ingesteld in `TransferManager.CopyAsync` naar `false` om aan te geven dat we willen een synchrone kopiëren. Dit betekent dat de resource is eerst naar de lokale computer gedownload en vervolgens geüpload naar Azure-Blob. De optie synchrone kopiëren is een uitstekende manier om ervoor te zorgen dat uw kopieerbewerking een consistente snelheid heeft. Daarentegen, is de snelheid van een asynchrone serverzijde-exemplaar afhankelijk van de beschikbare netwerkbandbreedte op de server, die kan variëren. Synchrone kopie kan echter aanvullende uitgaand verkeer kosten in vergelijking met asynchrone kopie genereren. De aanbevolen aanpak is het gebruik van synchrone kopiëren in een Azure-virtuele machine die zich in dezelfde regio als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaand verkeer.

## <a name="conclusion"></a>Conclusie
De toepassing van de verplaatsing van gegevens is nu voltooid. [Het volledige codevoorbeeld is beschikbaar op GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).

## <a name="next-steps"></a>Volgende stappen
In deze aan de slag, is er een toepassing die communiceert met Azure Storage en wordt uitgevoerd op Windows, Linux en macOS gemaakt. Deze aan de slag gericht op Blob-opslag. Deze dezelfde kennis kan echter worden toegepast met File Storage. Bekijk voor meer informatie, [naslagdocumentatie voor Azure Storage-bibliotheek voor gegevensverplaatsing](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]
