---
title: Versies 1 en 2 van Azure Data Factory vergelijken | Microsoft Docs
description: In dit artikel worden versies 1 en 2 van Azure Data Factory vergeleken.
services: data-factory
documentationcenter: 
author: kromerm
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/20/2017
ms.author: makromer
ms.openlocfilehash: f55348e9974de17c6d7e704e185f1ea9b21ff66e
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="compare-azure-data-factory-versions-1-and-2"></a>Versies 1 en 2 van Azure Data Factory vergelijken
In dit artikel wordt versie 2 (V2) van Azure Data Factory vergeleken met versie 1 (V1). Zie [Inleiding tot Azure Data Factory](v1/data-factory-introduction.md) voor een inleiding tot V1 van de service, en [Inleiding tot Azure Data Factory versie 2](introduction.md) voor V2.

## <a name="feature-comparison"></a>Vergelijking van functies
In de volgende tabel worden de functies van V1 en V2 vergeleken. 

| Functie | V1 | V2 | 
| ------- | --------- | --------- | 
| Gegevenssets | Een benoemde weergave van gegevens met een verwijzing naar de gegevens die u als in- en uitvoer wilt gebruiken in uw activiteiten. Met gegevenssets worden gegevens binnen andere gegevensarchieven geïdentificeerd, waaronder tabellen, bestanden, mappen en documenten. Een Azure Blob-gegevensset benoemt bijvoorbeeld de blobcontainer en -map in de Blob-opslag van waaruit de activiteit de gegevens moet lezen.<br/><br/>**Beschikbaarheid** definieert het segmenteringsmodel voor het verwerkingsvenster voor de gegevensset (bijvoorbeeld elk uur, dagelijks, enzovoort). | In V2 zijn de gegevenssets dezelfde. U hoeft echter geen **beschikbaarheidsschema's** voor gegevenssets te definiëren. U kunt een triggerbron definiëren waarmee pijplijnen kunnen worden gepland op basis van een klokplannermodel. Zie [Triggers](concepts-pipeline-execution-triggers.md#triggers) en [Gegevenssets](concepts-datasets-linked-services.md) voor meer informatie. | 
| Gekoppelde services | Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. | Hetzelfde als Data Factory V1, maar met een nieuwe **connectVia**-eigenschap voor het gebruik van de Data Factory V2 Integration Runtime-rekenomgeving. Zie [Integration runtimes](concepts-integration-runtime.md) (Integratieruntimes) en [Eigenschappen van de gekoppelde service voor Azure Blob-opslag](connector-azure-blob-storage.md#linked-service-properties). |
| Pijplijnen | Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. U hebt startTime, endTime en isPaused gebruikt voor het plannen en uitvoeren van pijplijnen. | Pijplijnen zijn nog steeds groepen met activiteiten die op gegevens moeten worden uitgevoerd. Het plannen van activiteiten in de pijplijn is echter gescheiden in nieuwe triggerbronnen. In Data Factory V2 kunt u zich pijplijnen voorstellen als 'werkstroomeenheden' die u afzonderlijk via triggers plant. <br/><br/>In Data Factory V2 hebben pijplijnen geen 'vensters' voor uitvoering in de tijd. De concepten startTime, endTime en isPaused van Data Factory V1 zijn niet meer aanwezig in Data Factory V2. Zie [Pijplijnuitvoering en triggers](concepts-pipeline-execution-triggers.md) en [Pijplijnen en activiteiten](concepts-pipelines-activities.md) voor meer informatie. |
| Activiteiten | Activiteiten definiëren welke acties binnen een pijplijn moeten worden uitgevoerd op uw gegevens. Activiteiten als het verplaatsen van gegevens (kopieeractiviteit) en het transformeren van gegevens (zoals Hive, Pig en MapReduce) worden ondersteund. | In Data Factory V2 zijn activiteiten nog steeds gedefinieerde acties binnen een pijplijn. In V2 worden nieuwe [controlestroomactiviteiten](concepts-pipelines-activities.md#control-activities) geïntroduceerd. U gebruikt deze activiteiten in controlestroom (lussen en vertakkingen). Gegevensverplaatsings- en gegevenstransformatieactiviteiten die in V1 werden ondersteund, worden in V2 ondersteund. In V2 kunt u transformatieactiviteiten definiëren zonder gegevenssets te gebruiken. |
| Hybride verplaatsing van gegevens en verzenden van activiteiten | Voorheen bekend als [gegevensbeheergateway](v1/data-factory-data-management-gateway.md) en bood ondersteuning voor het verplaatsen van gegevens tussen on-premises en de cloud. Het heeft nu de naam Integration Runtime.| Naar gegevensbeheergateway wordt nu verwezen als een zelfgehoste cloudintegratieruntime. Het biedt dezelfde mogelijkheden als in V1. <br/><br/> Azure-SSIS Integration Runtime in V2 ondersteunt teven het implementeren en uitvoeren van SSIS-pakketten (SQL Server Integration Services) in de cloud. Zie het artikel [Integration Runtime](concepts-integration-runtime.md) voor meer informatie.|
| Parameters | N.v.t. | Parameters zijn sleutel-waardeparen van alleen-lezen-configuratie-instellingen die in pijplijnen worden gedefinieerd. U kunt argumenten voor de parameters doorgeven als u de pijplijn handmatig uitvoert. Als u een scheduler-trigger gebruikt, kan de trigger ook waarden voor de parameters doorgeven. Activiteiten binnen de pijplijn gebruiken de parameterwaarden.  |
| Expressies | In Data Factory V1 kunt u functies en systeemvariabelen gebruiken in gegevensselectiequery's en activiteits-/gegevensseteigenschappen. | In Data Factory V2 kunt u overal in een JSON-tekenreekswaarde gebruikmaken van expressies. Zie [Expressies en functies in Azure Data Factory](control-flow-expression-language-functions.md) voor meer informatie.|
| Pijplijnuitvoeringen | N.v.t. | Eén instantie van een uitvoeringsbewerking van een pijplijn. Stel dat u een pijplijn hebt die wordt uitgevoerd om 8 uur, 9 uur en 10 uur. In dit geval wordt de pijplijn drie keer afzonderlijk uitgevoerd (pijplijnuitvoeringen). Elke pijplijnuitvoering heeft een unieke pijplijn-ID. Dit is een unieke GUID die de betreffende specifieke pijplijnuitvoering definieert. Pijplijnuitvoeringen worden doorgaans geïnstantieerd doordat argumenten worden doorgegeven aan gedefinieerde parameters in de pijplijnen. |
| Uitvoering van activiteiten | N.v.t. | Een instantie van een uitvoerbewerking van een activiteit binnen een pijplijn. | 
| Triggeruitvoeringen | N.v.t. | Een instantie van een uitvoerbewerking van een trigger. Zie [Triggers](concepts-pipeline-execution-triggers.md) voor meer informatie. |
| Planning | Planning is gebaseerd op de begin-/eindtijd van een pijplijn en de beschikbaarheid van gegevenssets. | Scheduler-trigger of uitvoering via externe planner. Zie [Pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md) voor meer informatie. |

De volgende secties bevatten meer informatie over versie 2: 

## <a name="control-flow"></a>Controlestroom  
Data Factory V2 heeft een nieuw flexibel gegevenspijplijnmodel dat niet langer is gekoppeld aan tijdseriegegevens ter ondersteuning van diverse integratiestromen en -patronen in het moderne datawarehouse. Dit zijn enkele veelvoorkomende stromen die nu mogelijk zijn geworden:   

### <a name="chaining-activities"></a>Koppelingsactiviteiten
In versie 1 moest u de uitvoer van een activiteit configureren als de uitvoer van een andere activiteit om ze te koppelen. In V2 kunt u activiteiten in een reeks binnen een pijplijn koppelen. U kunt de eigenschap **dependsOn** in een activiteitsdefinitie gebruiken om deze te koppelen aan een upstream-activiteit. Zie de artikelen [Pijplijnen en activiteiten](concepts-pipelines-activities.md#multiple-activities-in-a-pipeline) en [Vertakkings- en koppelingsactiviteiten](tutorial-control-flow.md) voor meer informatie en een voorbeeld. 

### <a name="branching-activities"></a>Vertakkingsactiviteiten
In V2 kunt u activiteiten binnen een pijplijn koppelen. De [activiteit Als-voorwaarde](control-flow-if-condition-activity.md) biedt dezelfde functionaliteit als een `if`-instructie in een programmeertaal. Er wordt een reeks activiteiten mee geëvalueerd als de voorwaarde resulteert in `true` en een andere reeks activiteiten als de voorwaarde resulteert in `false`. Zie [Vertakkings- en koppelingsactiviteiten](tutorial-control-flow.md) voor een voorbeeld van vertakkingsactiviteiten.

### <a name="parameters"></a>Parameters 
U kunt parameters definiëren op pijplijnniveau en de argumenten doorgeven tijdens het aanroepen van de pijplijn op aanvraag of vanuit een trigger. Activiteiten kunnen de argumenten die zijn doorgegeven aan de pijplijn gebruiken. Zie [Pijplijnen en triggers](concepts-pipeline-execution-triggers.md) voor meer informatie. 

### <a name="custom-state-passing"></a>Aangepaste status doorgeven
Activiteituitvoer zoals de status kan worden gebruikt door een volgende activiteit in de pijplijn. In de JSON-definitie van een activiteit bijvoorbeeld, hebt u toegang tot de uitvoer van de vorige activiteit door middel van de volgende syntaxis: `@activity('NameofPreviousActivity').output.value`. Met deze functie kunt u werkstromen bouwen waar waarden activiteiten kunnen passeren.

### <a name="looping-containers"></a>Herhalende containers
De [activiteit ForEach](control-flow-for-each-activity.md) definieert een herhalende controlestroom in de pijplijn. Deze activiteit wordt gebruikt om een verzameling te herhalen en voert opgegeven activiteiten uit in een lus. De lusimplementatie van deze activiteit is vergelijkbaar met Foreach-lusstructuur in computertalen. 

De activiteit [Until](control-flow-until-activity.md) biedt dezelfde functionaliteit als de lusstructuur do-until in een programmeertaal. Er wordt een reeks activiteiten uitgevoerd totdat de voorwaarde die aan de activiteit is gekoppeld, resulteert in waar. U kunt in Data Factory een time-outwaarde voor de Until-activiteit opgeven.  

### <a name="trigger-based-flows"></a>Op triggers gebaseerde stromen
Pijplijnen kunnen op aanvraag of volgens wandklokplanningen worden geactiveerd. In het artikel [Pijplijnen en triggers](concepts-pipeline-execution-triggers.md) vindt u meer informatie over triggers. 

### <a name="invoke-a-pipeline-from-another-pipeline"></a>Een pijplijn aanroepen vanaf een andere pijplijn
De [activiteit Execute Pipeline](control-flow-execute-pipeline-activity.md) stelt een Data Factory-pijplijn in staat om een andere pijplijn aan te roepen.

### <a name="delta-flows"></a>Deltastromen
Een belangrijke use case in ETL-patronen zijn 'deltaladingen', het laden van gegevens die zijn gewijzigd sinds de laatste iteratie van een pijplijn. Nieuwe mogelijkheden in versie 2, zoals de [activiteit lookup](control-flow-lookup-activity.md), flexibele planning en controlestroom maken deze use case op een natuurlijke manier mogelijk. Zie [Zelfstudie: stapsgewijs kopiëren](tutorial-incremental-copy-powershell.md) voor een zelfstudie met stapsgewijze instructies.

### <a name="other-control-flow-activities"></a>Andere controlestroomactiviteiten
Hier volgen nog enkele controlestroomactiviteiten die door Data Factory V2 worden ondersteund: 

Controleactiviteit | Beschrijving
---------------- | -----------
[ForEachActivity](control-flow-for-each-activity.md) | De ForEachActivity definieert een herhalende controlestroom in de pijplijn. Deze activiteit wordt gebruikt om een verzameling te herhalen en voert opgegeven activiteiten uit in een lus. De lusimplementatie van deze activiteit is vergelijkbaar met Foreach-lusstructuur in computertalen.
[WebActivity](control-flow-web-activity.md) | De WebActivity kan worden gebruikt om een aangepast REST-eindpunt aan te roepen vanaf een Data Factory-pijplijn. U kunt gegevenssets en gekoppelde services doorgeven die moten worden verbruikt door en die toegankelijk zijn voor de activiteit. 
[Lookup Activity](control-flow-lookup-activity.md) | De Lookup Activity kan worden gebruikt om een record/tabelnaam/waarde van een externe bron te lezen of op te zoeken. Er kan naar deze uitvoer worden verwezen door volgende activiteiten. 
[Get Metadata Activity](control-flow-get-metadata-activity.md) | De Get Metadata Activity kan worden gebruikt voor het ophalen van metagegevens van gegevens in Azure Data Factory. 
[Wait Activity](control-flow-wait-activity.md) | De pijplijn wacht (pauzeert) gedurende de opgegeven periode.

## <a name="deploy-ssis-packages-to-azure"></a>SSIS-pakketten implementeren in Azure 
Als u de SSIS-werkbelastingen (SQL Server Integration Services) naar de cloud wilt verplaatsen, maakt u een data factory V2 en richt u een Azure-SSIS Integration Runtime (IR) in. Azure-SSIS IR is een volledig beheerde cluster met virtuele Azure-machines (knooppunten) die uw SSIS-pakketten uitvoeren in de cloud. Als u Azure-SSIS IR hebt ingericht, kunt u dezelfde hulpmiddelen gebruiken die u hebt gebruikt voor het implementeren van SSIS-pakketten in een on-premises SSIS-omgeving. U kunt bijvoorbeeld SQL Server Data Tools (SSDT) of SQL Server Management Studio (SSMS) gebruiken om SSIS-pakketten te implementeren in deze runtime op Azure. Zie de zelfstudie [Pakketten van SQL Server Integration Services implementeren in Azure](tutorial-deploy-ssis-packages-azure.md) voor stapsgewijze instructies. 

## <a name="flexible-scheduling"></a>Flexibel plannen
In Data Factory V2 hoeft u geen beschikbaarheidschema's voor gegevenssets te definiëren. U kunt een triggerbron definiëren waarmee pijplijnen kunnen worden gepland op basis van een klokplannermodel. U kunt ook vanuit een trigger parameters aan pijplijnen doorgeven voor een flexibel plannings-/uitvoeringsmodel. In Data Factory V2 hebben pijplijnen geen 'vensters' voor uitvoering in de tijd. De concepten startTime, endTime en isPaused van Data Factory V1 zijn niet meer aanwezig in Data Factory V2. U bouwt en plant als volgt een pijplijn in Data Factory V2: [Pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md).

## <a name="support-for-more-data-stores"></a>Ondersteuning voor meer gegevensarchieven
V2 ondersteunt het kopiëren van gegevens van/naar meer gegevensarchieven dan V1. Zie de volgende artikelen voor een lijst met ondersteunde gegevensarchieven:

- [V1: ondersteunde gegevensarchieven](v1/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
- [V2: ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats)

## <a name="support-for-on-demand-spark-cluster"></a>Ondersteuning voor Apache Spark-cluster op aanvraag
V2 ondersteunt het maken van een HDInsight Apache Spark-cluster op aanvraag. Als u een Apache Spark-cluster op aanvraag wilt maken, geeft u het clustertype op als Apache Spark in uw definitie van uw gekoppelde HDInsight-service op aanvraag. Vervolgens configureert u de Apache Spark-activiteit in de pijplijn voor het gebruik van deze gekoppelde service. Tijdens runtime, als de activiteit wordt uitgevoerd, maakt de data factory-service automatisch de Apache Spark-cluster. Raadpleeg voor meer informatie de volgende artikelen:

- [Apache Spark-activiteit in V2](transform-data-using-spark.md)
- [Een gekoppelde Azure HDInsight-service op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service)

## <a name="custom-activities"></a>Aangepaste activiteiten
In V1 implementeert u (aangepaste) DotNet-activiteitscode door een .Net Framework-klassebibliotheekproject te maken met een klasse die de methode Execute van de IDotNetActivity-interface implementeert. Daarom moet de aangepaste code worden geschreven in .Framework 4.5.2 en worden uitgevoerd op Batch-poolknooppunten van Azure onder Windows. 

In aangepaste V2-activiteit bent u niet verplicht een .Net-interface te implementeren. U kunt nu rechtstreeks opdrachten en scripts uitvoeren en uw eigen, aangepaste code uitvoeren die als een uitvoerbaar bestand is gecompileerd. 

Zie [Verschil tussen aangepaste activiteit in V1 en V2](transform-data-using-dotnet-custom-activity.md#difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1) voor meer informatie.

## <a name="sdks"></a>SDK's
Versie 2 van Data Factory biedt een meer uitgebreide set SDK's, die kan worden gebruikt voor het ontwerpen, beheren en bewaken van pijplijnen.

- **.NET SDK**: de .NET SDK is bijgewerkt voor V2. 
- **PowerShell**: de PowerShell-cmdlets zijn bijgewerkt voor V2. De V2-cmdlets hebben **DataFactoryV2** in de naam. Bijvoorbeeld: Get-AzureRmDataFactoryV2. 
- **Python SDK**: deze SDK is nieuw in V2.
- **REST API**: de REST-API is bijgewerkt voor V2. 

De SDK's die zijn bijgewerkt voor V2 zijn niet achterwaarts compatibel met versie 1-clients. 

## <a name="authoring-experience"></a>Ontwerpen
Met Data Factory V1 kunt u pijplijnen ontwerpen met behulp van Data Factory Editor in Azure Portal. Data Factory V2 ondersteunt momenteel alleen programmatisch ontwerpen (.NET SDK, REST-API, PowerShell, Python, enzovoort) om data factory's te maken. Er bestaat nog geen gebruikersinterface.  Data Factory V1 ondersteunt ook ontwerp van SDK, REST en PowerShell.

## <a name="monitoring-experience"></a>Bewaken
In V2 kunt u ook data factory's bewaken met behulp van [Azure Monitor](monitor-using-azure-monitor.md). De nieuwe PowerShell-cmdlets bieden ondersteuning voor het bewaken van [integratie-runtimes](monitor-integration-runtime.md). Zowel V1 als V2 ondersteunen visueel bewaken via bewakingstoepassingen die vanuit Azure Portal kunnen worden gestart.


## <a name="next-steps"></a>Volgende stappen
In de volgende QuickStarts vindt u informatie over het maken van een data factory door het volgen van stapsgewijze instructies: [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md) en [REST-API](quickstart-create-data-factory-rest-api.md). 
