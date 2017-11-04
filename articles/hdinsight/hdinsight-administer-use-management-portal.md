---
title: Hadoop op basis van Windows-clusters in HDInsight met behulp van de Azure-portal beheren | Microsoft Docs
description: Informatie over het beheren van HDInsight-Service. Maken van een HDInsight-cluster, opent u de interactieve JavaScript-console en open de console van de opdracht Hadoop.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: ecaad702843a63bb82b781339d25fde10df0a0a4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Hadoop op basis van Windows-clusters in HDInsight beheren met behulp van de Azure-portal

Met behulp van de [Azure-portal][azure-portal], Hadoop op basis van Windows-clusters maken in Azure HDInsight, Hadoop-gebruikerswachtwoord wijzigen en schakelt Remote Desktop Protocol (RDP), zodat u toegang hebt tot de opdrachtconsole Hadoop op het cluster.

De informatie in dit artikel is alleen van toepassing op HDInsight clusters op basis van het venster. Zie voor meer informatie over het beheren van clusters op basis van Linux [beheren Hadoop-clusters in HDInsight met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md).

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.


## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure Storage-account** -An HDInsight cluster gebruikmaakt van een Azure Blob storage-container als het standaardbestandssysteem. Zie voor meer informatie over hoe Azure Blob-opslag een naadloze ervaring met HDInsight-clusters biedt [Azure Blob Storage gebruiken met HDInsight](hdinsight-hadoop-use-blob-storage.md). Zie voor meer informatie over het maken van een Azure Storage-account [het maken van een Opslagaccount](../storage/common/storage-create-storage-account.md).

