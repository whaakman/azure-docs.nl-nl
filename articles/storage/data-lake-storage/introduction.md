---
title: Azure Data Lake Storage Gen2 Preview Inleiding
description: Biedt een overzicht van Azure Data Lake Storage Gen2 Preview
services: storage
documentationcenter: ''
author: jamesbak
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 0631b1d0c8da925858f0b7fb1d778cb1161eb737
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061521"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Inleiding tot Azure Data Lake Storage-Gen2 Preview

Azure Data Lake Storage Gen2 Preview is een reeks mogelijkheden die specifiek zijn voor big data-analyses gebouwd boven [Azure Blob storage](../blobs/storage-blobs-introduction.md). Hiermee kunt u samenwerken met uw gegevens dankzij beide bestand systeem- en storage paradigma's. Hierdoor is Data Lake Storage Gen2 de enige cloud-gebaseerde meerdere modaal storage-service, zodat u analytics-waarde van al uw gegevens niet uitpakken.

Data Lake Storage Gen2 functies alle kenmerken die vereist voor de volledige levenscyclus van analytische gegevens zijn. Dit wordt veroorzaakt doordat de mogelijkheden van onze twee bestaande opslagservices geconvergeerd. Functies van [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), zoals file system-semantiek behouden, bestands-beveiliging en schaal worden gecombineerd met een lage kosten, gelaagde opslag, hoge beschikbaarheid na noodgevallen herstelfuncties en een grote SDK/tooling ecosysteem van [Azure Blob storage](../blobs/storage-blobs-introduction.md). In Data Lake Storage Gen2 blijven alle kenmerken van de opslag van objecten tijdens het toevoegen van de voordelen van een bestand system interface geoptimaliseerd voor analytics werkbelastingen.

## <a name="designed-for-enterprise-big-data-analytics"></a>Ontworpen voor enterprise big data-analyses

Data Lake Storage Gen2 is de fundamentele storage-service voor het bouwen van enterprise data meren (EDL) in Azure. Vanaf het begin ontworpen om meerdere petabytes aan gegevens terwijl de honderden Gigabit van doorvoer te verwerken, biedt Data Lake Storage Gen2 u een eenvoudige manier voor het beheren van de enorme hoeveelheden gegevens.

Een fundamenteel onderdeel van Data Lake Storage Gen2 is de toevoeging van een [hiërarchische naamruimte](./namespace.md) met de Blob storage-service waarmee objecten en-bestanden in een hiërarchie van mappen voor gegevenstoegang zodat ordent. De hiërarchische naamruimte kan ook Data Lake Storage Gen2 ter ondersteuning van zowel objectarchief en het bestand system paradigma's op hetzelfde moment. Algemene naamconventie voor object store gebruikt voor het exemplaar slashes in de naam van na te bootsen een hiërarchische mapstructuur. Deze structuur wordt met Data Lake Storage Gen2 echte. Bewerkingen zoals het wijzigen of verwijderen van een map worden één atomic metagegevens worden uitgevoerd op de directory in plaats van met het inventariseren en verwerken van alle objecten die het voorvoegsel van de naam van de map delen.

In het verleden moest analytics cloud-gebaseerd op het gebied van prestaties, beheer en beveiliging in gevaar brengen. Data Lake Storage Gen2 adressen elk aspect in de volgende manieren:

- **Prestaties** is geoptimaliseerd omdat u niet hoeft te kopiëren of transformeer gegevens als een vereiste voor analyse. De hiërarchische naamruimte wordt aanzienlijk verbetert de prestaties van het beheer van de directory die de algehele prestaties van de taak.

- **Management** is eenvoudiger omdat u kunt ordenen en manipuleren van bestanden via mappen en submappen.

- **Kosten effectiviteit** wordt mogelijk gemaakt als het Data Lake Storage Gen2 is ingebouwd in de lage kosten [Azure Blob storage](../blobs/storage-blobs-introduction.md). De aanvullende functies meer lagere totale eigendomskosten voor het uitvoeren van big data-analyses op Azure.

## <a name="key-features-of-data-lake-storage-gen2"></a>Belangrijke functies van Data Lake Storage Gen2

