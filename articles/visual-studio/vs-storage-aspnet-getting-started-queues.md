---
title: Aan de slag met Azure queue storage en Visual Studio verbonden Services (ASP.NET) | Microsoft Docs
description: Aan de slag met behulp van Azure-wachtrijopslag nadat u verbinding met een opslagaccount met behulp van Visual Studio Connected Services in een ASP.NET-project in Visual Studio
services: storage
author: ghogen
manager: douge
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/23/2016
ms.author: ghogen
ms.openlocfilehash: 3f3a649308449a8ba4e1dbaebd4db4afbf731c5b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58105757"
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Aan de slag met Azure queue storage en Visual Studio verbonden Services (ASP.NET)

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Azure queue storage biedt cloudberichtuitwisseling tussen toepassingscomponenten. Bij het ontwerpen van schaalbare toepassingen worden toepassingsonderdelen vaak ontkoppeld, zodat ze onafhankelijk van elkaar kunnen worden geschaald. Queue Storage biedt asynchrone uitwisseling van berichten voor communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt daarnaast ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

Deze zelfstudie laat zien hoe u ASP.NET-code voor enkele algemene scenario's met behulp van Azure queue storage-entiteiten te schrijven. Deze scenario's omvatten algemene taken, zoals het maken van een Azure-wachtrij en toevoegen, wijzigen, lezen en verwijderen van Wachtrijberichten.

## <a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-account](../storage/common/storage-quickstart-create-account.md)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Maken van een MVC-controller 

