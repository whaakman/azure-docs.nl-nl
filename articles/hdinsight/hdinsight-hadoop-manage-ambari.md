---
title: Controleren en beheren van Azure HDInsight met behulp van Ambari-Webinterface
description: Leer hoe u Ambari gebruiken om te controleren en Linux gebaseerde HDInsight-clusters beheren. In dit document leert u hoe u de Ambari-Webgebruikersinterface opgenomen met HDInsight-clusters.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/23/2018
ms.author: jasonh
ms.openlocfilehash: 3f2c9a246a7252241ffb65a4fa8545ba84e81df8
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43096464"
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-web-ui"></a>HDInsight-clusters beheren met behulp van de Ambari-Webinterface

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereenvoudigt het beheer en bewaking van een Hadoop-cluster met een eenvoudige web-UI en de REST-API gebruiken. Ambari is opgenomen op Linux gebaseerde HDInsight-clusters, en wordt gebruikt om te controleren van het cluster en configuratiewijzigingen aanbrengen.

In dit document leert u hoe u de Ambari-Webinterface gebruiken met een HDInsight-cluster.

## <a id="whatis"></a>Wat is Ambari?

[Apache Ambari](http://ambari.apache.org) vereenvoudigt het beheer van Hadoop door te geven van de web-UI van een eenvoudig te gebruiken. U kunt Ambari gebruiken om te beheren en bewaken van Hadoop-clusters. Ontwikkelaars kunnen deze mogelijkheden integreren in hun toepassingen met behulp van de [Ambari REST-API's](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

De Ambari-Webgebruikersinterface wordt geboden door standaard met HDInsight-clusters die gebruikmaken van de Linux-besturingssysteem.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. 

## <a name="connectivity"></a>Connectiviteit

De Ambari-Webinterface is beschikbaar op uw HDInsight-cluster op HTTPS://CLUSTERNAME.azurehdinsight.net, waarbij **CLUSTERNAME** is de naam van uw cluster.

> [!IMPORTANT]
> Verbinding maken met Ambari op HDInsight vereist HTTPS. Wanneer u hierom wordt gevraagd voor verificatie, gebruikt u de accountnaam van beheerder en het wachtwoord die u hebt opgegeven bij het cluster is gemaakt.

## <a name="ssh-tunnel-proxy"></a>SSH-tunnel (proxy)

Terwijl Ambari voor uw cluster rechtstreeks via Internet toegankelijk is, worden enkele koppelingen van de Ambari-Webgebruikersinterface (bijvoorbeeld bij de JobTracker) niet weergegeven op het internet. Voor toegang tot deze services, moet u een SSH-tunnel maken. Zie voor meer informatie, [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari-Webgebruikersinterface

> [!WARNING]
> Niet alle functies van de Ambari-Webgebruikersinterface worden ondersteund op HDInsight. Zie voor meer informatie de [niet-ondersteunde bewerkingen](#unsupported-operations) sectie van dit document.

Bij het verbinden met de Ambari-Webgebruikersinterface, wordt u gevraagd om te verifiëren naar de pagina. Gebruik de cluster de gebruiker met beheerdersrechten (standaard Admin) en het wachtwoord die u hebt gebruikt tijdens het maken van een cluster.

Wanneer de pagina wordt geopend, houd er rekening mee de balk aan de bovenkant. Deze balk bevat de volgende informatie en besturingselementen:

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari-logo** -Hiermee opent u het dashboard, die kan worden gebruikt voor het bewaken van het cluster.

* **De naam # ops cluster** -geeft het aantal actieve Ambari-bewerkingen. De clusternaam van de selecteren of **# ops** geeft een lijst van bewerkingen op de achtergrond.

* **# waarschuwingen** -waarschuwingen of kritieke waarschuwingen weergeven, indien aanwezig, voor het cluster.

* **Dashboard** -het dashboard wordt weergegeven.

* **Services** -gegevens en configuratie-instellingen voor de services in het cluster.

* **Hosts** -gegevens en configuratie-instellingen voor de knooppunten in het cluster.

* **Waarschuwingen** -een logboek van informatie, waarschuwingen en kritieke waarschuwingen.

* **Beheerder** -Software stack/services die zijn geïnstalleerd op het cluster, service-accountgegevens en Kerberos-beveiliging.

* **Beheerder knop** -Ambari management, gebruikersinstellingen en meld u af.

## <a name="monitoring"></a>Bewaking

### <a name="alerts"></a>Waarschuwingen

De volgende lijst bevat de algemene waarschuwing statussen die worden gebruikt door Ambari:

* **OK**
* **Waarschuwing**
* **KRITIEKE**
* **UNKNOWN**

Anders dan een waarschuwing **OK** ertoe leiden dat de **# waarschuwingen** vermelding aan de bovenkant van de pagina om het aantal waarschuwingen weer te geven. Dit item selecteren, geeft de waarschuwingen en hun status weer.

Waarschuwingen worden ingedeeld in verschillende standaardgroepen, dat kunnen worden weergegeven in de **waarschuwingen** pagina.

![Pagina met waarschuwingen](./media/hdinsight-hadoop-manage-ambari/alerts.png)

U kunt de groepen beheren met behulp van de **acties** menu en selecteer **waarschuwing groepen beheren**.

![dialoogvenster Waarschuwing groepen beheren](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

U kunt ook waarschuwingen methoden beheren en maken van meldingen van waarschuwingen van de **acties** menu hiervoor __waarschuwing meldingen beheren__. Huidige meldingen worden weergegeven. U kunt ook meldingen hier maken. Meldingen kunnen worden verzonden via **e** of **SNMP** wanneer specifieke waarschuwing/ernst combinaties worden uitgevoerd. Bijvoorbeeld, u kunt een e-mail sturen wanneer een van de waarschuwingen in de **YARN standaard** groep is ingesteld op **kritieke**.

![Waarschuwing dialoogvenster maken](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Ten slotte selecteren __Waarschuwingsinstellingen beheren__ uit de __acties__ menu kunt u instellen hoe vaak een waarschuwing plaatsvinden moet voordat een melding wordt verzonden. Deze instelling kan worden gebruikt om te voorkomen dat meldingen voor tijdelijke fouten.

### <a name="cluster"></a>Cluster

De **metrische gegevens** van het dashboard bevat een reeks widgets gemakkelijk de status van uw cluster in één oogopslag controleren. Diverse widgets, zoals **CPU-gebruik**, bieden aanvullende informatie wanneer geklikt.

![dashboard met metrische gegevens](./media/hdinsight-hadoop-manage-ambari/metrics.png)

De **Heatmaps** tabblad metrische gegevens als gekleurde heatmaps beschikbaar zijn, die van groen naar rood weergegeven.

![dashboard met heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Voor meer informatie over de knooppunten in het cluster, selecteert u **Hosts**. Selecteer vervolgens de specifieke knooppunt waarin u geïnteresseerd bent.

![details van de host](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Services

De **Services** zijbalk op het dashboard biedt snel inzicht in de status van de services die worden uitgevoerd op het cluster. Verschillende pictogrammen worden gebruikt om aan te geven van de status of de acties die moeten worden uitgevoerd. Een gele Prullenbak-symbool wordt bijvoorbeeld weergegeven als een service moet worden gerecycled.

![Services-zijbalk](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> De services die worden weergegeven verschillen van het HDInsight-clustertypen en opties versies. De services die hier wordt weergegeven, is mogelijk anders dan de services die worden weergegeven voor uw cluster.

Selecteren van een service, geeft meer gedetailleerde informatie weer op de service.

![samenvattende informatie van service](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Snelkoppelingen

De weergave van sommige services een **snelkoppelingen** koppelen aan de bovenkant van de pagina. Dit kan worden gebruikt voor toegang krijgen tot specifieke services webgebruikersinterfaces, zoals:

* **Taakgeschiedenis** -geschiedenis van MapReduce-taak.
* **Resource Manager** -YARN ResourceManager-interface.
* **NameNode** -Hadoop Distributed File System (HDFS) NameNode gebruikersinterface.
* **Oozie-Webgebruikersinterface** -Oozie-gebruikersinterface.

Wanneer u een van de volgende koppelingen, wordt een nieuw tabblad geopend in uw browser, die de geselecteerde pagina wordt weergegeven.

> [!NOTE]
> Selecteren van de **snelkoppelingen** vermelding voor een service een fout 'de server is niet gevonden' retourneert. Als u deze fout optreedt, moet u een SSH-tunnel gebruiken bij het gebruik van de **snelkoppelingen** vermelding voor deze service. Zie voor meer informatie, [SSH-Tunneling gebruiken met HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Beheer

### <a name="ambari-users-groups-and-permissions"></a>Ambari-gebruikers, groepen en machtigingen

Werken met gebruikers, groepen en machtigingen worden ondersteund bij het gebruik van een [toegevoegd aan een domein](./domain-joined/apache-domain-joined-introduction.md) HDInsight-cluster. Zie voor meer informatie over het gebruik van de Ambari-Webgebruikersinterface voor beheer op een domein gekoppeld cluster [aan domein gekoppelde HDInsight-clusters beheren](./domain-joined/apache-domain-joined-introduction.md).

> [!WARNING]
> Wijzig het wachtwoord van de Ambari-watchdog (hdinsightwatchdog) op uw Linux gebaseerde HDInsight-cluster niet. Wijzigen van het wachtwoord, verbreekt de mogelijkheid om te gebruiken van scriptacties of vergroten / verkleinen met uw cluster uit te voeren.

### <a name="hosts"></a>Hosts

De **Hosts** pagina geeft een lijst van alle hosts in het cluster. Volg deze stappen voor het beheren van hosts.

![hosts pagina](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Toe te voegen, uit bedrijf nemen en een host recommissioning mag niet worden gebruikt met HDInsight-clusters.

1. Selecteer de host die u wilt beheren.

2. Gebruik de **acties** menu selecteert u de actie die u wilt uitvoeren:

   * **Start alle onderdelen** -alle onderdelen starten op de host.

   * **Alle onderdelen stoppen** -alle onderdelen stoppen op de host.

   * **Alle onderdelen opnieuw starten** - stoppen en starten van alle onderdelen op de host.

   * **Onderhoudsmodus inschakelen** -onderdrukt waarschuwingen voor de host. In deze modus moet worden ingeschakeld als u bij het uitvoeren van acties die waarschuwingen genereren. Bijvoorbeeld een service starten en stoppen.

   * **De onderhoudsmodus uitschakelen** -retourneert de host de normale waarschuwingen.

   * **Stop** -stopt DataNode of NodeManagers op de host.

   * **Start** -DataNode wordt gestart of NodeManagers op de host.

   * **Opnieuw opstarten** -stopt en start DataNode of NodeManagers op de host.

   * **Uit bedrijf nemen** -Hiermee verwijdert u een host uit het cluster.

     > [!NOTE]
     > Deze actie niet gebruiken op HDInsight-clusters.

   * **Recommission** -voegt een eerder buiten gebruik gestelde host aan het cluster.

     > [!NOTE]
     > Deze actie niet gebruiken op HDInsight-clusters.

### <a id="service"></a>Services

Uit de **Dashboard** of **Services** pagina, gebruikt u de **acties** knop onder aan de lijst met services op alle services stoppen en starten.

![Service-acties](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> Terwijl **Add Service** wordt vermeld in dit menu het moet niet worden gebruikt voor nieuwe services toegevoegd aan het HDInsight-cluster. Nieuwe services moeten worden toegevoegd met behulp van een scriptactie tijdens de clusterinrichting. Zie voor meer informatie over het gebruik van scriptacties [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

Terwijl de **acties** knop kunt alle services opnieuw starten, vaak u wilt starten, stoppen of opnieuw opstarten van een specifieke service. Gebruik de volgende stappen om acties uitvoeren op een afzonderlijke service:

1. Uit de **Dashboard** of **Services** pagina, selecteert u een service.

2. Vanaf de bovenkant van de **samenvatting** tabblad, gebruikt u de **serviceacties** en selecteer de actie te ondernemen. De service op alle knooppunten opnieuw is opgestart.

    ![serviceactie](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > Sommige services opnieuw te starten terwijl de cluster wordt uitgevoerd, kan waarschuwingen genereren. Om waarschuwingen te voorkomen, kunt u de **serviceacties** knop om in te schakelen **onderhoudsmodus** voor de service voordat u het opnieuw opstarten.

3. Wanneer een actie is geselecteerd, de **# op** vermelding aan de bovenkant van de stappen van de pagina om weer te geven dat een achtergrondbewerking plaatsvindt. Als geconfigureerd om weer te geven, wordt de lijst met bewerkingen op de achtergrond wordt weergegeven.

   > [!NOTE]
   > Als u ingeschakeld **onderhoudsmodus** onthouden voor de service uitschakelen met behulp van de **serviceacties** knop nadat de bewerking is voltooid.

Voor het configureren van een service, gebruikt u de volgende stappen uit:

1. Uit de **Dashboard** of **Services** pagina, selecteert u een service.

2. Selecteer de **Peeringconfiguraties** tabblad. De huidige configuratie wordt weergegeven. Een lijst met vorige configuraties wordt ook weergegeven.

    ![Configuraties](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Gebruik de velden weergegeven voor het wijzigen van de configuratie en selecteer vervolgens **opslaan**. Of Selecteer een vorige configuratie en selecteer vervolgens **instellen als huidige** terug moet brengen naar de vorige instellingen.

## <a name="ambari-views"></a>Ambari-weergaven

Ambari-weergaven kunnen ontwikkelaars plug-UI-elementen in de Ambari-Webinterface met behulp van de [Ambari-weergaven Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight biedt de volgende weergaven met Hadoop-clustertypen:


* Hive-weergave: De weergave Hive kunt u het uitvoeren van Hive-query's rechtstreeks vanuit uw webbrowser. U kunt query's opslaan, resultaten weergeven, de resultaten in de clusteropslag opslaan of resultaten downloaden naar uw lokale systeem. Zie voor meer informatie over het gebruik van Hive-weergaven [Hive-weergaven gebruiken met HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md).

* Tez weergave: De Tez-weergave kunt u beter te begrijpen en optimaliseren van taken. U kunt informatie weergeven op hoe de Tez-taken worden uitgevoerd en welke resources worden gebruikt.

## <a name="unsupported-operations"></a>Niet-ondersteunde bewerkingen

De volgende Ambari-bewerkingen worden niet ondersteund op HDInsight:

* __De metrische gegevens Collector-service verplaatsen__. Als u informatie bekijkt in de metrische gegevens Collector-service, is een van de acties die beschikbaar zijn in het menu Acties van de Service __verplaatsen metrische gegevens collector__. Dit wordt niet ondersteund met HDInsight.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van de [Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md) met HDInsight.
