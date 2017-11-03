---
title: Azure Quickstart - Maak een opslagaccount met behulp van PowerShell | Microsoft Docs
description: Snel informatie over het maken van een nieuw opslagaccount met PowerShell
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/29/2017
ms.author: robinsh
ms.openlocfilehash: c9175cce0cb93e73009fb8d751e54f631603d482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-powershell"></a>Maak een opslagaccount met behulp van PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. Deze handleiding gegevens met behulp van PowerShell om een Azure Storage-account te maken. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze Quick Start is moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

Als u niet welke locatie die u wilt gebruiken weet, kunt u de beschikbare locaties weergeven. Nadat de lijst wordt weergegeven, vinden die u wilt gebruiken. Dit voorbeeld gebruiken **eastus**. Slaat u dit in een variabele en gebruik van de variabele, zodat u dit op één plek wijzigen kunt.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Een algemeen standard-opslagaccount maken

Er zijn verschillende typen opslagaccounts, afhankelijk van hoe u deze wilt gebruiken, en voor welke service (blobs, bestanden, tabellen of wachtrijen). Deze tabel ziet de mogelijkheden.

|**Type opslagaccount**|**Algemeen Standard**|**Algemeen Premium**|**Blob-opslag, 'hot' en 'cool' toegangslagen**|
|-----|-----|-----|-----|
|**Ondersteunde services**| BLOB, bestand, tabel, Queue-services | Blob service | Blob service|
|**Typen ondersteunde blobs**|Blok-blobs, pagina-blobs, toevoeg-blobs | Pagina-blobs | Blok-blobs en toevoeg-blobs|

Gebruik [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) voor het maken van een algemene standaard opslagaccount die u voor alle vier services gebruiken kunt. Naam van het opslagaccount *contosomvcstandard*, en configureer deze zodanig hebt lokaal redundante opslag en blob-versleuteling ingeschakeld.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) opdracht voor het verwijderen van de resourcegroep en alle bijbehorende resources. In dit geval wordt het opslagaccount verwijderd.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

U kunt een algemeen standaard opslagaccount hebt gemaakt in deze snel starten. Voor informatie over het uploaden en downloaden van BLOB storage-account, blijven de Blob storage-Quick Start.
> [!div class="nextstepaction"]
> [Objecten voor overdracht naar/van Azure Blob storage met PowerShell](../blobs/storage-quickstart-blobs-powershell.md)