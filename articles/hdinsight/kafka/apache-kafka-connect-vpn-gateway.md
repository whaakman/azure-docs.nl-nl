---
title: Verbinding maken met gebruik van virtuele netwerken - Azure HDInsight Kafka
description: Leer hoe u rechtstreeks verbinding maken met Kafka in HDInsight via een Virtueelnetwerk van Azure. Leer hoe u verbinding maken met Kafka van ontwikkeling van clients met behulp van een VPN-gateway of van clients in uw on-premises netwerk via een VPN-gateway-apparaat.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 6807a14bb7b46389abbc4c5a44e7699c79e29dff
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361606"
---
# <a name="connect-to-apache-kafka-on-hdinsight-through-an-azure-virtual-network"></a>Verbinding maken met Apache Kafka in HDInsight via een Azure-netwerk

Leer hoe u rechtstreeks verbinding maken met Apache Kafka in HDInsight via een Virtueelnetwerk van Azure. Dit document bevat informatie over verbinding maken met Kafka met behulp van de volgende configuraties:

* Van resources in een on-premises netwerk. Deze verbinding tot stand is gebracht via een VPN-apparaat (software of hardware) op uw lokale netwerk.
* Vanaf een ontwikkelomgeving met behulp van een VPN-softwareclient.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="architecture-and-planning"></a>Architectuur en planning

HDInsight kan geen rechtstreekse verbinding met Kafka via het openbare internet. Kafka-clients (producenten en consumenten) moeten in plaats daarvan een van de volgende verbindingsmethoden gebruiken:

* De client wordt uitgevoerd in hetzelfde virtuele netwerk als Kafka op HDInsight. Deze configuratie wordt gebruikt de [beginnen met Apache Kafka in HDInsight](apache-kafka-get-started.md) document. De client voert rechtstreeks op de knooppunten van HDInsight-cluster of op een andere virtuele machine in hetzelfde netwerk.

