---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 027b370d2497822dcbd6f3958556357957f9e8f5
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964659"
---
## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

Als u niet weet welke locatie u kunt gebruiken, kunt u een lijst met de beschikbare locaties weergeven. Geef de lijst met locaties weer met behulp van het volgende codevoorbeeld, en zoek de locatie die u wilt gebruiken. In dit voorbeeld wordt **eastus** gebruikt. Sla de locatie op in een variabele en gebruik de variabele zodat u de locatie op één plek kunt wijzigen.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Create a storage account

Maak een standaardopslagaccount voor algemeen gebruik met LRS-replicatie maken met behulp van [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Haal vervolgens de opslagaccountcontext op waarin het te gebruiken opslagaccount wordt gedefinieerd. Verwijs bij het werken met een opslagaccount naar de context in plaats van herhaaldelijk de referenties door te geven. Gebruik het volgende voorbeeld om een opslagaccount met de naam *mystorageaccount* met lokaal redundante opslag (LRS) en blob-codering (standaard ingeschakeld) te maken.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `
  -Kind Storage

$ctx = $storageAccount.Context
```
