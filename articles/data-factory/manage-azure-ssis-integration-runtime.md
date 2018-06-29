---
title: Configureren van de runtime Azure SSIS-integratie | Microsoft Docs
description: Informatie over het configureren van een Azure-SSIS-integratie runtime in Azure Data Factory nadat u al hebt ingericht.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 248967f736fcd10cf398917d3cd1e2760537df7c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051981"
---
# <a name="reconfigure-the-azure-ssis-integration-runtime"></a>De runtime Azure SSIS-integratie configureren
Dit artikel wordt beschreven hoe u een bestaande Azure SSIS-integratie runtime opnieuw configureren. Zie voor informatie over het maken van een Azure-SSIS-integratie runtime (IR) in Azure Data Factory [maken van een Azure-SSIS-integratie runtime](create-azure-ssis-integration-runtime.md).  

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory 
Kunt u Data Factory-gebruikersinterface om te stoppen, bewerken/opnieuw te configureren of een Azure-SSIS-IR verwijderen 

1. In de **Data Factory UI**, overschakelen naar de **bewerken** tabblad. Start de Data Factory-gebruikersinterface, klikt u op **auteur & Monitor** op de startpagina van uw gegevensfactory.
2. Klik in het linkerdeelvenster op **verbindingen**.
3. In het rechterdeelvenster overschakelen naar de **integratie Runtimes**. 
4. U kunt knoppen in de kolom acties **stoppen**, **bewerken**, of **verwijderen** de integratie-runtime. De **Code** knop in de **acties** kolom kunt u de JSON-definitie die is gekoppeld aan de integratie runtime bekijken.  
    
    ![Acties voor Azure SSIS-IR](./media/manage-azure-ssis-integration-runtime/actions-for-azure-ssis-ir.png)

### <a name="to-reconfigure-an-azure-ssis-ir"></a>Een Azure-SSIS-IR opnieuw configureren
1. Stoppen van de runtime integratie door te klikken op **stoppen** in de **acties** kolom. De om lijstweergave te vernieuwen, klikt u op **vernieuwen** op de werkbalk. Nadat de IR is gestopt, ziet u de eerste actie kunt u de IR starten 

    ![Acties voor Azure SSIS IR - nadat gestopt](./media/manage-azure-ssis-integration-runtime/actions-after-ssis-ir-stopped.png)
2. Reconfigure/bewerken IR door te klikken op **bewerken** knop in de **acties** kolom. In de **integratie Runtime-instellingen** venster wijzigingsinstellingen (bijvoorbeeld, de grootte van het knooppunt, het aantal knooppunten of maximale parallelle uitvoeringen per knooppunt). 
3. Als u wilt de IR starten, klikt u op **Start** knop in de **acties** kolom.     

## <a name="azure-powershell"></a>Azure PowerShell
Nadat u inrichten en een exemplaar van Azure SSIS-integratie runtime start, kunt u deze configureren door het uitvoeren van een reeks `Stop`  -  `Set`  -  `Start` PowerShell-cmdlets opeenvolgend. De volgende PowerShell-script wordt bijvoorbeeld het aantal knooppunten dat is toegewezen voor het exemplaar van Azure SSIS-integratie runtime tot vijf gewijzigd.

### <a name="reconfigure-an-azure-ssis-ir"></a>Een Azure-SSIS-IR configureren

1. De Azure-SSIS-integratie runtime eerst stoppen met het [Stop AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/stop-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet. Met deze opdracht alle knooppunten vrijgegeven en wordt gestopt facturering.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. De Azure-SSIS-IR vervolgens opnieuw te configureren met behulp van de [Set AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet. De volgende voorbeeldopdracht schaalt uit een Azure-SSIS-integratie runtime met vijf knooppunten.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Start vervolgens de runtime Azure SSIS-integratie met behulp van de [Start AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/start-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet. Met deze opdracht wijst alle knooppunten voor het uitvoeren van SSIS-pakketten.   

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

### <a name="delete-an-azure-ssis-ir"></a>Een Azure-SSIS-IR verwijderen
1. Eerst lijst van alle bestaande Azure SSIS belastingdienst onder uw gegevensfactory.

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. Stop vervolgens alle bestaande Azure SSIS belastingdienst in uw gegevensfactory.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. Vervolgens verwijdert u alle bestaande Azure SSIS belastingdienst in uw data factory één voor één.

    ```powershell
    Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Ten slotte verwijdert u uw gegevensfactory.

    ```powershell
    Remove-AzureRmDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Als u een nieuwe resourcegroep hebt gemaakt, verwijdert u de resourcegroep.

    ```powershell
    Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure-SSIS-runtime, in de volgende onderwerpen: 

- [Azure-SSIS-integratie Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat conceptuele informatie over de integratie runtimes in het algemeen met inbegrip van de Azure-SSIS-IR 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Procedure: Een Azure SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). Dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Managed Instance (preview) en het toevoegen van de IR aan een virtueel netwerk. 
- [Een Azure-SSIS-integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md). Dit artikel bevat algemene informatie over het toevoegen van een Azure-SSIS IR aan een virtueel netwerk van Azure. Er wordt ook beschreven hoe u Azure Portal gebruikt om een virtueel netwerk te configureren voor het deelnemen van Azure-SSIS IR aan het virtueel netwerk. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
 
