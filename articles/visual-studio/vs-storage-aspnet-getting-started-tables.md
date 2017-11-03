---
title: Aan de slag met Azure-tabelopslag en Visual Studio verbonden Services (ASP.NET) | Microsoft Docs
description: Hoe u aan de slag met Azure-tabelopslag in een ASP.NET-project in Visual Studio nadat u een opslagaccount met behulp van Visual Studio verbonden Services
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraigb
ms.openlocfilehash: 32a57e77bf6fe3cff88b9d6772ede9e6669ec75f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Aan de slag met Azure-tabelopslag en Visual Studio verbonden Services (ASP.NET)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Azure Table storage kunt u voor het opslaan van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen vanuit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens.

Deze zelfstudie laat zien hoe ASP.NET code schrijven voor enkele algemene scenario's met behulp van Azure table storage entiteiten. Deze scenario's omvatten maken van een tabel en toe te voegen, het uitvoeren van query's en het verwijderen van tabelentiteiten. 

##<a name="prerequisites"></a>Vereisten

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Maken van een MVC-controller 

1. In de **Solution Explorer**, met de rechtermuisknop op **domeincontrollers**, en selecteer in het contextmenu **toevoegen -> Controller**.

    ![Een domeincontroller toevoegen aan een ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. Op de **Add Scaffold** dialoogvenster Selecteer **MVC 5 Controller - leeg**, en selecteer **toevoegen**.

    ![Geef een MVC-controller](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. Op de **Controller toevoegen** dialoogvenster, de naam van de controller *TablesController*, en selecteer **toevoegen**.

    ![De naam van de MVC-controller](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. Voeg de volgende *met* richtlijnen voor de `TablesController.cs` bestand:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a>Maak een modelklasse

Veel van de voorbeelden in dit artikel gebruik een **TableEntity**-afgeleide klasse aangeroepen **CustomerEntity**. De volgende stappen begeleiden u bij het declareren van deze klasse als een modelklasse:

1. In de **Solution Explorer**, met de rechtermuisknop op **modellen**, en selecteer in het contextmenu **toevoegen -> klasse**.

1. Op de **Add New Item** dialoogvenster, de naam van de klasse **CustomerEntity**.

1. Open de `CustomerEntity.cs` bestand en voeg de volgende **met** richtlijn:

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. De klasse wijzigen zodat, wanneer u klaar bent, de klasse is gedeclareerd als in de volgende code. De klasse declareert een entiteitsklasse aangeroepen **CustomerEntity** die de voornaam van de klant gebruikt als de rijsleutel en de achternaam als de partitiesleutel.

    ```csharp
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }
    }
    ```

## <a name="create-a-table"></a>Een tabel maken

De volgende stappen uit te laten zien hoe u een tabel maken:

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u hebt de stappen in [de ontwikkelomgeving instellen](#set-up-the-development-environment). 

1. Open het `TablesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **CreateTable** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult CreateTable()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **CreateTable** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een **CloudTableClient** object vertegenwoordigt een tabelserviceclient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Ophalen van een **CloudTable** -object met een verwijzing naar de gewenste tabelnaam. De **CloudTableClient.GetTableReference** methode maakt u niet een aanvraag in voor de tabelopslag. De verwijzing wordt geretourneerd of de tabel bestaat of niet. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Roep de **CloudTable.CreateIfNotExists** methode voor het maken van de tabel als deze nog niet bestaat. De **CloudTable.CreateIfNotExists** methode retourneert **true** als de tabel niet bestaat en is gemaakt. Anders **false** wordt geretourneerd.    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. Update de **ViewBag** met de naam van de tabel.

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **tabellen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **CreateTable** voor de weergavenaam en selecteer **toevoegen**.

1. Open `CreateTable.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. Voer de toepassing en selecteer **tabel maken** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Tabel maken](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    Zoals eerder vermeld de **CloudTable.CreateIfNotExists** methode retourneert **true** alleen wanneer de tabel bestaat niet en is gemaakt. Dus als u de app uitvoeren wanneer de tabel bestaat, retourneert de methode **false**. Om de app meerdere keren uitvoert, moet u de tabel verwijderen voordat u de app opnieuw uitvoert. Kan het verwijderen van de tabel worden uitgevoerd via de **CloudTable.Delete** methode. U kunt ook verwijderen voor de tabel met behulp van de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) of de [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

*Entiteiten* zijn toegewezen aan C\# objecten met behulp van een aangepaste klasse wordt afgeleid van **TableEntity**. Als u een entiteit wilt toevoegen aan een tabel, maakt u een klasse die de eigenschappen van uw entiteit definieert. In deze sectie ziet u hoe u definieert een entiteitsklasse die de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel gebruikt. De partitie- en rijsleutel van een entiteit vormen samen de unieke id van de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel kunnen sneller worden opgevraagd dan entiteiten met verschillende partitiesleutels, maar het gebruik van verschillende partitiesleutels maakt een grotere schaalbaarheid van parallelle bewerkingen mogelijk. Voor elke eigenschap die moet worden opgeslagen in de tabelservice, moet de eigenschap een openbare eigenschap van een ondersteund type dat toegang biedt tot zowel instellen en ophalen van waarden.
De entiteitsklasse *moet* declareren van een openbare parameterloze constructor.

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u hebt de stappen in [de ontwikkelomgeving instellen](#set-up-the-development-environment).

1. Open het `TablesController.cs`-bestand.

1. Voeg de volgende instructie toe zodat de code in de `TablesController.cs` bestand heeft toegang tot de **CustomerEntity** klasse:

    ```csharp
    using StorageAspnet.Models;
    ```

1. Toevoegen van een methode aangeroepen **AddEntity** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult AddEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **AddEntity** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een **CloudTableClient** object vertegenwoordigt een tabelserviceclient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Ophalen van een **CloudTable** -object met een verwijzing naar de tabel waarnaar u wilt toevoegen, de nieuwe entiteit. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Exemplaar maken en initialiseren de **CustomerEntity** klasse.

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. Maak een **TableOperation** -object dat de klantentiteit wordt ingevoegd.

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. De insert-bewerking uitvoeren door het aanroepen van de **CloudTable.Execute** methode. U kunt het resultaat van de bewerking controleren door te inspecteren de **TableResult.HttpStatusCode** eigenschap. Statuscode 2xx geeft de actie die is aangevraagd door de client is verwerkt. Geslaagde invoegingen van nieuwe entiteiten resulteert in een HTTP-statuscode van 204, wat betekent dat de bewerking met succes is verwerkt en de server heeft bijvoorbeeld geen inhoud geretourneerd.

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. Update de **ViewBag** met de tabelnaam en de resultaten van de bewerking insert.

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **tabellen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **AddEntity** voor de weergavenaam en selecteer **toevoegen**.

1. Open `AddEntity.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. Voer de toepassing en selecteer **entiteit toevoegen** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Entiteit toevoegen](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    U kunt controleren of de entiteit is toegevoegd door de stappen in de sectie [één entiteit ophalen](#get-a-single-entity). U kunt ook de [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) om weer te geven van alle entiteiten voor de tabellen.

## <a name="add-a-batch-of-entities-to-a-table"></a>Een batch entiteiten toevoegen aan een tabel

Naast [een entiteit toevoegen aan een tabel een tegelijk](#add-an-entity-to-a-table), u kunt ook entiteiten in batch toevoegen. Het aantal retourbewerkingen tussen uw code en de service Azure-tabel toe te voegen entiteiten in batch worden beperkt. De volgende stappen laten zien hoe meerdere entiteiten aan een tabel met één bewerking insert toevoegen:

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u hebt de stappen in [de ontwikkelomgeving instellen](#set-up-the-development-environment).

1. Open het `TablesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **AddEntities** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult AddEntities()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **AddEntities** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een **CloudTableClient** object vertegenwoordigt een tabelserviceclient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Ophalen van een **CloudTable** -object met een verwijzing naar de tabel waarnaar u wilt toevoegen, de nieuwe entiteiten. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Exemplaar maken van een klantobjecten op basis van de **CustomerEntity** modelklasse weergegeven in de sectie [een entiteit toevoegen aan een tabel](#add-an-entity-to-a-table).

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. Ophalen van een **TableBatchOperation** object.

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. Entiteiten toevoegen aan het object batch insert-bewerking.

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. De batch-insert-bewerking uitvoeren door het aanroepen van de **CloudTable.ExecuteBatch** methode.   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. De **CloudTable.ExecuteBatch** methode retourneert een lijst met **TableResult** objecten waar elke **TableResult** object kan worden onderzocht om te bepalen het slagen of mislukken van elke afzonderlijke bewerking. In dit voorbeeld geeft de lijst op een weergave en laat de weergave de resultaten van elke bewerking weergeven. 
 
    ```csharp
    return View(results);
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **tabellen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **AddEntities** voor de weergavenaam en selecteer **toevoegen**.

1. Open `AddEntities.cshtml`, en zodat het lijkt erop dat het volgende te wijzigen.

    ```csharp
    @model IEnumerable<Microsoft.WindowsAzure.Storage.Table.TableResult>
    @{
        ViewBag.Title = "AddEntities";
    }
    
    <h2>Add-entities results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        @foreach (var result in Model)
        {
        <tr>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@result.HttpStatusCode</td>
        </tr>
        }
    </table>
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. Voer de toepassing en selecteer **entiteiten toevoegen** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Entiteiten toevoegen](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    U kunt controleren of de entiteit is toegevoegd door de stappen in de sectie [één entiteit ophalen](#get-a-single-entity). U kunt ook de [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) om weer te geven van alle entiteiten voor de tabellen.

## <a name="get-a-single-entity"></a>Één entiteit ophalen

Deze sectie ziet u hoe één entiteit ophalen uit een tabel met behulp van de entiteit rijsleutel en partitiesleutel. 

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u hebt de stappen in [de ontwikkelomgeving instellen](#set-up-the-development-environment), en maakt gebruik van gegevens van [een batch entiteiten toevoegen aan een tabel](#add-a-batch-of-entities-to-a-table). 

1. Open het `TablesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **GetSingle** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult GetSingle()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **GetSingle** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een **CloudTableClient** object vertegenwoordigt een tabelserviceclient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Ophalen van een **CloudTable** -object met een verwijzing naar de tabel waaruit u de entiteit ophalen. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Maakt een ophalen bewerking object waarvoor een entiteitsobject afgeleid van **TableEntity**. De eerste parameter is de *partitionKey*, en de tweede parameter is de *rowKey*. Met behulp van de **CustomerEntity** klasse en gegevens die zijn gepresenteerd in de sectie [een batch entiteiten toevoegen aan een tabel](#add-a-batch-of-entities-to-a-table), het volgende codefragment query's voor de tabel voor een **CustomerEntity** entiteit met een *partitionKey* waarde van 'Smith' en een *rowKey* waarde van 'Ben':

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. De ophalen-bewerking niet uitvoeren.   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. Het resultaat doorgeven aan de weergave om weer te geven.

    ```csharp
    return View(result);
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **tabellen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **GetSingle** voor de weergavenaam en selecteer **toevoegen**.

1. Open `GetSingle.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "GetSingle";
    }
    
    <h2>Get Single results</h2>
    
    <table border="1">
        <tr>
            <th>HTTP result</th>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        <tr>
            <td>@Model.HttpStatusCode</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).Email)</td>
        </tr>
    </table>
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. Voer de toepassing en selecteer **één ophalen** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Één ophalen](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen

Zoals vermeld in de sectie [een entiteit toevoegen aan een tabel](#add-an-entity-to-a-table), de combinatie van een partitie en een rijsleutel unieke identificatie van een entiteit in een tabel. Entiteiten met dezelfde partitiesleutel kunnen sneller worden opgevraagd dan entiteiten met verschillende partitiesleutels. Deze sectie ziet u hoe u een tabel met alle entiteiten uit een opgegeven partitie.  

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u hebt de stappen in [de ontwikkelomgeving instellen](#set-up-the-development-environment), en maakt gebruik van gegevens van [een batch entiteiten toevoegen aan een tabel](#add-a-batch-of-entities-to-a-table). 

1. Open het `TablesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **GetPartition** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult GetPartition()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **GetPartition** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een **CloudTableClient** object vertegenwoordigt een tabelserviceclient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Ophalen van een **CloudTable** -object met een verwijzing naar de tabel waaruit u de entiteiten ophalen. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Exemplaar maken van een **TableQuery** -object dat de query in de **waar** component. Met behulp van de **CustomerEntity** klasse en gegevens die zijn gepresenteerd in de sectie [een batch entiteiten toevoegen aan een tabel](#add-a-batch-of-entities-to-a-table), het volgende codefragment query's in de tabel voor een alle entiteiten waarbij de **PartitionKey** (achternaam van de klant) heeft de waarde 'Smith':

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. Aanroepen in een For-lus, de **CloudTable.ExecuteQuerySegmented** methode voor het doorgeven van de query-object dat u in de vorige stap hebt gestart.  De **CloudTable.ExecuteQuerySegmented** methode retourneert een **TableContinuationToken** die - object als **null** -geeft aan dat er geen entiteiten meer om op te halen. Gebruik een andere lus worden herhaald voor de geretourneerde entiteiten binnen de lus. In het volgende codevoorbeeld wordt elke geretourneerde entiteit toegevoegd aan een lijst. Zodra de lus is afgelopen, de lijst wordt doorgegeven aan een weergave om weer te geven: 

    ```csharp
    List<CustomerEntity> customers = new List<CustomerEntity>();
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = table.ExecuteQuerySegmented(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity customer in resultSegment.Results)
        {
            customers.Add(customer);
        }
    } while (token != null);

    return View(customers);
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **tabellen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **GetPartition** voor de weergavenaam en selecteer **toevoegen**.

1. Open `GetPartition.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @model IEnumerable<StorageAspnet.Models.CustomerEntity>
    @{
        ViewBag.Title = "GetPartition";
    }
    
    <h2>Get Partition results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        @foreach (var customer in Model)
        {
        <tr>
            <td>@(customer.RowKey)</td>
            <td>@(customer.PartitionKey)</td>
            <td>@(customer.Email)</td>
        </tr>
        }
    </table>
    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. Voer de toepassing en selecteer **partitie ophalen** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Partitie ophalen](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Deze sectie ziet u hoe een entiteit uit een tabel verwijderen.

> [!NOTE]
> 
> Deze sectie wordt ervan uitgegaan dat u hebt de stappen in [de ontwikkelomgeving instellen](#set-up-the-development-environment), en maakt gebruik van gegevens van [een batch entiteiten toevoegen aan een tabel](#add-a-batch-of-entities-to-a-table). 

1. Open het `TablesController.cs`-bestand.

1. Toevoegen van een methode aangeroepen **DeleteEntity** die retourneert een **ActionResult**.

    ```csharp
    public ActionResult DeleteEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. Binnen de **DeleteEntity** methode, krijgen een **CloudStorageAccount** -object met gegevens over uw storage-account. Gebruik de volgende code de verbindingsreeks voor opslag en storage-account-gegevens ophalen uit de configuratie van Azure service: (wijziging  *&lt;storage-account-name >* op de naam van de Azure storage-account u toegang hebt.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een **CloudTableClient** object vertegenwoordigt een tabelserviceclient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Ophalen van een **CloudTable** -object met een verwijzing naar de tabel waaruit u de entiteit wilt verwijderen. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Maakt een verwijderen bewerking object waarvoor een entiteitsobject afgeleid van **TableEntity**. In dit geval gebruiken we de **CustomerEntity** klasse en gegevens die zijn gepresenteerd in de sectie [een batch entiteiten toevoegen aan een tabel](#add-a-batch-of-entities-to-a-table). De entiteit **ETag** moet worden ingesteld op een geldige waarde.  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. De delete-bewerking uitvoeren.   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. Het resultaat doorgeven aan de weergave om weer te geven.

    ```csharp
    return View(result);
    ```

1. In de **Solution Explorer**, vouw de **weergaven** map met de rechtermuisknop op **tabellen**, en selecteer in het contextmenu **toevoegen -> weergave**.

1. Op de **weergave toevoegen** dialoogvenster Voer **DeleteEntity** voor de weergavenaam en selecteer **toevoegen**.

1. Open `DeleteEntity.cshtml`, en dit zodanig aanpassen dat het lijkt erop dat het volgende codefragment:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "DeleteEntity";
    }
    
    <h2>Delete Entity results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        <tr>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@Model.HttpStatusCode</td>
        </tr>
    </table>

    ```

1. In de **Solution Explorer**, vouw de **weergaven -> gedeelde** map en open `_Layout.cshtml`.

1. Nadat de laatste **Html.ActionLink**, voeg de volgende **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. Voer de toepassing en selecteer **entiteit verwijderen** om vergelijkbaar met de volgende schermopname resultaten te bekijken:
  
    ![Één ophalen](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a>Volgende stappen
Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.

  * [Aan de slag met Azure blob storage en Visual Studio verbonden Services (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Aan de slag met Azure queue storage- en Visual Studio verbonden Services (ASP.NET)](../storage/vs-storage-aspnet-getting-started-queues.md)
