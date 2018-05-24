---
title: Azure PowerShell-voorbeeldscript - Toepassingspoort openen in Azure-load balancer | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - Een poort in de Azure-load balancer openen voor een Service Fabric-toepassing.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 05/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 0549f5f2b5b0f8fdfc18b8c091c1065d6137b8c6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366168"
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Een toepassingspoort openen in de Azure-load balancer

Een Service Fabric-toepassing die in Azure wordt uitgevoerd, bevindt zich achter de Azure-load balancer. Met dit voorbeeldscript wordt een poort geopend in een Azure-load balancer, zodat een Service Fabric-toepassing kan communiceren met externe clients. Pas de parameters zo nodig aan. Als uw cluster deel uitmaakt van een netwerkbeveiligingsgroep, voegt u ook [een regel voor inkomende netwerkbeveiligingsgroepen toe](service-fabric-powershell-add-nsg-rule.md) om ingaand verkeer toe te staan.

Installeer indien nodig de Service Fabric PowerShell-module met de [Service Fabric-SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Haalt een Azure-resource op.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Haalt de Azure-load balancer op. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Voegt een testconfiguratie toe aan een load balancer.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Haalt een testconfiguratie op voor een load balancer. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Voegt een regelconfiguratie toe aan een load balancer. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Stelt de doelstatus voor een load balancer in. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer PowerShell-voorbeelden voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
