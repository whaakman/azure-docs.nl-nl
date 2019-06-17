---
title: Azure HDInsight 3.6 Hive-workloads migreren naar HDInsight 4.0
description: Leer hoe u Apache Hive-workloads in HDInsight 3.6 migreren naar HDInsight 4.0.
ms.service: hdinsight
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.topic: howto
ms.date: 04/24/2019
ms.openlocfilehash: b39279e560cb1738ff9b33ec587562efd2ed4e8d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65800956"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Azure HDInsight 3.6 Hive-workloads migreren naar HDInsight 4.0

Dit document laat zien hoe u Apache Hive en LLAP workloads op HDInsight 3.6 migreren naar HDInsight 4.0. HDInsight 4.0 biedt nieuwere Hive en LLAP functies zoals gerealiseerde weergaven en query resultaten in cache opslaan. Wanneer u uw workloads op HDInsight 4.0 migreert, kunt u veel nieuwe functies van Hive 3 die niet beschikbaar zijn in HDInsight 3.6.

In dit artikel bevat informatie over de volgende onderwerpen:

* Migratie van Hive-metagegevens naar HDInsight 4.0
* Veilige migratie van ACID- en niet-ACID-tabellen
* Behoud van het beveiligingsbeleid van Hive in HDInsight-versies
* Uitvoeren van query's en foutopsporing van HDInsight 3.6 HDInsight 4.0

## <a name="migrate-apache-hive-metadata-to-hdinsight-40"></a>Apache Hive-metagegevens migreren naar HDInsight 4.0

Een voordeel van Hive is de mogelijkheid om metagegevens te exporteren naar een externe database (aangeduid als de Hive-Metastore). De **Hive-Metastore** is verantwoordelijk voor het opslaan van de tabelstatistieken, met inbegrip van de opslaglocatie van de tabel, kolomnamen en tabelgegevens index. Het databaseschema metastore verschilt voor de Hive-versies. Voer het volgende om door te upgraden van een HDInsight 3.6 Hive-Metastore zodat deze compatibel is met HDInsight 4.0.

1. Maak een nieuwe kopie van uw externe metastore. HDInsight 3.6 en HDInsight 4.0 vereist verschillende metastore schema's en een enkele metastore niet delen. Zie [externe metagegevensopslag in Azure HDInsight gebruiken](../hdinsight-use-external-metadata-stores.md) voor meer informatie over het koppelen van een externe metastore met een HDInsight-cluster. 
2. Start een scriptactie op basis van het HDI 3.6-cluster met 'Hoofdknooppunten' als het knooppunttype voor de uitvoering. Plak de volgende URI in het tekstvak 'Bash-Script URI' gemarkeerd: https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh. Voer in het tekstvak gemarkeerd als 'Argumenten', de servernaam, -database, gebruikersnaam en wachtwoord voor de **gekopieerd** Hive-metastore, gescheiden door spaties. Maak daarbij dan geen '. database.windows.net "wanneer u de servernaam opgeeft.

> [!Warning]
> De upgrade die het metagegevensschema HDInsight 3.6 wordt omgezet in het HDInsight-4.0-schema kan niet ongedaan worden gemaakt.

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Migreren van Hive-tabellen naar HDInsight 4.0

