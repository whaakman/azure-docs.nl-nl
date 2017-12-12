---
title: HUE met Hadoop op HDInsight Linux gebaseerde clusters - Azure | Microsoft Docs
description: Informatie over het Hue installeren op HDInsight-clusters en -tunneling gebruiken voor het routeren van de aanvragen met Hue. Hue opslag bladeren en het uitvoeren van Hive of Pig gebruiken.
keywords: HUE hadoop
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 9e57fcca-e26c-479d-a745-7b80a9290447
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: 5cef5e72af8a8b7c007b688b029f875e89d163ae
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installeren en gebruiken van Hue op HDInsight Hadoop-clusters

Informatie over het Hue installeren op HDInsight-clusters en -tunneling gebruiken voor het routeren van de aanvragen met Hue.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="what-is-hue"></a>Wat is Hue?
HUE is een set van webtoepassingen die worden gebruikt om te communiceren met een Hadoop-cluster. U kunt Hue gebruiken om te bladeren, de opslag die is gekoppeld aan een Hadoop-cluster (in het geval van HDInsight-clusters WASB), uitvoeren van Hive-taken en Pig-scripts, enzovoort. De volgende onderdelen zijn beschikbaar bij Hue-installaties op een HDInsight Hadoop-cluster.

* Bijenwas Hive-Editor
* Pig
* Metastore manager
* Oozie
* FileBrowser (die wordt gesproken naar de standaardcontainer WASB)
* Taak Browser

> [!WARNING]
> Onderdelen van het HDInsight-cluster volledig worden ondersteund en Microsoft Support kunt opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen ontvangt binnen commercieel redelijke ondersteuning u helpen het probleem verder op te lossen. Dit kan leiden tot het oplossen van het probleem of vraag of u benaderen beschikbare kanalen voor de open-source technologieën waar grondige kennis van deze technologie kan worden gevonden. Bijvoorbeeld: Er zijn veel community-sites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ook hebben Apache projecten project-sites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).
>
>

## <a name="install-hue-using-script-actions"></a>Met behulp van scriptacties Hue installeren

Het script Hue installeren op een Linux gebaseerde HDInsight-cluster is beschikbaar op https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. U kunt dit script Hue installeren op clusters met Azure Storage-Blobs (WASB) of Azure Data Lake Store als standaard opslag gebruiken.

Deze sectie bevat instructies over het gebruik van het script bij het inrichten van het cluster met behulp van de Azure-portal.

