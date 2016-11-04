---
title: Inleiding tot Data Factory, een gegevensintegratieservice | Microsoft Docs
description: 'Leer wat een Data Factory is: een cloudgebaseerde gegevensintegratieservice waarmee de verplaatsing en transformatie van gegevens wordt beheerd en geautomatiseerd.'
keywords: gegevensintegratie, cloudgegevensintegratie, wat is Azure Data Factory
services: data-factory
documentationcenter: ''
author: spelluru
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/22/2016
ms.author: spelluru

---
# Inleiding tot de Azure Data Factory-service, een service voor gegevensintegratie in de cloud
## Wat is Azure Data Factory?
Een Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de **verplaatsing** en **transformatie** van gegevens wordt beheerd en geautomatiseerd. U kunt oplossingen voor gegevensintegratie maken met behulp van de Data Factory-service. Deze kan gegevens uit verschillende gegevensarchieven opnemen, de gegevens transformeren en verwerken, en de resulterende gegevens naar de gegevensarchieven publiceren. 

Met Data Factory-service kunt u gegevenspijplijnen maken die gegevens verplaatsen en transformeren, en de pijplijnen vervolgens uitvoeren volgens een opgegeven schema (per uur, dagelijks, wekelijks enz.). U vindt uitgebreide visualisaties om de afkomst en afhankelijkheden tussen uw gegevenspijplijnen weer te geven en al uw gegevenspijplijnen te controleren vanuit één centrale weergave zodat u eenvoudig problemen kunt detecteren en bewakingswaarschuwingen kunt instellen.

![Diagram: Data Factory-overzicht, een service voor gegevensintegratie](./media/data-factory-introduction/what-is-azure-data-factory.png)
**Afbeelding1.** Neem gegevens op van verschillende gegevensbronnen en bereid ze voor, transformeer en analyseer ze en publiceer daarna kant-en-klare gegevens die geschikt zijn voor gebruik.

## Pijplijnen en activiteiten
In een Data Factory-oplossing maakt u een of meer gegevens**pijplijnen**. Een pijplijn is een logische groep activiteiten. Ze worden gebruikt om activiteiten te groeperen in één eenheid waarmee vervolgens een taak kan worden uitgevoerd. 

Met **activiteiten** definieert u welke acties moeten worden uitgevoerd voor uw gegevens. U kunt bijvoorbeeld een kopieeractiviteit gebruiken om gegevens van één gegevensarchief naar een andere te kopiëren. U kunt ook een Hive-activiteit gebruiken, waarmee een Hive-query wordt uitgevoerd voor een Azure HDInsight-cluster om uw gegevens te transformeren of analyseren. Data Factory ondersteunt twee soorten activiteiten: activiteiten voor gegevensverplaatsing en activiteiten voor gegevenstransformatie. 

