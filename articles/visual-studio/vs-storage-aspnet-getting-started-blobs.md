---
title: Aan de slag met Azure blob storage en Visual Studio verbonden Services (ASP.NET) | Microsoft Docs
description: Hoe u aan de slag met Azure blob-opslag in een ASP.NET-project in Visual Studio nadat u een opslagaccount met behulp van Visual Studio verbonden Services
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: kraig
ms.openlocfilehash: f0d0815e71149749cb52efe21e1f0af3cabae21c
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Aan de slag met Azure blob storage en Visual Studio verbonden Services (ASP.NET)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure blob-opslag is een service die niet-gestructureerde gegevens in de cloud als objecten/blobs opslaat. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag.

Deze zelfstudie laat zien hoe ASP.NET code schrijven voor enkele algemene scenario's met behulp van Azure blob-opslag. Scenario's omvatten een blob-container maken en uploaden, aanbieding, downloaden en verwijderen van blobs.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]


[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Maken van een MVC-controller 

1. In de **Solution Explorer**, met de rechtermuisknop op **domeincontrollers**, en selecteer in het contextmenu **toevoegen -> Controller**.

    ![Een domeincontroller toevoegen aan een ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. Op de **Add Scaffold** dialoogvenster Selecteer **MVC 5 Controller - leeg**, en selecteer **toevoegen**.

    ![Geef een MVC-controller](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. Op de **Controller toevoegen** dialoogvenster, de naam van de controller *BlobsController*, en selecteer **toevoegen**.

    ![De naam van de MVC-controller](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Voeg de volgende *met* richtlijnen voor de `BlobsController.cs` bestand:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Verbinding maken met een opslagaccount en een containerverwijzing ophalen

Een blob-container is een geneste hiërarchie blobs en-mappen.  De rest van de stappen in dit document moet een verwijzing naar een blob-container code moet worden geplaatst op een eigen methode voor hergebruik.

De volgende stappen maakt u een methode voor het verbinding maken met de storage-account met de verbindingsreeks in **Web.config** en een verwijzing naar een container maken.  De instelling voor de tekenreeks in **Web.config** worden benoemd met de indeling `<storageaccountname>_AzureStorageConnectionString`. 

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **GetCloudBlobContainer** die retourneert een **CloudBlobContainer**.  Zorg ervoor dat u `<storageaccountname>_AzureStorageConnectionString` met de werkelijke naam van de sleutel in **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
                CloudConfigurationManager.GetSetting("<storageaccountname>_AzureStorageConnectionString"));
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> Hoewel *test blobcontainer* komt niet voor deze code wordt nog een verwijzing naar deze zodat de container kan worden gemaakt met de `CreateIfNotExists` methode die wordt weergegeven in de volgende stap.

## <a name="create-a-blob-container"></a>Een blob-container maken

De volgende stappen laten zien hoe u van een blob-container:

1. Toevoegen van een methode aangeroepen `CreateBlobContainer` die retourneert een `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Ophalen van een `CloudBlobContainer` -object met een verwijzing naar de naam van de gewenste blob-container. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Roep de `CloudBlobContainer.CreateIfNotExists` methode voor het maken van de container als deze nog niet bestaat. De `CloudBlobContainer.CreateIfNotExists` methode retourneert **true** als de container niet bestaat en is gemaakt. Anders **false** wordt geretourneerd.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Update de `ViewBag` met de naam van de blob-container.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    Hieronder vindt u de voltooide `CreateBlobContainer` methode:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExists();
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. In de **Solution Explorer**, met de rechtermuisknop op de **weergaven** map, en selecteer in het contextmenu **toevoegen -> nieuwe map**. Naam van de nieuwe map *Blobs*. 
 
1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **Blobs**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **CreateBlobContainer** voor de weergavenaam en selecteer **toevoegen**.

1. Open `CreateBlobContainer.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **Blob-Container maken** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Blob-container maken](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Zoals eerder vermeld de `CloudBlobContainer.CreateIfNotExists` methode retourneert **true** alleen wanneer de container bestaat niet en is gemaakt. Daarom als de app wordt uitgevoerd wanneer de container bestaat, retourneert de methode **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Een blob uploaden naar een blob-container

Eenmaal de [blobcontainer gemaakt](#create-a-blob-container), upload bestanden in die container. Deze sectie helpt bij het uploaden van een lokaal bestand naar een blob-container. De stappen wordt ervan uitgegaan dat er is een blob-container met de naam *test blobcontainer*. 

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen `UploadBlob` die een tekenreeks retourneert.

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

1. Zoals eerder beschreven, ondersteunt Azure storage blob met verschillende typen. Deze zelfstudie maakt gebruik van blok-blobs.  Aanroepen voor het ophalen van een verwijzing naar een blok-blob, de `CloudBlobContainer.GetBlockBlobReference` methode.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > De blob-naam maakt deel uit van de URL die wordt gebruikt voor het ophalen van een blob en kan een willekeurige tekenreeks, inclusief de naam van het bestand.

1. Zodra er een verwijzing naar een blob, uploadt u eventuele gegevensstroom naar het door het aanroepen van de blob-referentieobject `UploadFromStream` methode. De `UploadFromStream` methode maakt u de blob als deze niet bestaat, of deze wordt overschreven als deze bestaat. (Wijziging  *&lt;uploaden bestand >* naar een volledig gekwalificeerde pad naar een bestand te uploaden.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStream(fileStream);
    }
    ```
    
    Hieronder vindt u de voltooide `UploadBlob` methode (met een volledig gekwalificeerde pad voor het bestand moet worden geüpload):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStream(fileStream);
        }
        return "success!";
    }
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **blob uploaden**.  Het woord "success!" moet worden weergegeven.
    
    ![Verificatie geslaagd](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
De sectie - [lijst van de blobs in een blob-container](#list-the-blobs-in-a-blob-container) -ziet u hoe u de blobs in een blob-container.    

## <a name="list-the-blobs-in-a-blob-container"></a>Lijst van de blobs in een blob-container

Deze sectie ziet u hoe u de blobs in een blob-container. De voorbeeld-code verwijst naar de *test blobcontainer* gemaakt in de sectie [maken van een blob-container](#create-a-blob-container).

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen `ListBlobs` die retourneert een `ActionResult`.

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
   
1. Als de blobs in een blob-container wilt weergeven, gebruikt de `CloudBlobContainer.ListBlobs` methode. De `CloudBlobContainer.ListBlobs` methode retourneert een `IListBlobItem` -object dat kan worden geconverteerd naar een `CloudBlockBlob`, `CloudPageBlob`, of `CloudBlobDirectory` object. Het volgende codefragment inventariseren alle blobs in een blob-container. Elke blob is geconverteerd naar het juiste object op basis van het type en de naam ervan (of URI in het geval van een **CloudBlobDirectory**) is toegevoegd aan de lijst.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs())
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

    Naast blobs, kunnen de blob-containers mappen bevatten. Stel dat er is een blob-container aangeroepen *test blobcontainer* met de volgende hiërarchie:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Met behulp van het voorgaande voorbeeld, de **blobs** lijst vergelijkbaar met de volgende waarden bevat:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Zoals u ziet, bevat de lijst alleen de site op het hoogste entiteiten; geen geneste die (*bar.png* en *baz.png*). Als alle entiteiten in een blob-container wilt weergeven, de code te wijzigen zodat de **CloudBlobContainer.ListBlobs** wordt doorgegeven aan methode **true** voor de **useFlatBlobListing** parameter.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    Instellen van de **useFlatBlobListing** -parameter voor **true** retourneert een platte lijst van alle entiteiten in de blob-container en levert de volgende resultaten:

        foo.png
        dir1/bar.png
        dir2/baz.png
    
    Hieronder vindt u de voltooide **ListBlobs** methode:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing: true))
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

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **Blobs**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer `ListBlobs` voor de weergavenaam en selecteer **toevoegen**.

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

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **lijst blobs** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![BLOB-aanbieding](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blobs downloaden

Deze sectie ziet u hoe u een blob te downloaden en een persistent maken met lokale opslag of de inhoud lezen in een tekenreeks. De voorbeeld-code verwijst naar de *test blobcontainer* gemaakt in de sectie [maken van een blob-container](#create-a-blob-container).

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen `DownloadBlob` die een tekenreeks retourneert.

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

1. Een blob reference-object ophalen door het aanroepen van `CloudBlobContainer.GetBlockBlobReference` methode. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Als u wilt downloaden van een blob de `CloudBlockBlob.DownloadToStream` methode. De volgende code een blob-inhoud overgebracht naar een stroomobject dat vervolgens wordt opgeslagen in een lokaal bestand (wijziging  *&lt;lokale bestandsnaam >* naar de volledige bestandsnaam de naam die aangeeft waar de blob is worden gedownload.): 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```
    
    Hieronder vindt u de voltooide `ListBlobs` methode (met een volledig gekwalificeerde pad voor het lokale bestand wordt gemaakt):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStream(fileStream);
        }
        return "success!";
    }
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **downloaden blob** voor het downloaden van de blob. De blob die is opgegeven in de `CloudBlobContainer.GetBlockBlobReference` methodeaanroep downloadt naar de opgegeven locatie in de `File.OpenWrite` methodeaanroep.  De tekst "success!" moet worden weergegeven in de browser. 

## <a name="delete-blobs"></a>Blobs verwijderen

De volgende stappen uit te laten zien hoe u een blob verwijderen:

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen `DeleteBlob` die een tekenreeks retourneert.

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

1. Een blob reference-object ophalen door het aanroepen van `CloudBlobContainer.GetBlockBlobReference` methode. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Als u wilt verwijderen van een blob, gebruiken de `Delete` methode.

    ```csharp
    blob.Delete();
    ```
    
    De voltooide `DeleteBlob` methode ziet er als volgt:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.Delete();
        return "success!";
    }
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **verwijderen blob** verwijderen van de blob die is opgegeven in de `CloudBlobContainer.GetBlockBlobReference` methodeaanroep.  De tekst "success!" moet worden weergegeven in de browser.  Klik op de browser **terug** knop en selecteer vervolgens **lijst blobs** om te controleren of de blob is niet langer in de container.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe opslaan, weergeven en ophalen van de blobs in Azure Storage met ASP.NET.  Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.

  * [Aan de slag met Azure-tabelopslag en Visual Studio verbonden Services (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Aan de slag met Azure queue storage- en Visual Studio verbonden Services (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
