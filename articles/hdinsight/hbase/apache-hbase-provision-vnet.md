---
title: HBase-clusters maken in een virtueel netwerk - Azure | Microsoft Docs
description: Aan de slag met HBase in Azure HDInsight. Informatie over hoe HDInsight HBase-clusters maken op Azure Virtual Network.
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/03/2017
ms.author: jgao
ms.openlocfilehash: 1d3dba645acf51a7dcdd42fa23c82db962244b62
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>HBase-clusters maken in HDInsight in Azure Virtual Network
Informatie over het maken van Azure HDInsight HBase-clusters in een [Azure Virtual Network][1].

Met integratie van virtueel netwerk, kunnen HBase-clusters worden geïmplementeerd op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen rechtstreeks met HBase communiceren kunnen. De voordelen zijn:

* Directe verbinding van de webtoepassing aan de knooppunten van het HBase-cluster, waarmee communicatie via HBase Java externe procedureaanroepen (RPC) API's aanroepen.
* Verbeterde prestaties omdat u niet hoeft uw verkeer Ga via meerdere gateways en load balancers.
* De mogelijkheid om gevoelige informatie op een veiliger manier verwerken zonder een openbaar eindpunt bloot te stellen.

### <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een werkstation met Azure PowerShell**. Zie [installeren en gebruiken Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).

## <a name="create-hbase-cluster-into-virtual-network"></a>HBase-cluster in virtueel netwerk maken
In deze sectie maakt u een Linux-gebaseerde HBase-cluster maken met het afhankelijke Azure Storage-account aan een virtuele Azure-netwerk met een [Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-group-template-deploy.md). Voor andere methoden voor het maken van cluster en kennis van de instellingen, Zie [HDInsight-clusters maken](../hdinsight-hadoop-provision-linux-clusters.md). Zie voor meer informatie over het gebruik van een sjabloon voor het maken van Hadoop-clusters in HDInsight [maken Hadoop-clusters in HDInsight met behulp van Azure Resource Manager-sjablonen](../hdinsight-hadoop-create-linux-clusters-arm-templates.md)

