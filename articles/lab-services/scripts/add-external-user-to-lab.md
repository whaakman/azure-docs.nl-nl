---
title: 'PowerShell-script: Een externe gebruiker toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs'
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
ms.openlocfilehash: 042fa1e24ebadfb00a2d55cc97d742f198cb5662
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738348"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>PowerShell gebruiken voor het toevoegen van een externe gebruiker aan een lab in Azure DevTest Labs

Met dit PowerShell-voorbeeldscript wordt een externe gebruiker toegevoegd aan een lab in Azure DevTest Labs. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzADUser](/powershell/module/az.resources/get-azaduser) | Het gebruikersobject uit Azure active directory pogingen. |
| [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) | De opgegeven rol toegewezen aan de opgegeven principal, bij het opgegeven bereik. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van Azure Lab Services PowerShell-scripts vindt u de [voorbeelden van Azure Lab Services PowerShell](../samples-powershell.md).
