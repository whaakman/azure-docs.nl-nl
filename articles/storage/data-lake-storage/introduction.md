---
title: Azure Data Lake Storage Gen2 Preview Inleiding
description: Biedt een overzicht van Azure Data Lake Storage Gen2 Preview
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 51f38cf7ade01b58ad5ce7925af5546d1a4f1a0c
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525379"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Inleiding tot Azure Data Lake Storage Gen2 Preview

Azure Data Lake Storage Gen2 Preview is een verscheidenheid aan functies die zijn toegewezen aan de analyse van big data, gebouwd boven [Azure Blob-opslag](../blobs/storage-blobs-introduction.md). Hiermee kunt u samenwerken met uw gegevens met behulp van beide beschermingsparadigma in het systeem en de object-opslag. Hierdoor is Data Lake Storage Gen2 de enige cloud-gebaseerde multimodale storage-service, zodat u analytics-waarde uit al uw gegevens ophalen.

Data Lake Storage Gen2 bevat alle kenmerken die vereist voor de volledige levenscyclus van analytics-gegevens zijn. Dit resultaat is van de mogelijkheden van onze twee bestaande opslagservices geconvergeerd. Functies van [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), zoals de semantiek van het bestandssysteem, op bestandsniveau beveiliging en schaal worden gecombineerd met lage kosten, gelaagde opslag, mogelijkheden voor hoge beschikbaarheid/noodherstel herstel en een grote SDK/hulpprogramma's -ecosysteem van [Azure Blob-opslag](../blobs/storage-blobs-introduction.md). In Data Lake Storage Gen2 blijven alle kenmerken van de opslag van objecten tijdens het toevoegen van de voordelen van een interface file system geoptimaliseerd voor analytics werkbelastingen.

## <a name="designed-for-enterprise-big-data-analytics"></a>Ontworpen voor analyse van big data voor ondernemingen

Data Lake Storage Gen2 is de fundamentele storage-service voor het bouwen van enterprise datalakes (EDL) op Azure. Vanaf het begin ontworpen om meerdere petabytes aan gegevens terwijl de honderden gigabits van doorvoer, kunt Data Lake Storage Gen2 u een eenvoudige manier voor het beheren van enorme hoeveelheden gegevens.

Een fundamenteel onderdeel van de Data Lake Storage Gen2 is de toevoeging van een [hiërarchische naamruimte](./namespace.md) naar de Blob storage-service die organiseert van objecten/bestanden in een hiërarchie van mappen voor toegang tot de gegevens van goed presterende. De hiërarchische naamruimte kan ook Data Lake Storage Gen2 ter ondersteuning van beide objectarchief en bestand system paradigma's op hetzelfde moment. Bijvoorbeeld: algemene naamconventie voor object store maakt gebruik van slashes in de naam om na te bootsen een hiërarchische mapstructuur. Deze structuur wordt echte met Data Lake Storage Gen2. Bewerkingen zoals het hernoemen of verwijderen van een directory worden één atomic metagegevens worden uitgevoerd op de map in plaats van met het inventariseren van en verwerken van alle objecten die delen van het voorvoegsel van de naam van de map.

In het verleden had cloudanalyses te boeten op het gebied van prestaties, beheer en beveiliging. Data Lake Storage Gen2 adressen elk van deze aspecten in de volgende manieren:

- **Prestaties** is geoptimaliseerd omdat u niet nodig hebt om te kopiëren of transformatie van gegevens als een vereiste voor analyse. De hiërarchische naamruimte worden de prestaties van directory-beheerbewerkingen die de algehele prestaties van de taak aanzienlijk verbeterd.

- **Management** is eenvoudiger omdat u kunt ordenen en manipuleren van bestanden door middel van mappen en submappen.

- **Kosteneffectiviteit** wordt mogelijk gemaakt zoals Data Lake Storage Gen2 is gebaseerd op de lage kosten [Azure Blob-opslag](../blobs/storage-blobs-introduction.md). De aanvullende functies nog verder verlagen de totale eigendomskosten voor het uitvoeren van big data-analyses op Azure.

## <a name="key-features-of-data-lake-storage-gen2"></a>Belangrijke functies van Data Lake Storage Gen2

