---
title: Externe tabelbinding voor Azure Functions (experimentele)
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
ms.openlocfilehash: 8a4358fa67e45d0b7a2df1519d649099b5ef5850
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Externe tabelbinding voor Azure Functions (experimentele)

In dit artikel wordt uitgelegd hoe werken met tabellaire gegevens op de SaaS-providers, zoals Sharepoint en Dynamics in Azure Functions. Azure Functions ondersteunt invoer en uitvoer van de bindingen voor externe tabellen.

> [!IMPORTANT]
> De binding van de externe tabel experimentele en mogelijk nooit toegang tot de status in het algemeen beschikbaar (GA). Het is opgenomen in Azure alleen 1.x werkt en er zijn geen plannen toe te voegen aan de Azure Functions 2.x. Voor scenario's waarvoor toegang tot gegevens in de SaaS-providers, kunt u overwegen [logische apps die gebruikmaken van functies](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API-verbindingen

Tabelverbindingen gebruikmaken van externe verbindingen te verifiëren met de SaaS-providers van derden die API. 

Bij het toewijzen van een binding kunt u een nieuwe API-verbinding maken of een bestaande verbinding API binnen dezelfde resourcegroep gebruiken.

### <a name="available-api-connections-tables"></a>Beschikbare API-verbindingen (tabellen)

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
> Verbindingen met externe tabel kunnen ook worden gebruikt [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="creating-an-api-connection-step-by-step"></a>Maken van een API-verbinding: stap voor stap

1. Selecteer in de Azure portal-pagina voor de functie-app, het plusteken (**+**) voor het maken van een functie.

1. In de **Scenario** de optie **Experimental**.

1. Selecteer **externe tabel**.

1. Selecteer een taal.

2. Onder **externe tabel verbinding**, een bestaande verbinding selecteren of selecteer **nieuwe**.

1. Configureer de instellingen voor een nieuwe verbinding en selecteer **autoriseren**.

1. Selecteer **maken** voor het maken van de functie.

1. Selecteer **integreren > externe tabel**.

1. Configureer de verbinding voor het gebruik van de doeltabel. Deze instellingen variëren tussen aanbieders van SaaS. Voorbeelden zijn opgenomen in de volgende sectie.

## <a name="example"></a>Voorbeeld

In dit voorbeeld verbindt met een tabel met de naam 'Neem contact op met' met Id, LastName en FirstName kolommen. De code geeft een lijst van de contactpersoon entiteiten in de tabel en de voor- en achternamen registreert.

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

Een tabel te maken in SQL Server voor gebruik met dit voorbeeld, is hier een script. `dataSetName`is 'standaard'.

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

### <a name="google-sheets-data-source"></a>Google bladen-gegevensbron

Een tabel te maken voor gebruik met dit voorbeeld in Google Docs, maakt u een werkblad met een werkblad met de naam `Contact`. De connector niet de weergavenaam van het werkblad gebruiken. De interne naam (in de vet weergegeven) moet worden gebruikt als dataSetName, bijvoorbeeld: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  toevoegen de kolomnamen `Id`, `LastName`, `FirstName` naar de eerste rij vervolgens vullen gegevens op volgende rijen.

### <a name="salesforce"></a>SalesForce

In dit voorbeeld gebruiken met Salesforce, `dataSetName` is 'standaard'.

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|----------------------|
|**type** | moet worden ingesteld op `apiHubTable`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode. | 
|**verbinding**| Identificeert de app-instelling die de API-verbindingsreeks opslaat. De app-instelling wordt automatisch gemaakt wanneer u een API-verbinding in de gebruikersinterface van de integreren toevoegen.|
|**dataSetName**|De naam van de gegevensset met de tabel om te lezen.|
|**tableName**|De naam van de tabel|
|**id van de entiteit**|Moet leeg zijn voor Tabelverbindingen.

Een in tabelvorm connector biedt gegevenssets en elke gegevensset tabellen bevat. De naam van de standaardgegevensset is 'standaard'. De titels van een gegevensset en een tabel in verschillende SaaS-providers worden hieronder weergegeven:

|Connector|Gegevensset|Tabel|
|:-----|:---|:---| 
|**SharePoint**|Site|SharePoint-lijst
|**SQL**|Database|Tabel 
|**Google blad**|Spreadsheet|Werkblad 
|**Excel**|Excel-bestand|Spreadsheet 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
