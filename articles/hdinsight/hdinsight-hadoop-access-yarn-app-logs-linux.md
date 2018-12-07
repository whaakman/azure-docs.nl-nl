---
title: Toegang tot Apache Hadoop YARN-toepassingslogboeken op Linux gebaseerde HDInsight - Azure
description: Leer hoe u toegang tot YARN-toepassingslogboeken op een Linux gebaseerde HDInsight (Apache Hadoop)-cluster met behulp van de opdrachtregel en een webbrowser.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: hrasheed
ms.openlocfilehash: e6f778016f4f465cd438b74dff95cb1b37c42d79
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015667"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-linux-based-hdinsight"></a>Toegang tot Apache Hadoop YARN-toepassingslogboeken op Linux gebaseerde HDInsight

Leer hoe u toegang tot de logboeken voor [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) (nog een andere Resource Negotiator)-toepassingen op een [Apache Hadoop](https://hadoop.apache.org/) -cluster in Azure HDInsight.

> [!IMPORTANT]
> Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem gebruikt op HDInsight versie 3.6 of hoger. Zie voor meer informatie, [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="YARNTimelineServer"></a>YARN Timeline Server

De [Apache Hadoop YARN Timeline Server](http://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) biedt algemene informatie over voltooide toepassingen

YARN Timeline Server omvat het volgende type gegevens:

* De toepassings-ID, een unieke id van een toepassing
* De gebruiker die de toepassing is gestart
* Meer informatie over een poging om uit te voeren van de toepassing
* De containers die worden gebruikt door een bepaalde toepassing poging

## <a name="YARNAppsAndLogs"></a>YARN-toepassingen en Logboeken

YARN biedt ondersteuning voor meerdere programmeermodellen ([Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) wordt een van beide) door het resourcebeheer van toepassingen plannen/bewaking ontkoppeling. YARN maakt gebruik van een algemene *ResourceManager* (DB), per worker-knooppunt *NodeManagers* (NMs), en per toepassing *ApplicationMasters* (AMs). Het uur per toepassing onderhandelt over de resources (CPU, geheugen, schijf-, netwerk) voor het uitvoeren van uw toepassing met de RM. De RM werkt met NMs verlenen deze resources, die zijn verleend als *containers*. Het uur is verantwoordelijk voor het bijhouden van de voortgang van de containers die zijn toegewezen door de RM. Een toepassing mogelijk veel containers, afhankelijk van de aard van de toepassing.

Elke toepassing kan bestaan uit meerdere *toepassing pogingen*. Als een toepassing mislukt, wordt deze opnieuw uitgevoerd als een nieuwe poging. Elke poging wordt uitgevoerd in een container. In zekere zin biedt een container in de context voor de basiseenheid voor werk dat door een YARN-toepassing uitgevoerd. Al het werk dat wordt uitgevoerd binnen de context van een container wordt uitgevoerd op de één worker-knooppunt waarop de container is toegewezen. Zie [Apache Hadoop YARN concepten] [ YARN-concepts] voor verdere verwijzing.

Toepassingslogboeken (en de bijbehorende containerlogboeken), zijn essentieel tijdens de foutopsporing problematische Hadoop-toepassingen. YARN biedt een mooie raamwerk voor het verzamelen, verzamelen en opslaan van toepassingslogboeken met de [logboeken gegevensaggregatie] [ log-aggregation] functie. De functie Logboeken gegevensaggregatie kunt de toegang tot toepassingslogboeken meer deterministische. Deze logboeken verzamelt alle containers op een worker-knooppunt en slaat deze op als een geaggregeerde logboekbestand per worker-knooppunt. Het logboek wordt opgeslagen op het standaardbestandssysteem nadat een toepassing is voltooid. Uw toepassing kan gebruikmaken van honderden of duizenden containers, maar de logboeken voor alle containers die worden uitgevoerd op een knooppunt één werknemer altijd worden samengevoegd in één bestand. Er is dus alleen 1 logboek per worker-knooppunt wordt gebruikt door uw toepassing. Aggregatie van logboek is standaard op HDInsight-clusters versie 3.0 en hoger ingeschakeld. Samengevoegde logboeken bevinden zich in de standaardopslag voor het cluster. Het volgende pad, is het HDFS-pad naar de logboeken:

    /app-logs/<user>/logs/<applicationId>

In het pad `user` is de naam van de gebruiker die de toepassing is gestart. De `applicationId` is de unieke id die is toegewezen aan een toepassing door de YARN-RM.

De samengevoegde logboeken zijn niet rechtstreeks kan worden gelezen, zoals ze zijn geschreven in een [TFile][T-file], [binaire indeling] [ binary-format] geïndexeerd door de container. De ResourceManager YARN-Logboeken of de CLI-hulpprogramma's gebruiken om deze logboeken als tekst zonder opmaak voor toepassingen of containers van belang weer te geven.

## <a name="yarn-cli-tools"></a>YARN CLI-hulpprogramma 's

Voor het gebruik van de YARN CLI-hulpprogramma's, moet u eerst verbinding met het HDInsight-cluster via SSH. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

U kunt deze logboeken als tekst zonder opmaak weergeven door het uitvoeren van een van de volgende opdrachten:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Geef de &lt;applicationId >, &lt;-die-aan de slag-de-gebruikerstoepassing >, &lt;containerId >, en &lt;worker-knooppunt-adres > informatie bij het uitvoeren van deze opdrachten.

## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager-interface

De gebruikersinterface van YARN ResourceManager op het hoofdknooppunt van het cluster wordt uitgevoerd. Deze toegankelijk is via de Ambari-Webgebruikersinterface. Gebruik de volgende stappen uit om de YARN-logboeken weer te geven:

1. Navigeer in uw webbrowser naar https://CLUSTERNAME.azurehdinsight.net. CLUSTERNAME vervangen door de naam van uw HDInsight-cluster.
2. Selecteer in de lijst met services aan de linkerkant **YARN**.

    ![Yarn-service die zijn geselecteerd](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Uit de **snelkoppelingen** vervolgkeuzelijst, selecteer een van de hoofdknooppunten van het cluster en selecteer vervolgens **ResourceManager Log**.

    ![Yarn-snelkoppelingen](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    Krijgt u een lijst met koppelingen naar YARN-Logboeken.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
