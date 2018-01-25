---
title: Integration Runtime in Azure Data Factory | Microsoft Docs
description: Meer informatie over Integration Runtime in Azure Data Factory.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: 5285cddcb64e44cb61c99cc18f8a66d60cd1c0fc
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="integration-runtime-in-azure-data-factory"></a>Integration Runtime in Azure Data Factory
De Integratie Runtime (IR) is de rekeninfrastructuur die Azure Data Factory gebruikt om de volgende mogelijkheden voor gegevensintegratie in verschillende netwerkomgevingen te bieden:

- **Gegevensverplaatsing**: gegevens verplaatsen tussen gegevensarchieven in openbare netwerken en gegevensopslag in een particulier netwerk (on-premises of virtueel particulier netwerk). Deze optie biedt ondersteuning voor ingebouwde connectors, indelingsconversie, kolomtoewijzing en hoogwaardige en schaalbare gegevensoverdracht.
- **Activiteitverzending**: transformatieactiviteiten die worden uitgevoerd op verschillende rekenservices zoals Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server en meer verzenden en controleren.
- **SSIS-pakketuitvoering**: systeemeigen SSIS-pakketten (SQL Server Integration Services) uitvoeren in een beheerde Azure-rekenomgeving.


> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

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

### <a name="network-environment"></a>Netwerkomgeving
Azure Integration Runtime ondersteunt verbindingen met gegevensarchieven en rekenservices in openbare netwerken met openbaar toegankelijke eindpunten. Gebruik een zelf-hostende Integration Runtime voor een Azure Virtual Network-omgeving.

### <a name="compute-resource-and-scaling"></a>Rekenresource en schalen
Azure Integration Runtime biedt een volledig beheerde, serverloze rekenresource in Azure.  U hoeft zich geen zorgen te maken over het inrichten van de infrastructuur, de software-installatie, patchen of capaciteitsschaling.  Bovendien betaalt u alleen voor het werkelijke gebruik.

Azure Integration Runtime biedt de systeemeigen rekenkracht om gegevens te verplaatsen tussen gegevensarchieven in de cloud op een veilige, betrouwbare en krachtige manier.  U kunt instellen hoeveel eenheden voor gegevensverplaatsing worden gebruikt in de kopieeractiviteit. De rekencapaciteit van de Azure IR wordt flexibel opgeschaald om aan uw behoeften te voldoen, zonder dat u de grootte van de Azure Integration Runtime expliciet hoeft aan te passen.

Verzending van de activiteit is een lichte bewerking om de activiteit naar de doelrekenservice te routeren. U hoeft de rekenkracht niet op te schalen voor dit scenario.

Zie voor informatie over het maken en configureren van een Azure-IR 'Azure IR maken en configureren' bij de gidsen. 

## <a name="self-hosted-integration-runtime"></a>Zelf-hostende Integration Runtime
Een zelf-hostende IR is geschikt voor:

- Het uitvoeren van kopieeractiviteit tussen een gegevensarchief in de cloud en een gegevensarchief in een privénetwerk.
- Het verzenden van de volgende transformatieactiviteiten via on-premises rekenresources of in Azure Virtual Network: HDInsight Hive-activiteit (BYOC), HDInsight Pig-activiteit (BYOC), HDInsight MapReduce-activiteit (BYOC), HDInsight Spark-activiteit (BYOC), HDInsight Streaming-activiteit (BYOC), Machine Learning Batch Execution-activiteit, Machine Learning Update Resource-activiteiten, Stored Procedure-activiteit, Data Lake Analytics U-SQL-activiteit, aangepaste .NET-activiteit, opzoekactiviteit en ophaalactiviteit voor metagegevens.

> [!NOTE] 
> Gebruik zelf-gehoste Integration Runtime om gegevensarchieven te ondersteunen waarvoor een eigen stuurprogramma zoals SAP Hana, MySQL, enz. is vereist.  Zie voor meer informatie [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).

### <a name="network-environment"></a>Netwerkomgeving
Als u gegevensintegratie veilig wilt uitvoeren in een privénetwerkomgeving zonder rechtstreekse zichtbaarheid vanuit de openbare cloudomgeving, kunt u een zelf-hostende on-premises IR installeren achter de firewall van het bedrijf of in een virtueel privénetwerk.  De zelf-hostende Integration Runtime maakt alleen uitgaande HTTP-gebaseerde verbindingen met het openbare internet.

