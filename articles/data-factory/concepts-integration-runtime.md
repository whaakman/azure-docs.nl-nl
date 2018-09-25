---
title: Integration Runtime in Azure Data Factory | Microsoft Docs
description: Meer informatie over Integration Runtime in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: jingwang
ms.openlocfilehash: dafbfb959e70563f8619f7aea877a3aa1c380453
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997400"
---
# <a name="integration-runtime-in-azure-data-factory"></a>Integration Runtime in Azure Data Factory
De Integration Runtime (IR) is de rekeninfrastructuur die Azure Data Factory gebruikt om de volgende mogelijkheden voor gegevensintegratie in verschillende netwerkomgevingen te bieden:

- **Gegevensverplaatsing**: gegevens verplaatsen tussen gegevensarchieven in openbare netwerken en gegevensopslag in een particulier netwerk (on-premises of virtueel particulier netwerk). Deze optie biedt ondersteuning voor ingebouwde connectors, indelingsconversie, kolomtoewijzing en hoogwaardige en schaalbare gegevensoverdracht.
- **Activiteitverzending**: transformatieactiviteiten die worden uitgevoerd op verschillende rekenservices zoals Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server en meer verzenden en controleren.
- **SSIS-pakketuitvoering**: systeemeigen SSIS-pakketten (SQL Server Integration Services) uitvoeren in een beheerde Azure-rekenomgeving.

In de Data Factory definieert een activiteit de actie die moet worden uitgevoerd. Een gekoppelde service definieert een doelgegevensarchief of een rekenservice. Een Integration Runtime vormt de brug tussen de activiteit en de gekoppelde services.  Er wordt naar verwezen door de gekoppelde service en biedt de rekenomgeving waarop de activiteit wordt uitgevoerd of waaruit deze wordt opgehaald.  Op deze manier kan de activiteit optimaal worden uitgevoerd in de regio die het dichtst mogelijk bij het doelgegevensarchief of de rekenservice ligt, terwijl wordt voldaan aan vereisten rondom beveiliging en naleving.

## <a name="integration-runtime-types"></a>Typen Integration Runtime
Data Factory biedt drie typen Integration Runtime. Kies het type dat het beste aansluit op de mogelijkheden voor de gegevensintegratie en de behoeften op het gebied van de netwerkomgeving.  Deze drie typen zijn:

- Azure
- Zelf-hostend
- Azure-SSIS

De volgende tabel beschrijft de mogelijkheden en de netwerkondersteuning voor de drie typen Integration Runtime:

IR-type | Openbaar netwerk | Privénetwerk
------- | -------------- | ---------------
Azure | Gegevensverplaatsing<br/>Verzending van de activiteit | &nbsp;
Zelf-hostend | Gegevensverplaatsing<br/>Verzending van de activiteit | Gegevensverplaatsing<br/>Verzending van de activiteit
Azure-SSIS | Uitvoering van SSIS-pakket | Uitvoering van SSIS-pakket

Het volgende diagram toont hoe verschillende typen Integration Runtime gecombineerd kunnen worden gebruikt om geavanceerde mogelijkheden voor gegevensintegratie en netwerkondersteuning te bieden:

![Verschillende typen Integration Runtime](media\concepts-integration-runtime\different-integration-runtimes.png)

## <a name="azure-integration-runtime"></a>Azure Integration Runtime
Een Azure Integration Runtime is geschikt voor:

- Het uitvoeren van kopieeractiviteit tussen gegevensarchieven in de cloud
- Het verzenden van de volgende transformatieactiviteiten in openbare netwerken: HDInsight Hive-activiteit, HDInsight Pig-activiteit, HDInsight MapReduce-activiteit, HDInsight Spark-activiteit, HDInsight Streaming-activiteit, Machine Learning Batch Execution-activiteit, Machine Learning Update Resource-activiteiten, Stored Procedure-activiteit, Data Lake Analytics U-SQL-activiteit, aangepaste .NET-activiteit, webactiviteit, opzoekactiviteit en ophaalactiviteit voor metagegevens.

### <a name="azure-ir-network-environment"></a>Azure IR-netwerkomgeving
Azure Integration Runtime ondersteunt verbindingen met gegevensarchieven en rekenservices in openbare netwerken met openbaar toegankelijke eindpunten. Gebruik een zelf-hostende Integration Runtime voor een Azure Virtual Network-omgeving.