* Een particulier netwerk, zoals uw on-premises netwerk verbinden met het virtuele netwerk. Deze configuratie kan clients in uw on-premises netwerk rechtstreeks werken met Kafka. Als u wilt deze configuratie inschakelt, moet u de volgende taken uitvoeren:

  1. Maak een virtueel netwerk.
  2. Een VPN-gateway die gebruikmaakt van een site-naar-site-configuratie maken. De configuratie die in dit document wordt gebruikt, maakt verbinding met een VPN-gateway-apparaat in uw on-premises netwerk.
  3. Maak een DNS-server in het virtuele netwerk.
  4. Doorsturen van tussen de DNS-server in elk netwerk configureren.
  5. Maken van een Kafka op HDInsight-cluster in het virtuele netwerk.

     Zie voor meer informatie de [verbinding maken met Apache Kafka vanuit een on-premises netwerk](#on-premises) sectie. 

* Afzonderlijke computers verbinden met het virtuele netwerk met behulp van een VPN-gateway en VPN-client. Als u wilt deze configuratie inschakelt, moet u de volgende taken uitvoeren:

  1. Maak een virtueel netwerk.
  2. Een VPN-gateway die gebruikmaakt van een punt-naar-site-configuratie maken. Deze configuratie kan worden gebruikt met zowel Windows als Mac OS-clients.
  3. Maken van een Kafka op HDInsight-cluster in het virtuele netwerk.
  4. Kafka voor IP-reclame configureren. Deze configuratie kan de client verbinding maken met behulp van de broker IP-adressen in plaats van domeinnamen.
  5. Download en gebruik van de VPN-client op het ontwikkelingssysteem.

     Zie voor meer informatie de [verbinding maken met Apache Kafka met een VPN-client](#vpnclient) sectie.

     > [!WARNING]  
     > Deze configuratie wordt alleen aanbevolen voor ontwikkelingsdoeleinden vanwege de volgende beperkingen:
     >
     > * Elke client moet verbinding maken met behulp van een VPN-softwareclient.
     > * De VPN-client geeft niet aanvragen voor naamomzetting met het virtuele netwerk, zodat u IP-adressen om te communiceren met Kafka moet gebruiken. IP-communicatie vereist aanvullende configuratie van het Kafka-cluster.

Zie voor meer informatie over het gebruik van HDInsight in een virtueel netwerk [HDInsight uitbreiden met behulp van Azure Virtual Networks](../hdinsight-extend-hadoop-virtual-network.md).

## <a id="on-premises"></a> Verbinding maken met Apache Kafka vanuit een on-premises netwerk

Voor het maken van een Kafka-cluster die communiceert met uw on-premises netwerk, volg de stappen in de [HDInsight verbinden met uw on-premises netwerk](./../connect-on-premises-network.md) document.

> [!IMPORTANT]  
> Wanneer u de HDInsight-cluster maakt, selecteert u de __Kafka__ clustertype.

Deze stappen maakt u de volgende configuratie:

* Azure Virtual Network
* Site-naar-site VPN-gateway
* Azure Storage-account (gebruikt door HDInsight)
* Kafka op HDInsight

Als u wilt controleren of een Kafka-client kan verbinding met het cluster van on-premises, gebruikt u de stappen in de [voorbeeld: Python-client](#python-client) sectie.

## <a id="vpnclient"></a> Verbinding maken met Apache Kafka met een VPN-client

Gebruik de stappen in deze sectie om te maken van de volgende configuratie:

* Azure Virtual Network
* Punt-naar-site VPN-gateway
* Azure Storage-Account (gebruikt door HDInsight)
* Kafka op HDInsight

1. Volg de stappen in de [werken met zelfondertekende certificaten voor punt-naar-site-verbindingen](../../vpn-gateway/vpn-gateway-certificates-point-to-site.md) document. Dit document wordt gemaakt van de certificaten die nodig zijn voor de gateway.

2. Open een PowerShell-prompt en meld u aan bij uw Azure-abonnement met de volgende code:

    ```powershell
    Connect-AzAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzSubscription -SubscriptionName "name of your subscription"
    ```

3. Gebruik de volgende code om te maken van variabelen die configuratie-informatie bevatten:

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is the resource group name?"
    $baseName = Read-Host "What is the base name? It is used to create names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want to create the resources in?"
    $rootCert = Read-Host "What is the file path to the root certificate? It is used to secure the VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter the HTTPS user name and password for the HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter the SSH user name and password for the HDInsight cluster" -UserName "sshuser"

    # Names for Azure resources
    $networkName = "net-$baseName"
    $clusterName = "kafka-$baseName"
    $storageName = "store$baseName" # Can't use dashes in storage names
    $defaultContainerName = $clusterName
    $defaultSubnetName = "default"
    $gatewaySubnetName = "GatewaySubnet"
    $gatewayPublicIpName = "GatewayIp"
    $gatewayIpConfigName = "GatewayConfig"
    $vpnRootCertName = "rootcert"
    $vpnName = "VPNGateway"

    # Network settings
    $networkAddressPrefix = "10.0.0.0/16"
    $defaultSubnetPrefix = "10.0.0.0/24"
    $gatewaySubnetPrefix = "10.0.1.0/24"
    $vpnClientAddressPool = "172.16.201.0/24"

    # HDInsight settings
    $HdiWorkerNodes = 4
    $hdiVersion = "3.6"
    $hdiType = "Kafka"
    ```

4. Gebruik de volgende code om de Azure-resourcegroep en virtueel netwerk te maken:

    ```powershell
    # Create the resource group that contains everything
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
    New-AzVirtualNetworkGateway -Name $vpnName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -IpConfigurations $gatewayIpConfig `
        -GatewayType Vpn `
        -VpnType RouteBased `
        -EnableBgp $false `
        -GatewaySku Standard `
        -VpnClientAddressPool $vpnClientAddressPool `
        -VpnClientRootCertificates $p2sRootCert
    ```

    > [!WARNING]  
    > Het kan enige tijd duren voor dit proces is voltooid.

5. Gebruik de volgende code om de Azure Storage-Account en blob-container te maken:

    ```powershell
    # Create the storage account
    New-AzStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. Gebruik de volgende code om het HDInsight-cluster te maken:

    ```powershell
    # Create the HDInsight cluster
    New-AzHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $hdiWorkerNodes `
        -ClusterType $hdiType `
        -OSType Linux `
        -Version $hdiVersion `
        -HttpCredential $adminCreds `
        -SshCredential $sshCreds `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageKey `
        -DefaultStorageContainer $defaultContainerName `
        -DisksPerWorkerNode 2 `
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

   > [!WARNING]  
   > Dit proces duurt ongeveer 15 minuten om te voltooien.

### <a name="configure-kafka-for-ip-advertising"></a>Kafka configureren voor IP-advertenties

Standaard retourneert Apache Zookeeper de domeinnaam van het Kafka-brokers aan clients. Deze configuratie werkt niet met de VPN-softwareclient, zoals het naamomzetting niet voor entiteiten in het virtuele netwerk gebruiken. Gebruik de volgende stappen uit Kafka voor het adverteren van IP-adressen in plaats van domeinnaam configureren voor deze configuratie:

1. Met behulp van een webbrowser, gaat u naar https://CLUSTERNAME.azurehdinsight.net. Vervang __CLUSTERNAME__ met de naam van het Kafka in HDInsight-cluster.

    Wanneer u hierom wordt gevraagd, gebruikt u de HTTPS-gebruikersnaam en het wachtwoord voor het cluster. De Ambari-Webinterface voor het cluster wordt weergegeven.

2. Als u informatie op Kafka, selecteer __Kafka__ in de lijst aan de linkerkant.

    ![Lijst met Services met Kafka gemarkeerd](./media/apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. Als u wilt weergeven van Kafka-configuratie, selecteer __Peeringconfiguraties__ uit het midden.

    ![Koppelingen naar de configuraties voor Kafka](./media/apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. Om te zoeken de __kafka-env__ configuratie, voer `kafka-env` in de __Filter__ veld in de rechterbovenhoek.

    ![Kafka-configuratie voor kafka-env](./media/apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. Voor het configureren van Kafka voor het adverteren van IP-adressen, voeg de volgende tekst toe aan de onderkant van de __kafka-env-template__ veld:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Voer voor het configureren van de interface die Kafka luistert, `listeners` in de __Filter__ veld in de rechterbovenhoek.

7. Voor het configureren van Kafka om te luisteren op alle netwerkinterfaces, wijzigt u de waarde in de __listeners__ veld `PLAINTEXT://0.0.0.0:9092`.

8. Om de wijzigingen in de configuratie hebt opgeslagen, gebruikt u de __opslaan__ knop. Voer een SMS-bericht met een beschrijving van de wijzigingen. Selecteer __OK__ zodra de wijzigingen zijn opgeslagen.

    ![Configuratie van de knop Opslaan](./media/apache-kafka-connect-vpn-gateway/save-button.png)

9. Gebruik ter voorkoming van fouten bij het opnieuw opstarten van Kafka het __serviceacties__ en selecteer __onderhoudsmodus inschakelen__. Klik op OK om deze bewerking te voltooien.

    ![Service-acties, met inschakelen voor onderhoud gemarkeerd](./media/apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. Als u wilt starten Kafka, gebruikt u de __opnieuw__ en selecteer __start opnieuw op alle betrokken__. Bevestig het opnieuw starten en gebruik vervolgens de __OK__ knop nadat de bewerking is voltooid.

    ![Start opnieuw op de knop en alle betrokken opnieuw opgestart gemarkeerd](./media/apache-kafka-connect-vpn-gateway/restart-button.png)

11. Als u wilt uitschakelen in de onderhoudsmodus, gebruikt u de __serviceacties__ en selecteer __inschakelen uit de onderhoudsmodus__. Selecteer **OK** om deze bewerking te voltooien.

### <a name="connect-to-the-vpn-gateway"></a>Verbinding maken met de VPN-gateway

Voor verbinding met de VPN-gateway, gebruikt u de __verbinding maken met Azure__ sectie van de [een punt-naar-Site-verbinding configureren](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#connect) document.

## <a id="python-client"></a> Voorbeeld: Python-client

Gebruik de volgende stappen om te maken en uitvoeren van een Python-producent en consument voor het valideren van verbindingen met Kafka:

1. Gebruik een van de volgende methoden voor het ophalen van de volledig gekwalificeerde domeinnaam (FQDN) en IP-adressen van de knooppunten in het Kafka-cluster:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Met dit script wordt ervan uitgegaan dat `$resourceGroupName` is de naam van de Azure-resourcegroep waarin het virtuele netwerk.

    De geretourneerde gegevens voor gebruik opslaan in de volgende stappen.

2. Gebruik de volgende voor het installeren van de [kafka-python](https://kafka-python.readthedocs.io/) client:

        pip install kafka-python

3. Voor het verzenden van gegevens met Kafka, gebruik de volgende Python-code:

   ```python
   from kafka import KafkaProducer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # NOTE: you don't need the full list of worker nodes, just one or two.
   producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
   for _ in range(50):
      producer.send('testtopic', b'test message')
   ```

    Vervang de `'kafka_broker'` items met de adressen die zijn geretourneerd uit stap 1 in deze sectie:

   * Als u een __Software VPN-client__, vervangen de `kafka_broker` items met het IP-adres van uw worker-knooppunten.

   * Als u hebt __naamomzetting via een aangepaste DNS-server ingeschakeld__, vervangen de `kafka_broker` items met de FQDN-naam van de worker-knooppunten.

     > [!NOTE]
     > Deze code verzendt de tekenreeks `test message` naar het onderwerp `testtopic`. De standaardconfiguratie van Kafka in HDInsight is het maken van het onderwerp, als deze niet bestaat.

4. Als u wilt de berichten ophalen uit Kafka, gebruik de volgende Python-code:

   ```python
   from kafka import KafkaConsumer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # Again, you only need one or two, not the full list.
   # Note: auto_offset_reset='earliest' resets the starting offset to the beginning
   #       of the topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    Vervang de `'kafka_broker'` items met de adressen die zijn geretourneerd uit stap 1 in deze sectie:

    * Als u een __Software VPN-client__, vervangen de `kafka_broker` items met het IP-adres van uw worker-knooppunten.

    * Als u hebt __naamomzetting via een aangepaste DNS-server ingeschakeld__, vervangen de `kafka_broker` items met de FQDN-naam van de worker-knooppunten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van HDInsight met een virtueel netwerk, de [Azure HDInsight uitbreiden met behulp van een Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md) document.

Zie de volgende documenten voor meer informatie over het maken van een Azure-netwerk met punt-naar-Site VPN-gateway:

* [Een punt-naar-Site-verbinding met de Azure-portal configureren](../../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Een punt-naar-Site-verbinding met Azure PowerShell configureren](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

Zie de volgende documenten voor meer informatie over het werken met Apache Kafka in HDInsight:

* [Aan de slag met Apache Kafka in HDInsight](apache-kafka-get-started.md)
* [Spiegeling met Apache Kafka in HDInsight gebruiken](apache-kafka-mirroring.md)
