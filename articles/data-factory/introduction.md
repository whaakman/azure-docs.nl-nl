---
title: Inleiding tot Azure Data Factory | Microsoft Docs
description: 'Leer wat Azure Data Factory is: een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd.'
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
ms.date: 09/29/2017
ms.author: shlo
ms.openlocfilehash: 58e141498ed5cbaa110622d949a9627c98827ac3
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="introduction-to-azure-data-factory"></a>Inleiding tot Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-introduction.md)
> * [Versie 2 - Preview](introduction.md)

In de wereld van big data worden onbewerkte, ongeordende gegevens vaak opgeslagen in relationele, niet-relationele en andere opslagsystemen. Op zichzelf staande, onbewerkte gegevens hebben echter niet de juiste context of betekenis om nuttige inzichten te kunnen bieden aan analisten, gegevenswetenschappers en besluitvormers. 

Voor big data is een service nodig die het organiseren en het operationeel maken van processen mogelijk maakt om deze enorme hoeveelheden onbewerkte gegevens om te zetten naar bruikbare zakelijke inzichten. Azure Data Factory is een beheerde cloudservice die speciaal is ontworpen voor deze complexe hybride ETL- (extract-transform-load), ELT- (extract-load-transform) en gegevensintegratieprojecten.

Denk bijvoorbeeld aan een gamingbedrijf dat petabytes aan gamelogboeken verzamelt die worden geproduceerd door games in de cloud. Het bedrijf wil deze logboeken analyseren om inzicht te verkrijgen in de voorkeuren, de demografische gegevens en het gebruiksgedrag van klanten. Het bedrijf wil ook kansen voor up-selling en cross-selling identificeren, en nieuwe aantrekkelijke functies ontwikkelen om groei te stimuleren en klanten een betere ervaring te bieden.

Voor het analyseren van deze logboeken moet het bedrijf gebruikmaken van de referentiegegevens, zoals klantgegevens, game-informatie en marketingcampagnegegevens, die zich in een on-premises gegevensarchief bevinden. Het bedrijf wil deze gegevens uit het on-premises gegevensarchief gebruiken door ze te combineren met extra logboekgegevens in een gegevensarchief in de cloud. 

Om inzichten te extraheren, wil het bedrijf de verzamelde gegevens verwerken met behulp van een Spark-cluster in de cloud (HDInsight) en de getransformeerde gegevens publiceren in een datawarehouse in de cloud, zoals Azure SQL Data Warehouse, om er gemakkelijk een rapport over te maken. Het bedrijf wil deze werkstroom automatiseren en volgens een dagelijks schema controleren en beheren. Ook wil het bedrijf de werkstroom uitvoeren wanneer bestanden in een blob store-container terechtkomen.

Azure Data Factory is het platform dat dergelijke gegevensscenario's oplost. Het is een *cloud-gebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud en zo gegevensverplaatsing en -transformatie kunt indelen en automatiseren*. Met behulp van Azure Data Factory kunt u gegevensgestuurde werkstromen (pijplijnen genoemd) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen. Azure Data Factory kan de gegevens verwerken en transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. 

Bovendien kunt u uitvoergegevens publiceren naar gegevensopslaglocaties zoals Azure SQL Data Warehouse, waar BI-toepassingen (business intelligence-oplossingen) er gebruik van kunnen maken. Via de Azure Data Factory kunt onbewerkte gegevens uiteindelijk worden ingedeeld in zinvolle gegevensarchieven en gegevensmeren zodat u betere zakelijke beslissingen kunt nemen.

![Weergave van het hoogste niveau van Data Factory](media/introduction/big-picture.png)

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Inleiding tot versie 1 van Data Factory](v1/data-factory-introduction.md).

## <a name="how-does-it-work"></a>Hoe werkt het?
De pijplijnen (gegevensgestuurde werkstromen) in Azure Data Factory voeren normaal gesproken de volgende vier stappen uit:

![Vier stappen van een gegevensgestuurde werkstroom](media/introduction/four-steps-of-a-workflow.png)

### <a name="connect-and-collect"></a>Verbinding maken en verzamelen

Bedrijven hebben gegevens van verschillende typen in verschillende on-premises bronnen, in de cloud, gestructureerd, niet-gestructureerd en semi-gestructureerd, die allemaal binnenkomen met verschillende tussenpozen en snelheden. 

