---
title: Beheer Hadoop-clusters in HDInsight met behulp van Azure portal
description: Leer hoe u HDInsight-clusters met behulp van de Azure portal maken en beheren.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: jasonh
ms.openlocfilehash: 20a48dcd4a9c3dd4c89390c1048ec4fd5f5783ae
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597205"
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Met behulp van de [Azure-portal][azure-portal], kunt u Hadoop-clusters in Azure HDInsight beheren. Gebruik de tabselector boven voor informatie over het beheren van Hadoop-clusters in HDInsight met andere hulpprogramma's.

**Vereiste**

Als u wilt volgen de stappen in dit artikel, moet u een **Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-azure-portal"></a>De Azure-portal openen
1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com).
2. Nadat u de portal opent, kunt u het volgende doen:

   * Klik op **een resource maken** in het menu links om een nieuwe cluster te maken:

       ![knop Nieuwe HDInsight-cluster](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)

       Voer **HDInsight** in **Marketplace doorzoeken**, klikt u op **HDInsight**, en klik vervolgens op **maken**.

   * Klik op **HDInsight-Clusters** in het menu links om de bestaande clusters weer te geven:

       ![Azure HDInsight-cluster de knop portal](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Als er geen de **HDInsight-clusters** knop en klik vervolgens op **HDInsight-clusters** onder de **Intelligence en analyse** sectie.


## <a name="create-clusters"></a>Clusters maken
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight werkt met een breed bereik van Hadoop-onderdelen. Zie voor een lijst van de onderdelen die worden gecontroleerd en wordt ondersteund, [is welke versie van Hadoop in Azure HDInsight?](hdinsight-component-versioning.md) Zie voor meer informatie het maken van de algemene cluster, [Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Vereisten voor toegangsbeheer

Wanneer u een HDInsight-cluster maakt, moet u een Azure-abonnement opgeven. Het cluster kan worden gemaakt in een nieuwe Azure-resourcegroep of in een bestaande resourcegroep. U kunt de volgende stappen uit om te controleren of uw machtigingen voor het maken van HDInsight-clusters gebruiken:

- Een nieuwe resourcegroep maken:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Klik op **abonnement** in het menu links. Het heeft pictogram van een gele sleutel. U ziet een lijst met abonnementen.
    3. Klik op het abonnement dat u gebruiken om clusters te maken. 
    4. Klik op **mijn machtigingen**.  U ziet uw [rol](../role-based-access-control/built-in-roles.md) van het abonnement. U moet minimaal Inzender-toegang tot het HDInsight-cluster maken.

- Een bestaande resourcegroep gebruiken:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Klik op **resourcegroepen** in het menu links om de resourcegroepen weer te geven.
    3. Klik op de resourcegroep die u gebruiken wilt voor het maken van uw HDInsight-cluster.
    4. Klik op **toegangsbeheer (IAM)**, en controleer u (of een groep die u deel uitmaakt) hebben ten minste de Inzender-toegang tot de resourcegroep.

Als u de fout NoRegisteredProviderFound of de fout MissingSubscriptionRegistration ontvangt, raadpleegt u [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Clusters tonen en vermelden
1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com).
2. Klik op **HDInsight-Clusters** in het menu links om de bestaande clusters weer te geven. Als er geen **HDInsight-Clusters**, klikt u op **alle services** eerste.
3. Klik op de naam van het cluster. Als de clusterlijst te lang is, kunt u filteren boven aan de pagina.
4. Klik op een cluster op basis van de lijst om te zien van de overzichtspagina:

    ![Azure portal HDInsight-cluster essentials](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png) **menu overzicht:**
    * **Dashboard**: Hiermee opent u de Ambari-Webgebruikersinterface voor het cluster.
    * **Secure Shell**: ziet u de instructies voor het verbinden met het cluster via Secure Shell (SSH)-verbinding.
    * **Cluster schalen**: Hiermee kunt u het aantal worker-knooppunten voor dit cluster te wijzigen.
    * **Verplaats**: het cluster wordt verplaatst naar een andere resourcegroep of een ander abonnement.
    * **Verwijder**: Hiermee verwijdert u het cluster.

    **Linkermenu:**
    * **Activiteitenlogboeken**: activiteitenlogboeken weergeven en query's uitvoeren.
    * **Toegangsbeheer (IAM)**: roltoewijzingen gebruiken.  Zie [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md).
    * **Tags**: Hiermee kunt u instellen van sleutel/waarde-paren voor het definiëren van een aangepaste taxonomie van uw cloud-services. Bijvoorbeeld, u kunt maken met een sleutel met de naam **project**, en vervolgens een overeenkomende waarde gebruiken voor alle services die zijn gekoppeld aan een bepaald project.
    * **Problemen vaststellen en oplossen**: informatie over probleemoplossing weergeven.
    * **Hiermee vergrendelt u**: toevoegen van een vergrendeling om te voorkomen dat het cluster dat wordt gewijzigd of verwijderd.
    * **Automatiseringsscript**: weergeven en exporteren van de Azure Resource Manager-sjabloon voor het cluster. U kunt op dit moment alleen het afhankelijke Azure storage-account exporteren. Zie [maken Linux gebaseerde Hadoop-clusters in HDInsight met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Snel aan de slag**: geeft informatie weer die u helpt aan de slag met HDInsight.
    * **Hulpprogramma's voor HDInsight**: Help-informatie voor HDInsight gerelateerde hulpprogramma's.
    * **Abonnementsgebruik Core**: de gebruikte en beschikbare cores voor uw abonnement worden weergegeven.
    * **Cluster schalen**: vergroten en verkleinen het aantal worker-clusterknooppunten. Zie[schalen van clusters](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **SSH en clusteraanmelding**: ziet u de instructies voor het verbinden met het cluster via Secure Shell (SSH)-verbinding. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
    * **HDInsight-Partner**: de huidige HDInsight-Partner toevoegen of verwijderen.
    * **Externe Metastores**: de Hive- en Oozie metastores weergeven. De metastores kan alleen worden geconfigureerd tijdens het maken van het cluster. Zie [gebruik Hive/Oozie-metastore](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Scriptacties**: uitvoeren Bash-scripts in het cluster. Zie [aanpassen Linux gebaseerde HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md).
    * **Toepassingen**: toevoegen/verwijderen HDInsight-toepassingen.  Zie [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).
    * **Bewaking**: het cluster in Azure Log Analytics controleren.
    * **Eigenschappen**: de clustereigenschappen te bekijken.
    * **Storage-accounts**: de storage-accounts en de sleutels weergeven. De storage-accounts zijn geconfigureerd tijdens het maken van het cluster.
    * **Data Lake Store-toegang**: configureren van toegang tot Data Lake worden opgeslagen.  Zie [Quick Start: clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).
    * **Resourcestatus**: Zie [overzicht van Azure resource health](../service-health/resource-health-overview.md).
    * **Nieuwe ondersteuningsaanvraag**: kunt u een ondersteuningsticket maken met Microsoft ondersteuning.
    
6. Klik op **eigenschappen**:

    De eigenschappen zijn:

   * **Hostnaam**: de naam van het Cluster.
   * **Cluster-URL**: de URL voor de Ambari-webinterface.
   * **Secure shell (SSH)**: de gebruikersnaam en het host-naam moet worden gebruikt bij het openen van het cluster via SSH.
   * **Status**: een van: afgebroken, geaccepteerd, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, operationeel is, wordt uitgevoerd, fout, te verwijderen, verwijderd, time-out, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued of ClusterCustomization.
   * **Regio**: Azure-locatie. Zie voor een lijst van ondersteunde Azure-locaties, de **regio** vervolgkeuzelijst op [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Datum gemaakt**: de datum die het cluster is geïmplementeerd.
   * **Besturingssysteem**: beide **Windows** of **Linux**.
   * **Type**: Hadoop, HBase, Storm, Spark.
   * **Versie**. Zie [HDInsight-versies](hdinsight-component-versioning.md).
   * **Abonnement**: de naam van abonnement.
   * **Standaardgegevensbron**: het standaardbestandssysteem voor het cluster.
   * **Grootte van de worker-knooppunten**: de geselecteerde VM-grootte van de worker-knooppunten.
   * **De grootte van knooppunt HEAD**: de geselecteerde VM-grootte van de hoofdknooppunten.
   * **Virtueel netwerk**: de naam van het Virtueelnetwerk dat het cluster wordt geïmplementeerd als een is geselecteerd tijdens de implementatie.

## <a name="delete-clusters"></a>Clusters verwijderen
Verwijderen van een cluster, worden het standaardaccount voor opslag en alle gekoppelde opslagaccounts niet verwijderen. U kunt het cluster opnieuw maken met behulp van de dezelfde opslagaccounts en de dezelfde metastores. Het is raadzaam om met behulp van een nieuwe standaard Blob-container wanneer u het cluster opnieuw maakt.

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **HDInsight-Clusters** in het menu links. Als er geen **HDInsight-Clusters**, klikt u op **alle services** eerste.
3. Klik op het cluster dat u wilt verwijderen.
4. Klik op **verwijderen** in het menu bovenaan en volg de instructies.

Zie ook [onderbreken/clusters afsluiten](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Extra opslagaccounts toevoegen

U kunt aanvullende Azure Storage-accounts en Azure Data Lake Store-accounts toevoegen nadat een cluster is gemaakt. Zie [Extra opslagaccounts toevoegen aan HDInsight](./hdinsight-hadoop-add-storage.md) voor meer informatie.

## <a name="scale-clusters"></a>Clusters schalen
Het cluster schalen functie kunt u het aantal worker-knooppunten die worden gebruikt door een Azure HDInsight-cluster, zonder te hoeven maken van het cluster opnieuw wijzigen.

> [!NOTE]
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de eigenschappenpagina controleren.  Zie [clusters tonen en vermelden](#list-and-show-clusters).
>
>

De gevolgen van het wijzigen van het aantal gegevensknooppunten varieert voor elk type cluster die door HDInsight worden ondersteund:

* Hadoop

    Het aantal worker-knooppunten in een Hadoop-cluster dat wordt uitgevoerd zonder gevolgen voor alle taken die in behandeling of wordt uitgevoerd, kunt u naadloos verhogen. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor vergroten/verkleinen probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster is omlaag geschaald door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw opgestart. Dit gedrag zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking vergroten/verkleinen. U kunt echter de taken opnieuw zodra de bewerking voltooid is.
* HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de vergroten/verkleinen bewerking. U kunt echter ook handmatig de regionale servers verdelen door te melden bij het hoofdknooppunt van het cluster en de volgende opdrachten uitvoert vanuit een opdrachtpromptvenster:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

    Zie voor meer informatie over het gebruik van de HBase-shell [aan de slag met een voorbeeld voor Apache HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md).

* Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Na een geslaagde bewerking vergroten/verkleinen is voltooid moet u wordt echter opnieuw verdelen van de topologie.

    Herverdelen kan worden uitgevoerd op twee manieren:

  * Storm-Webgebruikersinterface
  * Opdrachtregelinterface (CLI)-hulpprogramma

    Raadpleeg de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight Storm schaal opnieuw verdelen](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Hier volgt een voorbeeld van de CLI-opdracht voor het opnieuw verdelen van de Storm-topologie:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

**Voor het schalen van clusters**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **HDInsight-Clusters** in het menu links.
3. Klik op het cluster dat u wilt schalen.
3. Klik op **Cluster schalen**.
4. Voer **nummer van de Worker-knooppunten**. De limiet voor het aantal clusterknooppunten verschilt tussen Azure-abonnementen. U kunt contact opnemen met ondersteuning voor facturering om de limiet te verhogen.  De kostengegevens weerspiegelt de wijzigingen die u hebt aangebracht in het aantal knooppunten.

    ![HDInsight hadoop hbase, storm spark schaal](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Onderbreken/clusters afsluiten

De meeste van Hadoop-taken zijn batchtaken die alleen af en toe worden uitgevoerd. Er zijn grote perioden die het cluster niet wordt gebruikt voor de verwerking voor de meeste Hadoop-clusters. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt.
Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Er zijn veel manieren waarop u het proces kunt programmeren:

* Gebruiker Azure-Gegevensfactory. Zie [maken bellen op Linux gebaseerde Hadoop-clusters in HDInsight met behulp van Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) gekoppelde services voor het maken van HDInsight op aanvraag.
* Azure PowerShell gebruiken.  Zie [gegevens over vertraagde vluchten analyseren](hdinsight-analyze-flight-delay-data.md).
* Azure CLI gebruiken. Zie [beheren HDInsight-clusters met behulp van Azure CLI](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK gebruiken. Zie [indienen Hadoop-taken](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Zie voor informatie over de prijzen, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/). Als een cluster verwijderen uit de Portal, Zie [clusters verwijderen](#delete-clusters)

## <a name="move-cluster"></a>Cluster verplaatsen

U kunt een HDInsight-cluster verplaatsen naar een andere Azure-resourcegroep of een ander abonnement.  Zie [clusters tonen en vermelden](#list-and-show-clusters).

## <a name="upgrade-clusters"></a>Bijwerken van clusters

Zie [Upgrade HDInsight-cluster naar een nieuwere versie](./hdinsight-upgrade-cluster.md).

## <a name="open-the-ambari-web-ui"></a>Open de Ambari-Webgebruikersinterface

Ambari biedt een intuïtieve, eenvoudig te gebruiken Hadoop management-Webgebruikersinterface ondersteund door de RESTful-API's. Ambari kan systeembeheerders het beheren en bewaken van Hadoop-clusters.

1. Open een HDInsight-cluster in Azure portal.  Zie [clusters tonen en vermelden](#list-and-show-clusters).
2. Klik op **Cluster-Dashboard**.

    ![Menu van HDInsight Hadoop-cluster](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu.png)

1. Voer de cluster-gebruikersnaam en wachtwoord.  De gebruikersnaam van het standaard-cluster is _admin_. De Ambari web UI ziet eruit zoals:

    ![HDInsight Hadoop Ambari-Webinterface](./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-ambari-web-ui.png)

Zie voor meer informatie, [beheren HDInsight-clusters met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

## <a name="change-passwords"></a>Wachtwoorden wijzigen
Een HDInsight-cluster kan twee gebruikersaccounts hebben. De HDInsight-cluster gebruikersaccount (ook wel) HTTP-gebruikersaccount) en de SSH-gebruikersaccount worden gemaakt tijdens het maken. U kunt de Ambari-Webgebruikersinterface gebruiken om te wijzigen van de gebruikersnaam voor account cluster en het wachtwoord en de scriptacties te wijzigen van de SSH-gebruikersaccount

### <a name="change-the-cluster-user-password"></a>Het wachtwoord van de cluster-gebruiker wijzigen
De Ambari-Webgebruikersinterface kunt u het wachtwoord van de Cluster-gebruiker wijzigen. Als u wilt zich aanmeldt bij de Ambari, moet u het bestaande cluster-gebruikersnaam en wachtwoord gebruiken.

> [!NOTE]
> Het wachtwoord van de cluster-gebruiker (beheerder) wijzigt, kan scriptacties uitvoeren op dit cluster mislukken. Als u een persistente scriptacties die doel worker-knooppunten hebt, is deze scripts mislukken als u knooppunten aan het cluster via vergroten of verkleinen operations toevoegt. Zie voor meer informatie over scriptacties [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Aanmelden bij de Ambari-Webinterface met behulp van de gebruikersreferenties van de HDInsight-cluster. De standaardgebruikersnaam **admin**. De URL is **https://&lt;de naam van de HDInsight-Cluster > azurehdinsight.net**.
2. Klik op **Admin** in het bovenste menu en klik vervolgens op 'Ambari beheren'.
3. Klik in het menu links op **gebruikers**.
4. Klik op **Admin**.
5. Klik op **wachtwoord wijzigen**.

Ambari wordt vervolgens het wachtwoord op alle knooppunten in het cluster.

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
3. Vanuit de Azure-portal, klikt u op **HDInsight-Clusters**.
4. Klik op uw HDInsight-cluster.
4. Klik op **scriptacties**.
4. Uit de **scriptacties** Selecteer **nieuwe verzenden**. Wanneer de **scriptactie verzenden** blade wordt weergegeven, voer de volgende informatie:

   | Veld | Waarde |
   | --- | --- |
   | Naam |Ssh wachtwoord wijzigen |
   | Bash-script-URI |De URI moet het bestand changepassword.sh |
   | Knooppunten (Head, werknemer, Nimbus, Supervisor, Zookeeper, enz.) |✓ voor alle typen vermeld |
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

Standaard worden deze services worden verleend om toegang te krijgen. U kunt in te trekken/verlenen de toegang via [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) en [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>De abonnements-ID vinden

**Uw Azure-abonnement aanleveren vinden**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **abonnementen**. Elk abonnement heeft een naam en een ID.

Elk cluster is gekoppeld aan een Azure-abonnement. Het abonnement ID wordt weergegeven op het cluster **essentiële** tegel. Zie [clusters tonen en vermelden](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Zoek de resourcegroep
In de modus Azure Resource Manager wordt elke HDInsight-cluster gemaakt met een Azure Resource Manager-groep. De Resource Manager-groep die deel uitmaakt van een cluster te wordt weergegeven in:

* De clusterlijst heeft een **resourcegroep** kolom.
* Cluster **essentiële** tegel.  

Zie [clusters tonen en vermelden](#list-and-show-clusters).

## <a name="find-the-storage-accounts"></a>De storage-accounts vinden

HDInsight-clusters een Azure Storage-account of een Azure Data Lake Store gebruiken voor het opslaan van gegevens. Elke HDInsight-cluster kan één standaard-opslagaccount en een aantal gekoppelde storage-accounts hebben. Als u de storage-accounts, u het cluster vanuit de portal voor het eerst opent, en klik vervolgens op **opslagaccounts**:

![HDInsight-cluster storage-accounts](./media/hdinsight-administer-use-portal-linux/hdinsight-storage-accounts.png)

Op de vorige schermafbeelding, er is een __standaard__ kolom die aangeeft of het account het standaardopslagaccount is.

Als u de Data Lake Store-accounts, klikt u op **Data Lake Store-toegang** in de vorige schermafbeelding.

## <a name="run-hive-queries"></a>Hive-query's uitvoeren
Hive-taak niet rechtstreeks vanuit de Azure portal kan niet worden uitgevoerd, maar u kunt de weergave Hive op Ambari-Webinterface gebruiken.

**Uitvoeren van Hive-query's met de Ambari Hive-weergave**

1. Aanmelden bij de Ambari-Webinterface met behulp van de gebruikersreferenties van de HDInsight-cluster. De standaardgebruikersnaam **admin**. De URL is **https://&lt;de naam van de HDInsight-Cluster > azurehdinsight.net**.
2. Open de Hive-weergave zoals wordt weergegeven in de volgende schermafbeelding:  

    ![HDInsight hive-weergave](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)

3. Klik op **Query** in het menu bovenaan.
4. Voer een Hive-query in **Query-Editor**, en klik vervolgens op **Execute**.

## <a name="monitor-jobs"></a>Taken controleren
Zie [beheren HDInsight-clusters met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Door bestanden bladeren
Met behulp van de Azure-portal, kunt u de inhoud van de standaardcontainer bladeren.

1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com).
2. Klik op **HDInsight-Clusters** in het menu links om de bestaande clusters weer te geven.
3. Klik op de naam van het cluster. Als de clusterlijst te lang is, kunt u filteren boven aan de pagina.
4. Klik op **Opslagaccounts** in het linkermenu van het cluster.
5. Klik op een Storage-account.
7. Klik op de **Blobs** tegel.
8. Klik op de standaardnaam van de container.

## <a name="monitor-cluster-usage"></a>Cluster-gebruik bewaken
De **gebruik** sectie van de HDInsight-cluster-blade vindt u informatie over het aantal kernen beschikbaar voor uw abonnement voor gebruik met HDInsight, evenals het aantal kernen dat is toegewezen aan dit cluster en hoe ze worden toegewezen voor de knooppunten in dit cluster. Zie [clusters tonen en vermelden](#list-and-show-clusters).

> [!IMPORTANT]
> Voor het controleren van de services die worden geleverd door het HDInsight-cluster, moet u de Ambari Web- of de Ambari REST-API gebruiken. Zie voor meer informatie over het gebruik van Ambari [HDInsight beheren met Ambari-clusters](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Verbinding maken met een cluster

* [Hive gebruiken met HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u enkele eenvoudige beheerfuncties geleerd. Zie de volgende artikelen voor meer informatie:

* [HDInsight met behulp van Azure PowerShell beheren](hdinsight-administer-use-powershell.md)
* [HDInsight met behulp van Azure CLI beheren](hdinsight-administer-use-command-line.md)
* [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md)
* [Meer informatie over het gebruik van de Ambari-Webinterface](hdinsight-hadoop-manage-ambari.md)
* [Informatie over het gebruik van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Hive in HDInsight gebruiken](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken in HDInsight](hadoop/hdinsight-use-pig.md)
* [Sqoop gebruiken in HDInsight](hadoop/hdinsight-use-sqoop.md)
* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Welke versie van Hadoop is in Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop-opdrachtregel"
