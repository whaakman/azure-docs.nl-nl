---
title: Hadoop-onderdelen en versies - Azure HDInsight | Microsoft Docs
description: Informatie over de onderdelen en -versies in HDInsight en het niveau van de service beschikbaar in deze cloud-distributie van Hortonworks Data Platform.
keywords: hadoop-versies, hadoop-ecosysteem onderdelen, hadoop-onderdelen, het controleren van hadoop-versie
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: ''
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: bprakash
ms.openlocfilehash: 813b05c5be4378ea6e57cb0b14fd8110e2772998
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="what-are-the-hadoop-components-and-versions-available-with-hdinsight"></a>Wat zijn de onderdelen en versies die beschikbaar met HDInsight?

Meer informatie over de Apache Hadoop-ecosysteem onderdelen en -versies in Microsoft Azure HDInsight, evenals het beveiligingspakket Enterprise. Ook informatie over het controleren van component-versies van Hadoop in HDInsight. 

Elke versie HDInsight is een cloud-distributiepunt van een versie van Hortonworks Data Platform HDP ().

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Hadoop-onderdelen met verschillende versies van HDInsight
Azure HDInsight biedt ondersteuning voor meerdere versies van Hadoop-cluster die op elk gewenst moment kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de distributie HDP en een reeks onderdelen die deel uitmaken van dit distributiepunt. Vanaf 17 februari 2017 de standaardversie van de cluster die wordt gebruikt door Azure HDInsight 3.5 is en is gebaseerd op HDP 2.5.

De onderdeel-versies die zijn gekoppeld aan de versies van HDInsight-cluster worden weergegeven in de volgende tabel: 

> [!NOTE]
> De standaardversie voor het HDInsight-service kan zonder voorafgaande kennisgeving worden gewijzigd. Als u een afhankelijkheid versie, de HDInsight-versie opgeven wanneer u clusters met de .NET SDK met Azure PowerShell en Azure CLI maken.

| Onderdeel | HDInsight 3.6 (standaard) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | 3.2 voor HDInsight | HDInsight 3.1 | HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Data Platform |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop en YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive en HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0 (alleen voor Linux) |1.6.2 + 2.0 (alleen voor Linux) |1.6.0 (alleen voor Linux) |1.5.2 (alleen experimentele build Linux) |1.3.1 (alleen Windows) |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>Controleren op huidige Hadoop component versie-informatie

De Hadoop-ecosysteem onderdeel versies die zijn gekoppeld aan de versies van HDInsight-cluster kunnen wijzigen met updates in HDInsight. Om te controleren van de Hadoop-onderdelen en om te controleren welke versies worden gebruikt voor een cluster, gebruikt u de Ambari REST-API. De **GetComponentInformation** opdracht haalt informatie over de onderdelen van service. Zie voor meer informatie de [Ambari documentatie][ambari-docs].

Voor Windows-clusters, wordt een andere manier om te controleren van de onderdeelversie is het Meld u aan bij een cluster met behulp van extern bureaublad en bekijkt u de inhoud van de map C:\apps\dist\.