## <a name="open-the-portal"></a>De Portal openen
1. Aanmelden bij [https://portal.azure.com](https://portal.azure.com).
2. Nadat u de portal geopend, kunt u:

   * Klik op **nieuw** in het menu links om een nieuw cluster te maken:

       ![knop Nieuw HDInsight-cluster](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Klik op **HDInsight-Clusters** in het menu links.

       ![Knop Azure portal HDInsight-cluster](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Als **HDInsight** niet weergegeven in het menu links, klikt u op **Bladeren**.

     ![Azure portal cluster bladerknop](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Clusters maken
Zie voor instructies voor het maken van via de Portal, [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md).

HDInsight werkt met een breed bereik van Hadoop-onderdelen. Zie voor een overzicht van de onderdelen die zijn geverifieerd en ondersteund [welke versie van Hadoop is in Azure HDInsight](hdinsight-component-versioning.md). U kunt HDInsight aanpassen met behulp van een van de volgende opties:

* Gebruik scriptactie om uit te voeren van aangepaste scripts die een cluster voor het wijzigen van de configuratie van het cluster of aangepaste onderdelen zoals Giraph of Solr installeren kunnen aanpassen. Zie voor meer informatie [aanpassen HDInsight-cluster via scriptactie](hdinsight-hadoop-customize-cluster.md).
* De parameters van de aanpassing cluster in HDInsight .NET SDK of Azure PowerShell gebruiken tijdens het maken van het cluster. Deze wijzigingen in de configuratie vervolgens via de levensduur van het cluster worden bewaard en worden niet beïnvloed door cluster knooppunt reimages die Azure-platform regelmatig voor onderhoud uitvoert. Zie voor meer informatie over het gebruik van de parameters van de aanpassing cluster [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md).
* Sommige systeemeigen Java-onderdelen, zoals Mahout en trapsgewijze, kunnen worden uitgevoerd op het cluster als JAR-bestanden. Deze JAR-bestanden worden gedistribueerd naar Azure Blob storage en verzonden naar HDInsight-clusters via Hadoop taak verzending mechanismen. Zie voor meer informatie [Hadoop verzenden via een programma taken](hadoop/submit-apache-hadoop-jobs-programmatically.md).

  > [!NOTE]
  > Als u problemen JAR-bestanden met HDInsight-clusters implementeren of als het aanroepen van de JAR-bestanden op HDInsight-clusters, neem contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).
  >
  > Trapsgewijze bewerking wordt niet ondersteund door HDInsight en is niet in aanmerking komen voor Microsoft Support. Zie voor een lijst met ondersteunde onderdelen, [wat is er nieuw in de clusterversies geleverd door HDInsight](hdinsight-component-versioning.md).
  >
  >

Installatie van de aangepaste software op het cluster met behulp van de verbinding met extern bureaublad wordt niet ondersteund. Vermijd het opslaan van de bestanden op de schijven van het hoofdknooppunt als ze niet verloren als u wilt opnieuw clusters te maken. Het is raadzaam bestanden opslaan op Azure Blob-opslag. BLOB-opslag is permanent.

## <a name="list-and-show-clusters"></a>Lijst en clusters weer te geven
1. Aanmelden bij [https://portal.azure.com](https://portal.azure.com).
2. Klik op **HDInsight-Clusters** in het menu links.
3. Klik op de naam van het cluster. Als de clusterlijst te lang is, kunt u het filter boven aan de pagina.
4. Dubbelklik op een cluster in de lijst om de details weer te geven.

    **Menu en essentials**:

    ![Azure portal HDInsight-cluster essentials](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * Voor het aanpassen van het menu met de rechtermuisknop op het menu en klik vervolgens op **aanpassen**.
   * **Instellingen** en **alle instellingen**: geeft de **instellingen** blade voor het cluster, zodat u kunt de gedetailleerde configuratie-informatie voor het cluster.
   * **Dashboard**, **Cluster-Dashboard** en **URL: dit zijn alle manieren toegang krijgen tot het cluster-dashboard Ambari Web voor clusters op basis van Linux is. -**Secure Shell **: Bevat de instructies voor het verbinden met het cluster via Secure Shell (SSH) verbinding.
   * **Cluster schalen**: Hiermee kunt u het aantal worker-knooppunten voor dit cluster wijzigen.
   * **Verwijderen**: Hiermee verwijdert u het cluster.
   * **Quick Start**: geeft informatie weer waarmee u kunt aan de slag met HDInsight.
   * ** Gebruikers: Hiermee kunt u machtigingen instellen voor *portal management* van dit cluster voor andere gebruikers op uw Azure-abonnement.

     > [!IMPORTANT]
     > Dit *alleen* is van invloed op toegang en machtigingen voor dit cluster in de Azure portal en heeft geen effect op wie kan verbinding maken met of verzenden van taken naar het HDInsight-cluster.
     >
     >
   * **Labels**: labels kunnen u instellen van sleutel-waardeparen voor het definiëren van een aangepaste taxonomie van uw cloud-services. Bijvoorbeeld, u kunt maken met een sleutel met de naam **project**, en gebruik vervolgens een overeenkomende waarde voor alle services die zijn gekoppeld aan een bepaald project.
   * **Ambari-weergaven**: koppelingen naar Ambari Web.

     > [!IMPORTANT]
     > Voor het beheren van de services geleverd door het HDInsight-cluster, moet u de Ambari Web of de Ambari REST-API gebruiken. Zie voor meer informatie over het gebruik van Ambari [HDInsight-clusters beheren met Ambari](hdinsight-hadoop-manage-ambari.md).
     >
     >

     **Gebruik**:

     ![Gebruik Azure portal HDInsight-cluster](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Klik op **instellingen**.

    ![Gebruik Azure portal HDInsight-cluster](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Eigenschappen**: Bekijk de clustereigenschappen.
   * **AAD-identiteit cluster**:
   * **Azure Storage-sleutels**: Bekijk het standaardopslagaccount en de bijbehorende sleutel. Het opslagaccount is de configuratie tijdens het maken van het cluster.
   * **Aanmelding cluster**: het cluster HTTP-gebruikersnaam en wachtwoord wijzigen.
   * **Met externe Metastores**: de metastores Hive en Oozie weergeven. De metastores kan alleen worden geconfigureerd tijdens het maken van het cluster.
   * **Cluster schalen**: verhogen en verklein het aantal worker-clusterknooppunten.
   * **Extern bureaublad**: inschakelen en uitschakelen van toegang tot extern bureaublad (RDP) en configureren van de RDP-gebruikersnaam.  De RDP-gebruikersnaam moet verschillen van de HTTP-gebruikersnaam.
   * **Partner of Record**:

     > [!NOTE]
     > Dit is een algemene lijst met beschikbare instellingen; niet alle niet aanwezig is voor alle clustertypen.
     >
     >
6. Klik op **eigenschappen**:

    De sectie met eigenschappen bevat de volgende informatie:

   * **Hostname**: de naam van het Cluster.
   * **Cluster-URL**.
   * **Status**: omvatten afgebroken, geaccepteerd, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, operationeel is, wordt uitgevoerd, fout, te verwijderen, verwijderd, time-out, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **Regio**: Azure-locatie. Zie voor een lijst met ondersteunde Azure locaties, de **regio** vervolgkeuzelijst op [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Gegevens die zijn gemaakt**.
   * **Besturingssysteem**: beide **Windows** of **Linux**.
   * **Type**: Hadoop, HBase, Storm, Spark.
   * **Versie**. Zie [HDInsight-versies](hdinsight-component-versioning.md)
   * **Abonnement**: de naam van het abonnement.
   * **Abonnements-ID**.
   * **Primaire gegevensbron**. De Azure Blob storage-account gebruikt als het standaardbestandssysteem voor Hadoop.
   * **Worker-knooppunten prijscategorie**.
   * **Hoofdknooppunt prijscategorie**.

## <a name="delete-clusters"></a>Clusters verwijderen
Verwijderen van een cluster wordt niet verwijderd voor het standaardopslagaccount of alle gekoppelde opslagaccounts weergegeven. U kunt het cluster opnieuw maken met behulp van de dezelfde storage-accounts en de dezelfde metastores.

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** vanuit het menu links op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **verwijderen** in het menu bovenaan en volg de instructies.

Zie ook [onderbreken/afsluiten clusters](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Clusters schalen
Het schalen van de functie cluster kunt u het aantal worker-knooppunten gebruikt door een cluster dat wordt uitgevoerd in Azure HDInsight zonder te hoeven maken van het cluster opnieuw wijzigen.

> [!NOTE]
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de pagina eigenschappen controleren.  Zie [lijst en geeft weer clusters](#list-and-show-clusters).
>
>

De gevolgen van het wijzigen van het aantal gegevensknooppunten voor elk type van ondersteund door de HDInsight-cluster:

* Hadoop

    U kunt naadloos Verhoog het aantal worker-knooppunten van een Hadoop-cluster dat wordt uitgevoerd zonder enige impact op alle taken die in behandeling of wordt uitgevoerd. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor schaal worden probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster wordt verkleind door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw gestart. Dit zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking uit te schalen. U kunt echter de taken verzenden zodra de bewerking voltooid is.
* HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de bewerking uit te schalen. U kunt echter ook handmatig de regionale servers verdelen door aan te melden bij de headnode van cluster en het gebruik van de volgende opdrachten vanuit een opdrachtpromptvenster:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Zie voor meer informatie over het gebruik van de HBase-shell]
* Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Maar na een geslaagde voltooiing van de bewerking uit te schalen, moet u de topologie opnieuw verdelen.

    Herverdeling kan worden uitgevoerd op twee manieren:

  * Storm-webgebruikersinterface
  * Hulpprogramma voor opdrachtregelinterface (CLI)

    Raadpleeg de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight storm scale deel opnieuw](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Hier volgt een voorbeeld van hoe u met de opdracht CLI opnieuw verdelen van de Storm-topologie:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**Schalen van clusters**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** vanuit het menu links op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik op **Scale Cluster**.
4. Voer **nummer van Worker-knooppunten**. De limiet van het aantal clusterknooppunt verschilt per Azure-abonnementen. Neem contact op met ondersteuning voor facturering om de maximumlimiet te verhogen.  De kostengegevens bevatten de wijzigingen die u hebt aangebracht in het aantal knooppunten.

    ![HDInsight Hadoop HBase, Storm Spark schaal](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Onderbreken/clusters afsluiten
De meeste Hadoop-taken zijn batchtaken die alleen van tijd tot tijd worden uitgevoerd. Er zijn grote perioden die het cluster niet wordt gebruikt voor de verwerking voor de meeste Hadoop-clusters. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt.
Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Er zijn veel manieren kunt u het proces programma:

* Gebruiker Azure Data Factory. Zie [gekoppelde Service van Azure HDInsight](../data-factory/compute-linked-services.md) en [transformeren en analyseren met Azure Data Factory](../data-factory/transform-data.md) voor op aanvraag en zelf gedefinieerde HDInsight gekoppelde services.
* Azure PowerShell gebruiken.  Zie [analyseren vertraging vluchtgegevens](hdinsight-analyze-flight-delay-data.md).
* Azure CLI gebruiken. Zie [HDInsight-clusters beheren met Azure CLI](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK gebruiken. Zie [indienen Hadoop-taken](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Zie voor de prijsinformatie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/). Zie voor informatie over het verwijderen van een cluster vanuit de Portal [clusters verwijderen](#delete-clusters)

## <a name="change-cluster-username"></a>Wijziging cluster gebruikersnaam
Een HDInsight-cluster kan twee gebruikersaccounts hebben. De gebruikersaccount van de HDInsight-cluster wordt gemaakt tijdens het maken. U kunt ook een RDP-gebruikersaccount voor toegang tot het cluster via RDP maken. Zie [extern bureaublad inschakelen](#connect-to-hdinsight-clusters-by-using-rdp).

**De HDInsight-cluster-gebruikersnaam en wachtwoord wijzigen**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** vanuit het menu links op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik op **Cluster aanmelding**.
4. Als **Cluster aanmelding** is ingeschakeld, klikt u op **uitschakelen**, en klik vervolgens op **inschakelen** voordat u de gebruikersnaam en wachtwoord kunt wijzigen...
5. Wijzig de **Cluster aanmeldingsnaam** en/of de **Cluster aanmeldingswachtwoord**, en klik vervolgens op **opslaan**.

    ![HDInsight cluster gebruiker gebruikersnaam wachtwoord http gebruiker wijzigen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>Toegang verlenen of in te trekken
HDInsight-clusters hebben de volgende HTTP-webservices (al deze services hebt RESTful eindpunten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Standaard worden deze services verleend om toegang te krijgen. U kunt in te trekken/grant de toegang van de Azure-portal.

> [!NOTE]
> Door de toegang verlenen/intrekken, stelt u de cluster-gebruikersnaam en wachtwoord.
>
>

**Web-services toegang te verlenen of in te trekken HTTP**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** vanuit het menu links op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik op **Cluster aanmelding**.
4. Als **Cluster aanmelding** is ingeschakeld, klikt u op **uitschakelen**, en klik vervolgens op **inschakelen** voordat u de gebruikersnaam en wachtwoord kunt wijzigen...
5. Voor **Clusteraanmeldgegevens** en **Cluster aanmeldingswachtwoord**, geeft u de nieuwe gebruikersnaam en het wachtwoord (respectievelijk) voor het cluster.
6. Klik op **OPSLAAN**.

    ![HDInsight eindtotaal HTTP-web-Servicetoegang verwijderen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a>Het standaardopslagaccount vinden
Elke HDInsight-cluster heeft storage-standaardaccount. Het standaardopslagaccount en de sleutels voor een cluster wordt weergegeven onder **instellingen**/**eigenschappen**/**Azure Opslagsleutels**. Zie [lijst en geeft weer clusters](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>De resourcegroep niet vinden
In de modus Azure Resource Manager wordt elk HDInsight-cluster gemaakt met een Azure-resourcegroep. De Azure-resourcegroep waartoe een cluster wordt weergegeven:

* De clusterlijst heeft een **resourcegroep** kolom.
* Cluster **essentiële** tegel.  

Zie [lijst en geeft weer clusters](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>HDInsight-Query-console openen
De console HDInsight Query bevat de volgende functies:

* **Hive-Editor**: een GUI-webinterface voor het indienen van Hive-taken.  Zie [uitvoeren Hive-query's via de Console Query](hadoop/apache-hadoop-use-hive-query-console.md).

    ![HDInsight portal hive-editor](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **Taakgeschiedenis**: Monitor Hadoop-taken.  

    ![De portal Taakgeschiedenis HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Klik op **querynaam** om weer te geven van de details taakeigenschappen, inclusief **Job Query**, en ** Taakuitvoer. U kunt zowel de query en de uitvoer ook downloaden naar uw werkstation.
* **Bestand Browser**: Blader door het standaardopslagaccount en de gekoppelde opslagaccounts.

    ![Portal bladeren browser HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Op de schermafbeelding de  **<Account>**  type geeft aan het item is een Azure storage-account.  Klik op de accountnaam te bladeren in de bestanden.
* **Hadoop-gebruikersinterface**.

    ![Portal HDInsight Hadoop UI](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    Van **Hadoop UI*, kunt u bladeren in bestanden en controleert u Logboeken.
* **Yarn UI**.

    ![YARN gebruikersinterface van de HDInsight-portal](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Hive-query's uitvoeren
Aan het Hive-taken via de Portal hebt uitgevoerd, klikt u op **Hive-Editor** in de Query voor HDInsight-console. Zie [openen HDInsight Query console](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>Taken controleren
Als u wilt bewaken taken vanuit de Portal, klikt u op **Taakgeschiedenis** in de Query voor HDInsight-console. Zie [openen HDInsight Query console](#open-hdinsight-query-console).

## <a name="browse-files"></a>Blader door bestanden
Om te bladeren naar bestanden opgeslagen in het standaardopslagaccount en de gekoppelde storage-accounts, klikt u op **bestandsbrowser** in de Query voor HDInsight-console. Zie [openen HDInsight Query console](#open-hdinsight-query-console).

U kunt ook de **het bestandssysteem** hulpprogramma van de **Hadoop UI** in de HDInsight-console.  Zie [openen HDInsight Query console](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Gebruik van het beeldscherm cluster
De **gebruik** sectie van de HDInsight-cluster-blade geeft informatie weer over het aantal kernen dat beschikbaar is voor uw abonnement voor gebruik met HDInsight, evenals het aantal kernen dat is toegewezen aan dit cluster en hoe ze worden toegewezen voor de knooppunten in dit cluster. Zie [lijst en geeft weer clusters](#list-and-show-clusters).

> [!IMPORTANT]
> Voor het bewaken van de services geleverd door het HDInsight-cluster, moet u de Ambari Web of de Ambari REST-API gebruiken. Zie voor meer informatie over het gebruik van Ambari [HDInsight-clusters beheren met Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="open-hadoop-ui"></a>Open Hadoop-gebruikersinterface
Voor het bewaken van het cluster, het bestandssysteem te bladeren en controleert u Logboeken, klikt u op **Hadoop UI** in de Query voor HDInsight-console. Zie [openen HDInsight Query console](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Yarn UI openen
Klik voor het gebruik van de gebruikersinterface van Yarn **gebruikersinterface van Yarn** in de Query voor HDInsight-console. Zie [openen HDInsight Query console](#open-hdinsight-query-console).

## <a name="connect-to-clusters-using-rdp"></a>Verbinding maken met clusters met RDP
De referenties voor het cluster die u hebt opgegeven bij het maken van de geven toegang tot de services op het cluster, maar niet aan het cluster zelf via Extern bureaublad. U kunt toegang tot extern bureaublad inschakelen wanneer u een cluster inrichten of nadat een cluster is ingericht. Zie voor instructies over het inschakelen van extern bureaublad bij het maken van [maken HDInsight-cluster](hdinsight-hadoop-provision-linux-clusters.md).

**Extern bureaublad inschakelen**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** vanuit het menu links op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik op **extern bureaublad**.
4. Voer **verloopt op**, **gebruikersnaam voor extern bureaublad** en **extern bureaublad-wachtwoord**, en klik vervolgens op **inschakelen**.

    ![HDInsight uitschakelen extern bureaublad configureren](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    De standaardwaarden voor verloopt op is een week.

   > [!NOTE]
   > U kunt ook de HDInsight .NET SDK gebruiken voor het inschakelen van extern bureaublad op een cluster. Gebruik de **EnableRdp** methode voor het HDInsight-client-object in de volgende manier: **client. EnableRdp (clusternaam, locatie, 'rdpuser', 'rdppassword', DateTime.Now.AddDays(6))**. Op deze manier voor het uitschakelen van extern bureaublad op het cluster, kunt u **client. DisableRdp (clustername, locatie)**. Zie voor meer informatie over deze methoden [HDInsight .NET SDK-naslaginformatie](http://go.microsoft.com/fwlink/?LinkId=529017). Dit is alleen van toepassing op HDInsight-clusters met Windows.
   >
   >

**Verbinding maken met een cluster met behulp van RDP**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** vanuit het menu links op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik op **extern bureaublad**.
4. Klik op **Connect** en volg de instructies. Als Connect uitschakelen, moet u het eerst inschakelen. Zorg ervoor dat de extern bureaublad-gebruikersnaam en wachtwoord.  U kunt de referenties van de Cluster-gebruiker niet gebruiken.

## <a name="open-hadoop-command-line"></a>Open Hadoop-opdrachtregel
Voor verbinding met het cluster met behulp van extern bureaublad en gebruik de Hadoop-opdrachtregel, moet u eerst hebt ingeschakeld extern bureaublad-toegang tot het cluster zoals beschreven in de vorige sectie.

**Openen van een Hadoop-opdrachtregel**

1. Verbinding maken met het cluster via Extern bureaublad.
2. Dubbelklik op het bureaublad op **Hadoop-opdrachtregel**.

    ![HDI. HadoopCommandLine][image-hadoopcommandline]

    Zie voor meer informatie over Hadoop opdrachten [Hadoop opdrachten verwijzing](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

Naam van de map heeft in de vorige schermafbeelding het Hadoop-versienummer ingesloten. Het versienummer kan gewijzigd op basis van de versie van de Hadoop-onderdelen geïnstalleerd op het cluster. U kunt de omgevingsvariabelen Hadoop gebruiken om te verwijzen naar deze mappen. Bijvoorbeeld:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe een HDInsight-cluster maken met behulp van de Portal en het openen van het Hadoop-opdrachtregelprogramma. Zie voor meer informatie de volgende artikelen:

* [HDInsight met behulp van Azure PowerShell beheren](hdinsight-administer-use-powershell.md)
* [Beheer van HDInsight Azure CLI gebruiken](hdinsight-administer-use-command-line.md)
* [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md)
* [Hadoop-taken programmatisch verzenden](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Welke versie van Hadoop is in Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop-opdrachtregel"
