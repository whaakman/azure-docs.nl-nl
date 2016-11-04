---
title: Hadoop-toepassingen installeren op HDInsight | Microsoft Docs
description: Informatie over het installeren van HDInsight-toepassingen op HDInsight-toepassingen.
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/14/2016
ms.author: jgao

---
# HDInsight-toepassingen installeren
Een HDInsight-toepassing is een toepassing die gebruikers kunnen installeren op een op Linux gebaseerd HDInsight-cluster. Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf. In dit artikel leert u een gepubliceerde toepassing installeren. Zie voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md). 

Momenteel is er één gepubliceerde toepassing:

* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) biedt analisten een interactieve wijze voor het detecteren, analyseren en visualiseren van de resultaten over big data. Betrek eenvoudig aanvullende gegevensbronnen en ontdek snel nieuwe relaties en antwoorden op vragen.

> [!NOTE]
> Datameer wordt momenteel alleen ondersteund in Azure HDInsight versie 3.2-clusters.
> 
> 

De instructies in dit artikel zijn bestemd voor gebruik met Azure Portal. U kunt ook de Azure Resource Manager-sjabloon exporteren vanuit de portal of een kopie van de Resource Manager-sjabloon van leveranciers verkrijgen, en Azure PowerShell en Azure CLI gebruiken om de sjabloon te implementeren.  Zie [Op Linux gebaseerde Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## Vereisten
Als u HDInsight-toepassingen wilt installeren op een bestaand HDInsight-cluster, hebt u een HDInsight-cluster nodig. Zie [Clusters maken](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster) voor informatie over het maken van een cluster. U kunt ook HDInsight-toepassingen installeren wanneer u een HDInsight-cluster maakt.

## Toepassingen installeren in bestaande clusters
De volgende procedure beschrijft hoe u HDInsight-toepassingen in een bestaand HDInsight-cluster installeert.

**Een HDInsight-toepassing installeren**

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**.  Als u dit niet ziet, klikt u op **Bladeren** en vervolgens op **HDInsight-clusters**.
3. Klik op een HDInsight-cluster.  Als u deze niet hebt, maakt u die eerst.  Zie [Clusters maken](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Klik op de blade **Instellingen** onder de categorie **Algemeen** op **Toepassingen**. Op de blade **Geïnstalleerde apps** vindt u een lijst van de geïnstalleerde toepassingen. 
   
    ![menu van HDInsight-toepassingenportal](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Klik op **Toevoegen** in de menu-blade. 
   
    ![hdinsight-toepassingen, geïnstalleerde apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Er wordt een lijst met bestaande HDInsight-toepassingen weergegeven.
   
    ![hdinsight-toepassingen, beschikbare toepassingen](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klik op een van de toepassingen, ga akkoord met de juridische bepalingen en klik vervolgens op **Selecteren**.

U kunt de installatiestatus zien in de portalmeldingen (klik boven aan de portal op het belpictogram). Nadat de toepassing is geïnstalleerd, wordt de toepassing weergegeven op de blade Geïnstalleerde apps.

## Toepassingen installeren tijdens het maken van het cluster
U hebt ook de optie HDInsight-toepassingen te installeren wanneer u een cluster maakt. Tijdens het proces worden HDInsight-toepassingen geïnstalleerd nadat het cluster is gemaakt en actief is. De volgende procedure beschrijft hoe u HDInsight-toepassingen installeert wanneer u een cluster maakt.

**Een HDInsight-toepassing installeren**

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw**, klik op **Gegevens en analyse** en klik vervolgens op **HDInsight**.
3. Voer **Clusternaam** in: deze naam moet uniek zijn.
4. Klik op **Abonnement** en selecteer het Azure-abonnement dat wordt gebruikt voor het cluster.
5. Klik op **Clustertype selecteren** en selecteer vervolgens:
   
   * **Clustertype**: als u niet weet wat u moet kiezen, selecteert u **Hadoop**. Dit is het meestgebruikte clustertype.
   * **Besturingssysteem**: selecteer **Linux**.
   * **Versie**: gebruik de standaardversie als u niet weet wat u moet kiezen. Zie [HDInsight-clusterversies](hdinsight-component-versioning.md) voor meer informatie.
   * **Clusterlaag**: Azure HDInsight heeft twee categorieën producten voor big data-clouds: de Standard-laag en de Premium-laag. Zie [Clusterlagen](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers) voor meer informatie.
6. Klik op **Toepassingen**, klik op een van de gepubliceerde toepassingen en klik vervolgens op **Selecteren**.
7. Klik op **Referenties** en voer een wachtwoord voor de beheerder in. U moet ook een **SSH-gebruikersnaam** en een **wachtwoord** of **openbare sleutel** invoeren. Deze worden gebruikt voor het verifiëren van de SSH-gebruiker. Het gebruik van een openbare sleutel is de aanbevolen methode. Klik onderaan op **Selecteren** om de referentieconfiguratie op te slaan.
8. Klik op **Gegevensbron**, selecteer een van de bestaande opslagaccounts of maak een nieuw opslagaccount dat moet worden gebruikt als het standaardopslagaccount voor het cluster.
9. Klik op **Resourcegroep** om een bestaande resourcegroep te selecteren of klik op **Nieuw** om een nieuwe resourcegroep te maken
10. Op de blade **Nieuw HDInsight-cluster** zorgt u ervoor dat **Vastmaken aan Startboard** is geselecteerd en vervolgens klikt u op **Maken**. 

## Lijst van geïnstalleerde HDInsight-apps en -eigenschappen
De portal toont een lijst van de geïnstalleerde HDInsight-toepassingen voor een cluster en de eigenschappen van elke geïnstalleerde toepassing.

**HDInsight-toepassingen en weergave-eigenschappen weergeven**

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**.  Als u dit niet ziet, klikt u op **Bladeren** en vervolgens op **HDInsight-clusters**.
3. Klik op een HDInsight-cluster.
4. Klik op de blade **Instellingen** onder de categorie **Algemeen** op **Toepassingen**. Op de blade Geïnstalleerde apps vindt u een lijst van de geïnstalleerde toepassingen. 
   
    ![hdinsight-toepassingen, geïnstalleerde apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Klik op een van de geïnstalleerde toepassingen om de eigenschap weer te geven. Op de blade Eigenschappen staat:
   
   * De naam van de app: de naam van de toepassing.
   * Status: toepassingsstatus. 
   * Webpagina: de URL van de webtoepassing die u hebt geïmplementeerd op het edge-knooppunt, als deze bestaat. De referenties zijn dezelfde als de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het cluster.
   * HTTP-eindpunt: de referenties zijn dezelfde als de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het cluster. 
   * SSH-eindpunt: u kunt [SSH](hdinsight-hadoop-linux-use-ssh-unix.md) gebruiken om verbinding te maken met het edge-knooppunt. De SSH-referenties zijn dezelfde als de SSH-gebruikersreferenties die u hebt geconfigureerd voor het cluster.
6. Als u een toepassing wilt verwijderen, klikt u met de rechtermuisknop op de toepassing en klikt u vervolgens op **Verwijderen** in het contextmenu.

## Verbinding maken met het edge-knooppunt
U kunt verbinding maken met het edge-knooppunt door middel van HTTP en SSH. Informatie over het eindpunt kunt u vinden in de [portal](#list-installed-hdinsight-apps-and-properties). Zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie over het gebruik van SSH. 

De HTTP-eindpuntreferenties zijn de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het HDInsight-cluster. De SSH-eindpuntreferenties zijn de SSH-referenties die u hebt geconfigureerd voor het HDInsight-cluster.

## Problemen oplossen
Zie [Problemen met de installatie oplossen](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## Volgende stappen
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Op Linux gebaseerde HDInsight-clusters aanpassen met behulp van een scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van een scriptactie om extra toepassingen te installeren.
* [Op Linux gebaseerde Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van Resource Manager-sjablonen om HDInsight-clusters te maken.
* [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) (Lege edge-knooppunten gebruiken in HDInsight): meer informatie over het gebruik van een leeg edge-knooppunt om toegang te krijgen tot het HDInsight-cluster, HDInsight toepassingen te testen en HDInsight-toepassingen te hosten.

<!--HONumber=Sep16_HO3-->


