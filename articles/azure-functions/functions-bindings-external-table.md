---
title: Binding van de externe tabel voor Azure Functions (experimenteel)
description: Met behulp van de externe tabel bindingen in Azure Functions
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 24728414747d8ad8a8d7ee0d8a21be2177a15ddd
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093809"
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Binding van de externe tabel voor Azure Functions (experimenteel)

In dit artikel wordt uitgelegd hoe u werkt met gegevens in tabelvorm op SaaS-providers, zoals Sharepoint en Dynamics, in Azure Functions. Azure Functions ondersteunt invoer- en uitvoerbindingen voor externe tabellen.

> [!IMPORTANT]
> De binding van de externe tabel is experimenteel en mogelijk nooit over het algemeen beschikbaar is (GA) status bereikt. Het is opgenomen in Azure Functions 1.x en er zijn geen plannen toe te voegen aan Azure Functions 2.x. Voor scenario's waarvoor toegang tot gegevens in SaaS-providers, kunt u overwegen [logische apps die gebruikmaken van functies](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API-verbindingen

Tabelbindingen gebruikmaken van externe API-verbindingen te verifiëren met SaaS-providers van derden. 

Bij het toewijzen van een binding kunt u een nieuwe API-verbinding maken of een bestaande API-verbinding in dezelfde resourcegroep gebruiken.

### <a name="available-api-connections-tables"></a>Beschikbare API-verbindingen (tabellen)

|Connector|Trigger|Invoer|Uitvoer|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 voor bewerkingen](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Google Spreadsheets](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 for Financials](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Database](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Common Data Service](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x

> [!NOTE]
> Externe verbindingen van de tabel kunnen ook worden gebruikt [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="creating-an-api-connection-step-by-step"></a>Het maken van een API-verbinding: stap voor stap

1. In de Azure portal-pagina voor uw functie-app, selecteer het plusteken (**+**) om een functie te maken.

1. In de **Scenario** Schakel **Experimental**.

1. Selecteer **externe tabel**.

1. Selecteer een taal.

2. Onder **verbinding voor externe tabel**, selecteert u een bestaande verbinding of **nieuwe**.

1. Configureer de instellingen voor een nieuwe verbinding en selecteer **autoriseren**.

1. Selecteer **maken** om de functie te maken.

1. Selecteer **integreren > externe tabel**.

1. Configureer de verbinding voor het gebruik van de doeltabel. Deze instellingen variëren tussen SaaS-providers. Voorbeelden zijn opgenomen in de volgende sectie.

## <a name="example"></a>Voorbeeld

In dit voorbeeld maakt verbinding met een tabel met de naam 'Neem contact op met' met Id, LastName en FirstName kolommen. De code geeft een lijst van de Neem contact op met entiteiten in de tabel en de voor- en achternamen geregistreerd.

Hier volgt de *function.json* bestand:

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

Dit is de C#-scriptcode:

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
        //retrieve table values
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

### <a name="sql-server-data-source"></a>SQL Server-gegevensbron

Voor het maken van een tabel in SQL Server voor gebruik met dit voorbeeld, is dit een script. `dataSetName` is 'standaard'.

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

### <a name="google-sheets-data-source"></a>Google spreadsheets-gegevensbron

Voor het maken van een tabel die u wilt gebruiken met dit voorbeeld in Google Docs, maakt u een werkblad met een werkblad met de naam `Contact`. De connector niet de weergavenaam van het werkblad gebruiken. De interne naam (in vet weergegeven) moet worden gebruikt als dataSetName, bijvoorbeeld: `docs.google.com/spreadsheets/d/` **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** toevoegen de kolomnamen `Id`, `LastName`, `FirstName` naar de eerste rij, klikt u vervolgens gegevens invullen op volgende rijen.

### <a name="salesforce"></a>SalesForce

In dit voorbeeld gebruiken met Salesforce, `dataSetName` is 'standaard'.

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|----------------------|
|**type** | Moet worden ingesteld op `apiHubTable`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | Moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode aan te geven. | 
|**verbinding**| Hiermee geeft u de app-instelling waarin de API-verbindingsreeks. De app-instelling wordt automatisch gemaakt wanneer u een API-verbinding in de gebruikersinterface van de integreren toevoegen.|
|**dataSetName**|De naam van de gegevensset met de tabel om te lezen.|
|**Tabelnaam**|De naam van de tabel|
|**id van de entiteit**|Moet leeg zijn voor tabelbindingen.

Een tabellair-connector biedt gegevenssets en elke gegevensset tabellen bevat. De naam van de standaardset van gegevens is 'standaard'. Hieronder vindt u de titels van een gegevensset en een tabel in de verschillende SaaS-providers:

|Connector|Gegevensset|Tabel|
|:-----|:---|:---| 
|**SharePoint**|Site|SharePoint-lijst
|**SQL**|Database|Tabel 
|**Google-spreadsheet**|Spreadsheet|Werkblad 
|**Excel**|Excel-bestand|Spreadsheet 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
