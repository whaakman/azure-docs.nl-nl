---
title: 'PowerShell-script: stapsgewijs gegevens laden met behulp van Azure Data Factory | Microsoft Docs'
description: "Deze PowerShell-script laat zien hoe Azure Data Factory gebruiken om gegevens stapsgewijs uit een Azure SQL Database kopiëren naar een Azure Blob Storage..."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: spelluru
ms.openlocfilehash: 36a8c8c4ed83a56dfd0f487ec4bcf030e3890ef6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---incrementally-load-data-by-using-azure-data-factory"></a>PowerShell-script - incrementeel gegevens laden met behulp van Azure Data Factory
Deze PowerShell-voorbeeldscript laadt alleen nieuwe of bijgewerkte records uit een gegevensopslag van bron naar een gegevensarchief sink na de eerste volledige kopie van gegevens van de bron naar de sink.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

Zie [zelfstudie: incrementele kopie](../tutorial-incremental-copy-powershell.md#prerequisites) voor de vereisten voor het uitvoeren van dit voorbeeld. 

## <a name="sample-script"></a>Voorbeeld van een script

> [!IMPORTANT]
> Dit script maakt een JSON-bestanden die definiëren van Data Factory-entiteiten (gekoppelde service, gegevensset en pijplijn) op de harde schijf in de map c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/incremental-copy-from-azure-sql-to-blob/incremental-copy-from-azure-sql-to-blob.ps1 "Incremental copy from Azure SQL Database to Azure Blob Storage")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Nadat u hebt het voorbeeldscript uitvoeren, kunt u de volgende opdracht om te verwijderen van de resourcegroep en alle resources die zijn gekoppeld:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Als u wilt de gegevensfactory verwijderen uit de resourcegroep, voer de volgende opdracht: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten: 

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Een data factory maken. |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Maakt een gekoppelde service in de gegevensfactory. Een gekoppelde service is een gegevensarchief of het compute gekoppeld aan een gegevensfactory. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Maakt een gegevensset in de gegevensfactory. Een gegevensset vertegenwoordigt invoer/uitvoer van een activiteit in een pijplijn. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Een pijplijn maakt in de gegevensfactory. Een pijplijn bevat een of meer activiteiten die een bepaalde bewerking uitvoert. In deze pijplijn kopieert een kopieeractiviteit gegevens van de ene locatie naar een andere locatie in een Azure Blob Storage. |
| [Aanroepen AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Maakt een uitvoering van de pijplijn. Met andere woorden, voert u de pijplijn. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Hiermee haalt details over het uitvoeren van de activiteit (uitvoeren) in de pijplijn. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/).

Aanvullende voorbeelden van Azure Data Factory PowerShell-script kunnen worden gevonden in de [Azure Data Factory PowerShell-scripts](../samples-powershell.md).