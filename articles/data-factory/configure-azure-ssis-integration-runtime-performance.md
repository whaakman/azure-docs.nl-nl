---
title: Configureren van de Runtime Azure SSIS-integratie voor hoge prestaties | Microsoft Docs
description: Informatie over het configureren van de eigenschappen van de Runtime Azure SSIS-integratie voor hoge prestaties
services: data-factory
ms.date: 01/10/2018
ms.topic: article
ms.service: data-factory
ms.workload: data-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7d0e75ad85731b10f9a993c2fa62f30c0142ed05
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>Configureren van de Runtime Azure SSIS-integratie voor hoge prestaties

Dit artikel wordt beschreven hoe u een Azure-SSIS integratie Runtime (IR) voor hoge prestaties configureert. De Azure-SSIS-IR kunt u implementeren en uitvoeren van SQL Server Integration Services (SSIS) pakketten in Azure. Zie voor meer informatie over Azure-SSIS-IR [integratie runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime) artikel. Zie voor meer informatie over het implementeren en uitvoeren van SSIS-pakketten op Azure [Lift en shift SQL Server integratieservices-werkbelastingen naar de cloud](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

> [!IMPORTANT]
> In dit artikel bevat prestatieresultaten en metingen uit interne tests uitgevoerd door leden van het team SSIS-ontwikkeling. De resultaten kunnen verschillen. Voer uw eigen tests voordat u uw configuratie-instellingen, waardoor zowel kosten en prestaties voltooit.

## <a name="properties-to-configure"></a>Eigenschappen configureren

Het volgende gedeelte van een configuratiescript ziet u de eigenschappen die u configureren kunt wanneer u een Azure-SSIS-integratie Runtime maakt. Zie voor de volledige PowerShell-script en de beschrijving [pakketten van de SQL Server Integration Services implementeren naar Azure](tutorial-deploy-ssis-packages-azure.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS, NorthEurope, and WestEurope are supported.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>"
```

## <a name="azuressislocation"></a>AzureSSISLocation
**AzureSSISLocation** is de locatie voor het werkrolknooppunt integratie-runtime. Het werkrolknooppunt onderhoudt een constante verbinding met de catalogus SSIS-database (SSISDB) op een Azure SQL database. Stel de **AzureSSISLocation** naar dezelfde locatie als de SQL-Database-server die als host fungeert voor SSISDB, waarmee de runtime integratie zo efficiënt mogelijk werken.

## <a name="azuressisnodesize"></a>AzureSSISNodeSize
De openbare evaluatieversie van Azure Data Factory-v2, met inbegrip van de Azure-SSIS-IR ondersteunt de volgende opties:
-   Standard\_A4\_v2
-   Standard\_A8\_v2
-   Standard\_D1\_v2
-   Standard\_D2\_v2
-   Standard\_D3\_v2
-   Standard\_D4\_v2.

De onofficiële interne tests door het technische team SSIS, lijkt de D-reeks beter geschikt is voor de uitvoering van SSIS-pakket dan de A-serie.

-   De verhouding van prestaties/prijs van de D-reeks is hoger dan de A-serie.
-   De doorvoer voor de D-reeks is hoger dan de A-reeks met dezelfde prijs.

### <a name="configure-for-execution-speed"></a>Configureren voor de snelheid van uitvoering
Gebruik de informatie in het volgende diagram als er geen veel pakketten om uit te voeren en u wilt dat de pakketten worden snel uitgevoerd, een type virtuele machine geschikt is voor uw scenario te kiezen.

Deze gegevens vertegenwoordigt de uitvoering van een enkel pakket op een enkele werkrolknooppunt. Het pakket wordt geladen 10 miljoen records met de naam van de eerste en laatste naam kolommen uit Azure Blob Storage, genereert een kolom van de volledige naam en schrijft de records die de volledige naam die meer dan 20 tekens naar Azure Blob Storage.

![Snelheid van SSIS-integratie Runtime-pakket van uitvoering](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speed.png)

### <a name="configure-for-overall-throughput"></a>Voor de totale doorvoer configureren

Als u veel pakketten om uit te voeren hebt en u het belangrijkst de totale doorvoer, gebruik de informatie in het volgende diagram kiezen een virtuele machine geschikt is voor uw scenario.

![De maximale totale doorvoer SSIS-integratie Runtime](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughput.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** past u de schaalbaarheid van de integratie-runtime. De doorvoer van de integratie-runtime is evenredig aan de **AzureSSISNodeNumber**. Stel de **AzureSSISNodeNumber** op een kleine waarde eerst de doorvoer van de runtime integratie bewaken en pas de waarde voor uw scenario. Zie voor het configureren van het aantal worker-knooppunten, [beheren van een Azure-SSIS-integratie runtime](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Als u al een krachtige werkrolknooppunt gebruikt om uit te voeren van pakketten, verhogen **AzureSSISMaxParallelExecutionsPerNode** de totale doorvoer van de runtime integratie mogelijk verhogen. Voor Standard_D1_v2 knooppunten, worden 1-4 parallelle uitvoeringen per knooppunt ondersteund. Voor alle andere soorten knooppunten, worden 1-8 parallelle uitvoeringen per knooppunt ondersteund.
U kunt een schatting maken van de juiste waarde op basis van de kosten van het pakket en de volgende configuraties voor worker-knooppunten. Zie voor meer informatie [grootte van algemene virtuele machines](../virtual-machines/windows/sizes-general.md).

| Grootte             | vCPU | Geheugen: GiB | Tijdelijke opslag (SSD) GiB | Maximale tijdelijke opslagdoorvoer: IOPS / MBps lezen / MBps schrijven | Maximumaantal gegevensschijven / doorvoer: IOPS | Maximum aantal NIC's/verwachte netwerkprestaties (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standard\_D1\_v2 | 1    | 3,5         | 50                     | 3000 / 46 / 23                                             | 2 / 2 x 500                         | 2 / 750                                        |
| Standard\_D2\_v2 | 2    | 7           | 100                    | 6000 / 93 / 46                                             | 4 / 4 x 500                         | 2 / 1500                                       |
| Standard\_D3\_v2 | 4    | 14          | 200                    | 12.000 / 187 / 93                                           | 8 / 8 x 500                         | 4 / 3000                                       |
| Standard\_D4\_v2 | 8    | 28          | 400                    | 24.000 / 375 / 187                                          | 16 / 16 x 500                       | 8 / 6000                                       |
| Standard\_A4\_v2 | 4    | 8           | 40                     | 4000 / 80 / 40                                             | 8 / 8 x 500                         | 4 / 1000                                       |
| Standard\_A8\_v2 | 8    | 16          | 80                     | 8000 / 160 / 80                                            | 16 / 16 x 500                       | 8 / 2000                                       |

Hier volgen de richtlijnen voor het instellen van de juiste waarde voor de **AzureSSISMaxParallelExecutionsPerNode** eigenschap: 

1. Stel deze in op een kleine waarde eerst.
2. Vergroot het met een kleine hoeveelheid om te controleren of de totale doorvoer wordt verbeterd.
3. Stop de hogere waarde wanneer de totale doorvoer de maximumwaarde heeft bereikt.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** is de prijscategorie van de catalogus SSIS-database (SSISDB) op een Azure SQL database. Deze instelling bepaalt het maximum aantal werknemers in het IR-exemplaar, de snelheid in de wachtrij van de pakketuitvoering van een en de snelheid voor het laden van het logboek kan worden uitgevoerd.

-   Als u geen belang bij de snelheid wachtrij pakket kan worden uitgevoerd en voor het laden van het logboek kan worden uitgevoerd, kunt u de laagste database prijscategorie kiezen. Azure SQL Database met Basic prijzen ondersteunt 8 werknemers in een exemplaar van de runtime integratie.

-   Kies een krachtiger dan een Basic-database als het aantal worker meer dan 8 is of het aantal kernen meer dan 50 is. Anders wordt het knelpunt van het exemplaar van de runtime integratie die de database en de prestaties nadelig wordt beïnvloed.

U kunt ook de prijscategorie op basis van de database aanpassen [database transaction unit](../sql-database/sql-database-what-is-a-dtu.md) (DTU) gebruiksinformatie beschikbaar in de Azure portal.

## <a name="design-for-high-performance"></a>Ontwerp voor hoge prestaties
Het ontwerpen van een SSIS-pakket kan worden uitgevoerd op Azure wijkt af van het ontwerpen van een pakket voor lokale uitvoering. In plaats van meerdere onafhankelijke taken in hetzelfde pakket worden gecombineerd, scheidt u deze in verschillende pakketten voor efficiënter worden uitgevoerd in de Azure-SSIS-IR Maak een pakket kan worden uitgevoerd voor elk pakket zodat ze niet hoeven te wachten voor elkaar te voltooien. Deze aanpak profiteert van de schaalbaarheid van de runtime Azure SSIS-integratie en verbetert de totale doorvoer.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de integratie van Azure SSIS Runtime. Zie [Azure SSIS-integratie Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime).