---
title: "PowerShell-script: gegevens kopiëren van on-premises naar Azure met behulp van de Data Factory | Microsoft Docs"
description: Deze PowerShell-script kopieert gegevens van een on-premises SQL Server database naar een andere Azure-blobopslag.
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
ms.openlocfilehash: 7f062a58482ad72e3dd3844431205502b4c44786
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-from-on-premises-to-azure"></a>PowerShell gebruiken voor het maken van een data factory-pijplijn om gegevens te kopiëren van on-premises naar Azure

Dit voorbeeld PowerShell-script maakt een pijplijn in Azure Data Factory waarmee gegevens worden gekopieerd van een on-premises SQL Server database naar een Azure Blob-opslag.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Vereisten

- **SQL Server**. U gebruikt een lokale SQL Server-database als een **bron** gegevens opslaan in dit voorbeeld.
- **Azure Storage-account**. Gebruik van Azure blob storage als een **bestemming/sink** gegevens opslaan in dit voorbeeld. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) voor de stappen voor het maken van een account.
- **Integratie runtime zelf de hosting**. Downloaden van de MSI-bestand van de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717) en voer dit voor het installeren van een host zichzelf integratie runtime op uw computer.  

### <a name="create-sample-database-in-sql-server"></a>Voorbeelddatabase in SQL Server maken
1. Maak in de lokale SQL Server-database een tabel met de naam **emp** met behulp van de volgende SQL-script: 

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Voorbeeldgegevens in de tabel invoegen:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```

## <a name="sample-script"></a>Voorbeeld van een script

> [!IMPORTANT]
> Dit script maakt een JSON-bestanden die definiëren van Data Factory-entiteiten (gekoppelde service, gegevensset en pijplijn) op de harde schijf in de map c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-onprem-sql-server-to-azure-blob/copy-from-onprem-sql-server-to-azure-blob.ps1 "Copy from on-premises SQL Server -> Azure Blob Storage")]


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
| [Nieuwe AzureRmDataFactoryV2LinkedServiceEncryptCredential](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2linkedserviceencryptedcredential) | Versleutelt de referenties in een gekoppelde service en genereert de definitie van een nieuwe gekoppelde service met de versleutelde referentie. 
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Maakt een gekoppelde service in de gegevensfactory. Een gekoppelde service is een gegevensarchief of het compute gekoppeld aan een gegevensfactory. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Maakt een gegevensset in de gegevensfactory. Een gegevensset vertegenwoordigt invoer/uitvoer van een activiteit in een pijplijn. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Een pijplijn maakt in de gegevensfactory. Een pijplijn bevat een of meer activiteiten die een bepaalde bewerking uitvoert. In deze pijplijn kopieert een kopieeractiviteit gegevens van de ene locatie naar een andere locatie in een Azure Blob Storage. |
| [Aanroepen AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Maakt een uitvoering van de pijplijn. Met andere woorden, voert u de pijplijn. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Hiermee haalt details over het uitvoeren van de activiteit (uitvoeren) in de pijplijn. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/).

Aanvullende voorbeelden van Azure Data Factory PowerShell-script kunnen worden gevonden in de [voorbeelden van Azure Data Factory PowerShell](../samples-powershell.md).