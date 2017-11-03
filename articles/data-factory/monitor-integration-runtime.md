---
title: Controleren van de runtime integratie in Azure Data Factory | Microsoft Docs
description: Informatie over het bewaken van verschillende soorten integratie runtime in Azure Data Factory.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2017
ms.author: spelluru
ms.openlocfilehash: e1bfb7199ddf9f02297db9de529729ba3833cf8c
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="monitor-an-integration-runtime-in-azure-data-factory"></a>Monitor een runtime integratie in Azure Data Factory  
**Integratie runtime** is de beheerinfrastructuur gebruikt door Azure Data Factory voor verschillende mogelijkheden voor de integratie van gegevens in verschillende netwerkomgevingen. Er zijn drie soorten integratie runtimes die worden aangeboden door de Data Factory:

- Azure Integration Runtime
- Zelf-hostende Integration Runtime
- Azure-SSIS-integratie-runtime

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

Als u de status van een exemplaar van de integratie-runtime (IR), voer de volgende PowerShell-opdracht: 

```powershell
Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName MyDataFactory -ResourceGroupName MyResourceGroup -Name MyAzureIR -Status
``` 

De cmdlet retourneert verschillende gegevens voor verschillende soorten integratie runtime. Dit artikel worden de eigenschappen en statussen voor elk type integratie runtime.  

## <a name="azure-integration-runtime"></a>Azure Integration Runtime
De compute-resource voor een integratie van Azure-runtime is volledig elastisch beheerd in Azure. De volgende tabel bevat beschrijvingen van eigenschappen die zijn geretourneerd door de **Get-AzureRmDataFactoryV2IntegrationRuntime** opdracht:

### <a name="properties"></a>Eigenschappen
De volgende tabel bevat beschrijvingen van eigenschappen die zijn geretourneerd door de cmdlet voor een runtime integratie van Azure:

| Eigenschap | Beschrijving |
-------- | ------------- | 
| Naam | De naam van de integratie van Azure-runtime. |  
| Status | De status van de integratie van Azure-runtime. | 
| Locatie | Locatie van de integratie van Azure-runtime. Zie voor meer informatie over de locatie van een Azure-integratie runtime [Inleiding tot integratie runtime](concepts-integration-runtime.md). |
| DataFactoryName | Naam van de gegevensfactory die de integratie van Azure-runtime bij hoort. | 
| resourceGroupName | Naam van de resourcegroep waartoe de gegevensfactory.  |
| Beschrijving | Beschrijving van de integratie-runtime.  |

### <a name="status"></a>Status
De volgende tabel bevat de mogelijke statussen van een runtime integratie van Azure:

| Status | Opmerkingen/scenario 's | 
| ------ | ------------------ |
| Online | De integratie van Azure-runtime is online en klaar om te worden gebruikt. | 
| Off line | De integratie van Azure-runtime is offline vanwege een interne fout. |

## <a name="self-hosted-integration-runtime"></a>Zelf-hostende Integration Runtime
Deze sectie vindt u beschrijvingen voor eigenschappen die worden geretourneerd door de cmdlet Get-AzureRmDataFactoryV2IntegrationRuntime. 

> [!NOTE] 
> De geretourneerde eigenschappen en de status bevatten informatie over de algehele host zichzelf integratie runtime en op elk knooppunt in de runtime.  

### <a name="properties"></a>Eigenschappen

De volgende tabel bevat beschrijvingen van de eigenschappen voor bewaking **elk knooppunt**:

| Eigenschap | Beschrijving | 
| -------- | ----------- | 
| Naam | Naam van de host zichzelf integratie runtime en knooppunten die zijn gekoppeld. Knooppunt is een lokale Windows-machine met de host zichzelf integratie-runtime is geïnstalleerd. |  
| Status | De status van de algehele host zichzelf integratie-runtime en elk knooppunt. Voorbeeld: Online/Offline/Limited/enz. Zie de volgende sectie voor informatie over deze statussen. | 
| Versie | De versie van de host zichzelf integratie runtime en elk knooppunt. De versie van de runtime host zichzelf integratie wordt bepaald op basis van de versie van het merendeel van de knooppunten in de groep. Als er knooppunten met verschillende versies in de host zichzelf integratie runtime-instellingen, alleen de knooppunten met dezelfde versienummer als de logische zelf de hosting-runtime-functie Integratie juist. Anderen in de beperkte modus zijn en moeten handmatig worden bijgewerkt (alleen als automatische updates is mislukt). | 
| Beschikbaar geheugen | Beschikbaar geheugen op een host zichzelf integratie runtime-knooppunt. Deze waarde is een momentopname van een bijna realtime. | 
| CPU-gebruik | CPU-gebruik van een knooppunt van de runtime host zichzelf integratie. Deze waarde is een momentopname van een bijna realtime. |
| Networking (In/Out) | Netwerkgebruik van een knooppunt van de runtime host zichzelf integratie. Deze waarde is een momentopname van een bijna realtime. | 
| Gelijktijdige taken (actief / beperken) | Het aantal taken of taken die op elk knooppunt worden uitgevoerd. Deze waarde is een momentopname van een bijna realtime. Limiet geeft aan dat het maximum aantal gelijktijdige taken voor elk knooppunt. Deze waarde is gedefinieerd op basis van de grootte van de machine. U kunt de limiet voor gelijktijdige taakuitvoering in geavanceerde scenario's waarbij geheugen-CPU-netwerk onder gebruikt, maar activiteiten zijn time-out opschalen verhogen. Deze mogelijkheid is ook beschikbaar met een runtime-host zichzelf integratie één knooppunt. |
| Rol | Er zijn twee soorten rollen in een runtime-host zichzelf integratie met meerdere knooppunten: dispatcher- en werkrollen. Alle knooppunten zijn werknemers, wat betekent dat ze kunnen alle worden gebruikt om uit te voeren taken. Er is slechts één dispatcher knooppunt, die wordt gebruikt voor het pull-taken/taken van cloudservices en ze verzenden naar andere worker-knooppunten. Het knooppunt dispatcher is ook een werkrolknooppunt. |

