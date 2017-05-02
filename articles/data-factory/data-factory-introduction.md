---
title: Inleiding tot Data Factory, een gegevensintegratieservice | Microsoft Docs
description: 'Leer wat een Data Factory is: een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd.'
keywords: gegevensintegratie, cloudgegevensintegratie, wat is Azure Data Factory
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/21/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 260208e7c7a08110eb3c885ef86ec4c18ff42fc9
ms.openlocfilehash: 40552b5d3cea5b04826c08e7b4b1d046a9fcefba
ms.lasthandoff: 04/23/2017


---
# <a name="introduction-to-azure-data-factory-service-a-data-integration-service-in-the-cloud"></a>Inleiding tot de Azure Data Factory-service, een service voor gegevensintegratie in de cloud
## <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory?
Hoe worden zakelijke gegevens in de wereld van big data hergebruikt? Is het mogelijk om gegevens die in de cloud zijn gegenereerd, waardevoller te maken met behulp van referentiegegevens uit on-premises gegevensbronnen of andere ongelijksoortige gegevensbronnen? Wat u dus nodig hebt, is een platform waarop u gegevens uit een groot aantal verschillende gegevensbronnen kunt samenvoegen en verwerken. Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de **verplaatsing** en **transformatie** van gegevens wordt beheerd en geautomatiseerd. U kunt oplossingen voor gegevensintegratie maken die invoergegevens uit verschillende gegevensarchieven opnemen, deze gegevens transformeren en verwerken, en de uitvoergegevens vervolgens publiceren naar andere gegevensarchieven. 

