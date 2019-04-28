---
title: Apache Sqoop taken uitvoeren met Azure HDInsight (Apache Hadoop)
description: Informatie over het gebruik van Azure PowerShell op een werkstation uitvoeren Sqoop importeren en exporteren tussen een Hadoop-cluster en een Azure SQL database.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: 6764d8d812789c9f54fa59e10b2a3e416e583a9c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129396"
---
# <a name="use-apache-sqoop-with-hadoop-in-hdinsight"></a>Apache Sqoop gebruiken met Hadoop in HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Leer hoe u Apache Sqoop gebruiken in HDInsight voor het importeren en exporteren van gegevens tussen een HDInsight-cluster en een Azure SQL database.

Hoewel Apache Hadoop een logische keuze is voor de verwerking van ongestructureerde en semi-gestructureerde gegevens, zoals Logboeken en -bestanden, er mogelijk ook nodig voor het verwerken van gestructureerde gegevens die zijn opgeslagen in relationele databases.

[Apache Sqoop](https://sqoop.apache.org/docs/1.99.7/user.html) is een hulpprogramma waarmee gegevens worden overgebracht tussen Hadoop-clusters en relationele databases. U kunt deze gebruiken om gegevens te importeren uit een relationeel databasebeheersysteem (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in Hadoop MapReduce of Apache Hive transformeren, en vervolgens de gegevens weer geëxporteerd naar een RDBMS . In dit artikel gebruikt u een SQL Server-database voor uw relationele database.

> [!IMPORTANT]  
> In dit artikel stelt u een testomgeving om uit te voeren van de gegevensoverdracht. U een methode voor het overbrengen van gegevens voor deze omgeving vervolgens kiezen uit een van de methoden in de sectie [Sqoop uitvoeren taken](#run-sqoop-jobs), verderop.

Zie voor Sqoop-versies die worden ondersteund op HDInsight-clusters, [wat is er nieuw in de clusterversies geleverd door HDInsight?](../hdinsight-component-versioning.md)

## <a name="understand-the-scenario"></a>Inzicht in het scenario

HDInsight-cluster wordt geleverd met enkele voorbeeldgegevens. U gebruikt de volgende twee voorbeelden:

* Een Apache Log4j-logboekbestand bevindt zich onder `/example/data/sample.log`. De volgende logboeken worden opgehaald uit het bestand:

```text
2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
...
```

* Een Hive-tabel met de naam `hivesampletable`, die verwijst naar het bestand dat zich bevindt in `/hive/warehouse/hivesampletable`. De tabel bevat enkele gegevens van mobiele apparaten.
  
  | Veld | Gegevenstype |
  | --- | --- |
  | clientid |string |
  | querytime |string |
  | markt |string |
  | deviceplatform |string |
  | devicemake |string |
  | devicemodel |string |
  | state |string |
  | Land/regio |string |
  | querydwelltime |double |
  | sessie-id |bigint |
  | sessionpagevieworder |bigint |

In dit artikel gebruikt u deze twee gegevenssets Sqoop-Importeer testen en te exporteren.

## <a name="create-cluster-and-sql-database"></a>Testomgeving instellen
Het cluster, SQL-database en andere objecten zijn gemaakt via de Azure-portal met behulp van een Azure Resource Manager-sjabloon. De sjabloon kunt u vinden in [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). De Resource Manager-sjabloon wordt een bacpac-pakket voor het implementeren van de tabel schema's met een SQL-database.  Het pakket bacpac bevindt zich in een openbare blob-container https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Als u een privé-container gebruiken voor het bacpac-bestanden wilt, gebruikt u de volgende waarden in de sjabloon:

```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

> [!NOTE]  
> Importeren met behulp van een sjabloon of de Azure-portal biedt alleen ondersteuning voor een BACPAC-bestand importeren uit Azure blob-opslag.

1. Selecteer de volgende afbeelding om te openen van de Resource Manager-sjabloon in Azure portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende eigenschappen in:

    |Veld |Value |
    |---|---|
    |Abonnement |Selecteer uw Azure-abonnement in de vervolgkeuzelijst.|
    |Resourcegroep |Selecteer uw resourcegroep in de vervolgkeuzelijst, of een nieuw wachtwoord maken|
    |Locatie |Selecteer een regio in de vervolgkeuzelijst.|
    |Clusternaam |Voer een naam in voor het Hadoop-cluster. Gebruik alleen kleine letter.|
    |Gebruikersnaam voor clusteraanmelding |De vooraf ingestelde waarde behouden `admin`.|
    |Wachtwoord voor clusteraanmelding |Voer een wachtwoord in.|
    |SSH-gebruikersnaam |De vooraf ingestelde waarde behouden `sshuser`.|
    |SSH wachtwoord |Voer een wachtwoord in.|
    |Aanmeldgegevens SQL-beheerder |De vooraf ingestelde waarde behouden `sqluser`.|
    |Wachtwoord SQL-beheerder |Voer een wachtwoord in.|
    |_artifacts locatie | Gebruik de standaardwaarde, tenzij u wilt gebruiken van uw eigen bacpac-bestand in een andere locatie.|
    |_artifacts locatie Sas-Token |Leeg laten.|
    |De naam van het Bacpac-bestand |Gebruik de standaardwaarde, tenzij u wilt gebruiken van uw eigen bacpac-bestand.|
    |Locatie |Gebruik de standaardwaarde.|

    Naam van de Azure SQL-Server worden `<ClusterName>dbserver`. Naam van de database zal worden `<ClusterName>db`. Naam van het standaard opslagaccount worden `e6qhezrh2pdqu`.

3. Selecteer **ik ga akkoord met de voorwaarden en bepalingen bovenstaande**.

4. Selecteer **Aankoop**. U ziet een nieuwe tegel met de titel implementatie indienen voor sjabloonimplementatie. Het duurt ongeveer 20 minuten om het cluster en de SQL-database te maken.

## <a name="run-sqoop-jobs"></a>Sqoop taken uitvoeren

HDInsight uitvoeren Sqoop taken met behulp van een aantal methoden. Gebruik de volgende tabel om te bepalen welke methode is geschikt voor u, en vervolgens de koppeling voor een overzicht.

| **Gebruik deze** als u wilt dat... | ...an **interactieve** shell | ...**batch** verwerken | ...from dit **clientbesturingssysteem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-sqoop-mac-linux.md) |? |? |Linux, Unix, Mac OS X of Windows |
| [.NET-SDK voor Hadoop](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |?  |Windows (voorlopig) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |? |Windows |

## <a name="limitations"></a>Beperkingen

* Bulksgewijs exporteren - met Linux gebaseerde HDInsight, de Sqoop-connector die wordt gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.
* Batchverwerking - met HDInsight op basis van Linux bij het gebruik van de `-batch` overschakelen tijdens het uitvoeren van ingevoegd, Sqoop meerdere invoegingen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe u Sqoop gebruiken. Voor meer informatie zie:

* [Apache Hive gebruiken met HDInsight](../hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](../hdinsight-use-pig.md)
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md): Andere methoden voor het uploaden van gegevens naar HDInsight/Azure-blobopslag te vinden.
