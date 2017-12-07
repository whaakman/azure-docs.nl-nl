---
title: Azure Quick Start - Een opslagaccount maken met PowerShell | Microsoft Docs
description: Leer snel hoe u een nieuw opslagaccount maakt met behulp van PowerShell
services: storage
documentationcenter: 
author: tamram
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
ms.author: tamram
ms.openlocfilehash: b4b917adfb3644cca71b6696df005fbf9e295240
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-using-powershell"></a>Een opslagaccount maken met PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. In deze Quick Start wordt beschreven hoe u met PowerShell een Azure Storage-account kunt maken. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze Quick Start is moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

Als u niet weet welke locatie u kunt gebruiken, kunt u een lijst met de beschikbare locaties weergeven. Selecteer de gewenste locatie in de lijst. In dit voorbeeld wordt **eastus** gebruikt. Sla deze locatie op in een variabele en gebruik de variabele zodat u de locatie op één plek kunt wijzigen.

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

## <a name="create-a-general-purpose-standard-storage-account"></a>Een standaardopslagaccount voor algemeen gebruik maken

Er zijn verschillende typen opslagaccounts, afhankelijk van hoe u deze wilt gebruiken en voor welke service ze zijn bedoeld (blobs, bestanden, tabellen of wachtrijen). Deze tabel laat de verschillende mogelijkheden zien.

|**Type opslagaccount**|**Algemeen Standard**|**Algemeen Premium**|**Blob-opslag, 'hot' en 'cool' toegangslagen**|
|-----|-----|-----|-----|
|**Ondersteunde services**| Blob-, bestands-, tabel- en wachtrijservices | Blob-service | Blob-service|
|**Typen ondersteunde blobs**|Blok-blobs, pagina-blobs, toevoeg-blobs | Pagina-blobs | Blok-blobs en toevoeg-blobs|

Gebruik [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) om een standaardopslagaccount voor algemeen gebruik te maken die u voor alle vier de services kunt gebruiken. Geef het opslagaccount de naam *contosomvcstandard* en configureer het account met lokaal redundante opslag en blob-versleuteling ingeschakeld.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt. In dit geval wordt het door u gemaakte opslagaccount verwijderd.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een standaardopslagaccount voor algemeen gebruik gemaakt. Voor informatie over het up- en downloaden van blobs voor uw opslagaccount gaat u verder met de Quick Start over Blob-opslag.
> [!div class="nextstepaction"]
> [Objecten overdragen naar/van Azure Blob-opslag met PowerShell](../blobs/storage-quickstart-blobs-powershell.md)