### <a name="compute-resource-and-scaling"></a>Rekenresource en schalen
Zelf-hostende IR moet worden geïnstalleerd op een on-premises computer of op een virtuele machine in een privénetwerk. Op dit moment ondersteunen we alleen zelf-hostende IR op een Windows-besturingssysteem.  

Voor hoge beschikbaarheid en schaalbaarheid kunt u de zelf-hostende IR uitbreiden door het logische exemplaar te koppelen aan meerdere on-premises computers in de modus actief-actief.  Zie voor meer informatie het artikel 'Zelf-hostende IR maken en configureren' bij de gidsen.

## <a name="azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime
Als u de bestaande SSIS-werkbelasting wilt opheffen of verplaatsen, kunt u een Azure-SSIS IR maken voor het uitvoeren van systeemeigen SSIS-pakketten.

### <a name="network-environment"></a>Netwerkomgeving
Azure-SSIS IR kan worden ingericht in een openbaar netwerk of privénetwerk.  Toegang tot on-premises gegevens wordt ondersteund door Azure SSIS IR te koppelen aan een virtueel netwerk (VNet) dat is verbonden met uw on-premises netwerk.  

### <a name="compute-resource-and-scaling"></a>Rekenresource en schalen
Azure-SSIS IR is een volledig beheerd cluster met virtuele Azure-machines die uw SSIS-pakketten uitvoeren. U kunt uw eigen Azure SQL Database-server of een beheerd exemplaar van uw server (privépreview) gebruiken voor het hosten van de catalogus van SSIS-projecten/-pakketten (SSISDB) die u eraan wilt koppelen. U kunt de rekenkracht opschalen door de grootte van het knooppunt op te geven en opschalen door het aantal knooppunten in het cluster aan te geven. U kunt de kosten van het uitvoeren van de uw Azure-SSIS Integration Runtime beheren door naar wens te stoppen en starten.

Zie voor meer informatie het artikel 'Azure-SSIS IR maken en configureren' bij de gidsen.  Wanneer u de Azure SSIS IR hebt gemaakt, kunt u uw bestaande SSIS-pakketten implementeren en beheren met weinig of geen wijzigingen met behulp van bekende hulpprogramma's zoals SQL Server Data Tools (SSDT) en SQL Server Management Studio (SSMS), net als bij on-premises gebruik van SSIS.

Zie de volgende artikelen voor meer informatie over Azure-SSIS Runtime: 

- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-deploy-ssis-packages-azure.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Procedure: Een Azure SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). Dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Managed Instance (Private Preview) en het toevoegen van de IR aan een VNet. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Er wordt ook uitgelegd hoe u een Azure-SSIS IR kunt uitschalen door meer knooppunten toe te voegen aan de IR. 
- [Een Azure-SSIS IR toevoegen aan een VNet](join-azure-ssis-integration-runtime-virtual-network.md). Dit artikel bevat algemene informatie over het toevoegen van een Azure-SSIS IR aan een virtueel netwerk (VNet) van Azure. Er wordt ook beschreven hoe u Azure Portal gebruikt om VNet te configureren voor het deelnemen van een Azure-SSIS IR aan het VNet. 

## <a name="determining-which-ir-to-use"></a>Bepalen welke IR u moet gebruiken
Elke transformatieactiviteit heeft een gekoppelde doelrekenservice die naar een Integration Runtime verwijst. Vanuit dit exemplaar van de Integration Runtime wordt de transformatieactiviteit verzonden.

Voor de kopieeractiviteit, zijn de gekoppelde bron- en sinkservices vereist voor het definiëren van de richting van de gegevensstroom. De volgende logica wordt gebruikt om te bepalen welk exemplaar van Integration Runtime wordt gebruikt voor het uitvoeren van de kopieeractiviteit: 

- **Kopiëren tussen twee gegevensbronnen in de cloud**: wanneer zowel de gekoppelde bronservices als de sinkservices Azure IR gebruiken, wordt de Integration Runtime die wordt gebruikt door de gekoppelde sinkservice gebruikt voor het uitvoeren van de kopieeractiviteit.
- **Kopiëren tussen een gegevensbron in de cloud en een gegevensbron in een privénetwerk**: als de gekoppelde bron- of sinkservice verwijst naar een zelf-hostende IR, wordt de kopieerbewerking uitgevoerd op die zelf-hostende Integration Runtime.
- **Kopiëren tussen twee gegevensbronnen in een privénetwerk**: zowel de gekoppelde bronservice als de sinkservice moet verwijzen naar hetzelfde exemplaar van Integration Runtime, en deze wordt gebruikt voor het uitvoeren van de kopieeractiviteit.