De eerste stap voor het bouwen van een informatieproductiesysteem bestaat uit het verbinden van alle vereiste gegevens- en verwerkingsbronnen, zoals SaaS-services (Software-as-a-Service), databases, bestandsshares en FTP-webservices. De volgende stap bestaat uit het zo nodig verplaatsen van gegevens naar een centrale locatie voor verdere verwerking.

Zonder Data Factory moeten ondernemingen aangepaste onderdelen voor gegevensverplaatsing ontwikkelen of aangepaste services schrijven om deze gegevensbronnen en verwerking te integreren. Het is duur en moeilijk om dergelijke systemen te integreren en te onderhouden. Bovendien beschikken ze vaak niet over de functionaliteit voor bewaking, waarschuwing en besturingselementen op bedrijfsniveau die een volledig beheerde service kan bieden.

Met Data Factory kunt u de [kopieeractiviteit](copy-activity-overview.md) in een pijplijn gebruiken om gegevens van on-premises gegevensarchieven en gegevensarchieven uit de cloud te verplaatsen naar een gecentraliseerd gegevensarchief in de cloud voor verdere analyse. Zo kunt u bijvoorbeeld gegevens in Azure Data Lake Store verzamelen en later transformeren met behulp van een Azure Data Lake Analytics-rekenservice. U kunt ook gegevens verzamelen in Azure Blob Storage en later transformeren met behulp van een Hadoop-cluster van Azure HDInsight.

### <a name="transform-and-enrich"></a>Transformeren en verrijken
Wanneer de gegevens eenmaal aanwezig zijn in een gecentraliseerd gegevensarchief in de cloud, verwerkt of transformeert u de verzamelde gegevens met behulp van rekenservices zoals HDInsight Hadoop, Spark, Data Lake Analytics of Machine Learning. U wilt dat de gegevens op een betrouwbare manier en volgens een beheersbare en gecontroleerde planning worden geproduceerd om productieomgevingen te voorzien van vertrouwde gegevens.

### <a name="publish"></a>Publiceren
Nadat de onbewerkte gegevens zijn omgezet in gebruiksklare bedrijfsgegevens, laadt u de gegevens in Azure Data Warehouse, Azure SQL Database, Azure CosmosDB of een andere analyse-engine waar gebruikers in uw bedrijf naar kunnen verwijzen met hun hulpprogramma's voor business intelligence.

### <a name="monitor"></a>Bewaken
Nadat u uw pijplijn voor gegevensintegratie hebt gemaakt en geïmplementeerd, en op die manier toegevoegde waarde biedt met getransformeerde gegevens, controleert u hoe vaak de geplande activiteiten en pijplijnen slagen en mislukken. Azure Data Factory heeft ingebouwde ondersteuning voor pijplijnbewaking via Azure Monitor, API, PowerShell, Microsoft Operations Management Suite en statusvensters in Azure Portal.

## <a name="whats-different-in-version-2"></a>Wat is er anders in versie 2?
Versie 2 van Azure Data Factory is gebaseerd op de oorspronkelijke gegevensverplaatsings- en transformatieservice Azure Data Factory. De service is uitgebreid tot een bredere set van scenario's voor gegevensintegratie in de cloud. Azure Data Factory versie 2 heeft de volgende mogelijkheden:

- Controlestroom en schalen
- SQL Server Integration Services-pakketten (SSIS) implementeren en uitvoeren in Azure

Na de release van versie 1 merkten we dat de klanten complexe hybride gegevensintegratiescenario's moesten ontwerpen waarbij verplaatsing van gegevens en verwerking ervan in de cloud, on-premises en in VM's in de cloud nodig waren. Door deze vereisten ontstond de behoefte aan beveiligde virtuele netwerkomgevingen om gegevens in te verzenden en verwerken, en aan de mogelijkheid om op te schalen met verwerkingskracht op aanvraag.

We hebben gezien dat er een flexibele planning nodig is voor de ondersteuning van het laden van incrementele gegevens en door gebeurtenis geactiveerde uitvoeringen, zodra gegevenspijplijnen een belangrijk onderdeel van een strategie voor business analytics worden. Ten slotte is het, omdat deze bewerkingen steeds complexer worden, des te belangrijker dat de service de algemene werkstroomparadigma's zoals vertakkingen, lussen en voorwaardelijke verwerking kan ondersteunen.

