---
title: Apache Hadoop-onderdelen en versies - Azure HDInsight
description: Informatie over de Apache Hadoop-onderdelen en versies in HDInsight en het niveau van de service beschikbaar is in deze cloud-distributie van Hortonworks Data Platform.
keywords: hadoop-versies, onderdelen van hadoop-ecosysteem, hadoop-onderdelen, het controleren van hadoop-versie
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: overview
ms.date: 06/07/2019
ms.openlocfilehash: 847b6f12a2be0b92fe59b19b81e7953e261d1dd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67129257"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>Wat zijn de Apache Hadoop-onderdelen en versies die beschikbaar met HDInsight?

Meer informatie over de [Apache Hadoop](https://hadoop.apache.org/) ecosysteem onderdelen en versies in Microsoft Azure HDInsight, evenals de Enterprise-beveiligingspakket. Ook informatie over het controleren van de versies van Hadoop-onderdelen in HDInsight.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Apache Hadoop-onderdelen die beschikbaar zijn met verschillende versies van HDInsight

Azure HDInsight biedt ondersteuning voor meerdere versies van de Hadoop-cluster dat op elk gewenst moment kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de HDP-distributie en een set onderdelen die deel uitmaken van dit distributiepunt. Vanaf 4 April 2017, de standaard-cluster-versie die wordt gebruikt door Azure HDInsight 3.6 is en is gebaseerd op HDP 2.6.

De component-versies die zijn gekoppeld aan de versies van HDInsight-cluster worden vermeld in de volgende tabel: 

> [!NOTE]  
> De standaardversie voor de HDInsight-service kan zonder kennisgeving worden gewijzigd. Als u een afhankelijkheid versie hebt, geeft u de HDInsight-versie wanneer u uw clusters met de .NET-SDK met Azure PowerShell en de klassieke Azure-CLI maken.

| Onderdeel | HDInsight 4.0 | HDInsight 3.6 (standaard) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 |
|---------------------------|---------------|-----------------------------|---------------|---------------|---------------|----------------------|
| Hortonworks Data Platform | 3.0 | 2.6 | 2.5 | 2.4 | 2.3 | 2.2 |
| Apache Hadoop en YARN | 3.1.1 | 2.7.3 | 2.7.3 | 2.7.1 | 2.7.1 | 2.6.0 |
| Apache Tez | 0.9.1 | 0.7.0 | 0.7.0 | 0.7.0 | 0.7.0 | 0.5.2 |
| Apache Pig | 0.16.0 | 0.16.0 | 0.16.0 | 0.15.0 | 0.15.0 | 0.14.0 |
| Apache Hive en HCatalog | - | 1.2.1 | 1.2.1 | 1.2.1 | 1.2.1 | 0.14.0 |
| Apache Hive | 3.1.0 | 2.1.0 | - | - | - | - |
| Apache Tez Hive2 | - | 0.8.4 | - | - | - | - |
| Apache Ranger | 1.1.0 | 0.7.0 | 0.6.0 | - | - | - |
| Apache HBase | 2.0.1 | 1.1.2 | 1.1.2 | 1.1.2 | 1.1.1 | 0.98.4 |
| Apache Sqoop | 1.4.7 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.5 |
| Apache Oozie | 4.3.1 | 4.2.0 | 4.2.0 | 4.2.0 | 4.2.0 | 4.1.0 |
| Apache Zookeeper | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 |
| Apache Storm | - | 1.1.0 | 1.0.1 | 0.10.0 | 0.10.0 | 0.9.3 |
| Apache Mahout | - | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0 |
| Apache Phoenix | 5 | 4.7.0 | 4.7.0 | 4.4.0 | 4.4.0 | 4.2.0 |
| Apache Spark | 2.3.1 | 2.3.0, 2.2.0, 2.1.0 | 1.6.2, 2.0 | 1.6.0 | 1.5.2 | 1.3.1 (alleen Windows) |
| Apache Livy | 0.5 | 0.4, 0.4, 0.3 | 0.3 | 0.3 | 0.2 | - |
| Apache Kafka | 1.1.1 | 1.1, 1.0 * (Zie opmerking hieronder) | 0.10.0 | 0.9.0 | - | - |
| Apache Ambari | 2.7.0 | 2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 | - |
| Apache Zeppelin | 0.8.0 | 0.7.0 | - | - | - | - |
| Mono | 4.2.1 | 4.2.1 | 4.2.1 | 3.2.8 | - | - |

> [!NOTE]
> Ondersteuning voor Kafka versie 0.10 is vanwege system prestatie-overwegingen verlopen in maart 2019.

## <a name="check-for-current-hadoop-component-version-information"></a>Controleren op huidige Hadoop component versie-informatie

De Hadoop-ecosysteem onderdeel versies die zijn gekoppeld aan het HDInsight-clusterversies kunnen wijzigen met updates in HDInsight. Om te controleren op Hadoop-onderdelen en om te controleren welke versies worden gebruikt voor een cluster, gebruikt u de Ambari REST-API. De **GetComponentInformation** opdracht haalt informatie op over de onderdelen van service. Zie voor meer informatie, de [Apache Ambari-documentatie](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

### <a name="release-notes"></a>Releaseopmerkingen

Zie [opmerkingen bij de release van HDInsight](hdinsight-release-notes.md) voor extra releaseopmerkingen op de meest recente versies van HDInsight.

## <a name="supported-hdinsight-versions"></a>Ondersteunde versies van HDInsight

De volgende tabellen worden de versies van HDInsight. De HDP-versies die met elke versie van HDInsight overeenkomen worden, samen met de release product datums weergegeven. De vervaldatum en buiten gebruik stellen datums zijn ook beschikbaar wanneer ze bekend.

### <a name="available-versions"></a>Beschikbare versies

De volgende tabel bevat de versies van HDInsight die beschikbaar in de Azure-portal, evenals andere methoden voor het implementeren, zoals PowerShell en .NET SDK zijn.

| HDInsight-versie | HDP-versie | VM OS | Releasedatum | Datum van afloop voor ondersteuning | Vervaldatum | Hoge beschikbaarheid |  Beschikbaarheid in Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |HDP 3.0 |Ubuntu 16.0.4 LTS |24 september 2018 | | |Ja |Ja |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |4 april 2017 | En met 30 juni 2020 |En met 31 december 2020 |Ja |Ja |


> [!NOTE]  
> Na de ondersteuning voor een versie is verlopen, wordt mogelijk niet beschikbaar via de Microsoft Azure-portal. Clusterversies blijven echter zijn beschikbaar met de `Version` parameter in de Windows PowerShell [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) opdracht en de .NET SDK tot de vervaldatum van de versie.
>

### <a name="retired-versions"></a>Buiten gebruik gestelde versies

De volgende tabel bevat de versies van HDInsight die **niet** beschikbaar in de Azure portal.

| HDInsight-versie | HDP-versie | VM OS | Releasedatum | Datum van afloop voor ondersteuning | Vervaldatum | Hoge beschikbaarheid |  Beschikbaarheid in Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 <br> (Niet-Spark) |HDP 2.5 |Ubuntu 16.0.4 LTS |En met 30 september 2016 |5 september 2017 |28 juni 2018 |Ja |Nee |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29 maart 2016 |29 december 2016 |9 januari 2018 |Ja |Nee |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 december 2015 |27 juni 2016 |31 juli 2018 |Ja |Nee |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2 december 2015 |27 juni 2016 |En met 31 juli 2017 |Ja |Nee |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS of Windows Server 2012 R2 |18 februari 2015 |1 maart 2016 |Vanaf 1 april 2017 |Ja |Nee |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |24 juni 2014 |18 mei 2015 |Tot 30 juni 2016 |Ja |Nee |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |Op 11 februari 2014 |17 september 2014 |En met 30 juni 2015 |Ja |Nee |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |28 oktober 2013 |Vanaf 12 mei 2014 |En met 31 mei 2015 |Ja |Nee |
| HDInsight 1.6 |HDP 1.1 | |28 oktober 2013 |26 april 2014 |En met 31 mei 2015 |Nee |Nee |

> [!NOTE]  
> Maximaal beschikbare clusters met twee hoofdknooppunten worden geïmplementeerd die standaard voor HDInsight versie 2.1 en hoger. Ze zijn niet beschikbaar voor HDInsight versie 1.6-clusters.

## <a name="enterprise-security-package-for-hdinsight"></a>Enterprise-beveiligingspakket voor HDInsight

Beveiliging voor bedrijven is een optioneel pakket die u als onderdeel van de cluster-werkstroom maken op uw HDInsight-cluster kunt toevoegen. De Enterprise-beveiligingspakket wordt ondersteund:

- Integratie met Active Directory voor verificatie.

    In het verleden, kunt u alleen een HDInsight-clusters maken met een lokale beheerder en een lokale SSH-gebruiker. De gebruiker met lokale beheerdersrechten kan toegang tot alle bestanden, mappen, tabellen en kolommen.  Met de Enterprise-beveiligingspakket, kunt u op rollen gebaseerd toegangsbeheer inschakelen door de integratie van HDInsight-clusters met uw eigen Active Directory, waaronder on-premises Active Directory, Azure Active Directory Domain Services, of Active Directory op IaaS de virtuele machine. Domeinbeheerder op het cluster kan gebruikers voor het gebruik van hun eigen bedrijf (domein)-gebruikersnaam en wachtwoord voor toegang tot het cluster verlenen. 

    Zie voor meer informatie:

    - [Een inleiding tot Apache Hadoop-beveiliging met aan domein gekoppelde HDInsight-clusters](./domain-joined/apache-domain-joined-introduction.md)
    - [Azure domein Apache Hadoop-clusters in HDInsight plannen](./domain-joined/apache-domain-joined-architecture.md)
    - [Aan domein gekoppelde sandbox-omgeving configureren](./domain-joined/apache-domain-joined-configure.md)
    - [Aan domein gekoppelde HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autorisatie voor gegevens

  - Integratie met Apache Ranger voor autorisatie voor Hive, Apache Spark SQL en Yarn-wachtrijen.
  - U kunt toegangsbeheer voor bestanden en mappen instellen.

    Zie voor meer informatie:

  - [Apache Hive-beleid configureren in aan domein gekoppelde HDInsight](./domain-joined/apache-domain-joined-run-hive.md)

- Bekijk de auditlogboeken voor toegang tot een monitor en het geconfigureerde beleid. 

### <a name="supported-cluster-types"></a>Ondersteunde clustertypen

Op dit moment ondersteunen alleen de volgende clustertypen de Enterprise-beveiligingspakket:

- Hadoop (alleen HDInsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-storage"></a>Ondersteuning voor Azure Data Lake Storage

De Enterprise-beveiligingspakket ondersteunt het gebruik van Azure Data Lake Storage als zowel de primaire opslag en de extra opslag.

### <a name="pricing-and-service-level-agreement"></a>Prijs- en service level agreement

Zie voor meer informatie over prijzen en SLA voor de Enterprise-beveiligingspakket [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).


## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Service level agreement voor versies van HDInsight-cluster

De service level agreement (SLA) wordt gedefinieerd in termen van een _ondersteuning venster_. Het venster ondersteuning is de periode gedurende welke een HDInsight-cluster-versie wordt ondersteund door Microsoft Customer Service and Support. Als de versie is een _ondersteuning voor de datum van afloop_ die is doorgegeven, is het HDInsight-cluster buiten het venster ondersteuning. De vervaldatum voor de ondersteuning voor een opgegeven HDInsight versie X (nadat er een nieuwere X + 1-versie beschikbaar is) wordt berekend als de meest recente van:  

* 1-formule: 180 dagen toevoegen aan de datum waarop de HDInsight-clusterversie X werd uitgebracht.
* 2-formule: 90 dagen toevoegen aan de datum waarop de HDInsight-clusterversie X + 1 beschikbaar in Azure portal is gemaakt.

De _vervaldatum_ is de datum waarna de clusterversie in HDInsight kan worden gemaakt. Vanaf 31 juli 2017, u kan niet de grootte van een HDInsight-cluster na de vervaldatum. 

> [!NOTE]  
> HDInsight-Windows-clusters (inclusief versie 2.1, 3.0, 3.1, 3.2 en 3.3) worden uitgevoerd op Azure Guest OS Family versie 4, die gebruikmaakt van de 64-bits versie van Windows Server 2012 R2. Azure Guest OS Family versie 4 ondersteunt de versies van .NET Framework 4.0, 4.5 4.5.1 en 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks releaseopmerkingen die zijn gekoppeld aan het HDInsight-versies

De sectie bevat koppelingen naar de release-opmerkingen voor de Hortonworks Data Platform-distributies en de Apache-onderdelen die worden gebruikt met HDInsight.
* HDInsight-clusterversie 4.0 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html)
* HDInsight-clusterversie 3.6 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.6](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* HDInsight-clusterversie 3.5 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.5](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). HDInsight-clusterversie 3.5 is de _standaard_ Hadoop-cluster dat is gemaakt in de Azure-portal.
* HDInsight-clusterversie 3.4 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.4](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* HDInsight-clusterversie 3.3 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Opmerkingen bij de release van Apache Storm](https://storm.apache.org/2015/11/05/storm0100-released.html) zijn beschikbaar op de website van Apache.
  * [Opmerkingen bij de release Apache Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) zijn beschikbaar op de website van Apache.
* HDInsight-clusterversie 3.2 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.2][hdp-2-2].

  * Opmerkingen bij de release voor onderdelen die specifieke Apache zijn als volgt beschikbaar: [Hive-0,14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0,14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [algemene](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [0.9.3 Storm](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), en [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight-clusterversie 3.1 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. 3\.1 HDInsight-clusters die zijn gemaakt vóór November, 7, 2014, zijn gebaseerd op [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight-clusterversie 3.0 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight-clusterversie 2.1 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight-clusterversie 1.6 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 1.1][hdp-1-1-0].

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Standaard configuratie en de virtuele machine knooppuntgrootten voor clusters

De volgende tabellen worden de standaardgrootte van de virtuele machine (VM) voor HDInsight-clusters.  In deze grafiek is nodig om meer informatie over de VM-formaten moet worden gebruikt bij het maken van PowerShell of Azure CLI-scripts naar HDInsight-clusters implementeren.

> [!IMPORTANT]  
> Als u meer dan 32 worker-knooppunten in een cluster, moet u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB aan RAM-geheugen.

* Alle ondersteunde regio's met uitzondering van Brazilië-Zuid en Japan (West):

|Clustertype|Hadoop|HBase|Interactive Query|Storm|Spark|ML Server|Kafka|
|---|---|---|---|---|---|---|---|
|HEAD: standaard VM-grootte|D12 v2|D12 v2|D13 v2|A3|D12 v2|D12 v2|D3v2|
|HEAD: aanbevolen VM-grootten|D3 v2|D3 v2|D13|A4 v2|D12 v2|D12 v2|A2M v2|
||D4 v2|D4 v2|D14|A8 v2|D13 v2|D13 v2|D3 v2|
||D12 v2|D12 v2|E16 v3|A2m v2|D14 v2|D14 v2|D4 v2|
||E4 v3|E4 v3|E32 v3|E4 v3|E4 v3|E4 v3|D12 v2|
|Werknemer: standaard VM-grootte|D4 v2|D4 v2|D14 v2|D3 v2|D13 v2|D4 v2|4 D12v2 met 2 S30 schijven per broker|
|Werknemer: aanbevolen VM-grootten|D3 v2|D3 v2|D13|D3 v2|D4 v2|D4 v2|D13 v2|
||D4 v2|D4 v2|D14|D4 v2|D12 v2|D12 v2|DS12 v2|
||D12 v2|D12 v2|E16 v3|D12 v2|D13 v2|D13 v2|DS13 v2|
||E4 v3|E4 v3|E20 v3|E4 v3|D14 v2|D14 v2|E4 v3|
||||E32 v3||E16 v3|E16 v3|ES4 v3|
||||E64 v3||E20 v3|E20 v3|E8 v3|
||||||E32 v3|E32 v3|ES8 v3|
||||||E64 v3|E64 v3||
|ZooKeeper: standaard VM-grootte||A4 v2|A4 v2|A4 v2||A2 v2|D3v2|
|ZooKeeper: aanbevolen VM-grootten||A4 v2||A2 v2|||A2M v2|
|||A8 v2||A4 v2|||D3 v2|
|||A2m v2||A8 v2|||E8 v3|
|ML-Services: standaard VM-grootte||||||D4 v2||
|ML-Services: aanbevolen VM-grootte||||||D4 v2||
|||||||D12 v2||
|||||||D13 v2||
|||||||D14 v2||
|||||||E16 v3||
|||||||E20 v3||
|||||||E32 v3||
|||||||E64 v3||

* Brazilië-Zuid en Japan-West alleen (geen v2-grootte):

  | Clustertype | Hadoop | HBase | Interactive Query |Storm | Spark | ML Services |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: standaard VM-grootte |D12 |D12  | D13 |A3 |D12 |D12 |
  | HEAD: aanbevolen VM-grootten |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |A3,<br/> A4,<br/> A5 |D12,<br/> D13,<br/> D14 |D12,<br/> D13,<br/> D14 |
  | Werknemer: standaard VM-grootte |D4 |D4  |  D14 |D3 |D13 |D4 |
  | Werknemer: aanbevolen VM-grootten |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |D3,<br/> D4,<br/> D12 |D4,<br/> D12,<br/> D13,<br/> D14 | D4,<br/> D12,<br/> D13,<br/> D14 |
  | ZooKeeper: standaard VM-grootte | |A4 v2 | A4 v2| A4 v2 | | A2 v2|
  | ZooKeeper: aanbevolen VM-grootten | |A2,<br/> A3,<br/> A4 | |A2,<br/> A3,<br/> A4 | | |
  | ML-Services: standaard VM-grootten | | | | | |D4 |
  | ML-Services: aanbevolen VM-grootten | | | | | |D4,<br/> D12,<br/> D13,<br/> D14 |

> [!NOTE]
> - HEAD staat bekend als *Nimbus* voor de Storm-cluster type.
> - Werknemer staat bekend als *Supervisor* voor de Storm-cluster type.
> - Werknemer staat bekend als *regio* voor het HBase-cluster type.

## <a name="next-steps"></a>Volgende stappen
- [Instellingen voor Apache Hadoop, Spark- en meer informatie over de HDInsight-cluster](hdinsight-hadoop-provision-linux-clusters.md)
- [Werken in Apache Hadoop op HDInsight vanaf een Windows-PC](hdinsight-hadoop-windows-tools.md)

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html
