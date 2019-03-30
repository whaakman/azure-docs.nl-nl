---
title: 'Azure PowerShell-Script voorbeeld: een regel voor netwerkbeveiligingsgroep toevoegen | Microsoft Docs'
description: Voorbeeld van Azure PowerShell-Script - voegt een netwerkbeveiligingsgroep waarmee binnenkomend verkeer op een specifieke poort toe.
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
ms.date: 11/28/2017
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 526a568bbcd7249e4f8917e8cdd82a0de71bfb0a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664995"
---
# <a name="add-an-inbound-network-security-group-rule"></a>Een inkomende regel voor de netwerkbeveiligingsgroep toevoegen

Met dit voorbeeldscript maakt een regel voor netwerkbeveiligingsgroep om binnenkomend verkeer op poort 8081 toestaan.  Het script haalt de `Microsoft.Network/networkSecurityGroups` resource waarin het cluster zich bevindt, maakt u een nieuwe configuratie voor netwerkbeveiligingsregel en werkt de netwerkbeveiligingsgroep. Pas de parameters zo nodig aan.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installeer zo nodig Azure PowerShell met behulp van de instructies in de [Azure PowerShell-handleiding](/powershell/azure/overview). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-inbound-nsg-rule/add-inbound-nsg-rule.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Haalt de `Microsoft.Network/networkSecurityGroups`-resource op. |
|[Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup)| Hiermee wordt de netwerkbeveiligingsgroep opgehaald met de naam.|
|[Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Voegt een regelconfiguratie voor netwerk-beveiliging toe aan een netwerkbeveiligingsgroep. |
|[Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup)| Stelt de doelstatus voor een netwerkbeveiligingsgroep.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).