1. In de **Solution Explorer**, met de rechtermuisknop op **Controllers**, en selecteer in het contextmenu **Add -> Controller**.

    ![Een domeincontroller toevoegen aan een ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. Op de **Add Scaffold** dialoogvenster, selecteer **MVC 5 Controller - leeg**, en selecteer **toevoegen**.

    ![MVC-controller opgeven](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. Op de **Controller toevoegen** dialoogvenster, de naam van de controller *QueuesController*, en selecteer **toevoegen**.

    ![De naam van de MVC-controller](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Voeg de volgende *met behulp van* instructies aan het `QueuesController.cs` bestand:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
   ## <a name="create-a-queue"></a>Een wachtrij maken

De volgende stappen laten zien hoe u een wachtrij maken:

> [!NOTE]
> 
> In deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand. 

1. Voeg de methode met de naam **CreateQueue** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **CreateQueue** methode, krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service: (Wijziging  *&lt;storage-account-name >* op de naam van de Azure-opslagaccount u toegang wilt krijgen tot.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Krijgen een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Krijgen een **CloudQueue** object op dat een verwijzing naar de gewenste wachtrijnaam vertegenwoordigt. De **CloudQueueClient.GetQueueReference** methode maakt een aanvraag indient voor queue-opslag. De verwijzing wordt geretourneerd, ongeacht of de wachtrij bestaat of niet. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.CreateIfNotExists** methode voor het maken van de wachtrij als deze nog niet bestaat. De **CloudQueue.CreateIfNotExists** methode retourneert **waar** als de wachtrij niet bestaat en is gemaakt. Anders **false** wordt geretourneerd.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. Update de **ViewBag** met de naam van de wachtrij.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **Add -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **CreateQueue** voor de weergavenaam en selecteer **toevoegen**.

1. Open `CreateQueue.cshtml`, en dit zodanig aanpassen dat het ziet als het volgende codefragment eruit:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Voer de toepassing uit en selecteer **wachtrij maken** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Wachtrij maken](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Zoals eerder vermeld de **CloudQueue.CreateIfNotExists** methode retourneert **waar** alleen wanneer de wachtrij bestaat niet en is gemaakt. Dus als u de app uitvoert wanneer de wachtrij bestaat, retourneert de methode **false**. Als u wilt de app meerdere keren uitvoeren, moet u de wachtrij verwijderen voordat u de app nogmaals uit te voeren. Verwijderen van de wachtrij kan worden gedaan de **CloudQueue.Delete** methode. U kunt ook verwijderen de wachtrij met behulp van de [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) of de [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij

Nadat u hebt [een wachtrij gemaakt](#create-a-queue), u kunt berichten naar die wachtrij toevoegen. In deze sectie helpt u bij het toevoegen van een bericht naar een wachtrij *test-wachtrij*. 

> [!NOTE]
> 
> In deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Voeg de methode met de naam **AddMessage** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **AddMessage** methode, krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service: (Wijziging  *&lt;storage-account-name >* op de naam van de Azure-opslagaccount u toegang wilt krijgen tot.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Krijgen een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Krijgen een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Maak de **CloudQueueMessage** -object als het bericht dat u wilt toevoegen aan de wachtrij. Een **CloudQueueMessage** object kan worden gemaakt op basis van een tekenreeks (in UTF-8-indeling) of een bytematrix.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. Roep de **CloudQueue.AddMessage** methode de messaged toevoegen aan de wachtrij.

    ```csharp
    queue.AddMessage(message);
    ```

1. Maken en instellen van een aantal **ViewBag** eigenschappen voor weergave in de weergave.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **Add -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **AddMessage** voor de weergavenaam en selecteer **toevoegen**.

1. Open `AddMessage.cshtml`, en dit zodanig aanpassen dat het ziet als het volgende codefragment eruit:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Voer de toepassing uit en selecteer **bericht toevoegen** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Bericht toevoegen](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

De twee secties - [een bericht zonder deze te verwijderen uit een wachtrij gelezen](#read-a-message-from-a-queue-without-removing-it) en [lezen en verwijderen van een bericht van een wachtrij](#read-and-remove-a-message-from-a-queue) -laten zien hoe u berichten in een wachtrij gelezen.    

## <a name="read-a-message-from-a-queue-without-removing-it"></a>Een bericht in een wachtrij gelezen zonder deze te verwijderen

In deze sectie ziet u hoe u kunt bekijken van een bericht in de wachtrij (het eerste bericht lezen zonder deze te verwijderen).  

> [!NOTE]
> 
> In deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Voeg de methode met de naam **PeekMessage** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **PeekMessage** methode, krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service: (Wijziging  *&lt;storage-account-name >* op de naam van de Azure-opslagaccount u toegang wilt krijgen tot.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Krijgen een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Krijgen een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.PeekMessage** methode voor het lezen van het eerste bericht in de wachtrij zonder deze te verwijderen uit de wachtrij. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. Update de **ViewBag** met twee waarden: naam van de wachtrij en het bericht dat is gelezen. De **CloudQueueMessage** object beschrijft de twee eigenschappen voor het ophalen van de waarde van het object: **CloudQueueMessage.AsBytes** en **CloudQueueMessage.AsString**. **AsString** (gebruikt in dit voorbeeld) retourneert een tekenreeks, terwijl **AsBytes** retourneert een matrix van bytes.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **Add -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **PeekMessage** voor de weergavenaam en selecteer **toevoegen**.

1. Open `PeekMessage.cshtml`, en dit zodanig aanpassen dat het ziet als het volgende codefragment eruit:

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

1. Na de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Voer de toepassing uit en selecteer **message Peek** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Bericht bekijken](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>Lezen en verwijderen van een bericht van een wachtrij

In deze sectie leert u hoe u om te lezen en verwijderen van een bericht van een wachtrij.   

> [!NOTE]
> 
> In deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Voeg de methode met de naam **gelezenBericht** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **gelezenBericht** methode, krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service: (Wijziging  *&lt;storage-account-name >* op de naam van de Azure-opslagaccount u toegang wilt krijgen tot.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Krijgen een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Krijgen een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.GetMessage** methode voor het lezen van het eerste bericht in de wachtrij. De **CloudQueue.GetMessage** methode wordt het bericht onzichtbaar gedurende 30 seconden (standaard) voor andere codes die berichten lezen zodat er geen andere code kunt wijzigen of verwijderen van het bericht tijdens de verwerking. Als u wilt wijzigen van de hoeveelheid tijd die het bericht onzichtbaar is, wijzig de **visibilityTimeout** parameter die wordt doorgegeven aan de **CloudQueue.GetMessage** methode.

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. Roep de **CloudQueueMessage.Delete** methode voor het verwijderen van het bericht uit de wachtrij.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Update de **ViewBag** met het bericht is verwijderd, en de naam van de wachtrij.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **Add -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **gelezenBericht** voor de weergavenaam en selecteer **toevoegen**.

1. Open `ReadMessage.cshtml`, en dit zodanig aanpassen dat het ziet als het volgende codefragment eruit:

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

1. Na de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Voer de toepassing uit en selecteer **lezen/verwijderen bericht** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Lezen en verwijderen van bericht](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

In deze sectie ziet u hoe u aan de lengte van de wachtrij (aantal berichten). 

> [!NOTE]
> 
> In deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Voeg de methode met de naam **GetQueueLength** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **gelezenBericht** methode, krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service: (Wijziging  *&lt;storage-account-name >* op de naam van de Azure-opslagaccount u toegang wilt krijgen tot.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Krijgen een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Krijgen een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.FetchAttributes** methode voor het ophalen van de wachtrij-kenmerken (met inbegrip van de lengte ervan). 

    ```csharp
    queue.FetchAttributes();
    ```

1. Toegang tot de **CloudQueue.ApproximateMessageCount** eigenschap lengte van de wachtrij ophalen.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. Update de **ViewBag** met de naam van de wachtrij en de lengte ervan.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **Add -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **GetQueueLength** voor de weergavenaam en selecteer **toevoegen**.

1. Open `GetQueueLengthMessage.cshtml`, en dit zodanig aanpassen dat het ziet als het volgende codefragment eruit:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Voer de toepassing uit en selecteer **ophalen wachtrijlengte** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Lengte van de wachtrij ophalen](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Een wachtrij verwijderen
In deze sectie ziet u hoe u een wachtrij verwijderen. 

> [!NOTE]
> 
> In deze sectie wordt ervan uitgegaan dat u de stappen hebt voltooid [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `QueuesController.cs`-bestand.

1. Voeg de methode met de naam **DeleteQueue** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Binnen de **DeleteQueue** methode, krijgen een **CloudStorageAccount** -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service: (Wijziging  *&lt;storage-account-name >* op de naam van de Azure-opslagaccount u toegang wilt krijgen tot.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Krijgen een **CloudQueueClient** object vertegenwoordigt een queue-serviceclient.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Krijgen een **CloudQueueContainer** -object met een verwijzing naar de wachtrij. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Roep de **CloudQueue.Delete** methode voor het verwijderen van de wachtrij die wordt vertegenwoordigd door de **CloudQueue** object.

    ```csharp
    queue.Delete();
    ```

1. Update de **ViewBag** met de naam van de wachtrij en de lengte ervan.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **wachtrijen**, en selecteer in het contextmenu **Add -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **DeleteQueue** voor de weergavenaam en selecteer **toevoegen**.

1. Open `DeleteQueue.cshtml`, en dit zodanig aanpassen dat het ziet als het volgende codefragment eruit:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Na de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Voer de toepassing uit en selecteer **ophalen wachtrijlengte** om te zien van de resultaten die vergelijkbaar is met de volgende schermafbeelding:
  
    ![Wachtrij verwijderen](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Volgende stappen
Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.

  * [Aan de slag met Azure blob storage en Visual Studio verbonden Services (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Aan de slag met Azure table storage en Visual Studio verbonden Services (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
