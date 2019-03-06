---
title: 'PowerShell-script: Incrementeel gegevens kopiëren met behulp van Azure Data Factory | Microsoft Docs'
description: Dit PowerShell-script laat zien hoe u Azure Data Factory gegevens stapsgewijs uit een Azure SQL Database kopiëren naar een Azure Blob-opslag gebruiken...
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2017
ms.author: jingwang
ms.openlocfilehash: 5ae6d6ed06aa5734dc601e6e72ba55ec8ddf7bcf
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57440587"
---
# <a name="powershell-script---incrementally-load-data-by-using-azure-data-factory"></a>PowerShell-script - stapsgewijs gegevens laden met behulp van Azure Data Factory
Met dit PowerShell-voorbeeldscript worden alleen nieuwe of bijgewerkte records van een brongegevensarchief naar een sink-gegevensarchief geladen na de eerste volledige kopie van gegevens uit de bron voor de sink.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

Zie [zelfstudie: incrementele kopie](../tutorial-incremental-copy-powershell.md#prerequisites) voor de vereisten voor het uitvoeren van dit voorbeeld. 

## <a name="sample-script"></a>Voorbeeldscript

> [!IMPORTANT]
> Dit script maakt een JSON-bestanden die definiëren van Data Factory-entiteiten (gekoppelde service, gegevensset en pijplijn) op de harde schijf in de map c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/incremental-copy-from-azure-sql-to-blob/incremental-copy-from-azure-sql-to-blob.ps1 "Incremental copy from Azure SQL Database to Azure Blob Storage")]

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
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Een data factory maken. |
| [Set-AzDataFactoryV2LinkedService](/powershell/module/az.datafactory/Set-Azdatafactoryv2linkedservice) | Hiermee maakt u een gekoppelde service in de data factory. Een gekoppelde service verbindt een gegevensopslag of compute naar een data factory. |
| [Set-AzDataFactoryV2Dataset](/powershell/module/az.datafactory/Set-Azdatafactoryv2dataset) | Hiermee maakt u een gegevensset in de data factory. Een gegevensset vertegenwoordigt i/o voor een activiteit in een pijplijn. | 
| [Set-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Set-Azdatafactoryv2pipeline) | Maakt een pijplijn in de data factory. Een pijplijn bevat een of meer activiteiten die een bepaalde bewerking uitvoert. In deze pijpelijn gebruikt een kopieeractiviteit kopieert gegevens van de ene locatie naar een andere locatie in een Azure Blob-opslag. |
| [Invoke-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Invoke-Azdatafactoryv2pipeline) | Hiermee maakt u een uitvoering voor de pijplijn. Met andere woorden, voert de pijplijn. |
| [Get-AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-Azdatafactoryv2activityrun) | Hiermee haalt details over het uitvoeren van de activiteit (uitgevoerde activiteit) in de pijplijn. 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Meer Azure Data Factory PowerShell-voorbeeldscripts vindt u de [Azure Data Factory PowerShell-scripts](../samples-powershell.md).