> [!NOTE]
> Tijdens de openbare preview van Data Lake Storage Gen2 kunnen enkele van de hieronder vermelde functies verschillen in de beschikbaarheid ervan. Als u nieuwe functies en regio's worden uitgebracht tijdens de preview-programma, wordt deze informatie worden gecommuniceerd.
> [Aanmelden](https://aka.ms/adlsgen2signup) naar de openbare evaluatieversie van Data Lake Storage Gen2.  

- **Hadoop-compatibele toegang**: Data Lake Storage Gen2 kunt u beheren en toegang tot gegevens, net zoals u zou met doen een [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). De nieuwe [ABFS stuurprogramma](./abfs-driver.md) is beschikbaar in alle Apache Hadoop-omgevingen, met inbegrip van [Azure HDInsight](../../hdinsight/index.yml) en [Azure Databricks](../../azure-databricks/index.yml) voor toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen2.

- **Toegang tot gegevens voor multiprotocol en meerdere modaal**: Data Lake Storage Gen2 wordt beschouwd als een **meerdere modale** opslagservice zoals deze biedt object store en de bestandsnaam systeeminterfaces tot dezelfde gegevens **tegelijkertijd tijd**. Dit wordt bereikt door meerdere protocoleindpunten die kunnen toegang krijgen tot dezelfde gegevens. 

    In tegenstelling tot andere analyseoplossingen hoeft gegevens die zijn opgeslagen in Data Lake Storage Gen2 niet te verplaatsen of worden omgezet voordat u tal van hulpprogramma's voor webanalyse kunt uitvoeren. U toegang hebt tot gegevens via traditionele [Blob storage-API's](../blobs/storage-blobs-introduction.md) (bijvoorbeeld: voor het opnemen van gegevens via [Event Hubs vastleggen](../../event-hubs/event-hubs-capture-enable-through-portal.md)) en deze gegevens met HDInsight of Azure Databricks tegelijk verwerken. 

- **Rendabel**: Data Lake Storage Gen2 functies opslagcapaciteit lage kosten en transacties. Als gegevens overgangen via de volledige levenscyclus, facturering tarieven wijzigen te bewaren kosten tot een minimum via ingebouwde functies zoals [Azure Blob storage lifecycle](../common/storage-lifecycle-managment-concepts.md).

- **Werkt met hulpprogramma's voor Blob storage en frameworks apps**: Data Lake Storage Gen2 blijft werken met een breed scala aan hulpprogramma's, frameworks en toepassingen die u vandaag bestaan voor Blob-opslag.

- **Geoptimaliseerde stuurprogramma**: de `abfs` stuurprogramma is [speciaal geoptimaliseerd](./abfs-driver.md) voor big data-analyses. De bijbehorende REST-API's worden opgehaald via de `dfs` eindpunt, `dfs.core.windows.net`.

## <a name="scalability"></a>Schaalbaarheid

Azure-opslag is standaard schaalbare of u toegang tot via Data Lake Storage Gen2 of Blob storage-interfaces krijgen. Kan opslaan en leveren *veel exabytes-aan gegevens*. Deze hoeveelheid opslag is beschikbaar met doorvoer, gemeten in gigabits per seconde (Gbps) op een hoge mate van i/o-bewerkingen per seconde (IOPS). Verwerking wordt dan alleen persistentie uitgevoerd op in de buurt van constante per aanvraag latenties die worden gemeten op de service-, account- en bestandsniveau.

## <a name="cost-effectiveness"></a>Kosteneffectiviteit

Een van de vele voordelen van het bouwen van Data Lake Storage Gen2 boven op Azure Blob storage is de [goedkope](https://azure.microsoft.com/pricing/details/storage) van opslagcapaciteit en transacties. In tegenstelling tot andere services voor cloudopslag verlaagt Data Lake Storage Gen2 de kosten omdat er geen gegevens moeten worden verplaatst of getransformeerd voorafgaand aan het uitvoeren van analyses.

Daarnaast functies zoals de [hiërarchische naamruimte](./namespace.md) aanzienlijk verbeteren de algehele prestaties van veel analytics-taken. Deze verbetering van de prestaties betekent dat u nodig minder rekencapaciteit verwerken dezelfde hoeveelheid gegevens hebt, wat resulteert in een lagere totale eigendomskosten (TCO) voor de end-to-end analytics-taak.

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen wordt enkele van de belangrijkste concepten van Data Lake Storage Gen2 en detail beschreven hoe opslaan, openen, beheren en inzicht in uw gegevens krijgen:

* [Hiërarchische naamruimte](./namespace.md)
* [Een opslagaccount maken](./quickstart-create-account.md)
* [Een HDInsight-cluster maken met Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
* [Een Azure Data Lake Storage Gen2-account in Azure Databricks gebruiken](./quickstart-create-databricks-account.md) 