---
title: Azure Functions externe tabel binding (Preview) | Microsoft Docs
description: Met behulp van de externe tabel bindingen in de Azure-functies
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 7b226aa4ec71535aa0222389aacd74764a80021a
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-functions-external-table-binding-preview"></a>Azure Functions externe tabel binding (Preview)
In dit artikel laat zien hoe manipuleren tabelgegevens op aanbieders van SaaS (zoals Sharepoint, Dynamics) binnen de functie met ingebouwde bindingen. Azure Functions ondersteunt de invoer en uitvoer bindingen voor externe tabellen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API-verbindingen

Tabelverbindingen gebruikmaken van externe verbindingen te verifiëren met 3e SaaS leveranciers die API. 

Bij het toewijzen van een binding kunt u een nieuwe API-verbinding te maken of moet u een bestaande verbinding API binnen dezelfde resourcegroep gebruiken

### <a name="supported-api-connections-tables"></a>Ondersteunde API-verbindingen (tabel) s

|Connector|Trigger|Invoer|Uitvoer|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 voor bewerkingen](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Google Spreadsheets](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 voor financiële gegevens](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Database](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Algemene gegevensservice](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x


> [!NOTE]
> Verbindingen met externe tabel kunnen ook worden gebruikt [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

### <a name="creating-an-api-connection-step-by-step"></a>Maken van een API-verbinding: stap voor stap

1. Maak een functie > aangepaste functie ![geen aangepaste functie maken](./media/functions-bindings-storage-table/create-custom-function.jpg)
1. Scenario `Experimental`  >  `ExternalTable-CSharp` sjabloon > Maak een nieuwe `External Table connection` 
 ![invoer sjabloon kiezen](./media/functions-bindings-storage-table/create-template-table.jpg)
1. Kies uw SaaS-provider > Kies/maken van een verbinding ![configureren SaaS-verbinding](./media/functions-bindings-storage-table/authorize-API-connection.jpg)
1. Selecteer uw API-verbinding > de functie maken ![tabelfunctie maken](./media/functions-bindings-storage-table/table-template-options.jpg)
1. Selecteer`Integrate` > `External Table`
    1. Configureer de verbinding voor het gebruik van de doeltabel. Deze instellingen wordt zeer tussen aanbieders van SaaS. Ze zijn overzicht hieronder in [gegevensbron instellingen](#datasourcesettings)
![tabel configureren](./media/functions-bindings-storage-table/configure-API-connection.jpg)

## <a name="usage"></a>Gebruik

In dit voorbeeld verbindt met een tabel met de naam 'Neem contact op met' met Id, LastName en FirstName kolommen. De code geeft een lijst van de contactpersoon entiteiten in de tabel en de voor- en achternamen registreert.

### <a name="bindings"></a>Bindingen
```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```
`entityId`moet leeg zijn voor Tabelverbindingen.

`ConnectionAppSettingsKey`identificeert de app-instelling die de API-verbindingsreeks opslaat. De app-instelling wordt automatisch gemaakt wanneer u een API-verbinding in de gebruikersinterface van de integreren toevoegen.

Een in tabelvorm connector biedt gegevenssets en elke gegevensset tabellen bevat. De naam van de standaardgegevensset is 'standaard'. De titels van een gegevensset en een tabel in verschillende SaaS-providers worden hieronder weergegeven:

|Connector|Gegevensset|Tabel|
|:-----|:---|:---| 
|**SharePoint**|Site|SharePoint-lijst
|**SQL**|Database|Tabel 
|**Google blad**|Spreadsheet|Werkblad 
|**Excel**|Excel-bestand|Spreadsheet 

<!--
See the language-specific sample that copies the input file to the output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a>Gebruik in C# #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retreive table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

<!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->
<a name="datasourcesettings"></a>
##Instellingen voor de gegevensbron

### <a name="sql-server"></a>SQL Server

Het script te maken en gegevens van de tabel Contact is lager dan. dataSetName is 'standaard'.

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets"></a>Google Sheets
In Google Docs, maakt u een werkblad met een werkblad met de naam `Contact`. De connector niet de weergavenaam van het werkblad gebruiken. De interne naam (in de vet weergegeven) moet worden gebruikt als dataSetName, bijvoorbeeld: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  toevoegen de kolomnamen `Id`, `LastName`, `FirstName` naar de eerste rij vervolgens vullen gegevens op volgende rijen.

### <a name="salesforce"></a>SalesForce
dataSetName is 'standaard'.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
