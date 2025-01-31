---
title: Azure-SSIS-Integratieruntime configureren voor failover van de SQL-Database | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Azure-SSIS-Integratieruntime configureren met Azure SQL Database geo-replicatie en failover voor de SSISDB-database
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/14/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f0612a688bb1e0fd79325b9a1f9b43731a210d10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399244"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>De Azure-SSIS Integration Runtime met Azure SQL Database geo-replicatie en failover configureren

In dit artikel wordt beschreven hoe u de Azure-SSIS-Integratieruntime configureren met Azure SQL Database geo-replicatie voor de SSISDB-database. Wanneer er een failover optreedt, kunt u ervoor zorgen dat de Azure-SSIS-IR met de secundaire database werken blijft.

Zie voor meer informatie over geo-replicatie en failover voor SQL-Database [overzicht: Actieve geo-replicatie en automatische failover-groepen](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Scenario 1: Azure-SSIS IR verwijst voor listener-eindpunt voor lezen / schrijven

### <a name="conditions"></a>Voorwaarden

In deze sectie is van toepassing wanneer de volgende voorwaarden voldaan wordt:

- De Azure-SSIS-IR verwijst naar de lezen / schrijven-listener-eindpunt van de failovergroep.

  EN

- De SQL Database-server is *niet* geconfigureerd met de regel voor virtuele netwerken service-eindpunt.

### <a name="solution"></a>Oplossing

Wanneer een failover optreedt, is het transparant voor de Azure-SSIS-IR. De Azure-SSIS-IR wordt automatisch verbinding met de nieuwe primaire van de failovergroep.

## <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Scenario 2: Azure-SSIS IR verwijst naar het eindpunt van de primaire server

### <a name="conditions"></a>Voorwaarden

In deze sectie is van toepassing wanneer een van de volgende voorwaarden voldaan wordt:

- De Azure-SSIS-IR verwijst naar het eindpunt van de primaire server van de failovergroep. Dit eindpunt wordt gewijzigd wanneer een failover optreedt.

  OF

- De Azure SQL Database-server is geconfigureerd met de regel voor virtuele netwerken service-eindpunt.

  OF

- De database-server is een SQL Database Managed Instance geconfigureerd met een virtueel netwerk.

### <a name="solution"></a>Oplossing

Wanneer failover optreedt, hebt u het volgende doen:

1. Stoppen van de Azure-SSIS-IR.

2. De configuratie van de IR om te verwijzen naar de nieuwe primaire eindpunt en met een virtueel netwerk in de nieuwe regio.

3. Opnieuw opstarten van de IR.

De volgende secties beschrijven deze stappen nader uiteengezet.

### <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u herstel na noodgevallen hebt ingeschakeld voor uw Azure SQL Database-server als de server een storing op hetzelfde moment heeft. Zie voor meer informatie, [overzicht van bedrijfscontinuïteit met Azure SQL Database](../sql-database/sql-database-business-continuity.md).

- Als u een virtueel netwerk in de huidige regio gebruikt, moet u een ander virtueel netwerk in de nieuwe regio gebruiken om uw Azure-SSIS integratieruntime verbinding te maken. Zie voor meer informatie, [een Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md).

- Als u een aangepaste installatie gebruikt, moet u mogelijk om voor te bereiden van een andere SAS-URI voor de blob-container waarin uw aangepaste setup-script en de bijbehorende bestanden, zodat deze blijven toegankelijk tijdens een storing. Zie voor meer informatie, [configureren van een aangepaste installatie op een Azure-SSIS integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="steps"></a>Stappen

Volg deze stappen voor uw Azure-SSIS IR stopt, de IR overschakelen naar een nieuwe regio en probeer het opnieuw.

1. Stop de IR in de oorspronkelijke regio.

2. Roep de volgende opdracht uit in PowerShell om bij te werken van de IR met de nieuwe instellingen.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Zie voor meer informatie over deze PowerShell-opdracht [maken van de Azure-SSIS integratieruntime in Azure Data Factory](create-azure-ssis-integration-runtime.md)

3. De IR opnieuw starten.

## <a name="scenario-3---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Scenario 3: een bestaande SSISDB (SSIS-catalogus) koppelen aan een nieuwe Azure-SSIS-IR

Wanneer er zich een noodgeval ADF of Azure-SSIS IR in de huidige regio voordoet, kunt u uw SSISDB blijft werken met een nieuwe Azure-SSIS-IR in een nieuwe regio.

### <a name="prerequisites"></a>Vereisten

- Als u een virtueel netwerk in de huidige regio gebruikt, moet u een ander virtueel netwerk in de nieuwe regio gebruiken om uw Azure-SSIS integratieruntime verbinding te maken. Zie voor meer informatie, [een Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md).

- Als u een aangepaste installatie gebruikt, moet u mogelijk om voor te bereiden van een andere SAS-URI voor de blob-container waarin uw aangepaste setup-script en de bijbehorende bestanden, zodat deze blijven toegankelijk tijdens een storing. Zie voor meer informatie, [configureren van een aangepaste installatie op een Azure-SSIS integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="steps"></a>Stappen

Volg deze stappen voor uw Azure-SSIS IR stopt, de IR overschakelen naar een nieuwe regio en probeer het opnieuw.

1. Opgeslagen procedure als u wilt maken die is gekoppeld aan SSISDB uitvoeren **\<new_data_factory_name\>** of  **\<new_integration_runtime_name\>** .
   
  ```SQL
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
   ```

2. Maak een nieuwe data factory met de naam **\<new_data_factory_name\>** in de nieuwe regio. Zie voor meer informatie, maken een data factory.

     ```powershell
     Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
     ```
    Zie voor meer informatie over deze PowerShell-opdracht [maken van een Azure data factory met behulp van PowerShell](quickstart-create-data-factory-powershell.md)

3. Maak een nieuwe Azure-SSIS-IR met de naam **\<new_integration_runtime_name\>** in de nieuwe regio met Azure PowerShell.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
                                           -DataFactoryName "new data factory name" `
                                           -Name "<new_integration_runtime_name>" `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId "new vnet" `
                                           -Subnet "new subnet" `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier
    ```

    Zie voor meer informatie over deze PowerShell-opdracht [maken van de Azure-SSIS integratieruntime in Azure Data Factory](create-azure-ssis-integration-runtime.md)

4. De IR opnieuw starten.

## <a name="next-steps"></a>Volgende stappen

Houd rekening met deze andere configuratieopties voor de Azure-SSIS-IR:

- [De Azure-SSIS Integration Runtime voor hoge prestaties configureren](configure-azure-ssis-integration-runtime-performance.md)

- [Instelling voor Azure-SSIS Integration Runtime aanpassen](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Enterprise-editie voor de Azure-SSIS-Integratieruntime inrichten](how-to-configure-azure-ssis-ir-enterprise-edition.md)
