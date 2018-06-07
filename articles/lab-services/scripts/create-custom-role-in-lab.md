---
title: 'PowerShell-script: een aangepaste beveiligingsrol maken in een testomgeving in Azure DevTest Labs | Microsoft Docs'
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
ms.openlocfilehash: 295f742342fba7d77b556724c8005f3ac4816482
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636712"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>PowerShell gebruiken voor het maken van een aangepaste rol in een testomgeving in Azure DevTest Labs

Dit voorbeeld PowerShell-script maakt een aangepaste beveiligingsrol te gebruiken in een testomgeving in Azure DevTest Labs. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) | Hiermee haalt u de bewerkingen voor een Azure-resourceprovider die worden beveiligd met Azure RBAC. |
| [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) | Hier worden alle Azure RBAC-rollen die beschikbaar voor toewijzing zijn. |
| [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) | Hiermee maakt u een aangepaste beveiligingsrol. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Voorbeelden van extra Azure Lab Services PowerShell-script kunnen worden gevonden in de [Azure Lab Services PowerShell-voorbeelden](../samples-powershell.md).