### <a name="azure-ir-compute-resource-and-scaling"></a>Azure IR-rekenresource en -schalen
Azure Integration Runtime biedt een volledig beheerde, serverloze rekenresource in Azure.  U hoeft zich geen zorgen te maken over het inrichten van de infrastructuur, de software-installatie, patchen of capaciteitsschaling.  Bovendien betaalt u alleen voor het werkelijke gebruik.

Azure Integration Runtime biedt de systeemeigen rekenkracht om gegevens te verplaatsen tussen gegevensarchieven in de cloud op een veilige, betrouwbare en krachtige manier.  U kunt instellen hoeveel eenheden voor gegevensintegratie worden gebruikt in de kopieeractiviteit. De rekencapaciteit van de Azure IR wordt flexibel opgeschaald om aan uw behoeften te voldoen, zonder dat u de grootte van de Azure Integration Runtime expliciet hoeft aan te passen.

Verzending van de activiteit is een lichte bewerking om de activiteit naar de doelrekenservice te routeren. U hoeft de rekenkracht niet op te schalen voor dit scenario.

Zie voor informatie over het maken en configureren van een Azure-IR 'Azure IR maken en configureren' bij de gidsen. 

## <a name="self-hosted-integration-runtime"></a>Zelf-hostende Integration Runtime
Een zelf-hostende IR is geschikt voor:

- Het uitvoeren van kopieeractiviteit tussen een gegevensarchief in de cloud en een gegevensarchief in een privénetwerk.
- Het verzenden van de volgende transformatieactiviteiten via on-premises rekenresources of in Azure Virtual Network: HDInsight Hive-activiteit (BYOC), HDInsight Pig-activiteit (BYOC), HDInsight MapReduce-activiteit (BYOC), HDInsight Spark-activiteit (BYOC), HDInsight Streaming-activiteit (BYOC), Machine Learning Batch Execution-activiteit, Machine Learning Update Resource-activiteiten, Stored Procedure-activiteit, Data Lake Analytics U-SQL-activiteit, aangepaste .NET-activiteit, opzoekactiviteit en ophaalactiviteit voor metagegevens.

> [!NOTE] 
> Gebruik zelf-gehoste Integration Runtime om gegevensarchieven te ondersteunen waarvoor een eigen stuurprogramma zoals SAP Hana, MySQL, enz. is vereist.  Zie voor meer informatie [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).

### <a name="self-hosted-ir-network-environment"></a>Zelf-hostende IR-netwerkomgeving
Als u gegevensintegratie veilig wilt uitvoeren in een privénetwerkomgeving zonder rechtstreekse zichtbaarheid vanuit de openbare cloudomgeving, kunt u een zelf-hostende on-premises IR installeren achter de firewall van het bedrijf of in een virtueel privénetwerk.  De zelf-hostende Integration Runtime maakt alleen uitgaande HTTP-gebaseerde verbindingen met het openbare internet.

### <a name="self-hosted-ir-compute-resource-and-scaling"></a>Zelf-hostende IR-rekenresource en -schalen
Zelf-hostende IR moet worden geïnstalleerd op een on-premises computer of op een virtuele machine in een privénetwerk. Op dit moment ondersteunen we alleen zelf-hostende IR op een Windows-besturingssysteem.  

Voor hoge beschikbaarheid en schaalbaarheid kunt u de zelf-hostende IR uitbreiden door het logische exemplaar te koppelen aan meerdere on-premises computers in de modus actief-actief.  Zie voor meer informatie het artikel 'Zelf-hostende IR maken en configureren' bij de gidsen.

## <a name="azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime
Als u de bestaande SSIS-werkbelasting wilt opheffen of verplaatsen, kunt u een Azure-SSIS IR maken voor het uitvoeren van systeemeigen SSIS-pakketten.

### <a name="azure-ssis-ir-network-environment"></a>Azure-SSIS IR-netwerkomgeving
Azure-SSIS IR kan worden ingericht in een openbaar netwerk of privénetwerk.  Toegang tot on-premises gegevens wordt ondersteund door Azure SSIS IR te koppelen aan een virtueel netwerk dat is verbonden met uw on-premises netwerk.  

### <a name="azure-ssis-ir-compute-resource-and-scaling"></a>Azure-SSIS IR-rekenresource en -schalen
Azure-SSIS IR is een volledig beheerd cluster met virtuele Azure-machines die uw SSIS-pakketten uitvoeren. U kunt uw eigen Azure SQL Database of Managed Instance-server voor het hosten van de catalogus van SSIS-projecten /-pakketten (SSISDB) die moet worden gekoppeld aan deze op te halen. U kunt de rekenkracht opschalen door de grootte van het knooppunt op te geven en opschalen door het aantal knooppunten in het cluster aan te geven. U kunt de kosten van het uitvoeren van de uw Azure-SSIS Integration Runtime beheren door naar wens te stoppen en starten.

