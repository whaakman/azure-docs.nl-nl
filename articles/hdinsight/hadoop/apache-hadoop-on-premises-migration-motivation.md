---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - motivatie en voordelen
description: Meer informatie over de motivatie en de voordelen voor migratie on-premises Hadoop-clusters op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: efaedccde854d8cd7fa777aa9457db7203ce833a
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221882"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---motivation-and-benefits"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - motivatie en voordelen

In dit artikel wordt de eerste in een reeks over aanbevolen procedures voor het migreren van on-premises implementaties van Apache Hadoop-ecosysteem voor Azure HDInsight. Deze reeks artikelen is voor personen die verantwoordelijk voor het ontwerp, implementatie en migratie van Apache Hadoop-oplossingen in Azure HDInsight zijn. De rollen die van deze artikelen profiteren kunnen bevatten cloud-architecten, Hadoop-beheerders en DevOps-technici. Software-ontwikkelaars, gegevenstechnici en gegevenswetenschappers moeten ook profiteren van de uitleg van hoe verschillende typen clusters werk in de cloud.

## <a name="why-to-migrate-to-azure-hdinsight"></a>Reden voor het migreren naar Azure HDInsight

Azure HDInsight is een cloud-distributie van Hadoop-onderdelen uit de [Hortonworks Data Platform(HDP)](https://hortonworks.com/products/data-center/hdp/). Azure HDInsight maakt het eenvoudig, snel en kosteneffectief om enorme hoeveelheden gegevens te verwerken. HDInsight bevat de meest populaire open-source frameworks zoals:

- Apache Hadoop
- Apache Spark
- Apache Hive met LLAP
- Apache Kafka
- Apache Storm
- Apache HBase
- R

## <a name="advantages-that-azure-hdinsight-offers-over-on-premises-hadoop"></a>Voordelen van Azure HDInsight ten opzichte van on-premises Hadoop biedt

- **Lage kosten** -kosten kunnen worden teruggebracht door [het maken van clusters op aanvraag](../hdinsight-hadoop-create-linux-clusters-adf.md) en betaal alleen voor wat u gebruikt. Rekenwerk en opslag biedt een flexibiliteit doordat het gegevensvolume onafhankelijk van de grootte van het cluster.
- **Automatisch maken van een cluster** - geautomatiseerde maken van een cluster vereist minimale installatie en configuratie. Automation kan worden gebruikt voor clusters op aanvraag.
- **Hardware- en configuratie van beheerde** -hoeft u geen zorgen te hoeven maken over de fysieke hardware of infrastructuur met een HDInsight-cluster. Geef alleen de configuratie van het cluster en Azure dit ingesteld.
- **Makkelijk schaalbaar** -HDInsight kunt u [schaal](../hdinsight-administer-use-portal-linux.md) workloads omhoog of omlaag. Azure zorgt gegevens herdistributie en werkbelasting herverdeling zonder onderbreking van taken voor gegevensverwerking.
- **Wereldwijde beschikbaarheid** -HDInsight is wereldwijd beschikbaar in meer [regio's](https://azure.microsoft.com/regions/services/) dan een andere big data analytics-aanbieding. Azure HDInsight is ook beschikbaar in Azure Government, China en Duitsland, waarmee u kunt voldoen aan de behoeften van uw bedrijf in belangrijke soevereine gebieden.
- **Beveiligd en compatibel** -HDInsight kunt u uw zakelijke gegevensassets met beschermen [Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md), [versleuteling](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md), en integratie met [Azure Active Directory](../domain-joined/apache-domain-joined-introduction.md). HDInsight voldoet ook aan de meest populaire industrie en de overheid [nalevingsstandaarden](https://azure.microsoft.com/overview/trusted-cloud).
- **Vereenvoudigd beheer van** -Azure HDInsight beheert de versie van onderdelen van Hadoop-ecosysteem en deze up-to-date blijven. Software-updates zijn meestal een complex proces voor on-premises implementaties.
- **Kleinere clusters die zijn geoptimaliseerd voor specifieke werkbelastingen met minder afhankelijkheden tussen onderdelen** -een typische on-premises Hadoop-instelling maakt gebruik van één cluster die veel doeleinden fungeert. Met Azure HDInsight, kunnen specifiek voor werkbelasting-clusters worden gemaakt. Het maken van clusters voor specifieke werkbelastingen, wordt de complexiteit van het onderhouden van een cluster met groeiende complexiteit.
- **Productiviteit** -u kunt verschillende hulpprogramma's voor Hadoop en Spark gebruiken in uw favoriete ontwikkelomgeving.
- **Uitbreidbaarheid met aangepaste hulpprogramma's of toepassingen van derden** -HDInsight-clusters kunnen worden uitgebreid met geïnstalleerde onderdelen en kunnen ook worden geïntegreerd met andere big data-oplossingen met behulp van [één muisklik](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/)  -implementaties van de Azure-marktplaats.
- **Eenvoudig beheer, beheer en controle** -Azure HDInsight kan worden geïntegreerd met [Azure Log Analytics](../hdinsight-hadoop-oms-log-analytics-tutorial.md) en biedt een enkele interface waarmee u al uw clusters kunt bewaken.
- **Integratie met andere Azure-services** -HDInsight kan eenvoudig worden geïntegreerd met andere populaire Azure-services, zoals het volgende:

    - Azure Data Factory (ADF)
    - Azure Blob Storage
    - Azure Data Lake Storage Gen2
    - Azure Cosmos DB
    - Azure SQL Database
    - Azure Analysis Services

- **Zelfherstel processen en -componenten** -HDInsight controleert voortdurend de infrastructuur en open-source-onderdelen met behulp van een eigen bewakingsinfrastructuur. Deze herstelt kritieke fouten zoals onbeschikbaarheid van open-source-onderdelen en knooppunten ook automatisch. Waarschuwingen worden geactiveerd in Ambari als een OSS-onderdeel is mislukt.

Zie voor meer informatie het artikel [wat is Azure HDInsight en de Hadoop-technologiestack](../hadoop/apache-hadoop-introduction.md).

## <a name="migration-planning-process"></a>Planning-migratieproces

De volgende stappen worden aanbevolen voor het plannen van een migratie van on-premises Hadoop-clusters op Azure HDInsight:

1. Informatie over de huidige on-premises implementatie en topologieën.
2. Inzicht in de huidige projectbereik, tijdlijnen en team kennis.
3. Inzicht in de Azure-vereisten.
4. Bouw een gedetailleerde plan op basis van aanbevolen procedures.

## <a name="next-steps"></a>Volgende stappen

Lees het volgende artikel in deze reeks:

- [Aanbevolen procedures van de architectuur voor on-premises naar Azure HDInsight Hadoop-migratie](apache-hadoop-on-premises-migration-best-practices-architecture.md)