In het volgende diagram ziet u twee voorbeelden voor de kopieeractiviteit:

- De bron van kopieeractiviteit 1 is een gekoppelde SQL Server-service die verwijst naar zelf-hostende IR A. De sink is een gekoppelde Azure Storage-service die verwijst naar Azure IR B. Wanneer de kopieeractiviteit wordt uitgevoerd, wordt deze uitgevoerd op de zelf-hostende IR A.
- De bron van kopieeractiviteit 2 is een gekoppelde Azure SQL Database-service die verwijst naar Azure IR C. De sink is een gekoppelde Azure Storage-service die verwijst naar Azure IR B. Wanneer de kopieeractiviteit wordt uitgevoerd, wordt deze uitgevoerd op Azure IR B, omdat deze Integration Runtime wordt gebruikt door de gekoppelde sinkservice.

![Welke IR u moet gebruiken](media/concepts-integration-runtime/which-integration-runtime-to-use.png)

## <a name="integration-runtime-location"></a>Locatie van Integration Runtime
De Data Factory-locatie is waar de metagegevens van de data factory worden opgeslagen en waar van het activeren van de pijplijn wordt gestart. De ondersteunde locaties voor Data Factory zijn momenteel: VS - oost, VS -oost 2 en West-Europa. Een gegevensfactory heeft echter wel toegang tot gegevensarchieven en Compute Services in andere Azure-regio’s om gegevens te verplaatsen tussen gegevensarchieven of om gegevens te verwerken middels Compute Services. Dit gedrag wordt gerealiseerd via de IR die algemeen beschikbaar is in meerdere regio's om de gegevensnaleving, efficiëntie en verminderde kosten voor uitgaand netwerkverkeer te realiseren.

De locatie van de IR definieert de locatie van de back-end rekenkracht en in wezen de locatie waar de verplaatsing van gegevens, het verzenden van activiteit en de uitvoering van het SSIS-pakket worden uitgevoerd. De locatie van de IR kan afwijken van de locatie van de data factory waar hij bij hoort. Het volgende diagram toont de locatie-instellingen van Data Factory en het aantal keren dat de integratie wordt uitgevoerd:

![Locatie van Integration Runtime](media/concepts-integration-runtime/integration-runtime-location.png)

### <a name="azure-ir"></a>Azure IR
Data Factory maakt gebruik van een Azure IR in de regio die zich het dichtst bij de sink in dezelfde geografie bevindt om de gegevens te verplaatsen. Raadpleeg de volgende tabel voor toewijzing:

Geografie van het gegevensarchief dat als sink fungeert | Locatie van het gegevensarchief dat als sink fungeert | Locatie die wordt gebruikt voor Azure Integration Runtime
-------------------------------| ----------------| ------------------
Verenigde Staten | VS - oost | VS - oost
&nbsp; | VS - oost 2 | VS - oost 2
&nbsp; | VS - midden | VS - midden
&nbsp; | Noord-centraal VS | Noord-centraal VS
&nbsp; | Zuid-centraal VS | Zuid-centraal VS
&nbsp; | West-centraal VS | West-centraal VS
&nbsp; | VS - west | VS - west
&nbsp; | VS - west 2 | VS - west 2
Canada | Canada - oost | Canada - midden
&nbsp; | Canada - midden | Canada - midden
Brazilië | Brazilië - zuid | Brazilië - zuid
Europa | Noord-Europa | Noord-Europa
&nbsp; | West-Europa | West-Europa
Verenigd Koninkrijk | Verenigd Koninkrijk West | Verenigd Koninkrijk Zuid
&nbsp; | Verenigd Koninkrijk Zuid | Verenigd Koninkrijk Zuid
Azië en Stille Oceaan | Zuidoost-Azië | Zuidoost-Azië
&nbsp; | Oost-Azië | Zuidoost-Azië
Australië | Australië - oost | Australië - oost
&nbsp; | Australië - zuidoost | Australië - zuidoost
Japan | Japan - oost | Japan - oost
&nbsp; | Japan - west | Japan - oost
Korea | Korea - centraal | Korea - centraal
&nbsp; | Korea - zuid | Korea - centraal
India | Centraal-India | Centraal-India
&nbsp; | West-India | Centraal-India
&nbsp; | Zuid-India | Centraal-India

