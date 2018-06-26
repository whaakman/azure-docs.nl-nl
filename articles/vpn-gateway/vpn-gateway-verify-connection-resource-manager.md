---
title: Een VPN-Gateway-verbinding controleren | Microsoft Docs
description: In dit artikel leest u hoe om te controleren of een virtueel netwerk-VPN-gatewayverbinding.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: 037c1c7dd73f668bd8ad95568743b223b1e11c79
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750194"
---
# <a name="verify-a-vpn-gateway-connection"></a>Een VPN-Gateway-verbinding controleren

In dit artikel leest u hoe om te controleren of een VPN-gatewayverbinding voor zowel het klassieke als Resource Manager-implementatiemodel.

## <a name="azure-portal"></a>Azure Portal

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

Om te controleren of een VPN-gatewayverbinding voor het Resource Manager-implementatiemodel met behulp van PowerShell, installeer de nieuwste versie van de [Azure Resource Manager PowerShell-cmdlets](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

Om te controleren of een VPN-gatewayverbinding voor het Resource Manager-implementatiemodel met Azure CLI, installeer de nieuwste versie van de [CLI-opdrachten](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 of hoger).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure-portal (klassiek)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (klassiek)

Om te controleren of uw VPN-gateway-verbinding voor het klassieke implementatiemodel met behulp van PowerShell, installeer de nieuwste versies van de Azure PowerShell-cmdlets. Zorg ervoor dat download en installeer de [Service Management](https://docs.microsoft.com/en-us/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets) module. Gebruik de Add-AzureAccount aan te melden het klassieke implementatiemodel.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Volgende stappen

* U kunt virtuele machines toevoegen aan uw virtuele netwerken. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.