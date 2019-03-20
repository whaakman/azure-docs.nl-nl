---
title: Bewaken van Hadoop-clusters in HDInsight met behulp van de Ambari-API - Azure
description: Gebruik de Apache Ambari APIs voor het maken, beheren en controleren van Hadoop-clusters. API's en hulpprogramma's voor intuïtieve operators verbergen de complexiteit van Hadoop.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/07/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: ff6601042c82cef2b0101833117f17aca8b463dc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58223254"
---
# <a name="monitor-apache-hadoop-clusters-in-hdinsight-using-the-apache-ambari-api"></a>Controleren van Apache Hadoop-clusters in HDInsight met Apache Ambari-API
Leer hoe u HDInsight-clusters controleren met behulp van Apache Ambari APIs.

> [!NOTE]  
> De informatie in dit artikel is voornamelijk bedoeld voor HDInsight op basis van een Windows-clusters, waardoor een alleen-lezen versie van de Ambari REST-API. Zie voor Linux gebaseerde clusters [beheren Apache Hadoop-clusters met behulp van Apache Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Wat is Ambari?
[Apache Ambari] [ ambari-home] wordt gebruikt voor het inrichten, beheren en controleren van Apache Hadoop-clusters. Het bevat een intuïtieve verzameling hulpprogramma‘s voor operators en een krachtige reeks API‘s die de complexiteit van Hadoop verbergen en de werking van clusters vereenvoudigen. Zie voor meer informatie over de API's, [Ambari API Reference][ambari-api-reference]. 

HDInsight ondersteunt momenteel alleen de Ambari-monitorfunctie. Ambari API 1.0 wordt ondersteund door HDInsight versie 3.0 en 2.1-clusters. In dit artikel bevat informatie over toegang tot Ambari APIs HDInsight versie 3.1 en 2.1-clusters. Het belangrijkste verschil tussen de twee is dat sommige van de onderdelen zijn gewijzigd met de introductie van nieuwe mogelijkheden (zoals de taakserver geschiedenis). 

**Vereisten**

Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een werkstation met Azure PowerShell**.
* (Optioneel) [cURL][curl]. Om het te installeren, Zie [cURL Releases en Downloads][curl-download].
  
  > [!NOTE]  
  > Als de cURL-opdracht gebruiken in Windows, gebruik dubbele aanhalingstekens in plaats van enkele aanhalingstekens voor de optiewaarden.

* **Een Azure HDInsight-cluster**. Zie voor instructies over het inrichten van het cluster [aan de slag met HDInsight] [ hdinsight-get-started] of [inrichten HDInsight-clusters][hdinsight-provision]. U moet de volgende gegevens op de zelfstudie te volgen:
  
  | Cluster-eigenschap | Naam van Azure PowerShell-variabele | Value | Description |
  | --- | --- | --- | --- |
  |   De naam van de HDInsight-cluster |$clusterName | |De naam van uw HDInsight-cluster. |
  |   Gebruikersnaam voor cluster |$clusterUsername | |Clusternaam van de gebruiker opgegeven wanneer het cluster is gemaakt. |
  |   Het wachtwoord van cluster |$clusterPassword | |Gebruikerswachtwoord van het cluster. |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a>Snel aan de slag
Er zijn verschillende manieren om te Ambari gebruiken om het HDInsight-clusters controleren.

**Azure PowerShell gebruiken**

De volgende Azure PowerShell-script wordt de informatie van MapReduce-taak tracker *in een HDInsight 3.5-cluster.*  Het belangrijkste verschil is dat we deze details van de YARN-service (plaats van MapReduce) ophalen.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

De volgende PowerShell-script wordt de informatie van MapReduce-taak tracker *in een cluster met HDInsight 2.1*:

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

Het volgende voorbeeld wordt informatie van de cluster met behulp van cURL:

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

Bij het gebruik van het eindpunt van de Ambari 'https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}', de *hostnaam* veld retourneert de volledig gekwalificeerde domeinnaam (FQDN) van het knooppunt in plaats van de hostnaam. Vóór de release van 8-10-2014, in dit voorbeeld geretourneerd gewoon '**headnode0**'. Na de 8-10-2014-release, krijgt u de FQDN-naam '**headnode0. { ClusterDNS} azurehdinsight.NET .net**', zoals wordt weergegeven in het vorige voorbeeld. Deze wijziging is vereist om scenario's waarbij meerdere clustertypen (zoals HBase en Hadoop) kunnen worden geïmplementeerd in een virtueel netwerk (VNET) mogelijk te maken. Dit gebeurt, bijvoorbeeld, als u HBase als een back-endplatform voor Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari API's bewaken
De volgende tabel bevat enkele van de meest voorkomende Ambari controle van het API-aanroepen. Zie voor meer informatie over de API [Apache Ambari API Reference][ambari-api-reference].

| Monitor, API-aanroep | URI | Description |
| --- | --- | --- |
| Clusters ophalen |`/api/v1/clusters` | |
| Cluster ophalen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |clusters, services, hosts |
| Profiteer van services |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |Services omvatten: hdfs, mapreduce |
| Services ophalen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| Service-onderdelen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker |
| Onderdeel ophalen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |ServiceComponentInfo, host-onderdelen, metrische gegevens |
| Hosts ophalen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |headnode0, workernode0 |
| Ophalen van de hostgegevens. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| Host-onderdelen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |namenode, resourcemanager |
| Host onderdeel gegevens opvragen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |HostRoles, component, host, metrische gegevens |
| Configuraties ophalen |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |Typen config: core-site, hdfs-site, mapred-site, hive-site |
| Configuratie-informatie ophalen. |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |Typen config: core-site, hdfs-site, mapred-site, hive-site |

## <a name="next-steps"></a>Volgende stappen
Nu hebt u geleerd hoe u bewaking Apache Ambari-API-aanroepen. Voor meer informatie zie:

* [Apache Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md)
* [HDInsight-clusters met behulp van Azure PowerShell beheren][hdinsight-admin-powershell]
* [HDInsight-clusters met behulp van de opdrachtregelinterface beheren][hdinsight-admin-cli]
* [Documentatie voor HDInsight][hdinsight-documentation]
* [Aan de slag met HDInsight][hdinsight-get-started]

[ambari-home]: https://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: https://curl.haxx.se
[curl-download]: https://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: https://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
