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
ms.openlocfilehash: f899c2fb871ee528219bd48e94de62746626447f
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416117"
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

## <a name="gathering-details-to-prepare-for-a-migration"></a>Verzamelen van gegevens om voor te bereiden voor migratie

Deze sectie bevat sjabloon vragenlijsten zodat belangrijke informatie verzamelen over:

- De on-premises implementatie
- Projectdetails
- Azure-vereisten

### <a name="on-premises-deployment-questionnaire"></a>On-Premises implementatie vragenlijst

| **Vraag** | **Voorbeeld** | **Antwoord** |
|---|---|---|
|**Onderwerp**: **omgeving**|||
|Distributietype van cluster|Hortonworks, Cloudera, MapR| |
|Clusterversie distributie|HDP 2.6.5, CDH 5.7|
|Onderdelen van big Data-ecosysteem|HDFS, Yarn, Hive, LLAP, Impala, Kudu, HBase, Spark, MapReduce, Kafka, Zookeeper, Solr, Sqoop, Oozie, Ranger, Atlas, Falcon, Zeppelin, R|
|Clustertypen|Hadoop, Spark, confluente Kafka, Storm, Solr|
|Aantal clusters|4|
|Aantal Master-knooppunten|2|
|Het aantal Worker-knooppunten|100|
|Aantal Edge-knooppunten| 5|
|Totale schijfruimte|100 TB|
|Master-knooppuntconfiguratie|m/y, cpu, schijf, enzovoort.|
|Configuratie voor opslagknooppunten gegevens|m/y, cpu, schijf, enzovoort.|
|Edge-configuratie voor opslagknooppunten|m/y, cpu, schijf, enzovoort.|
|HDFS-codering?|Ja|
|Hoge beschikbaarheid|HDFS HA, HA Metastore|
|Herstel na noodgevallen / back-up maken|Back-cluster?|  
|Systemen die afhankelijk van Cluster zijn|SQL Server, Teradata, Power BI, MongoDB|
|Integratie van derden|Tableau, GridGain, Qubole, Informatica, Splunk|
|**Onderwerp**: **beveiliging**|||
|Perimeterbeveiliging|Firewalls|
|Cluster-verificatie en autorisatie|Active Directory, Ambari, Cloudera Manager, is geen verificatie|
|HDFS-toegangsbeheer|  Handmatige, ssh gebruikers|
|Hive-verificatie en autorisatie|SENTRY, LDAP, AD met Kerberos, Ranger|
|Controleren|Ambari, Cloudera Navigator Ranger|
|Bewaking|Grafiek, verzamelde, statsd, Telegraf, InfluxDB|
|Waarschuwingen|Kapacitor, Prometheus, Datadog|
|Bewaartermijn voor gegevens| drie jaar, 5 jaar|
|Clusterbeheerders|Beheerder van één, meerdere beheerders|

### <a name="project-details-questionnaire"></a>Vragenlijst voor project-details

|**Vraag**|**Voorbeeld**|**Antwoord**|
|---|---|---|
|**Onderwerp**: **Workloads en de frequentie**|||
|MapReduce-taken|10 taken--twee keer per dag||
|Hive-taken|100 taken--elk uur||
|Spark-batch-taken|50 taken--om de 15 minuten||
|Spark Streaming-taken|5 taken--om de 3 minuten||
|Structured Streaming van taken|5 taken--elke minuut||
|Trainingstaken ML-Model|2-taken, in één keer per week||
|Programmeertalen|Python, Scala, Java||
|Scripts|Python-shell||
|**Onderwerp**: **gegevens**|||
|Gegevensbronnen|Platte bestanden, Json, Kafka, RDBMS||
|Gegevensindeling|Oozie-werkstromen, luchtstroom||
|In geheugen zoekacties|Apache Ignite, Redis||
|Gegevensbestemmingen|HDFS, RDBMS van TOPKLASSE, Kafka, MPP ||
|**Onderwerp**: **metagegevens**|||
|Hive-databasetype|MySQL, Postgres||
|Nee. van Hive metastores|2||
|Nee. van Hive-tabellen|100||
|Nee. van Ranger-beleidsregels|20||
|Nee. Oozie-werkstromen|100||
|**Onderwerp**: **schaal**|||
|Gegevensvolume, met inbegrip van replicatie|100 TB||
|Dagelijkse opname-volume|50 GB||
|Groei van gegevens|10% per jaar||
|Groei van de cluster-knooppunten|5% per jaar
|**Onderwerp**: **gebruik van Cluster**|||
|Gemiddeld CPU-percentage gebruikt|60%||
|Gemiddeld geheugen percentage gebruikt|75%||
|Gebruikte schijfruimte|75%||
|Gemiddelde netwerk % gebruikt|25%
|**Onderwerp**: **personeel**|||
|Nee. Beheerders|2||
|Nee. van ontwikkelaars|10||
|Nee. van eindgebruikers|100||
|Vaardigheden|Hadoop, Spark||
|Nee. beschikbare resources voor migratie inspanningen|2||
|**Onderwerp**: **beperkingen**|||
|Huidige beperkingen|Is hoog||
|Huidige uitdagingen|Gelijktijdigheid van probleem||