U kunt ook de locatie van een Azure IR automatisch laten oplossen, wat betekent dat Data Factory automatisch probeert de best mogelijke locatie te vinden op basis van de definitie van de gekoppelde service.

> [!NOTE] 
> Als de regio van het doelgegevensarchief niet in de lijst staat of niet kan worden getraceerd, kan de activiteit niet worden uitgevoerd. Er wordt niet naar een andere regio overgeschakeld in verband met de naleving. In dit geval moet u expliciet een alternatieve locatie opgeven om te gebruiken voor het uitvoeren van de kopieeractiviteit.
 
De volgende afbeelding toont een voorbeeld van de effectieve locatie als de locatie van Azure IR is ingesteld op automatisch oplossen. Wanneer een kopieeractiviteit wordt uitgevoerd, wordt de locatie van de gegevensbestemming gedetecteerd. In dit voorbeeld is dit Japan - west.  Op basis van de tabel wordt een Azure IR in Japan - oost gebruikt voor het uitvoeren van de daadwerkelijke kopieeractiviteit. Wanneer dezelfde IR wordt gebruikt om verbinding te maken met HDInsight voor een Spark-activiteit, verloopt de verzending van de Spark-toepassing vanaf de locatie van Data Factory gebeurt. In dit voorbeeld is dat VS - oost. De daadwerkelijke uitvoering van de Spark-toepassing wordt uitgevoerd op de locatie van de HDInsight-server. 

![Effectieve locatie](media/concepts-integration-runtime/effective-location.png)

### <a name="self-hosted-ir"></a>Zelf-hostende IR
De zelf-hostende IR is logisch geregistreerd bij de Data Factory en de rekenkracht die wordt gebruikt ter ondersteuning van de functionaliteiten die u hebt opgegeven. Er bestaat daarom geen expliciete locatie-eigenschap voor de zelf-hostende IR. 

Wanneer de zelf-hostende IR wordt gebruikt voor het uitvoeren van de gegevensverplaatsing, haalt deze gegevens uit de bron en schrijft naar de bestemming.

### <a name="azure-ssis-ir"></a>Azure-SSIS IR
Het selecteren van de juiste locatie voor uw Azure-SSIS IR is essentieel voor het bereiken van hoge prestaties in uw ETL-werkstromen (extract-transform-load).  Er zijn twee locaties beschikbaar voor preview (VS - oost en Noord-Europa).

- De locatie van uw Azure-SSIS IR hoeft niet gelijk te zijn aan de locatie van uw data factory, maar moet wel gelijk zijn aan de locatie van uw eigen Azure SQL Database-/MI-server (privépreview) waarop SSISDB wordt gehost. Op deze manier heeft uw Azure-SSIS Integration Runtime eenvoudig toegang tot SSISDB, zonder overmatig verkeer tussen verschillende locaties.
- Als u geen bestaande Azure SQL Database-/MI-server (privépreview) hebt om SSISDB op te hosten, maar u on-premises gegevensbronnen/bestemmingen hebt, moet u een nieuwe Azure SQL Database-/MI-server (privépreview) maken op de locatie van een VNet dat is verbonden met uw on-premises netwerk.  Op deze manier kunt u uw Azure-SSIS IR maken met behulp van de nieuwe Azure SQL Database-/MI-server (privépreview) en deze koppelen aan het VNet op dezelfde locatie. Zo minimaliseert u het aantal verplaatsingen van gegevens op verschillende locaties.
- Als de locatie van de bestaande Azure SQL Database-/MI-server (privépreview) waarop de SSISDB wordt gehost niet hetzelfde is als de locatie van een VNet dat is verbonden met uw on-premises netwerk, moet u eerst uw Azure-SSIS IR maken met een bestaande Azure SQL Database-/MI-server (privépreview) en deze toevoegen aan een ander VNet op dezelfde locatie. Vervolgens configureert u een VNet-naar-VNet-verbinding tussen de verschillende locaties.


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Zelf-hostende integratie-runtime maken](create-self-hosted-integration-runtime.md)
- [Een Azure-SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). Dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Managed Instance (Private Preview) en het toevoegen van de IR aan een VNet. 
