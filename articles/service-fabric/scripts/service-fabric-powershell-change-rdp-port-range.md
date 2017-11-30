---
title: Azure PowerShell-Script voorbeeld - Wijzig het RDP-poortbereik | Microsoft Docs
description: "Azure PowerShell-voorbeeldscript - verandert het RDP-poortbereik van een geïmplementeerde cluster."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9089d071c69830323a94140a5ebb4e9e865499c6
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-rdp-port-range-values"></a>Bijwerken van de waarden voor het RDP-poort

Dit voorbeeldscript verandert de waarden voor het RDP-poort op het clusterknooppunt VM's na de implementatie van het cluster.  Azure PowerShell gebruikt, zodat de onderliggende virtuele machines niet cyclus.  Het script wordt de `Microsoft.Network/loadBalancers` resource in de resourcegroep en de updates van het cluster de `inboundNatPools.frontendPortRangeStart` en `inboundNatPools.frontendPortRangeEnd` waarden. Pas de parameters zo nodig aan.

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](/powershell/azure/overview). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Haalt de `Microsoft.Network/loadBalancers` resource. |
|[Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource)|Updates de `Microsoft.Network/loadBalancers` resource.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
