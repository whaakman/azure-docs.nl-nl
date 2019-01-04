---
title: Toegang tot Hadoop YARN-toepassingslogboeken programmatisch - Azure
description: Access-toepassing zich aanmeldt via een programma een Hadoop-cluster in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 7326cf6a1153d5dc1f7e5f910a376a21b05db606
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725437"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-windows-based-hdinsight"></a>Toegang Apache Hadoop YARN-toepassingslogboeken in HDInsight op basis van Windows
Dit document wordt uitgelegd hoe u toegang tot de logboeken voor [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) toepassingen die klaar bent met het op een Windows-gebaseerde Apache Hadoop-cluster in Azure HDInsight

> [!IMPORTANT]  
> De informatie in dit document geldt alleen voor Windows-gebaseerde HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over het verkrijgen van toegang tot YARN-op Linux gebaseerde HDInsight-clusters logboeken [toegang Apache Hadoop YARN-toepassingslogboeken in Linux-gebaseerde Apache Hadoop op HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md).


### <a name="prerequisites"></a>Vereisten
* Een Windows-gebaseerde HDInsight-cluster.  Zie [Windows maken op basis van Apache Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN Timeline Server
De <a href="https://hadoop.apache.org/docs/r2.4.1/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Apache Hadoop YARN Timeline Server</a> biedt algemene informatie over voltooide toepassingen ook als frameworkspecifieke toepassingsinformatie via twee verschillende interfaces. Specifiek:

* Opslaan en ophalen van informatie over algemene toepassingen op HDInsight-clusters is ingeschakeld met versie 3.1.1.374 of hoger zijn.
* Een onderdeel van de application framework-specifieke gegevens van de tijdlijn-Server is momenteel niet beschikbaar op HDInsight-clusters.

Algemene informatie over toepassingen bevat de volgende soorten gegevens:

* De toepassings-ID, een unieke id van een toepassing
* De gebruiker die de toepassing is gestart
* Meer informatie over een poging om uit te voeren van de toepassing
* De containers die worden gebruikt door een bepaalde toepassing poging

Deze informatie wordt op uw HDInsight-clusters, Azure Resource Manager worden opgeslagen. De informatie wordt opgeslagen in het archief van de geschiedenis in de standaardopslag voor het cluster. Deze algemene gegevens over voltooide toepassingen kan worden opgehaald via een REST-API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>YARN-toepassingen en Logboeken
YARN biedt ondersteuning voor meerdere programmeermodellen door het resourcebeheer van toepassingen plannen/bewaking ontkoppeling. YARN maakt gebruik van een algemene *ResourceManager* (DB), per worker-knooppunt *NodeManagers* (NMs), en per toepassing *ApplicationMasters* (AMs). Het uur per toepassing onderhandelt over de resources (CPU, geheugen, schijf-, netwerk) voor het uitvoeren van uw toepassing met de RM. De RM werkt met NMs verlenen deze resources, die zijn verleend als *containers*. Het uur is verantwoordelijk voor het bijhouden van de voortgang van de containers die zijn toegewezen door de RM. Een toepassing mogelijk veel containers, afhankelijk van de aard van de toepassing.

* Elke toepassing kan bestaan uit meerdere *toepassing pogingen*. 
* Containers zijn verleend aan een specifieke poging van een toepassing. 
* Een container biedt de context van een basiseenheid voor werk. 
* Werk dat wordt uitgevoerd binnen de context van een container wordt uitgevoerd op de één worker-knooppunt dat de container is toegewezen aan. 

Zie voor meer informatie, [Apache Hadoop YARN concepten][YARN-concepts].

Toepassingslogboeken (en de bijbehorende containerlogboeken), zijn essentieel tijdens de foutopsporing problematische Hadoop-toepassingen. YARN biedt een mooie raamwerk voor het verzamelen, verzamelen en opslaan van toepassingslogboeken met de [logboeken gegevensaggregatie] [ log-aggregation] functie. De functie Logboeken gegevensaggregatie kunt toegang tot toepassingslogboeken meer deterministische, omdat deze logboeken alle containers op een worker-knooppunt verzamelt en slaat deze op als een geaggregeerde logboekbestand per worker-knooppunt op het standaardbestandssysteem nadat een toepassing is voltooid. Uw toepassing kan gebruikmaken van honderden of duizenden containers, maar de logboeken voor alle containers die worden uitgevoerd op een knooppunt één werknemer worden samengevoegd in één bestand, wat resulteert in één bestand per worker-knooppunt wordt gebruikt door uw toepassing. Aggregatie van logboek is standaard ingeschakeld op HDInsight-clusters (versie 3.0 en hoger), en samengevoegde logboeken kunnen worden gevonden in de standaardcontainer van uw cluster op de volgende locatie:

    wasb:///app-logs/<user>/logs/<applicationId>

Op die locatie *gebruiker* is de naam van de gebruiker die de toepassing is gestart en *applicationId* is de unieke id van een toepassing, zoals toegewezen door de YARN-RM.

De samengevoegde logboeken zijn niet rechtstreeks kan worden gelezen, zoals ze zijn geschreven in een [TFile][T-file], [binaire indeling] [ binary-format] geïndexeerd door de container. YARN biedt CLI-hulpprogramma voor het dumpen van deze logboeken als tekst zonder opmaak voor toepassingen of containers van belang zijn. U kunt deze logboeken kunt weergeven, zoals tekst zonder opmaak door het uitvoeren van een van de volgende YARN rechtstreeks op de clusterknooppunten opdrachten (na het maken van een verbinding via RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager-interface
De gebruikersinterface van YARN ResourceManager wordt uitgevoerd op het hoofdknooppunt van het cluster, en is toegankelijk via de Azure portal-dashboard:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
2. Klik in het menu links op **Bladeren**, klikt u op **HDInsight-Clusters**, klikt u op een Windows-clusters die u toegang tot de YARN-toepassingslogboeken.
3. Klik op het bovenste menu **Dashboard**. U ziet een pagina geopend op een nieuwe browser tabblad met de naam **HDInsight Queryconsole**.
4. Van **HDInsight Queryconsole**, klikt u op **gebruikersinterface van Yarn**.

[YARN-timeline-server]:https://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
