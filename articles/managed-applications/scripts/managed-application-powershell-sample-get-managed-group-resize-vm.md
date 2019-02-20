---
title: Voorbeeld van Azure PowerShell-script - Een beheerde resourcegroep opvragen en de grootte van virtuele machines wijzigen | Microsoft Docs
description: Voorbeeld van Azure PowerShell-script - Een beheerde resourcegroep opvragen en de grootte van virtuele machines wijzigen
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
ms.openlocfilehash: 9e8930c95495673c0082a82757ed6d8137900b6f
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110894"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Resources in een beheerde resourcegroep opvragen en de grootte van virtuele machines wijzigen met Azure PowerShell

Met dit script worden resources opgehaald uit een beheerde resourcegroep en wordt vervolgens de grootte van de virtuele machines in die resourcegroep aangepast.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de beheerde toepassing te implementeren. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | Hiermee vraagt u een lijst met de beheerde toepassingen op. Geef de naam op van de resourcegroep waarop u de resultaten wilt focussen. |
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Hiermee vraagt u een lijst met resources op. Geef de resourcegroep en het resourcetype op waarop u het resultaat wilt focussen. |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | Hiermee werkt u de grootte van een virtuele machine bij. |


## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van door Azure beheerde toepassingen](../overview.md) voor algemene informatie over beheerde toepassingen.
* Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie over PowerShell.
