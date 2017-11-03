---
title: 'Azure PowerShell script voorbeeld: een groep beheerde bron ophalen en vergroten of verkleinen van virtuele machines | Microsoft Docs'
description: Azure PowerShell-voorbeeldscript - ophalen van een groep beheerde bron en het formaat van virtuele machines
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: poweshell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: f549f26cb3f9fdb2d805d2efb2c0e1706abe3edb
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Ophalen van resources in een groep beheerde bron en het formaat van virtuele machines met PowerShell

Dit script resources opgehaald uit een groep van beheerde bronnen en Hiermee wordt ingesteld voor de virtuele machines in die resourcegroep.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de beheerde toepassing te implementeren. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmManagedApplication](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermmanagedapplication) | Beheerde toepassingen weergeven. Naam van resourcegroep richten van de resultaten bevatten. |
| [Get-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresource) | Lijst met bronnen. Geef een resourcegroep en het resourcetype voor het richten van het resultaat. |
| [Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) | Grootte van een virtuele machine bijwerken. |


## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](../overview.md).
* Zie voor meer informatie over PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
