---
title: Gegevensoverdracht met het Microsoft Azure Storage-bibliotheek voor gegevensverplaatsing | Microsoft Docs
description: "Gebruik de bibliotheek voor gegevensverplaatsing verplaatsen of kopiëren van gegevens of naar blob- en -inhoud. Gegevens van lokale bestanden kopiëren naar Azure Storage of kopiëren van gegevens binnen of tussen opslagaccounts. Uw gegevens eenvoudig migreren naar Azure Storage."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/27/2017
ms.author: seguler
ms.openlocfilehash: eb96f3697d5369ba96a1b0c491e3eacf09e7aac4
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="transfer-data-with-the-microsoft-azure-storage-data-movement-library"></a>Gegevensoverdracht met het Microsoft Azure Storage-bibliotheek voor gegevensverplaatsing

## <a name="overview"></a>Overzicht
De Microsoft Azure Storage-bibliotheek voor gegevensverplaatsing is een open-source platformoverschrijdende-bibliotheek die is ontworpen voor hoge prestaties uploaden, downloaden en uit Azure Storage-Blobs en bestanden te kopiëren. Deze bibliotheek is core data movement framework die ook door [AzCopy](../storage-use-azcopy.md). De bibliotheek voor gegevensverplaatsing biedt handige methoden die niet beschikbaar zijn in onze traditionele [.NET Azure Storage-clientbibliotheek](../blobs/storage-dotnet-how-to-use-blobs.md). Dit biedt de mogelijkheid om het aantal parallelle bewerkingen instellen, overdracht voortgang volgen, hervat eenvoudig een geannuleerde overdracht en nog veel meer.  

