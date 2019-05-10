---
title: Prestaties voor de Azure-SSIS-Integratieruntime configureren | Microsoft Docs
description: Informatie over het configureren van de eigenschappen van de Azure-SSIS Integration Runtime voor hoge prestaties
services: data-factory
ms.date: 01/10/2018
ms.topic: conceptual
ms.service: data-factory
ms.workload: data-services
author: swinarko
ms.author: sawinark
ms.reviewer: ''
manager: craigg
ms.openlocfilehash: 42c69653a002446552da998320a43730dfdaadf5
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232537"
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>De Azure-SSIS Integration Runtime voor hoge prestaties configureren

In dit artikel wordt beschreven hoe het configureren van een Azure-SSIS Integration Runtime (IR) voor hoge prestaties. De Azure-SSIS IR kunt u het implementeren en uitvoeren van pakketten van SQL Server Integration Services (SSIS) in Azure. Zie voor meer informatie over Azure-SSIS IR [integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime) artikel. Zie voor meer informatie over het implementeren en uitvoeren van SSIS-pakketten op Azure [Lift- and -shift SQL Server integratieservices-workloads naar de cloud](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

> [!IMPORTANT]
> In dit artikel bevat prestatieresultaten en metingen uit interne tests uitgevoerd door leden van de SSIS-ontwikkelteam. De resultaten kunnen verschillen. Voer uw eigen tests voordat u uw configuratie-instellingen, die invloed hebben op zowel de kosten en de prestaties voltooit.

## <a name="properties-to-configure"></a>Eigenschappen configureren

Het volgende gedeelte van een configuratiescript bevat de eigenschappen die u configureren kunt wanneer u een Azure-SSIS Integration Runtime maken. Zie voor de volledige PowerShell-script en de beschrijving, [pakketten van SQL Server Integration Services implementeren naar Azure](tutorial-deploy-ssis-packages-azure-powershell.md).

```powershell
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to max(2 x number of cores, 8) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database with virtual network service endpoints or a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"
```

## <a name="azuressislocation"></a>AzureSSISLocation
**AzureSSISLocation** is de locatie voor het worker-knooppunt van integration runtime. De worker-knooppunt onderhoudt een constante verbinding met de SSIS-catalogusdatabase (SSISDB) op een Azure SQL database. Stel de **AzureSSISLocation** naar dezelfde locatie als de SQL-Database-server die als host fungeert voor SSISDB, waarmee de integratieruntime zo efficiënt mogelijk werken.

## <a name="azuressisnodesize"></a>AzureSSISNodeSize
Data Factory, met inbegrip van de Azure-SSIS-IR, ondersteunt de volgende opties:
-   Standard\_A4\_v2
-   Standard\_A8\_v2
-   Standard\_D1\_v2
-   Standard\_D2\_v2
-   Standard\_D3\_v2
-   Standard\_D4\_v2
-   Standard\_D2\_v3
-   Standard\_D4\_v3
-   Standard\_D8\_v3
-   Standard\_D16\_v3
-   Standard\_D32\_v3
-   Standard\_D64\_v3
-   Standard\_E2\_v3
-   Standard\_E4\_v3
-   Standard\_E8\_v3
-   Standard\_E16\_v3
-   Standard\_E32\_v3
-   Standard\_E64\_v3

In de niet-officiële interne tests door het engineeringteam van SSIS, lijkt de D-serie meer geschikt zijn voor uitvoering van SSIS-pakket dan de A-serie.

-   De verhouding prestaties/prijs van de D-serie is hoger dan de A-serie en de verhouding prestaties/prijs van de v3-serie is hoger dan de v2-serie.
-   De doorvoer voor de D-serie is hoger dan de A-serie tegen dezelfde prijs en de doorvoer voor de v3-serie is hoger dan de v2-serie tegen dezelfde prijs.
-   De knooppunten van de serie v2 van Azure-SSIS IR zijn niet geschikt voor aangepaste installatie, dus gebruik in plaats daarvan de v3-serie-knooppunten. Als u al de knooppunten van de serie v2, ga dan voor het gebruik van de knooppunten van de serie v3 zo snel mogelijk.
-   De E-serie is geoptimaliseerd voor geheugen VM-grootten die biedt een hogere geheugen-naar-CPU-snelheid dan andere virtuele machines. Als uw pakket een grote hoeveelheid geheugen vereist, kunt u rekening houden met E-serie VM kiezen.

### <a name="configure-for-execution-speed"></a>Configureren voor de snelheid van uitvoering
Als u hoeft niet veel pakketten om uit te voeren en u wilt dat de pakketten te snel worden uitgevoerd, gebruik de informatie in de volgende tabel om te kiezen van een type virtuele machine geschikt is voor uw scenario.

Deze gegevens vertegenwoordigt een enkel pakket kan worden uitgevoerd op een enkele worker-knooppunt. Het pakket wordt 3 miljoen records met de naam van de eerste en laatste naamkolom geladen uit Azure Blob Storage, genereert een kolom van de volledige naam en schrijft de records die de volledige naam langer is dan 20 tekens naar Azure Blob Storage.

![Snelheid van uitvoering van SSIS Integration Runtime-pakket](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speedV2.png)

### <a name="configure-for-overall-throughput"></a>Configureren voor de totale doorvoer

Gebruik de informatie in het volgende diagram als hebt u veel pakketten om uit te voeren en u het belangrijkst de algehele doorvoer, een type virtuele machine geschikt is voor uw scenario te kiezen.

![Maximale totale doorvoersnelheid van SSIS-Integratieruntime](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughputV2.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** past u de schaalbaarheid van de integratieruntime. De doorvoer van de integratieruntime is evenredig aan de **AzureSSISNodeNumber**. Stel de **AzureSSISNodeNumber** op een kleine waarde in eerste instantie de doorvoer van de integratieruntime controleren en vervolgens pas de waarde voor uw scenario. Als u wilt configureren van het aantal worker-knooppunten, Zie [beheren van een Azure-SSIS integratieruntime](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Wanneer u al een krachtige worker-knooppunt-pakketten uitvoeren, verhogen **AzureSSISMaxParallelExecutionsPerNode** verhogen de algehele doorvoer van de integratieruntime. Standard_D1_v2 knooppunten worden 1-4 parallelle uitvoeringen per knooppunt ondersteund. Voor alle andere soorten knooppunten, worden 1-max(2 x number of cores, 8) parallelle uitvoeringen per knooppunt ondersteund. Als u wilt dat **AzureSSISMaxParallelExecutionsPerNode** dan de maximale waarde wordt ondersteund, kunt u een ondersteuningsticket openen en er kan de maximale waarde voor u en na die u moet Azure Powershell gebruiken om te werken  **AzureSSISMaxParallelExecutionsPerNode**.
U kunt een schatting maken van de juiste waarde op basis van de kosten van het pakket en de volgende configuraties voor de worker-knooppunten. Zie voor meer informatie, [voor algemeen gebruik-VM-grootten](../virtual-machines/windows/sizes-general.md).

| Grootte             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Max. aantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standard\_D1\_v2 | 1    | 3,5         | 50                     | 3000 / 46 / 23                                             | 2 / 2 x 500                         | 2 / 750                                        |
| Standard\_D2\_v2 | 2    | 7           | 100                    | 6000 / 93 / 46                                             | 4 / 4 x 500                         | 2 / 1500                                       |
| Standard\_D3\_v2 | 4    | 14          | 200                    | 12.000 / 187 / 93                                           | 8 / 8 x 500                         | 4 / 3000                                       |
| Standard\_D4\_v2 | 8    | 28          | 400                    | 24.000 / 375 / 187                                          | 16 / 16 x 500                       | 8 / 6000                                       |
| Standard\_A4\_v2 | 4    | 8           | 40                     | 4000 / 80 / 40                                             | 8 / 8 x 500                         | 4 / 1000                                       |
| Standard\_A8\_v2 | 8    | 16          | 80                     | 8000 / 160 / 80                                            | 16 / 16 x 500                       | 8 / 2000                                       |
| Standard\_D2\_v3 | 2    | 8           | 50                     | 3000 / 46 / 23                                             | 4 / 6 x 500                         | 2 / 1000                                       |
| Standard\_D4\_v3 | 4    | 16          | 100                    | 6000 / 93 / 46                                             | 8 / 12 x 500                        | 2 / 2000                                       |
| Standard\_D8\_v3 | 8    | 32          | 200                    | 12.000 / 187 / 93                                           | 16 / 24 x 500                       | 4 / 4000                                       |
| Standard\_D16\_v3| 16   | 64          | 400                    | 24.000 / 375 / 187                                          | 32 / 48 x 500                        | 8 / 8000                                       |
| Standard\_D32\_v3| 32   | 128         | 800                    | 48.000 / 750 / 375                                          | 32 / 96 x 500                       | 8 / 16000                                      |
| Standard\_D64\_v3| 64   | 256         | 1600                   | 96000 / 1000 / 500                                         | 32 / 192 x 500                      | 8 / 30000                                      |
| Standard\_E2\_v3 | 2    | 16          | 50                     | 3000 / 46 / 23                                             | 4 / 6 x 500                         | 2 / 1000                                       |
| Standard\_E4\_v3 | 4    | 32          | 100                    | 6000 / 93 / 46                                             | 8 / 12 x 500                        | 2 / 2000                                       |
| Standard\_E8\_v3 | 8    | 64          | 200                    | 12.000 / 187 / 93                                           | 16 / 24 x 500                       | 4 / 4000                                       |
| Standard\_E16\_v3| 16   | 128         | 400                    | 24.000 / 375 / 187                                          | 32 / 48 x 500                       | 8 / 8000                                       |
| Standard\_E32\_v3| 32   | 256         | 800                    | 48.000 / 750 / 375                                          | 32 / 96 x 500                       | 8 / 16000                                      |
| Standard\_E64\_v3| 64   | 432         | 1600                   | 96000 / 1000 / 500                                         | 32 / 192 x 500                      | 8 / 30000                                      |

Hier vindt u de richtlijnen voor het instellen van de juiste waarde voor de **AzureSSISMaxParallelExecutionsPerNode** eigenschap: 

1. Stel deze in op een kleine waarde in eerste instantie.
2. Verhogen met een kleine hoeveelheid om te controleren of de algemene doorvoer wordt verbeterd.
3. Stop waardoor de waarde wordt verhoogd wanneer de algemene doorvoer de maximale waarde bereikt.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** is de prijscategorie voor de SSIS-catalogusdatabase (SSISDB) op een Azure SQL database. Deze instelling is van invloed op het maximum aantal werknemers in het IR-exemplaar, de snelheid in de wachtrij van de uitvoering van een pakket en de snelheid te laden in het uitvoeringslogboek.

-   Als u geen belang bij de snelheid tot het uitvoeren van de wachtrij-pakket en voor het laden van het logboek kan worden uitgevoerd, kunt u de laagste prijscategorie-database. Azure SQL Database met de prijzen voor Basic ondersteunt 8 werknemers in een exemplaar van integration runtime.

-   Kies een krachtige database dan basis als het aantal worker meer dan 8 is, of het aantal kernen meer dan 50 is. Anders wordt het knelpunt van het exemplaar van integration runtime die de database en de algehele prestaties nadelig wordt beïnvloed.

-   Kies een krachtige database zoals s3 als het niveau van logboekregistratie voor op uitgebreide is ingesteld. Volgens onze onofficiële intern testen, s3-prijscategorie biedt ondersteuning voor uitvoering van SSIS-pakket met 2 knooppunten, 128 parallelle telt het aantal en het niveau van logboekregistratie.

U kunt ook de prijscategorie op basis van database [database transaction Units](../sql-database/sql-database-what-is-a-dtu.md) (DTU) gebruiksgegevens beschikbaar in Azure portal.

## <a name="design-for-high-performance"></a>Ontwerp voor hoge prestaties
Het ontwerpen van een SSIS-pakket uit te voeren op Azure verschilt van het ontwerpen van een pakket voor on-premises worden uitgevoerd. In plaats van meerdere onafhankelijke taken in hetzelfde pakket combineren, scheidt u deze in verschillende pakketten voor efficiënter kan worden uitgevoerd in de Azure-SSIS-IR. Maak een pakket kan worden uitgevoerd voor elk pakket, zodat ze zich niet hoeven te wachten op elkaar om te voltooien. Deze aanpak profiteert van de schaalbaarheid van de Azure-SSIS integratieruntime en verbetert de algehele doorvoer.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de Azure-SSIS Integration Runtime. Zie [Azure-SSIS Integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime).
