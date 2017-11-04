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
ms.date: 12/21/2016
ms.author: kraig
ms.openlocfilehash: e953c7978705379a28581213e8f1c665473ddd60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Aan de slag met Azure blob storage en Visual Studio verbonden Services (ASP.NET)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure blob-opslag is een service die niet-gestructureerde gegevens in de cloud als objecten/blobs opslaat. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag.

Deze zelfstudie laat zien hoe ASP.NET code schrijven voor enkele algemene scenario's met behulp van Azure blob-opslag. Scenario's omvatten een blob-container maken en uploaden, aanbieding, downloaden en verwijderen van blobs.

##<a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

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
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>Een blob-container maken

Een blob-container is een geneste hiërarchie blobs en-mappen. De volgende stappen laten zien hoe u van een blob-container:

> [!NOTE]
> 
> De code in deze sectie wordt ervan uitgegaan dat u de stappen in de sectie hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **CreateBlobContainer** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **CreateBlobContainer** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van de Azure-service. (Wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudBlobClient** object vertegenwoordigt een blob-service-client.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Ophalen van een **CloudBlobContainer** -object met een verwijzing naar de naam van de gewenste blob-container. De **CloudBlobClient.GetContainerReference** methode maakt u niet een aanvraag bij blob storage. De verwijzing wordt geretourneerd of de blob-container bestaat of niet. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Roep de **CloudBlobContainer.CreateIfNotExists** methode voor het maken van de container als deze nog niet bestaat. De **CloudBlobContainer.CreateIfNotExists** methode retourneert **true** als de container niet bestaat en is gemaakt. Anders **false** wordt geretourneerd.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Update de **ViewBag** met de naam van de blob-container.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

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
  
    ![blob-container maken](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Zoals eerder vermeld de **CloudBlobContainer.CreateIfNotExists** methode retourneert **true** alleen wanneer de container bestaat niet en is gemaakt. Dus als u de app uitvoeren wanneer de container bestaat, retourneert de methode **false**. Als u wilt de app meerdere keren uitvoert, moet u de container verwijderen voordat u de app opnieuw uitvoert. Verwijderen van de container kan worden uitgevoerd via de **CloudBlobContainer.Delete** methode. U kunt ook verwijderen voor de container met behulp van de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) of de [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="upload-a-blob-into-a-blob-container"></a>Een blob uploaden naar een blob-container

Zodra u hebt [een blob-container gemaakt](#create-a-blob-container), kunt u bestanden uploaden naar deze container. Deze sectie helpt u bij het uploaden van een lokaal bestand naar een blob-container. De stappen wordt ervan uitgegaan dat u hebt gemaakt dat een blob-container met de naam *test blobcontainer*. 

> [!NOTE]
> 
> De code in deze sectie wordt ervan uitgegaan dat u de stappen in de sectie hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **UploadBlob** die retourneert een **EmptyResult**.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Binnen de **UploadBlob** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudBlobClient** object vertegenwoordigt een blob-service-client.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Ophalen van een **CloudBlobContainer** -object met een verwijzing naar de naam van de blob-container. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Zoals eerder beschreven, ondersteunt Azure storage blob met verschillende typen. Aanroepen voor het ophalen van een verwijzing naar een pagina-blob, de **CloudBlobContainer.GetPageBlobReference** methode. Aanroepen voor het ophalen van een verwijzing naar een blok-blob, de **CloudBlobContainer.GetBlockBlobReference** methode. Meestal is de blok-blob het aangewezen type om te gebruiken. (Wijziging < blob-naam > * op de naam die u wilt geven de blob eenmaal geüpload.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Zodra u een blobverwijzing hebt, kunt u elke gewenste gegevensstroom gegevens uploaden naar het door het aanroepen van de blob-verwijzingsobject **UploadFromStream** methode. De **UploadFromStream** methode maakt u de blob als deze niet bestaat, of deze wordt overschreven als deze bestaat. (Wijziging  *&lt;uploaden bestand >* naar een volledig gekwalificeerde pad naar het bestand dat u wilt uploaden.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **Blobs**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **blob uploaden**.  
  
De sectie - [lijst van de blobs in een blob-container](#list-the-blobs-in-a-blob-container) -ziet u hoe u de blobs in een blob-container.    

## <a name="list-the-blobs-in-a-blob-container"></a>Lijst van de blobs in een blob-container

Deze sectie ziet u hoe u de blobs in een blob-container. De voorbeeld-code verwijst naar de *test blobcontainer* gemaakt in de sectie [maken van een blob-container](#create-a-blob-container).

> [!NOTE]
> 
> De code in deze sectie wordt ervan uitgegaan dat u de stappen in de sectie hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **ListBlobs** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **ListBlobs** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudBlobClient** object vertegenwoordigt een blob-service-client.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Ophalen van een **CloudBlobContainer** -object met een verwijzing naar de naam van de blob-container. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Als de blobs in een blob-container wilt weergeven, gebruikt de **CloudBlobContainer.ListBlobs** methode. De **CloudBlobContainer.ListBlobs** methode retourneert een **IListBlobItem** -object dat u geconverteerd naar een **CloudBlockBlob**, **CloudPageBlob**, of **CloudBlobDirectory** object. Het volgende codefragment inventariseren alle blobs in een blob-container. Elke blob is geconverteerd naar het juiste object op basis van het type en de naam ervan (of URI in het geval van een **CloudBlobDirectory**) is toegevoegd aan de lijst.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

    Naast blobs, kunnen de blob-containers mappen bevatten. Stel u hebt een blob-container aangeroepen *test blobcontainer* met de volgende hiërarchie:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Met behulp van het voorgaande voorbeeld, de **blobs** lijst vergelijkbaar met de volgende waarden bevat:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Zoals u ziet, bevat de lijst alleen de site op het hoogste entiteiten; geen geneste die (*bar.png* en *baz.png*). Als alle entiteiten in een blob-container wilt weergeven, moet u aanroepen de **CloudBlobContainer.ListBlobs** methode aan en geeft **true** voor de **useFlatBlobListing** parameter.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    Instellen van de **useFlatBlobListing** -parameter voor **true** retourneert een platte lijst van alle entiteiten in de blob-container en levert de volgende resultaten:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **Blobs**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **ListBlobs** voor de weergavenaam en selecteer **toevoegen**.

1. Open `ListBlobs.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

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

1. Toevoegen van een methode aangeroepen **DownloadBlob** die retourneert een **ActionResult**.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Binnen de **DownloadBlob** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudBlobClient** object vertegenwoordigt een blob-service-client.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Ophalen van een **CloudBlobContainer** -object met een verwijzing naar de naam van de blob-container. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Een blob reference-object ophalen door het aanroepen van **CloudBlobContainer.GetBlockBlobReference** of **CloudBlobContainer.GetPageBlobReference** methode. (Wijziging  *&lt;blob-naam >* op de naam van de blob die u wilt downloaden.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Als u wilt downloaden van een blob de **CloudBlockBlob.DownloadToStream** of **CloudPageBlob.DownloadToStream** methode, afhankelijk van het blobtype. De volgende code codefragment gebruikt de **CloudBlockBlob.DownloadToStream** methode een blob-inhoud overdraagt naar een stroomobject dat vervolgens wordt opgeslagen in een lokaal bestand: (wijziging  *&lt;lokale bestandsnaam >* naar het bestand volledig gekwalificeerde naam die aangeeft waar u de blob gedownload.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **downloaden blob** voor het downloaden van de blob. De blob die is opgegeven in de **CloudBlobContainer.GetBlockBlobReference** methodeaanroep downloadt naar de locatie die u opgeeft in de **File.OpenWrite** methodeaanroep. 

## <a name="delete-blobs"></a>Blobs verwijderen

De volgende stappen uit te laten zien hoe u een blob verwijderen:

> [!NOTE]
> 
> De code in deze sectie wordt ervan uitgegaan dat u de stappen in de sectie hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `BlobsController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **DeleteBlob** die retourneert een **ActionResult**.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Ophalen van een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudBlobClient** object vertegenwoordigt een blob-service-client.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Ophalen van een **CloudBlobContainer** -object met een verwijzing naar de naam van de blob-container. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Een blob reference-object ophalen door het aanroepen van **CloudBlobContainer.GetBlockBlobReference** of **CloudBlobContainer.GetPageBlobReference** methode. (Wijziging  *&lt;blob-naam >* op de naam van de blob die u wilt verwijderen.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. To delete a blob, use the **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Voer de toepassing en selecteer **verwijderen blob** verwijderen van de blob die is opgegeven in de **CloudBlobContainer.GetBlockBlobReference** methodeaanroep. 

## <a name="next-steps"></a>Volgende stappen
Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.

  * [Aan de slag met Azure-tabelopslag en Visual Studio verbonden Services (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Aan de slag met Azure queue storage- en Visual Studio verbonden Services (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