![Diagram: Data Factory-overzicht, een service voor gegevensintegratie](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Afbeelding1.** Neem gegevens op van verschillende gegevensbronnen en bereid ze voor, transformeer en analyseer ze en publiceer daarna kant-en-klare gegevens die geschikt zijn voor gebruik.


## <a name="what-does-it-offer"></a>Wat wordt er aangeboden? 
Traditioneel hebben gegevensintegratieprojecten altijd gedraaid rondom het maken van ETL-processen (Extract-Transform-Load) die gegevens ophalen uit verschillende gegevensbronnen binnen een organisatie, deze gegevens transformeren, zodat ze voldoen aan het doelschema van een EDW (Enterprise Data Warehouse), en de gegevens ten slotte laden in een EDW, zoals wordt weergegeven in de volgende afbeelding. Dit EDW wordt vervolgens geopend als één betrouwbare bron voor BI-analyseoplossingen.

![Traditionele ETL](media/data-factory-introduction/traditional-etl.png)
**Traditionele ETL**

Het gegevenslandschap voor ondernemingen blijft exponentieel toenemen in omvang, diversiteit en complexiteit, zoals in de volgende afbeelding wordt weergegeven. Het is diverser dan ooit, met on-premises en cloudgegevens in verschillende vormen en snelheden. Gegevensverwerking moet plaatsvinden op verschillende geografische locaties en omvat een combinatie van open-source software, commerciële oplossingen en aangepaste verwerkingsservices, die soms duur zijn en moeilijk te integreren en te onderhouden. De flexibiliteit die nodig is om bij te blijven in het snel veranderende big data-landschap, heeft geleid tot een combinatie van het traditionele EDW met de functionaliteit die vereist is in een modern informatieproductiesysteem. Azure Data Factory is een overkoepelend platform waarin zowel met traditionele EDW's als met innovatieve gegevensgerelateerde functies kan worden gewerkt. Zo krijgen ondernemingen de mogelijkheid om alle beschikbare gegevens in te zetten voor gegevensgestuurde besluitvorming.

![Nieuw big data landschap](media/data-factory-introduction/new-big-data-landscape.png)
**Nieuwe big data landschap**

De service Azure Data Factory biedt ondernemingen de mogelijkheid om deze diversiteit aan te wenden door een platform te bieden waarop ze **services voor gegevensverwerking, -opslag en -verkeer kunnen bundelen in informatieproductiepijplijnen** en vertrouwde gegevensassets kunnen beheren.

Met de Azure Data Factory-service kunt u:
- **Eenvoudig met diverse gegevensopslag- en -verwerkingssystemen werken**. 

    Ervoor zorgen dat ondernemingen met gegevens kunnen werken van verschillende typen en opgeslagen op verschillende bronnen. De eerste stap voor het bouwen van een informatieproductiesysteem bestaat uit het verbinden van alle vereiste gegevens- en verwerkingsbronnen, zoals SaaS-services, bestandsshares, FTP en webservices, en het zo nodig verplaatsen van gegevens naar een centrale locatie voor verdere verwerking.

    Zonder Data Factory moeten ondernemingen aangepaste onderdelen voor gegevensverplaatsing ontwikkelen of aangepaste services schrijven om deze gegevensbronnen en verwerking te integreren. Het is duur en lastig om dergelijke systemen te integreren en te onderhouden, en vaak ontbreken de adequate bewakingssystemen, waarschuwingssystemen en besturingselementen die een volledig beheerde service wel kan bieden.

    Met Data Factory kunt u de kopieeractiviteit in een pijplijn gebruiken om gegevens van on-premises gegevensarchieven en gegevensarchieven uit de cloud te verplaatsen naar een gecentraliseerd gegevensarchief in de cloud voor verdere analyse. Zo kunt u bijvoorbeeld gegevens in Azure Data Lake Store verzamelen en later transformeren met behulp van een Azure Data Lake Analytics Compute-service. Of u kunt gegevens verzamelen in Azure Blob Storage en later transformeren met behulp van een Hadoop-cluster van Azure HDInsight.
- **Gegevens transformeren tot vertrouwde informatie**. 

    Als de gegevens eenmaal in een gecentraliseerd gegevensarchief staan, kunt u gegevenspijplijnen maken en implementeren om de getransformeerde gegevens op betrouwbare wijze en volgens een hanteerbaar en gecontroleerd schema te produceren, en uw productieomgevingen te voorzien van betrouwbare gegevens. Gegevenstransformatie in Azure Data Factory wordt uitgevoerd door transformatieactiviteiten, zoals Hive, Pig, MapReduce, Azure Machine Learning-batchuitvoering en aangepaste C#-activiteiten, uitgevoerd op Azure HDInsight Hadoop-clusters, Azure Machine Learning-VM's of een Azure Batch-groep van virtuele machines.
- **Gegevenspijplijnen op één plaats bewaken**.

    Bij een gevarieerde gegevensportfolio is het belangrijk dat u beschikt over een betrouwbaar en volledig beeld van uw services voor opslag, verwerking en gegevensverplaatsing. Met Data Factory kunt u snel de status van de volledige gegevenspijplijn vaststellen, problemen identificeren en zo nodig actie ondernemen. Gebruik visuele weergaven om de afkomst van gegevens en de relatie tussen gegevens van de verschillende gegevensbronnen inzichtelijk te maken. Bekijk een compleet historisch overzicht van taakuitvoering, systeemstatus en afhankelijkheden vanuit één controledashboard.

## <a name="how-does-it-work"></a>Hoe werkt het? 
Azure Data Factory wordt gekenmerkt door drie informatieproductiefasen:

![Drie informatieproductiefasen](media/data-factory-introduction/three-information-production-stages.png)

- **Verbinding maken en verzamelen**: in deze fase worden gegevens uit diverse bronnen op één plek verzameld.
- **Transformeren en verrijken**: in deze fase worden de verzamelde gegevens verwerkt of getransformeerd.
- **Publiceren**. In deze fase worden de gegevens zo gepubliceerd dat ze kunnen worden gebruikt door BI-hulpprogramma's, analyseprogramma's en andere toepassingen.

## <a name="key-components"></a>Belangrijkste onderdelen
Een Azure-abonnement kan een of meer Azure Data Factory-exemplaren (oftewel 'data factory's') hebben. Azure Data Factory bestaat uit vier hoofdonderdelen die samenwerken om een platform te bieden waarop u voor uw gegevensstroom eenvoudige tot complexe indelingen voor gegevenstransformatie en -verplaatsing kunt samenstellen.

### <a name="activity"></a>Activiteit
Met activiteiten definieert u welk acties moeten worden uitgevoerd voor uw gegevens. U kunt bijvoorbeeld een kopieeractiviteit gebruiken om gegevens van één gegevensarchief naar een andere te kopiëren. U kunt ook een Hive-activiteit gebruiken, waarmee een Hive-query wordt uitgevoerd voor een Azure HDInsight-cluster om uw gegevens te transformeren of analyseren. Data Factory ondersteunt twee soorten activiteiten: activiteiten voor gegevensverplaatsing en activiteiten voor gegevenstransformatie.

Elke activiteit kan nul of meer invoergegevenssets hebben en een of meer uitvoergegevenssets produceren. 


### <a name="data-movement-activities"></a>Activiteiten voor gegevensverplaatsing
De kopieeractiviteit in Data Factory kopieert gegevens van een brongegevensarchief naar een sinkgegevensarchief. Data Factory ondersteunt de volgende gegevensarchieven. Gegevens vanuit elke willekeurige bron kunnen naar een sink worden geschreven. Klik op een gegevensarchief voor informatie over het kopiëren van gegevens naar en van dat archief.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Zie het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie.

### <a name="data-transformation-activities"></a>Activiteiten voor gegevenstransformatie
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Zie het artikel [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) voor meer informatie.

Als u gegevens wilt verplaatsen naar/van een gegevensarchief dat niet wordt ondersteund door de kopieeractiviteit, of gegevens wilt transformeren met behulp van uw eigen logica, maakt u een **aangepaste .NET-activiteit**. Zie [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor meer informatie over het maken en gebruiken van een aangepaste activiteit.

### <a name="pipeline"></a>Pijplijn
Een pijplijn is een groep activiteiten. De activiteiten in een pijplijn voeren samen een taak uit. Zo kan een pijplijn bijvoorbeeld een groep activiteiten bevatten die gegevens van een Azure-blob opnemen en vervolgens een Hive-query uitvoeren op een HDInsight-cluster om de logboekgegevens te partitioneren. Het voordeel van een pijplijn is dat u de activiteiten kunt beheren als een groep in plaats van afzonderlijke activiteiten. U kunt de pijplijn bijvoorbeeld in zijn geheel implementeren en plannen, in plaats van de afzonderlijke activiteiten.

### <a name="datasets"></a>Gegevenssets
Gegevenssets vertegenwoordigen gegevensstructuren in de gegevensarchieven die simpelweg verwijzen naar de gegevens die u in uw activiteiten als in- of uitvoer wilt gebruiken. Een Azure Blob-gegevensset benoemt bijvoorbeeld de blobcontainer en -map in de Azure Blob Storage van waaruit de pijplijn de gegevens moet lezen. 

### <a name="linked-services"></a>Gekoppelde services
Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Als u het op deze manier bekijkt, vertegenwoordigt de gegevensset in feite de structuur van de gegevens en definieert de gekoppelde service de verbinding met de gegevensbron.  Gekoppelde services worden voor twee doeleinden gebruikt in een Data Factory:

* Als vertegenwoordiging van een **gegevensarchief**, zoals een on-premises SQL Server, een Oracle-database, een bestandsshare of een Azure Blob Storage-account. Zie het gedeelte [Activiteiten voor gegevensverplaatsing](#data-movement-activities) voor een lijst met ondersteunde gegevensarchieven.
* Ter vertegenwoordiging van een **rekenresource** die de uitvoering van een activiteit kan hosten. De activiteit HDInsightHive wordt bijvoorbeeld uitgevoerd in een HDInsight Hadoop-cluster. Zie de sectie [Activiteiten voor gegevenstransformatie](#data-transformation-activities) voor een lijst met ondersteunde rekenomgevingen.

### <a name="relationship-between-data-factory-entities"></a>Relatie tussen Data Factory-entiteiten
![Diagram: Data Factory, een service voor gegevensintegratie in de cloud - belangrijkste concepten](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Afbeelding 2.** Relaties tussen de gegevensset, de activiteit, de pijplijn en de gekoppelde service

## <a name="supported-regions"></a>Ondersteunde regio’s
U kunt op dit moment gegevensfactory’s maken in de regio’s **VS - west**, **VS - oost** en **Noord-Europa**. Een gegevensfactory heeft echter wel toegang tot gegevensarchieven en Compute Services in andere Azure-regio’s om gegevens te verplaatsen tussen gegevensarchieven of om gegevens te verwerken middels Compute Services.

Azure Data Factory zelf slaat geen gegevens op. U kunt er gegevensgestuurde stromen mee maken om de verplaatsing van gegevens te beheren tussen [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) en om er gegevens mee te verwerken middels [Compute Services](data-factory-compute-linked-services.md) in andere regio's of in een on-premises omgeving. U kunt er ook [werkstromen mee bewaken en beheren](data-factory-monitor-manage-pipelines.md) met zowel programmatische als gebruikersinterfacemechanismen.

Hoewel Data Factory alleen beschikbaar is in **VS - west**, **VS - oost** en **Noord-Europa**, is de service die gegevensverplaatsing in Data Factory mogelijk maakt, [wereldwijd](data-factory-data-movement-activities.md#global) beschikbaar in meerdere regio’s. Als een gegevensarchief zich achter een firewall bevindt, worden de gegevens verplaatst middels een [gegevensbeheergateway](data-factory-move-data-between-onprem-and-cloud.md) die is geïnstalleerd in uw on-premises omgeving.

Voorbeeld: uw berekeningsomgevingen, zoals een Azure HDInsight-cluster en Azure Machine Learning, worden uitgevoerd in de regio West-Europa. U kunt een Azure Data Factory-exemplaar maken en gebruiken in Noord-Europa en dit gebruiken om taken te plannen in uw berekeningsomgevingen in West-Europa. Het duurt enkele milliseconden voordat Data Factory de taak in uw berekeningsomgeving activeert, maar de uitvoertijd van de taak verandert niet in uw berekeningsomgeving.

Het plan bestaat om ervoor te zorgen dat in de toekomst Azure Data Factory in alle regio’s wordt ondersteund door Azure.

## <a name="next-steps"></a>Volgende stappen
Volg de stapsgewijze instructies in de onderstaande zelfstudies voor informatie over het bouwen van gegevensfactory’s met gegevenspijplijnen:

| Zelfstudie | Beschrijving |
| --- | --- |
| [Gegevens verplaatsen tussen twee cloudlocaties voor gegevensopslag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |In deze zelfstudie maakt u een gegevensfactory met een pijplijn die **gegevens verplaatst** van Blob Storage naar SQL Database. |
| [Gegevens transformeren met een Hadoop-cluster](data-factory-build-your-first-pipeline.md) |In deze zelfstudie bouwt u uw eerste Azure-gegevensfactory met een gegevenspijplijn die **gegevens verwerkt** door Hive-script uit te voeren op een Azure HDInsight-cluster (Hadoop). |
| [Gegevens verplaatsen tussen een on-premises gegevensopslag en een gegevensarchief in de cloud met behulp van Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) |In deze zelfstudie maakt u een gegevensfactory met een pijplijn die **gegevens verplaatst** van een **on-premises** SQL Server-database naar een Azure-blob. Als onderdeel van de procedure installeert en configureert u de gegevensbeheergateway op uw computer. |

