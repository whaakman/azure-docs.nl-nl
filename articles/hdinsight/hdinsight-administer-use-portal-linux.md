---
title: Hadoop-clusters in HDInsight met behulp van Azure-portal beheren | Microsoft Docs
description: Informatie over het maken en beheren van HDInsight-clusters met behulp van de Azure-portal.
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: jgao
ms.openlocfilehash: 90261e090f87a5ca0d92b86c33addce2449cfd24
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Met behulp van de [Azure-portal][azure-portal], kunt u in Azure HDInsight Hadoop-clusters beheren. Gebruik de tabselector boven voor informatie over het beheren van Hadoop-clusters in HDInsight met andere hulpprogramma's.

**Vereiste**

Als u wilt de stappen in dit artikel, moet u een **Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-azure-portal"></a>De Azure portal openen
1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com).
2. Nadat u de portal geopend, kunt u:

   * Klik op **maken van een resource** in het menu links om een nieuw cluster te maken:

       ![knop Nieuw HDInsight-cluster](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)

       Voer **HDInsight** in **zoeken van de Marketplace**, klikt u op **HDInsight**, en klik vervolgens op **maken**.

   * Klik op **HDInsight-Clusters** in het menu links voor een lijst met bestaande clusters:

       ![Knop Azure portal HDInsight-cluster](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Als er geen de **HDInsight-clusters** knop en klik vervolgens op **HDInsight-clusters** onder de **Intelligence en analyse** sectie.


## <a name="create-clusters"></a>Clusters maken
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight werkt met een breed bereik van Hadoop-onderdelen. Zie voor een overzicht van de onderdelen die zijn geverifieerd en ondersteund [welke versie van Hadoop is in Azure HDInsight?](hdinsight-component-versioning.md) Zie voor informatie voor het maken van algemene cluster [maken Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Vereisten voor toegangsbeheer

Wanneer u een HDInsight-cluster maakt, moet u een Azure-abonnement opgeven. Het cluster kan worden gemaakt in een nieuwe Azure-resourcegroep of in een bestaande resourcegroep. U kunt de volgende stappen uit om te controleren of uw machtigingen voor het maken van HDInsight-clusters:

- Een nieuwe resourcegroep maken:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Klik op **abonnement** in het menu links. Het heeft pictogram van een gele sleutel. U ziet een lijst met abonnementen.
    3. Klik op het abonnement dat u gebruiken om clusters te maken. 
    4. Klik op **mijn machtigingen**.  Het bevat uw [rol](../role-based-access-control/overview.md#built-in-roles) voor het abonnement. U moet ten minste Inzender toegang tot het HDInsight-cluster maken.

- Een bestaande resourcegroep gebruiken:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Klik op **resourcegroepen** in het menu links voor een lijst met resourcegroepen.
    3. Klik op de resourcegroep die u gebruiken wilt voor het maken van uw HDInsight-cluster.
    4. Klik op **toegangsbeheer (IAM)**, en controleer of u (of een groep die u deel uitmaakt) ten minste de Inzender toegang hebben tot de resourcegroep.

Als u de fout NoRegisteredProviderFound of de fout MissingSubscriptionRegistration ontvangt, raadpleegt u [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Lijst en clusters weer te geven
1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com).
2. Klik op **HDInsight-Clusters** in het menu links voor een lijst met bestaande clusters. Als er geen **HDInsight-Clusters**, klikt u op **alle services** eerste.
3. Klik op de naam van het cluster. Als de clusterlijst te lang is, kunt u het filter boven aan de pagina.
4. Klik op een cluster in de lijst om te zien van de overzichtspagina:

    ![Azure portal HDInsight-cluster essentials](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png) **overzicht menu:**
    * **Dashboard**: Hiermee opent u de Ambari-webgebruikersinterface voor het cluster.
    * **Secure Shell**: ziet u de instructies voor het verbinden met het cluster via Secure Shell (SSH) verbinding.
    * **Cluster schalen**: Hiermee kunt u het aantal worker-knooppunten voor dit cluster wijzigen.
    * **Verplaats**: het cluster wordt verplaatst naar een andere resourcegroep of een ander abonnement.
    * **Verwijderen**: Hiermee verwijdert u het cluster.

    **Menu aan de linkerkant:**
    * **Activiteitenlogboeken**: weergeven en query activiteitenlogboeken.
    * **Toegangsbeheer (IAM)**: roltoewijzingen gebruiken.  Zie [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md).
    * **Labels**: Hiermee kunt u instellen van sleutel-waardeparen voor het definiëren van een aangepaste taxonomie van uw cloud-services. Bijvoorbeeld, u kunt maken met een sleutel met de naam **project**, en gebruik vervolgens een overeenkomende waarde voor alle services die zijn gekoppeld aan een bepaald project.
    * **Diagnosticeren en oplossen van problemen met**: informatie over probleemoplossing weergeven.
    * **Hiermee vergrendelt u**: toevoegen van een vergrendeling om te voorkomen dat het cluster dat wordt gewijzigd of verwijderd.
    * **Automatiseringsscript**: weergeven en exporteren van de Azure Resource Manager-sjabloon voor het cluster. Op dit moment kunt u alleen het afhankelijke Azure storage-account exporteren. Zie [maken Linux gebaseerde Hadoop-clusters in HDInsight met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Snel starten**: geeft informatie weer die u helpt aan de slag met HDInsight.
    * **Hulpprogramma's voor HDInsight**: Help-informatie voor HDInsight gerelateerde hulpprogramma's.
    * **Abonnement Core gebruik**: de gebruikte en beschikbare kernen voor uw abonnement worden weergegeven.
    * **Cluster schalen**: verhogen en verklein het aantal worker-clusterknooppunten. Zie[schalen clusters](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **SSH + Cluster aanmelding**: ziet u de instructies voor het verbinden met het cluster via Secure Shell (SSH) verbinding. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
    * **HDInsight-Partner**: de huidige HDInsight-Partner toevoegen of verwijderen.
    * **Met externe Metastores**: de metastores Hive en Oozie weergeven. De metastores kan alleen worden geconfigureerd tijdens het maken van het cluster. Zie [gebruik Hive/Oozie-metastore](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Acties script**: Voer Bash-scripts op het cluster. Zie [aanpassen Linux gebaseerde HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md).
    * **Toepassingen**: toevoegen/verwijderen HDInsight-toepassingen.  Zie [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).
    * **Bewaking**: het cluster in Azure-logboekanalyse bewaken.
    * **Eigenschappen**: Bekijk de clustereigenschappen.
    * **Storage-accounts**: de storage-accounts en de sleutels weergeven. De storage-accounts zijn geconfigureerd tijdens het maken van het cluster.
    * **Data Lake Store toegang**: Data Lake slaat toegang configureren.  Zie [HDInsight-clusters maken met Data Lake Store met behulp van de Azure-portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
    * **Resourcestatus**: Zie [overzicht van Azure-resource health](../service-health/resource-health-overview.md).
    * **Nieuw ondersteuningsverzoek**: Hiermee kunt u een ondersteuningsticket maken met Microsoft ondersteuning.
    
6. Klik op **eigenschappen**:

    De eigenschappen zijn:

   * **Hostname**: de naam van het Cluster.
   * **Cluster-URL**: de URL voor de Ambari webinterface.
   * **Secure shell (SSH)**: de gebruikersnaam en het host-naam moet worden gebruikt bij het openen van het cluster via SSH.
   * **Status**: een van: afgebroken, geaccepteerd, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, operationeel is, wordt uitgevoerd, fout, te verwijderen, verwijderd, time-out, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued of ClusterCustomization.
   * **Regio**: Azure-locatie. Zie voor een lijst met ondersteunde Azure locaties, de **regio** vervolgkeuzelijst op [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Aanmaakdatum**: de datum die het cluster is geïmplementeerd.
   * **Besturingssysteem**: beide **Windows** of **Linux**.
   * **Type**: Hadoop, HBase, Storm, Spark.
   * **Versie**. Zie [versies van HDInsight](hdinsight-component-versioning.md).
   * **Abonnement**: de naam van het abonnement.
   * **Standaardgegevensbron**: het standaardbestandssysteem voor het cluster.
   * **De grootte van de worker-knooppunten**: het geselecteerde VM-grootte van de worker-knooppunten.
   * **De grootte van knooppunt HEAD**: het geselecteerde VM-grootte van de hoofdknooppunten.
   * **Virtueel netwerk**: de naam van het virtuele netwerk dat het cluster wordt geïmplementeerd als een is geselecteerd tijdens de implementatie.

## <a name="delete-clusters"></a>Clusters verwijderen
Verwijderen van een cluster wordt niet verwijderd en het standaardopslagaccount ook alle gekoppelde opslagaccounts weergegeven. U kunt het cluster opnieuw maken met behulp van de dezelfde storage-accounts en de dezelfde metastores. U kunt het beste een standaard Blob-container gebruikt wanneer u het cluster opnieuw maakt.

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **HDInsight-Clusters** in het menu links. Als er geen **HDInsight-Clusters**, klikt u op **alle services** eerste.
3. Klik op het cluster dat u wilt verwijderen.
4. Klik op **verwijderen** in het menu bovenaan en volg de instructies.

Zie ook [onderbreken/afsluiten clusters](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Extra opslagaccounts toevoegen

U kunt extra Azure Storage-accounts en Azure Data Lake Store-accounts toevoegen nadat een cluster wordt gemaakt. Zie [Extra opslagaccounts toevoegen aan HDInsight](./hdinsight-hadoop-add-storage.md) voor meer informatie.

## <a name="scale-clusters"></a>Clusters schalen
Het schalen van de functie cluster kunt u het aantal worker-knooppunten gebruikt door een Azure HDInsight-cluster, zonder dat er opnieuw maken van het cluster te wijzigen.

> [!NOTE]
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de pagina eigenschappen controleren.  Zie [lijst en geeft weer clusters](#list-and-show-clusters).
>
>

De gevolgen van het wijzigen van het aantal gegevensknooppunten varieert voor elk type van ondersteund door de HDInsight-cluster:

* Hadoop

    U kunt naadloos Verhoog het aantal worker-knooppunten van een Hadoop-cluster dat wordt uitgevoerd zonder enige impact op alle taken die in behandeling of wordt uitgevoerd. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor schaal worden probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster wordt verkleind door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw gestart. Dit gedrag zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking uit te schalen. U kunt echter de taken verzenden zodra de bewerking voltooid is.
* HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de bewerking uit te schalen. U kunt echter ook handmatig de regionale servers verdelen door aanmelden bij de headnode van cluster en het gebruik van de volgende opdrachten vanuit een opdrachtpromptvenster:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

    Zie voor meer informatie over het gebruik van de HBase-shell [aan de slag met een voorbeeld van Apache HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md).

* Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Echter na een geslaagde voltooiing van de bewerking uit te schalen moet u de topologie opnieuw verdelen.

    Herverdeling kan worden uitgevoerd op twee manieren:

  * Storm-webgebruikersinterface
  * Hulpprogramma voor opdrachtregelinterface (CLI)

    Raadpleeg de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight Storm scale deel opnieuw](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Hier volgt een voorbeeld CLI-opdracht opnieuw verdelen van de Storm-topologie:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

**Schalen van clusters**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **HDInsight-Clusters** in het menu links.
3. Klik op het cluster dat u wilt schalen.
3. Klik op **Cluster schalen**.
4. Voer **nummer van Worker-knooppunten**. De limiet voor het aantal clusterknooppunten verschilt per Azure-abonnementen. Neem contact op met ondersteuning voor facturering om de maximumlimiet te verhogen.  De kostengegevens staan de wijzigingen die u hebt aangebracht in het aantal knooppunten.

    ![HDInsight hadoop hbase, storm spark schaal](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Onderbreken/clusters afsluiten

De meeste Hadoop-taken zijn batchtaken die alleen van tijd tot tijd worden uitgevoerd. Er zijn grote perioden die het cluster niet wordt gebruikt voor de verwerking voor de meeste Hadoop-clusters. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt.
Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Er zijn veel manieren kunt u het proces programma:

* Gebruiker Azure Data Factory. Zie [maken via bellen op Linux gebaseerde Hadoop-clusters in HDInsight met behulp van Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) gekoppelde services voor het maken van HDInsight op aanvraag.
* Azure PowerShell gebruiken.  Zie [analyseren vertraging vluchtgegevens](hdinsight-analyze-flight-delay-data.md).
* Azure CLI gebruiken. Zie [HDInsight-clusters beheren met Azure CLI](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK gebruiken. Zie [indienen Hadoop-taken](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Zie voor de prijsinformatie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/). Zie voor informatie over het verwijderen van een cluster vanuit de Portal [clusters verwijderen](#delete-clusters)

## <a name="move-cluster"></a>Cluster verplaatsen

U kunt een HDInsight-cluster verplaatsen naar een andere Azure-resourcegroep of een ander abonnement.  Zie [lijst en geeft weer clusters](#list-and-show-clusters).

## <a name="upgrade-clusters"></a>Bijwerken van clusters

Zie [Upgrade HDInsight-cluster naar een nieuwere versie](./hdinsight-upgrade-cluster.md).

## <a name="open-the-ambari-web-ui"></a>Open de Ambari-webgebruikersinterface

Ambari bevat een intuïtieve, eenvoudig te gebruiken Hadoop management webgebruikersinterface ondersteund door de RESTful-API's. Ambari kan systeembeheerders in staat te beheren en controleren van Hadoop-clusters.

1. Open een HDInsight-cluster van de Azure-portal.  Zie [lijst en geeft weer clusters](#list-and-show-clusters).
2. Klik op **Dashboard Cluster**.

    ![Menu van HDInsight Hadoop-cluster](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu.png)

1. De cluster-gebruikersnaam en wachtwoord invoeren.  De standaardgebruikersnaam cluster _admin_. Het Ambari web UI ziet eruit als:

    ![Ambari-Webgebruikersinterface HDInsight Hadoop](./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-ambari-web-ui.png)

Zie voor meer informatie [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md).

## <a name="change-passwords"></a>Wachtwoorden wijzigen
Een HDInsight-cluster kan twee gebruikersaccounts hebben. De HDInsight-cluster gebruikersaccount (ook Gebruikersaccount HTTP) en de SSH-gebruikersaccount worden gemaakt tijdens het maken. U kunt de Ambari-webgebruikersinterface cluster user account gebruikersnaam en wachtwoord en scriptacties wijzigen van de SSH-gebruikersaccount wijzigen

### <a name="change-the-cluster-user-password"></a>Het gebruikerswachtwoord cluster wijzigen
De Ambari-Webgebruikersinterface kunt u het gebruikerswachtwoord Cluster wijzigen. Voor het aanmelden bij de Ambari, moet u het bestaande cluster gebruikersnaam en wachtwoord gebruiken.

> [!NOTE]
> Het wachtwoord van de cluster-gebruiker (admin) wijzigt, kan scriptacties uitvoeren op dit cluster uitvalt. Als u een persistente scriptacties die doel worker-knooppunten hebt, mislukken deze scripts wanneer u knooppunten aan het cluster via vergroten of verkleinen operations toevoegt. Zie voor meer informatie over scriptacties [aanpassen HDInsight-clusters met behulp van scriptacties](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Aanmelden bij de Ambari-Webgebruikersinterface met behulp van de gebruikersreferenties voor HDInsight-cluster. De standaardgebruikersnaam **admin**. De URL is **https://&lt;de naam van de HDInsight-Cluster > azurehdinsight.net**.
2. Klik op **Admin** in het bovenste menu en klik vervolgens op 'Ambari beheren'.
3. Klik in het menu links op **gebruikers**.
4. Klik op **Admin**.
5. Klik op **wachtwoord wijzigen**.

Ambari wordt vervolgens het wachtwoord op alle knooppunten in het cluster.

### <a name="change-the-ssh-user-password"></a>Het SSH-gebruikerswachtwoord wijzigen
1. De volgende tekst met een teksteditor opslaan als een bestand met de naam **changepassword.sh**.

    > [!IMPORTANT]
    > U moet een editor die gebruikmaakt van LF als het einde van regel gebruiken. Als de editor CRLF gebruikt, klikt u vervolgens werkt het script niet.

    ```bash
    #! /bin/bash
    USER=$1
    PASS=$2
    usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
    ```

2. Upload het bestand naar een locatie voor de opslag die toegankelijk zijn vanuit HDInsight met behulp van een HTTP of HTTPS-adres. Bijvoorbeeld een bestand met een openbare opslaan zoals OneDrive of Azure Blob-opslag. De URI (HTTP of HTTPS-adres) naar het bestand niet opslaan omdat deze URI in de volgende stap is vereist.
3. Klik in de Azure-portal op **HDInsight-Clusters**.
4. Klik op uw HDInsight-cluster.
4. Klik op **acties Script**.
4. Van de **scriptacties** blade Selecteer **nieuwe indienen**. Wanneer de **scriptactie indienen** blade wordt weergegeven, voer de volgende informatie:

   | Veld | Waarde |
   | --- | --- |
   | Naam |Ssh wachtwoord wijzigen |
   | Bash-script-URI |De URI naar het bestand changepassword.sh |
   | Knooppunten (Head, Worker, Nimbus, leidinggevende, Zookeeper, enz.) |✓ voor alle knooppunttypen vermeld |
   | Parameters |Voer de SSH-gebruikersnaam en klik vervolgens op het nieuwe wachtwoord. Er moet één spatie tussen de gebruikersnaam en het wachtwoord. |
   | Deze scriptactie... |Laat dit veld uitgeschakeld. |
5. Selecteer **maken** om toe te passen van het script. Nadat het script is voltooid, bent u geen verbinding maken met het cluster via SSH met het nieuwe wachtwoord.

## <a name="grantrevoke-access"></a>Toegang verlenen of in te trekken
HDInsight-clusters hebben de volgende HTTP-webservices (al deze services hebt RESTful eindpunten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Standaard worden deze services verleend om toegang te krijgen. U kunt in te trekken/grant de toegang met behulp van [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) en [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>De abonnements-ID vinden

**Vinden van uw Azure-abonnement id 's**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **abonnementen**. Elk abonnement heeft een naam en ID.

Elk cluster is gekoppeld aan een Azure-abonnement. Het abonnement ID wordt weergegeven op het cluster **essentiële** tegel. Zie [lijst en geeft weer clusters](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>De resourcegroep niet vinden
In de modus Azure Resource Manager elk HDInsight-cluster gemaakt met een Azure Resource Manager-groep. De Resource Manager-groep die een cluster tot behoort wordt weergegeven:

* De clusterlijst heeft een **resourcegroep** kolom.
* Cluster **essentiële** tegel.  

Zie [lijst en geeft weer clusters](#list-and-show-clusters).

## <a name="find-the-storage-accounts"></a>De storage-accounts gevonden

HDInsight-clusters maken gebruik van een Azure Storage-account of een Azure Data Lake Store gegevens wilt opslaan. Elke HDInsight-cluster kan één standaard-opslagaccount en een aantal gekoppelde storage-accounts hebben. Als u de storage-accounts, u het cluster voor het eerst opent vanuit de portal en klik vervolgens op **opslagaccounts**:

![HDInsight-cluster storage-accounts](./media/hdinsight-administer-use-portal-linux/hdinsight-storage-accounts.png)

In de vorige schermafbeelding er is een __standaard__ kolom die aangeeft of het account het standaardopslagaccount is.

Als de Data Lake Store-accounts wilt weergeven, klikt u op **Data Lake Store toegang** in de vorige schermafbeelding.

## <a name="run-hive-queries"></a>Hive-query's uitvoeren
Hive-taak niet rechtstreeks vanuit de Azure portal kan niet worden uitgevoerd, maar u kunt de weergave Hive op Ambari-Webgebruikersinterface gebruiken.

**Uitvoeren van Hive-query's met Ambari Hive-weergave**

1. Aanmelden bij de Ambari-Webgebruikersinterface met behulp van de gebruikersreferenties voor HDInsight-cluster. De standaardgebruikersnaam **admin**. De URL is **https://&lt;de naam van de HDInsight-Cluster > azurehdinsight.net**.
2. Open de Hive-weergave zoals weergegeven in de volgende schermafbeelding:  

    ![HDInsight hive-weergave](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)

3. Klik op **Query** in het menu bovenaan.
4. Voer een Hive-query in **Query-Editor**, en klik vervolgens op **Execute**.

## <a name="monitor-jobs"></a>Taken controleren
Zie [HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Blader door bestanden
De Azure-portal kunt u de inhoud van de standaardcontainer bladeren.

1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com).
2. Klik op **HDInsight-Clusters** in het menu links voor een lijst met bestaande clusters.
3. Klik op de naam van het cluster. Als de clusterlijst te lang is, kunt u het filter boven aan de pagina.
4. Klik op **Opslagaccounts** in het linkermenu cluster.
5. Klik op een opslagaccount.
7. Klik op de **Blobs** tegel.
8. Klik op de standaardnaam van de container.

## <a name="monitor-cluster-usage"></a>Gebruik van het beeldscherm cluster
De **gebruik** sectie van de HDInsight-cluster-blade geeft informatie weer over het aantal kernen dat beschikbaar is voor uw abonnement voor gebruik met HDInsight, evenals het aantal kernen dat is toegewezen aan dit cluster en hoe ze worden toegewezen voor de knooppunten in dit cluster. Zie [lijst en geeft weer clusters](#list-and-show-clusters).

> [!IMPORTANT]
> Voor het bewaken van de services geleverd door het HDInsight-cluster, moet u de Ambari Web of de Ambari REST-API gebruiken. Zie voor meer informatie over het gebruik van Ambari [HDInsight-clusters beheren met Ambari](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Verbinding maken met een cluster

* [Hive gebruiken met HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd sommige basic beheerfuncties. Zie voor meer informatie de volgende artikelen:

* [HDInsight met behulp van Azure PowerShell beheren](hdinsight-administer-use-powershell.md)
* [Beheer van HDInsight Azure CLI gebruiken](hdinsight-administer-use-command-line.md)
* [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md)
* [Lees meer over het gebruik van de Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)
* [Informatie over het gebruik van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Hive in HDInsight gebruiken](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken in HDInsight](hadoop/hdinsight-use-pig.md)
* [Sqoop gebruiken in HDInsight](hadoop/hdinsight-use-sqoop.md)
* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Welke versie van Hadoop is in Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop-opdrachtregel"
