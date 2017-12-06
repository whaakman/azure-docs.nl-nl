---
title: Migreren van HDInsight op basis van Windows naar Linux gebaseerde HDInsight - Azure | Microsoft Docs
description: Informatie over het migreren van een HDInsight op basis van Windows-cluster naar een Linux gebaseerde HDInsight-cluster.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: ff35be59-bae3-42fd-9edc-77f0041bab93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: 764a41dc9e890de85c3bfab3d2f78d5a07b39dff
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migreren van een HDInsight op basis van Windows-cluster naar een cluster op basis van Linux

Dit document bevat informatie over de verschillen tussen HDInsight op Windows en Linux. Ook biedt richtlijnen voor het migreren van bestaande werkbelastingen naar een cluster op basis van Linux.

Hoewel HDInsight op basis van Windows een eenvoudige manier biedt om het gebruik van Hadoop in de cloud, moet u wellicht migreren naar een cluster op basis van Linux. Als u bijvoorbeeld om te profiteren van Linux-gebaseerde hulpprogramma's en -technologieën die nodig voor uw oplossing zijn. Groot aantal dingen in het Hadoop-ecosysteem op Linux gebaseerde systemen zijn ontwikkeld en mogelijk niet beschikbaar voor gebruik met HDInsight op basis van Windows. Veel boeken, video's en andere trainingsmateriaal wordt ervan uitgegaan dat u van een Linux-systeem gebruikmaakt bij het werken met Hadoop.

