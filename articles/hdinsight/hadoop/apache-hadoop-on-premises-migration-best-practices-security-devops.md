---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - beveiligings- en aanbevolen procedures voor DevOps
description: Meer informatie over beveiliging en DevOps-procedures voor migratie on-premises Hadoop-clusters op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 1a1cf731678ef7678b740020a4d61725f9a2b32a
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221885"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - beveiligings- en aanbevolen procedures voor DevOps

In dit artikel biedt aanbevelingen voor beveiliging en DevOps in Azure HDInsight-systemen. Het onderdeel van een serie die biedt best practices om u te helpen migreren on-premises Apache Hadoop-systemen tot Azure HDInsight.

## <a name="use-the-enterprise-security-package-to-secure-and-govern-the-cluster"></a>De Enterprise-beveiligingspakket gebruikt om te beveiligen en beheren van het cluster

Enterprise Security Package (ESP) biedt ondersteuning voor verificatie op basis van Active Directory, ondersteuning voor meerdere gebruikers en rollen gebaseerd toegangsbeheer. Met de gekozen ESP optie HDInsight-cluster is toegevoegd aan het Active Directory-domein en de enterprise-beheerder op rollen gebaseerd toegangsbeheer (RBAC) voor Hive-beveiliging met behulp van Apache Ranger kunt configureren. De beheerder kan ook de toegang tot de gegevens door werknemers en eventuele wijzigingen worden aangebracht aan het beleid voor toegangsbeheer controleren.

ESP-functies zijn momenteel in preview en zijn alleen beschikbaar op de volgende clustertypen: Apache Hadoop, Apache Spark, Apache HBase, Apache Kafka en Apache Interactive Query.

Gebruik de volgende stappen uit om de aan domein gekoppelde HDInsight-cluster te implementeren:

- Azure Active Directory (AAD) implementeren door door te geven van de domeinnaam
- Azure Active Directory Domain Services (AAD DS) implementeren
- De vereiste Virtueelnetwerk en subnet maken
- Een virtuele machine in het Virtueelnetwerk voor het beheren van AAD DS implementeren
- De virtuele machine toevoegen aan het domein
- Installatie van AD en hulpprogramma's voor DNS
- Laat de AAD-DS-beheerder maken van een organisatie-eenheid (OE)
- LDAPS voor AAD DS inschakelen
- Maak een service-account in Azure Active Directory met gedelegeerde lezen en schrijven-beheerder toestemming voor de organisatie-eenheid, zodat deze kan. Deze serviceaccount kan vervolgens machines toevoegen aan het domein en plaats machine principals binnen de organisatie-eenheid. Het kan ook service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.

    > [!Note]
    > Het serviceaccount hoeft niet te worden beheerdersaccount van AD-domein

- ESP HDInsight-cluster implementeren door in te stellen van de volgende parameters:
    - **Domeinnaam**: de naam van het domein dat is gekoppeld aan Azure AD DS.
    - **Domeingebruikersnaam**: het serviceaccount in de Azure AD DS-domeincontroller beheerd domein die u in de vorige sectie hebt gemaakt, bijvoorbeeld: `hdiadmin@contoso.onmicrosoft.com`. In dit de domeingebruiker is de beheerder van dit HDInsight-cluster.
    - **Domeinwachtwoord**: het wachtwoord van het serviceaccount.
    - **Organisatie-eenheid**: de DN-naam van de organisatie-eenheid die u wilt gebruiken met het HDInsight-cluster, bijvoorbeeld: `OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com`. Als deze organisatie-eenheid niet bestaat, wordt het HDInsight-cluster probeert te maken van de organisatie-eenheid met behulp van de bevoegdheden van het serviceaccount.
    - **LDAPS-URL**: bijvoorbeeld `ldaps://contoso.onmicrosoft.com:636`.
    - **Gebruikersgroep openen**: de beveiligingsgroepen waarvan gebruikers die u wilt synchroniseren met het cluster, bijvoorbeeld: `HiveUsers`. Als u meerdere gebruikersgroepen opgeven wilt, gescheiden door puntkomma (;). De groep(en) moeten zich in de map voordat het ESP-cluster te maken.

Raadpleeg voor meer informatie de volgende artikelen:

- [Een inleiding tot Hadoop-beveiliging met aan domein gekoppelde HDInsight-clusters](../domain-joined/apache-domain-joined-introduction.md)
- [Azure domein Hadoop-clusters in HDInsight plannen](../domain-joined/apache-domain-joined-architecture.md)
- [Configureren van een domein gekoppeld HDInsight-cluster met behulp van Azure Active Directory Domain Services](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)
- [Synchronisatie van Azure Active Directory-gebruikers met een HDInsight-cluster](../hdinsight-sync-aad-users-to-cluster.md)
- [Hive-beleidsregels configureren in aan domein gekoppelde HDInsight](../domain-joined/apache-domain-joined-run-hive.md)
- [Apache Oozie in aan domein gekoppelde HDInsight Hadoop clusters uitvoeren](../domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)

