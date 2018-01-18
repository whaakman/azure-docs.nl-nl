---
title: Toegang tot Hadoop YARN toepassingslogboeken op Linux gebaseerde HDInsight - Azure | Microsoft Docs
description: Meer informatie over het openen van YARN-logboeken op een HDInsight (Hadoop) op basis van Linux-cluster met behulp van de opdrachtregel en een webbrowser.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: larryfr
ms.openlocfilehash: 5f61e94d37a5e5f958a706f0db82526996a4ec02
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Toegang toepassingslogboeken YARN op Linux gebaseerde HDInsight

Meer informatie over het openen van de logboeken voor toepassingen op een Hadoop-cluster in Azure HDInsight YARN (nog een andere Resource onderhandelaar).

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="YARNTimelineServer"></a>YARN tijdlijn Server

De [YARN tijdlijn Server](http://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) bevat algemene informatie over het voltooide toepassingen en framework-specifieke toepassingsinformatie via twee verschillende interfaces. Specifiek:

* Opslaan en ophalen van informatie over algemene toepassing op HDInsight-clusters is ingeschakeld met versie 3.1.1.374 of hoger.
* Een onderdeel van de application framework-specifieke gegevens van de tijdlijn-Server is momenteel niet beschikbaar op HDInsight-clusters.

Algemene informatie over toepassingen omvat het volgende type gegevens:

* De toepassings-ID, een unieke id van een toepassing
* De gebruiker die de toepassing is gestart
* Informatie over pogingen gedaan om te voltooien van de toepassing
* De containers die worden gebruikt door een bepaalde toepassing poging

## <a name="YARNAppsAndLogs"></a>Logboeken en YARN-toepassingen

YARN biedt ondersteuning voor meerdere programmeermodellen (MapReduce wordt een van beide) door ontkoppeling bronbeheer van toepassing planning/bewaking. YARN maakt gebruik van een globale *ResourceManager* (RM) per worker-knooppunten *NodeManagers* (NMs) en per toepassing *ApplicationMasters* (AMs). De AM per toepassing onderhandelt resources (CPU, geheugen, schijf-, netwerk) over het uitvoeren van uw toepassing met de RM. De RM werkt met NMs verlenen deze resources die worden verleend als *containers*. De AM is verantwoordelijk voor het volgen van de voortgang van de containers die zijn toegewezen door de RM. Een toepassing moet mogelijk veel containers afhankelijk van de aard van de toepassing.

Elke toepassing kan bestaan uit meerdere *toepassing pogingen*. Als een toepassing mislukt, wordt deze opnieuw uitgevoerd als een nieuwe poging. Elke poging wordt uitgevoerd in een container. In een zin biedt een container in de context voor basic-eenheid van het werk dat door een YARN-toepassing uitgevoerd. Al het werk dat wordt uitgevoerd in de context van een container wordt uitgevoerd op het één werkrolknooppunt waarop de container is toegewezen. Zie [YARN concepten] [ YARN-concepts] voor verdere verwijzing.

Toepassingslogboeken (en de logboeken van de bijbehorende container) zijn essentieel bij het opsporen van fouten problematisch Hadoop-toepassingen. YARN biedt een nice framework voor het verzamelen, aggregeren en opslaan van de toepassingslogboeken van de met de [logboek aggregatie] [ log-aggregation] functie. De aggregatie van logboek-functie kunt de toegang tot toepassingslogboeken meer deterministische. Deze logboeken hierbij op alle containers op een knooppunt van de werknemer en worden opgeslagen als een samengevoegde logboekbestand per werkrolknooppunt. Het logboek is opgeslagen op het standaardbestandssysteem nadat een toepassing is voltooid. Toepassingen kan gebruikmaken van honderden of duizenden containers, maar de logboeken voor alle containers worden uitgevoerd op een enkele werkrolknooppunt altijd worden samengevoegd in één bestand. Er is daarom alleen 1 logboek per werkrolknooppunt gebruikt door de toepassing. Aggregatie van logboek is standaard op HDInsight-clusters versie 3.0 en hoger. Samengevoegde logboeken bevinden zich in de standaard opslagruimte voor het cluster. Het volgende pad is de HDFS-pad naar de logboeken:

    /app-logs/<user>/logs/<applicationId>

In het pad `user` is de naam van de gebruiker die de toepassing is gestart. De `applicationId` is de unieke id die is toegewezen aan een toepassing door de YARN-RM.

De geaggregeerde logboeken zijn niet rechtstreeks kan worden gelezen, zoals ze zijn geschreven in een [TFile][T-file], [binaire indeling] [ binary-format] geïndexeerd door de container. Gebruik de ResourceManager YARN-Logboeken of de CLI-hulpprogramma's om weer te geven deze logboeken als tekst zonder opmaak voor toepassingen of containers van belang.

## <a name="yarn-cli-tools"></a>YARN CLI-hulpprogramma 's

Als u wilt de YARN CLI-hulpprogramma's gebruiken, moet u eerst verbinding met het HDInsight-cluster via SSH. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

U kunt deze logboeken als tekst zonder opmaak bekijken door een van de volgende opdrachten uit te voeren:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Geef de &lt;applicationId >, &lt;-die-slag-de-gebruikerstoepassing >, &lt;containerId >, en &lt;worker-knooppunt-adres > informatie wanneer deze opdrachten uit te voeren.

## <a name="yarn-resourcemanager-ui"></a>Gebruikersinterface van YARN ResourceManager

De gebruikersinterface van YARN ResourceManager op het cluster headnode wordt uitgevoerd. Deze is toegankelijk via de Ambari-webgebruikersinterface. Gebruik de volgende stappen uit om de YARN-logboeken weer te geven:

1. Ga naar https://CLUSTERNAME.azurehdinsight.net in uw webbrowser. CLUSTERNAAM vervangen door de naam van uw HDInsight-cluster.
2. Selecteer in de lijst van services aan de linkerkant **YARN**.

    ![Yarn service geselecteerd](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Van de **snelkoppelingen** vervolgkeuzelijst, selecteer een van de hoofdknooppunten van het cluster en selecteer vervolgens **ResourceManager logboek**.

    ![Yarn snelle koppelingen](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    Krijgt u een lijst met koppelingen naar YARN-Logboeken.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