Sommige instellingen van de eigenschappen ook beter wanneer er twee of meer knooppunten (scale-out-scenario) in de runtime host zichzelf integratie. 
  
### <a name="status-per-node"></a>Status (per knooppunt)
De volgende tabel bevat de mogelijke statussen van een knooppunt van de runtime host zichzelf integratie:

| Status | Beschrijving |
| ------ | ------------------ | 
| Online | Knooppunt is verbonden met de Data Factory-service. |
| Off line | Knooppunt is offline. |
| Een upgrade | Het knooppunt wordt automatisch bijgewerkt. |
| Beperkt | Vanwege een verbindingsprobleem. Kan worden veroorzaakt door HTTP-poort 8050 probleem, service bus-connectiviteitsprobleem of een synchronisatieprobleem in de referentie. |
| Inactieve | Er is een knooppunt in een configuratie van de configuratie van andere knooppunten van de meeste andere. |

Een knooppunt mag inactief zijn als er geen verbinding met andere knooppunten maken.

### <a name="status-overall-self-hosted-integration-runtime"></a>Status (runtime algehele host zichzelf integratie)
De volgende tabel bevat de mogelijke statussen van een host zichzelf integratie-runtime. Deze status is afhankelijk van de status van alle knooppunten die deel uitmaken van de runtime. 

| Status | Beschrijving |
| ------ | ----------- | 
| Registratie moet | Er is geen knooppunt is nog geregistreerd voor deze host zichzelf integratie-runtime. |
| Online | Alle knooppunten zijn online. |
| Off line | Er is geen knooppunt is online. |
| Beperkt | Niet alle knooppunten in de runtime van deze host zichzelf integratie zijn in een foutloze toestand bevindt. Deze status is een waarschuwing dat sommige knooppunten mogelijk niet actief. Deze status kan worden veroorzaakt door een synchronisatieprobleem in de referentie op dispatcher/werkrolknooppunt. |

Gebruik de **Get-AzureRmDataFactoryV2IntegrationRuntimeMetric** cmdlet voor het ophalen van de JSON-nettolading met de gedetailleerde gehost zelf integratie runtime eigenschappen en hun momentopname waarden tijdens de periode van de uitvoering van de cmdlet.

```powershell
Get-AzureRmDataFactoryV2IntegrationRuntimeMetric -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName  | | ConvertTo-Json 
```

Voorbeeld van uitvoer (wordt ervan uitgegaan dat er twee knooppunten die zijn gekoppeld aan deze host zichzelf integratie runtime zijn):

```json
{
    "IntegrationRuntimeName":  "<Name of your integration runtime>",
    "ResourceGroupName":  "<Resource Group Name>",
    "DataFactoryName":  "<Data Factory Name>",
    "Nodes":  [
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        },
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        }

    ]
} 
```


## <a name="azure-ssis-integration-runtime"></a>Azure-SSIS-integratie-runtime
Azure-SSIS-integratie runtime is een volledig beheerde cluster van Azure virtuele machines (of knooppunten) toegewezen voor het uitvoeren van SSIS-pakketten. Alle andere activiteiten van Azure Data Factory kan niet worden uitgevoerd. Eenmaal ingericht, kunt u deze kunt opvragen van de eigenschappen en de algemene/knooppuntspecifieke-statussen controleren.

### <a name="properties"></a>Eigenschappen

