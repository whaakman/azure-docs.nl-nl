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
ms.openlocfilehash: 40ae634897361219c39e60d2161d3576cc44a400
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077525"
---
Voor het snel maken van een VNet, kunt u 'Try It' in dit artikel een PowerShell-console openen in Azure Cloud Shell. Pas de waarden aan en kopieer en plak de opdrachten in het consolevenster. 

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
