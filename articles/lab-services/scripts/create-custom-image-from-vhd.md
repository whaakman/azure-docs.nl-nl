---
title: 'PowerShell-script: een aangepaste installatiekopie maken van een VHD-bestand in Azure Lab Services | Microsoft Docs'
description: Deze PowerShell-script maakt een aangepaste installatiekopie van een VHD-bestand in Azure Lab-Services.
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
ms.openlocfilehash: 19b7c3c6018ec56b056761c336bc56c8b63b47a2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636399"
---
# <a name="use-powershell-to-create-a-custom-image-from-a-vhd-file-in-azure-lab-services"></a>PowerShell gebruiken voor het maken van een aangepaste installatiekopie van een VHD-bestand in Azure Lab Services

Dit voorbeeld PowerShell-script maakt een aangepaste installatiekopie van een VHD-bestand in Azure Lab Services

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten
* **Een lab**. Het script moet u een bestaande lab hebben. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-image-from-vhd/create-custom-image-from-vhd.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: 

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Hiermee haalt u resources. |
| [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Hiermee worden de toegangssleutels voor Azure opslagaccounts weergegeven. |
| [Nieuwe AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) | Hiermee voegt u een Azure-implementatie toe aan een resourcegroep. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Voorbeelden van extra Azure Lab Services PowerShell-script kunnen worden gevonden in de [Azure Lab Services PowerShell-voorbeelden](../samples-powershell.md).