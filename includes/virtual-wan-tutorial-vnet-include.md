---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 04/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 40c8cb41ad3bcd46e9973a5f96134ff1bfd02fd2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150829"
---
Voor het snel maken van een VNet, kunt u 'Try It' in dit artikel om een PowerShell-console te openen. Pas de waarden aan en kopieer en plak de opdrachten in het consolevenster. Zie voor meer informatie over de nieuwe Az-module en compatibiliteit met AzureRM [Introductie van de nieuwe Az-module van Azure PowerShell](/powershell/azure/new-azureps-module-az). Zie voor instructies over Az-module installeren, [Azure PowerShell installeren](/powershell/azure/install-az-ps).

Controleer wel of de adresruimte voor het VNet dat u maakt niet overlapt met een van de adresbereiken van andere VNet's waarmee u verbinding wilt maken of met de adresruimten van uw on-premises netwerk.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Als u geen al een resourcegroep die u wilt gebruiken, kunt u een nieuwe maken. Aanpassen van de PowerShell-opdrachten in overeenstemming met de naam van de resource dat u wilt gebruiken, en voer vervolgens de volgende cmdlet:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Een VNet maken

De PowerShell-opdrachten voor het maken van een VNet dat compatibel is voor uw omgeving aanpassen.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```