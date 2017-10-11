---
title: Hadoop-toepassingen van derden installeren op Azure HDInsight | Microsoft Docs
description: Informatie over het installeren van Hadoop-toepassingen van derden op Azure HDInsight.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/16/2017
ms.author: jgao
ms.openlocfilehash: 3b3ff8d33959978ddd648e59a6a301f00c247964
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Hadoop-toepassingen van derden op Azure HDInsight installeren

In dit artikel leert u hoe u een toepassing al gepubliceerde van derden Hadoop installeert op Azure HDInsight. Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

Een HDInsight-toepassing is een toepassing die gebruikers kunnen installeren op een op Linux gebaseerd HDInsight-cluster. Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf.  

Momenteel zijn er vier gepubliceerde toepassingen:

* **DATAIKU DDS op HDInsight**: Dataiku DSS (Data Science Studio) is software waarmee gegevensprofessionals (gegevenswetenschappers, bedrijfsanalisten, ontwikkelaars enz.) zeer specifieke services kunnen bouwen, testen en implementeren. Met deze services worden onbewerkte gegevens getransformeerd in belangrijke bedrijfsvoorspellingen.
* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) biedt analisten een interactieve wijze voor het detecteren, analyseren en visualiseren van de resultaten over big data. Betrek eenvoudig aanvullende gegevensbronnen en ontdek snel nieuwe relaties en antwoorden op vragen.
* **StreamSets Data Collector voor HDInsight** biedt een complete geïntegreerde IDE (Integrated Development Environment) waarin u opnamepijplijnen die stream- en batchgegevens filteren en diverse in-stream transformaties bevatten, in alle richtingen kunt ontwerpen, testen, implementeren en beheren, en dat alles zonder aangepaste code te hoeven schrijven. 
* **Cask CDAP voor HDInsight** biedt de eerste unified integratieplatform voor big data dat wordt het de tijd voor de productie voor data-toepassingen en gegevens meren met 80%. Deze toepassing biedt alleen ondersteuning voor standaard HBase 3.4-clusters.
* **H2O kunstmatige Intelligence voor HDInsight (bèta)** H2O mineraalwater ondersteunt de volgende algoritmen voor gedistribueerde: GLM, Naïve Bayes, gedistribueerde willekeurige Forest, kleurovergang versterking Machine, Deep Neural Networks, Deep leren, K-means, PCA, Lage waarden van positie modellen, Afwijkingsdetectie en Autoencoders gegeneraliseerd.
* **Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) is een enterprise-ready datawarehouse aangedreven door Apache Kylin en Apache Hadoop; het onderliggende seconde Querylatentie voor grootschalige gegevensset machtigt en gegevensanalyse voor vereenvoudigt zakelijke gebruikers en analisten. 
* **SnapLogic Hadooplex** de SnapLogic Hadooplex uitgevoerd op HDInsight kunnen klanten om te krijgen tot zakelijke inzichten sneller door selfservice gegevensopname en de voorbereiding van vrijwel elke bron voor het Microsoft Azure cloud-platform.
* **Spark-taakserver voor KNIME Spark Executor** Spark-taakserver voor KNIME Spark Executor wordt gebruikt voor het KNIME Analytics Platform verbinding met HDInsight-clusters.

