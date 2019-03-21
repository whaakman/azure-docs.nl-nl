---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 660bbf50e1a8ae73bd7bbe1f7c42691ed62d276a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552972"
---
Als u nog geen een VNet hebt, kunt u er eenvoudig een maken met behulp van PowerShell. U kunt ook een virtueel netwerk maken via Azure Portal.

* Controleer wel of de adresruimte voor het VNet dat u maakt niet overlapt met een van de adresbereiken van andere VNet's waarmee u verbinding wilt maken of met de adresruimten van uw on-premises netwerk. 
* Als u al een VNet hebt, dient u te controleren u of dit VNet voldoet aan de vereiste criteria en geen virtuele netwerkgateway heeft.

U kunt eenvoudig een VNet maken door in dit artikel te klikken op 'Try it', waardoor een PowerShell-console wordt geopend. Pas de waarden aan en kopieer en plak de opdrachten in het consolevenster.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Pas de PowerShell-opdrachten aan en maak een resourcegroep.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Een VNet maken

Pas de PowerShell-opdrachten aan om een VNet te maken dat compatibel is met uw omgeving.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```