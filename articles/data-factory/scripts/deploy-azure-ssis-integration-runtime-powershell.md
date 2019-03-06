---
title: PowerShell-script - implementatie van Azure-SSIS-integratieruntime | Microsoft Docs
description: Dit PowerShell-script maakt een Azure-SSIS integratieruntime die SSIS-pakketten in de cloud kunt uitvoeren.
services: data-factory
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/12/2017
ms.author: douglasl
ms.openlocfilehash: 437b8df66bca2923a342584c4deec5b2947365c5
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57443746"
---
# <a name="powershell-script---deploy-azure-ssis-integration-runtime"></a>PowerShell-script - implementatie van Azure-SSIS-integratieruntime

Met dit PowerShell-voorbeeldscript maakt u een Azure-SSIS integratieruntime die uw SSIS-pakketten kan worden uitgevoerd in Azure.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/data-factory/deploy-azure-ssis-integration-runtime/deploy-azure-ssis-integration-runtime.ps1 "Deploy Azure-SSIS Integration Runtime")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Nadat u het voorbeeld van een script uitvoert, kunt u de volgende opdracht uit om te verwijderen van de resourcegroep en alle bijbehorende resources:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
Als u wilt de data factory uit de resourcegroep verwijdert, moet u de volgende opdracht uitvoeren: 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt:

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Een data factory maken. |
| [Set-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) | Hiermee maakt u een Azure-SSIS integratieruntime die SSIS-pakketten kan worden uitgevoerd in de cloud |
| [Start-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) | Hiermee start u de Azure-SSIS integratieruntime. |
| [Get-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/get-Azdatafactoryv2integrationruntime) | Hiermee haalt u informatie over de Azure-SSIS integratieruntime. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Meer Azure Data Factory PowerShell-voorbeeldscripts vindt u de [voorbeelden van Azure Data Factory PowerShell](../samples-powershell.md).
