---
title: 'PowerShell-script: een marketplace-installatiekopie toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs'
description: Dit PowerShell-script voegt een marketplace-installatiekopie toe aan een lab in Azure DevTest Labs.
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
ms.openlocfilehash: a3a007bf19a28e6f361837856f83a191a761ef9b
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43247411"
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-lab-in-azure-devtest-labs"></a>PowerShell gebruiken voor een marketplace-installatiekopie toevoegen aan een lab in Azure DevTest Labs

Met dit PowerShell-voorbeeldscript wordt een marketplace-installatiekopie toegevoegd aan een lab in Azure DevTest Labs. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a lab")]

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
