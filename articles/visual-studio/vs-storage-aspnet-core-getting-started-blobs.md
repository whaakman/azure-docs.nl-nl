---
title: Aan de slag met Azure Blob storage en Visual Studio verbonden services (ASP.NET Core) | Microsoft Docs
description: Aan de slag verbonden met Azure Blob-opslag in een ASP.NET Core-project in Visual Studio, nadat u verbinding met een opslagaccount met behulp van Visual Studio services
services: storage
documentationcenter: ''
author: camsoper
manager: wpickett
editor: ''
ms.service: storage
ms.workload: web
ms.custom: vs-azure
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: casoper
ms.openlocfilehash: f8de7f7a263a087abb16ed48d05b2cae9834b4ad
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52961107"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (ASP.NET Core)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure Blob-opslag is een service waarmee ongestructureerde gegevens worden opgeslagen in de cloud-objecten of-blobs. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag. Zie voor meer informatie over Blob-opslag, [Inleiding tot Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md).

Deze zelfstudie laat zien hoe u ASP.NET Core-code voor enkele algemene scenario's die gebruikmaken van Blob-opslag te schrijven. Scenario's omvatten het maken van een blob-container en uploaden, vermelden, downloaden en verwijderen van blobs.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

In deze sectie helpt bij het instellen van de ontwikkelomgeving. Dit omvat het maken van een ASP.NET-Model-View-Controller (MVC)-app, de verbinding van een verbonden services toe te voegen, een controller toe te voegen en de vereiste naamruimte richtlijnen op te geven.

### <a name="create-an-aspnet-mvc-app-project"></a>Een ASP.NET MVC-app-project maken

1. Open Visual Studio.

1. Selecteer in het hoofdmenu van **bestand** > **nieuw** > **Project**.

1. In de **nieuw Project** in het dialoogvenster, selecteer **Web** > **ASP.NET Core-webtoepassing** > **AspNetCoreStorage**. Selecteer vervolgens **OK**.

    ![Schermopname van Visual Studio het dialoogvenster Nieuw Project](./media/vs-storage-aspnet-core-getting-started-blobs/new-project.png)

