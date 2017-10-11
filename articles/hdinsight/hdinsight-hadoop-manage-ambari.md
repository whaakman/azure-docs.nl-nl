---
title: Bewaken en beheren van Azure HDInsight met behulp van de Ambari-Webgebruikersinterface | Microsoft Docs
description: Informatie over het Ambari gebruiken om te controleren en beheren van Linux gebaseerde HDInsight-clusters. In dit document leert u hoe u de Ambari-Webgebruikersinterface opgenomen met HDInsight-clusters.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: dc0f9ff030f70985dad0f3b74ba0ee3dda1d9f4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-web-ui"></a>HDInsight-clusters beheren met behulp van de Ambari-Webgebruikersinterface

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereenvoudigt het beheer en bewaking van een Hadoop-cluster met een eenvoudige web-UI en REST-API gebruiken. Ambari is opgenomen op Linux gebaseerde HDInsight-clusters en wordt gebruikt om te controleren van het cluster en configuratiewijzigingen aanbrengen.

In dit document leert u hoe u de Ambari-Webgebruikersinterface met een HDInsight-cluster.

## <a id="whatis"></a>Wat is Ambari?

[Apache Ambari](http://ambari.apache.org) vereenvoudigt Hadoop-beheer door de webgebruikersinterface van een eenvoudig te gebruiken. Kunt u Ambari maken, beheren en bewaken van Hadoop-clusters. Ontwikkelaars kunnen deze mogelijkheden integreren in hun toepassingen met behulp van de [Ambari REST-API's](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

De Ambari-Webgebruikersinterface is standaard met HDInsight-clusters die gebruikmaken van de Linux-besturingssysteem opgegeven.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. 

## <a name="connectivity"></a>Connectiviteit

De Ambari-Webgebruikersinterface is beschikbaar op uw HDInsight-cluster op HTTPS://CLUSTERNAME.azurehdidnsight.net, waarbij **CLUSTERNAME** is de naam van uw cluster.

> [!IMPORTANT]
> Verbinding maken met Ambari op HDInsight HTTPS is vereist. Wanneer u om verificatie wordt gevraagd, gebruikt de admin-accountnaam en het wachtwoord die u hebt opgegeven toen het cluster is gemaakt.

## <a name="ssh-tunnel-proxy"></a>SSH-tunnel (proxy)

Terwijl de Ambari voor uw cluster rechtstreeks via Internet toegankelijk is, worden sommige koppelingen van de Ambari-Webgebruikersinterface (zoals het de JobTracker) niet weergegeven op het internet. Voor toegang tot deze services, moet u een SSH-tunnel maken. Zie voor meer informatie [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari-webgebruikersinterface

Bij het verbinden met de Ambari-Webgebruikersinterface, wordt u gevraagd om de pagina te verifiëren. Gebruik de gebruiker met beheerdersrechten cluster (standaard Admin) en het wachtwoord waarmee u tijdens het maken van het cluster.

Wanneer de pagina wordt geopend, noteert u de balk aan de bovenkant. Deze balk bevat de volgende informatie en besturingselementen:

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari-logo** -Hiermee opent u het dashboard, die kan worden gebruikt voor het bewaken van het cluster.

* **Naam # ops cluster** -geeft het aantal actieve Ambari-bewerkingen. Naam van het cluster te selecteren of **# ops** geeft een lijst met bewerkingen op de achtergrond.

* **waarschuwingen voor #** -waarschuwingen of kritieke waarschuwingen weergeven, indien aanwezig, voor het cluster.

* **Dashboard** -het dashboard wordt weergegeven.

* **Services** -gegevens en configuratie-instellingen voor de services in het cluster.

* **Hosts** -gegevens en configuratie-instellingen voor de knooppunten in het cluster.

* **Waarschuwingen** -een logboek van informatie, waarschuwingen en kritieke waarschuwingen.

* **Beheerder** -Software stack/services die zijn geïnstalleerd op de cluster-serviceaccount en Kerberos-beveiliging.

* **Knop Admin** -Ambari beheer, gebruikersinstellingen en meld u af.

## <a name="monitoring"></a>Bewaking

### <a name="alerts"></a>Waarschuwingen

De volgende lijst bevat de algemene waarschuwing statussen die worden gebruikt door Ambari:

* **OK**
* **Waarschuwing**
* **KRITIEKE**
* **ONBEKEND**

Anders dan waarschuwingen **OK** ertoe leiden dat de **# waarschuwingen** vermelding aan de bovenkant van de pagina om het aantal waarschuwingen weer te geven. Als u dit item wordt weergegeven, de waarschuwingen en hun status.

Waarschuwingen worden ingedeeld in verschillende standaardgroepen die kunnen worden bekeken in de **waarschuwingen** pagina.

![pagina waarschuwingen](./media/hdinsight-hadoop-manage-ambari/alerts.png)

U kunt de groepen beheren met behulp van de **acties** menu en selecteer **waarschuwing groepen beheren**.

![dialoogvenster Waarschuwing groepen beheren](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

U kunt ook waarschuwingen methoden beheren en meldingen van waarschuwingen van de **acties** menu door te selecteren __waarschuwing meldingen beheren__. Huidige meldingen worden weergegeven. U kunt ook meldingen van hieruit maken. Meldingen kunnen worden verzonden **e** of **SNMP** wanneer specifieke waarschuwing/ernst combinaties optreden. U kunt bijvoorbeeld een e-mailbericht wanneer een van de waarschuwingen in verzenden de **YARN standaard** groep is ingesteld op **Kritiek**.

![Waarschuwing dialoogvenster maken](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Ten slotte selecteren __Waarschuwingsinstellingen beheren__ van de __acties__ menu kunt u instellen hoe vaak een waarschuwing optreden moet voordat een melding wordt verzonden. Deze instelling kan worden gebruikt om te voorkomen dat meldingen voor tijdelijke fouten.

### <a name="cluster"></a>Cluster

De **metrische gegevens** van het dashboard bevat een reeks widgets om gemakkelijk om de status van het cluster in één oogopslag te controleren. Verschillende widgets zoals **CPU-gebruik**, vindt u aanvullende informatie wanneer geklikt.

![dashboard met metrische gegevens](./media/hdinsight-hadoop-manage-ambari/metrics.png)

De **Heatmaps** tabblad metrische gegevens als gekleurde heatmaps, gaan van groen in rood weergegeven.

![dashboard met heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Voor meer informatie over de knooppunten in het cluster, selecteert u **Hosts**. Selecteer het specifieke knooppunt dat u geïnteresseerd bent in.

![details van de host](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Services

De **Services** zijbalk op het dashboard biedt snel inzicht in de status van de services die worden uitgevoerd op het cluster. Verschillende pictogrammen worden gebruikt om aan te geven, status of acties die moeten worden uitgevoerd. Een gele recyclen symbool wordt bijvoorbeeld weergegeven als een service moet worden gerecycled.

![Services Zijmarge](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> De services die worden weergegeven verschillen van HDInsight-clustertypen en versies. De services die worden hier weergegeven mogelijk anders dan de services die worden weergegeven voor uw cluster.

Selecteren van een service geeft meer gedetailleerde informatie weer voor de service.

![samenvattende informatie van service](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Snelkoppelingen

Sommige services weer een **snelkoppelingen** koppeling aan de bovenkant van de pagina. Dit kan worden gebruikt om te openen, servicespecifieke web UI, zoals:

* **Taakgeschiedenis** -geschiedenis van MapReduce-taak.
* **Resource Manager** -gebruikersinterface van YARN ResourceManager.
* **NameNode** -Hadoop Distributed File System (HDFS) NameNode gebruikersinterface.
* **Oozie-webgebruikersinterface** -Oozie-gebruikersinterface.

Wanneer u een van de volgende koppelingen, wordt een nieuw tabblad geopend in uw browser, die de geselecteerde pagina wordt weergegeven.

> [!NOTE]
> Als u de **snelkoppelingen** vermelding voor een service een fout 'de server is niet gevonden' retourneert. Als deze fout optreedt, moet u een SSH-tunnel gebruiken wanneer u de **snelkoppelingen** post voor deze service. Zie voor informatie [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Beheer

### <a name="ambari-users-groups-and-permissions"></a>Ambari-gebruikers, groepen en machtigingen

Werken met gebruikers, groepen en machtigingen worden ondersteund wanneer een [verbonden met het domein](hdinsight-domain-joined-introduction.md) HDInsight-cluster. Zie voor meer informatie over het gebruik van de Ambari-Management-gebruikersinterface op een cluster domein [domein HDInsight-clusters beheren](hdinsight-domain-joined-introduction.md).

> [!WARNING]
> Wijzig het wachtwoord van de Ambari-watchdog (hdinsightwatchdog) op uw Linux gebaseerde HDInsight-cluster niet. Het wachtwoord wilt wijzigen, verbreekt de mogelijkheid voor het gebruik van scriptacties of vergroten/verkleinen bewerkingen uitvoeren met uw cluster.

### <a name="hosts"></a>Hosts

De **Hosts** pagina geeft een lijst van alle hosts in het cluster. Volg deze stappen voor het beheren van hosts.

![hosts pagina](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Toe te voegen, uit bedrijf nemen en een host recommissioning mag niet worden gebruikt met HDInsight-clusters.

1. Selecteer de host die u wilt beheren.

2. Gebruik de **acties** om te selecteren van de actie die u wilt uitvoeren:

   * **Start alle onderdelen** -alle onderdelen die zijn gestart op de host.

   * **Stopt alle onderdelen** -stopt alle onderdelen op de host.

   * **Alle onderdelen opnieuw** - stoppen en starten van alle onderdelen op de host.

   * **Onderhoudsmodus inschakelen** -waarschuwingen onderdrukt voor de host. Deze modus moet worden ingeschakeld als u bij het uitvoeren van acties die waarschuwingen genereren. Bijvoorbeeld, een service starten en stoppen.

   * **De onderhoudsmodus uitschakelen** -retourneert de host de normale waarschuwingen.

   * **Stop** -stopt DataNode of NodeManagers op de host.

   * **Start** -DataNode wordt gestart of NodeManagers op de host.

   * **Opnieuw opstarten** -wordt gestopt en gestart DataNode of NodeManagers op de host.

   * **Buiten gebruik stellen** -Hiermee verwijdert u een host uit het cluster.

     > [!NOTE]
     > Deze actie niet gebruiken op HDInsight-clusters.

   * **Recommission** -voegt een eerder buiten gebruik gestelde host aan het cluster.

     > [!NOTE]
     > Deze actie niet gebruiken op HDInsight-clusters.

### <a id="service"></a>Services

Van de **Dashboard** of **Services** pagina, gebruikt u de **acties** knop aan de onderkant van de lijst met services stoppen en starten van alle services.

![serviceacties](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> Terwijl **Service toevoegen** wordt vermeld in dit menu deze moet niet worden gebruikt voor nieuwe services toegevoegd aan het HDInsight-cluster. Nieuwe services moeten worden toegevoegd met behulp van een scriptactie tijdens de clusterinrichting. Zie voor meer informatie over het gebruik van scriptacties [aanpassen HDInsight-clusters met behulp van scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

Terwijl de **acties** knop kunt alle services opnieuw starten, vaak u wilt starten, stoppen of opnieuw opstarten van een specifieke service. Gebruik de volgende stappen voor het uitvoeren van acties op een afzonderlijke service:

1. Van de **Dashboard** of **Services** pagina, selecteert u een service.

2. Vanaf de bovenkant van de **samenvatting** tabblad, gebruikt u de **serviceacties** knop en selecteer de actie te ondernemen. De service op alle knooppunten opnieuw is opgestart.

    ![serviceactie](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > Sommige services opnieuw te starten, terwijl het cluster wordt uitgevoerd, kan waarschuwingen genereren. Om waarschuwingen te voorkomen, kunt u de **serviceacties** knop om in te schakelen **onderhoudsmodus** voor de service voordat u de computer opnieuw uitvoert.

3. Wanneer een actie is geselecteerd, de **op #** vermelding aan de bovenkant van de pagina stapsgewijs verhoogd om weer te geven dat een achtergrondbewerking is gemaakt. Als geconfigureerd om weer te geven, wordt de lijst met bewerkingen op de achtergrond weergegeven.

   > [!NOTE]
   > Als u ingeschakeld **onderhoudsmodus** voor de service, moet u dit programma uitschakelen met de **serviceacties** knop nadat de bewerking is voltooid.

Gebruik de volgende stappen voor het configureren van een service:

1. Van de **Dashboard** of **Services** pagina, selecteert u een service.

2. Selecteer de **Configs** tabblad. De huidige configuratie wordt weergegeven. Een lijst met vorige configuraties wordt ook weergegeven.

    ![Configuraties](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Gebruik de velden weergegeven om te wijzigen van de configuratie en selecteer vervolgens **opslaan**. Of Selecteer een configuratie van de vorige en selecteert u **instellen als huidige** kunt terugkeren naar de vorige instellingen.

## <a name="ambari-views"></a>Ambari-weergaven

Ambari-weergaven kunnen ontwikkelaars plug-UI-elementen in de Ambari-Webgebruikersinterface met behulp van de [Ambari-weergaven Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight biedt de volgende weergaven met Hadoop-clustertypen:

* Yarn Queue Manager: queue manager biedt een eenvoudige gebruikersinterface voor het weergeven en wijzigen van YARN-wachtrijen.

* Hive-weergave: De Hive-weergave kunt u het uitvoeren van Hive-query's rechtstreeks vanuit uw webbrowser. U kunt query's opslaan, resultaten weergeven, resultaten opslaan in de clusteropslag of resultaten downloaden naar het lokale systeem. Zie voor meer informatie over het gebruik van Hive-weergaven [Hive-weergaven gebruiken met HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).

* Tez weergave: De Tez-weergave kunt u beter te begrijpen en taken te optimaliseren. U kunt informatie weergeven over hoe Tez-taken worden uitgevoerd en welke bronnen worden gebruikt.
