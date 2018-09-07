---
title: 'PowerShell-script: Set toegestaan VM-grootten in Azure Lab-Services | Microsoft Docs'
description: Dit PowerShell-script wordt toegestane VM-grootten in Azure Lab-Services.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: 559e74675a5d113584dca21979c20462c9cdf19c
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44054703"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Gebruik PowerShell om in te stellen toegestaan VM-grootten in Azure Lab Services

Met dit PowerShell-voorbeeldscript stelt grootten van toegestane virtuele machines (VM) in Azure Lab-Services.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) | Zoekt naar resources op basis van de opgegeven parameters. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | -Resources ophaalt. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Hiermee wijzigt u een resource. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Een resource maken. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van Azure Lab Services PowerShell-scripts vindt u de [voorbeelden van Azure Lab Services PowerShell](../samples-powershell.md).
