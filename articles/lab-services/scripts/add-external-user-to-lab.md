---
title: 'PowerShell-script: een externe gebruiker toevoegen aan een aangepaste lab in Azure Lab Services | Microsoft Docs'
description: Deze PowerShell-script wordt een externe gebruiker toegevoegd aan een aangepaste lab in Azure Lab-Services.
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
ms.openlocfilehash: b089067a889f0ffd3b317fcc3f0784d176473b91
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-add-an-external-user-to-a-custom-lab"></a>PowerShell gebruiken voor een externe gebruiker toevoegen aan een aangepaste lab

Deze PowerShell-voorbeeldscript wordt een externe gebruiker toegevoegd aan een aangepaste lab in Azure Lab-Services. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een aangepaste lab**. Het script moet u een bestaande aangepaste lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a custom lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) | Het gebruikersobject van Azure active directory pogingen. |
| [New-AzureRmRoleAssignment](/module/azurerm.resources/new-azurermroleassignment) | De opgegeven rol toegewezen aan de opgegeven principal, bij het opgegeven bereik. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Voorbeelden van extra Azure Lab Services PowerShell-script kunnen worden gevonden in de [Azure Lab Services PowerShell-voorbeelden](../samples-powershell.md).