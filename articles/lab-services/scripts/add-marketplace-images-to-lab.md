---
title: 'PowerShell-script: een marketplace-installatiekopie toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs'
description: Deze PowerShell-script wordt een marketplace-installatiekopie aan een lab in Azure DevTest Labs toegevoegd.
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
ms.openlocfilehash: 9a65237b3eba6c9878d73148f0143f20dd60dd79
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636545"
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-lab-in-azure-devtest-labs"></a>PowerShell gebruiken voor een marketplace-installatiekopie toevoegen aan een lab in Azure DevTest Labs

Deze PowerShell-voorbeeldscript wordt een marketplace-installatiekopie aan een lab in Azure DevTest Labs toegevoegd. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Find-AzureRmResource](/module/azurerm.resources/find-azurermresource) | Hiermee zoekt u naar resources op basis van de opgegeven parameters. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Hiermee haalt u resources. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Hiermee wijzigt u een resource. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Maak een resource. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Voorbeelden van extra Azure Lab Services PowerShell-script kunnen worden gevonden in de [Azure Lab Services PowerShell-voorbeelden](../samples-powershell.md).