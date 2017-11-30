---
title: Azure PowerShell-Script steekproef - bijwerken RDP-gebruikersnaam en wachtwoord | Microsoft Docs
description: Azure PowerShell-Script steekproef - bijwerken van de RDP-gebruikersnaam en het wachtwoord voor alle knooppunten van het Service Fabric-cluster van een specifiek knooppunttype.
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
ms.date: 11/17/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 3b97cee11c9a85cbd60a05bdbdcd010a0f0a106f
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Bijwerken van de gebruikersnaam van de beheerder en het wachtwoord van de virtuele machines in een cluster

Elk knooppunttype in een Service Fabric-cluster is een virtuele-machineschaalset. Dit voorbeeldscript updates de gebruikersnaam van de beheerder en het wachtwoord voor het cluster virtuele machines in een specifiek knooppunttype.  De extensie VMAccessAgent aan de schaalaanpassingsset toevoegen omdat de Administrator-wachtwoord is niet dat een bewerkbaar schaal eigenschap instellen.  De gebruikersnaam en wachtwoord wijzigingen van toepassing op alle knooppunten in de schaalset. Pas de parameters zo nodig aan.

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](/powershell/azure/overview). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten: elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Hiermee haalt u de eigenschappen van het type in een cluster-knooppunt (een virtuele-machineschaalset).   |
| [Voeg AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)| Toevoegen van een uitbreiding op de virtuele-machineschaalset.|
| [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)|De status van een virtuele-machineschaalset ingesteld op de status van een lokale VMSS-object bijgewerkt.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
