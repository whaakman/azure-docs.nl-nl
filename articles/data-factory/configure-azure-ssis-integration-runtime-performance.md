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
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 2592c81947f48c10891fe920647612d5c30af64f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989073"
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>De Azure-SSIS Integration Runtime voor hoge prestaties configureren

In dit artikel wordt beschreven hoe het configureren van een Azure-SSIS Integration Runtime (IR) voor hoge prestaties. De Azure-SSIS IR kunt u het implementeren en uitvoeren van pakketten van SQL Server Integration Services (SSIS) in Azure. Zie voor meer informatie over Azure-SSIS IR [integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime) artikel. Zie voor meer informatie over het implementeren en uitvoeren van SSIS-pakketten op Azure [Lift- and -shift SQL Server integratieservices-workloads naar de cloud](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

> [!IMPORTANT]
> In dit artikel bevat prestatieresultaten en metingen uit interne tests uitgevoerd door leden van de SSIS-ontwikkelteam. De resultaten kunnen verschillen. Voer uw eigen tests voordat u uw configuratie-instellingen, die invloed hebben op zowel de kosten en de prestaties voltooit.

## <a name="properties-to-configure"></a>Eigenschappen configureren

Het volgende gedeelte van een configuratiescript bevat de eigenschappen die u configureren kunt wanneer u een Azure-SSIS Integration Runtime maken. Zie voor de volledige PowerShell-script en de beschrijving, [pakketten van SQL Server Integration Services implementeren naar Azure](tutorial-deploy-ssis-packages-azure-powershell.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# Only EastUS, NorthEurope, and WestEurope are supported.
$AzureSSISLocation = "EastUS" 
# Only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D3_v2"
# Only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Database Managed Instance
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>"
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
-   Standard\_D4\_v2.

In de niet-officiële interne tests door het engineeringteam van SSIS, lijkt de D-serie meer geschikt zijn voor uitvoering van SSIS-pakket dan de A-serie.

-   De verhouding prestaties/prijs van de D-serie is hoger dan de A-serie.
-   De doorvoer voor de D-serie is hoger dan de A-serie tegen dezelfde prijs.

### <a name="configure-for-execution-speed"></a>Configureren voor de snelheid van uitvoering
Als u hoeft niet veel pakketten om uit te voeren en u wilt dat de pakketten te snel worden uitgevoerd, gebruik de informatie in de volgende tabel om te kiezen van een type virtuele machine geschikt is voor uw scenario.

Deze gegevens vertegenwoordigt een enkel pakket kan worden uitgevoerd op een enkele worker-knooppunt. Het pakket wordt 10 miljoen records met de naam van de eerste en laatste naamkolom geladen uit Azure Blob Storage, genereert een kolom van de volledige naam en schrijft de records die de volledige naam langer is dan 20 tekens naar Azure Blob Storage.

![Snelheid van uitvoering van SSIS Integration Runtime-pakket](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speed.png)

### <a name="configure-for-overall-throughput"></a>Configureren voor de totale doorvoer

Gebruik de informatie in het volgende diagram als hebt u veel pakketten om uit te voeren en u het belangrijkst de algehele doorvoer, een type virtuele machine geschikt is voor uw scenario te kiezen.

![Maximale totale doorvoersnelheid van SSIS-Integratieruntime](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughput.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** past u de schaalbaarheid van de integratieruntime. De doorvoer van de integratieruntime is evenredig aan de **AzureSSISNodeNumber**. Stel de **AzureSSISNodeNumber** op een kleine waarde in eerste instantie de doorvoer van de integratieruntime controleren en vervolgens pas de waarde voor uw scenario. Als u wilt configureren van het aantal worker-knooppunten, Zie [beheren van een Azure-SSIS integratieruntime](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Wanneer u al een krachtige worker-knooppunt-pakketten uitvoeren, verhogen **AzureSSISMaxParallelExecutionsPerNode** verhogen de algehele doorvoer van de integratieruntime. Standard_D1_v2 knooppunten worden 1-4 parallelle uitvoeringen per knooppunt ondersteund. Voor alle andere soorten knooppunten, worden 1-8 parallelle uitvoeringen per knooppunt ondersteund.
U kunt een schatting maken van de juiste waarde op basis van de kosten van het pakket en de volgende configuraties voor de worker-knooppunten. Zie voor meer informatie, [voor algemeen gebruik-VM-grootten](../virtual-machines/windows/sizes-general.md).

| Grootte             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standard\_D1\_v2 | 1    | 3,5         | 50                     | 3000 / 46 / 23                                             | 2 / 2 x 500                         | 2 / 750                                        |
| Standard\_D2\_v2 | 2    | 7           | 100                    | 6000 / 93 / 46                                             | 4 / 4 x 500                         | 2 / 1500                                       |
| Standard\_D3\_v2 | 4    | 14          | 200                    | 12.000 / 187 / 93                                           | 8 / 8 x 500                         | 4 / 3000                                       |
| Standard\_D4\_v2 | 8    | 28          | 400                    | 24.000 / 375 / 187                                          | 16 / 16 x 500                       | 8 / 6000                                       |
| Standard\_A4\_v2 | 4    | 8           | 40                     | 4000 / 80 / 40                                             | 8 / 8 x 500                         | 4 / 1000                                       |
| Standard\_A8\_v2 | 8    | 16          | 80                     | 8000 / 160 / 80                                            | 16 / 16 x 500                       | 8 / 2000                                       |

Hier vindt u de richtlijnen voor het instellen van de juiste waarde voor de **AzureSSISMaxParallelExecutionsPerNode** eigenschap: 

1. Stel deze in op een kleine waarde in eerste instantie.
2. Verhogen met een kleine hoeveelheid om te controleren of de algemene doorvoer wordt verbeterd.
3. Stop waardoor de waarde wordt verhoogd wanneer de algemene doorvoer de maximale waarde bereikt.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** is de prijscategorie voor de SSIS-catalogusdatabase (SSISDB) op een Azure SQL database. Deze instelling is van invloed op het maximum aantal werknemers in het IR-exemplaar, de snelheid in de wachtrij van de uitvoering van een pakket en de snelheid te laden in het uitvoeringslogboek.

-   Als u geen belang bij de snelheid tot het uitvoeren van de wachtrij-pakket en voor het laden van het logboek kan worden uitgevoerd, kunt u de laagste prijscategorie-database. Azure SQL Database met de prijzen voor Basic ondersteunt 8 werknemers in een exemplaar van integration runtime.

-   Kies een krachtige database dan basis als het aantal worker meer dan 8 is, of het aantal kernen meer dan 50 is. Anders wordt het knelpunt van het exemplaar van integration runtime die de database en de algehele prestaties nadelig wordt beïnvloed.

U kunt ook de prijscategorie op basis van database [database transaction Units](../sql-database/sql-database-what-is-a-dtu.md) (DTU) gebruiksgegevens beschikbaar in Azure portal.

## <a name="design-for-high-performance"></a>Ontwerp voor hoge prestaties
Het ontwerpen van een SSIS-pakket uit te voeren op Azure verschilt van het ontwerpen van een pakket voor on-premises worden uitgevoerd. In plaats van meerdere onafhankelijke taken in hetzelfde pakket combineren, scheidt u deze in verschillende pakketten voor efficiënter kan worden uitgevoerd in de Azure-SSIS-IR. Maak een pakket kan worden uitgevoerd voor elk pakket, zodat ze zich niet hoeven te wachten op elkaar om te voltooien. Deze aanpak profiteert van de schaalbaarheid van de Azure-SSIS integratieruntime en verbetert de algehele doorvoer.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de Azure-SSIS Integration Runtime. Zie [Azure-SSIS Integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime).
