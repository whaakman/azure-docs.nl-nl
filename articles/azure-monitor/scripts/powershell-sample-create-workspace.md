---
title: Voorbeeld van Azure PowerShell-script - Een Log Analytics-werkruimte maken | Microsoft Docs
description: Voorbeeld van Azure PowerShell-script - Een Log Analytics-werkruimte maken
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: log-analytics
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: magoedte
ms.openlocfilehash: 5ad04c52da4709a7097ff7915d7af7404d6725eb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53110350"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>Een Log Analytics-werkruimte maken met PowerShell

Met dit script kunt u snel aan de slag met een Azure Log Analytics-werkruimte. Deze is vereist als u gegevens wilt verzamelen, analyseren of er actie op wilt ondernemen.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een nieuwe Log Analytics-werkruimte voor uw abonnement te maken. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace) | Hiermee wordt informatie opgehaald over een bestaande werkruimte. |
| [New-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace) | Hiermee wordt een werkruimte gemaakt in de opgegeven resourcegroep en locatie. |


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