De instructies in dit artikel zijn bestemd voor gebruik met Azure Portal. U kunt ook de Azure Resource Manager-sjabloon exporteren vanuit de portal of een kopie van de Resource Manager-sjabloon van leveranciers verkrijgen, en Azure PowerShell en Azure CLI gebruiken om de sjabloon te implementeren.  Zie [Op Linux gebaseerde Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Vereisten
Als u HDInsight-toepassingen wilt installeren op een bestaand HDInsight-cluster, hebt u een HDInsight-cluster nodig. Zie [Clusters maken](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster) voor informatie over het maken van een cluster. U kunt ook HDInsight-toepassingen installeren wanneer u een HDInsight-cluster maakt.

## <a name="install-applications-to-existing-clusters"></a>Toepassingen installeren in bestaande clusters
De volgende procedure beschrijft hoe u HDInsight-toepassingen in een bestaand HDInsight-cluster installeert.

**Een HDInsight-toepassing installeren**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**.  Als u dit niet ziet, klikt u op **Meer services** en vervolgens op **HDInsight-clusters**.
3. Klik op een HDInsight-cluster.  Als u deze niet hebt, maakt u die eerst.  Zie [Clusters maken](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Klik op **Toepassingen** onder de categorie **Configuraties**. Hier ziet u een lijst met geïnstalleerde toepassingen. Als u Toepassingen niet kunt vinden, betekent dit dat er geen toepassingen zijn voor deze versie van het HDInsight-cluster.
   
    ![Menu van HDInsight-toepassingenportal](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Klik op **Toevoegen** in de menu-blade. 
   
    ![HDInsight-toepassingen, geïnstalleerde apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Er wordt een lijst met bestaande HDInsight-toepassingen weergegeven.
   
    ![HDInsight-toepassingen, beschikbare toepassingen](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klik op een van de toepassingen, ga akkoord met de juridische bepalingen en klik vervolgens op **Selecteren**.

U kunt de installatiestatus zien in de portalmeldingen (klik boven aan de portal op het belpictogram). Nadat de toepassing is geïnstalleerd, wordt de toepassing weergegeven op de blade Geïnstalleerde apps.

## <a name="install-applications-during-cluster-creation"></a>Toepassingen installeren tijdens het maken van het cluster
U hebt ook de optie HDInsight-toepassingen te installeren wanneer u een cluster maakt. Tijdens het proces worden HDInsight-toepassingen geïnstalleerd nadat het cluster is gemaakt en actief is. De volgende procedure beschrijft hoe u HDInsight-toepassingen installeert wanneer u een cluster maakt.

**Een HDInsight-toepassing installeren**

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw**, klik op **Gegevens en analyse** en klik vervolgens op **HDInsight**.
3. Voer **Clusternaam** in: deze naam moet uniek zijn.
4. Klik op **abonnement** selecteren van het Azure-abonnement dat wordt gebruikt voor het cluster.
5. Klik op **Clustertype selecteren** en selecteer vervolgens:
   
   * **Clustertype**: als u niet weet wat u moet kiezen, selecteert u **Hadoop**. Dit is het meestgebruikte clustertype.
   * **Besturingssysteem**: selecteer **Linux**.
   * **Versie**: gebruik de standaardversie als u niet weet wat u moet kiezen. Zie [HDInsight-clusterversies](hdinsight-component-versioning.md) voor meer informatie.
   * **Clusterlaag**: Azure HDInsight heeft twee categorieën producten voor big data-clouds: de Standard-laag en de Premium-laag. Zie [Clusterlagen](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers) voor meer informatie.
6. Klik op **Toepassingen**, klik op een van de gepubliceerde toepassingen en klik vervolgens op **Selecteren**.
7. Klik op **Referenties** en voer een wachtwoord voor de beheerder in. U moet ook invoeren een **SSH-gebruikersnaam** en een **wachtwoord** of **openbare sleutel**, dat wordt gebruikt voor het verifiëren van de SSH-gebruiker. Het gebruik van een openbare sleutel is de aanbevolen methode. Klik onderaan op **Selecteren** om de referentieconfiguratie op te slaan.
8. Klik op **gegevensbron**, selecteer een van de bestaande opslagaccounts of maak een nieuw opslagaccount moet worden gebruikt als het standaardopslagaccount voor het cluster.
9. Klik op **Resourcegroep** om een bestaande resourcegroep te selecteren of klik op **Nieuw** om een nieuwe resourcegroep te maken
10. Op de blade **Nieuw HDInsight-cluster** zorgt u ervoor dat **Vastmaken aan Startboard** is geselecteerd en vervolgens klikt u op **Maken**. 

## <a name="list-installed-hdinsight-apps-and-properties"></a>Lijst van geïnstalleerde HDInsight-apps en -eigenschappen
De portal toont een lijst van de geïnstalleerde HDInsight-toepassingen voor een cluster en de eigenschappen van elke geïnstalleerde toepassing.

**HDInsight-toepassingen en weergave-eigenschappen weergeven**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**.  Als u dit niet ziet, klikt u op **Bladeren** en vervolgens op **HDInsight-clusters**.
3. Klik op een HDInsight-cluster.
4. Klik op de blade **Instellingen** onder de categorie **Algemeen** op **Toepassingen**. Op de blade Geïnstalleerde apps vindt u een lijst van de geïnstalleerde toepassingen. 
   
    ![HDInsight-toepassingen, geïnstalleerde apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Klik op een van de geïnstalleerde toepassingen om de eigenschap weer te geven. Op de blade Eigenschappen staat:
   
   * De naam van de app: de naam van de toepassing.
   * Status: toepassingsstatus. 
   * Webpagina: De URL van de webtoepassing die u hebt geïmplementeerd met het edge-knooppunt. De referenties zijn dezelfde als de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het cluster.
   * HTTP-eindpunt: de referenties zijn dezelfde als de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het cluster. 
   * SSH-eindpunt: U kunt SSH verbinding maken met het edge-knooppunt. De SSH-referenties zijn dezelfde als de SSH-gebruikersreferenties die u hebt geconfigureerd voor het cluster. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.
6. Voor het verwijderen van een toepassing met de rechtermuisknop op de toepassing en klik vervolgens op **verwijderen** in het contextmenu.

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

