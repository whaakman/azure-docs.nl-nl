---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1fae3c3889242dfbf8f270d3762ea7434ceda6da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004138"
---
Als u nog geen een VNet hebt, kunt u er eenvoudig een maken met behulp van PowerShell. U kunt ook een virtueel netwerk maken via Azure Portal.

* Controleer wel of de adresruimte voor het VNet dat u maakt niet overlapt met een van de adresbereiken van andere VNet's waarmee u verbinding wilt maken of met de adresruimten van uw on-premises netwerk. 
* Als u al een VNet hebt, dient u te controleren u of dit VNet voldoet aan de vereiste criteria en geen virtuele netwerkgateway heeft.

U kunt eenvoudig een VNet maken door in dit artikel te klikken op 'Try it', waardoor een PowerShell-console wordt geopend. Pas de waarden aan en kopieer en plak de opdrachten in het consolevenster.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Pas de PowerShell-opdrachten aan en maak een resourcegroep.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Een VNet maken

Pas de PowerShell-opdrachten aan om een VNet te maken dat compatibel is met uw omgeving.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```