Zie voor meer informatie het artikel 'Azure-SSIS IR maken en configureren' bij de gidsen.  Wanneer u de Azure SSIS IR hebt gemaakt, kunt u uw bestaande SSIS-pakketten implementeren en beheren met weinig of geen wijzigingen met behulp van bekende hulpprogramma's zoals SQL Server Data Tools (SSDT) en SQL Server Management Studio (SSMS), net als bij on-premises gebruik van SSIS.

Zie de volgende artikelen voor meer informatie over Azure-SSIS Runtime: 

- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Procedure: Een Azure SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). In dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Database Managed Instance en toevoegen van de IR aan een virtueel netwerk. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Er wordt ook uitgelegd hoe u een Azure-SSIS IR kunt uitschalen door meer knooppunten toe te voegen aan de IR. 
- [Een Azure-SSIS-integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md). Dit artikel bevat algemene informatie over het toevoegen van een Azure-SSIS IR aan een virtueel netwerk van Azure. Er wordt ook beschreven hoe u Azure Portal gebruikt om een virtueel netwerk te configureren voor het deelnemen van Azure-SSIS IR aan het virtueel netwerk. 

## <a name="integration-runtime-location"></a>Locatie van Integration Runtime
De Data Factory-locatie is waar de metagegevens van de data factory worden opgeslagen en waar van het activeren van de pijplijn wordt gestart. Ondertussen heeft een data factory wel toegang tot gegevensarchieven en Compute Services in andere Azure-regio’s om gegevens te verplaatsen tussen gegevensarchieven of om gegevens te verwerken middels Compute Services. Dit gedrag wordt gerealiseerd via de [IR die algemeen beschikbaar](https://azure.microsoft.com/global-infrastructure/services/) is om de gegevensnaleving, efficiëntie en verminderde kosten voor uitgaand netwerkverkeer te realiseren.

De locatie van de IR definieert de locatie van de back-endrekenkracht en in wezen de locatie waar de gegevensverplaatsing, het verzenden van activiteit en de uitvoering van het SSIS-pakket worden uitgevoerd. De locatie van de IR kan afwijken van de locatie van de data factory waar hij bij hoort. 

### <a name="azure-ir-location"></a>Locatie van Azure IR
U kunt een bepaalde locatie van een Azure IR instellen, in welk geval de gegevensverplaatsing of verzendactiviteit in die specifieke regio plaatsvindt. 

Als u ervoor kiest de Azure IR voor automatisch oplossen te gebruiken (de standaardwaarde), gebeurt het volgende, 

- Voor kopieeractiviteit probeert ADF om automatisch uw sink- en brongegevensopslag te detecteren om de beste locatie te vinden in dezelfde regio, indien beschikbaar, of in de dichtstbijzijnde regio in dezelfde geografie. Indien niet detecteerbaar, wordt de regio van de data factory gebruikt.
- Voor de uitvoering van de activiteit Lookup/GetMetadata en verzending van transformatie-activiteiten gebruikt ADF de IR in de regio van de data factory.

U kunt controleren welke IR-locatie van kracht wordt tijdens het uitvoeren van activiteiten in de weergave voor het controleren van de pijplijnactiviteit in de gebruikersinterface of nettolading voor het controleren van activiteiten.

>[!TIP]
>Als u strikte gegevensnalevingsvereisten hebt en u ervoor moet zorgen moet dat gegevens een bepaalde geografie niet verlaten, kunt u expliciet een Azure IR maken in een bepaalde regio en de gekoppelde service naar deze IR laten wijzen met behulp van de eigenschap ConnectVia. Als u bijvoorbeeld gegevens uit Blob in UK - zuid naar SQL DW in UK - zuid wilt kopiëren en u ervoor wilt zorgen dat de gegevens het UK niet verlaten, maakt u een Azure IR en koppelt u beide gekoppelde services aan deze IR.

### <a name="self-hosted-ir-location"></a>Locatie zelf-hostende IR
De zelf-hostende IR is logisch geregistreerd bij de Data Factory en de rekenkracht die wordt gebruikt ter ondersteuning van de functionaliteiten die u hebt opgegeven. Er bestaat daarom geen expliciete locatie-eigenschap voor de zelf-hostende IR. 

Wanneer de zelf-hostende IR wordt gebruikt voor het uitvoeren van de gegevensverplaatsing, haalt deze gegevens uit de bron en schrijft naar de bestemming.

### <a name="azure-ssis-ir-location"></a>Locatie Azure-SSIS IR
Het selecteren van de juiste locatie voor uw Azure-SSIS IR is essentieel voor het bereiken van hoge prestaties in uw ETL-werkstromen (extract-transform-load).

- De locatie van uw Azure-SSIS IR hoeft niet gelijk zijn aan de locatie van uw data factory, maar deze moet gelijk zijn aan de locatie van uw eigen Azure SQL Database/MI-server waarop de SSISDB wordt gehost. Op deze manier heeft uw Azure-SSIS Integration Runtime eenvoudig toegang tot SSISDB, zonder overmatig verkeer tussen verschillende locaties.
- Als u geen een bestaande Azure SQL Database/MI-server host SSISDB, maar u on-premises gegevensbronnen/bestemmingen hebt, moet u een nieuwe Azure SQL Database/MI-server maken op de locatie van een virtueel netwerk is verbonden met uw on-premises netwerk.  Op deze manier kunt u uw Azure-SSIS-IR effectief minimaliseren verplaatsingen van gegevens op verschillende locaties met behulp van de nieuwe Azure SQL Database/MI-server en deze koppelen aan dit virtuele netwerk, op dezelfde locatie, maken.
- Als de locatie van uw bestaande Azure SQL Database/MI-server waarop de SSISDB wordt gehost niet hetzelfde zijn als de locatie van een virtueel netwerk is verbonden met uw on-premises netwerk, maakt u eerst uw Azure-SSIS-IR met een bestaande Azure SQL-Database / Beheerd exemplaar van server en deze koppelen aan een ander virtueel netwerk op dezelfde locatie en configureer vervolgens een virtueel netwerk met virtuele netwerkverbinding tussen verschillende locaties.

Het volgende diagram toont de locatie-instellingen van Data Factory en het aantal keren dat de integratie wordt uitgevoerd:

![Locatie van Integration Runtime](media/concepts-integration-runtime/integration-runtime-location.png)

## <a name="determining-which-ir-to-use"></a>Bepalen welke IR u moet gebruiken

### <a name="copy-activity"></a>Kopieeractiviteit

Voor de kopieeractiviteit, zijn de gekoppelde bron- en sinkservices vereist voor het definiëren van de richting van de gegevensstroom. De volgende logica wordt gebruikt om te bepalen welk exemplaar van Integration Runtime wordt gebruikt voor het uitvoeren van de kopieeractiviteit: 

- **Kopiëren tussen twee gegevensbronnen in de cloud:** : wanneer zowel de gekoppelde bronservices als de sinkservices Azure IR gebruiken, gebruikt ADF de regionale Azure IR die u hebt opgegeven of wordt automatisch een locatie van Azure bepaald als u ervoor kiest de IR voor automatisch oplossen (standaardwaarde) te gebruiken, zoals beschreven in de sectie [Locatie van Integration Runtime](#integration-runtime-location).
- **Kopiëren tussen een gegevensbron in de cloud en een gegevensbron in een privénetwerk**: als de gekoppelde bron- of sinkservice verwijst naar een zelf-hostende IR, wordt de kopieerbewerking uitgevoerd op die zelf-hostende Integration Runtime.
- **Kopiëren tussen twee gegevensbronnen in een privénetwerk**: zowel de gekoppelde bronservice als de sinkservice moet verwijzen naar hetzelfde exemplaar van Integration Runtime, en deze wordt gebruikt voor het uitvoeren van de kopieeractiviteit.

### <a name="lookup-and-getmetadata-activity"></a>Activiteit Lookup en GetMetadata

De activiteit Lookup en GetMetadata wordt uitgevoerd voor de integratieruntime die is gekoppeld aan de service die aan de gegevensopslag is gekoppeld.

### <a name="transformation-activity"></a>Transformatieactiviteit

Elke transformatieactiviteit heeft een gekoppelde doelrekenservice die naar een Integration Runtime verwijst. Vanuit dit exemplaar van de Integration Runtime wordt de transformatieactiviteit verzonden.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Zelf-hostende integratie-runtime maken](create-self-hosted-integration-runtime.md)
- [Een Azure-SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). In dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Database Managed Instance en toevoegen van de IR aan een virtueel netwerk. 
