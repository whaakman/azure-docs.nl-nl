---
title: Azure PowerShell-Script steekproef - toevoegen van een groep voor de netwerkbeveiligingsregel | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - voegt een netwerkbeveiligingsgroep waarmee binnenkomend verkeer op een specifieke poort.
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
ms.openlocfilehash: fd3c648ee63c45bef305658832a4d31dfdb213be
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="add-an-inbound-network-security-group-rule"></a>Een binnenkomende groep van de netwerkbeveiligingsregel toevoegen

Dit voorbeeldscript wordt een groep van de netwerkbeveiligingsregel zodat binnenkomend verkeer op poort 8081 gemaakt.  Het script wordt de `Microsoft.Network/networkSecurityGroups` resource waarin het cluster zich bevindt, maakt u een nieuwe configuratie voor netwerkbeveiligingsregel en updates van de netwerkbeveiligingsgroep. Pas de parameters zo nodig aan.

Installeer zo nodig de Azure PowerShell met behulp van de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-inbound-nsg-rule/add-inbound-nsg-rule.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Haalt de `Microsoft.Network/networkSecurityGroups` resource. |
|[Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)| Hiermee wordt de netwerkbeveiligingsgroep opgehaald met de naam.|
|[Voeg AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig)| Een regel voor de netwerkbeveiligingsconfiguratie toevoegen aan een netwerkbeveiligingsgroep |
|[Set-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)| Hiermee stelt de status van het doel voor een netwerkbeveiligingsgroep.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).