> [!IMPORTANT]
> Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.4 of hoger. Zie voor meer informatie [buiten gebruik stellen van Windows op HDInsight](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Releaseopmerkingen

Zie [HDInsight releaseopmerkingen](hdinsight-release-notes.md) voor extra release-opmerkingen op de nieuwste versies van HDInsight.

## <a name="supported-hdinsight-versions"></a>Ondersteunde versies van HDInsight
De volgende tabellen worden de versies van HDInsight. De HDP-versies die met elke versie HDInsight overeenkomen worden samen met de release product datums weergegeven. De datums voor verlopen en buiten gebruik stellen worden ook gegeven wanneer deze zijn bepaald.

### <a name="available-versions"></a>Beschikbare versies

De volgende tabel geeft de versies van HDInsight die beschikbaar in de Azure Portal, evenals andere implementatiemethoden zoals PowerShell en .NET SDK zijn.

| HDInsight-versie | HDP versie | VM OS | Releasedatum | Ondersteuning voor vervaldatum | Vervaldatum | Hoge beschikbaarheid |  Beschikbaarheid in de Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |4 april 2017 | | |Ja |Ja |
| HDInsight 3.5 |HDP 2,5 |Ubuntu 16.0.4 LTS |30 september 2016 |5 september 2017 |31 mei 2018 |Ja |Ja |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 december 2015 |27 juni 2016 |31 juli 2018 |Ja |Nee |

> [!NOTE]
> Na de ondersteuning voor een versie is verlopen, deze mogelijk niet beschikbaar is via de Microsoft Azure portal. Clusterversies blijven echter zijn beschikbaar via de `Version` parameter in de Windows PowerShell [nieuw AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) opdracht en de .NET SDK totdat de einddatum van de versie.
>

### <a name="retired-versions"></a>Buiten gebruik gestelde versies

De volgende tabel bevat de versies van HDInsight die zijn **niet** beschikbaar in de Azure-Portal.

| HDInsight-versie | HDP versie | VM OS | Releasedatum | Ondersteuning voor vervaldatum | Vervaldatum | Hoge beschikbaarheid |  Beschikbaarheid in de Azure portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.4 |2.4 HDP |Ubuntu 14.0.4 TNS |29 maart 2016 |29 december 2016 |9 januari 2018 |Ja |Nee |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 TNS |2 december 2015 |27 juni 2016 |31 juli 2017 |Ja |Nee |
| 3.2 voor HDInsight |HDP 2.2 |Ubuntu 12.04 TNS of Windows Server 2012 R2 |18 februari 2015 |1 maart 2016 |1 april 2017 |Ja |Nee |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |24 juni 2014 |18 mei 2015 |30 juni 2016 |Ja |Nee |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |11 februari 2014 |17 september 2014 |30 juni 2015 |Ja |Nee |
| HDInsight 2.1 |1.3 HDP |Windows Server 2012 R2 |28 oktober 2013 |12 mei 2014 |31 mei 2015 |Ja |Nee |
| HDInsight 1.6 |HDP 1.1 | |28 oktober 2013 |26 april 2014 |31 mei 2015 |Nee |Nee |

> [!NOTE]
> Maximaal beschikbare clusters met twee hoofdknooppunten worden standaard voor HDInsight versie 2.1 en hoger geïmplementeerd. Ze zijn niet beschikbaar voor HDInsight versie 1.6-clusters.

## <a name="enterprise-security-package-for-hdinsight"></a>Enterprise-beveiligingspakket voor HDInsight

Enterprise-Security is een optioneel pakket die u op uw HDInsight-cluster als onderdeel van de cluster-werkstroom maken toevoegen kunt. Het beveiligingspakket Enterprise ondersteunt:

- Integratie met Active Directory voor verificatie.

    U kunt alleen in het verleden HDInsight-clusters maken met een gebruiker met lokale beheerdersrechten en een lokale SSH-gebruiker gebruikt. De gebruiker met lokale beheerdersrechten toegang tot de bestanden, mappen, tabellen en kolommen.  U kunt met het beveiligingspakket Enterprise toegangsbeheer op basis van rollen inschakelen door het HDInsight-clusters integreren met uw eigen Active Directory, zoals lokale Active Directory, Azure Active Directory Domain Services of Active Directory op IaaS virtuele machine. De domeinbeheerder op het cluster kan gebruikers hun eigen (bedrijfsdomein)-gebruikersnaam en wachtwoord gebruiken voor toegang tot het cluster verlenen. 

    Zie voor meer informatie:

    - [Een inleiding tot Hadoop-beveiliging met HDInsight-clusters domein](./domain-joined/apache-domain-joined-introduction.md)
    - [Azure domein Hadoop-clusters in HDInsight plannen](./domain-joined/apache-domain-joined-architecture.md)
    - [Domein-sandbox-omgeving configureren](./domain-joined/apache-domain-joined-configure.md)
    - [Domein-HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autorisatie voor gegevens

    - Integratie met Apache Zwerver voor autorisatie voor Hive, Spark SQL en Yarn-wachtrijen.
    - U kunt toegangsbeheer voor bestanden en mappen instellen.

    Zie voor meer informatie:

    - [Op domein HDInsight Hive-beleid configureren](./domain-joined/apache-domain-joined-run-hive.md)

- De controlelogboeken voor toegang tot een monitor en het geconfigureerde beleid weergeven. 

### <a name="supported-cluster-types"></a>Ondersteunde clustertypen

De volgende clustertypen ondersteunen momenteel het beveiligingspakket Enterprise:

- Hadoop (alleen HDInsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-store"></a>Ondersteuning voor Azure Data Lake Store

De Enterprise-beveiligingspakket ondersteunt het gebruik van Azure Data Lake Store als zowel de primaire opslag en de opslag van de invoegtoepassing.

### <a name="pricing-and-sla"></a>Prijzen en SLA
Zie voor informatie over de prijzen en SLA het beveiligingspakket Enterprise, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hdinsight-windows-retirement"></a>Buitengebruikstelling van HDInsight Windows
Microsoft Azure HDInsight versie 3.3 is de laatste versie van HDInsight in Windows. De intrekkingsdatum voor HDInsight in Windows is 31 juli 2018. Als u een HDInsight-clusters op Windows 3.3 of eerder hebt, moet u voordat 31 juli 2018 HDInsight op Linux (HDInsight versie 3.5 of nieuwer) migreren. Migreren naar de Linux-besturingssysteem, kunt u de mogelijkheid om te maken of het formaat van uw HDInsight-clusters niet behouden. Ondersteuning voor HDInsight versie 3.3 in Windows is verlopen op 27 juni 2016.

Beginnen met HDInsight versie 3.4, heeft Microsoft HDInsight alleen op de Linux-besturingssysteem. Hierdoor kunnen zijn sommige van de onderdelen in HDInsight beschikbaar voor Linux alleen. Deze omvatten Apache Zwerver, Kafka, interactieve Query, Spark, HDInsight-toepassingen en Azure Data Lake Store als de primaire-bestandssysteem. Toekomstige versies van HDInsight zijn alleen beschikbaar voor de Linux-besturingssysteem. Er zijn geen toekomstige versies van HDInsight in Windows. 

## <a name="faqs"></a>Veelgestelde vragen

### <a name="what-is-the-timeline-for-retiring-hdinsight-on-windows"></a>Wat is de planning voor het buiten gebruik stellen HDInsight op Windows?
31 juli 2018, is de intrekkingsdatum voor HDInsight in Windows. Als de geplande intrekkingsdatum voor uw regio verschilt, wordt u afzonderlijk gewaarschuwd. 

### <a name="what-is-the-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>Wat zijn de gevolgen van het buiten gebruik stellen van HDInsight in Windows voor bestaande klanten?
Nadat HDInsight op Windows is buiten gebruik gesteld, kan u een nieuw HDInsight Windows-cluster maken of vergroten of verkleinen van een bestaand HDInsight Windows-cluster. Ondersteuning voor HDInsight versie 3.3 verlopen op 27 juni 2016. Er is daarom geen ondersteuning of oplossingen voor problemen voor HDInsight 3.3 of eerdere versies. Toekomstige versies van HDInsight zijn alleen beschikbaar voor de Linux-besturingssysteem. Er zijn geen toekomstige versies van HDInsight in Windows.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Welke versies van HDInsight op Windows optreedt?
Azure HDInsight versie 3.3 is de laatste versie van HDInsight voor Windows. Voordat HDInsight op Windows is buiten gebruik gesteld, moet alle HDInsight Windows-clusters versie 3.3 of eerder worden gemigreerd naar HDInsight op Linux versie 3.5 of hoger. Uw clusters migreren naar HDInsight op Linux, kunt u de mogelijkheid om te maken van nieuwe clusters of het formaat van de bestaande clusters niet behouden. 

### <a name="what-do-i-need-to-do"></a>Wat moet ik doen?
Uw HDInsight Windows-clusters migreren naar een ondersteunde HDInsight Linux-cluster voordat 31 juli 2018. Meer informatie in de [HDInsight migratie document](hdinsight-migrate-from-windows-to-linux.md). Zie voor meer informatie over Azure HDInsight-versies, de lijst met [ondersteunde versies](hdinsight-component-versioning.md#supported-hdinsight-versions). 

### <a name="where-do-i-find-the-cluster-os-type"></a>Waar vind ik het BS-type van het cluster
Ga naar de overzichtspagina van HDInsight-Cluster in de Azure-portal en zoek **type Cluster** onder **Essentials**. De cluster-OS-typen worden weergegeven op die pagina. 

### <a name="i-cant-migrate-to-an-hdinsight-linux-cluster-by-july-31-2018-what-is-the-impact-to-my-hdinsight-windows-cluster"></a>Ik migreren niet naar een HDInsight Linux-cluster door 31 juli 2018. Wat is het van invloed op mijn HDInsight Windows-cluster?
Het HDInsight Windows-cluster wordt uitgevoerd als-is, maar u kunt een nieuw HDInsight Windows-cluster maken of een bestaand HDInsight Windows-cluster vergroten of verkleinen. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>Mijn cluster heeft een .NET-afhankelijkheid. Hoe kan ik deze afhankelijkheid op Linux oplossen?
U kunt uw Linux-cluster afhankelijkheid oplossen met behulp van de [Mono project](http://www.mono-project.com/). Deze implementatie open source van .NET is beschikbaar voor HDInsight Linux-clusters. Meer informatie in de [HDInsight migratie document](hdinsight-migrate-from-windows-to-linux.md). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>Ik ben een nieuwe klant voor HDInsight in Windows. Hoe kan ik een HDInsight Windows-cluster maken?
Vanaf 3 juli 2017 kunnen alleen bestaande HDInsight Windows-klanten nieuwe HDInsight Windows clusters maken. Nieuwe klanten kunnen een HDInsight Windows-cluster maken in de Azure-portal met behulp van PowerShell of de SDK. U kunt het beste nieuwe klanten een Linux-HDInsight-cluster maken. Bestaande klanten kunnen nieuwe HDInsight Windows clusters maken totdat de HDInsight op Windows intrekkingsdatum. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-to-hdinsight-on-linux"></a>Is er een prijscategorie invloed die zijn gekoppeld aan het verplaatsen van HDInsight in Windows naar HDInsight op Linux?
Nee, is hetzelfde voor HDInsight op beide OS prijzen. 

### <a name="what-are-the-customer-advantages-associated-with-the-move-to-only-using-hdinsight-on-linux"></a>Wat zijn de voordelen van de klant die zijn gekoppeld aan de overstap naar alleen met HDInsight op Linux?
* Snellere tijd op de markt voor open source big data-technologieën via de HDInsight-service
* Een grote community en ecosysteem voor ondersteuning
* Mogelijkheid om uit te oefenen active ontwikkeld door de open source-community voor Hadoop en andere technologieën voor big data

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>Biedt HDInsight op Linux aanvullende functionaliteit naast wat beschikbaar in HDInsight op Windows is?
Beginnen met HDInsight versie 3.4, heeft Microsoft HDInsight alleen op de Linux-besturingssysteem. Hierdoor kunnen zijn sommige van de onderdelen in HDInsight beschikbaar voor Linux alleen. Deze omvatten Apache Zwerver, Kafka, interactieve Query, Spark, HDInsight-toepassingen en Azure Data Lake Store als de primaire-bestandssysteem. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Serviceovereenkomst voor versies van HDInsight-cluster
Service level agreement (SLA) is gedefinieerd in termen van een _ondersteuning venster_. Het venster ondersteuning is de periode dat een HDInsight-cluster versie wordt ondersteund door de klantenservice van Microsoft. Als de versie heeft een _ondersteunen vervaldatum_ die is doorgegeven, is het HDInsight-cluster buiten het venster voor ondersteuning. Zie voor meer informatie over ondersteunde versies, de lijst met [ondersteunde versies van HDInsight-cluster](hdinsight-migrate-from-windows-to-linux.md). De vervaldatum van ondersteuning voor een opgegeven HDInsight versie X (nadat er een nieuwere versie van de X + 1 beschikbaar is) wordt berekend als de meest recente van:  

* Formule 1: 180 dagen toevoegen aan de datum waarop de HDInsight-cluster versie X is uitgebracht.
* Formule 2: 90 dagen toevoegen aan de datum waarop de HDInsight-cluster versie X + 1 beschikbaar in Azure portal is gemaakt.

De _intrekkingsdatum_ is de datum waarna de versie van het cluster in HDInsight kan niet worden gemaakt. Vanaf 31 juli 2017 u kan niet de grootte van een HDInsight-cluster na de datum buiten gebruik stellen. 

> [!NOTE]
> HDInsight Windows-clusters (inclusief versie 2.1, 3.0, 3.1, 3.2 en 3.3) worden uitgevoerd op Azure Gast OS-familie versie 4, die gebruikmaakt van de 64-bits versie van Windows Server 2012 R2. Azure Gast OS-familie versie 4 ondersteunt de .NET Framework versie 4.0, 4.5 4.5.1 en 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks release-opmerkingen die zijn gekoppeld aan de versies van HDInsight

De sectie bevat koppelingen naar de release-opmerkingen voor de Hortonworks Data Platform distributies en de Apache-onderdelen die worden gebruikt met HDInsight.
* HDInsight-cluster versie 3.6 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* HDInsight-cluster versie 3.5 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). HDInsight-cluster versie 3.5 is de _standaard_ Hadoop-cluster dat is gemaakt in de Azure-portal.
* HDInsight-cluster versie 3.4 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* HDInsight-cluster versie 3.3 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Opmerkingen bij de release van Apache Storm](https://storm.apache.org/2015/11/05/storm0100-released.html) zijn beschikbaar op de Apache-website.
  * [Apache Hive releaseopmerkingen](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) zijn beschikbaar op de Apache-website.
* HDInsight-cluster versie 3.2 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.2][hdp-2-2].

  * Releaseopmerkingen voor specifieke onderdelen van Apache beschikbaar zijn als volgt: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [algemene](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm-0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), en [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight-cluster versie 3.1 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. 3.1 HDInsight-clusters die zijn gemaakt vóór November, 7, 2014, zijn gebaseerd op [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight-cluster versie 3.0 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight-cluster versie 2.1 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight-cluster versie 1.6 maakt gebruik van een Hadoop-distributie die is gebaseerd op [Hortonworks Data Platform 1.1][hdp-1-1-0].






## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Standaard configuratie en de virtuele machine knooppuntgrootten voor clusters
De volgende tabellen worden de standaardgrootte van de virtuele machine (VM) voor HDInsight-clusters.

> [!IMPORTANT]
> Als u meer dan 32 worker-knooppunten in een cluster, moet u een grootte van het hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen.
> 
> 

* Alle ondersteunde regio's, behalve Brazilië-Zuid en Japan-West:

  | Clustertype | Hadoop | HBase | Interactive Query | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: standaard VM-grootte |D3 v2 |D3 v2 | D13, D14 |A3 |D12 v2 |D12 v2 |
  | HEAD: aanbevolen VM-grootten |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |A3, A4, A5 |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Worker: standaard VM-grootte |D3 v2 |D3 v2  | D13, D14 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Werknemer: aanbevolen VM-grootten |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | ZooKeeper: standaard VM-grootte | |A3 | |A2 | | |
  | ZooKeeper: aanbevolen VM-grootten | |A3, A4, A5 | | A2, A3, A4 | | |
  | Rand: standaard VM-grootte | | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Rand: aanbevolen VM-grootte | | | | | |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Brazilië-Zuid en Japan-West alleen (geen grootten v2):

  | Clustertype | Hadoop | HBase | Interactive Query |Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: standaard VM-grootte |D3 |D3  | D13, D14 |A3 |D12 |D12 |
  | HEAD: aanbevolen VM-grootten |D3, D4, D12 |D3, D4, D12  | D13, D14 |A3, A4, A5 |D12, D13, D14 |D12, D13, D14 |
  | Worker: standaard VM-grootte |D3 |D3  | D13, D14 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Werknemer: aanbevolen VM-grootten |D3, D4, D12 |D3, D4, D12  | D13, D14 |D3, D4, D12 |Windows: D12, D13 D14; Linux: D4, D12, D13 D14 |Windows: D12, D13 D14; Linux: D4, D12, D13 D14 |
  | ZooKeeper: standaard VM-grootte | |A2 | | A2 | | |
  | ZooKeeper: aanbevolen VM-grootten | |A2, A3, A4 | |A2, A3, A4 | | |
  | Rand: standaard VM-grootten | | | | | |Windows: D12; Linux: D4 |
  | Rand: aanbevolen VM-grootten | | | | | |Windows: D12, D13 D14; Linux: D4, D12, D13 D14 |

> [!NOTE]
> - HEAD staat bekend als *Nimbus* type voor de Storm-cluster.
> - Werknemer staat bekend als *Supervisor* type voor de Storm-cluster.
> - Werknemer staat bekend als *regio* type voor de HBase-cluster.

## <a name="next-steps"></a>Volgende stappen
- [Setup voor Hadoop, Spark en meer informatie over de HDInsight-cluster](hdinsight-hadoop-provision-linux-clusters.md)
- [Werken in Hadoop in HDInsight via Windows-PC](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
