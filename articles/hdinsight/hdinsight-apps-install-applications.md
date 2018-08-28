---
title: Hadoop-toepassingen van derden op Azure HDInsight installeren
description: Informatie over het installeren van Hadoop-toepassingen van derden op Azure HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: c355d1c4021d8d0877dea216e0c11002b70595e4
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105215"
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Hadoop-toepassingen van derden op Azure HDInsight installeren

Informatie over het installeren van een Hadoop-toepassing van derden op Azure HDInsight. Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

Een HDInsight-toepassing is een toepassing die gebruikers op een HDInsight-cluster installeren kunnen. Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf.  

De volgende lijst bevat de gepubliceerde toepassingen:

* **AtScale Informatieplatform** schakelt het HDInsight-cluster in een scale-out OLAP-server. De toepassing kunt u query miljarden rijen met gegevens die interactief met behulp van de BI-hulpmiddelen van Microsoft Excel, Power BI, Tableau Software voor het QlikView.
* **Cask CDAP voor HDInsight** biedt het eerste uniforme integratieplatform voor big data waarvan de tijd in productie voor gegevenstoepassingen en datalakes met 80%. Deze toepassing biedt alleen ondersteuning voor standaard HBase 3.4-clusters.
* **DATAIKU DDS op HDInsight** kunnen gegevens professionals prototypen maken, bouwen en implementeren van zeer specifieke services die onbewerkte gegevens getransformeerd in bedrijfsvoorspellingen.
* **H2O kunstmatige intelligentie voor HDInsight (bèta)** H2O Sparkling Water ondersteunt de volgende gedistribueerde algoritmen: GLM, Naïve Bayes, gedistribueerde willekeurige Forest, Gradient Boosting Machine, Deep Neural Networks, Deep learning, K-means, PCA, Lage positie modellen, detectie van afwijkingen en Autoencoders gegeneraliseerd.
* **Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) is een datawarehouse in enterprise-ready mogelijk gemaakt door Apache Kylin en Apache Hadoop; het biedt dan een seconde, latentie op grote schaal gegevensset opvragen en gegevensanalyse voor vereenvoudigt zakelijke gebruikers en analisten. 
* **Paxata Self-service gegevensvoorbereiding**
* **Spark-taakserver voor Spark-uitvoerder KNIME** Spark-taakserver voor Spark-uitvoerder KNIME wordt gebruikt voor het Analytics-Platform KNIME verbinding met HDInsight-clusters.
* **StreamSets Data Collector voor HDInsight** biedt een complete geïntegreerde IDE (Integrated Development Environment) waarin u opnamepijplijnen die stream- en batchgegevens filteren en diverse in-stream transformaties bevatten, in alle richtingen kunt ontwerpen, testen, implementeren en beheren, en dat alles zonder aangepaste code te hoeven schrijven. 
* **[Trifacta](http://www.trifacta.com/)**  kunnen data-engineers en bedrijfsanalisten efficiënter verkennen en de verschillende gegevens van vandaag voorbereiden door het gebruik van machine learning om aan te bieden een baanbrekende gebruikerservaring, de werkstroom en de architectuur.
* **WANdisco Fusion HDI App** kunt continue consistente verbinding met de gegevens gewijzigd waar deze zich bevindt. Het biedt u met toegang tot uw gegevens op elk gewenst moment en overal met zonder uitvaltijd en geen onderbreking.

De instructies in dit artikel zijn bestemd voor gebruik met Azure Portal. U kunt ook de Azure Resource Manager-sjabloon exporteren vanuit de portal of een kopie van de Resource Manager-sjabloon van leveranciers verkrijgen, en Azure PowerShell en Azure CLI gebruiken om de sjabloon te implementeren.  Zie [Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Vereisten
Als u HDInsight-toepassingen wilt installeren op een bestaand HDInsight-cluster, hebt u een HDInsight-cluster nodig. Zie [Clusters maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) voor informatie over het maken van een cluster. U kunt ook HDInsight-toepassingen installeren wanneer u een HDInsight-cluster maakt.

## <a name="install-applications-to-existing-clusters"></a>Toepassingen installeren in bestaande clusters
De volgende procedure beschrijft hoe u HDInsight-toepassingen in een bestaand HDInsight-cluster installeert.

**Een HDInsight-toepassing installeren**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**.
3. Klik op een HDInsight-cluster.  Als u deze niet hebt, maakt u die eerst.  Zie [Clusters maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Klik op **Toepassingen** onder de categorie **Configuraties**. U ziet een lijst met geïnstalleerde toepassingen. Als u Toepassingen niet kunt vinden, betekent dit dat er geen toepassingen zijn voor deze versie van het HDInsight-cluster.
   
    ![Menu van HDInsight-toepassingenportal](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Klik op **toevoegen** in het menu. Er wordt een lijst met bestaande HDInsight-toepassingen weergegeven.
   
    ![HDInsight-toepassingen, beschikbare toepassingen](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klik op een van de beschikbare toepassingen en volg de instructies om de juridische voorwaarden te accepteren.

U kunt de installatiestatus zien in de portalmeldingen (klik boven aan de portal op het belpictogram). Nadat de toepassing is geïnstalleerd, wordt de toepassing wordt weergegeven in de lijst met geïnstalleerde Apps.

## <a name="install-applications-during-cluster-creation"></a>Toepassingen installeren tijdens het maken van het cluster
U hebt ook de optie HDInsight-toepassingen te installeren wanneer u een cluster maakt. Tijdens het proces worden HDInsight-toepassingen geïnstalleerd nadat het cluster is gemaakt en actief is. Voor het installeren van toepassingen tijdens het maken van een cluster met behulp van de Azure portal, u de optie--aangepast--in plaats van de standaard--snel maken - optie.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Lijst van geïnstalleerde HDInsight-apps en -eigenschappen
De portal toont een lijst van de geïnstalleerde HDInsight-toepassingen voor een cluster en de eigenschappen van elke geïnstalleerde toepassing.

**HDInsight-toepassingen en weergave van de eigenschappen weergeven**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**. 
3. Klik op een HDInsight-cluster.
4. Van **instellingen**, klikt u op **toepassingen** onder de **configuratie** categorie. De geïnstalleerde Apps worden weergegeven aan de rechterkant. 
   
    ![HDInsight-toepassingen, geïnstalleerde apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Klik op een van de geïnstalleerde toepassingen om de eigenschap weer te geven. De lijsten met zoekeigenschappen:
   
   * De naam van de app: de naam van de toepassing.
   * Status: toepassingsstatus. 
   * Webpagina: De URL van de web-App die u hebt geïmplementeerd op het edge-knooppunt. De referenties zijn dezelfde als de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het cluster.
   * HTTP-eindpunt: de referenties zijn dezelfde als de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het cluster. 
   * SSH-eindpunt: U kunt SSH gebruiken om te verbinden met het edge-knooppunt. De SSH-referenties zijn dezelfde als de SSH-gebruikersreferenties die u hebt geconfigureerd voor het cluster. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.
6. Als u wilt verwijderen van een toepassing, met de rechtermuisknop op de toepassing en klik vervolgens op **verwijderen** in het contextmenu.

## <a name="connect-to-the-edge-node"></a>Verbinding maken met het edge-knooppunt
U kunt verbinding maken met het edge-knooppunt door middel van HTTP en SSH. Informatie over het eindpunt kunt u vinden in de [portal](#list-installed-hdinsight-apps-and-properties). Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

De HTTP-eindpuntreferenties zijn de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het HDInsight-cluster. De SSH-eindpuntreferenties zijn de SSH-referenties die u hebt geconfigureerd voor het HDInsight-cluster.

## <a name="troubleshoot"></a>Problemen oplossen
Zie [Problemen met de installatie oplossen](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Volgende stappen
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Op Linux gebaseerde HDInsight-clusters aanpassen met behulp van een scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van een scriptactie om extra toepassingen te installeren.
* [Op Linux gebaseerde Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van Resource Manager-sjablonen om HDInsight-clusters te maken.
* [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) (Lege edge-knooppunten gebruiken in HDInsight): meer informatie over het gebruik van een leeg edge-knooppunt om toegang te krijgen tot het HDInsight-cluster, HDInsight toepassingen te testen en HDInsight-toepassingen te hosten.

