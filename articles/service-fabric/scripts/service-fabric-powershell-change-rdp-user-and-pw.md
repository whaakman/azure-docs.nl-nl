---
title: Azure PowerShell-voorbeeldscript - RDP-gebruikersnaam en -wachtwoord bijwerken| Microsoft Docs
description: Azure PowerShell-voorbeeldscript - RDP-gebruikersnaam en -wachtwoord bijwerken voor alle Service Fabric-clusterknooppunten van een bepaald knooppunttype.
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
ms.date: 03/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: ff9cfabc4ac7b759a916ddaaeb3f4c95ceecd452
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Gebruikersnaam en wachtwoord van de beheerder van de VM's in een cluster bijwerken

Elk [knooppunttype](../service-fabric-cluster-nodetypes.md) in een Service Fabric-cluster is een virtuele-machineschaalset. Met dit voorbeeldscript worden de gebruikersnaam en het wachtwoord van de beheerder voor de virtuele clustermachines in een specifiek knooppunttype bijgewerkt.  Voeg de extensie VMAccessAgent aan de schaalset toe, want het beheerderswachtwoord is geen bewerkbare schaalseteigenschap.  De wijziging van de gebruikersnaam en het wachtwoord is van toepassing op alle knooppunten in de schaalset. Pas de parameters zo nodig aan.

Installeer zo nodig Azure PowerShell met behulp van de instructies in de [Azure PowerShell-handleiding](/powershell/azure/overview). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Hiermee worden de eigenschappen van een clusterknooppunttype (een virtuele-machineschaalset) opgehaald.   |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)| Hiermee wordt een extensie toegevoegd aan de virtuele-machineschaalset.|
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)|Hiermee wordt de status van een virtuele-machineschaalset ingesteld op de status van een lokaal VMSS-object.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