> [!NOTE]
> HDInsight-clusters gebruiken op lange termijn Ubuntu-ondersteuning (TNS) als het besturingssysteem voor de knooppunten in het cluster. Zie voor informatie over de versie van Ubuntu beschikbaar met HDInsight, samen met andere versiegegevens onderdeel, [HDInsight onderdeel versies](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Migratietaken

De algemene werkstroom voor de migratie is als volgt.

![Werkstroomdiagram van migratie](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Elke sectie van dit document om te begrijpen wijzigingen die mogelijk vereist zijn bij de migratie worden gelezen.

2. Een cluster op basis van Linux maken als een test/quality assurance-omgeving. Zie voor meer informatie over het maken van een cluster op basis van Linux [clusters in HDInsight op basis van Linux maken](hdinsight-hadoop-provision-linux-clusters.md).

3. Bestaande projecten, gegevensbronnen en put kopiëren naar de nieuwe omgeving.

4. Voer de validatie testen om ervoor te zorgen dat uw taken werken zoals verwacht op het nieuwe cluster.

Zodra u hebt gecontroleerd dat alles werkt zoals verwacht, plant u uitvaltijd voor de migratie. Tijdens deze uitvaltijd, moet u de volgende acties uitvoeren:

1. Back-up tijdelijke gegevens die lokaal zijn opgeslagen op de clusterknooppunten. Bijvoorbeeld, als u gegevens hebt opgeslagen rechtstreeks op een hoofdknooppunt.

2. Verwijdert het cluster op basis van Windows.

3. Maak een cluster op basis van Linux met hetzelfde standaard gegevensarchief die het cluster op basis van Windows gebruikt. Het cluster op basis van Linux kunt blijven werken met uw bestaande productiegegevens.

4. Importeer tijdelijke gegevens die u een back-up.

5. Start taken/doorgaan met het verwerken met behulp van het nieuwe cluster.

### <a name="copy-data-to-the-test-environment"></a>Gegevens kopiëren naar de testomgeving

Er zijn veel methoden om de gegevens en taken te kopiëren, de twee besproken in deze sectie zijn echter de eenvoudigste methoden voor het rechtstreeks verplaatsen van bestanden naar een testcluster.

#### <a name="hdfs-copy"></a>HDFS kopiëren

Gebruik de volgende stappen uit om gegevens te kopiëren uit het productiecluster voor de testcluster. Deze stappen wordt gebruik de `hdfs dfs` hulpprogramma dat wordt meegeleverd met HDInsight.

1. De storage-account en het standaard container informatie vinden voor het bestaande cluster. PowerShell wordt het volgende voorbeeld deze informatie op te halen:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. Volg de stappen in de clusters op basis van Linux maken in HDInsight document voor het maken van een testomgeving. Beëindigen voordat het maken van het cluster en selecteert u in plaats daarvan **optionele configuratie**.

3. Selecteer in de sectie optionele configuratie **gekoppelde Storage-Accounts**.

4. Selecteer **opslag van een sleutel**, en wanneer u wordt gevraagd, selecteert u het opslagaccount dat is geretourneerd door het PowerShell-script in stap 1. Klik op **Selecteer** in elke sectie. Maak ten slotte het cluster.

5. Zodra het cluster is gemaakt, verbinding maken met de **SSH.** Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

6. Gebruik de volgende opdracht om bestanden te kopiëren van het gekoppelde opslagaccount naar het nieuwe standaardaccount voor opslag van de SSH-sessie. CONTAINER vervangen door de container-informatie geretourneerd door PowerShell. Vervang __ACCOUNT__ met de accountnaam. Het pad naar gegevens vervangen door het pad naar een bestand.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]
    > Als u de mapstructuur die de gegevens bevat, is niet aanwezig op de testomgeving, kunt u met behulp van de volgende opdracht:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    De `-p` schakelt u het maken van alle mappen in het pad.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Directe kopiëren tussen blobs in Azure Storage

U kunt ook gebruik van de `Start-AzureStorageBlobCopy` Azure PowerShell-cmdlet voor het kopiëren van BLOB's tussen opslagaccounts buiten HDInsight. Zie voor meer informatie de sectie Azure Blobs van Azure PowerShell gebruiken met Azure Storage beheren.

## <a name="client-side-technologies"></a>Client-side '-technologieën

Client-side '-technologieën zoals [Azure PowerShell-cmdlets](/powershell/azureps-cmdlets-docs), [Azure CLI](../cli-install-nodejs.md), of de [.NET SDK voor Hadoop](https://hadoopsdk.codeplex.com/) blijven werken op basis van Linux-clusters. Deze technologieën zijn afhankelijk van REST-API's die hetzelfde voor beide clustertypen OS zijn.

## <a name="server-side-technologies"></a>Server-side technologieën

De volgende tabel biedt richtlijnen voor migratie specifiek voor Windows zijn server-side-onderdelen.

| Als u van deze technologie gebruikmaakt... | Deze actie duren... |
| --- | --- |
| **PowerShell** (serverzijde scripts, met inbegrip van scriptacties gebruikt tijdens het maken van het cluster) |Herschrijf als Bash-scripts. Zie voor scriptacties, [HDInsight op basis van Linux aanpassen met scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [Scriptactieontwikkeling voor HDInsight op basis van Linux](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (serverzijde scripts) |De Azure CLI is beschikbaar op Linux, is niet afkomstig vooraf worden geïnstalleerd op de hoofdknooppunten van het HDInsight-cluster. Zie voor meer informatie over het installeren van de Azure CLI [aan de slag met Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **.NET-onderdelen** |.NET wordt ondersteund op Linux gebaseerde HDInsight via [Mono](https://mono-project.com). Zie voor meer informatie [migreren .NET-oplossingen voor Linux gebaseerde HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **Win32-onderdelen of andere alleen Windows technologie** |Richtlijnen, is afhankelijk van het onderdeel of technologie. U kunt mogelijk een versie die compatibel is met Linux vinden. Zo niet, moet u een alternatieve oplossing vinden of dit onderdeel herschrijven. |

> [!IMPORTANT]
> Het beheer van HDInsight SDK is niet volledig compatibel is met Mono. Gebruik deze niet als onderdeel van oplossingen die zijn geïmplementeerd op het HDInsight-cluster.

## <a name="cluster-creation"></a>Maken van het cluster

Deze sectie bevat informatie over de verschillen in het maken van het cluster.

### <a name="ssh-user"></a>SSH gebruiker

Linux gebaseerde HDInsight-clusters gebruiken de **Secure Shell (SSH)** protocol voor externe toegang tot de clusterknooppunten. In tegenstelling tot extern bureaublad voor Windows gebaseerde clusters bieden meeste SSH-clients geen een grafische gebruikersinterface. SSH-clients bieden in plaats daarvan een opdrachtregel waarmee u opdrachten uit te voeren op het cluster. Sommige clients (zoals [MobaXterm](http://mobaxterm.mobatek.net/)) bieden een grafische bestand system browser naast een extern vanaf de opdrachtregel.

Tijdens het maken, moet u een SSH-gebruiker en een opgeven een **wachtwoord** of **openbare-sleutelcertificaat** voor verificatie.

Wordt u aangeraden openbare-sleutelcertificaat, omdat het is veiliger dan een wachtwoord te gebruiken. Verificatie via certificaat werkt door een ondertekende openbaar/persoonlijk sleutelpaar genereren en vervolgens de openbare sleutel bieden bij het maken van het cluster. Wanneer u verbinding maakt met de server via SSH, biedt de persoonlijke sleutel op de client verificatie voor de verbinding.

Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

### <a name="cluster-customization"></a>Aanpassing van het cluster

**Acties script** gebruikt met clusters op basis van Linux in Bash-scripts moeten worden geschreven. Op basis van Linux-clusters kunt scriptacties tijdens of na het maken van het cluster. Zie voor meer informatie [HDInsight op basis van Linux aanpassen met scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [Scriptactieontwikkeling voor HDInsight op basis van Linux](hdinsight-hadoop-script-actions-linux.md).

Een andere aanpassing-functie is **bootstrap**. Voor Windows-clusters kunt deze functie u de locatie van aanvullende bibliotheken voor gebruik met Hive opgeven. Na het maken van het cluster deze bibliotheken zijn automatisch beschikbaar voor gebruik met Hive-query's zonder te hoeven gebruiken `ADD JAR`.

De Bootstrap-functie voor op basis van Linux-clusters biedt deze functionaliteit niet. Gebruik in plaats daarvan scriptactie gedocumenteerd in [toevoegen Hive-bibliotheken tijdens het maken van het cluster](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuele netwerken

HDInsight op basis van Windows-clusters alleen werken met klassieke virtuele netwerken, terwijl Linux gebaseerde HDInsight-clusters Resource Manager virtuele netwerken vereisen. Als u een klassiek virtueel netwerk dat het Linux-HDInsight-cluster verbinding met maken moet resources hebt, raadpleegt u [een klassiek virtueel netwerk verbinden met een Resource Manager virtuele netwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Zie voor meer informatie over configuratievereisten de [mogelijkheden uitbreiden HDInsight met behulp van een virtueel netwerk](hdinsight-extend-hadoop-virtual-network.md) document.

## <a name="management-and-monitoring"></a>Beheer en controle

Veel van de web-UI dat u in combinatie met HDInsight op basis van Windows zoals Taakgeschiedenis of de gebruikersinterface van Yarn gebruikt zijn beschikbaar via Ambari. Daarnaast biedt de Ambari Hive-weergave Hive-query's via uw webbrowser uitvoert. De Ambari-Webgebruikersinterface is beschikbaar op Linux gebaseerde clusters op https://CLUSTERNAME.azurehdinsight.net.

Zie de volgende documenten voor meer informatie over het werken met Ambari:

* [Ambari Web](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari-waarschuwingen

Ambari is een waarschuwing systeem waarmee u mogelijke problemen met het cluster. Waarschuwingen worden weergegeven als rood of geel vermeldingen in de Ambari-Webgebruikersinterface, maar u ze ook via de REST-API ophalen kunt.

> [!IMPORTANT]
> Ambari waarschuwingen geven aan dat er *mogelijk* is er een probleem niet dat er *is* een probleem. Bijvoorbeeld, verschijnt een waarschuwing dat HiveServer2 kan niet worden geopend, zelfs als u toegang hebt tot het normaal.
>
> Veel waarschuwingen worden geïmplementeerd als op interval gebaseerde query uitgevoerd naar een service en een reactie binnen een specifiek tijdsbestek verwachten. Zodat de waarschuwing niet noodzakelijkerwijs dat de service niet actief is, resultaten maar het niet binnen het verwachte tijdsbestek.

## <a name="file-system-locations"></a>De locaties van systeem

Het bestandssysteem van de Linux-cluster worden anders dan Windows gebaseerde HDInsight-clusters verspreid. Gebruik de volgende tabel om te zoeken naar gebruikte bestanden.

| Ik wil... | Deze bevindt zich... |
| --- | --- |
| Configuratie |`/etc`. Bijvoorbeeld: `/etc/hadoop/conf/core-site.xml` |
| Logboekbestanden |`/var/logs` |
| Hortonworks Data Platform HDP) |`/usr/hdp`. Er zijn twee directory's die u hier, die de huidige versie van de HDP en `current`. De `current` directory bevat symbolische koppelingen naar bestanden en mappen die zich in de map van versie-nummer. De `current` directory is opgegeven als een handige manier toegang krijgen tot HDP bestanden sinds verandert het versienummer als het HDP versie is bijgewerkt. |
| hadoop-streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

In het algemeen als u de naam van het bestand, kunt u de volgende opdracht vanaf een SSH-sessie het bestandspad vinden:

    find / -name FILENAME 2>/dev/null

U kunt ook jokertekens gebruiken met de bestandsnaam. Bijvoorbeeld: `find / -name *streaming*.jar 2>/dev/null` resulteert in het pad naar het jar-bestanden die het woord streaming als onderdeel van de bestandsnaam bevatten.

## <a name="hive-pig-and-mapreduce"></a>Hive, Pig en MapReduce

Pig en MapReduce-belastingen lijken op Linux gebaseerde clusters. Linux gebaseerde HDInsight-clusters kunnen echter worden gemaakt met een nieuwere versie van Hadoop Hive en Pig. Deze verschillen tussen versies kunnen leiden tot wijzigingen in hoe uw bestaande oplossingen-functie. Zie voor meer informatie over de versies van onderdelen in HDInsight [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md).

Linux gebaseerde HDInsight biedt geen functionaliteit voor extern bureaublad. U kunt in plaats daarvan SSH gebruiken voor externe verbinding met de hoofdknooppunten van het cluster. Zie de volgende documenten voor meer informatie:

* [Hive gebruiken met SSH](hdinsight-hadoop-use-hive-ssh.md)
* [Pig gebruiken met SSH](hadoop/apache-hadoop-use-pig-ssh.md)
* [U MapReduce gebruikt met SSH](hadoop/apache-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]
> Als u een externe Hive-metastore gebruikt, moet u back-up de metastore voordat u deze gebruikt met HDInsight op basis van Linux. HDInsight op basis van Linux is beschikbaar met nieuwere versies van Hive, die mogelijk incompatibel met metastores gemaakt met eerdere versies.

De volgende tabel biedt richtlijnen voor het migreren van uw Hive-werkbelastingen.

| Op Windows gebaseerde ik gebruik... | Op Linux gebaseerde... |
| --- | --- |
| **Hive-Editor** |[Weergave in de Ambari hive](hadoop/apache-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`inschakelen van Tez |Tez is de engine voor het uitvoeren van standaard voor op basis van Linux-clusters, zodat de set-instructie niet langer nodig is. |
| C# gebruiker gedefinieerde functies | Zie voor informatie over het valideren van C#-onderdelen met HDInsight op basis van Linux, [migreren .NET-oplossingen voor HDInsight op basis van Linux](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-bestanden of scripts op de server die als onderdeel van een Hive-taak wordt aangeroepen |Bash-scripts gebruiken |
| `hive`de opdracht van een extern bureaublad |Gebruik [Beeline](hadoop/apache-hadoop-use-hive-beeline.md) of [Hive van een SSH-sessie](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| Op Windows gebaseerde ik gebruik... | Op Linux gebaseerde... |
| --- | --- |
| C# gebruiker gedefinieerde functies | Zie voor informatie over het valideren van C#-onderdelen met HDInsight op basis van Linux, [migreren .NET-oplossingen voor HDInsight op basis van Linux](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-bestanden of scripts op de server die als onderdeel van een Pig-taak wordt aangeroepen |Bash-scripts gebruiken |

### <a name="mapreduce"></a>MapReduce

| Op Windows gebaseerde ik gebruik... | Op Linux gebaseerde... |
| --- | --- |
| C# toewijzen en reducer-onderdelen | Zie voor informatie over het valideren van C#-onderdelen met HDInsight op basis van Linux, [migreren .NET-oplossingen voor HDInsight op basis van Linux](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-bestanden of scripts op de server die als onderdeel van een Hive-taak wordt aangeroepen |Bash-scripts gebruiken |

## <a name="oozie"></a>Oozie

> [!IMPORTANT]
> Als u een externe Oozie-metastore gebruikt, moet u back-up de metastore voordat u deze gebruikt met HDInsight op basis van Linux. HDInsight op basis van Linux is beschikbaar met nieuwere versies van Oozie, die mogelijk incompatibel met metastores gemaakt met eerdere versies.

Oozie werkstromen shell acties voor toestaan. Acties Shell gebruiken de standaardshell voor het besturingssysteem vanaf de opdrachtregel opdrachten uit te voeren. Als u Oozie-werkstromen die afhankelijk van de Windows-shell zijn hebt, moet u de werkstromen voor het vertrouwen op de Linux-shell-omgeving (Bash) herschrijven. Zie voor meer informatie over het gebruik van de shell-acties met Oozie [Oozie actie shelluitbreiding](http://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

Als u een werkstroom die gebruikmaakt van een C#-toepassing hebt, valideren deze toepassingen in een omgeving met Linux. Zie voor meer informatie [migreren .NET-oplossingen voor Linux gebaseerde HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| Op Windows gebaseerde ik gebruik... | Op Linux gebaseerde... |
| --- | --- |
| Storm-Dashboard |Het Storm-Dashboard is niet beschikbaar. Zie [implementeren en beheren van Storm-topologieën op Linux gebaseerde HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md) voor manieren om in te dienen topologieën |
| Storm-gebruikersinterface |De Storm-gebruikersinterface is beschikbaar op https://CLUSTERNAME.azurehdinsight.NET/stormui |
| Visual Studio maken, implementeren en beheren van C# of hybride topologieën |Visual Studio kan worden gebruikt voor het maken, implementeren en beheren van C# (SCP.NET) of hybride topologieën op Linux gebaseerde Storm op HDInsight. Alleen kan worden gebruikt met clusters die zijn gemaakt na 28-10-2016. |

## <a name="hbase"></a>HBase

Op Linux gebaseerde clusters, de bovenliggende znode voor HBase is `/hbase-unsecure`. Stel deze waarde in de configuratie voor elke client Java-toepassingen die gebruikmaken van systeemeigen HBase Java API.

Zie [een HBase op basis van Java-toepassing bouwt](hdinsight-hbase-build-java-maven.md) voor een voorbeeld van de client die deze waarde wordt ingesteld.

## <a name="spark"></a>Spark

Spark-clusters zijn beschikbaar op Windows-clusters tijdens de preview. Spark GA is alleen beschikbaar bij op basis van Linux-clusters. Er is geen migratiepad van een preview Spark op basis van Windows cluster naar een release op basis van Linux Spark-cluster.

## <a name="known-issues"></a>Bekende problemen

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory aangepaste .NET-activiteiten

Azure Data Factory aangepaste .NET-activiteiten worden momenteel niet ondersteund op Linux gebaseerde HDInsight-clusters. In plaats daarvan moet u een van de volgende methoden gebruiken voor het implementeren van aangepaste activiteiten als onderdeel van uw ADF-pijplijn.

* .NET-activiteiten niet uitvoeren op Azure Batch-pool. Zie de gebruik Azure Batch service-sectie van gekoppelde [aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](../data-factory/transform-data-using-dotnet-custom-activity.md)
* De activiteit als een MapReduce-activiteit worden geïmplementeerd. Zie voor meer informatie [MapReduce-programma's uit Data Factory aanroepen](../data-factory/transform-data-using-hadoop-map-reduce.md).

### <a name="line-endings"></a>Regeleinden

In het algemeen gebruik regeleinden op Windows-systemen bij van CRLF, op basis van Linux-systemen LF gebruiken. Mogelijk moet u bestaande gegevensproducenten en consumenten werken met LF wijzigen.

Bijvoorbeeld, retourneert met behulp van Azure PowerShell query HDInsight op een cluster met Windows-gegevens met CRLF. Dezelfde query met een cluster op basis van Linux retourneert LF. Controleer of het einde van regel zorgt ervoor een probleem met uw oplossing dat voordat u migreert naar een cluster op basis van Linux.

Gebruik altijd LF als een regel die eindigt voor scripts die worden uitgevoerd op de clusterknooppunten. Als u CRLF gebruikt, kunt u misschien fouten ziet wanneer de scripts uitgevoerd op een cluster op basis van Linux.

Als de scripts tekenreeksen met ingesloten CR tekens niet bevatten, kunt u wijzigen bulksgewijs de regeleinden met behulp van een van de volgende methoden:

* **Voordat u uploadt naar het cluster**: Gebruik de volgende PowerShell-instructies om de regeleinden van CRLF te LF wijzigen voordat u het script uploadt naar het cluster.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **Na het uploaden van het cluster**: Gebruik de volgende opdracht vanaf een SSH-sessie met het cluster op basis van Linux te wijzigen van het script.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het maken van Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Beheer op basis van Linux clusters met Ambari](hdinsight-hadoop-manage-ambari.md)
