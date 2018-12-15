---
title: Migreren van Windows op basis van HDInsight op Linux gebaseerde HDInsight - Azure
description: Informatie over het migreren van een HDInsight op basis van een Windows-cluster naar een Linux gebaseerde HDInsight-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: hrasheed
ms.openlocfilehash: bcaf59e1d9b36dfbb17f1e0b8089cd88e626e2b9
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437126"
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migreren van een HDInsight op basis van een Windows-cluster naar een cluster op basis van Linux

Dit document bevat informatie over de verschillen tussen HDInsight op Windows en Linux. Het bevat ook richtlijnen voor bestaande workloads migreren naar een cluster op basis van Linux.

HDInsight op basis van Windows biedt een eenvoudige manier om het gebruik van Apache Hadoop in de cloud, moet u mogelijk migreren naar een cluster op basis van Linux. Als u bijvoorbeeld om te profiteren van de Linux-gebaseerde hulpprogramma's en technologieën die vereist voor uw oplossing zijn. Veel dingen in het Hadoop-ecosysteem zijn ontwikkeld op basis van Linux-systemen, en mogelijk niet beschikbaar voor gebruik met HDInsight op basis van Windows. Veel boeken, video's en andere trainingsmateriaal wordt ervan uitgegaan dat u een Linux-systeem gebruikt bij het werken met Hadoop.

