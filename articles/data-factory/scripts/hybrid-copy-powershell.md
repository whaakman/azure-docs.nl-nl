---
title: 'PowerShell-script: Gegevens kopiëren van on-premises naar Azure met behulp van Data Factory | Microsoft Docs'
description: Dit PowerShell-script worden gegevens gekopieerd van een on-premises SQL Server-database naar een andere Azure Blob Storage.
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
ms.openlocfilehash: 1218408bee067d6edb274fdcbf4fae62b8245a01
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57455051"
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-from-on-premises-to-azure"></a>PowerShell gebruiken voor het maken van een data factory-pijplijn om gegevens te kopiëren van on-premises naar Azure

Met dit PowerShell-voorbeeldscript maakt een pijplijn in Azure Data Factory waarmee gegevens uit een on-premises SQL Server-database gekopieerd naar een Azure Blob Storage.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Vereisten

- **SQL Server**. Gebruikt u een on-premises SQL Server-database als een **bron** gegevens opslaan in dit voorbeeld.
- **Een Azure Storage-account**. U gebruikt Azure blob-opslag als een **doel/sink** gegevens opslaan in dit voorbeeld. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../../storage/common/storage-quickstart-create-account.md) voor de stappen voor het maken van een account.
- **Zelf-hostende integratieruntime**. Download de MSI-bestand vanaf de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717) en uit te voeren naar een zelf-hostende integratieruntime installeren op uw computer.  

### <a name="create-sample-database-in-sql-server"></a>-Voorbeelddatabase maken in SQL Server
1. Maak in de on-premises SQL Server-database, een tabel met de naam **emp** met behulp van de volgende SQL-script: 

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

2. Enkele voorbeeldgegevens ingevoegd in de tabel:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```

## <a name="sample-script"></a>Voorbeeldscript

> [!IMPORTANT]
> Dit script maakt een JSON-bestanden die definiëren van Data Factory-entiteiten (gekoppelde service, gegevensset en pijplijn) op de harde schijf in de map c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-onprem-sql-server-to-azure-blob/copy-from-onprem-sql-server-to-azure-blob.ps1 "Copy from on-premises SQL Server -> Azure Blob Storage")]


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
| [New-AzDataFactoryV2LinkedServiceEncryptCredential](/powershell/module/az.datafactory/new-Azdatafactoryv2linkedserviceencryptedcredential) | Versleutelt de referenties in een gekoppelde service en genereert de definitie van een nieuwe gekoppelde service met de versleutelde referentie. 
| [Set-AzDataFactoryV2LinkedService](/powershell/module/az.datafactory/Set-Azdatafactoryv2linkedservice) | Hiermee maakt u een gekoppelde service in de data factory. Een gekoppelde service verbindt een gegevensopslag of compute naar een data factory. |
| [Set-AzDataFactoryV2Dataset](/powershell/module/az.datafactory/Set-Azdatafactoryv2dataset) | Hiermee maakt u een gegevensset in de data factory. Een gegevensset vertegenwoordigt i/o voor een activiteit in een pijplijn. | 
| [Set-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Set-Azdatafactoryv2pipeline) | Maakt een pijplijn in de data factory. Een pijplijn bevat een of meer activiteiten die een bepaalde bewerking uitvoert. In deze pijpelijn gebruikt een kopieeractiviteit kopieert gegevens van de ene locatie naar een andere locatie in een Azure Blob-opslag. |
| [Invoke-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Invoke-Azdatafactoryv2pipeline) | Hiermee maakt u een uitvoering voor de pijplijn. Met andere woorden, voert de pijplijn. |
| [Get-AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-Azdatafactoryv2activityrun) | Hiermee haalt details over het uitvoeren van de activiteit (uitgevoerde activiteit) in de pijplijn. 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/) voor meer informatie over Azure PowerShell.

Meer Azure Data Factory PowerShell-voorbeeldscripts vindt u de [voorbeelden van Azure Data Factory PowerShell](../samples-powershell.md).