> [!NOTE]
> Azure PowerShell, de Azure CLI, de HDInsight .NET SDK of Azure Resource Manager-sjablonen kunnen ook worden gebruikt om toe te passen scriptacties. U kunt ook scriptacties toepassen op clusters al wordt uitgevoerd. Zie voor meer informatie [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Start de inrichting van een cluster met behulp van de stappen in [HDInsight-clusters inrichten op Linux](hdinsight-hadoop-provision-linux-clusters.md), maar inrichting niet voltooid.

   > [!NOTE]
   > Wilt Hue installeren op HDInsight-clusters, de grootte van de aanbevolen headnode is ten minste A4 (8 kerngeheugens, 14 GB geheugen).
   >
   >
2. Op de **optionele configuratie** blade Selecteer **scriptacties**, en geef de gegevens, zoals hieronder wordt weergegeven:

    ![Script actieparameters bieden voor Hue](./media/hdinsight-hadoop-hue-linux/hue-script-action.png "script actieparameters bieden voor Hue")

   * **NAAM**: een beschrijvende naam voor de scriptactie.
   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **HEAD**: Schakel deze optie
   * **WERKNEMER**: leeg laten.
   * **ZOOKEEPER**: leeg laten.
   * **PARAMETERS**: leeg laten.
3. Aan de onderkant van de **scriptacties**, gebruiken de **Selecteer** om op te slaan van de configuratie. Gebruik tot slot de **Selecteer** knop aan de onderkant van de **optionele configuratie** blade informatie optionele configuratie wilt opslaan.
4. Doorgaan met het inrichten van het cluster, zoals beschreven in [HDInsight-clusters inrichten op Linux](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Hue gebruiken met HDInsight-clusters

SSH-Tunneling is de enige manier om op het cluster toegang kunnen krijgen tot Hue zodra deze wordt uitgevoerd. Via SSH-tunneling, kunt het verkeer naar gaat u rechtstreeks naar de headnode van het cluster waarop Hue wordt uitgevoerd. Nadat het cluster inrichten is voltooid, gebruik de volgende stappen Hue op een HDInsight Linux-cluster.

> [!NOTE]
> U wordt aangeraden met Firefox-webbrowser aan de onderstaande instructies te volgen.
>
>

1. Gebruik de informatie in [gebruiken SSH-Tunneling voor toegang tot de Ambari-webgebruikersinterface, ResourceManager JobHistory, NameNode, Oozie en andere webgebruikersinterface van](hdinsight-linux-ambari-ssh-tunnel.md) voor een SSH-tunnel maken van uw clientsysteem naar het HDInsight-cluster en configureer vervolgens uw webbrowser om de SSH-tunnel gebruiken als een proxy.

2. Zodra u hebt een SSH-tunnel gemaakt en de browser op het verkeer via deze proxy geconfigureerd, moet u de hostnaam van de primaire hoofdknooppunt vinden. U kunt dit doen door verbinding te maken met het cluster via SSH op poort 22. Bijvoorbeeld: `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` waar **gebruikersnaam** uw SSH-gebruikersnaam en **CLUSTERNAME** is de naam van uw cluster.

    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

3. Eenmaal zijn verbonden, voer de volgende opdracht om de volledig gekwalificeerde domeinnaam van de primaire headnode:

        hostname -f

    Hiermee herstelt u een naam in de volgende strekking:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Dit is de hostnaam van de primaire headnode waar de Hue-website zich bevindt.
4. Gebruik de browser de Hue-portal op http://HOSTNAME:8888 openen. HOSTNAME vervangen door de naam die u hebt verkregen in de vorige stap.

   > [!NOTE]
   > Wanneer u zich voor het eerst aanmeldt, wordt u gevraagd om een account voor aanmelding bij de Hue-portal te maken. De referenties die u hier opgeeft, worden beperkt tot de portal en niet zijn gerelateerd aan de beheerder of de SSH-gebruikersreferenties die u hebt opgegeven tijdens het inrichten van het cluster.
   >
   >

    ![Meld u aan bij de Hue-portal](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Geef referenties op voor de Hue-portal")

### <a name="run-a-hive-query"></a>Een Hive-query uitvoeren
1. Klik in de Hue-portal op **Query Editors**, en klik vervolgens op **Hive** om het Hive-editor te openen.

    ![Hive gebruiken](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "Hive gebruiken")
2. Op de **helpen** tabblad onder **Database**, ziet u **hivesampletable**. Dit is een voorbeeldtabel dat bij alle Hadoop-clusters in HDInsight wordt geleverd. Voer een voorbeeldquery in het rechter deelvenster en de uitvoer zien op de **resultaten** tabblad in het deelvenster hieronder, zoals wordt weergegeven in de schermopname.

    ![Uitvoeren van Hive-query](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "uitvoeren Hive-query")

    U kunt ook de **grafiek** tabblad om te zien van een visuele representatie van het resultaat.

### <a name="browse-the-cluster-storage"></a>De clusteropslag bladeren
1. Klik in de Hue-portal op **bestandsbrowser** in de rechterbovenhoek van de menubalk.
2. Standaard wordt de browser geopend op de **/gebruiker/myuser** directory. Klik op de slash aan vóór de map van de gebruiker in het pad naar de hoofdmap van de Azure storage-container die is gekoppeld aan het cluster.

    ![Gebruik de Verkenner](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "gebruik bestandsbrowser")
3. Klik met de rechtermuisknop op een bestand of map om de beschikbare bewerkingen te bekijken. Gebruik de **uploaden** knop in de rechterhoek bestanden uploaden naar de huidige map. Gebruik de **nieuw** knop nieuwe bestanden of mappen te maken.

> [!NOTE]
> De browser Hue-bestand ziet alleen de inhoud van de standaard-container die is gekoppeld aan het HDInsight-cluster. Geen extra opslagruimte accounts/containers die u mogelijk hebt gekoppeld aan het cluster meer niet toegankelijk met behulp van de browser. De extra containers die zijn gekoppeld aan het cluster wordt echter altijd zijn toegankelijk voor de Hive-taken. Bijvoorbeeld, als u de opdracht `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` in de Hive-editor, ziet u de inhoud van evenals aanvullende containers. In deze opdracht **newcontainer** is niet de standaard-container die is gekoppeld aan een cluster.
>
>

## <a name="important-considerations"></a>Belangrijke overwegingen
1. Het script dat wordt gebruikt voor het installeren van Hue installeert deze alleen op de primaire headnode van het cluster.

2. Tijdens de installatie worden meerdere Hadoop-services (HDFS, YARN, MR2, Oozie) voor het bijwerken van de configuratie opnieuw gestart. Nadat het script is voltooid Hue installeren, kan het even duren voor andere Hadoop-services te starten. Dit mogelijk invloed op de prestaties van de Hue in eerste instantie. Nadat alle services worden gestart, wordt Hue volledig functioneel zijn.
3. HUE begrijpt niet Tez-taken, de huidige standaardinstelling voor Hive. Als u wilt u MapReduce gebruikt als de engine voor het uitvoeren van Hive, moet u het script voor het gebruik van de volgende opdracht in het script bijwerken:

        set hive.execution.engine=mr;

4. Met Linux-clusters hebt u een scenario waarbij uw services worden uitgevoerd op de primaire headnode terwijl de Resource Manager kan worden uitgevoerd op de secundaire server. Dit scenario kan leiden tot fouten (Zie hieronder) wanneer u Hue details wilt weergeven van taken wordt uitgevoerd op het cluster. U kunt echter de taakdetails weergeven wanneer de taak is voltooid.

   ![HUE-portal fout](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "portal Hue-fout")

   Dit is vanwege een bekend probleem. Als tijdelijke oplossing Ambari zodanig aanpassen dat de actieve Resource Manager wordt ook uitgevoerd op de primaire headnode.
5. HUE begrijpt WebHDFS bij gebruik maken van HDInsight-clusters met behulp van Azure Storage `wasb://`. Het aangepaste script gebruikt met scriptactie installeert dus WebWasb een WebHDFS compatibele service voor het praten met WASB. Ja, hoewel de Hue-portal HDFS op plaatsen zegt (zoals wanneer u de muisaanwijzer over de **bestandsbrowser**), moet dit worden beschouwd als WASB.

## <a name="next-steps"></a>Volgende stappen
* [Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md). Aanpassing van de cluster Giraph installeren op HDInsight Hadoop-clusters gebruiken. Giraph Hiermee kunt u de verwerking van de grafiek is met Hadoop en deze kan worden gebruikt met Azure HDInsight.
* [Solr installeren op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md). Aanpassing van de cluster Solr installeren op HDInsight Hadoop-clusters gebruiken. Solr kunt u krachtige zoekbewerkingen opgeslagen gegevens uitvoeren.
* [R installeren op HDInsight-clusters](hdinsight-hadoop-r-scripts-linux.md). Aanpassing van de cluster gebruiken voor het installeren van R op HDInsight Hadoop-clusters. R is een open source-taal en de omgeving voor statistische computing. Het biedt honderden ingebouwde statistische functies en een eigen programmeertaal die aspecten van het functionele en objectgeoriënteerd programmeren combineert. Het bevat ook uitgebreide grafische mogelijkheden.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
