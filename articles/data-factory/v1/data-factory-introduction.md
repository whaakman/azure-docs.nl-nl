---
title: Inleiding tot Data Factory, een gegevensintegratieservice | Microsoft Docs
description: 'Leer wat een Data Factory is: een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd.'
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
ms.date: 08/14/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 8d4d574dee4b993d8a464482b244e1f63ade8a57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-data-factory"></a>Inleiding tot Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-introduction.md)
> * [Versie 2 - Preview](../introduction.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service gebruikt, die zich in de previewfase bevindt, raadpleegt u [Inleiding tot Data Factory versie 2](../introduction.md).


## <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory?
Hoe worden zakelijke gegevens in de wereld van big data hergebruikt? Is het mogelijk om gegevens die in de cloud zijn gegenereerd, waardevoller te maken met behulp van referentiegegevens uit on-premises gegevensbronnen of andere ongelijksoortige gegevensbronnen? We nemen een gamingbedrijf als voorbeeld. Het verzamelt veel logboeken die wordt geproduceerd door games in de cloud. Het bedrijf wil deze logboeken analyseren en inzicht krijgen in klantvoorkeuren, demografische gegevens, gebruiksgedrag, enz. Zo wil het mogelijkheden voor bijverkopen en kruisverkopen ontdekken, nieuwe interessante functies ontwikkelen om de groei van het bedrijf te stimuleren en een betere ervaring bieden aan klanten. 

Voor het analyseren van deze logboeken moet het bedrijf gebruikmaken van de referentiegegevens, zoals klantgegevens, game-informatie en marketingcampagnegegevens, die zich in een on-premises gegevensarchief bevinden. Daarom wil het bedrijf logboekgegevens opnemen uit het gegevensarchief in de cloud en gegevens uit het on-premises gegevensarchief raadplegen. Vervolgens moeten de gegevens worden verwerkt met behulp van Hadoop in de cloud (Azure HDInsight) en moeten de resulterende gegevens worden gepubliceerd in een datawarehouse in de cloud, zoals Azure SQL Data Warehouse, of een on-premises gegevensarchief zoals SQL Server. Deze werkstroom moet wekelijks worden uitgevoerd. 

Hier is een platform nodig waarmee het bedrijf een werkstroom kan maken die gegevens uit zowel een on-premises gegevensarchief als een gegevensarchief in de cloud kan opnemen. Daarnaast moeten deze gegevens kunnen worden getransformeerd of verwerkt met behulp van bestaande rekenservices zoals Hadoop, en moeten deze worden gepubliceerd naar een on-premises gegevensarchief of een gegevensarchief in de cloud. Zo kunnen BI-toepassingen deze gegevens gebruiken. 

![Overzicht van Data Factory](media/data-factory-introduction/what-is-azure-data-factory.png) 

Azure Data Factory is het ideale platform voor dit soort scenario's. Het is een **cloud-gebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud en zo gegevensverplaatsing en -transformatie kunt indelen en automatiseren**. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken.  

Het is eerder een EL-platform (Extract-and-Load) en vervolgens een TL-platform (Transform-and-Load) dan een traditioneel ETL-platform (Extract-Transform-and-Load). De transformaties die worden uitgevoerd zijn transformaties/verwerkingen van gegevens met behulp van rekenservices in plaats van uitvoertransformaties, zoals die voor het toevoegen van afgeleide kolommen, het tellen van het aantal rijen, het sorteren van gegevens, enzovoort. 

De gegevens die in Azure Data Factory worden verwerkt en geproduceerd door werkstromen zijn momenteel **tijdgesegmenteerd** (per uur, dagelijks, wekelijks, enzovoort). Een pijplijn kan bijvoorbeeld één keer per dag invoergegevens lezen, gegevens verwerken en uitvoer produceren. U kunt er ook voor kiezen om een werkstroom slechts één keer uit te voeren.  
  

## <a name="how-does-it-work"></a>Hoe werkt het? 
De pijplijnen (gegevensgestuurde werkstromen) in Azure Data Factory voeren normaal gesproken de volgende drie stappen uit:

![Drie fasen van Azure Data Factory](media/data-factory-introduction/three-information-production-stages.png)

### <a name="connect-and-collect"></a>Verbinding maken en verzamelen
Ondernemingen hebben verschillende typen gegevens, opgeslagen op verschillende bronnen. De eerste stap voor het bouwen van een informatieproductiesysteem bestaat uit het verbinden van alle vereiste gegevens- en verwerkingsbronnen, zoals SaaS-services, bestandsshares, FTP en webservices, en het zo nodig verplaatsen van gegevens naar een centrale locatie voor verdere verwerking.

Zonder Data Factory moeten ondernemingen aangepaste onderdelen voor gegevensverplaatsing ontwikkelen of aangepaste services schrijven om deze gegevensbronnen en verwerking te integreren. Het is duur en lastig om dergelijke systemen te integreren en te onderhouden, en vaak ontbreken de adequate bewakingssystemen, waarschuwingssystemen en besturingselementen die een volledig beheerde service wel kan bieden.

Met Data Factory kunt u de kopieeractiviteit in een pijplijn gebruiken om gegevens van on-premises gegevensarchieven en gegevensarchieven uit de cloud te verplaatsen naar een gecentraliseerd gegevensarchief in de cloud voor verdere analyse. Zo kunt u bijvoorbeeld gegevens in Azure Data Lake Store verzamelen en later transformeren met behulp van een Azure Data Lake Analytics Compute-service. Of u kunt gegevens verzamelen in Azure Blob Storage en later transformeren met behulp van een Hadoop-cluster van Azure HDInsight.

### <a name="transform-and-enrich"></a>Transformeren en verrijken
Als gegevens aanwezig zijn in een gecentraliseerd gegevensarchief in de cloud, wilt u dat de verzamelde gegevens worden verwerkt of getransformeerd met behulp van rekenservices zoals HDInsight Hadoop, Spark, Data Lake Analytics of Machine Learning. U wilt dat de gegevens op een betrouwbare manier en volgens een beheersbare en gecontroleerde planning worden geproduceerd om productieomgevingen te voorzien van vertrouwde gegevens. 

### <a name="publish"></a>Publiceren 
Lever vanuit de cloud getransformeerde gegevens aan on-premises bronnen zoals SQL Server, of sla de informatie op in uw cloudopslagbronnen voor gebruik door BI- (Business Intelligence) en analysehulpprogramma's en andere toepassingen.

## <a name="key-components"></a>Belangrijkste onderdelen
Een Azure-abonnement kan een of meer Azure Data Factory-exemplaren (oftewel 'data factory's') hebben. Azure Data Factory bestaat uit vier hoofdonderdelen die samenwerken om een platform te bieden waarop u voor uw gegevensgestuurde werkstromen kunt maken met stappen voor de verplaatsing en transformatie van gegevens. 

### <a name="pipeline"></a>Pijplijn
Een data factory kan één of meer pijplijnen hebben. Een pijplijn is een groep activiteiten. De activiteiten in een pijplijn voeren samen een taak uit. Zo kan een pijplijn bijvoorbeeld een groep activiteiten bevatten die gegevens van een Azure-blob opnemen en vervolgens een Hive-query uitvoeren op een HDInsight-cluster om de gegevens te partitioneren. Het voordeel van een pijplijn is dat u de activiteiten kunt beheren als een groep in plaats van afzonderlijke activiteiten. U kunt de pijplijn bijvoorbeeld in zijn geheel implementeren en plannen, in plaats van de afzonderlijke activiteiten. 

### <a name="activity"></a>Activiteit
Een pijplijn kan één of meer activiteiten bevatten. Met activiteiten definieert u welk acties moeten worden uitgevoerd voor uw gegevens. U kunt bijvoorbeeld een kopieeractiviteit gebruiken om gegevens van één gegevensarchief naar een andere te kopiëren. U kunt ook een Hive-activiteit gebruiken, waarmee een Hive-query wordt uitgevoerd voor een Azure HDInsight-cluster om uw gegevens te transformeren of analyseren. Data Factory ondersteunt twee soorten activiteiten: activiteiten voor gegevensverplaatsing en activiteiten voor gegevenstransformatie.

### <a name="data-movement-activities"></a>Activiteiten voor gegevensverplaatsing
De kopieeractiviteit in Data Factory kopieert gegevens van een brongegevensarchief naar een sinkgegevensarchief. Data Factory ondersteunt de volgende gegevensarchieven. Gegevens vanuit elke willekeurige bron kunnen naar een sink worden geschreven. Klik op een gegevensarchief voor informatie over het kopiëren van gegevens naar en van dat archief.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

Zie het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie.

### <a name="data-transformation-activities"></a>Activiteiten voor gegevenstransformatie
[!INCLUDE [data-factory-transformation-activities](../../../includes/data-factory-transformation-activities.md)]

Zie het artikel [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) voor meer informatie.

### <a name="custom-net-activities"></a>Aangepaste .NET-activiteiten
Als u gegevens wilt verplaatsen naar/van een gegevensarchief dat niet wordt ondersteund door de kopieeractiviteit, of gegevens wilt transformeren met behulp van uw eigen logica, maakt u een **aangepaste .NET-activiteit**. Zie [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor meer informatie over het maken en gebruiken van een aangepaste activiteit.

### <a name="datasets"></a>Gegevenssets
Voor een activiteit zijn nul of meer gegevenssets nodig als invoer en één of meer gegevenssets als uitvoer. Gegevenssets vertegenwoordigen gegevensstructuren in de gegevensarchieven die simpelweg verwijzen naar de gegevens die u in uw activiteiten als in- of uitvoer wilt gebruiken. Een Azure Blob-gegevensset benoemt bijvoorbeeld de blobcontainer en -map in de Azure Blob Storage van waaruit de pijplijn de gegevens moet lezen. Of een Azure SQL Table-gegevensset bevat de tabel waar de uitvoergegevens worden geschreven door de activiteit. 

### <a name="linked-services"></a>Gekoppelde services
Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Als u het op deze manier bekijkt, vertegenwoordigt de gegevensset in feite de structuur van de gegevens en definieert de gekoppelde service de verbinding met de gegevensbron. Een met Azure Storage gekoppelde service geeft bijvoorbeeld een verbindingsreeks zodat deze verbinding kan maken met het Azure Storage-account. En een Azure Blob-gegevenssets geeft de blobcontainer op, en de map met de gegevens.   

Gekoppelde services worden voor twee doeleinden gebruikt in een Data Factory:

* Als vertegenwoordiging van een **gegevensarchief**, zoals een on-premises SQL Server, een Oracle-database, een bestandsshare of een Azure Blob Storage-account. Zie het gedeelte [Activiteiten voor gegevensverplaatsing](#data-movement-activities) voor een lijst met ondersteunde gegevensarchieven.
* Ter vertegenwoordiging van een **rekenresource** die de uitvoering van een activiteit kan hosten. De activiteit HDInsightHive wordt bijvoorbeeld uitgevoerd in een HDInsight Hadoop-cluster. Zie de sectie [Activiteiten voor gegevenstransformatie](#data-transformation-activities) voor een lijst met ondersteunde rekenomgevingen.

### <a name="relationship-between-data-factory-entities"></a>Relatie tussen Data Factory-entiteiten
![Diagram: Data Factory, een service voor gegevensintegratie in de cloud - belangrijkste concepten](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Afbeelding 2.** Relaties tussen de gegevensset, de activiteit, de pijplijn en de gekoppelde service

## <a name="supported-regions"></a>Ondersteunde regio’s
U kunt op dit moment gegevensfactory’s maken in de regio’s **VS - west**, **VS - oost** en **Noord-Europa**. Een gegevensfactory heeft echter wel toegang tot gegevensarchieven en Compute Services in andere Azure-regio’s om gegevens te verplaatsen tussen gegevensarchieven of om gegevens te verwerken middels Compute Services.

Azure Data Factory zelf slaat geen gegevens op. U kunt er gegevensgestuurde werkstromen mee maken om de verplaatsing van gegevens te beheren tussen [ondersteunde gegevensarchieven](#data-movement-activities) en om er gegevens mee te verwerken middels [Compute Services](#data-transformation-activities) in andere regio's of in een on-premises omgeving. U kunt er ook [werkstromen mee bewaken en beheren](data-factory-monitor-manage-pipelines.md) met zowel programmatische als gebruikersinterfacemechanismen.

Hoewel Data Factory alleen beschikbaar is in **VS - west**, **VS - oost** en **Noord-Europa**, is de service die gegevensverplaatsing in Data Factory mogelijk maakt, [wereldwijd](data-factory-data-movement-activities.md#global) beschikbaar in meerdere regio’s. Als een gegevensarchief zich achter een firewall bevindt, worden de gegevens verplaatst middels een [gegevensbeheergateway](data-factory-move-data-between-onprem-and-cloud.md) die is geïnstalleerd in uw on-premises omgeving.

Voorbeeld: uw berekeningsomgevingen, zoals een Azure HDInsight-cluster en Azure Machine Learning, worden uitgevoerd in de regio West-Europa. U kunt een Azure Data Factory-exemplaar maken en gebruiken in Noord-Europa en dit gebruiken om taken te plannen in uw berekeningsomgevingen in West-Europa. Het duurt enkele milliseconden voordat Data Factory de taak in uw berekeningsomgeving activeert, maar de uitvoertijd van de taak verandert niet in uw berekeningsomgeving.

## <a name="get-started-with-creating-a-pipeline"></a>Aan de slag met het maken van een pijplijn
U kunt een van deze hulpprogramma's of API's gebruiken om gegevenspijplijnen te maken in Azure Data Factory: 

- Azure Portal
- Visual Studio
- PowerShell
- .NET API
- REST API
- Azure Resource Manager-sjabloon. 

Volg de stapsgewijze instructies in de onderstaande zelfstudies voor informatie over het bouwen van gegevensfactory’s met gegevenspijplijnen:

| Zelfstudie | Beschrijving |
| --- | --- |
| [Gegevens verplaatsen tussen twee cloudlocaties voor gegevensopslag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |In deze zelfstudie maakt u een gegevensfactory met een pijplijn die **gegevens verplaatst** van Blob Storage naar SQL Database. |
| [Gegevens transformeren met een Hadoop-cluster](data-factory-build-your-first-pipeline.md) |In deze zelfstudie bouwt u uw eerste Azure-gegevensfactory met een gegevenspijplijn die **gegevens verwerkt** door Hive-script uit te voeren op een Azure HDInsight-cluster (Hadoop). |
| [Gegevens verplaatsen tussen een on-premises gegevensopslag en een gegevensarchief in de cloud met behulp van Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) |In deze zelfstudie maakt u een gegevensfactory met een pijplijn die **gegevens verplaatst** van een **on-premises** SQL Server-database naar een Azure-blob. Als onderdeel van de procedure installeert en configureert u de gegevensbeheergateway op uw computer. |
