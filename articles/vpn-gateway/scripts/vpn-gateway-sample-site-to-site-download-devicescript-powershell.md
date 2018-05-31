---
title: Voorbeeld van Azure PowerShell-script - Sjabloon voor apparaatconfiguratie downloaden| Microsoft Docs
description: Lees hoe u een sjabloon voor apparaatconfiguratie downloadt.
services: vpn-gateway
documentationcenter: vpn-gateway
author: cherylmc
manager: jpconnock
editor: ''
tags: ''
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/30/2018
ms.author: anzaman
ms.openlocfilehash: 2a1b05de7e7c5a5a99dae990a34c9368c8b8b9a9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34211249"
---
# <a name="download-vpn-device-template-using-powershell"></a>VPN-apparaatsjabloon downloaden met PowerShell

Met dit script downloadt u de VPN-apparaatsjabloon voor een verbinding.


```azurepowershell-interactive
# Declare variables
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite1"

# List the available VPN device models and versions
Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration template for the connection
Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG `
-DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de gemaakte resources niet meer nodig hebt, gebruikt u de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) om de resourcegroep te verwijderen. Hiermee verwijdert u de resourcegroep en alle resources die deze bevat.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name TestRG1
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de implementatie te maken. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice](/powershell/module/azurerm.network/Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice) | Hiermee geeft u een lijst weer met alle beschikbare VPN-apparaatmodellen en -versies. |
| [Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript](/powershell/module/azurerm.network/Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript) | Hiermee downloadt u de configuratiesjabloon voor de verbinding. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).
