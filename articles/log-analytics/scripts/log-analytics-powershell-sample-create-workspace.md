---
title: Azure PowerShell-Script steekproef - maken van een werkruimte voor logboekanalyse | Microsoft Docs
description: 'Azure PowerShell-Script voorbeeld: een Log Analytics-werkruimte maken'
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
tags: 
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: magoedte
ms.openlocfilehash: 30d036ae56acc3a798d2776f292243f65cbea43d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>Maken van een werkruimte voor logboekanalyse met PowerShell

Dit script haalt u actief en werkend snel met een Azure-logboekanalyse-werkruimte vereist is als u verzamelen wilt, analyseren en nemen actie starten op de gegevens.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het maken van een nieuwe werkruimte voor logboekanalyse in uw abonnement. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace) | Hiermee haalt u informatie over een bestaande werkruimte. |
| [Nieuwe AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace) | Maakt een werkruimte in de opgegeven resourcegroep en locatie. |


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