> [!NOTE]  
> HDInsight-clusters voor het gebruik van Ubuntu op lange termijn ondersteuning (TNS) als het besturingssysteem voor de knooppunten in het cluster. Zie voor meer informatie over de versie van Ubuntu beschikbaar met HDInsight, samen met andere versiebeheer onderdeelinformatie [HDInsight onderdeel versies](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Migratietaken

De algemene werkstroom voor de migratie is als volgt.

![Werkstroomdiagram van migratie](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Elke sectie van dit document om te begrijpen van wijzigingen die mogelijk vereist bij het migreren van lezen.

2. Maak een cluster op basis van Linux als een test/kwaliteit assurance-omgeving. Zie voor meer informatie over het maken van een cluster op basis van Linux [clusters in HDInsight op basis van Linux maken](hdinsight-hadoop-provision-linux-clusters.md).

3. Bestaande taken, gegevensbronnen en sinks kopiëren naar de nieuwe omgeving.

4. Voer validatietests om ervoor te zorgen dat uw taken werken zoals verwacht op het nieuwe cluster.

Nadat u hebt gecontroleerd dat alles werkt zoals verwacht, plant u uitvaltijd voor de migratie. Tijdens deze uitval, moet u de volgende acties uitvoeren:

1. Back-up van alle tijdelijke gegevens die lokaal zijn opgeslagen op de clusterknooppunten. Bijvoorbeeld, als u gegevens hebt opgeslagen rechtstreeks op een hoofdknooppunt.

2. Verwijder het cluster op basis van Windows.

3. Maak een cluster op basis van Linux met behulp van de dezelfde standaard-gegevensarchief die het cluster op basis van Windows gebruikt. Het cluster op basis van Linux kunt blijven werken met uw bestaande productiegegevens.

4. Importeer geen tijdelijke gegevens die u back-up gemaakt.

5. Start taken/doorgaan met het verwerken met behulp van het nieuwe cluster.

### <a name="copy-data-to-the-test-environment"></a>Gegevens kopiëren naar de testomgeving

Er zijn veel methoden om de gegevens en taken te kopiëren, de twee besproken in deze sectie zijn echter de meest eenvoudige methoden voor het rechtstreeks bestanden verplaatsen naar een testcluster.

#### <a name="hdfs-copy"></a>HDFS-kopie

Gebruik de volgende stappen uit om gegevens te kopiëren van de productiecluster met de testcluster. Deze stappen wordt gebruikgemaakt van de `hdfs dfs` hulpprogramma dat is opgenomen in HDInsight.

1. Informatie over de storage-account en standaard-container voor uw bestaande cluster. Het volgende voorbeeld wordt PowerShell gebruikt om op te halen van deze informatie:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. Volg de stappen in de clusters op basis van Linux maken in HDInsight document voor het maken van een testomgeving. Beëindigen voordat het cluster is gemaakt en selecteert u in plaats daarvan **optionele configuratie**.

3. Selecteer in de sectie optionele configuratie **gekoppelde Storage-Accounts**.

4. Selecteer **een opslagsleutel toevoegen**, en wanneer u wordt gevraagd, selecteert u het opslagaccount dat is geretourneerd door het PowerShell-script in stap 1. Klik op **Selecteer** in elke sectie. Maak ten slotte het cluster.

5. Zodra het cluster is gemaakt, verbinding maken met behulp van **SSH.** Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

6. Gebruik de volgende opdracht om bestanden te kopiëren uit het gekoppelde opslagaccount naar het nieuwe standaardaccount voor opslag van de SSH-sessie. CONTAINER vervangen door de informatie van de container die wordt geretourneerd door PowerShell. Vervang __ACCOUNT__ met de naam van het. Het pad naar gegevens vervangen door het pad naar een bestand.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]  
    > Als de mapstructuur die de gegevens bevat niet in de testomgeving bestaat, kunt u maken met behulp van de volgende opdracht:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    De `-p` schakelt u het maken van alle mappen in het pad.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Directe kopiëren tussen de blobs in Azure Storage

U wilt ook gebruiken de `Start-AzureStorageBlobCopy` Azure PowerShell-cmdlet om te kopiëren van BLOB's tussen opslagaccounts buiten HDInsight. Zie voor meer informatie de sectie van de Azure-Blobs van Azure PowerShell gebruiken met Azure Storage beheren.

## <a name="client-side-technologies"></a>Client-side-technologieën

Client-side-technologieën zoals [Azure PowerShell-cmdlets](/powershell/azureps-cmdlets-docs), [klassieke Azure-CLI](../cli-install-nodejs.md), of de [.NET SDK voor Apache Hadoop](https://hadoopsdk.codeplex.com/) blijven werken op basis van Linux-clusters. Deze technologieën zijn afhankelijk van REST-API's die hetzelfde voor beide clustertypen OS zijn.

## <a name="server-side-technologies"></a>Server-side-technologieën

De volgende tabel biedt richtlijnen voor migratie specifiek voor Windows zijn server-side-onderdelen.

| Als u van deze technologie gebruikmaakt... | Deze maatregelen nemen... |
| --- | --- |
| **PowerShell** (serverzijde scripts, met inbegrip van scriptacties gebruikt tijdens het maken van een cluster) |Als de Bash-scripts schrijven. Zie voor scriptacties, [HDInsight op basis van Linux aanpassen met scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [Scriptactieontwikkeling voor HDInsight op basis van Linux](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI voor klassieke** (serverscripts) |Terwijl de klassieke Azure-CLI beschikbaar in Linux is, is niet afkomstig vooraf worden geïnstalleerd op de hoofdknooppunten van het HDInsight-cluster. Zie voor meer informatie over het installeren van de klassieke Azure-CLI [aan de slag met de klassieke Azure-CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **.NET-onderdelen** |.NET wordt ondersteund op Linux gebaseerde HDInsight via [Mono](https://mono-project.com). Zie voor meer informatie, [migreren .NET-oplossingen op Linux gebaseerde HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **Win32-onderdelen of andere alleen-Windows-technologie** |Richtlijnen, is afhankelijk van het onderdeel of technologie. U kunt mogelijk een versie die compatibel is met Linux. Zo niet, moet u een alternatieve oplossing te vinden of dit onderdeel te herschrijven. |

> [!IMPORTANT]  
> Het beheer van HDInsight SDK is niet volledig compatibel met Mono. Gebruik deze niet als onderdeel van de oplossingen die zijn geïmplementeerd in het HDInsight-cluster.

## <a name="cluster-creation"></a>Het maken van clusters

In deze sectie bevat informatie over de verschillen in het maken van clusters.

### <a name="ssh-user"></a>SSH gebruiker

HDInsight op basis van Linux-clusters gebruiken de **Secure Shell (SSH)** protocol voor externe toegang tot de clusterknooppunten. In tegenstelling tot op basis van een extern bureaublad voor Windows-clusters bieden de meeste SSH-clients geen een grafische gebruikersinterface. SSH-clients bieden in plaats daarvan een opdrachtregel weergegeven die kunt u opdrachten uitvoeren op het cluster. Sommige clients (zoals [MobaXterm](http://mobaxterm.mobatek.net/)) bieden een grafische bestand system browser naast een extern vanaf de opdrachtregel.

Tijdens het maken, moet u een SSH-gebruiker en ofwel een **wachtwoord** of **openbare-sleutelcertificaat** voor verificatie.

Wordt u aangeraden openbare-sleutelcertificaat dat, als het is veiliger dan het gebruik van een wachtwoord. Verificatie via certificaat werkt door een ondertekende openbaar/persoonlijk sleutelpaar genereren, en vervolgens de openbare sleutel die bij het maken van het cluster. Wanneer u verbinding maakt met de server via SSH, bevat de persoonlijke sleutel op de client-verificatie voor de verbinding.

Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

### <a name="cluster-customization"></a>Clusteraanpassing

**Scriptacties** gebruikt in combinatie met Linux gebaseerde clusters moeten zijn geschreven in Bash-script. Op basis van Linux-clusters kunnen scriptacties gebruiken tijdens of na het maken van clusters. Zie voor meer informatie, [HDInsight op basis van Linux aanpassen met scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [Scriptactieontwikkeling voor HDInsight op basis van Linux](hdinsight-hadoop-script-actions-linux.md).

Een andere aanpassing-functie is **bootstrap**. Voor Windows-clusters kunt deze functie u de locatie opgeven van meer bibliotheken voor gebruik met Hive. Na het maken van een cluster, deze bibliotheken zijn automatisch beschikbaar voor gebruik met Hive-query's zonder de noodzaak om te gebruiken `ADD JAR`.

De Bootstrap-functie voor Linux gebaseerde clusters beschikt niet over deze functionaliteit. Gebruik in plaats daarvan scriptactie gedocumenteerd in [toevoegen Apache Hive-bibliotheken tijdens het maken van een cluster](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuele netwerken

HDInsight op basis van een Windows-clusters werken alleen voor klassieke virtuele netwerken, hoewel u met HDInsight op basis van Linux-clusters Resource Manager-virtuele netwerken. Als u resources in een klassiek Virtueelnetwerk dat het Linux-HDInsight-cluster verbinding met maken moet hebt, raadpleegt u [een klassiek Virtueelnetwerk verbinden met een virtueel netwerk van Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Zie voor meer informatie over de configuratievereisten, de [mogelijkheden voor HDInsight uitbreiden met behulp van een Virtueelnetwerk](hdinsight-extend-hadoop-virtual-network.md) document.

## <a name="management-and-monitoring"></a>Beheer en controle

Veel van de web-UI's die u hebt gebruikt met HDInsight op basis van Windows, zoals Taakgeschiedenis of de gebruikersinterface van Yarn, zijn beschikbaar via Apache Ambari. Daarnaast bieden de Ambari Hive-weergave een manier om uit te voeren van Hive-query's via uw webbrowser. De Ambari-Webinterface is beschikbaar op Linux gebaseerde clusters op https://CLUSTERNAME.azurehdinsight.net.

Zie de volgende documenten voor meer informatie over het werken met Ambari:

* [Apache Ambari Web](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari-waarschuwingen

Ambari is voorzien van een waarschuwing die u aan potentiële problemen met het cluster kunt zien. Waarschuwingen worden weergegeven als rood of geel vermeldingen in de Ambari-Webgebruikersinterface, maar u ze ook via de REST-API ophalen kunt.

> [!IMPORTANT]  
> Ambari-waarschuwingen geven aan dat er *kan* een probleem is, worden niet dat er *is* een probleem. Bijvoorbeeld, verschijnt een waarschuwing dat HiveServer2 kan niet worden geopend, zelfs als u het normaal gesproken kunt openen.
>
> Veel waarschuwingen worden geïmplementeerd als query's op basis van een interval voor een service en een reactie binnen een specifiek tijdsbestek verwachten. Zodat de waarschuwing hoeft niet te betekenen dat de service niet actief is, is niet maar het resultaten binnen het verwachte tijdsbestek.

## <a name="file-system-locations"></a>Locaties van systeem

Het bestandssysteem van de Linux-cluster is anders dan HDInsight op basis van een Windows-clusters weergegeven. Gebruik de volgende tabel om te zoeken naar gangbare bestanden.

| Ik wil zoeken... | Deze bevindt zich... |
| --- | --- |
| Configuratie |`/etc`. Bijvoorbeeld: `/etc/hadoop/conf/core-site.xml` |
| Logboekbestanden |`/var/logs` |
| Hortonworks Data Platform (HDP) |`/usr/hdp`. Er zijn twee directory's die u hier, één met de huidige versie van de HDP en `current`. De `current` directory symbolische koppelingen naar bestanden en mappen in de map die versie nummer bevat. De `current` directory is opgegeven als een handige manier toegang krijgen tot bestanden HDP sinds de versie verandert als de HDP versie is bijgewerkt. |
| hadoop-streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

In het algemeen als u de naam van het bestand weet, kunt u de volgende opdracht vanaf een SSH-sessie het bestandspad te vinden:

    find / -name FILENAME 2>/dev/null

U kunt ook jokertekens gebruiken met de naam van het bestand. Bijvoorbeeld, `find / -name *streaming*.jar 2>/dev/null` resulteert in het pad naar het jar-bestanden die het woord 'streaming' als onderdeel van de bestandsnaam bevatten.

## <a name="apache-hive-apache-pig-and-mapreduce"></a>Apache Hive, Apache Pig en MapReduce

Pig en MapReduce workloads lijken op basis van Linux-clusters. HDInsight op basis van Linux-clusters kunnen echter worden gemaakt met een nieuwere versie van Hadoop-, Hive en Pig. Deze verschillen in besturingssysteemversies leiden tot wijzigingen in hoe u uw bestaande oplossingen-functie. Zie voor meer informatie over de versies van onderdelen in HDInsight [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md).

HDInsight op basis van Linux biedt geen functionaliteit voor extern bureaublad. In plaats daarvan kunt u SSH op afstand verbinding maken met de hoofdknooppunten van het cluster. Zie de volgende documenten voor meer informatie:

* [Apache Hive gebruiken met SSH](hdinsight-hadoop-use-hive-ssh.md)
* [Apache Pig gebruiken met SSH](hadoop/apache-hadoop-use-pig-ssh.md)
* [MapReduce gebruiken met SSH](hadoop/apache-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]  
> Als u een externe Hive-metastore gebruikt, moet u back-up van de metastore voordat u deze gebruikt met HDInsight op basis van Linux. HDInsight op basis van Linux is beschikbaar met een nieuwere versie van Hive, die mogelijk compatibiliteitsproblemen met metastores gemaakt met eerdere versies.

De volgende tabel bevat richtlijnen over het migreren van uw Hive-workloads.

| Op Windows gebaseerde ik gebruik... | Op basis van Linux... |
| --- | --- |
| **Hive-Editor** |[Apache Hive-weergave in Ambari](hadoop/apache-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;` inschakelen van Tez |Apache Tez is de engine voor het uitvoeren van standaard voor op basis van Linux-clusters, zodat de set-instructie is niet meer nodig. |
| C#-door gebruiker gedefinieerde functies | Zie voor meer informatie over het valideren van C#-onderdelen met HDInsight op basis van Linux [migreren .NET-oplossingen op Linux gebaseerde HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-bestanden of scripts op de server worden aangeroepen als onderdeel van een Hive-taak |Bash-scripts gebruiken |
| `hive` opdracht van extern bureaublad |Gebruik [Beeline](hadoop/apache-hadoop-use-hive-beeline.md) of [Apache Hive van een SSH-sessie](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| Op Windows gebaseerde ik gebruik... | Op basis van Linux... |
| --- | --- |
| C#-door gebruiker gedefinieerde functies | Zie voor meer informatie over het valideren van C#-onderdelen met HDInsight op basis van Linux [migreren .NET-oplossingen op Linux gebaseerde HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-bestanden of scripts op de server worden aangeroepen als onderdeel van een Pig-taak |Bash-scripts gebruiken |

### <a name="mapreduce"></a>MapReduce

| Op Windows gebaseerde ik gebruik... | Op basis van Linux... |
| --- | --- |
| C#-toewijzing en reducer onderdelen | Zie voor meer informatie over het valideren van C#-onderdelen met HDInsight op basis van Linux [migreren .NET-oplossingen op Linux gebaseerde HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-bestanden of scripts op de server worden aangeroepen als onderdeel van een Hive-taak |Bash-scripts gebruiken |

## <a name="apache-oozie"></a>Apache Oozie

> [!IMPORTANT]  
> Als u een externe Oozie-metastore gebruikt, moet u back-up van de metastore voordat u deze gebruikt met HDInsight op basis van Linux. HDInsight op basis van Linux is beschikbaar met een nieuwere versie van Oozie, die mogelijk compatibiliteitsproblemen met metastores gemaakt met eerdere versies.

Oozie werkstromen shell acties voor toestaan. Shell-acties gebruiken de standaardshell voor het besturingssysteem om uit te voeren opdrachtregelopdrachten. Als u Oozie-werkstromen die afhankelijk van de Windows-shell zijn hebt, moet u de workflows zijn afhankelijk van de Linux-shell-omgeving (Bash) herschrijven. Zie voor meer informatie over het gebruik van shell-acties met Oozie [Oozie shell action-extensie](http://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

Als u een werkstroom die gebruikmaakt van een C#-toepassing hebt, controleert u deze toepassingen in een Linux-omgeving. Zie voor meer informatie, [migreren .NET-oplossingen op Linux gebaseerde HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| Op Windows gebaseerde ik gebruik... | Op basis van Linux... |
| --- | --- |
| Storm-Dashboard |De Storm-Dashboard is niet beschikbaar. Zie [implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Linux](storm/apache-storm-deploy-monitor-topology-linux.md) voor manieren om in te dienen topologieën |
| Storm-gebruikersinterface |De Storm-gebruikersinterface is beschikbaar op https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio maken, implementeren en beheren van C# of hybride topologieën |Visual Studio kan worden gebruikt om te maken, implementeren en beheren van C# (SCP.NET) of hybride topologieën op Linux gebaseerde Storm op HDInsight. Het kan alleen worden gebruikt met de clusters die zijn gemaakt na 10/28/2016. |

## <a name="apache-hbase"></a>Apache HBase

Op Linux gebaseerde clusters, de bovenliggende znode voor HBase is `/hbase-unsecure`. Stel deze waarde in de configuratie voor elke Java-client toepassingen die gebruikmaken van systeemeigen HBase Java-API.

Zie [een Apache HBase op basis van een Java-toepassing bouwen](hdinsight-hbase-build-java-maven.md) voor een voorbeeld van de client die deze waarde wordt ingesteld.

## <a name="spark"></a>Spark

Spark-clusters zijn beschikbaar op Windows-clusters tijdens de preview. Spark-algemene beschikbaarheid is alleen beschikbaar bij op basis van Linux-clusters. Er is geen migratiepad van een preview-cluster van Spark op basis van Windows naar een release op basis van Linux Spark-cluster.

## <a name="known-issues"></a>Bekende problemen

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory aangepaste .NET-activiteiten

Azure Data Factory aangepaste .NET-activiteiten worden momenteel niet ondersteund op Linux gebaseerde HDInsight-clusters. In plaats daarvan moet u een van de volgende methoden gebruiken voor het implementeren van aangepaste activiteiten als onderdeel van uw pijplijn ADF.

* .NET-activiteiten uitvoeren op Azure Batch-pool. Zie de met Azure Batch service-sectie van de gekoppelde [aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](../data-factory/transform-data-using-dotnet-custom-activity.md)
* Implementeer de activiteit als een MapReduce-activiteit. Zie voor meer informatie, [MapReduce-programma's van Data Factory aanroepen](../data-factory/transform-data-using-hadoop-map-reduce.md).

### <a name="line-endings"></a>Regeleinden

In het algemeen gebruik regeleinden op Windows-systemen bij van CRLF, op basis van Linux-systemen gebruiken LF. Mogelijk moet u bestaande gegevensproducenten en consumenten om te werken met LF wijzigen.

Bijvoorbeeld, retourneert met behulp van Azure PowerShell query HDInsight op een cluster op basis van een Windows-gegevens met CRLF. Dezelfde query met een op basis van Linux-cluster retourneert LF. Test om te zien als het einde van regel zorgt ervoor een probleem met uw oplossing dat voordat u migreert naar een cluster op basis van Linux.

Gebruik altijd LF als de regel die eindigt voor scripts die worden uitgevoerd op de clusterknooppunten. Als u CRLF gebruikt, kunt u misschien fouten ziet wanneer de scripts uitgevoerd op een cluster op basis van Linux.

Als de scripts tekenreeksen met ingesloten CR tekens niet bevatten, kunt u wijzigen bulksgewijs de regeleinden met behulp van een van de volgende methoden:

* **Voordat u uploadt naar het cluster**: Gebruik de volgende PowerShell-instructies om de regeleinden van CRLF te LF wijzigen voordat u het script uploadt naar het cluster.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **Na het uploaden van het cluster**: Gebruik de volgende opdracht vanaf een SSH-sessie met het cluster op basis van Linux om het script.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het maken van HDInsight op basis van Linux-clusters](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH gebruiken om te verbinden met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Een cluster op basis van Linux met behulp van Apache Ambari beheren](hdinsight-hadoop-manage-ambari.md)
