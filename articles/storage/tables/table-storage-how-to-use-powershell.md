---
title: Azure Table storage bewerkingen uitvoeren met PowerShell | Microsoft Docs
description: Azure Table storage-bewerkingen met PowerShell uitvoeren.
services: cosmos-db
author: roygara
ms.service: cosmos-db
ms.topic: article
ms.date: 03/14/2018
ms.author: rogarana
ms.component: cosmosdb-table
ms.openlocfilehash: 9aa3431e91196d7c33582d17494ddf0c32fba704
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53634470"
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

De voorbeelden moet Azure PowerShell-module `AzureRM` versie 4.4.0 of hoger. Voer in een PowerShell-venster `Get-Module -ListAvailable AzureRM` de versie te vinden. Als er niets wordt weergegeven, of u wilt upgraden, Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

Nadat Azure PowerShell is geïnstalleerd of bijgewerkt, moet u de module installeren **AzureRmStorageTable**, heeft de opdrachten voor het beheren van de entiteiten. Voor het installeren van deze module PowerShell ook uitvoeren als beheerder en gebruik de **Install-Module** opdracht.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Lijst met locaties ophalen

Als u niet weet welke locatie u kunt gebruiken, kunt u een lijst met de beschikbare locaties weergeven. Selecteer de gewenste locatie in de lijst. Deze voorbeelden **eastus**. Deze waarde in de variabele Store **locatie** voor toekomstig gebruik.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup). 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Naam van de resourcegroep Store in een variabele voor toekomstig gebruik. In dit voorbeeld wordt een resourcegroep met de naam *pshtablesrg* wordt gemaakt in de *eastus* regio.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Een opslagaccount maken

Een standaardopslagaccount voor algemeen gebruik te maken met het gebruik van lokaal redundante opslag (LRS) [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Krijg de opslagaccountcontext op waarin het opslagaccount dat moet worden gebruikt. Als u werkt met een opslagaccount, verwijst u naar de context in plaats van herhaaldelijk de referenties op te geven.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Een nieuwe tabel maken

U kunt een tabel maken met de [New-AzureStorageTable](/powershell/module/azure.storage/New-AzureStorageTable) cmdlet. In dit voorbeeld wordt de tabel wordt genoemd `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Een lijst met tabellen in het opslagaccount ophalen

Ophalen van een lijst met tabellen in het opslagaccount door middel [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Een verwijzing naar een specifieke tabel ophalen

Als u wilt uitvoeren van bewerkingen in een tabel, moet u een verwijzing naar de specifieke tabel. Een verwijzing met behulp van [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Een tabel verwijderen

Als u wilt verwijderen van een tabel, gebruikt u [Remove-AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Deze cmdlet wordt de tabel, inclusief alle gegevens verwijderd.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resource-groep en storage-account aan het begin van deze instructies hebt gemaakt, kunt u alle activa die u in deze oefening hebt gemaakt door het verwijderen van de resourcegroep verwijderen. Met deze opdracht worden alle resources binnen de groep, evenals de resourcegroep zelf verwijderd.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
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

* [PowerShell Storage-cmdlets](/powershell/module/azurerm.storage#storage)

* [Werken met Azure Storage-tabellen vanuit PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