### <a name="azure-requirements-questionnaire"></a>Vragenlijst voor Azure-vereisten

|**Onderwerp**: **infrastructuur** |||
|---|---|---|
|**Vraag**|**Voorbeeld**|**Antwoord**|
| De Voorkeursregio|US - oost||
|VNet de voorkeur?|Ja||
|Hoge beschikbaarheid / herstel na Noodgevallen nodig?|Ja||
|Integratie met andere cloudservices?|ADF, CosmosDB||
|**Onderwerp**: **verplaatsing van gegevens**  |||
|Voorkeur voor de eerste keer wordt geladen|DistCp, gegevens-vak, ADF, WANDisco||
|Gegevens overbrengen delta|DistCp, AzCopy||
|Continue incrementele gegevensoverdracht|DistCp, Sqoop||
|**Onderwerp**: **bewaking en waarschuwingen** |||
|Gebruik Azure bewaking en waarschuwingen over Visual Studio-integratie van derden controleren|Gebruik Azure bewaking en waarschuwingen||
|**Onderwerp**: **voorkeuren voor beveiliging** |||
|Privé en beschermd gegevenspijplijn?|Ja||
|Domein apparaat is toegevoegd aan cluster (ESPP)?|     Ja||
|On-Premises AD-synchronisatie naar de Cloud?|     Ja||
|Nee. van AD-gebruikers kunnen synchroniseren?|          100||
|Op OK om te synchroniseren van wachtwoorden naar de cloud?|    Ja||
|Alleen gebruikers in de cloud?|                 Ja||
|MFA vereist?|                       Nee|| 
|Vereisten voor autorisatie van gegevens?|  Ja||
|Toegangsbeheer op basis van rollen?|        Ja||
|Controle nodig?|                  Ja||
|Gegevensversleuteling in rust?|          Ja||
|Gegevensversleuteling onderweg zijn?|       Ja||
|**Onderwerp**: **voorkeuren Re-architectuur** |||
|Één cluster vs specifieke clustertypen|Specifieke clustertypen||
|Geplaatste opslag en externe opslag?|Externe opslag||
|Kleinere clusters als gegevens worden op afstand opgeslagen?|Kleinere clusters||
|Meerdere kleinere clusters in plaats van één groot cluster gebruiken?|Meerdere kleinere clusters gebruiken||
|Gebruik een externe metastore?|Ja||
|Metastores tussen verschillende clusters delen?|Ja||
|Ontleden werkbelastingen?|Hive-taken vervangen door de Spark-taken||
|ADF gebruiken voor het indelen van?|Nee||
|HDInsight Visual Studio Hortonworks Data Platform op IaaS?|HDInsight||

## <a name="next-steps"></a>Volgende stappen

Lees het volgende artikel in deze reeks:

- [Aanbevolen procedures van de architectuur voor on-premises naar Azure HDInsight Hadoop-migratie](apache-hadoop-on-premises-migration-best-practices-architecture.md)