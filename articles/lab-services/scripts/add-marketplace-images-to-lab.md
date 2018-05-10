---
title: 'PowerShell-script: een marketplace-installatiekopie toevoegen aan een aangepaste lab in Azure Lab Services | Microsoft Docs'
description: Deze PowerShell-script voegt een marketplace-installatiekopie aan een aangepaste lab in Azure Lab-Services.
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
ms.openlocfilehash: 64d168c132edce4ecd128b795fbfa5ab2607cb19
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-custom-lab"></a>PowerShell gebruiken voor een marketplace-installatiekopie toevoegen aan een aangepaste lab

Deze PowerShell-voorbeeldscript wordt een marketplace-installatiekopie aan een aangepaste lab in Azure-Services voor Lab toegevoegd.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een aangepaste lab**. Het script moet u een bestaande aangepaste lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a custom lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Zoeken naar AzureRmResource](/module/azurerm.resources/find-azurermresource) | Hiermee zoekt u naar resources op basis van de opgegeven parameters. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Hiermee haalt u resources. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Hiermee wijzigt u een resource. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Maak een resource. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Voorbeelden van extra Azure Lab Services PowerShell-script kunnen worden gevonden in de [Azure Lab Services PowerShell-voorbeelden](../samples-powershell.md).