## Activiteiten voor gegevensverplaatsing
[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Zie het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie. 

## Activiteiten voor gegevenstransformatie
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Zie het artikel [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) voor meer informatie.

Als u gegevens wilt verplaatsen naar/van een gegevensarchief dat niet wordt ondersteund door de kopieeractiviteit, of gegevens wilt transformeren met behulp van uw eigen logica, maakt u een **aangepaste .NET-activiteit**. Zie [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor meer informatie over het maken en gebruiken van een aangepaste activiteit.

## Gekoppelde services
Met gekoppelde services wordt gedefinieerd welke informatie nodig is om een Data Factory verbinding te laten maken met externe resources (bijvoorbeeld Azure Storage, on-premises SQL Server, Azure HDInsight). Gekoppelde services worden voor twee doeleinden gebruikt in een Data Factory:

* Als vertegenwoordiging van een **gegevensarchief**, zoals een on-premises SQL Server, een Oracle-database, een bestandsshare of een Azure Blob Storage-account. Zie het gedeelte [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor een lijst met ondersteunde gegevensarchieven. 
* Ter vertegenwoordiging van een **rekenresource** die de uitvoering van een activiteit kan hosten. De activiteit HDInsightHive wordt bijvoorbeeld uitgevoerd in een HDInsight Hadoop-cluster. Zie de sectie [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) voor een lijst met ondersteunde rekenomgevingen. 

## Gegevenssets
Met gekoppelde services worden gegevensarchieven gekoppeld aan een Azure Data Factory. Gegevenssets vertegenwoordigen gegevensstructuren binnen de gegevensarchieven. Een met Azure Storage gekoppelde service geeft bijvoorbeeld verbindingsgegevens voor de Data Factory zodat deze verbinding kan maken met een Azure Storage-account. Een Azure Blob-gegevensset specificeert de blobcontainer en -map in de Azure Blob-opslag van waaruit de pijplijn de gegevens moet lezen. Op deze manier biedt een gekoppelde Azure SQL-service verbindingsgegevens voor een Azure SQL-database en een Azure SQL-gegevensset specificeert de tabel die de gegevens bevat.   

## Relatie tussen Data Factory-entiteiten
Data Factory heeft enkele belangrijke entiteiten die samenwerken om de in- en uitvoergegevens, verwerkingsgebeurtenissen en planning en resources te definiëren die nodig zijn om de gewenste gegevensstroom uit te voeren.

![Diagram: Data Factory, een service voor gegevensintegratie in de cloud - Belangrijkste concepten](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Afbeelding 2.** Relaties tussen de gegevensset, de activiteit, de pijplijn en de gekoppelde service

Met de vier eenvoudige concepten van gekoppelde services, gegevenssets, activiteiten en pijplijnen kunt u snel aan de slag. U kunt [Uw eerste pijplijn maken](data-factory-build-your-first-pipeline.md). 

## Ondersteunde regio’s
U kunt op dit moment gegevensfactory’s maken in de regio’s **VS - west**, **VS - oost** en **Noord-Europa**. Een gegevensfactory heeft echter wel toegang tot gegevensarchieven en Compute Services in andere Azure-regio’s om gegevens te verplaatsen tussen gegevensarchieven of om gegevens te verwerken middels Compute Services. 

Azure Data Factory zelf slaat geen gegevens op. U kunt er gegevensgestuurde stromen mee maken om de verplaatsing van gegevens te beheren tussen [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores) en om er gegevens mee te verwerken middels [Compute Services](data-factory-compute-linked-services.md) in andere regio's of in een on-premises omgeving. U kunt er ook [werkstromen mee bewaken en beheren](data-factory-monitor-manage-pipelines.md) met zowel programmatische als gebruikersinterfacemechanismen. 

Hoewel Azure Data Factory alleen beschikbaar is in **VS - west**, **VS - oost** en **Noord-Europa**, is de service die gegevensverplaatsing in Data Factory mogelijk maakt, [wereldwijd](data-factory-data-movement-activities.md#global) beschikbaar in meerdere regio’s. Als een gegevensarchief zich achter een firewall bevindt, worden de gegevens verplaatst middels een [gegevensbeheergateway](data-factory-move-data-between-onprem-and-cloud.md) die is geïnstalleerd in uw on-premises omgeving. 

Voorbeeld: uw berekeningsomgevingen, zoals een Azure HDInsight-cluster en Azure Machine Learning, worden uitgevoerd in de regio West-Europa. U kunt een Azure Data Factory-exemplaar maken en gebruiken in Noord-Europa en dit gebruiken om taken te plannen in uw berekeningsomgevingen in West-Europa. Het duurt enkele milliseconden voordat Data Factory de taak in uw berekeningsomgeving activeert, maar de uitvoertijd van de taak verandert niet in uw berekeningsomgeving.

Het plan bestaat om ervoor te zorgen dat in de toekomst Azure Data Factory in alle regio’s wordt ondersteund door Azure.

## Volgende stappen
Volg de stapsgewijze instructies in de onderstaande zelfstudies voor informatie over het bouwen van gegevensfactory’s met gegevenspijplijnen. 

| Zelfstudie | Beschrijving |
| --- | --- |
| [Een gegevenspijplijn maken die gegevens verwerkt met behulp van Hadoop-cluster](data-factory-build-your-first-pipeline.md) |In deze zelfstudie bouwt u uw eerste Azure-gegevensfactory met een gegevenspijplijn die **gegevens verwerkt** door Hive-script uit te voeren op een Azure HDInsight-cluster (Hadoop). |
| [Een gegevenspijplijn maken voor het verplaatsen van gegevens tussen twee gegevensarchieven in de cloud](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |In deze zelfstudie maakt u een gegevensfactory met een pijplijn die **gegevens verplaatst** van Blob Storage naar SQL Database. |
| [Een gegevenspijplijn maken voor het verplaatsen van gegevens tussen een on-premises gegevensopslag en een gegevensarchief in de cloud met behulp van Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) |In deze zelfstudie maakt u een gegevensfactory met een pijplijn die **gegevens verplaatst** van een **on-premises** SQL Server-database naar een Azure-blob. Als onderdeel van de procedure installeert en configureert u de gegevensbeheergateway op uw computer. |

<!--HONumber=Oct16_HO3-->