Met versie 2 kunt u ook bestaande SSIS-pakketten naar de cloud migreren. U kunt SSIS inzetten als een Azure-service die wordt beheerd in ADF, met behulp van de nieuwe functie 'Integration Runtimes' (IR). Door de SSIS-IR in versie 2 beschikt u over de mogelijkheid om SSIS-pakketten in de cloud uit te voeren, te beheren, te controleren en te bouwen.

### <a name="control-flow-and-scale"></a>Controlestroom en schalen 
Data Factory heeft een nieuw flexibel gegevenspijplijnmodel dat niet langer is gekoppeld aan tijdseriegegevens ter ondersteuning van de diverse integratiestromen en -patronen in het moderne datawarehouse. Met deze versie kunt u modellen maken voor voorwaarden, vertakkingen maken in de controlestroom van een pijplijn en expliciet parameters doorgeven binnen en tussen deze stromen.

U hebt nu de vrijheid om modellen te maken voor alle stroomstijlen die vereist zijn voor de integratie van gegevens. Deze kunnen op aanvraag of herhaaldelijk op basis van een klokplanning worden verzonden. Dit zijn enkele veelvoorkomende stromen die nu mogelijk zijn geworden:   

- Controlestroom:
    - Activiteiten in een volgorde binnen een pijplijn koppelen
    - Activiteiten vertakken binnen een pijplijn
    - Parameters
        - Parameters kunnen worden gedefinieerd op pijplijnniveau en de argumenten kunnen worden doorgegeven tijdens het aanroepen van de pijplijn op aanvraag of vanuit een trigger.
        - Activiteiten kunnen de argumenten die zijn doorgegeven aan de pijplijn gebruiken.
    - Aangepaste status doorgeven
        - Activiteituitvoer zoals de status kan worden gebruikt door een volgende activiteit in de pijplijn.
    - Herhalende containers
        - For-each 
- Op triggers gebaseerde stromen
    - Pijplijnen kunnen op aanvraag of volgens wandklokplanningen worden geactiveerd.
- Deltastromen
    - Gebruik parameters en geef de bovengrens op voor deltakopieerbewerkingen tijdens het verplaatsen van dimensie of verwijzingstabellen in een relationele opslagplaats, on-premises of in de cloud, om gegevens in het meer te laden. 

Zie [Activiteiten vertakken en koppelen in een Data Factory-pijplijn](tutorial-control-flow.md) voor meer informatie.

### <a name="deploy-ssis-packages-to-azure"></a>SSIS-pakketten implementeren in Azure 
Als u uw SSIS-werkbelastingen wilt verplaatsen, kunt u een data factory in versie 2 maken en een Azure-SSIS Integration Runtime (IR) inrichten. Azure-SSIS IR is een volledig beheerd cluster met virtuele Azure-machines (knooppunten) die uw SSIS-pakketten uitvoeren in de cloud. Zie de zelfstudie [Pakketten van SQL Server Integration Services implementeren in Azure](tutorial-deploy-ssis-packages-azure.md) voor stapsgewijze instructies. 
 

### <a name="sdks"></a>SDK's
Als u een ervaren gebruiker bent en op zoek bent naar een programma-interface, dan is versie 2 ideaal voor u. Deze versie biedt een uitgebreide set SDK's die kunnen worden gebruikt voor het ontwerpen, beheren en controleren van pijplijnen met behulp van uw favoriete IDE.

- *.NET SDK*: de .NET SDK is bijgewerkt voor versie 2. 
- *PowerShell*: de PowerShell-cmdlets zijn bijgewerkt voor versie 2. De cmdlets voor versie 2 hebben **DataFactoryV2** in de naam. Bijvoorbeeld: Get-AzureRmDataFactoryV2. 
- *Python SDK*: deze SDK is nieuw in versie 2.
- *REST API*: de REST-API is bijgewerkt voor versie 2.  

De SDK's die zijn bijgewerkt voor versie 2 zijn niet achterwaarts compatibel met versie 1-clients. 