> [!NOTE]
> Tijdens de openbare preview van Data Lake Storage Gen2, kunnen sommige van de onderstaande functies variëren in hun beschikbaarheid. Als u nieuwe functies en regio's zijn die zijn uitgebracht tijdens de preview-programma, wordt deze informatie worden gecommuniceerd.
> [Meld u](https://aka.ms/adlsgen2signup) naar de openbare preview van Data Lake Storage Gen2.  

- **Hadoop-compatibele toegang**: Data Lake Storage Gen2 kunt u beheren en toegang tot gegevens, net zoals u zou met doen een [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). De nieuwe [ABFS stuurprogramma](./abfs-driver.md) is beschikbaar in alle Apache Hadoop-omgevingen, met inbegrip van [Azure HDInsight](../../hdinsight/index.yml) en [Azure Databricks](../../azure-databricks/index.yml) voor toegang tot gegevens die zijn opgeslagen in Data Lake-opslag Gen2.

- **Toegang tot gegevens van meerdere protocollen en multimodale**: Data Lake Storage Gen2 wordt beschouwd als een **meerdere modale** storage-service omdat deze biedt opslag en de bestandsnaam systeeminterfaces met dezelfde gegevens **tegelijkertijd tijd**. Dit wordt bereikt door op te geven meerdere protocoleindpunten die toegang hebben tot dezelfde gegevens. 

    In tegenstelling tot andere analyseoplossingen hoeft gegevens die zijn opgeslagen in Data Lake Storage Gen2 te verplaatsen of worden omgezet voordat u tal van analysehulpprogramma's kunt uitvoeren. U kunt toegang tot gegevens via traditionele [Blob storage-API's](../blobs/storage-blobs-introduction.md) (bijvoorbeeld: opnemen van gegevens via [Event Hubs Capture](../../event-hubs/event-hubs-capture-enable-through-portal.md)) en die gegevens met HDInsight of Azure Databricks op hetzelfde moment te verwerken. 

- **Voordelige**: Data Lake Storage Gen2 is uitgerust met lage kosten voor de opslagcapaciteit en transacties. Als gegevens overgangen gedurende de volledige levensduur, factureringstarieven invloed op de bewaren kosten tot een minimum beperkt via de ingebouwde functies zoals [Azure Blob storage-levenscyclus](../common/storage-lifecycle-managment-concepts.md).

- **Werkt met Blob storage-hulpprogramma's, frameworks en -apps**: Data Lake Storage Gen2 blijft gewoon werken met een breed scala aan hulpprogramma's, frameworks en -toepassingen die vandaag beschikbaar zijn voor Blob-opslag.

- **Geoptimaliseerde stuurprogramma**: de `abfs` stuurprogramma is [speciaal geoptimaliseerd](./abfs-driver.md) voor analyse van big data. De bijbehorende REST-API's zijn opgehaald via de `dfs` eindpunt, `dfs.core.windows.net`.

## <a name="scalability"></a>Schaalbaarheid

Azure Storage is schaalbaar standaard of u toegang via Data Lake Storage Gen2 of Blob storage-interfaces hebt. Wordt uitgevoerd om te slaan en te gebruiken om *vele exabytes aan gegevens*. Deze hoeveelheid opslag is beschikbaar met doorvoer, gemeten in gigabits per seconde (Gbps) op hoog niveau van invoer/uitvoer-bewerkingen per seconde (IOPS). Verwerking wordt dan alleen persistentie uitgevoerd op in de buurt van constante per aanvraag latenties die worden gemeten op service-, account- en bestandsniveau.

## <a name="cost-effectiveness"></a>Kosteneffectiviteit

Een van de vele voordelen van het bouwen van Data Lake Storage Gen2 boven op Azure Blob-opslag is de [goedkope](https://azure.microsoft.com/pricing/details/storage) van opslagcapaciteit en transacties. In tegenstelling tot andere cloud storage services, Data Lake Storage Gen2 kosten worden verlaagd omdat gegevens niet vereist om te worden verplaatst of getransformeerd zijn vóór het uitvoeren van analyses.

Daarnaast functies zoals de [hiërarchische naamruimte](./namespace.md) aanzienlijk verbeteren de algehele prestaties van veel analytics-taken. Deze verbetering in de prestaties van betekent dat u nodig minder rekenkracht voor het verwerken van de dezelfde hoeveelheid gegevens hebt, wat resulteert in een lagere totale eigendomskosten (TCO) voor de end-to-end-analytics-taak.

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen beschreven enkele van de belangrijkste concepten van Data Lake Storage Gen2 en details over het opslaan, openen, beheren en haal praktische informatie uit uw gegevens:

* [Hiërarchische naamruimte](./namespace.md)
* [Een opslagaccount maken](./quickstart-create-account.md)
* [Een HDInsight-cluster maken met Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
* [Een Azure Data Lake Storage Gen2-account in Azure Databricks gebruiken](./quickstart-create-databricks-account.md) 