| De eigenschap/Status | Beschrijving |
| --------------- | ----------- |
| createTime | De UTC-tijd waarop de runtime van uw Azure-SSIS-integratie is gemaakt. |
| Knooppunten | De toegewezen/beschikbare knooppunten van de runtime van uw Azure-SSIS-integratie met knooppunt-specifieke statussen (starten/beschikbare/recycling/niet beschikbaar) en fouten actie worden uitgevoerd. |
| OtherErrors | Het knooppunt specifieke bruikbare fouten op uw Azure-SSIS-integratie-runtime. |
| LastOperation | Het resultaat van de laatste starten/stoppen-bewerking op de runtime van uw Azure SSIS-integratie met bruikbare fout(en) als is mislukt. |
| Status | De algehele status (initiële/starten/gestart/gestopt/gestopt) van uw Azure-SSIS-integratie-runtime. |
| Locatie | De locatie van uw Azure-SSIS-integratie-runtime. |
| NodeSize | De grootte van elk knooppunt van uw Azure-SSIS-integratie-runtime. |
| NodeCount | Het aantal knooppunten in uw Azure-SSIS-integratie-runtime. |
| MaxParallelExecutionsPerNode | Het aantal parallelle uitvoeringen per knooppunt in de runtime van uw Azure-SSIS-integratie. |
| CatalogServerEndpoint | Het eindpunt van uw bestaande exemplaar van Azure SQL Database/beheerde server host SSISDB. |
| CatalogAdminUserName | De beheerdersgebruikersnaam van uw bestaande exemplaar van Azure SQL Database/beheerde server. Data Factory-service gebruikt deze informatie voor het voorbereiden en SSISDB beheren namens jou. |
| CatalogAdminPassword | Het beheerderswachtwoord van uw bestaande exemplaar van Azure SQL Database/beheerde server. |
| CatalogPricingTier | De prijscategorie voor SSISDB gehost door uw bestaande Azure SQL Database-server.  Niet van toepassing op Azure SQL beheerd exemplaar SSISDB hosten. |
| VNetId | Het virtueel netwerk (VNet) resource-ID voor uw Azure-SSIS-integratie runtime om toe te voegen. |
| Subnet | De subnetnaam voor uw Azure-SSIS-integratie runtime om toe te voegen. |
| Id | De resource-ID van de runtime van uw Azure-SSIS-integratie. |
| Type | Het type (beheerde/zelfstandige-Hosted) van de runtime van uw Azure-SSIS-integratie. |
| resourceGroupName | De naam van uw Azure-resourcegroep, waarin uw gegevensfactory en de Azure-SSIS-integratie runtime zijn gemaakt. |
| DataFactoryName | De naam van uw Azure-gegevensfactory. |
| Naam | De naam van uw Azure-SSIS-integratie-runtime. |
| Beschrijving | De beschrijving van de runtime van uw Azure-SSIS-integratie. |

  
### <a name="status-per-node"></a>Status (per knooppunt)

| Status | Beschrijving |
| ------ | ----------- | 
| Starting | Dit knooppunt wordt voorbereid. |
| Beschikbaar | Dit knooppunt is gereed voor u SSIS-pakketten implementeren/uitvoeren. |
| Recyclen | Dit knooppunt is wordt hersteld/opnieuw te starten. |
| Niet beschikbaar | Dit knooppunt is niet gereed voor u SSIS-pakketten implementeren/uitvoeren en heeft bruikbare fouten/problemen die kunnen worden opgelost. |

### <a name="status-overall-azure-ssis-integration-runtime"></a>Status (algemene Azure SSIS-integratie runtime)

| Algemene status | Beschrijving | 
| -------------- | ----------- | 
| Initiële | De knooppunten van de runtime van uw Azure-SSIS-integratie is niet toegewezen/voorbereid. | 
| Starting | De knooppunten van de runtime van uw Azure-SSIS-integratie worden toegewezen/voorbereid en facturering is gestart. |
| Gestart | De knooppunten van de runtime van uw Azure-SSIS-integratie zijn toegewezen/voorbereid en ze gereed zijn voor u SSIS-pakketten implementeren/uitvoeren. |
| Stopping  | De knooppunten van de runtime van uw Azure-SSIS-integratie zijn wordt vrijgegeven. |
| Stopped | De knooppunten van de runtime van uw Azure-SSIS-integratie zijn vrijgegeven en facturering is gestopt. |

Zie de volgende artikelen voor meer informatie over Azure-SSIS-integratie runtime:

- [Azure-SSIS-integratie Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat conceptuele informatie over de integratie runtimes in het algemeen met inbegrip van de Azure-SSIS-IR 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-deploy-ssis-packages-azure.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Procedure: Een Azure SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). Dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Managed Instance (Private Preview) en het toevoegen van de IR aan een VNet. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Er wordt ook uitgelegd hoe u een Azure-SSIS IR kunt uitschalen door meer knooppunten toe te voegen aan de IR. 
- [Een Azure-SSIS IR toevoegen aan een VNet](join-azure-ssis-integration-runtime-virtual-network.md). Dit artikel bevat algemene informatie over het toevoegen van een Azure-SSIS IR aan een virtueel netwerk (VNet) van Azure. Er wordt ook beschreven hoe u Azure Portal gebruikt om VNet te configureren voor het deelnemen van een Azure-SSIS IR aan het VNet. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor het bewaken van pijplijnen op verschillende manieren: 

- [Snelstartgids: Maak een gegevensfactory](quickstart-create-data-factory-dot-net.md).
- [Gebruik Azure Monitor Data Factory-pijplijnen bewaken](monitor-using-azure-monitor.md)