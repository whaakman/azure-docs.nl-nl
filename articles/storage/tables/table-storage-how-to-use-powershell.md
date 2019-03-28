---
title: Azure Table storage bewerkingen uitvoeren met PowerShell | Microsoft Docs
description: Azure Table storage-bewerkingen met PowerShell uitvoeren.
services: cosmos-db
author: roygara
ms.service: cosmos-db
ms.topic: article
ms.date: 03/27/2019
ms.author: rogarana
ms.subservice: cosmosdb-table
ms.openlocfilehash: bb8f0fd98296d0cc4de1596480988b154a731d41
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540224"
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Azure Table storage bewerkingen uitvoeren met Azure PowerShell 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Azure Table storage is een NoSQL-gegevensarchief die u gebruiken kunt voor het opslaan en query uitvoeren op zeer grote sets gestructureerde, niet-relationele gegevens. De belangrijkste onderdelen van de service zijn tabellen, entiteiten en eigenschappen. Een tabel is een verzameling entiteiten. Een entiteit is een set eigenschappen. Elke entiteit kan maximaal 252 eigenschappen die alle naam / waarde-paren zijn hebben. In dit artikel wordt ervan uitgegaan dat u al bekend met de concepten van Azure Table Storage-Service bent. Zie voor gedetailleerde informatie [inzicht in het Table Service Data Model](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) en [aan de slag met Azure Table storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md).

In dit artikel bevat informatie over algemene Azure Table storage-bewerkingen. In deze zelfstudie leert u procedures om het volgende te doen: 

> [!div class="checklist"]
> * Een tabel maken
> * Een tabel ophalen
> * Tabelentiteiten toevoegen
> * Query uitvoeren op een tabel
> * Tabelentiteiten verwijderen
> * Een tabel verwijderen

In dit artikel laat zien hoe een nieuw Azure Storage-account maken in een nieuwe resourcegroep, zodat u het gemakkelijk kunt verwijderen wanneer u klaar bent. Als u in plaats van een bestaand opslagaccount gebruikt, kunt u dat in plaats daarvan doen.

De voorbeelden moet Az PowerShell-modules `Az.Storage (1.1.3 or greater)` en `Az.Resources (1.2.0 or greater)`. Voer in een PowerShell-venster `Get-Module -ListAvailable Az*` de versie te vinden. Als er niets wordt weergegeven, of u wilt upgraden, Zie [Azure PowerShell-module installeren](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Gebruik deze functie van Azure PowerShell vereist dat u hebt de `Az` -module geïnstalleerd. De huidige versie van AzureRmStorageTable is niet compatibel met de oudere AzureRM-module.
> Ga als volgt de [meest recente installatie-instructies voor het installeren van de module Az](/powershell/azure/install-az-ps) indien nodig.

Nadat Azure PowerShell is geïnstalleerd of bijgewerkt, moet u de module installeren **AzureRmStorageTable**, heeft de opdrachten voor het beheren van de entiteiten. Voor het installeren van deze module PowerShell ook uitvoeren als beheerder en gebruik de **Install-Module** opdracht.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Add-AzAccount` en volg de instructies op het scherm.

```powershell
Add-AzAccount
```

## <a name="retrieve-list-of-locations"></a>Lijst met locaties ophalen

Als u niet weet welke locatie u kunt gebruiken, kunt u een lijst met de beschikbare locaties weergeven. Selecteer de gewenste locatie in de lijst. Deze voorbeelden **eastus**. Deze waarde in de variabele Store **locatie** voor toekomstig gebruik.

```powershell
Get-AzLocation | select Location
$location = "eastus"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Naam van de resourcegroep Store in een variabele voor toekomstig gebruik. In dit voorbeeld wordt een resourcegroep met de naam *pshtablesrg* wordt gemaakt in de *eastus* regio.

```powershell
$resourceGroup = "pshtablesrg"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Een opslagaccount maken

Een standaardopslagaccount voor algemeen gebruik te maken met het gebruik van lokaal redundante opslag (LRS) [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). Zorg ervoor dat een unieke opslagaccountnaam opgeven. Krijg vervolgens de opslagaccountcontext op waarin het storage-account vertegenwoordigt. Als u werkt in een storage-account, kunt u verwijzen naar de context in plaats van herhaaldelijk de referenties op te geven.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Een nieuwe tabel maken

U kunt een tabel maken met de [New-AzStorageTable](/powershell/module/az.storage/New-AzStorageTable) cmdlet. In dit voorbeeld wordt de tabel wordt genoemd `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Een lijst met tabellen in het opslagaccount ophalen

Ophalen van een lijst met tabellen in het opslagaccount door middel [Get-AzStorageTable](/powershell/module/az.storage/Get-AzureStorageTable).

```powershell
Get-AzStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Een verwijzing naar een specifieke tabel ophalen

Als u wilt uitvoeren van bewerkingen in een tabel, moet u een verwijzing naar de specifieke tabel. Een verwijzing met behulp van [Get-AzStorageTable](/powershell/module/az.storage/Get-AzureStorageTable).

```powershell
$storageTable = Get-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="reference-cloudtable-property-of-a-specific-table"></a>Verwijzing CloudTable-eigenschap van een specifieke tabel

> [!IMPORTANT]
> Het gebruik van CloudTable is verplicht als u werkt met **AzureRmStorageTable** PowerShell-module. Roep de **Get-AzTableTable** opdracht voor het ophalen van de verwijzing naar dit object. Met deze opdracht maakt ook de tabel als deze niet al bestaat.

Uitvoeren van bewerkingen op een tabel met **AzureRmStorageTable**, moet u een verwijzing naar CloudTable-eigenschap van een specifieke tabel.

```powershell
$cloudTable = (Get-AzStorageTable –Name $tableName –Context $ctx).CloudTable
```

[!INCLUDE [storage-table-entities-powershell-include](../../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Een tabel verwijderen

Als u wilt verwijderen van een tabel, gebruikt u [Remove-AzStorageTable](/powershell/module/az.storage/Remove-AzStorageTable). Deze cmdlet wordt de tabel, inclusief alle gegevens verwijderd.

```powershell
Remove-AzStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resource-groep en storage-account aan het begin van deze instructies hebt gemaakt, kunt u alle activa die u in deze oefening hebt gemaakt door het verwijderen van de resourcegroep verwijderen. Met deze opdracht worden alle resources binnen de groep, evenals de resourcegroep zelf verwijderd.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies, hebt u geleerd over algemene Azure Table storage-bewerkingen met PowerShell, onder andere: 

> [!div class="checklist"]
> * Een tabel maken
> * Een tabel ophalen
> * Tabelentiteiten toevoegen
> * Query uitvoeren op een tabel
> * Tabelentiteiten verwijderen
> * Een tabel verwijderen

Zie de volgende artikelen voor meer informatie.

* [PowerShell Storage-cmdlets](/powershell/module/az.storage#storage)

* [Werken met Azure-tabellen vanuit PowerShell - Module-v2.0 AzureRmStorageTable PS](https://paulomarquesc.github.io/working-with-azure-storage-tables-from-powershell)

* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
