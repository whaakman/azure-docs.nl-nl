---
title: Hadoop-clusters in HDInsight met behulp van de Ambari-API - Azure bewaken | Microsoft Docs
description: "Gebruik de Apache Ambari APIs voor het maken, beheren en controleren van Hadoop-clusters. API's en hulpprogramma's voor intuïtieve operators verbergen de complexiteit van Hadoop"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
editor: cgronlun
manager: jhubbard
ms.assetid: 052135b3-d497-4acc-92ff-71cee49356ff
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 57b63490037760c9150fd4b8fdb66267739f20ae
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Controleer Hadoop-clusters in HDInsight met de Ambari-API
Informatie over het HDInsight-clusters bewaken met behulp van Ambari APIs.

> [!NOTE]
> De informatie in dit artikel is hoofdzakelijk voor HDInsight op basis van Windows-clusters, die geen alleen-lezen-versie van de Ambari REST-API. Zie voor Linux gebaseerde clusters [beheren Hadoop-clusters met Ambari](hdinsight-hadoop-manage-ambari.md).
> 
> 

## <a name="what-is-ambari"></a>Wat is Ambari?
[Apache Ambari] [ ambari-home] wordt gebruikt voor het inrichten, beheren en controleren van Apache Hadoop-clusters. Het bevat een intuïtieve verzameling hulpprogramma‘s voor operators en een krachtige reeks API‘s die de complexiteit van Hadoop verbergen en de werking van clusters vereenvoudigen. Zie voor meer informatie over de API [Ambari-API-referentiemateriaal][ambari-api-reference]. 

HDInsight ondersteunt momenteel alleen de Ambari-monitorfunctie. Ambari-API 1.0 wordt ondersteund door clusters van HDInsight versie 3.0 en 2.1. In dit artikel bevat informatie over toegang tot Ambari APIs HDInsight versie 3.1 of 2.1-clusters. Het belangrijkste verschil tussen de twee is dat sommige onderdelen hebt gewijzigd door de introductie van nieuwe mogelijkheden (zoals de taakserver geschiedenis). 

**Vereisten**

Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een werkstation met Azure PowerShell**.
* (Optioneel) [cURL][curl]. Om het te installeren, Zie [cURL Releases en Downloads][curl-download].
  
  > [!NOTE]
  > Wanneer gebruikt u de cURL-opdracht in Windows, gebruik dubbele aanhalingstekens in plaats van enkele aanhalingstekens voor de optiewaarden.
  > 
  > 
* **Een Azure HDInsight-cluster**. Zie voor instructies over het cluster inrichten [aan de slag met HDInsight] [ hdinsight-get-started] of [HDInsight-clusters inrichten][hdinsight-provision]. De volgende gegevens om de zelfstudie hebt u nodig:
  
  | Cluster-eigenschap | Azure PowerShell de naam van variabele | Waarde | Beschrijving |
  | --- | --- | --- | --- |
  |   De naam van de HDInsight-cluster |$clusterName | |De naam van uw HDInsight-cluster. |
  |   Cluster-gebruikersnaam |$clusterUsername | |Clusternaam van de gebruiker opgegeven wanneer het cluster is gemaakt. |
  |   Het wachtwoord van cluster |$clusterPassword | |Wachtwoord van de gebruiker cluster. |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a>Snel
Er zijn verschillende manieren Ambari gebruiken om te bewaken van HDInsight-clusters.

**Azure PowerShell gebruiken**

De volgende Azure PowerShell-script wordt de informatie van MapReduce-taak tracering *in een 3.5 HDInsight-cluster.*  Het belangrijkste verschil is dat we deze details van de YARN-service (in plaats MapReduce) ophalen.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

De volgende PowerShell-script wordt de informatie van MapReduce-taak tracering *in een cluster HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

De uitvoer is:

![Jobtracker uitvoer][img-jobtracker-output]

**cURL gebruiken**

Het volgende voorbeeld wordt clustergegevens opgehaald met behulp van cURL:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

De uitvoer is:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Voor de 8-10-2014-release**:

Wanneer u het eindpunt Ambari 'https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}', de *hostnaam* veld retourneert de volledig gekwalificeerde domeinnaam (FQDN) van het knooppunt in plaats van de hostnaam. Vóór de 8-10-2014-release geretourneerd in dit voorbeeld gewoon '**headnode0**'. Na de 8-10-2014-release, krijgt u de FQDN-naam '**headnode0. { ClusterDNS} azurehdinsight.NET .net**', zoals wordt weergegeven in het vorige voorbeeld. Deze wijziging is vereist voor scenario's waarbij meerdere clustertypen (zoals HBase en Hadoop) kunnen worden geïmplementeerd in een virtueel netwerk (VNET) vergemakkelijken. Dit gebeurt, bijvoorbeeld: bij gebruik van HBase als een back-end-platform voor Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari-API's bewaken
De volgende tabel vindt u enkele van de meest voorkomende Ambari-API-aanroepen bewaking. Zie voor meer informatie over de API [Ambari-API-referentiemateriaal][ambari-api-reference].

| Monitor voor API-aanroep | URI | Beschrijving |
| --- | --- | --- |
| Ophalen van clusters |`/api/v1/clusters` | |
| Cluster-gegevens opvragen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |clusters, services, hosts |
| Ophalen van services |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |Services bevatten: hdfs, mapreduce |
| Services-gegevens opvragen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| Serviceonderdelen downloaden |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker |
| Onderdeel gegevens opvragen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |ServiceComponentInfo, host-onderdelen, metrische gegevens |
| Hosts ophalen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |headnode0, workernode0 |
| Hostgegevens niet ophalen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| Host-onderdelen downloaden |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |namenode, resourcemanager |
| Host onderdeel gegevens opvragen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |HostRoles, component, host, metrische gegevens |
| Configuraties ophalen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |Typen config: core-site, hdfs-site, mapred-site, hive-site |
| Configuratie-informatie ophalen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |Typen config: core-site, hdfs-site, mapred-site, hive-site |

## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe u de Ambari-API-aanroepen bewaking. Voor meer informatie zie:

* [HDInsight-clusters met de Azure portal beheren][hdinsight-admin-portal]
* [HDInsight-clusters met Azure PowerShell beheren][hdinsight-admin-powershell]
* [HDInsight-clusters via opdrachtregelinterface beheren][hdinsight-admin-cli]
* [HDInsight-documentatie][hdinsight-documentation]
* [Aan de slag met HDInsight][hdinsight-get-started]

[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
