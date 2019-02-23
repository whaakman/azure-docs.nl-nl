---
title: 'PowerShell-script: Een aangepaste rol maken in een lab in Azure DevTest Labs | Microsoft Docs'
description: Dit PowerShell-script wordt een externe gebruiker toegevoegd aan een lab in Azure DevTest Labs.
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
ms.openlocfilehash: 4d0c795dffb40ab7efec9005660439f9baef9f3f
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56732429"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>PowerShell gebruiken voor het maken van een aangepaste rol in een lab in Azure DevTest Labs

Met dit PowerShell-voorbeeldscript maakt u een aangepaste rol te gebruiken in een lab in Azure DevTest Labs. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) | Hiermee haalt u de bewerkingen voor de provider van een Azure-resource die worden beveiligd met Azure RBAC. |
| [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) | Geeft een lijst van alle Azure RBAC-rollen die beschikbaar voor toewijzing zijn. |
| [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) | Hiermee maakt u een aangepaste rol. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van Azure Lab Services PowerShell-scripts vindt u de [voorbeelden van Azure Lab Services PowerShell](../samples-powershell.md).
