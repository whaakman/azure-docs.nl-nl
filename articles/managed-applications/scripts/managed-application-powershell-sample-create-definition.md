---
title: Azure PowerShell-script steekproef - definitie van een beheerde maken | Microsoft Docs
description: 'Azure PowerShell-script voorbeeld: de definitie van een beheerde toepassing maken'
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: ee721b7c68924f5e05fbdc4bc4b919837d4cc05b
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="create-a-managed-application-definition-with-powershell"></a>De definitie van een beheerde toepassing maken met PowerShell

Dit script wordt de definitie van een beheerde toepassing publiceert naar een Servicecatalogus.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/managed-applications/create-definition/create-definition.ps1 "Create definition")]


## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdracht om de definitie van de beheerde toepassing te maken. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmManagedApplicationDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermmanagedapplicationdefinition) | De definitie van een beheerde toepassing maken. Geef het pakket met de vereiste bestanden. |


## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](../overview.md).
* Zie voor meer informatie over PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
