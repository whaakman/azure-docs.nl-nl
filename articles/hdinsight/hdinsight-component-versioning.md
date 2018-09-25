---
title: Hadoop-onderdelen en versies - Azure HDInsight
description: Informatie over de Hadoop-onderdelen en versies in HDInsight en het niveau van de service beschikbaar is in deze cloud-distributie van Hortonworks Data Platform.
keywords: hadoop-versies, onderdelen van hadoop-ecosysteem, hadoop-onderdelen, het controleren van hadoop-versie
services: hdinsight
ms.reviewer: jasonh
author: kkampf
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: kakampf
ms.openlocfilehash: 2f6ced6603c6736200f4474fbb1dc9b0ef1535d9
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056801"
---
# <a name="what-are-the-hadoop-components-and-versions-available-with-hdinsight"></a>Wat zijn de Hadoop-onderdelen en versies die beschikbaar met HDInsight?

Meer informatie over de Apache Hadoop-ecosysteem onderdelen en versies in Microsoft Azure HDInsight, evenals de Enterprise-beveiligingspakket. Ook informatie over het controleren van de versies van Hadoop-onderdelen in HDInsight. 

Elke versie van HDInsight is een cloud-distributie van een versie van Hortonworks Data Platform (HDP).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Beschikbaar met verschillende versies van HDInsight Hadoop-onderdelen
Azure HDInsight biedt ondersteuning voor meerdere versies van de Hadoop-cluster dat op elk gewenst moment kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de HDP-distributie en een set onderdelen die deel uitmaken van dit distributiepunt. Vanaf 4 April 2017, de standaard-cluster-versie die wordt gebruikt door Azure HDInsight 3.6 is en is gebaseerd op HDP 2.6.

De component-versies die zijn gekoppeld aan de versies van HDInsight-cluster worden vermeld in de volgende tabel: 

> [!NOTE]
> De standaardversie voor de HDInsight-service kan zonder kennisgeving worden gewijzigd. Als u een afhankelijkheid versie hebt, geeft u de HDInsight-versie wanneer u uw clusters met de .NET-SDK met Azure PowerShell en de klassieke Azure-CLI maken.

| Onderdeel | HDInsight 4.0 | HDInsight 3.6 (standaard) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | 3.2 voor HDInsight | HDInsight 3.1 | HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Data Platform |3.0 |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop en YARN |2.9.1 |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.9.1 |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive en HCatalog |-|1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive |3.1.0 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 |-| 0.8.4 |-|-|-|-|-|-|
| Apache Ranger |1.1.0 |0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |2.0.1 |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.7 |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.3.1 |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.2.1 |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |-|0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |5 |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.3.1 |2.3.0, 2.2.0, 2.1.0 |1.6.2, 2.0 |1.6.0 |1.5.2 |1.3.1 (alleen Windows) |-|-|
| Apache Livy |0.5 |0.4 |0,3 |0,3 |0.2 |-|-|-|
| Apache Kafka | 1.0.1 |1.1, 1.0, 0.10.1 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.7.0 |2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.8.0 |0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |4.2.1 |3.2.8 |-|-|-|
| Apache schuifregelaar |-| 0.92.0 |-|-|-|-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>Controleren op huidige Hadoop component versie-informatie

De Hadoop-ecosysteem onderdeel versies die zijn gekoppeld aan het HDInsight-clusterversies kunnen wijzigen met updates in HDInsight. Om te controleren op Hadoop-onderdelen en om te controleren welke versies worden gebruikt voor een cluster, gebruikt u de Ambari REST-API. De **GetComponentInformation** opdracht haalt informatie op over de onderdelen van service. Zie voor meer informatie, de [Ambari documentatie][ambari-docs].

Voor Windows-clusters is een andere manier om te controleren of de onderdeelversie Meld u aan bij een cluster met behulp van extern bureaublad en bekijk de inhoud van de map C:\apps\dist\.