### <a name="monitoring"></a>Bewaking
In versie 2 kunnen data factory's momenteel alleen worden bewaakt met behulp van SDK's. De portal biedt nog geen ondersteuning voor bewaking van data factory's versie 2. 

## <a name="load-the-data-into-a-lake"></a>De gegevens in een meer laden
Data Factory heeft 30+ connectors waarmee u gegevens kunt laden vanuit hybride en heterogene omgevingen naar Azure. Zie [Prestatie- en afstemmingshandleiding](copy-activity-performance.md) voor de meest recente prestatieresultaten van interne testen en suggesties voor het afstemmen. 

Daarnaast hebben we onlangs hoge beschikbaarheid en schaalbaarheid ingeschakeld voor de zelfgehoste Integration Runtime die u in een particuliere netwerkomgeving installeert. Hiermee wordt voldaan aan de vereisten van zakelijke klanten van laag 1 voor betere beschikbaarheid en schaalbaarheid.

## <a name="top-level-concepts-in-version-2"></a>Concepten op het hoogste niveau concepten in versie 2
Een Azure-abonnement kan een of meer Azure Data Factory-exemplaren (ofwel 'data factory's') hebben. Azure Data Factory bestaat uit vier hoofdonderdelen. Deze onderdelen werken samen om een platform te bieden waarop u gegevensgestuurde werkstromen kunt maken met stappen voor de verplaatsing en transformatie van gegevens.

### <a name="pipeline"></a>Pijplijn
Een data factory kan één of meer pijplijnen hebben. Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. De activiteiten in een pijplijn voeren samen een taak uit. Zo kan een pijplijn bijvoorbeeld een groep activiteiten bevatten die gegevens van een Azure-blob opnemen en vervolgens een Hive-query uitvoeren op een HDInsight-cluster om de gegevens te partitioneren. 

Het voordeel van een pijplijn is dat u de activiteiten kunt beheren als een groep in plaats van afzonderlijke activiteiten te beheren. De activiteiten in een pijplijn kunnen in een keten worden samengevoegd om na elkaar of onafhankelijk en parallel te worden uitgevoerd.

### <a name="activity"></a>Activiteit
Activiteiten vertegenwoordigen een verwerkingsstap in een pijplijn. U kunt bijvoorbeeld een kopieeractiviteit gebruiken om gegevens van één gegevensarchief naar een ander te kopiëren. U kunt ook een Hive-activiteit gebruiken, waarmee een Hive-query wordt uitgevoerd voor een Azure HDInsight-cluster om uw gegevens te transformeren of analyseren. Data Factory ondersteunt drie soorten activiteiten: activiteiten voor gegevensverplaatsing, activiteiten voor gegevenstransformatie en controleactiviteiten.

### <a name="datasets"></a>Gegevenssets
Gegevenssets vertegenwoordigen gegevensstructuren in de gegevensarchieven die simpelweg verwijzen naar de gegevens die u in uw activiteiten als in- of uitvoer wilt gebruiken. 

### <a name="linked-services"></a>Gekoppelde services
Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Als u het op deze manier bekijkt, vertegenwoordigt de gegevensset in feite de structuur van de gegevens en definieert de gekoppelde service de verbinding met de gegevensbron. Een met Azure Storage gekoppelde service geeft bijvoorbeeld een verbindingsreeks zodat deze verbinding kan maken met het Azure Storage-account. Ook geeft een Azure Blob-gegevenssets de blobcontainer op, en de map met de gegevens.

Gekoppelde services worden voor twee doeleinden gebruikt in een Data Factory:

- Als vertegenwoordiging van een **gegevensarchief**, zoals een on-premises SQL Server-database, een Oracle-database, een bestandsshare of een Azure Blob Storage-account. Zie het artikel [kopieeractiviteit](copy-activity-overview.md) voor een lijst met ondersteunde gegevensarchieven.

- Ter vertegenwoordiging van een **rekenresource** die de uitvoering van een activiteit kan hosten. De activiteit HDInsightHive wordt bijvoorbeeld uitgevoerd in een HDInsight Hadoop-cluster. Zie het artikel [Gegevens transformeren](transform-data.md) voor een lijst met transformatieactiviteiten en ondersteunde rekenomgevingen.

### <a name="triggers"></a>Triggers
Triggers zijn verwerkingseenheden die bepalen wanneer een pijplijnuitvoering moet worden gestart. Er zijn verschillende soorten triggers voor verschillende soorten gebeurtenissen. Voor de preview ondersteunen wij de trigger voor de kloktijd. 


### <a name="pipeline-runs"></a>Pijplijnuitvoeringen
Een pijplijnuitvoering is een exemplaar van de uitvoering van de pijplijn. Pijplijnuitvoeringen worden doorgaans geïnstantieerd doordat argumenten worden doorgegeven aan de parameters die zijn gedefinieerd in pijplijnen. De argumenten kunnen handmatig of in de triggerdefinitie worden doorgegeven.

### <a name="parameters"></a>Parameters
Parameters zijn sleutel-waardeparen van een alleen-lezen-configuratie.  Parameters worden gedefinieerd in de pijplijn. De argumenten voor de gedefinieerde parameters worden doorgegeven tijdens het uitvoeren van de uitvoeringscontext, die wordt gemaakt door een trigger of een pijplijn die handmatig wordt uitgevoerd. Activiteiten binnen de pijplijn gebruiken de parameterwaarden.

Een gegevensset is een algemeen type parameter en een herbruikbare entiteit waarnaar kan worden verwezen. Een activiteit kan verwijzen naar gegevenssets en kan de eigenschappen die zijn gedefinieerd in de definitie van de gegevensset gebruiken.

Een gekoppelde service is ook een algemeen type parameter met de verbindingsgegevens voor een gegevensarchief of een rekenomgeving. Deze is ook een herbruikbare entiteit waarnaar kan worden verwezen.

### <a name="control-flow"></a>Controlestroom
De controlestroom is een indeling van activiteiten van de pijplijn, waaronder het koppelen van activiteiten in een reeks, maken van vertakkingen en definiëren van parameters op het niveau van de pijplijn. Argumenten worden doorgegeven tijdens het aanroepen van de pijplijn op aanvraag of vanuit een trigger. Deze omvat ook het doorgeven van een aangepaste status en luscontainers, ofwel For-each-iterators.


Zie de volgende artikelen voor meer informatie over Data Factory-concepten:

- [Gegevenssets en gekoppelde services](concepts-datasets-linked-services.md)
- [Pijplijnen en activiteiten](concepts-pipelines-activities.md)
- [Integration Runtime](concepts-integration-runtime.md)

## <a name="supported-regions"></a>Ondersteunde regio’s

U kunt op dit moment gegevensfactory’s maken in de regio’s VS - oost en VS - oost 2. Een gegevensfactory heeft echter wel toegang tot gegevensarchieven en Compute Services in andere Azure-regio’s om gegevens te verplaatsen tussen gegevensarchieven of om gegevens te verwerken middels Compute Services.

Azure Data Factory zelf slaat geen gegevens op. U kunt er gegevensgestuurde werkstromen mee maken om de verplaatsing van gegevens te beheren tussen ondersteunde gegevensarchieven en om er gegevens mee te verwerken middels Compute Services in andere regio's of in een on-premises omgeving. U kunt er ook werkstromen mee bewaken en beheren met zowel programmatische als gebruikersinterfacemechanismen.

Hoewel Data Factory alleen beschikbaar is in VS - oost en VS - oost 2, is de service die gegevensverplaatsing in Data Factory mogelijk maakt, wereldwijd beschikbaar in meerdere regio’s. Als een gegevensarchief zich achter een firewall bevindt, worden de gegevens verplaatst middels een gegevensbeheergateway die is geïnstalleerd in uw on-premises omgeving.

Voorbeeld: uw berekeningsomgevingen, zoals een Azure HDInsight-cluster en Azure Machine Learning, worden uitgevoerd in de regio West-Europa. U kunt een Azure Data Factory-exemplaar maken en gebruiken in Noord-Europa en dit gebruiken om taken te plannen in uw berekeningsomgevingen in West-Europa. Het duurt enkele milliseconden voordat Data Factory de taak in uw rekenomgeving activeert, maar de uitvoertijd van de taak verandert niet in uw rekenomgeving.

## <a name="next-steps"></a>Volgende stappen
In de volgende snelstartgidsen vindt u informatie over het maken van een data factory door het volgen van stapsgewijze instructies: [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST-API](quickstart-create-data-factory-rest-api.md) en Azure Portal. 
