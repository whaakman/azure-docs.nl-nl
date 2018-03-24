---
title: bevat het bestand voor PowerShell voor Azure DNS
description: bevat het bestand voor PowerShell voor Azure DNS
services: dns
author: subsarma
ms.service: dns
ms.topic: include file for PowerShell for Azure DNS
ms.date: 03/21/2018
ms.author: subsarma
ms.custom: include file for PowerShell for Azure DNS
ms.openlocfilehash: e96b312f03069256396261bd6efe2f2586cdadea
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
## <a name="set-up-azure-powershell-for-azure-dns"></a>Azure PowerShell voor Azure DNS instellen

### <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* U moet de meest recente versie van de Azure Resource Manager PowerShell-cmdlets installeren. Zie [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) voor meer informatie.

Persoonlijke om Zones te gebruiken (openbare Preview), moet u bovendien zorg ervoor dat u hebt de onderstaande PowerShell-modules en versies. 
* AzureRM.Dns - [versie 4.1.0](https://www.powershellgallery.com/packages/AzureRM.Dns/4.1.0) of hoger
* AzureRM.Network - [versie 5.4.0](https://www.powershellgallery.com/packages/AzureRM.Network/5.4.0) of hoger

U kunt de bovenstaande modules downloaden van de PowerShell-galerie via de koppelingen hierboven naast de Module-versies. U kunt deze vervolgens installeren met behulp van de onderstaande opdrachten. De modules die beide zijn vereist en zijn volledig achterwaarts compatibel. 

```powershell
Install-Module -Name AzureRM.Dns -Force
```

```powershell
Install-Module -Name AzureRM.Network -Force
```

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Open de PowerShell-console en maak verbinding met uw account. Zie voor meer informatie [met behulp van PowerShell met Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a>Het abonnement selecteren
 
Controleer de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

Kies welk Azure-abonnement u wilt gebruiken.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Aangezien alle DNS-resources globaal en niet regionaal zijn, is de keuze van de locatie voor de resourcegroep niet van invloed op Azure DNS.

U kunt deze stap overslaan als u een bestaande resourcegroep gebruikt.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Resourceprovider registreren

De Azure DNS-service wordt beheerd door de Microsoft.Network-resourceprovider. Uw Azure-abonnement moet worden geregistreerd voor het gebruik van deze resourceprovider voordat u Azure DNS kunt gebruiken. Dit is een eenmalige bewerking voor elk abonnement.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```