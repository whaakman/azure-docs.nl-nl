---
title: Aan de slag met Azure queue storage- en Visual Studio verbonden Services (ASP.NET) | Microsoft Docs
description: Hoe u aan de slag met Azure queue storage in een ASP.NET-project in Visual Studio nadat u een opslagaccount met behulp van Visual Studio verbonden Services
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/23/2016
ms.author: kraigb
ms.openlocfilehash: 4687e5dfce72583728068c176d86d100313badf6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Aan de slag met Azure queue storage- en Visual Studio verbonden Services (ASP.NET)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Azure queue storage biedt cloud messaging tussen toepassingsonderdelen. Bij het ontwerpen van schaalbare toepassingen worden toepassingsonderdelen vaak ontkoppeld, zodat ze onafhankelijk van elkaar kunnen worden geschaald. Queue Storage biedt asynchrone uitwisseling van berichten voor communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt daarnaast ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

Deze zelfstudie laat zien hoe ASP.NET code schrijven voor enkele algemene scenario's met Azure queue storage entiteiten. Deze scenario's omvatten algemene taken, zoals het maken van een Azure-wachtrij en toevoegen, wijzigen, lezen en verwijderen van Wachtrijberichten.

##<a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Maken van een MVC-controller 

1. In de **Solution Explorer**, met de rechtermuisknop op **domeincontrollers**, en selecteer in het contextmenu **toevoegen -> Controller**.

    ![Een domeincontroller toevoegen aan een ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. Op de **Add Scaffold** dialoogvenster Selecteer **MVC 5 Controller - leeg**, en selecteer **toevoegen**.

    ![Geef een MVC-controller](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. Op de **Controller toevoegen** dialoogvenster, de naam van de controller *QueuesController*, en selecteer **toevoegen**.

    ![De naam van de MVC-controller](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Voeg de volgende *met* richtlijnen voor de `QueuesController.cs` bestand:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a>Een wachtrij maken

De volgende stappen laten zien hoe een wachtrij maken:

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand. 

1. Toevoegen van een methode aangeroepen **CreateQueue** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **CreateQueue** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Ophalen van een **CloudQueue** -object met een verwijzing naar de naam van de gewenste wachtrij. De **CloudQueueClient.GetQueueReference** methode heeft een aanvraag in voor wachtrijopslag moet fungeren niet maken. De verwijzing wordt geretourneerd of de wachtrij bestaat of niet. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.CreateIfNotExists** methode voor het maken van de wachtrij als deze nog niet bestaat. De **CloudQueue.CreateIfNotExists** methode retourneert **true** als de wachtrij niet bestaat en is gemaakt. Anders **false** wordt geretourneerd.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. Update de **ViewBag** met de naam van de wachtrij.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **CreateQueue** voor de weergavenaam en selecteer **toevoegen**.

1. Open `CreateQueue.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Voer de toepassing en selecteer **wachtrij maken** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Wachtrij maken](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Zoals eerder vermeld de **CloudQueue.CreateIfNotExists** methode retourneert **true** alleen wanneer de wachtrij bestaat niet en is gemaakt. Dus als u de app wordt uitgevoerd wanneer de wachtrij bestaat, de methode retourneert **false**. Als u wilt de app meerdere keren uitvoert, moet u de wachtrij verwijderen voordat u de app opnieuw uitvoert. Kan het verwijderen van de wachtrij worden uitgevoerd via de **CloudQueue.Delete** methode. U kunt ook verwijderen voor de wachtrij met behulp van de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) of de [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij

Zodra u hebt [wachtrij](#create-a-queue), kunt u berichten naar die wachtrij toevoegen. Deze sectie helpt u bij het toevoegen van een bericht naar een wachtrij *test wachtrij*. 

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **AddMessage** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **AddMessage** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Ophalen van een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Maak de **CloudQueueMessage** -object dat het bericht dat u wilt toevoegen aan de wachtrij aangeeft. Een **CloudQueueMessage** object kan worden gemaakt vanuit een tekenreeks (in UTF-8-indeling) of een byte-matrix.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. Roep de **CloudQueue.AddMessage** methode de messaged toevoegen aan de wachtrij.

    ```csharp
    queue.AddMessage(message);
    ```

1. Maken en een aantal **ViewBag** eigenschappen voor weergave in de weergave.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **AddMessage** voor de weergavenaam en selecteer **toevoegen**.

1. Open `AddMessage.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Voer de toepassing en selecteer **toevoegen bericht** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Bericht toevoegen](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

De twee secties - [een bericht in een wachtrij lezen zonder het te verwijderen](#read-a-message-from-a-queue-without-removing-it) en [lezen en verwijderen van een bericht van een wachtrij](#read-and-remove-a-message-from-a-queue) -te laten zien hoe u berichten in een wachtrij lezen.    

## <a name="read-a-message-from-a-queue-without-removing-it"></a>Een bericht in een wachtrij lezen zonder het te verwijderen

Deze sectie ziet u hoe bekijken van een wachtrij bericht (het eerste bericht gelezen zonder het te verwijderen).  

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **PeekMessage** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **PeekMessage** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Ophalen van een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.PeekMessage** methode om te lezen van het eerste bericht in de wachtrij zonder het te verwijderen uit de wachtrij. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. Update de **ViewBag** met twee waarden: naam van de wachtrij en het bericht dat is gelezen. De **CloudQueueMessage** object beschrijft de twee eigenschappen voor het ophalen van de waarde van het object: **CloudQueueMessage.AsBytes** en **CloudQueueMessage.AsString**. **AsString** (gebruikt in dit voorbeeld) retourneert een tekenreeks, terwijl **AsBytes** retourneert een bytematrix.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **PeekMessage** voor de weergavenaam en selecteer **toevoegen**.

1. Open `PeekMessage.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "PeekMessage";
    }
    
    <h2>Peek Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Peeked Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>    
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Voer de toepassing en selecteer **Peek bericht** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Bericht](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>Lezen en verwijderen van een bericht van een wachtrij

Informatie over het lezen en verwijderen van een bericht van een wachtrij in deze sectie.   

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **gelezenBericht** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **gelezenBericht** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Ophalen van een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.GetMessage** methode het eerste bericht in de wachtrij te lezen. De **CloudQueue.GetMessage** methode kunt u het bericht onzichtbaar gedurende 30 seconden (standaard) om te lezen van berichten, zodat er geen andere code kunt wijzigen of verwijderen van bericht tijdens de verwerking van deze code. Als u wilt wijzigen van de hoeveelheid tijd die het bericht onzichtbaar is, wijzigt u de **visibilityTimeout** parameter worden doorgegeven aan de **CloudQueue.GetMessage** methode.

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. Roep de **CloudQueueMessage.Delete** methode om te verwijderen van het bericht uit de wachtrij.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Update de **ViewBag** met het bericht is verwijderd en de naam van de wachtrij.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **gelezenBericht** voor de weergavenaam en selecteer **toevoegen**.

1. Open `ReadMessage.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "ReadMessage";
    }
    
    <h2>Read Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Read (and Deleted) Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Voer de toepassing en selecteer **Lees-/ verwijderingsbericht** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Lezen en verwijderen van bericht](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

Deze sectie ziet u hoe u de lengte van de wachtrij (het aantal berichten). 

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **GetQueueLength** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **gelezenBericht** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Ophalen van een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.FetchAttributes** methode voor het ophalen van de wachtrij-kenmerken (met inbegrip van de lengte). 

    ```csharp
    queue.FetchAttributes();
    ```

6. Toegang tot de **CloudQueue.ApproximateMessageCount** eigenschap ophalen van de lengte van de wachtrij.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. Update de **ViewBag** met de naam van de wachtrij en de lengte.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **GetQueueLength** voor de weergavenaam en selecteer **toevoegen**.

1. Open `GetQueueLengthMessage.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Voer de toepassing en selecteer **ophalen wachtrijlengte** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Wachtrijlengte ophalen](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Een wachtrij verwijderen
Deze sectie ziet u hoe een wachtrij verwijderen. 

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **DeleteQueue** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **DeleteQueue** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Ophalen van een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Ophalen van een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.Delete** methode om te verwijderen van de wachtrij dat wordt vertegenwoordigd door de **CloudQueue** object.

    ```csharp
    queue.Delete();
    ```

1. Update de **ViewBag** met de naam van de wachtrij en de lengte.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **DeleteQueue** voor de weergavenaam en selecteer **toevoegen**.

1. Open `DeleteQueue.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Voer de toepassing en selecteer **ophalen wachtrijlengte** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Wachtrij verwijderen](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Volgende stappen
Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.

  * [Aan de slag met Azure blob storage en Visual Studio verbonden Services (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Aan de slag met Azure-tabelopslag en Visual Studio verbonden Services (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
