---
title: Azure PowerShell-voorbeeldscript - Het RDP-poortbereik wijzigen | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - Wijzigt het RDP-poortbereik van een geïmplementeerd cluster.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 871fe502a40b4c22f3e57a2f357e940c7f659314
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663924"
---
# <a name="update-the-rdp-port-range-values"></a>De waarden voor het RDP-poortbereik bijwerken

Met dit voorbeeldscript worden de waarden voor het RDP-poortbereik op de clusterknooppunt-VM's gewijzigd nadat het cluster is geïmplementeerd.  Azure PowerShell wordt gebruikt, zodat de onderliggende VM's niet uit en aan hoeven te worden gezet.  Het script haalt de `Microsoft.Network/loadBalancers`-resource in de resourcegroep van het cluster op en werkt de waarden `inboundNatPools.frontendPortRangeStart` en `inboundNatPools.frontendPortRangeEnd` bij. Pas de parameters zo nodig aan.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installeer zo nodig Azure PowerShell met behulp van de instructies in de [Azure PowerShell-handleiding](/powershell/azure/overview). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Haalt de `Microsoft.Network/loadBalancers`-resource op. |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|Werkt de `Microsoft.Network/loadBalancers`-resource bij.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
