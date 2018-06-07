---
title: 'PowerShell-script: een externe gebruiker toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs'
description: Deze PowerShell-script wordt een externe gebruiker toegevoegd aan een lab in Azure DevTest Labs.
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
ms.openlocfilehash: bd86e44c21ca3c0b9c061f9a5c24bd19c4b207f9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636440"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>PowerShell gebruiken voor een externe gebruiker toevoegen aan een lab in Azure DevTest Labs

Deze PowerShell-voorbeeldscript wordt een externe gebruiker toegevoegd aan een lab in Azure DevTest Labs. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) | Het gebruikersobject van Azure active directory pogingen. |
| [New-AzureRmRoleAssignment](/module/azurerm.resources/new-azurermroleassignment) | De opgegeven rol toegewezen aan de opgegeven principal, bij het opgegeven bereik. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Voorbeelden van extra Azure Lab Services PowerShell-script kunnen worden gevonden in de [Azure Lab Services PowerShell-voorbeelden](../samples-powershell.md).