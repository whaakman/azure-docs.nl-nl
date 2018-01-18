---
title: Azure Table storage bewerkingen uitvoeren met PowerShell | Microsoft Docs
description: Azure Table storage bewerkingen uitvoeren met PowerShell
services: cosmos-db
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: robinsh
ms.openlocfilehash: 15a4ed2370598cb98565c48b4563bee3a4445827
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Azure Table storage bewerkingen uitvoeren met Azure PowerShell 

>[!NOTE]
>Premium-functies biedt Azure Cosmos DB tabel API voor tabelopslag zoals klare globale distribueren, lage latentie leesbewerkingen en schrijfbewerkingen, automatische secundaire indexeren en de speciale doorvoer. In de meeste gevallen zijn de PowerShell-opdrachten in dit artikel werk voor zowel Azure Cosmos DB tabel API en Azure Table storage, maar in dit artikel is specifiek voor Azure Table storage. Als u Azure Cosmos DB tabel API gebruikt, raadpleegt u [Azure Cosmos DB tabel API-bewerkingen met Azure PowerShell](table-powershell.md).
>

Azure Table storage is een NoSQL-gegevensarchief die u kunt opslaan en grote sets gestructureerde, niet-relationele gegevens opvragen. De belangrijkste onderdelen van de service zijn tabellen, entiteiten en eigenschappen. Een tabel is een verzameling entiteiten. Een entiteit is een set eigenschappen. Elke entiteit kan maximaal 252 eigenschappen die alle naam / waarde-paren hebben. In dit artikel wordt ervan uitgegaan dat u al bekend met de concepten met Azure Table Storage-Service bent. Zie voor gedetailleerde informatie [inzicht in de tabel Service Data Model](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) en [aan de slag met Azure Table storage met .NET](table-storage-how-to-use-dotnet.md).

Dit artikel bevat informatie over algemene bewerkingen voor Azure Table-opslag. Procedures voor: 

> [!div class="checklist"]
> * Een tabel maken
> * Ophalen van een tabel
> * Tabelentiteiten toevoegen
> * Een tabel
> * Tabelentiteiten verwijderen
> * Een tabel verwijderen

Dit artikel leest u hoe een nieuw opslagaccount maken in een nieuwe resourcegroep, zodat u het eenvoudig verwijderen kunt wanneer u klaar bent. Als u liever een bestaand opslagaccount, kunt u dat in plaats daarvan doen.

De voorbeelden moet Azure PowerShell-moduleversie 4.4.0 of hoger. Voer in een PowerShell-venster `Get-Module -ListAvailable AzureRM` de versie te vinden. Als niets wordt weergegeven of dat u wilt bijwerken, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). 

Nadat u Azure PowerShell is geïnstalleerd of bijgewerkt, moet u de module installeren **AzureRmStorageTable**, die de opdrachten voor het beheren van de entiteiten heeft. Installeert deze module voeren PowerShell als beheerder en gebruik de **Install-Module** opdracht.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Lijst met locaties ophalen

Als u niet weet welke locatie u kunt gebruiken, kunt u een lijst met de beschikbare locaties weergeven. Selecteer de gewenste locatie in de lijst. Deze voorbeelden gebruikt **eastus**. Deze waarde wordt opgeslagen in de variabele **locatie** voor toekomstig gebruik.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup). 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Naam van de resourcegroep opgeslagen in een variabele voor toekomstig gebruik. In dit voorbeeld wordt een resourcegroep met de naam *pshtablesrg* wordt gemaakt in de *eastus* regio.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Een opslagaccount maken

Maken van een standaard algemeen opslagaccount met lokaal redundante opslag (LRS) met behulp van [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Haal de context van het opslagaccount waarin de storage-account moet worden gebruikt. Wanneer optreedt op een storage-account, maar u verwijzen naar de context in plaats van herhaaldelijk geven de referenties.

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

Een tabel te maken, gebruikt u de [nieuw AzureStorageTable](/powershell/module/azure.storage/New-AzureStorageTable) cmdlet. In dit voorbeeld wordt de tabel wordt genoemd `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Een lijst met tabellen in de storage-account ophalen

Ophalen van een lijst met tabellen in het storage-account met [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
$storageTable = Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Een verwijzing naar een specifieke tabel ophalen

Voor het uitvoeren van bewerkingen in een tabel, moet u een verwijzing naar de specifieke tabel. Ophalen van een referentie met [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Een tabel verwijderen

Als u wilt verwijderen van een tabel, gebruikt u [verwijderen AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Deze cmdlet wordt de tabel, inclusief alle gegevens verwijderd.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resource groep en storage-account aan het begin van deze instructies hebt gemaakt, kunt u alle van de activa die u in deze oefening hebt gemaakt door het verwijderen van de resourcegroep verwijderen. Deze opdracht verwijdert u alle resources die zich in de groep, evenals de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel how-to, hebt u geleerd over algemene bewerkingen van Azure Table storage met PowerShell, inclusief hoe: 

> [!div class="checklist"]
> * Een tabel maken
> * Ophalen van een tabel
> * Tabelentiteiten toevoegen
> * Een tabel
> * Tabelentiteiten verwijderen
> * Een tabel verwijderen

Zie de volgende artikelen voor meer informatie.

* [PowerShell Storage-cmdlets](/powershell/module/azurerm.storage#storage)

* [Werken met Azure Storage-tabellen vanuit PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.