> [!NOTE]
> Sommige eigenschappen zijn hardgecodeerd in de sjabloon. Bijvoorbeeld:
>
> * **Locatie**: VS-Oost 2
> * **Cluster versie**: 3.6
> * **Aantal worker-knooppunten cluster**: 2
> * **Storage-account standaard**: een unieke tekenreeks
> * **Virtuele-netwerknaam**: &lt;Clusternaam >-vnet
> * **Adresruimte voor virtueel netwerk**: 10.0.0.0/16
> * **De subnetnaam van het**: subnet1
> * **Adresbereik van**: 10.0.0.0/24
>
> &lt;Clusternaam > is vervangen door de clusternaam die u opgeeft wanneer u de sjabloon.
>
>

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen. De sjabloon bevindt zich in [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Van de **aangepaste implementatie** blade, voer de volgende eigenschappen:

   * **Abonnement**: Selecteer een Azure-abonnement gebruikt voor het maken van het HDInsight-cluster, het afhankelijke opslagaccount en de virtuele Azure-netwerk.
   * **Resourcegroep**: Selecteer **nieuw**, en geef een nieuwe Resourcegroepnaam.
   * **Locatie**: selecteer een locatie voor de resourcegroep.
   * **Clusternaam**: Voer een naam voor het Hadoop-cluster moet worden gemaakt.
   * **Aanmeldgegevens voor het cluster**: de standaardaanmeldnaam is **admin**.
   * **SSH-gebruikersnaam en -wachtwoord**: de standaardgebruikersnaam is **sshuser**.  U kunt de naam wijzigen.
   * **Ik ga akkoord met de voorwaarden de hierboven vermelde**: (selecteren)
3. Klik op **Kopen**. Het duurt ongeveer 20 minuten om een cluster te maken. Zodra het cluster is gemaakt, kunt u de cluster-blade in de portal om deze te openen.

Nadat u de zelfstudie hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt. Zie voor de instructies van het verwijderen van een cluster [beheren Hadoop-clusters in HDInsight met behulp van de Azure-portal](../hdinsight-administer-use-management-portal.md#delete-clusters).

Als u wilt werken met uw nieuwe HBase-cluster, kunt u de procedures die zijn gevonden in [aan de slag met HBase met Hadoop in HDInsight](./apache-hbase-tutorial-get-started-linux.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Verbinding maken met de HBase-cluster met HBase Java RPC-API 's
1. Maak een infrastructuur als een dienst (IaaS) virtuele machine in hetzelfde virtuele netwerk van Azure en hetzelfde subnet. Zie voor instructies over het maken van een nieuwe virtuele machine voor IaaS [maken van een virtuele Machine Running Windows Server](../../virtual-machines/windows/quick-create-portal.md). Wanneer u de stappen in dit document, moet u de volgende waarden gebruiken voor de netwerkconfiguratie:

   * **Virtueel netwerk**: &lt;Clusternaam >-vnet
   * **Subnet**: subnet1

   > [!IMPORTANT]
   > Vervang &lt;Clusternaam > met de naam die u hebt gebruikt bij het maken van het HDInsight-cluster in de vorige stappen.
   >
   >

   De virtuele machine wordt met behulp van deze waarden geplaatst in het hetzelfde virtuele netwerk en subnet als het HDInsight-cluster. Deze configuratie kan ze rechtstreeks met elkaar communiceren. Er is een manier om een HDInsight-cluster maken met een leeg edge-knooppunt. Het edge-knooppunt kan worden gebruikt voor het beheren van het cluster.  Zie voor meer informatie [leeg edge-knooppunten gebruiken in HDInsight](../hdinsight-apps-use-edge-node.md).

2. Wanneer u een Java-toepassing op afstand verbinding maken met HBase, moet u de volledig gekwalificeerde domeinnaam (FQDN). Als u wilt bepalen, moet u het verbindingsspecifieke DNS-achtervoegsel van de HBase-cluster ophalen. Hiervoor kunt u een van de volgende methoden gebruiken:

   * Een webbrowser gebruiken om een oproep Ambari te doen:

     Blader naar https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / als host fungeert voor? minimal_response = true. Hiermee schakelt u een JSON-bestand met de DNS-achtervoegsels.
   * Gebruik de Ambari-website:

     1. Blader naar https://&lt;ClusterName >. azurehdinsight.net.
     2. Klik op **Hosts** in het menu bovenaan.
   * Gebruik Curl REST-aanroepen:

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     In de notatie JSON (JavaScript Object)-gegevens geretourneerd, de vermelding 'hostnaam' niet vinden. Het bevat de FQDN-naam voor de knooppunten in het cluster. Bijvoorbeeld:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     Het gedeelte van het domein naam die begint met de naam van het cluster is het DNS-achtervoegsel. Bijvoorbeeld: mycluster.b1.cloudapp.net.
   * Azure PowerShell gebruiken

     De volgende Azure PowerShell-script gebruiken om te registreren de **Get-ClusterDetail** functie, die kan worden gebruikt voor het retourneren van de DNS-achtervoegsel:

    ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
            .Description
            This command shows the following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows the value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run the HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows the FQDN suffix of hosts in the cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows the value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run the HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows the FQDN suffix of hosts in the cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
    ```

     Nadat de Azure PowerShell-script is uitgevoerd, kunt u de volgende opdracht gebruiken om te retourneren van de DNS-achtervoegsel met behulp van de **Get-ClusterDetail** functie. Geef uw naam van HDInsight HBase-cluster, naam van de serverbeheerder en beheerderswachtwoord wanneer u deze opdracht.

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     Deze opdracht retourneert de DNS-achtervoegsel. Bijvoorbeeld: **yourclustername.b4.internal.cloudapp.net**.


<!--
3.    Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix](./media/apache-hbase-provision-vnet/PrimaryDNSSuffix.png)
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Gebruik de opdracht om te controleren of de virtuele machine met de HBase-cluster communiceren kan, `ping headnode0.<dns suffix>` van de virtuele machine. Bijvoorbeeld: ping headnode0.mycluster.b1.cloudapp.net.

Als u deze informatie in een Java-toepassing, kunt u de stappen in [Maven gebruiken voor het ontwikkelen van Java-toepassingen die gebruikmaken van HBase met HDInsight (Hadoop)](./apache-hbase-build-java-maven-linux.md) een toepassing maken. Als u de toepassing verbinding maken met een externe HBase-server wilt weergeven, wijzigen de **hbase-site.xml** bestand in dit voorbeeld met de FQDN-naam voor Zookeeper. Bijvoorbeeld:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> Zie voor meer informatie over naamomzetting in Azure virtuele netwerken, inclusief het gebruik van uw eigen DNS-server, [Name Resolution (DNS)](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
>
>

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe een HBase-cluster maken. Voor meer informatie zie:

* [Aan de slag met HDInsight](../hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Lege edge-knooppunten in HDInsight gebruiken](../hdinsight-apps-use-edge-node.md)
* [HBase-replicatie in HDInsight configureren](apache-hbase-replication.md)
* [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Aan de slag met HBase met Hadoop in HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Overzicht van Virtual Network](../../virtual-network/virtual-networks-overview.md)

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

