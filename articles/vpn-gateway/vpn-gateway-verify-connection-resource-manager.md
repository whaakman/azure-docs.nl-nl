---
title: Een VPN-Gateway-verbinding controleren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een virtueel netwerk VPN-gatewayverbinding controleren.
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
ms.openlocfilehash: 9dfea33b558b4b568da1fd9ed90402e6c5ce54bf
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573192"
---
# <a name="verify-a-vpn-gateway-connection"></a>Een VPN-gatewayverbinding verifiëren

In dit artikel laat u zien hoe om te controleren of een VPN-gatewayverbinding voor zowel het klassieke en Resource Manager-implementatiemodel.

## <a name="azure-portal"></a>Azure Portal

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

Als u wilt controleren of een VPN-gatewayverbinding voor het Resource Manager-implementatiemodel met behulp van PowerShell, installeer de nieuwste versie van de [Azure Resource Manager PowerShell-cmdlets](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure-CLI

Als u wilt controleren of een VPN-gatewayverbinding voor het Resource Manager-implementatiemodel met behulp van Azure CLI, installeer de nieuwste versie van de [CLI-opdrachten](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 of hoger).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure portal (klassiek)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (klassiek)

Als u wilt controleren of uw VPN-gatewayverbinding voor het klassieke implementatiemodel met behulp van PowerShell, installeer de nieuwste versies van de Azure PowerShell-cmdlets. Zorg ervoor dat u downloadt en installeert de [servicebeheer](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets) module. Add-AzureAccount gebruiken voor aanmelding bij het klassieke implementatiemodel.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Volgende stappen

* U kunt virtuele machines toevoegen aan uw virtuele netwerken. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.