Deze bibliotheek gebruikt ook .NET Core, wat betekent dat u deze kunt gebruiken dat bij het bouwen van .NET-toepassingen voor Windows, Linux en Mac OS. Raadpleeg voor meer informatie over .NET Core, de [.NET Core documentatie](https://dotnet.github.io/). Deze bibliotheek werkt ook voor traditionele .NET Framework-apps voor Windows. 

Dit document wordt getoond hoe .NET Core-console een toepassing maken die wordt uitgevoerd op Windows, Linux en Mac OS en voert de volgende scenario's:

- Het van uploadbestanden en mappen naar Blob Storage.
- Definieer het aantal parallelle bewerkingen bij de overdracht van gegevens.
- Voortgang van het overbrengen bijhouden.
- Overdracht van gegevens hervatten geannuleerd. 
- Bestand kopiëren van de URL naar Blob Storage. 
- Kopiëren van Blob-opslag naar Blob Storage.

**Wat u nodig hebt:**

* [Visual Studio Code](https://code.visualstudio.com/)
* Een [Azure Storage-account](storage-create-storage-account.md#create-a-storage-account)

> [!NOTE]
> Deze handleiding wordt ervan uitgegaan dat u al bekend met bent [Azure Storage](https://azure.microsoft.com/services/storage/). Als u niet lezen van de [Inleiding tot Azure Storage](storage-introduction.md) documentatie is het handig. Het belangrijkste is, moet u [een opslagaccount maken](storage-create-storage-account.md#create-a-storage-account) aan de slag met de bibliotheek voor gegevensverplaatsing.
> 
> 

## <a name="setup"></a>Instellen  

1. Ga naar de [installatiehandleiding voor .NET Core](https://www.microsoft.com/net/core) .NET Core installeren. Als u uw omgeving, kiest u de opdrachtregeloptie. 
2. Maak een map voor uw project vanaf de opdrachtregel. Navigeer naar deze map staan, typ `dotnet new console -o <sample-project-name>` een C#-console-project maken.
3. Deze map openen in Visual Studio Code. Deze stap kan worden snel gedaan via de opdrachtregel te typen `code .` in Windows.  
4. Installeer de [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) vanuit Visual Studio Code Marketplace. Start Visual Studio Code opnieuw. 
5. U ziet nu twee keer worden gevraagd. Een is voor het toevoegen van 'vereist activa voor het bouwen en debug'. Klik op 'Ja'. Een andere vraag is voor het herstellen van niet-omgezette afhankelijkheden. Klik op 'herstellen'.
6. Wijzig `launch.json` onder `.vscode` externe terminal gebruiken als een console. Deze instelling moet worden gelezen als` "console": "externalTerminal"`
7. Visual Studio Code kunt u foutopsporing .NET Core-toepassingen. Klik op `F5` voor het uitvoeren van uw toepassing en controleren of uw instellingen werkt. U ziet "Hello World!" aan de console afgedrukt. 

## <a name="add-data-movement-library-to-your-project"></a>Bibliotheek voor gegevensverplaatsing toevoegen aan uw project

1. Toevoegen van de nieuwste versie van de bibliotheek voor gegevensverplaatsing voor de `dependencies` gedeelte van uw `<project-name>.csproj` bestand. Op het moment van schrijven is deze versie`"Microsoft.Azure.Storage.DataMovement": "0.6.2"` 
2. Een prompt moet worden weergegeven voor het herstellen van uw project. Klik op de knop 'restore'. U kunt ook uw project terugzetten vanaf de opdrachtregel typt u de opdracht `dotnet restore` in de hoofdmap van uw projectmap.

Wijzig `<project-name>.csproj`:

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
Het eerste wat dat we doen is de code 'basisproject' van de toepassing ingesteld. Deze code ons gevraagd een naam en het account opslagaccountsleutel en deze referenties gebruikt voor het maken een `CloudStorageAccount` object. Dit object wordt gebruikt om te communiceren met de Storage-account in alle scenario's voor overdracht. De code wordt ook gevraagd ons Kies het type overdrachtsbewerking willen we uitvoeren. 

Wijzig `Program.cs`:

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

## <a name="transfer-local-file-to-azure-blob"></a>Lokale bestand overdragen naar Azure-Blob
Voeg de methoden `GetSourcePath` en `GetBlob` naar `Program.cs`:

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

Deze code wordt ons gevraagd om het pad naar een lokaal bestand, de naam van een nieuwe of bestaande container en de naam van een nieuwe blob. De `TransferManager.UploadAsync` methode voert u het gebruik van deze informatie uploaden. 

Raak `F5` uw toepassing uit te voeren. U kunt controleren dat de upload is opgetreden door het bekijken van uw opslagaccount met de [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="set-number-of-parallel-operations"></a>Aantal parallelle bewerkingen
Een goede functie aangeboden door de bibliotheek voor gegevensverplaatsing is de mogelijkheid het aantal parallelle bewerkingen te verhogen van de overdracht gegevensdoorvoer instellen. De bibliotheek voor gegevensverplaatsing wordt standaard het aantal parallelle bewerkingen zijn ingesteld op 8 * het aantal kernen op uw computer. 

Houd er rekening mee dat veel parallelle bewerkingen in een omgeving met lage bandbreedte mogelijk de netwerkverbinding overbelast en daadwerkelijk te voorkomen dat de bewerkingen van het volledig is voltooid. U wilt experimenteren met deze instelling om te bepalen wat het beste op basis werkt op de beschikbare netwerkbandbreedte. 

Laten we code waarmee we het aantal parallelle bewerkingen instellen toevoegen. We gaan ook programmacode toevoegen die hoe lang het duurt voor de overdracht time-out te voltooien.

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

Wijzig de `TransferLocalFileToAzureBlob` methode een timer gebruiken:

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
Weten hoe lang geduurd voor onze gegevens om over te dragen is uitstekend. Echter, wordt de voortgang van onze overdracht zien *tijdens* de overdrachtsbewerking nog beter zou zijn. Voor dit scenario, moeten we maken een `TransferContext` object. De `TransferContext` object kent twee vormen: `SingleTransferContext` en `DirectoryTransferContext`. De voormalige is voor de overdracht van één bestand (dit is wat we geven nu) en de laatste is voor de overdracht van een map met bestanden (die we later toevoegen).

Voeg de methoden `GetSingleTransferContext` en `GetDirectoryTransferContext` naar `Program.cs`: 

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

## <a name="resume-a-canceled-transfer"></a>Een geannuleerde overdracht hervat
Een andere handige functie aangeboden door de bibliotheek voor gegevensverplaatsing is de mogelijkheid een geannuleerde overdracht hervatten. Laten we de code die kan worden uitgevoerd op de overdracht tijdelijk annuleren door op te geven toevoegen `c`, en vervolgens de overdracht 3 seconden later hervatten.

Wijzig `TransferLocalFileToAzureBlob`:

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

Tot nu toe onze `checkpoint` waarde is altijd ingesteld op `null`. Nu als we de overdracht annuleert, we het laatste controlepunt van onze overdracht ophalen en dit nieuwe controlepunt in de context van onze overdracht gebruiken. 

## <a name="transfer-local-directory-to-azure-blob-directory"></a>Lokale directory overdragen naar Azure Blob-directory
Het normaal zou zijn geplaatst als de bibliotheek voor gegevensverplaatsing slechts één tegelijk bestandsoverdracht kan. Gelukkig is dit niet het geval. De bibliotheek voor gegevensverplaatsing biedt de mogelijkheid om over te dragen van een map met bestanden en alle submappen. Laten we code waarmee we dit wordt geregeld toevoegen.

Voeg eerst de methode `GetBlobDirectory` naar `Program.cs`:

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

Wijzig `TransferLocalDirectoryToAzureBlobDirectory`:

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

Er zijn een aantal verschillen tussen deze methode en de methode voor het uploaden van één bestand. We maken nu gebruik `TransferManager.UploadDirectoryAsync` en de `getDirectoryTransferContext` methode we eerder hebben gemaakt. Bovendien we bieden nu een `options` waarde aan onze uploadbewerking, waardoor we om aan te geven dat we willen submappen opnemen in de upload. 

## <a name="copy-file-from-url-to-azure-blob"></a>Bestand kopiëren vanuit de URL naar Azure-Blob
Nu gaan we code toevoegen die kan worden uitgevoerd op een bestand kopiëren vanuit een URL naar een Azure-Blob. 

Wijzig `TransferUrlToAzureBlob`:

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

Een belangrijk gebruiksvoorbeeld voor deze functie is als u nodig hebt om gegevens te verplaatsen van een andere cloudservice (bijvoorbeeld AWS) naar Azure. Als u een URL die u toegang tot de resource biedt hebt, kunt u eenvoudig verplaatsen die resource in Azure Blobs met behulp van de `TransferManager.CopyAsync` methode. Deze methode wordt ook introduceert een nieuwe Boole-parameter. Als deze parameter `true` geeft aan dat er een asynchrone serverzijde kopiëren wilt maken. Als deze parameter `false` synchrone kopiëren - wat betekent dat de resource eerst gedownload naar de lokale computer, wordt geüpload naar Azure-Blob. Synchrone kopie is echter momenteel alleen beschikbaar voor het kopiëren van een Azure Storage-resource naar een andere. 

## <a name="transfer-azure-blob-to-azure-blob"></a>Azure Blob overdragen naar Azure Blob
Een andere functie die uniek opgegeven door de bibliotheek voor gegevensverplaatsing is de mogelijkheid om te kopiëren van de ene Azure Storage resource naar een andere. 

Wijzig `TransferAzureBlobToAzureBlob`:

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

In dit voorbeeld wordt de Booleaanse parameter ingesteld in `TransferManager.CopyAsync` naar `false` om aan te geven dat we willen een synchrone kopiëren. Dit betekent dat de resource is eerst gedownload naar de lokale computer vervolgens geüpload naar Azure-Blob. De optie synchrone kopie is een uitstekende manier om ervoor te zorgen dat uw kopieerbewerking een consistente snelheid. Daarentegen is is de snelheid van een asynchrone serverzijde kopie afhankelijk van de beschikbare netwerkbandbreedte op de server, kan variëren. Synchrone kopie mogelijk echter extra uitgaande kosten vergeleken met de asynchrone kopie genereren. De aanbevolen aanpak is het synchrone exemplaar gebruiken op een virtuele machine van Azure die zich in dezelfde regio bevinden als uw storage-account van de bron om te voorkomen dat de kosten voor uitgaande gegevens.

## <a name="conclusion"></a>Conclusie
Onze toepassing van de verplaatsing van gegevens is voltooid. [Het volledige voorbeeld is beschikbaar op GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app). 

## <a name="next-steps"></a>Volgende stappen
In deze aan de slag, is er een toepassing die communiceert met Azure Storage en wordt uitgevoerd op Windows, Linux en Mac OS gemaakt. Deze aan de slag gericht op het Blob-opslag. Deze dezelfde kennis kan echter worden toegepast op File Storage. Bekijk voor meer informatie, [Azure Storage-bibliotheek voor gegevensverplaatsing naslagdocumentatie](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]