## <a name="implement-end-to-end-enterprise-security-management"></a>End-to-enterprise security management implementeren

End-to-end bedrijfsbeveiliging kan worden bereikt met behulp van de volgende:

- **Privé en beschermd gegevenspijplijn** (perimeterbeveiliging niveau):
    - Perimeter-level Security kan worden bereikt via Azure Virtual Networks, Network Security Groups en Gateway-service

- **Verificatie en autorisatie voor toegang tot gegevens**
    - Domein HDI-cluster met behulp van Azure Active Directory Domain Services maken. (Enterprise-beveiligingspakket)
    - Ambari gebruiken op basis van rollen om toegang te bieden tot clusterresources voor AD-gebruikers
    - Gebruik Apache Ranger om in te stellen toegang beheren beleid voor Hive op de tabel / kolom / rijniveau.
    - SSH-toegang tot het cluster kan worden beperkt tot alleen de beheerder.

- **Controle**
    - Weergeven en rapporteren van alle toegang tot het HDInsight-clusterresources en gegevens.
    - Weergeven en rapporteren van alle wijzigingen aan het beleid voor toegangsbeheer

- **Versleuteling**
    - Transparante versleuteling van Server-Side met behulp van Microsoft beheerde sleutels of door de klant beheerde sleutels.
    - Versleuteling tijdens overdracht met behulp van versleuteling van de Client-Side-, https- en TLS

Raadpleeg voor meer informatie de volgende artikelen:

- [Overzicht van de virtuele netwerken van Azure](../../virtual-network/virtual-networks-overview.md)
- [Overzicht van Azure Network Security Groups](../../virtual-network/security-overview.md)
- [Azure Virtual Network-peering](../../virtual-network/virtual-network-peering-overview.md)
- [Azure storage-beveiligingshandleiding](../../storage/common/storage-security-guide.md)
- [Azure Storage-Service-versleuteling ' at rest '](../../storage/common/storage-service-encryption.md)

## <a name="use-monitoring--alerting"></a>Gebruik bewaking en waarschuwingen

Zie het artikel voor meer informatie:

[Overzicht van Azure Monitor](../../azure-monitor/overview.md)

## <a name="upgrade-clusters"></a>Bijwerken van clusters

Regelmatig bijwerken naar de nieuwste versie van HDInsight om te profiteren van de nieuwste functies. De volgende stappen kunnen worden gebruikt voor het cluster upgraden naar de nieuwste versie:

- Maak een nieuwe TEST HDI-cluster met behulp van de meest recente beschikbare HDI-versie.
- Testen op het nieuwe cluster om ervoor te zorgen dat de taken en werkbelastingen werken zoals verwacht.
- Taken of toepassingen of workloads zo nodig wijzigen.
- Back-up van alle tijdelijke gegevens die lokaal zijn opgeslagen op de clusterknooppunten.
- Het bestaande cluster verwijderen.
- Een van de meest recente versie van HDInsight-cluster maken in het hetzelfde VNET-subnet, met behulp van de dezelfde gegevens en metagegevens standaardopslag als de vorige cluster.
- Importeer alle tijdelijke gegevens die back-up is gemaakt.
- Start taken/doorgaan met het verwerken met behulp van het nieuwe cluster.

Zie het artikel voor meer informatie: [Upgrade HDInsight-cluster naar een nieuwe versie](../hdinsight-upgrade-cluster.md)

## <a name="patch-cluster-operating-systems"></a>Patch voor cluster-besturingssystemen

HDInsight zorgt patchen van het besturingssysteem van de virtuele machines die door HDInsight-clusters worden gebruikt als een beheerde Hadoop-service.

Zie het artikel voor meer informatie: [OS patches voor HDInsight](../hdinsight-os-patching.md)

## <a name="post-migration"></a>Na de migratie

1. **Toepassingen herstellen** - iteratief Breng de benodigde wijzigingen aan de taken, processen en scripts
2. **Uitvoeren van Tests** - iteratief uitvoeren functionele en prestaties testen
3. **Optimaliseren** : los eventuele problemen met prestaties met op basis van de resultaten van de bovenstaande en vervolgens testen om te bevestigen dat de prestatieverbeteringen.

## <a name="appendix-gathering-details-to-prepare-for-a-migration"></a>Bijlage: verzamelen van gegevens om voor te bereiden voor migratie

Deze sectie bevat sjabloon vragenlijsten zodat belangrijke informatie verzamelen over:

- De on-premises implementatie.
- Details van het project.
- Vereisten voor Azure.

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
|HDI vs HDP op IaaS?|HDI||

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [HDInsight 4.0](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-introduction)