Na het voltooien van de voorgaande reeks stappen voor het migreren van de Hive-Metastore naar HDInsight 4.0, de tabellen en de databases die zijn opgenomen in de metastore zichtbaar zijn vanaf binnen het cluster voor HDInsight 4.0 door het uitvoeren van `show tables` of `show databases` van binnen het cluster . Zie [tot uitvoering van query's uitvoeren voor HDInsight-versies](#query-execution-across-hdinsight-versions) voor informatie over de uitvoering van de query in HDInsight 4.0-clusters.

De werkelijke gegevens uit de tabellen, maar is niet toegankelijk totdat het cluster toegang tot de benodigde storage-accounts heeft. Als u wilt controleren of dat uw 4.0 HDInsight-cluster toegang tot dezelfde gegevens als het oude cluster met HDInsight 3.6, voert u de volgende stappen uit:

1. Bepalen van de Azure storage-account van de tabel of een database met behulp van beschrijving geformatteerd.
2. Als uw cluster HDInsight 4.0 wordt al uitgevoerd, moet u de Azure storage-account koppelen aan het cluster via Ambari. Als u nog de 4.0 HDInsight-cluster hebt gemaakt, zorg er dan voor dat de Azure storage-account is opgegeven als de primaire of een secundaire cluster storage-account. Zie voor meer informatie over het toevoegen van storage-accounts met HDInsight-clusters [extra opslagaccounts toevoegen aan HDInsight](../hdinsight-hadoop-add-storage.md).

> [!Note]
> Tabellen zijn anders worden behandeld in HDInsight 3.6 en HDInsight 4.0. Daarom moet delen u de dezelfde tabellen voor clusters van verschillende versies niet. Als u gebruiken van HDInsight 3.6 op hetzelfde moment als HDInsight 4.0 wilt, moet u afzonderlijke exemplaren van de gegevens voor elke versie hebben.

Uw Hive-workloads, omvat mogelijk een combinatie van ACID- en niet-ACID-tabellen. Één belangrijk verschil tussen Hive in HDInsight 3.6 (Hive-2) en Hive in HDInsight 4.0 (Hive-3) is ACID-naleving voor tabellen. In HDInsight 3.6, Hive ACID-naleving inschakelen is aanvullende configuratie vereist, maar in HDInsight 4.0 tabellen ACID-compatibel zijn standaard. De enige actie die vereist is voordat de migratie is een belangrijke compressie op basis van de ACID-tabel op de 3.6 cluster uitvoeren. Vanuit de Hive-weergave of vanuit Beeline, moet u de volgende query uitvoeren:

```bash
alter table myacidtable compact 'major';
```

Deze compressie is vereist omdat HDInsight 3.6-en HDInsight 4.0 ACID ACID delta's anders begrijpen. Compressie wordt een schone lei dat ervoor consistentie zorgt afgedwongen. Sectie 4 van de [migratiedocumentatie Hive](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) bevat richtlijnen voor het bulksgewijs compressie van HDInsight 3.6 ACID-tabellen.

Nadat u de metastore migratie en comprimeren de stappen hebt voltooid, kunt u de werkelijke datawarehouse kunt migreren. Nadat u de migratie van de Hive-warehouse hebt voltooid, wordt het magazijn 4.0 HDInsight de volgende eigenschappen hebben:

* Externe tabellen in HDInsight 3.6 worden externe tabellen in HDInsight 4.0
* Niet-transactionele beheerde tabellen in HDInsight 3.6 zijn externe tabellen in HDInsight 4.0
* Transactionele beheerde tabellen in HDInsight 3.6 worden beheerde tabellen in HDInsight 4.0

Mogelijk moet u de eigenschappen van de datawarehouse aanpassen voordat u de migratie uitvoert. Bijvoorbeeld, als u verwacht dat sommige tabel wordt gebruikt door een derde partij (zoals een HDInsight 3.6-cluster), moet die tabel externe zodra de migratie voltooid is. In HDInsight 4.0 worden alle beheerde tabellen transactionele. Beheerde tabellen in HDInsight 4.0 mag daarom alleen worden geopend door 4.0 HDInsight-clusters.

Wanneer de eigenschappen van de tabel correct zijn ingesteld, voert u het hulpprogramma voor migratie van Hive datawarehouse uit vanaf een van de hoofdknooppunten van het cluster met behulp van de SSH-shell:

1. Verbinding maken met het hoofdknooppunt van het cluster via SSH. Zie voor instructies [verbinding maken met HDInsight met behulp van SSH](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. Open een aanmeldingsshell als de Hive-gebruiker door te voeren `sudo su - hive`
1. De versie van de stack Hortonworks Data Platform bepalen door het uitvoeren van `ls /usr/hdp`. Hiermee wordt een versietekenreeks in die u moet gebruiken in de volgende opdracht weergegeven.
1. De volgende opdracht uitvoeren vanuit de shell. Vervang `${{STACK_VERSION}}` met de tekenreeks in de vorige stap:

```bash
/usr/hdp/${{STACK_VERSION}}/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true  -m automatic  automatic  --modifyManagedTables --oldWarehouseRoot /apps/hive/warehouse
```

Nadat het hulpprogramma voor migratie is voltooid, worden uw Hive-warehouse gereed voor HDInsight 4.0. 

> [!Important]
> Beheerde tabellen in HDInsight 4.0 (inclusief tabellen gemigreerd vanuit 3.6) mag niet worden geopend door andere services of toepassingen, inclusief clusters in HDInsight 3.6.

## <a name="secure-hive-across-hdinsight-versions"></a>Beveiligde Hive in HDInsight-versies

Omdat HDInsight 3.6, HDInsight kan worden geïntegreerd met Azure Active Directory met HDInsight Enterprise Security Package (ESP). ESP maakt gebruik van Kerberos en Apache Ranger om de machtigingen van specifieke bronnen binnen het cluster te beheren. Ranger-beleidsregels die zijn geïmplementeerd op basis van Hive in HDInsight 3.6 kunnen worden gemigreerd naar HDInsight 4.0 met de volgende stappen uit:

1. Ga naar het deelvenster Ranger Service Manager in uw HDInsight 3.6-cluster.
2. Ga naar het beleid met de naam **HIVE** en het beleid exporteren naar een json-bestand.
3. Zorg ervoor dat alle gebruikers in de json van de geëxporteerde beleid genoemd in het nieuwe cluster. Als een gebruiker in de json van het beleid naar wordt verwezen, maar niet in het nieuwe cluster bestaat, de gebruiker toevoegen aan het nieuwe cluster of verwijder de verwijzing van het beleid.
4. Navigeer naar de **Ranger Service Manager** paneel in het HDInsight-4.0-cluster.
5. Ga naar het beleid met de naam **HIVE** en importeert u het beleid voor ranger json uit stap 2.

## <a name="query-execution-across-hdinsight-versions"></a>Uitvoering van de query in HDInsight-versies

Er zijn twee manieren uit te voeren en fouten opsporen in Hive LLAP/query's in een HDInsight 3.6-cluster. HiveCLI biedt een nieuwe opdrachtregel en de weergave van de weergave of Hive Tez biedt een werkstroom op basis van GUI.

In HDInsight 4.0, is met Beeline HiveCLI vervangen. HiveCLI is een thrift-client voor Hiveserver 1 en Beeline is een JDBC-client die toegang tot Hiveserver 2 biedt. Beeline kan ook worden gebruikt om verbinding met een andere JDBC-compatibele database-eindpunt te maken. Beeline is beschikbaar out-of-box op HDInsight 4.0 zonder een installatie nodig.

In HDInsight 3.6 is de GUI-client voor interactie met Hive-server de Ambari Hive-weergave. HDInsight 4.0 wordt vervangen door de Hive-weergave met Hortonworks Data Analytics Studio (DAS). DAS niet geleverd met HDInsight-clusters out-of-box en is niet een officieel ondersteund-pakket. Echter, DAS kan worden geïnstalleerd op het cluster als volgt:

Start een scriptactie op basis van uw cluster, met 'Hoofdknooppunten' als het knooppunttype voor de uitvoering. Plak de volgende URI in het tekstvak gemarkeerd als 'Bash-Script URI': https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh

Data Analytics Studio kan worden gestart met de URL: https://<clustername>.azurehdinsight.net/das/



Wanneer DAS is geïnstalleerd, als u de query's die u hebt uitgevoerd in de viewer voor query's niet ziet, voert u de volgende stappen uit:

1. De configuraties voor Hive Tez en DAS ingesteld zoals wordt beschreven in [in deze handleiding voor het oplossen van DAS installatie](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html).
2. Zorg ervoor dat de volgende configuraties voor de directory van Azure storage-pagina-blobs zijn, en waarin ze worden weergegeven onder `fs.azure.page.blob.dirs`:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Start opnieuw op HDFS, Hive Tez en DAS op beide hoofdknooppunten.

## <a name="next-steps"></a>Volgende stappen

* [HDInsight 4.0 aankondiging](../hdinsight-version-release.md)
* [Gedetailleerde informatie over HDInsight 4.0](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Hive-3 ACID-tabellen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