1. In de **nieuwe ASP.NET Core-webtoepassing** in het dialoogvenster, selecteer **.NET Core** > **ASP.NET Core 2.0** > **(Web-App Model-View-Controller)**. Selecteer vervolgens **OK**.

    ![Schermafbeelding van de nieuwe ASP.NET Core-webtoepassing-dialoogvenster](./media/vs-storage-aspnet-core-getting-started-blobs/new-mvc.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Verbonden services gebruiken voor verbinding met een Azure storage-account

1. In **Solution Explorer**, met de rechtermuisknop op het project.

2. Selecteer in het contextmenu **toevoegen** > **Connected Service**.

1. In de **Connected Services** in het dialoogvenster, selecteer **opslag in de Cloud met Azure Storage**, en selecteer vervolgens **configureren**.

    ![In het dialoogvenster Schermafbeelding van de verbonden Services](./media/vs-storage-aspnet-core-getting-started-blobs/connected-services.png)

1. In de **Azure Storage** in het dialoogvenster, selecteer het Azure storage-account moet worden gebruikt voor deze zelfstudie. Voor het maken van een nieuw Azure storage-account selecteert **maken van een nieuw Opslagaccount**, en vul het formulier. Selecteer na het selecteren van een bestaand opslagaccount of het maken van een nieuwe, **toevoegen**. Visual Studio installeert het NuGet-pakket voor Azure Storage en een verbindingsreeks voor opslag op **appsettings.json**.

> [!TIP]
> Voor meer informatie over het maken van een storage-account met de [Azure-portal](https://portal.azure.com), Zie [een opslagaccount maken](../storage/common/storage-quickstart-create-account.md).
>
> U kunt ook een storage-account maken met behulp van [Azure PowerShell](../storage/common/storage-powershell-guide-full.md), [Azure CLI](../storage/common/storage-azure-cli.md), of [Azure Cloud Shell](../cloud-shell/overview.md).


### <a name="create-an-mvc-controller"></a>Maken van een MVC-controller 

1. In **Solution Explorer**, met de rechtermuisknop op **Controllers**.

2. Selecteer in het contextmenu **toevoegen** > **Controller**.

    ![Schermopname van Solution Explorer](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-menu.png)

1. In de **Add Scaffold** in het dialoogvenster, selecteer **MVC-Controller - leeg**, en selecteer **toevoegen**.

    ![Schermafbeelding van de Add Scaffold-dialoogvenster](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller.png)

1. In de **leeg MVC-Controller toevoegen** in het dialoogvenster, de naam van de controller *BlobsController*, en selecteer **toevoegen**.

    ![Schermafbeelding van toevoegen leeg MVC-Controller-dialoogvenster](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-name.png)

1. Voeg de volgende `using` instructies aan het `BlobsController.cs` bestand:

    ```csharp
    using System.IO;
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Verbinding maken met een opslagaccount en een containerverwijzing ophalen

Een blob-container is een geneste hiërarchie van blobs en mappen. De rest van de stappen in dit document vereisen een verwijzing naar een blob-container, zodat code moet worden geplaatst in een eigen methode voor hergebruik.

De volgende stappen maakt u een methode voor het verbinding maken met de storage-account met behulp van de verbindingsreeks in **appsettings.json**. De stappen ook maken een verwijzing naar een container. De instelling voor de verbindingsreeks in **appsettings.json** is met de naam met de indeling `<storageaccountname>_AzureStorageConnectionString`. 

1. Open het `BlobsController.cs`-bestand.

1. Voeg de methode met de naam **GetCloudBlobContainer** die retourneert een **CloudBlobContainer**. Vervang `<storageaccountname>_AzureStorageConnectionString` met de werkelijke naam van de sleutel in **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json");
        IConfigurationRoot Configuration = builder.Build();
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.Parse(Configuration["ConnectionStrings:aspnettutorial_AzureStorageConnectionString"]);
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> Hoewel *test-blob-container* niet bestaat nog, deze code maakt u een verwijzing naar deze. Dit is de container kan worden gemaakt met de `CreateIfNotExists` methode die wordt weergegeven in de volgende stap.

## <a name="create-a-blob-container"></a>Een blob-container maken

De volgende stappen laten zien hoe u een blob-container maken:

1. Voeg de methode met de naam `CreateBlobContainer` die retourneert een `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Krijgen een `CloudBlobContainer` -object met een verwijzing naar de naam van de gewenste blob-container. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Roep de `CloudBlobContainer.CreateIfNotExists` methode voor het maken van de container als deze nog niet bestaat. De `CloudBlobContainer.CreateIfNotExists` methode retourneert **waar** als de container niet bestaat en is gemaakt. Anders retourneert de methode **false**.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExistsAsync().Result;
    ```

1. Update `ViewBag` met de naam van de blob-container.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    Hieronder ziet u de voltooide `CreateBlobContainer` methode:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExistsAsync().Result;
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. In **Solution Explorer**, met de rechtermuisknop op de **weergaven** map.

2. Selecteer in het contextmenu **toevoegen** > **nieuwe map**. Naam van de nieuwe map *Blobs*. 

1. In **Solution Explorer**, vouw de **weergaven** map en klik met de rechtermuisknop **Blobs**.

4. Selecteer in het contextmenu **toevoegen** > **weergave**.

1. In de **weergave toevoegen** dialoogvenster vak, voer **CreateBlobContainer** voor de weergavenaam en selecteer **toevoegen**.

1. Open `CreateBlobContainer.cshtml`, en dit zodanig aanpassen dat het ziet als het volgende codefragment eruit:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>
    
    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. In **Solution Explorer**, vouw de **weergaven** > **gedeelde** map en open `_Layout.cshtml`.

1. Zoek naar de niet-geordende lijst er als volgt uitzien: `<ul class="nav navbar-nav">`.  Na de laatste `<li>` -element in de lijst met toevoegen de volgende HTML-code een andere navigatie menu-item toe te voegen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="CreateBlobContainer">Create blob container</a></li>
    ```

1. Voer de toepassing uit en selecteer **Blob-Container maken** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Schermafbeelding van het maken van de blob-container](./media/vs-storage-aspnet-core-getting-started-blobs/create-blob-container-results.png)

    Zoals eerder vermeld de `CloudBlobContainer.CreateIfNotExists` methode retourneert **waar** alleen wanneer de container bestaat niet en is gemaakt. Dus als de app wordt uitgevoerd wanneer de container bestaat, retourneert de methode **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Een blob uploaden naar een blob-container

Wanneer de [blob-container wordt gemaakt](#create-a-blob-container), upload bestanden in die container. In deze sectie helpt bij het uploaden van een lokaal bestand naar een blob-container. De stappen wordt ervan uitgegaan dat er is een blob-container met de naam *test-blob-container*. 

1. Open het `BlobsController.cs`-bestand.

1. Voeg de methode met de naam `UploadBlob` die retourneert een tekenreeks.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Binnen de `UploadBlob` methode, krijgen een `CloudBlobContainer` -object met een verwijzing naar de naam van de gewenste blob-container. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Azure storage ondersteunt verschillende blob-typen. In deze zelfstudie maakt gebruik van blok-blobs. Als u wilt ophalen van een verwijzing naar een blok-blob, aanroepen de `CloudBlobContainer.GetBlockBlobReference` methode.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > De blobnaam maakt deel uit van de URL die wordt gebruikt voor het ophalen van een blob, en kan er een willekeurige tekenreeks, met inbegrip van de naam van het bestand.

1. Nadat er een verwijzing naar het blob is, kunt u een gegevensstroom naar uploaden door het aanroepen van de blob-verwijzingsobject `UploadFromStream` methode. De `UploadFromStream` methode wordt de blob gemaakt als deze niet bestaat, of overschreven als deze bestaat. (Wijziging  *&lt;uploaden bestand >* naar een volledig gekwalificeerde pad naar een bestand dat moet worden geüpload.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStreamAsync(fileStream).Wait();
    }
    ```
    
    Hieronder ziet u de voltooide `UploadBlob` methode (met een volledig gekwalificeerde pad voor het bestand dat moet worden geüpload):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. In **Solution Explorer**, vouw de **weergaven** > **gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste `<li>` -element in de lijst met toevoegen de volgende HTML-code een andere navigatie menu-item toe te voegen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="UploadBlob">Upload blob</a></li>
    ```

1. Voer de toepassing uit en selecteer **blob uploaden**. Het woord *succes!* moet worden weergegeven.
    
    ![Schermopname van geslaagde verificatie](./media/vs-storage-aspnet-core-getting-started-blobs/upload-blob.png)
  
## <a name="list-the-blobs-in-a-blob-container"></a>De blobs in een blob-container te vermelden

In deze sectie ziet u hoe u de blobs in een blob-container te vermelden. De voorbeeld-code verwijst naar de *test-blob-container* gemaakt in de sectie [maken van een blob-container](#create-a-blob-container).

1. Open het `BlobsController.cs`-bestand.

1. Voeg de methode met de naam `ListBlobs` die retourneert een `ActionResult`.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. Binnen de `ListBlobs` methode, krijgen een `CloudBlobContainer` -object met een verwijzing naar de blob-container. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Als u wilt de blobs in een blob-container te vermelden, gebruikt u de `CloudBlobContainer.ListBlobsSegmentedAsync` methode. De `CloudBlobContainer.ListBlobsSegmentedAsync` methode retourneert een `BlobResultSegment`. Dit document bevat `IListBlobItem` objecten die kunnen worden geconverteerd naar `CloudBlockBlob`, `CloudPageBlob`, of `CloudBlobDirectory` objecten. Het volgende codefragment inventariseren alle blobs in een blob-container. Elke blob is geconverteerd naar het desbetreffende object, op basis van het type. De naam ervan (of URI in het geval van een `CloudBlobDirectory`) wordt toegevoegd aan een lijst.

    ```csharp
    List<string> blobs = new List<string>();
    BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
    foreach (IListBlobItem item in resultSegment.Results)
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```
    Hieronder ziet u de voltooide `ListBlobs` methode:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. In **Solution Explorer**, vouw de **weergaven** map en klik met de rechtermuisknop **Blobs**.

2. Selecteer in het contextmenu **toevoegen** > **weergave**.

1. In de **weergave toevoegen** dialoogvenster vak, voer `ListBlobs` voor de weergavenaam en selecteer **toevoegen**.

1. Open `ListBlobs.cshtml`, en vervang de inhoud door de volgende code:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
            <li>@item</li>
        }
    </ul>
    ```

1. In **Solution Explorer**, vouw de **weergaven** > **gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste `<li>` -element in de lijst met toevoegen de volgende HTML-code een andere navigatie menu-item toe te voegen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="ListBlobs">List blobs</a></li>
    ```

1. Voer de toepassing uit en selecteer **blobs te vermelden** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Schermafbeelding van de lijst met blobs](./media/vs-storage-aspnet-core-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blobs downloaden

In deze sectie ziet u hoe u om een blob te downloaden. U kunt deze bewaard in een lokale opslag of de inhoud lezen in een tekenreeks. De voorbeeld-code verwijst naar de *test-blob-container* gemaakt in de sectie [maken van een blob-container](#create-a-blob-container).

1. Open het `BlobsController.cs`-bestand.

1. Voeg de methode met de naam `DownloadBlob` die retourneert een tekenreeks.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Binnen de `DownloadBlob` methode, krijgen een `CloudBlobContainer` -object met een verwijzing naar de blob-container.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Een blob-referentie-object ophalen door het aanroepen van de `CloudBlobContainer.GetBlockBlobReference` methode. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Als u wilt een blob downloaden, gebruikt u de `CloudBlockBlob.DownloadToStream` methode. De volgende code overdraagt van een blob-inhoud naar een stream-object. Dit object worden vervolgens naar een lokaal bestand opgeslagen. (Wijziging  *&lt;lokale-file-name >* naar de volledig gekwalificeerde bestand naam die aangeeft waar de blob is om te worden gedownload.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStreamAsync(fileStream).Wait();
    }
    ```
    
    Hieronder ziet u de voltooide `ListBlobs` methode (met een volledig gekwalificeerde pad voor het lokale bestand wordt gemaakt):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. In **Solution Explorer**, vouw de **weergaven** > **gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste `<li>` -element in de lijst met toevoegen de volgende HTML-code een andere navigatie menu-item toe te voegen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DownloadBlob">Download blob</a></li>
    ```

1. Voer de toepassing uit en selecteer **downloaden blob** om de blob te downloaden. De blob die is opgegeven in de `CloudBlobContainer.GetBlockBlobReference` methodeaanroep downloadt naar de locatie die is opgegeven de `File.OpenWrite` aanroep van methode. De tekst *succes!* moet worden weergegeven in de browser. 

## <a name="delete-blobs"></a>Blobs verwijderen

De volgende stappen laten zien hoe u een blob verwijderen:

1. Open het `BlobsController.cs`-bestand.

1. Voeg de methode met de naam `DeleteBlob` die retourneert een tekenreeks.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. Binnen de `DeleteBlob` methode, krijgen een `CloudBlobContainer` -object met een verwijzing naar de blob-container.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Een blob-referentie-object ophalen door het aanroepen van de `CloudBlobContainer.GetBlockBlobReference` methode. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Als u wilt verwijderen van een blob, gebruikt u de `Delete` methode.

    ```csharp
    blob.DeleteAsync().Wait();
    ```
    
    De voltooide `DeleteBlob` methode moet er als volgt uitzien:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.DeleteAsync().Wait();
        return "success!";
    }
    ```

1. In **Solution Explorer**, vouw de **weergaven** > **gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste `<li>` -element in de lijst met toevoegen de volgende HTML-code een andere navigatie menu-item toe te voegen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DeleteBlob">Delete blob</a></li>
    ```

1. Voer de toepassing uit en selecteer **verwijderen blob** verwijderen van de blob die is opgegeven in de `CloudBlobContainer.GetBlockBlobReference` aanroep van methode. De tekst *succes!* moet worden weergegeven in de browser. Selecteren van de browser **terug** knop en selecteer vervolgens **blobs te vermelden** om te controleren of de blob niet meer in de container.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u opslaat, lijst en blobs in Azure Storage ophalen met behulp van ASP.NET Core. Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.

  * [Aan de slag met Azure Table storage en Visual Studio verbonden services (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Aan de slag met Azure Queue storage en Visual Studio verbonden services (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
