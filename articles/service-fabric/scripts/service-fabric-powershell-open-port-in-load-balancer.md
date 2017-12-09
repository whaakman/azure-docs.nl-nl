---
title: Azure PowerShell-Script voorbeeld - toepassing Open poort in de load balancer | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Open een poort in de Azure load balancer voor een Service Fabric-toepassing.
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
ms.date: 12/08/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c643fc9e575a8e836a361893d78348bbd627a425
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>De poort van een toepassing in de Azure load balancer openen

Een Service Fabric-toepassing in Azure wordt uitgevoerd, bevindt zich achter de Azure load balancer. Dit voorbeeldscript wordt een poort geopend in een Azure load balancer, zodat een Service Fabric-toepassing met externe clients communiceren kan. Pas de parameters zo nodig aan. Als uw cluster in gebruik is een netwerkbeveiligingsgroep ook [toevoegen van een binnenkomende groep van de netwerkbeveiligingsregel](service-fabric-powershell-add-nsg-rule.md) voor binnenkomend verkeer toestaan.

Installeer zo nodig de Service Fabric PowerShell-module met de [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Hiermee haalt u een Azure-resource.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Hiermee haalt u de Azure load balancer. |
| [Voeg AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Een test-configuratie toevoegen aan een load balancer.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Hiermee haalt u een test-configuratie voor een load balancer. |
| [Voeg AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | De regelconfiguratie van een toevoegen aan een load balancer. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Hiermee stelt de status van het doel voor een load balancer. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
