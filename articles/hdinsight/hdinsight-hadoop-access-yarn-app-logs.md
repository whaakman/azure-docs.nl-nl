---
title: Toegang tot Hadoop YARN toepassingslogboeken programmatisch - Azure | Microsoft Docs
description: De toegangstoepassing aanmeldt via een programma een Hadoop-cluster in HDInsight.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0198d6c9-7767-4682-bd34-42838cf48fc5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 90323af4a1f4526ab9b26811c8679337076112d1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Toegang toepassingslogboeken YARN op HDInsight op basis van Windows
In dit onderwerp wordt uitgelegd hoe u toegang tot de logboeken voor YARN (nog een andere Resource onderhandelaar)-toepassingen die u opgegeven op een Windows gebaseerde Hadoop-cluster in Azure HDInsight hebt

> [!IMPORTANT]
> De informatie in dit document geldt alleen voor Windows gebaseerde HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor informatie over YARN toegang tot op Linux gebaseerde HDInsight-clusters Logboeken, [toepassingslogboeken op Linux gebaseerde Hadoop op HDInsight YARN toegang](hdinsight-hadoop-access-yarn-app-logs-linux.md)
>


### <a name="prerequisites"></a>Vereisten
* Een Windows gebaseerde HDInsight-cluster.  Zie [Windows maken op basis van een Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN tijdlijn Server
De <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN tijdlijn Server</a> bevat algemene informatie over het voltooide toepassingen ook als framework-specifieke toepassingsinformatie via twee verschillende interfaces. Specifiek:

* Opslaan en ophalen van informatie over algemene toepassing op HDInsight-clusters is ingeschakeld met versie 3.1.1.374 of hoger.
* Een onderdeel van de application framework-specifieke gegevens van de tijdlijn-Server is momenteel niet beschikbaar op HDInsight-clusters.

Algemene informatie over toepassingen bevat de volgende soorten gegevens:

* De toepassings-ID, een unieke id van een toepassing
* De gebruiker die de toepassing is gestart
* Informatie over pogingen gedaan om te voltooien van de toepassing
* De containers die worden gebruikt door een bepaalde toepassing poging

Op uw HDInsight-clusters wordt deze informatie opgeslagen door Azure Resource Manager naar een winkel geschiedenis in de standaardcontainer van uw Azure Storage-standaardaccount. Deze algemene gegevens voor voltooide toepassingen kan worden opgehaald via een REST-API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Logboeken en YARN-toepassingen
YARN biedt ondersteuning voor meerdere programmeermodellen (MapReduce wordt een van beide) door ontkoppeling bronbeheer van toepassing planning/bewaking. Dit wordt gedaan door een globale *ResourceManager* (RM) per worker-knooppunten *NodeManagers* (NMs) en per toepassing *ApplicationMasters* (AMs). De AM per toepassing onderhandelt resources (CPU, geheugen, schijf-, netwerk) over het uitvoeren van uw toepassing met de RM. De RM werkt met NMs verlenen deze resources die worden verleend als *containers*. De AM is verantwoordelijk voor het volgen van de voortgang van de containers die zijn toegewezen door de RM. Een toepassing moet mogelijk veel containers afhankelijk van de aard van de toepassing.

Bovendien elke toepassing kan bestaan uit meerdere *toepassing pogingen* om te kunnen voltooien met crashes of als gevolg van het verlies van communicatie tussen een uur en een RM. Daarom worden containers verleend aan een specifieke poging van een toepassing. In een zin een container biedt de context voor basic-eenheid van uitgevoerd door een toepassing YARN werk en al het werk dat wordt uitgevoerd in de context van een container is uitgevoerd op het één werkrolknooppunt waarop de container is toegewezen. Zie [YARN concepten] [ YARN-concepts] voor verdere verwijzing.

Toepassingslogboeken (en de logboeken van de bijbehorende container) zijn essentieel bij het opsporen van fouten problematisch Hadoop-toepassingen. YARN biedt een nice framework voor het verzamelen, aggregeren en opslaan van de toepassingslogboeken van de met de [logboek aggregatie] [ log-aggregation] functie. De functie logboek aggregatie kunnen toegang tot toepassingslogboeken meer deterministische naar Logboeken hierbij op alle containers op een knooppunt van de werknemer en worden opgeslagen als een samengevoegde logboekbestand per knooppunt van de werknemer op het standaardbestandssysteem nadat een toepassing is voltooid. Toepassingen kan gebruikmaken van honderden of duizenden containers, maar de logboeken voor alle containers worden uitgevoerd op een enkele werkrolknooppunt altijd worden samengevoegd in één bestand, wat resulteert in één logboek per werkrolknooppunt gebruikt door de toepassing. Aggregatie van logboek is standaard ingeschakeld op HDInsight-clusters (versie 3.0 en hoger), en geaggregeerde logboeken kunnen worden gevonden in de standaardcontainer van uw cluster op de volgende locatie:

    wasb:///app-logs/<user>/logs/<applicationId>

Op die locatie *gebruiker* is de naam van de gebruiker die de toepassing is gestart en *applicationId* is de unieke id van een toepassing die is toegewezen door de YARN-RM.

De geaggregeerde logboeken zijn niet rechtstreeks kan worden gelezen, zoals ze zijn geschreven in een [TFile][T-file], [binaire indeling] [ binary-format] geïndexeerd door de container. YARN biedt CLI-hulpprogramma's voor deze logboeken dump als tekst zonder opmaak voor toepassingen of containers van belang. U kunt deze logboeken bekijken als tekst zonder opmaak door het uitvoeren van een van de volgende YARN rechtstreeks op de clusterknooppunten (na verbinding mee te maken via RDP)-opdrachten:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>Gebruikersinterface van YARN ResourceManager
De gebruikersinterface van YARN ResourceManager wordt uitgevoerd op de headnode cluster en toegankelijk zijn via de Azure-portaldashboard:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
2. Klik in het menu links op **Bladeren**, klikt u op **HDInsight-Clusters**, klikt u op een cluster op basis van Windows die u wilt toegang krijgen tot de YARN-Logboeken.
3. Klik op het bovenste menu **Dashboard**. Ziet u een pagina in een nieuwe browser geopend tabblad aangeroepen **HDInsight Query Console**.
4. Van **HDInsight Query Console**, klikt u op **gebruikersinterface van Yarn**.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
