---
title: Tabel-API van Azure Cosmos DB bewerkingen uitvoeren met PowerShell | Microsoft Docs
description: Het uitvoeren van Azure Cosmos DB tabel API-bewerkingen met PowerShell
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/15/2017
ms.author: robinsh
ms.openlocfilehash: d0f835db8a9fbe3833a9c7931ad1d8b4a778f016
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="perform-azure-cosmos-db-table-api-operations-with-azure-powershell"></a>Tabel-API van Azure Cosmos DB bewerkingen uitvoeren met Azure PowerShell 

>[!NOTE]
>Premium-functies biedt Azure Cosmos DB tabel API voor tabelopslag zoals klare globale distribueren, lage latentie leesbewerkingen en schrijfbewerkingen, automatische secundaire indexeren en de speciale doorvoer. In de meeste gevallen zijn de PowerShell-opdrachten in dit artikel werk voor zowel Azure Cosmos DB tabel API en Azure Table storage, maar in dit artikel is specifiek voor Azure Cosmos DB tabel-API. Als u van Azure Table storage gebruikmaakt, Zie [uitvoeren Azure Table storage bewerkingen met Azure PowerShell](table-storage-how-to-use-powershell.md).
>

Azure Cosmos DB tabel API kunt u op te slaan en grote sets gestructureerde, niet-relationele gegevens opvragen. De belangrijkste onderdelen van de service zijn tabellen, entiteiten en eigenschappen. Een tabel is een verzameling entiteiten. Een entiteit is een set eigenschappen. Elke entiteit kan maximaal 252 eigenschappen die alle naam / waarde-paren hebben. In dit artikel wordt ervan uitgegaan dat u al bekend met de concepten Azure Cosmos DB tabel-API bent. Zie voor gedetailleerde informatie [Inleiding tot Azure Cosmos DB tabel API](table-introduction.md) en [bouwen van een .NET-toepassing met behulp van de API van de tabel](create-table-dotnet.md).

Dit artikel bevat informatie over algemene tabel API-bewerkingen. Procedures voor: 

> [!div class="checklist"]
> * Een tabel maken
> * Ophalen van een tabel
> * Tabelentiteiten toevoegen
> * Een tabel
> * Tabelentiteiten verwijderen

## <a name="prerequisites"></a>Vereisten

De voorbeelden moet Azure PowerShell-moduleversie 4.4.0 of hoger. Voer in een PowerShell-venster `Get-Module -ListAvailable AzureRM` de versie te vinden. Als niets wordt weergegeven of dat u wilt bijwerken, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps). 

Nadat u Azure PowerShell is geïnstalleerd of bijgewerkt, moet u de module installeren **AzureRmStorageTable**, die de opdrachten voor het beheren van de entiteiten heeft. Installeert deze module voeren PowerShell als beheerder en gebruik de **Install-Module** opdracht.

```powershell
Install-Module AzureRmStorageTable
```

Installeer de Azure DB die Cosmos-assembly's lokaal om deze PowerShell-cmdlets gebruiken. Zie voor instructies over hoe u dat doet, [Azure RM opslag tabellen PowerShell-module voor Cosmos DB tabellen](https://blogs.technet.microsoft.com/paulomarques/2017/05/23/azure-rm-storage-tables-powershell-module-now-includes-support-for-cosmos-db-tables/).

Als u wilt de volgende oefeningen uitproberen, moet u een databaseaccount Azure Cosmos DB. Als u nog geen hebt, maak een nieuwe Azure Cosmos DB rekening houden met de [Azure-portal](https://portal.azure.com). Zie voor informatie over het maken van een nieuwe databaseaccount, [Azure Cosmos DB: een databaseaccount maken](create-table-dotnet.md#create-a-database-account).

De database en de bron-accountgroep niet ophalen uit de portal; u moet deze waarden in uw script om de toegang tot de tabellen te plaatsen. 

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-a-table-or-reference-a-table"></a>Een tabel maken of te verwijzen naar een tabel

Een tabel te maken of een verwijzing naar een tabel, gebruikt u **Get-AzureStorageTableTable**. Als u deze cmdlet met de naam van een tabel die niet bestaat, maakt een nieuwe tabel met die naam en retourneert een verwijzing naar de nieuwe tabel. Als de tabel bestaat, wordt een verwijzing naar de bestaande tabel.

```powershell
# set the name of the resource group in which your Cosmos DB Account resides.
$resourceGroup = "contosocosmosrg"
# if you want to make sure the resource group is valid, try this command
Get-AzureRmResourceGroup -Name $resourceGroup

# set the Cosmos DB account name 
$cosmosDBAccountName = "contosocosmostbl" 

# set the table name 
$tableName = "contosotable1"

# Get a reference to a table. This creates the table if it doesn't exist.
$storageTable = Get-AzureStorageTableTable `
  -resourceGroup $resourceGroup `
  -tableName $tableName `
  -cosmosDbAccount $cosmosDBAccountName 
```

U kunt de tabellen in de Azure DB die Cosmos-account met behulp van PowerShell kan niet weergeven, maar u kunt zich aanmelden bij de portal en Zie de tabel. Nu gaan we kijken hoe voor het beheren van de entiteiten in de tabel.

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Een tabel verwijderen 

Verwijderen van de tabellen uit Azure Cosmos DB biedt geen ondersteuning voor PowerShell. Als u wilt verwijderen van een tabel, gaat u naar de [Azure-portal](https://azure.portal.com), zoek de Azure DB die Cosmos-account dat u gebruikt, vinden en verwijderen van de tabel. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u een nieuwe resourcegroep hebt gemaakt en een nieuwe Azure DB die Cosmos-account in de groep hebt gemaakt, kunt u alle activa verwijdert u de hebt gemaakt in deze oefening door het verwijderen van de resourcegroep. Deze opdracht verwijdert u alle resources die zich in de groep, evenals de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel how-to, hebt u geleerd over algemene tabel API-bewerkingen met PowerShell, inclusief hoe: 

> [!div class="checklist"]
> * Een tabel maken
> * Ophalen van een tabel
> * Tabelentiteiten toevoegen
> * Een tabel
> * Tabelentiteiten verwijderen

Raadpleeg voor meer informatie de volgende artikelen:

* [Werken met Azure Storage-tabellen vanuit PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.