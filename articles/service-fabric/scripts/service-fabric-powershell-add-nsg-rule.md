---
title: Azure PowerShell-voorbeeld script-een regel voor een netwerk beveiligings groep toevoegen | Microsoft Docs
description: 'Azure PowerShell script-voor beeld: Hiermee wordt een netwerk beveiligings groep toegevoegd om binnenkomend verkeer op een specifieke poort toe te staan.'
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 11/28/2017
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 1418b85bfa2579ed2dee5211a79a75adf14f1547
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592290"
---
# <a name="add-an-inbound-network-security-group-rule"></a>Een regel voor een inkomende netwerk beveiligings groep toevoegen

Met dit voorbeeld script maakt u een regel voor de netwerk beveiligings groep om binnenkomend verkeer op poort 8081 toe te staan.  Met het script wordt `Microsoft.Network/networkSecurityGroups` de bron opgehaald waarin het cluster zich bevindt, wordt er een nieuwe netwerk beveiligings configuratie regel gemaakt en wordt de netwerk beveiligings groep bijgewerkt. Pas de parameters zo nodig aan.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installeer zo nodig Azure PowerShell met behulp van de instructies in de [Azure PowerShell-handleiding](/powershell/azure/overview). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-inbound-nsg-rule/add-inbound-nsg-rule.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Haalt de `Microsoft.Network/networkSecurityGroups`-resource op. |
|[Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup)| Hiermee wordt de netwerk beveiligings groep op naam opgehaald.|
|[Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Hiermee voegt u een configuratie van een netwerk beveiligings regel toe aan een netwerk beveiligings groep. |
|[Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup)| Hiermee stelt u de doel status voor een netwerk beveiligings groep in.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).