> [!IMPORTANT]
> Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.4 of hoger. Zie voor meer informatie, [buitengebruikstelling van HDInsight Windows](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Releaseopmerkingen

Zie [opmerkingen bij de release van HDInsight](hdinsight-release-notes.md) voor extra releaseopmerkingen op de meest recente versies van HDInsight.

## <a name="supported-hdinsight-versions"></a>Ondersteunde versies van HDInsight
De volgende tabellen worden de versies van HDInsight. De HDP-versies die met elke versie van HDInsight overeenkomen worden, samen met de release product datums weergegeven. De vervaldatum en buiten gebruik stellen datums zijn ook beschikbaar wanneer ze bekend.

### <a name="available-versions"></a>Beschikbare versies

De volgende tabel bevat de versies van HDInsight die beschikbaar in de Azure-Portal, evenals andere methoden voor het implementeren, zoals PowerShell en .NET SDK zijn.

| HDInsight-versie | HDP-versie | VM OS | Releasedatum | Datum van afloop voor ondersteuning | Vervaldatum | Hoge beschikbaarheid |  Beschikbaarheid in Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |HDP 3.0 |Ubuntu 16.0.4 LTS |24 september 2018 | | |Ja |Ja |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |4 april 2017 | | |Ja |Ja |

> [!NOTE]
> Na de ondersteuning voor een versie is verlopen, wordt mogelijk niet beschikbaar via de Microsoft Azure-portal. Clusterversies blijven echter zijn beschikbaar met de `Version` parameter in de Windows PowerShell [New-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightcluster) opdracht en de .NET SDK tot de vervaldatum van de versie.
>

### <a name="retired-versions"></a>Buiten gebruik gestelde versies

De volgende tabel bevat de versies van HDInsight die **niet** beschikbaar in de Azure-Portal.

| HDInsight-versie | HDP-versie | VM OS | Releasedatum | Datum van afloop voor ondersteuning | Vervaldatum | Hoge beschikbaarheid |  Beschikbaarheid in Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 |HDP 2.5 |Ubuntu 16.0.4 LTS |En met 30 september 2016 |5 september 2017 |28 juni 2018 |Ja |Nee |
| HDInsight 3.4 |HDP 2.4 |14.0.4 Ubuntu LTS |29 maart 2016 |29 december 2016 |9 januari 2018 |Ja |Nee |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 december 2015 |27 juni 2016 |31 juli 2018 |Ja |Nee |
| HDInsight 3.3 |HDP 2.3 |14.0.4 Ubuntu LTS |2 december 2015 |27 juni 2016 |En met 31 juli 2017 |Ja |Nee |
| 3.2 voor HDInsight |HDP 2.2 |Ubuntu 12.04 LTS of Windows Server 2012 R2 |18 februari 2015 |1 maart 2016 |Vanaf 1 april 2017 |Ja |Nee |
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

    - [Een inleiding tot Hadoop-beveiliging met aan domein gekoppelde HDInsight-clusters](./domain-joined/apache-domain-joined-introduction.md)
    - [Azure domein Hadoop-clusters in HDInsight plannen](./domain-joined/apache-domain-joined-architecture.md)
    - [Aan domein gekoppelde sandbox-omgeving configureren](./domain-joined/apache-domain-joined-configure.md)
    - [Aan domein gekoppelde HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autorisatie voor gegevens

    - Integratie met Apache Ranger voor autorisatie voor Hive, Apache Spark SQL en Yarn-wachtrijen.
    - U kunt toegangsbeheer voor bestanden en mappen instellen.

    Zie voor meer informatie:

    - [Hive-beleidsregels configureren in aan domein gekoppelde HDInsight](./domain-joined/apache-domain-joined-run-hive.md)

- Bekijk de auditlogboeken voor toegang tot een monitor en het geconfigureerde beleid. 

### <a name="supported-cluster-types"></a>Ondersteunde clustertypen

Op dit moment ondersteunen alleen de volgende clustertypen de Enterprise-beveiligingspakket:

- Hadoop (alleen HDInsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-store"></a>Ondersteuning voor Azure Data Lake Store

De Enterprise-beveiligingspakket ondersteunt het gebruik van Azure Data Lake Store als zowel de primaire opslag en de extra opslag.

### <a name="pricing-and-sla"></a>Prijzen en SLA
Zie voor meer informatie over prijzen en SLA voor de Enterprise-beveiligingspakket [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hdinsight-windows-retirement"></a>Buitengebruikstelling van HDInsight Windows
Microsoft Azure HDInsight versie 3.3 is de laatste versie van HDInsight in Windows. De vervaldatum voor HDInsight in Windows is en met 31 juli 2018. Als u een HDInsight-clusters op Windows 3.3 of eerder hebt, moet u vóór 31 juli 2018 migreren naar HDInsight in Linux (HDInsight versie 3.5 of hoger). Migreren naar de Linux-besturingssysteem, kunt u de mogelijkheid om te maken of het formaat wijzigen van uw HDInsight-clusters te behouden. Ondersteuning voor HDInsight versie 3.3 op Windows is verlopen op 27 juni 2016.

Beginnen met een HDInsight versie 3.4, heeft Microsoft HDInsight alleen op het besturingssysteem Linux uitgebracht. Als gevolg hiervan zijn enkele van de onderdelen in HDInsight uitsluitend beschikbaar voor Linux. Deze omvatten Apache Ranger, Kafka, Interactive Query, Spark, HDInsight-toepassingen, en Azure Data Lake Store als het primaire bestandssysteem. Toekomstige versies van HDInsight zijn alleen beschikbaar op de Linux-besturingssysteem. Er zijn geen toekomstige versies van HDInsight in Windows. 

## <a name="faqs"></a>Veelgestelde vragen

### <a name="what-is-the-timeline-for-retiring-hdinsight-on-windows"></a>Wat is de planning voor het buiten gebruik stellen van HDInsight in Windows?
31 juli 2018 wordt de vervaldatum voor HDInsight in Windows. Als de geplande vervaldatum voor uw regio verschilt, wordt u afzonderlijk gewaarschuwd. 

### <a name="what-is-the-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>Wat zijn de gevolgen van het buiten gebruik stellen van HDInsight in Windows voor bestaande klanten?
Nadat HDInsight op Windows is buiten gebruik gesteld, kunt u een nieuw HDInsight-Windows-cluster maken of vergroten of verkleinen van een bestaand HDInsight-Windows-cluster. Er is ondersteuning voor HDInsight versie 3.3 verlopen op 27 juni 2016. Daarom is er geen ondersteuning of oplossingen voor HDInsight 3.3 of eerdere versies. Toekomstige versies van HDInsight zijn alleen beschikbaar op de Linux-besturingssysteem. Er zijn geen toekomstige versies van HDInsight in Windows.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Welke versies van HDInsight in Windows worden beïnvloed?
Azure HDInsight versie 3.3 is de laatste versie van HDInsight voor Windows. Voordat HDInsight op Windows is buiten gebruik gesteld, moet alle HDInsight-Windows-clusters versie 3.3 of lager worden gemigreerd naar HDInsight op Linux-versie 3.5 of hoger. Uw clusters migreren naar HDInsight op Linux, kunt u de mogelijkheid om te maken van nieuwe clusters of het formaat wijzigen van bestaande clusters behouden. 

### <a name="what-do-i-need-to-do"></a>Wat moet ik doen?
Uw HDInsight-Windows-clusters migreren naar een ondersteunde HDInsight Linux-cluster vóór 31 juli 2018. Meer informatie in de [migratiedocument door HDInsight](hdinsight-migrate-from-windows-to-linux.md). Zie voor meer informatie over Azure HDInsight-versies, de lijst met [ondersteunde versies](hdinsight-component-versioning.md#supported-hdinsight-versions). 

### <a name="where-do-i-find-the-cluster-os-type"></a>Waar vind ik het besturingssysteemtype cluster?
Ga naar de overzichtspagina van HDInsight-Cluster in Azure portal en zoek **clustertype** onder **Essentials**. De cluster-OS-typen worden weergegeven op de pagina. 

### <a name="i-cant-migrate-to-an-hdinsight-linux-cluster-by-july-31-2018-what-is-the-impact-to-my-hdinsight-windows-cluster"></a>Ik migreren niet naar een HDInsight Linux-cluster vóór 31 juli 2018. Wat zijn de gevolgen voor mijn HDInsight-Windows-cluster?
Het HDInsight-Windows-cluster wordt uitgevoerd als-is, maar u kunt een nieuw HDInsight-Windows-cluster maken of een bestaand HDInsight-Windows-cluster vergroten of verkleinen. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>Mijn cluster heeft een afhankelijkheid .NET. Hoe kan ik deze afhankelijkheid in Linux oplossen?
U kunt de afhankelijkheid van uw Linux-cluster oplossen met behulp van de [Mono project](http://www.mono-project.com/). Deze open-source implementatie van .NET is beschikbaar voor HDInsight Linux-clusters. Meer informatie in de [migratiedocument door HDInsight](hdinsight-migrate-from-windows-to-linux.md). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>Ik ben een nieuwe klant voor HDInsight in Windows. Hoe kan ik een HDInsight-Windows-cluster maken?
Vanaf 3 juni 2017, kunnen klanten met bestaande HDInsight Windows alleen nieuwe HDInsight Windows clusters maken. Nieuwe klanten kunnen niet een HDInsight-Windows-cluster maken in Azure portal met behulp van PowerShell of de SDK. Het wordt aangeraden om nieuwe klanten een Linux HDInsight-cluster maken. Bestaande klanten kunnen nieuwe HDInsight Windows clusters maken totdat de HDInsight op Windows vervaldatum. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-to-hdinsight-on-linux"></a>Is er een invloed op de prijzen die zijn gekoppeld aan het verplaatsen van HDInsight in Windows naar HDInsight op Linux?
Nee, de prijzen zijn hetzelfde voor HDInsight voor beide besturingssystemen. 

### <a name="what-are-the-customer-advantages-associated-with-the-move-to-only-using-hdinsight-on-linux"></a>Wat zijn de voordelen van de klant die zijn gekoppeld aan de overstap naar alleen met HDInsight in Linux?
* Snellere time-to-market voor open source big data-technologieën via de HDInsight-service
* Een grote gebruikerscommunity en ecosysteem voor ondersteuning
* Mogelijkheid om uit te oefenen ontwikkeling door de open-source-community voor Hadoop en andere big data-technologieën

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>Biedt HDInsight op Linux aanvullende functionaliteit naast wat beschikbaar in HDInsight op Windows is?
Beginnen met een HDInsight versie 3.4, heeft Microsoft HDInsight alleen op het besturingssysteem Linux uitgebracht. Als gevolg hiervan zijn enkele van de onderdelen in HDInsight uitsluitend beschikbaar voor Linux. Deze omvatten Apache Ranger, Kafka, Interactive Query, Spark, HDInsight-toepassingen, en Azure Data Lake Store als het primaire bestandssysteem. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Service level agreement voor versies van HDInsight-cluster
De service level agreement (SLA) wordt gedefinieerd in termen van een _ondersteuning venster_. Het venster ondersteuning is de periode gedurende welke een HDInsight-cluster-versie wordt ondersteund door Microsoft Customer Service and Support. Als de versie is een _ondersteuning voor de datum van afloop_ die is doorgegeven, is het HDInsight-cluster buiten het venster ondersteuning. Zie voor meer informatie over ondersteunde versies, de lijst met [ondersteunde versies van HDInsight-cluster](hdinsight-migrate-from-windows-to-linux.md). De vervaldatum voor de ondersteuning voor een opgegeven HDInsight versie X (nadat er een nieuwere X + 1-versie beschikbaar is) wordt berekend als de meest recente van:  

* Formule 1: Voeg 180 dagen toe aan de datum waarop de HDInsight-clusterversie X werd uitgebracht.
* Formule 2: 90 dagen toevoegen aan de datum waarop de HDInsight-clusterversie X + 1 beschikbaar in Azure portal is gemaakt.

De _vervaldatum_ is de datum waarna de clusterversie in HDInsight kan worden gemaakt. Vanaf 31 juli 2017, u kan niet de grootte van een HDInsight-cluster na de vervaldatum. 

> [!NOTE]
> HDInsight-Windows-clusters (inclusief versie 2.1, 3.0, 3.1, 3.2 en 3.3) worden uitgevoerd op Azure Guest OS Family versie 4, die gebruikmaakt van de 64-bits versie van Windows Server 2012 R2. Azure Guest OS Family versie 4 ondersteunt de versies van .NET Framework 4.0, 4.5 4.5.1 en 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks releaseopmerkingen die zijn gekoppeld aan het HDInsight-versies

De sectie bevat koppelingen naar de release-opmerkingen voor de Hortonworks Data Platform-distributies en de Apache-onderdelen die worden gebruikt met HDInsight.
* HDInsight-clusterversie 4.0 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html)
* HDInsight-clusterversie 3.6 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* HDInsight-clusterversie 3.5 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). HDInsight-clusterversie 3.5 is de _standaard_ Hadoop-cluster dat is gemaakt in de Azure-portal.
* HDInsight-clusterversie 3.4 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* HDInsight-clusterversie 3.3 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Opmerkingen bij de release van Apache Storm](https://storm.apache.org/2015/11/05/storm0100-released.html) zijn beschikbaar op de website van Apache.
  * [Opmerkingen bij de release Apache Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) zijn beschikbaar op de website van Apache.
* HDInsight-clusterversie 3.2 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.2][hdp-2-2].

  * Opmerkingen bij de release voor onderdelen die specifieke Apache beschikbaar zijn als volgt: [Hive 0,14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0,14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [algemene](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), en [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight-clusterversie 3.1 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. 3.1 HDInsight-clusters die zijn gemaakt vóór November, 7, 2014, zijn gebaseerd op [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight-clusterversie 3.0 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight-clusterversie 2.1 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight-clusterversie 1.6 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 1.1][hdp-1-1-0].






## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Standaard configuratie en de virtuele machine knooppuntgrootten voor clusters
De volgende tabellen worden de standaardgrootte van de virtuele machine (VM) voor HDInsight-clusters.

> [!IMPORTANT]
> Als u meer dan 32 worker-knooppunten in een cluster, moet u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB aan RAM-geheugen.
> 
> 

* Alle ondersteunde regio's met uitzondering van Brazilië-Zuid en Japan (West):

  | Clustertype | Hadoop | HBase | Interactive Query | Storm | Spark | ML-Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: standaard VM-grootte |D3 v2 |D3 v2 | D13, D14 |A4 v2 |D12 v2 |D12 v2 |
  | HEAD: aanbevolen VM-grootten |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |A4 versie 2, A8 versie 2, A2m versie 2 |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Werknemer: standaard VM-grootte |D3 v2 |D3 v2  | D13, D14 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Werknemer: aanbevolen VM-grootten |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | ZooKeeper: standaard VM-grootte | |A4 v2 | |A2 v2 | | |
  | ZooKeeper: aanbevolen VM-grootten | |A4 versie 2, A8 versie 2, A2m versie 2 | | A2 versie 2, A4 versie 2, A8 versie 2 | | |
  | Edge: standaard VM-grootte | | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Edge: aanbevolen VM-grootte | | | | | |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Brazilië-Zuid en Japan-West alleen (geen v2-grootte):

  | Clustertype | Hadoop | HBase | Interactive Query |Storm | Spark | ML-Service |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: standaard VM-grootte |D3 |D3  | D13, D14 |A3 |D12 |D12 |
  | HEAD: aanbevolen VM-grootten |D3, D4, D12 |D3, D4, D12  | D13, D14 |A3, A4, A5 |D12, D13, D14 |D12, D13, D14 |
  | Werknemer: standaard VM-grootte |D3 |D3  | D13, D14 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Werknemer: aanbevolen VM-grootten |D3, D4, D12 |D3, D4, D12  | D13, D14 |D3, D4, D12 |Windows: D12, D13, D14; Linux: D4, D13, D12 D14 |Windows: D12, D13, D14; Linux: D4, D13, D12 D14 |
  | ZooKeeper: standaard VM-grootte | |A2 | | A2 | | |
  | ZooKeeper: aanbevolen VM-grootten | |A2, A3, A4 | |A2, A3, A4 | | |
  | Edge: standaard VM-grootten | | | | | |Windows: D12; Linux: D4 |
  | Edge: aanbevolen VM-grootten | | | | | |Windows: D12, D13, D14; Linux: D4, D13, D12 D14 |

> [!NOTE]
> - HEAD staat bekend als *Nimbus* voor de Storm-cluster type.
> - Werknemer staat bekend als *Supervisor* voor de Storm-cluster type.
> - Werknemer staat bekend als *regio* voor het HBase-cluster type.

## <a name="next-steps"></a>Volgende stappen
- [Instellingen voor Hadoop, Spark- en meer informatie over de HDInsight-cluster](hdinsight-hadoop-provision-linux-clusters.md)
- [Werken in Hadoop op HDInsight vanaf een Windows-PC](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
