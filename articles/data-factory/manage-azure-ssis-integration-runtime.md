---
title: De Azure-SSIS-integratieruntime configureren | Microsoft Docs
description: Leer hoe u een Azure-SSIS integratieruntime in Azure Data Factory opnieuw configureren nadat u al hebt ingericht.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 3c1178a20debc36fbdbbd374eaf9adb6005a93a7
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57454933"
---
# <a name="reconfigure-the-azure-ssis-integration-runtime"></a>De Azure-SSIS-integratieruntime configureren
In dit artikel wordt beschreven hoe u een bestaande Azure-SSIS integratieruntime opnieuw te configureren. Zie voor informatie over het maken van een Azure-SSIS integratieruntime (IR) in Azure Data Factory [maken van een Azure-SSIS integratieruntime](create-azure-ssis-integration-runtime.md).  

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory 
U kunt Data Factory-gebruikersinterface gebruiken om te stoppen, bewerken/opnieuw te configureren of verwijderen van een Azure-SSIS IR. 

1. In de **Data Factory-UI**, Ga naar de **bewerken** tabblad. Als u wilt starten Data Factory-gebruikersinterface, klikt u op **Author & Monitor** op de startpagina van uw data factory.
2. Klik in het linkerdeelvenster op **verbindingen**.
3. Ga in het rechter deelvenster naar de **Integratieruntimes**. 
4. U kunt de knoppen gebruiken in de kolom acties **stoppen**, **bewerken**, of **verwijderen** de integratieruntime. De **Code** knop in de **acties** kolom kunt u de JSON-definitie die is gekoppeld aan de integratieruntime weergeven.  
    
    ![Acties voor Azure SSIS-IR](./media/manage-azure-ssis-integration-runtime/actions-for-azure-ssis-ir.png)

### <a name="to-reconfigure-an-azure-ssis-ir"></a>Een Azure-SSIS IR configureren
1. Stoppen van de integratieruntime door te klikken op **stoppen** in de **acties** kolom. Als u wilt de lijstweergave met vernieuwen, klikt u op **vernieuwen** op de werkbalk. Nadat de IR is gestopt, ziet u dat de eerste actie kunt u de IR. 

    ![Acties voor Azure SSIS-IR - nadat gestopt](./media/manage-azure-ssis-integration-runtime/actions-after-ssis-ir-stopped.png)
2. Bewerken/reconfigure IR door te klikken op **bewerken** knop in de **acties** kolom. In de **installatie van Integratieruntime** venster, wijzigingsinstellingen (bijvoorbeeld, de grootte van het knooppunt, het aantal knooppunten en het maximum aantal parallelle uitvoeringen per knooppunt). 
3. Als u wilt de IR starten, klikt u op **Start** knop in de **acties** kolom.     

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Nadat u inrichten en een exemplaar van Azure-SSIS integratieruntime start, u kunt deze opnieuw configureren door het uitvoeren van een reeks `Stop`  -  `Set`  -  `Start` PowerShell-cmdlets opeenvolgend. De volgende PowerShell-script wijzigt u bijvoorbeeld het aantal toegewezen voor het exemplaar van de Azure-SSIS integration runtime op vijf knooppunten.

### <a name="reconfigure-an-azure-ssis-ir"></a>Een Azure-SSIS IR configureren

1. Stop eerst de Azure-SSIS integratieruntime met behulp van de [Stop-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/stop-Azdatafactoryv2integrationruntime) cmdlet. Met deze opdracht alle knooppunten worden vrijgegeven en stopt facturering.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. De Azure-SSIS-IR vervolgens opnieuw te configureren met behulp van de [Set AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) cmdlet. De volgende voorbeeldopdracht wordt uitgeschaald een Azure-SSIS integratieruntime met vijf knooppunten.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Start vervolgens de Azure-SSIS integratieruntime met behulp van de [Start AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) cmdlet. Met deze opdracht worden alle knooppunten toegewezen voor het uitvoeren van SSIS-pakketten.   

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

### <a name="delete-an-azure-ssis-ir"></a>Verwijderen van een Azure-SSIS IR
1. Geef eerst alle bestaande Azure-SSIS-IR. onder uw data factory.

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. Stop vervolgens alle bestaande Azure SSIS IR's in uw gegevensfactory.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. Vervolgens verwijdert u alle bestaande Azure SSIS-IR's in uw data factory één voor één.

    ```powershell
    Remove-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Ten slotte verwijdert u uw data factory.

    ```powershell
    Remove-AzDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Als u een nieuwe resourcegroep hebt gemaakt, verwijdert u de resourcegroep.

    ```powershell
    Remove-AzResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over Azure-SSIS runtime: 

- [Azure-SSIS Integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat algemene informatie over integratieruntimes in het algemeen met inbegrip van de Azure-SSIS-IR. 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Procedure: Een Azure-SSIS integratieruntime maken](create-azure-ssis-integration-runtime.md). In dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Database Managed Instance en toevoegen van de IR aan een virtueel netwerk. 
- [Een Azure-SSIS-integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md). Dit artikel bevat algemene informatie over het toevoegen van een Azure-SSIS IR aan een virtueel netwerk van Azure. Er wordt ook beschreven hoe u Azure Portal gebruikt om een virtueel netwerk te configureren voor het deelnemen van Azure-SSIS IR aan het virtueel netwerk. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
 
