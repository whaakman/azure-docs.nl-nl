---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: e8c5bf8e3c4cd63b7eec278c480527e95455140d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166224"
---
<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Tabelentiteiten beheren

Nu u hebt een tabel, gaan we bekijken over het beheren van entiteiten of rijen in de tabel. 

Een entiteit kan maximaal 255 eigenschappen, met inbegrip van 3 eigenschappen hebben: **PartitionKey**, **RowKey**, en **Timestamp**. U bent verantwoordelijk voor het invoegen en bijwerken van de waarden van **PartitionKey** en **RowKey**. De server beheert de waarde van **tijdstempel**, die kan niet worden gewijzigd. Samen de **PartitionKey** en **RowKey** unieke identificatie van elke entiteit in een tabel.

* **PartitionKey**: Hiermee bepaalt u de partitie die de entiteit is opgeslagen in.
* **RowKey**: de entiteit in de partitie wordt aangeduid.

U kunt maximaal 252 aangepaste eigenschappen voor een entiteit kunt definiëren. 

### <a name="add-table-entities"></a>Tabelentiteiten toevoegen

Entiteiten toevoegen aan een tabel met **toevoegen StorageTableRow**. Deze voorbeelden gebruiken de partitiesleutels van waarden "Partitie1" en "partition2" en recordsleutels die gelijk is aan de afkortingen van Staten. De eigenschappen in elke entiteit zijn gebruikersnaam en het gebruikers-id. 

```powershell
$partitionKey1 = "partition1"
$partitionKey2 = "partition2"

# add four rows 
Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey1 `
    -rowKey ("CA") -property @{"username"="Chris";"userid"=1}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey2 `
    -rowKey ("NM") -property @{"username"="Jessie";"userid"=2}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey1 `
    -rowKey ("WA") -property @{"username"="Christine";"userid"=3}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey2 `
    -rowKey ("TX") -property @{"username"="Steven";"userid"=4}
```

### <a name="query-the-table-entities"></a>Query uitvoeren op de tabelentiteiten

Er zijn verschillende manieren om op te vragen van de entiteiten in een tabel.

#### <a name="retrieve-all-entities"></a>Alle entiteiten ophalen

Als u wilt alle entiteiten ophalen, gebruikt u **Get-AzureStorageTableRowAll**.

```powershell
Get-AzureStorageTableRowAll -table $storageTable | ft
```

Met deze opdracht levert resultaat is vergelijkbaar met de volgende tabel:

| gebruikers-id | gebruikersnaam | partitie | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Partitie1 | CA |
| 3 | Christine | Partitie1 | WA |
| 2 | Jessie | partition2 | NM |
| 4 | Steven | partition2 | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Entiteiten voor een specifieke partitie ophalen

Gebruiken om op te halen van alle entiteiten in een specifieke partitie, **Get-AzureStorageTableRowByPartitionKey**.

```powershell
Get-AzureStorageTableRowByPartitionKey -table $storageTable -partitionKey $partitionKey1 | ft
```
De resultaten in de volgende tabel als volgt uitzien:

| gebruikers-id | gebruikersnaam | partitie | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Partitie1 | CA |
| 3 | Christine | Partitie1 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Entiteiten voor een specifieke waarde in een specifieke kolom ophalen

Gebruiken om op te halen waarbij de waarde in een specifieke kolom gelijk is aan een bepaalde waarde entiteiten, **Get-AzureStorageTableRowByColumnName**.

```powershell
Get-AzureStorageTableRowByColumnName -table $storageTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Deze query wordt één record opgehaald.

|Veld|waarde|
|----|----|
| gebruikers-id | 1 |
| gebruikersnaam | Chris |
| PartitionKey | Partitie1 |
| RowKey      | CA |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Entiteiten met behulp van een aangepast filter ophalen 

Gebruiken om op te halen entiteiten met behulp van een aangepast filter, **Get-AzureStorageTableRowByCustomFilter**.

```powershell
Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter "(userid eq 1)"
```

Deze query wordt één record opgehaald.

|Veld|waarde|
|----|----|
| gebruikers-id | 1 |
| gebruikersnaam | Chris |
| PartitionKey | Partitie1 |
| RowKey      | CA |

### <a name="updating-entities"></a>Entiteiten bijwerken 

Er zijn drie stappen voor het bijwerken van entiteiten. Eerst ophalen van de entiteit te wijzigen. Ten tweede moet u de wijziging. Derde doorvoeren de wijzigen met behulp van **Update AzureStorageTableRow**.

De entiteit bijwerken met gebruikersnaam = 'Jessie' als u wilt dat de gebruikersnaam = 'Jessie2'. In dit voorbeeld ziet u ook een andere manier om te maken van een aangepast filter met behulp van .NET-typen. 

```powershell
# Create a filter and get the entity to be updated.
[string]$filter = `
    [Microsoft.WindowsAzure.Storage.Table.TableQuery]::GenerateFilterCondition("username",`
    [Microsoft.WindowsAzure.Storage.Table.QueryComparisons]::Equal,"Jessie")
$user = Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter $filter

# Change the entity.
$user.username = "Jessie2" 

# To commit the change, pipe the updated record into the update cmdlet.
$user | Update-AzureStorageTableRow -table $storageTable 

# To see the new record, query the table.
Get-AzureStorageTableRowByCustomFilter -table $storageTable `
    -customFilter "(username eq 'Jessie2')"
```

De resultaten weergegeven de record Jessie2.

|Veld|waarde|
|----|----|
| gebruikers-id | 2 |
| gebruikersnaam | Jessie2 |
| PartitionKey | partition2 |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>Tabelentiteiten verwijderen

U kunt één entiteit of alle entiteiten in de tabel verwijderen.

#### <a name="deleting-one-entity"></a>Het verwijderen van een entiteit

Als u wilt verwijderen van een enkele entiteit, geen verwijzing ophalen naar die entiteit en doorsluizen naar **Remove-AzureStorageTableRow**.

```powershell
# Set filter.
[string]$filter = `
  [Microsoft.WindowsAzure.Storage.Table.TableQuery]::GenerateFilterCondition("username",`
  [Microsoft.WindowsAzure.Storage.Table.QueryComparisons]::Equal,"Jessie2")

# Retrieve entity to be deleted, then pipe it into the remove cmdlet.
$userToDelete = Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter $filter
$userToDelete | Remove-AzureStorageTableRow -table $storageTable 

# Retrieve entities from table and see that Jessie2 has been deleted.
Get-AzureStorageTableRowAll -table $storageTable | ft
```

#### <a name="delete-all-entities-in-the-table"></a>Alle entiteiten in de tabel verwijderen 

Als u wilt verwijderen van alle entiteiten in de tabel, die u kunt ze ophalen en de resultaten doorsluizen naar de cmdlet remove. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzureStorageTableRowAll `
    -table $storageTable | Remove-AzureStorageTableRow -table $storageTable 

# List entities in the table (there won't be any).
Get-AzureStorageTableRowAll -table $storageTable | ft
```
