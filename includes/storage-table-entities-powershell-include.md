---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/27/2019
ms.author: tamram
ms.openlocfilehash: 9a60c624b181a1efd2f6deebd349daa82214a8a4
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541380"
---
<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Tabelentiteiten beheren

Nu u hebt een tabel, gaan we bekijken over het beheren van entiteiten of rijen in de tabel. 

Entiteiten kunnen maximaal 255 eigenschappen, met inbegrip van drie eigenschappen hebben: **PartitionKey**, **RowKey**, en **Timestamp**. U bent verantwoordelijk voor het invoegen en bijwerken van de waarden van **PartitionKey** en **RowKey**. De server beheert de waarde van **tijdstempel**, die kan niet worden gewijzigd. Samen de **PartitionKey** en **RowKey** unieke identificatie van elke entiteit in een tabel.

* **PartitionKey**: Bepaalt de partitie die de entiteit is opgeslagen in.
* **RowKey**: Unieke identificatie van de entiteit in de partitie.

U kunt maximaal 252 aangepaste eigenschappen voor een entiteit kunt definiëren. 

### <a name="add-table-entities"></a>Tabelentiteiten toevoegen

Entiteiten toevoegen aan een tabel met **toevoegen AzTableRow**. Deze voorbeelden partitiesleutels gebruiken met waarden `partition1` en `partition2`, en recordsleutels gelijk is aan de afkortingen van Staten. De eigenschappen in elke entiteit zijn `username` en `userid`. 

```powershell
$partitionKey1 = "partition1"
$partitionKey2 = "partition2"

# add four rows 
Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey1 `
    -rowKey ("CA") -property @{"username"="Chris";"userid"=1}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey2 `
    -rowKey ("NM") -property @{"username"="Jessie";"userid"=2}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey1 `
    -rowKey ("WA") -property @{"username"="Christine";"userid"=3}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey2 `
    -rowKey ("TX") -property @{"username"="Steven";"userid"=4}
```

### <a name="query-the-table-entities"></a>Query uitvoeren op de tabelentiteiten

U kunt de entiteiten in een tabel met behulp van een query de **Get-AzTableRow** opdracht.

> [!NOTE]
> De cmdlets **Get-AzureStorageTableRowAll**, **Get-AzureStorageTableRowByPartitionKey**, **Get-AzureStorageTableRowByColumnName**, en  **Get-AzureStorageTableRowByCustomFilter** zijn afgeschaft en wordt verwijderd in een toekomstige versie-update.

#### <a name="retrieve-all-entities"></a>Alle entiteiten ophalen

```powershell
Get-AzTableRow -table $cloudTable | ft
```

Met deze opdracht levert resultaat is vergelijkbaar met de volgende tabel:

| userid | gebruikersnaam | partitie | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Partitie1 | CA |
| 3 | Christine | Partitie1 | WA |
| 2 | Jessie | partition2 | NM |
| 4 | Steven | partition2 | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Entiteiten voor een specifieke partitie ophalen

```powershell
Get-AzTableRow -table $cloudTable -partitionKey $partitionKey1 | ft
```

De resultaten in de volgende tabel als volgt uitzien:

| userid | gebruikersnaam | partitie | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Partitie1 | CA |
| 3 | Christine | Partitie1 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Entiteiten voor een specifieke waarde in een specifieke kolom ophalen

```powershell
Get-AzTableRow -table $cloudTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Deze query wordt één record opgehaald.

|Veld|waarde|
|----|----|
| userid | 1 |
| gebruikersnaam | Chris |
| PartitionKey | Partitie1 |
| RowKey      | CA |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Entiteiten met behulp van een aangepast filter ophalen 

```powershell
Get-AzTableRow `
    -table $cloudTable `
    -customFilter "(userid eq 1)"
```

Deze query wordt één record opgehaald.

|Veld|waarde|
|----|----|
| userid | 1 |
| gebruikersnaam | Chris |
| PartitionKey | Partitie1 |
| RowKey      | CA |

### <a name="updating-entities"></a>Entiteiten bijwerken 

Er zijn drie stappen voor het bijwerken van entiteiten. Eerst ophalen van de entiteit te wijzigen. Ten tweede moet u de wijziging. Derde doorvoeren de wijzigen met behulp van **Update AzTableRow**.

De entiteit bijwerken met gebruikersnaam = 'Jessie' als u wilt dat de gebruikersnaam = 'Jessie2'. In dit voorbeeld ziet u ook een andere manier om te maken van een aangepast filter met behulp van .NET-typen.

```powershell
# Create a filter and get the entity to be updated.
[string]$filter = `
    [Microsoft.Azure.Cosmos.Table.TableQuery]::GenerateFilterCondition("username",`
    [Microsoft.Azure.Cosmos.Table.QueryComparisons]::Equal,"Jessie")
$user = Get-AzTableRow `
    -table $cloudTable `
    -customFilter $filter

# Change the entity.
$user.username = "Jessie2"

# To commit the change, pipe the updated record into the update cmdlet.
$user | Update-AzTableRow -table $cloudTable

# To see the new record, query the table.
Get-AzTableRow -table $cloudTable `
    -customFilter "(username eq 'Jessie2')"
```

De resultaten weergegeven de record Jessie2.

|Veld|waarde|
|----|----|
| userid | 2 |
| gebruikersnaam | Jessie2 |
| PartitionKey | partition2 |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>Tabelentiteiten verwijderen

U kunt één entiteit of alle entiteiten in de tabel verwijderen.

#### <a name="deleting-one-entity"></a>Het verwijderen van een entiteit

Als u wilt verwijderen van een enkele entiteit, geen verwijzing ophalen naar die entiteit en doorsluizen naar **Remove-AzTableRow**.

```powershell
# Set filter.
[string]$filter = `
  [Microsoft.Azure.Cosmos.Table.TableQuery]::GenerateFilterCondition("username",`
  [Microsoft.Azure.Cosmos.Table.QueryComparisons]::Equal,"Jessie2")

# Retrieve entity to be deleted, then pipe it into the remove cmdlet.
$userToDelete = Get-AzTableRow `
    -table $cloudTable `
    -customFilter $filter
$userToDelete | Remove-AzTableRow -table $cloudTable

# Retrieve entities from table and see that Jessie2 has been deleted.
Get-AzTableRow -table $cloudTable | ft
```

#### <a name="delete-all-entities-in-the-table"></a>Alle entiteiten in de tabel verwijderen

Als u wilt verwijderen van alle entiteiten in de tabel, die u kunt ze ophalen en de resultaten doorsluizen naar de cmdlet remove. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzTableRow `
    -table $cloudTable | Remove-AzTableRow -table $cloudTable 

# List entities in the table (there won't be any).
Get-AzTableRow -table $cloudTable | ft
```
