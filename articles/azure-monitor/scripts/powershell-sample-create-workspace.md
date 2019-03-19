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
ms.openlocfilehash: 4dc98fc3912f9d9c659ecfcaa3df253d30bfcc78
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57306966"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>Een Log Analytics-werkruimte maken met PowerShell

Met dit script kunt u snel aan de slag met een Azure Log Analytics-werkruimte. Deze is vereist als u gegevens wilt verzamelen, analyseren of er actie op wilt ondernemen.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een nieuwe Log Analytics-werkruimte voor uw abonnement te maken. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace) | Hiermee wordt informatie opgehaald over een bestaande werkruimte. |
| [New-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace) | Hiermee wordt een werkruimte gemaakt in de opgegeven resourcegroep en locatie. |


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

