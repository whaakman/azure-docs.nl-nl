---
title: Apache Hadoop-clusters in HDInsight met behulp van Azure portal beheren
description: Leer hoe u HDInsight-clusters met behulp van de Azure portal maken en beheren.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/26/2018
ms.author: hrasheed
ms.openlocfilehash: ce30b752ecf1d5413ae534fa03907cbf11b1c694
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794478"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Apache Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Met behulp van de [Azure-portal][azure-portal], kunt u beheren [Apache Hadoop](https://hadoop.apache.org/) -clusters in Azure HDInsight. Gebruik de tabselector boven voor informatie over het beheren van Hadoop-clusters in HDInsight met andere hulpprogramma's.

**Vereisten**
- Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een bestaande Apache Hadoop-cluster in HDInsight.  Zie [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="getting-started"></a>Aan de slag
Meld u aan bij [https://portal.azure.com](https://portal.azure.com).


## <a name="showClusters"></a> Clusters tonen en vermelden
De **HDInsight-clusters** pagina wordt een lijst met uw bestaande clusters.  Vanuit de portal:
1. Selecteer **alle services** in het menu links.
2. Selecteer **HDInsight-clusters** onder **ANALYTICS**.

## <a name="homePage"></a> Cluster-startpagina 
Selecteer de clusternaam van uw vanaf het [ **HDInsight-clusters** pagina](#showClusters).  Hiermee opent u de **overzicht** weergave, die op de volgende afbeelding lijkt:

![Azure portal HDInsight-cluster essentials](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials2.png)

**Menu bovenaan:**  
- **Verplaats**: Hiermee wordt het cluster verplaatst naar een andere resourcegroep of een ander abonnement.  
- **Verwijder**: Hiermee verwijdert u het cluster.  
- **Vernieuwen**:  Hiermee vernieuwt u de weergave.

**Linkermenu:**  
 - **Menu linksboven**  
    - **Overzicht**:  Bevat algemene informatie voor uw cluster.
    -  **Activiteitenlogboek**: Weergeven en activiteitenlogboeken op te vragen.
    - **Toegangsbeheer (IAM)**: Roltoewijzingen gebruiken.  Zie [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md).
    - **Tags**: Hiermee kunt u instellen van sleutel/waarde-paren voor het definiëren van een aangepaste taxonomie van uw cloud-services. Bijvoorbeeld, u kunt maken met een sleutel met de naam **project**, en vervolgens een overeenkomende waarde gebruiken voor alle services die zijn gekoppeld aan een bepaald project.
    - **Problemen vaststellen en oplossen**: Probleemoplossingsinformatie weergeven.
    - **Snel aan de slag**:  Geeft informatie weer die u helpt aan de slag met HDInsight.
    - **Hulpprogramma's**: Help-informatie voor HDInsight gerelateerde hulpprogramma's.

- **Instellingenmenu**  
  - **Clustergrootte**: Controleren, verhogen en verlagen van het aantal worker-clusterknooppunten. Zie [schalen van clusters](hdinsight-administer-use-management-portal.md#scale-clusters).
  - **De quotalimieten**: De gebruikte en beschikbare cores voor uw abonnement worden weergegeven.
  - **SSH en clusteraanmelding**: Ziet u de instructies voor het verbinden met het cluster via Secure Shell (SSH)-verbinding. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
  - **Data Lake Storage Gen1**: Toegang tot Data Lake Storage Gen1 configureren.  Zie [Quick Start: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).
  - **Storage-accounts**: De storage-accounts en de sleutels weergeven. De storage-accounts zijn geconfigureerd tijdens het maken van het cluster.
  - **Toepassingen**: HDInsight-toepassingen toevoegen/verwijderen.  Zie [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).
  - **Scriptacties**: Bash-scripts uitvoeren op het cluster. Zie [aanpassen Linux gebaseerde HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md).
  - **Externe metastores**: Weergave de [Apache Hive](https://hive.apache.org/) en [Apache Oozie](https://oozie.apache.org/) metastores. De metastores kan alleen worden geconfigureerd tijdens het maken van het cluster.
  - **HDInsight-partner**: De huidige HDInsight-Partner toevoegen/verwijderen.
  - **Eigenschappen**: Weergave de [eigenschappen van het cluster](#properties).
  - **Hiermee vergrendelt u**: Voeg een vergrendeling om te voorkomen dat het cluster worden gewijzigd of verwijderd.
  -  **Automatiseringsscript**: Weergeven en exporteren van het Azure Resource Manager-sjabloon voor het cluster. U kunt op dit moment alleen het afhankelijke Azure storage-account exporteren. Zie [Linux maken op basis van Apache Hadoop-clusters in HDInsight met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

- **Bewaking van menu**
  - **Waarschuwingen**: De waarschuwingen en acties beheren.
  - **Metrische gegevens**: Controleer de metrische gegevens voor het cluster in Azure Log Analytics.
  - **Diagnose-instellingen**: Instellingen voor het opslaan van de diagnose metrische gegevens.
  - **Operations Management Suite**:  Controleer uw cluster in Azure Operations Management Suite (OMS) en Azure Log Analytics.

- **Ondersteuning en probleemoplossing van menu**
  - **Resourcestatus**: Zie [overzicht van Azure resource health](../service-health/resource-health-overview.md).
  - **Nieuwe ondersteuningsaanvraag**: Hiermee kunt u een ondersteuningsticket maken met Microsoft ondersteuning.
    
## <a name="properties"></a> Eigenschappen van cluster
Uit de [cluster startpagina](#homePage)onder **instellingen** Selecteer **eigenschappen**.
* **Hostnaam**: De naam van het cluster.
* **Cluster-URL**: De URL van de Ambari-webinterface.
* **Secure shell (SSH)**: De gebruikersnaam en het host-naam moet worden gebruikt bij het openen van het cluster via SSH.
* **Status**: Een van: Afgebroken, geaccepteerd, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, operationeel is, wordt uitgevoerd, fout, te verwijderen, verwijderd, time-out, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, of ClusterCustomization.
* **Regio**: Azure-locatie. Zie voor een lijst van ondersteunde Azure-locaties, de **regio** vervolgkeuzelijst vak op [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).
* **Datum gemaakt**: De datum waarop die het cluster is geïmplementeerd.
* **Besturingssysteem**: Een van beide **Windows** of **Linux**.
* **Type**: Hadoop, HBase, Storm, Spark.
* **Versie**. Zie [HDInsight-versies](hdinsight-component-versioning.md).
* **Abonnement**: Abonnementsnaam.
* **Standaardgegevensbron**: Het standaardbestandssysteem cluster.
* **Grootte van de worker-knooppunten**: De geselecteerde VM-grootte van de worker-knooppunten.
* **De grootte van knooppunt HEAD**: De geselecteerde VM-grootte van de hoofdknooppunten.
* **Virtueel netwerk**: De naam van het Virtueelnetwerk dat het cluster wordt geïmplementeerd als een is geselecteerd tijdens de implementatie.

## <a name="move-clusters"></a>Clusters verplaatsen

U kunt een HDInsight-cluster verplaatsen naar een andere Azure-resourcegroep of een ander abonnement. 

Uit de [cluster startpagina](#homePage):

1. Selecteer **verplaatsen** in het menu bovenaan.
2. Selecteer **verplaatsen naar een andere resourcegroep** of **verplaatsen naar een ander abonnement**.
3. Volg de instructies van de nieuwe pagina.

## <a name="delete-clusters"></a>Clusters verwijderen
Verwijderen van een cluster, worden het standaardaccount voor opslag en alle gekoppelde opslagaccounts niet verwijderen. U kunt het cluster opnieuw maken met behulp van de dezelfde opslagaccounts en de dezelfde metastores. Het is raadzaam om met behulp van een nieuwe standaard Blob-container wanneer u het cluster opnieuw maakt.

Uit de [cluster startpagina](#homePage):

1. Selecteer **verwijderen** in het menu bovenaan.
2. Volg de instructies van de nieuwe pagina.

Zie ook [onderbreken/clusters afsluiten](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Extra opslagaccounts toevoegen

U kunt aanvullende Azure Storage-accounts en Azure Data Lake Storage-accounts toevoegen nadat een cluster is gemaakt. Zie [Extra opslagaccounts toevoegen aan HDInsight](./hdinsight-hadoop-add-storage.md) voor meer informatie.

## <a name="scale-clusters"></a>Clusters schalen
Het cluster schalen functie kunt u het aantal worker-knooppunten die worden gebruikt door een Azure HDInsight-cluster, zonder te hoeven maken van het cluster opnieuw wijzigen.

> [!NOTE]  
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de eigenschappenpagina controleren.  Zie [clusters tonen en vermelden](#list-and-show-clusters).

Uit de [cluster startpagina](#homePage):

1. Onder **instellingen**, selecteer **clustergrootte**.
2. Voer **nummer van de Worker-knooppunten** in het vak numerieke tekst. De limiet voor het aantal clusterknooppunten verschilt tussen Azure-abonnementen. U kunt contact opnemen met ondersteuning voor facturering om de limiet te verhogen.  De kostengegevens weerspiegelt de wijzigingen die u hebt aangebracht in het aantal knooppunten.
3. Selecteer **Opslaan**.

    ![HDInsight hadoop hbase, storm spark schaal](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster2.png)

De gevolgen van het wijzigen van het aantal gegevensknooppunten varieert voor elk type cluster die door HDInsight worden ondersteund:

* Apache Hadoop

    Het aantal worker-knooppunten in een Hadoop-cluster dat wordt uitgevoerd zonder gevolgen voor alle taken die in behandeling of wordt uitgevoerd, kunt u naadloos verhogen. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor vergroten/verkleinen probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster is omlaag geschaald door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw opgestart. Dit gedrag zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking vergroten/verkleinen. U kunt echter de taken opnieuw zodra de bewerking voltooid is.
* Apache HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de vergroten/verkleinen bewerking. U kunt echter ook handmatig de regionale servers verdelen door te melden bij het hoofdknooppunt van het cluster en de volgende opdrachten uitvoert vanuit een opdrachtpromptvenster:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    Zie voor meer informatie over het gebruik van de HBase-shell [aan de slag met een voorbeeld voor Apache HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md).

* Apache Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Na een geslaagde bewerking vergroten/verkleinen is voltooid moet u wordt echter opnieuw verdelen van de topologie.

    Herverdelen kan worden uitgevoerd op twee manieren:

  * Storm-Webgebruikersinterface
  * Opdrachtregelinterface (CLI)-hulpprogramma

    Raadpleeg de [Apache Storm documentatie](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight Storm schaal opnieuw verdelen](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Hier volgt een voorbeeld van de CLI-opdracht voor het opnieuw verdelen van de Storm-topologie:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```


## <a name="pauseshut-down-clusters"></a>Onderbreken/clusters afsluiten

De meeste van Hadoop-taken zijn batchtaken die alleen af en toe worden uitgevoerd. Er zijn grote perioden die het cluster niet wordt gebruikt voor de verwerking voor de meeste Hadoop-clusters. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt.
Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Er zijn veel manieren waarop u het proces kunt programmeren:

* Gebruiker Azure-Gegevensfactory. Zie [maken op aanvraag op basis van Linux Apache Hadoop-clusters in HDInsight met behulp van Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) gekoppelde services voor het maken van HDInsight op aanvraag.
* Azure PowerShell gebruiken.  Zie [gegevens over vertraagde vluchten analyseren](hdinsight-analyze-flight-delay-data.md).
* Gebruik Azure klassieke CLI. Zie [beheren HDInsight-clusters met behulp van de klassieke Azure-CLI](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK gebruiken. Zie [indienen Apache Hadoop-taken](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Zie voor informatie over de prijzen, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/). Als een cluster verwijderen uit de Portal, Zie [clusters verwijderen](#delete-clusters)



## <a name="upgrade-clusters"></a>Bijwerken van clusters

Zie [Upgrade HDInsight-cluster naar een nieuwere versie](./hdinsight-upgrade-cluster.md).

## <a name="open-the-apache-ambari-web-ui"></a>Open de Apache Ambari-Webgebruikersinterface

Ambari biedt een intuïtieve, eenvoudig te gebruiken Hadoop management-Webgebruikersinterface ondersteund door de RESTful-API's. Ambari kan systeembeheerders het beheren en bewaken van Hadoop-clusters.

Uit de [cluster startpagina](#homePage):

1. Selecteer **Clusterdashboards**.

    ![Menu van HDInsight Hadoop-cluster](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu2.png)

1. Selecteer **Ambari home** vanuit de nieuwe pagina.
2. Voer de cluster-gebruikersnaam en wachtwoord.  De gebruikersnaam van het standaard-cluster is _admin_. De Ambari web UI ziet eruit zoals:

Zie voor meer informatie, [beheren HDInsight-clusters met behulp van de Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

## <a name="change-passwords"></a>Wachtwoorden wijzigen
Een HDInsight-cluster kan twee gebruikersaccounts hebben. De HDInsight-cluster gebruikersaccount (ook wel) HTTP-gebruikersaccount) en de SSH-gebruikersaccount worden gemaakt tijdens het maken. U kunt de portal gebruiken om het cluster het wachtwoord voor gebruikersaccount en scriptacties te wijzigen van de SSH-gebruikersaccount te wijzigen.

### <a name="change-the-cluster-user-password"></a>Het wachtwoord van de cluster-gebruiker wijzigen

> [!NOTE]  
> Het wachtwoord van de cluster-gebruiker (beheerder) wijzigt, kan scriptacties uitvoeren op dit cluster mislukken. Als u een persistente scriptacties die doel worker-knooppunten hebt, is deze scripts mislukken als u knooppunten aan het cluster via vergroten of verkleinen operations toevoegt. Zie voor meer informatie over scriptacties [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

Uit de [cluster startpagina](#homePage):
1. Selecteer **SSH- + Cluster aanmelding** onder **instellingen**.
2. Selecteer **referentie opnieuw instellen**.
3. Typ en Bevestig nieuw wachtwoord in de tekstvakken.
4. Selecteer **OK**.

Het wachtwoord is gewijzigd op alle knooppunten in het cluster.

### <a name="change-the-ssh-user-password"></a>Het SSH-gebruikerswachtwoord wijzigen
1. De volgende tekst met een teksteditor opslaan als een bestand met de naam **changepassword.sh**.

    > [!IMPORTANT]  
    > U moet een editor die gebruikmaakt van LF als het einde van regel gebruiken. Als de editor wordt CRLF gebruikt, klikt u vervolgens werkt het script niet.

    ```bash
    #! /bin/bash
    USER=$1
    PASS=$2
    usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
    ```

2. Upload het bestand naar een opslaglocatie bevinden die toegankelijk is vanuit HDInsight met behulp van een HTTP of HTTPS-adres. Bijvoorbeeld een bestand met de openbare opslaan, zoals OneDrive of Azure Blob storage. Sla de URI (HTTP of HTTPS-adres) naar het bestand, zoals deze URI is vereist in de volgende stap.
3. Uit de [cluster startpagina](#homePage) Selecteer **scriptacties** onder **instellingen**.
4. Uit de **scriptacties** Selecteer **nieuwe verzenden**. 
5. Uit de **scriptactie verzenden** blade, voer de volgende informatie:

   | Veld | Waarde |
   | --- | --- |
   | Scripttype | Selecteer **: aangepaste** uit de vervolgkeuzelijst.|
   | Name |'Ssh wachtwoord wijzigen' |
   | Bash-script-URI |De URI moet het bestand changepassword.sh |
   | Knooppunttype(n): (Head, werknemer, Nimbus, Supervisor, Zookeeper, enz.) |✓ voor alle typen vermeld |
   | Parameters |Voer de naam van de SSH-gebruiker en klik vervolgens op het nieuwe wachtwoord. Er moet één ruimte tussen de gebruikersnaam en het wachtwoord. |
   | Deze scriptactie... |Laat dit veld uitgeschakeld. |
5. Selecteer **maken** om toe te passen van het script. Als het script is voltooid, bent u geen verbinding maken met het cluster via SSH met het nieuwe wachtwoord.

## <a name="grantrevoke-access"></a>Toegang verlenen of in te trekken
HDInsight-clusters hebben de volgende HTTP-webservices (al deze services hebben RESTful-eindpunten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Standaard worden deze services worden verleend om toegang te krijgen. U kunt in te trekken/verlenen de toegang via [klassieke Azure-CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) en [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>De abonnements-ID vinden
Elk cluster is gekoppeld aan een Azure-abonnement.  Het Azure-abonnement-ID zichtbaar zijn vanaf is de [cluster startpagina](#homePage).

## <a name="find-the-resource-group"></a>Zoek de resourcegroep
In de modus Azure Resource Manager wordt elke HDInsight-cluster gemaakt met een Azure Resource Manager-groep. De Resource Manager-groep is zichtbaar zijn vanaf de [cluster startpagina](#homePage).

## <a name="find-the-storage-accounts"></a>De storage-accounts vinden
HDInsight-clusters maken gebruik van een Azure Storage-account of een Azure Data Lake Storage voor het opslaan van gegevens. Elke HDInsight-cluster kan één standaard-opslagaccount en een aantal gekoppelde storage-accounts hebben. Om de storage-accounts van weer te geven de [cluster startpagina](#homePage) onder **instellingen**, selecteer **opslagaccounts**.


## <a name="monitor-jobs"></a>Taken controleren
Zie [beheren HDInsight-clusters met behulp van de Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md#monitoring).


## <a name="monitor-cluster-usage"></a>Cluster-gebruik bewaken
De **gebruik** sectie van de HDInsight-cluster-blade vindt u informatie over het aantal kernen beschikbaar voor uw abonnement voor gebruik met HDInsight, evenals het aantal kernen dat is toegewezen aan dit cluster en hoe ze worden toegewezen voor de knooppunten in dit cluster. Zie [clusters tonen en vermelden](#list-and-show-clusters).

> [!IMPORTANT]  
> Voor het controleren van de services die worden geleverd door het HDInsight-cluster, moet u de Ambari Web- of de Ambari REST-API gebruiken. Zie voor meer informatie over het gebruik van Ambari [beheren HDInsight-clusters met behulp van Apache Ambari](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Verbinding maken met een cluster

* [Apache Hive gebruiken met HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u enkele eenvoudige beheerfuncties geleerd. Zie de volgende artikelen voor meer informatie:

* [HDInsight met behulp van Azure PowerShell beheren](hdinsight-administer-use-powershell.md)
* [HDInsight met behulp van Azure Classic CLI beheren](hdinsight-administer-use-command-line.md)
* [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md)
* [Meer informatie over het gebruik van de Apache Ambari-Webinterface](hdinsight-hadoop-manage-ambari.md)
* [Informatie over het gebruik van de Apache Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Apache Hive in HDInsight gebruiken](hadoop/hdinsight-use-hive.md)
* [Apache Pig in HDInsight gebruiken](hadoop/hdinsight-use-pig.md)
* [Apache Sqoop gebruiken in HDInsight](hadoop/hdinsight-use-sqoop.md)
* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Welke versie van Apache Hadoop is in Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop-opdrachtregel"
