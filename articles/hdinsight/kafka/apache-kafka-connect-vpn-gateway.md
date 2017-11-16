---
title: Verbinding maken met virtuele netwerken - Azure HDInsight met Kafka | Microsoft Docs
description: Informatie over het rechtstreeks verbinding maken met Kafka in HDInsight via een virtueel netwerk van Azure. Informatie over het verbinden met Kafka van ontwikkeling clients met behulp van een VPN-gateway of van clients in uw on-premises netwerk via een VPN-gateway-apparaat.
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.custom: hdinsightactive
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: 2b55de4de6bb94be78649112161211346090b23a
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="connect-to-kafka-on-hdinsight-through-an-azure-virtual-network"></a>Verbinding maken met Kafka in HDInsight via een virtuele Azure-netwerk

Informatie over het rechtstreeks verbinding maken met Kafka in HDInsight via een virtueel netwerk van Azure. Dit document bevat informatie over verbinding maken met Kafka met behulp van de volgende configuraties:

* Basis van resources in een on-premises netwerk. Deze verbinding is gemaakt met behulp van een VPN-apparaat (software of hardware) op uw lokale netwerk.
* Met behulp van een VPN-software-client van een ontwikkelomgeving.

## <a name="architecture-and-planning"></a>Architectuur en planning

HDInsight kan geen directe verbinding met Kafka via het openbare internet. Kafka-clients (producenten en consumenten) moeten in plaats daarvan een van de volgende verbindingsmethoden gebruiken:

* Voer de client in hetzelfde virtuele netwerk als Kafka op HDInsight. Deze configuratie wordt gebruikt in de [beginnen met Apache Kafka op HDInsight](apache-kafka-get-started.md) document. De client voert rechtstreeks op de clusterknooppunten HDInsight of op een andere virtuele machine in hetzelfde netwerk.

* Verbinding maken met een particulier netwerk, zoals uw on-premises netwerk aan het virtuele netwerk. Deze configuratie kan clients in uw on-premises netwerk rechtstreeks werken met Kafka. Om deze configuratie inschakelt, moet u de volgende taken uitvoeren:

    1. Maak een virtueel netwerk.
    2. Maak een VPN-gateway die gebruikmaakt van een site-naar-site-configuratie. De configuratie die in dit document gebruikt verbindt met een VPN-gateway-apparaat in uw on-premises netwerk.
    3. Maak een DNS-server in het virtuele netwerk.
    4. Doorsturen van tussen de DNS-server in elk netwerk configureren.
    5. Installeer Kafka op HDInsight in het virtuele netwerk.

    Zie voor meer informatie de [verbinding maken met Kafka uit een on-premises netwerk](#on-premises) sectie. 

* Verbinding met het maken van afzonderlijke computers aan het virtuele netwerk via een VPN-gateway en de VPN-client. Om deze configuratie inschakelt, moet u de volgende taken uitvoeren:

    1. Maak een virtueel netwerk.
    2. Maak een VPN-gateway die gebruikmaakt van een punt-naar-site-configuratie. Deze configuratie biedt een VPN-client die kan worden geïnstalleerd op Windows-clients.
    3. Installeer Kafka op HDInsight in het virtuele netwerk.
    4. Kafka voor IP-reclame configureren. Deze configuratie kan de client verbinding maken via IP-adressen in plaats van domeinnamen.
    5. Download en gebruik van de VPN-client op het ontwikkelsysteem.

    Zie voor meer informatie de [Kafka met een VPN-client verbinding maken met](#vpnclient) sectie.

    > [!WARNING]
    > Deze configuratie wordt alleen aanbevolen voor ontwikkelingsdoeleinden vanwege de volgende beperkingen:
    >
    > * Elke client moet verbinding maken met behulp van een VPN-software-client. Azure biedt alleen een Windows-client.
    > * De VPN-client geeft niet aanvragen voor naamomzetting aan het virtuele netwerk, zodat u IP-adressen om te communiceren met Kafka moet gebruiken. IP-communicatie is aanvullende configuratie van het cluster Kafka vereist.

Zie voor meer informatie over het gebruik van HDInsight in een virtueel netwerk [HDInsight uitbreiden met behulp van Azure Virtual Networks](../hdinsight-extend-hadoop-virtual-network.md).

## <a id="on-premises"></a>Verbinding maken met Kafka vanuit een on-premises netwerk

Maakt een Kafka-cluster die communiceert met uw on-premises netwerk, volg de stappen in de [HDInsight verbinding maken met uw lokale netwerk](./../connect-on-premises-network.md) document.

> [!IMPORTANT]
> Bij het maken van het HDInsight-cluster, selecteer de __Kafka__ type cluster.

Deze stappen maken de volgende configuratie:

* Azure Virtual Network
* Site-naar-site VPN-gateway
* Azure Storage-account (gebruikt door HDInsight)
* Kafka op HDInsight

Om te controleren of een Kafka client kan verbinding maken met het cluster van on-premises, gebruikt u de stappen in de [voorbeeld:-client voor Python](#python-client) sectie.

## <a id="vpnclient"></a>Verbinding maken met Kafka met een VPN-client

Gebruik de stappen in deze sectie voor het maken van de volgende configuratie:

* Azure Virtual Network
* Punt-naar-site VPN-gateway
* Azure Storage-Account (gebruikt door HDInsight)
* Kafka op HDInsight

1. Volg de stappen in de [werken met zelfondertekende certificaten voor punt-naar-site-verbindingen](../../vpn-gateway/vpn-gateway-certificates-point-to-site.md) document. Dit document wordt gemaakt van de certificaten die nodig zijn voor de gateway.

2. Open een PowerShell-prompt en de volgende code gebruiken voor aanmelding bij uw Azure-abonnement:

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. De volgende code gebruiken voor het maken van variabelen die configuratie-informatie bevatten:

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

4. De volgende code gebruiken voor het maken van de Azure-resourcegroep en het virtuele netwerk:

    ```powershell
    # Create the resource group that contains everything
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
    New-AzureRmVirtualNetworkGateway -Name $vpnName `
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
    > Het kan enkele minuten duren voordat dit proces te voltooien.

5. Gebruik de volgende code en de Azure Storage-Account en de blob-container te maken:

    ```powershell
    # Create the storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. De volgende code gebruiken voor het maken van het HDInsight-cluster:

    ```powershell
    # Create the HDInsight cluster
    New-AzureRmHDInsightCluster `
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
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

  > [!WARNING]
  > Dit proces duurt ongeveer 15 minuten te voltooien.

8. Gebruik de volgende cmdlet voor het ophalen van de URL voor de Windows VPN-client voor het virtuele netwerk:

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    Gebruik de geretourneerde URI in uw webbrowser voor het downloaden van de Windows VPN-client.

### <a name="configure-kafka-for-ip-advertising"></a>Kafka configureren voor IP-advertenties

Standaard retourneert Zookeeper de domeinnaam van de beleggingsmakelaars Kafka aan clients. Deze configuratie werkt niet met de VPN-software-client, zoals naamomzetting voor entiteiten in het virtuele netwerk niet kiezen. Gebruik de volgende stappen voor het configureren van Kafka voor het adverteren van IP-adressen in plaats van domeinnamen voor deze configuratie:

1. Met een webbrowser, gaat u naar https://CLUSTERNAME.azurehdinsight.net. Vervang __CLUSTERNAME__ met de naam van de Kafka op HDInsight-cluster.

    Wanneer u wordt gevraagd, gebruikt u de HTTPS-gebruikersnaam en het wachtwoord voor het cluster. De Ambari-Webgebruikersinterface voor het cluster wordt weergegeven.

2. Als u informatie op Kafka, selecteer __Kafka__ uit de lijst aan de linkerkant.

    ![Lijst met Kafka service gemarkeerd](./media/apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. Selecteer om te geven Kafka configuratie, __Configs__ uit het midden van de bovenste.

    ![Koppelingen naar de configuraties voor Kafka](./media/apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. Vinden de __kafka env__ configuratie, voer `kafka-env` in de __Filter__ op de rechterbovenhoek.

    ![Kafka-configuratie voor kafka env](./media/apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. Toevoegen voor het configureren van Kafka voor het adverteren van IP-adressen, de volgende tekst naar de onderkant van de __kafka-env-sjabloon__ veld:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Voer voor het configureren van de interface die Kafka luistert op `listeners` in de __Filter__ op de rechterbovenhoek.

7. Als u wilt configureren Kafka om te luisteren op alle netwerkinterfaces, wijzig de waarde in de __listeners__ veld `PLAINTEXT://0.0.0.0:9092`.

8. Voor het opslaan van wijzigingen in de configuratie, gebruiken de __opslaan__ knop. Voer een SMS-bericht met een beschrijving van de wijzigingen. Selecteer __OK__ nadat de wijzigingen zijn opgeslagen.

    ![Configuratie knop Opslaan](./media/apache-kafka-connect-vpn-gateway/save-button.png)

9. Gebruiken om fouten te voorkomen bij het opnieuw opstarten Kafka, de __serviceacties__ en selecteer __inschakelen op onderhoudsmodus__. Klik op OK om deze bewerking te voltooien.

    ![Service-acties, met inschakelen onderhoud gemarkeerd](./media/apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. Als opnieuw Kafka, wilt u de __opnieuw__ en selecteer __start opnieuw alle van invloed op een__. Bevestig het opnieuw opstarten en gebruik vervolgens de __OK__ knop nadat de bewerking is voltooid.

    ![Start opnieuw op de knop met alle van invloed op een opnieuw opstarten gemarkeerd](./media/apache-kafka-connect-vpn-gateway/restart-button.png)

11. U schakelt onderhoudsmodus uit de __serviceacties__ en selecteer __inschakelen uit de onderhoudsmodus__. Selecteer **OK** om deze bewerking te voltooien.

### <a name="connect-to-the-vpn-gateway"></a>Verbinding maken met de VPN-gateway

Verbinding maken met de VPN-gateway van een __Windows-client__, gebruiken de __verbinding maken met Azure__ sectie van de [een punt-naar-Site-verbinding configureren](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) document.

## <a id="python-client"></a>Voorbeeld:-Client voor Python

Gebruik de volgende stappen voor het maken en uitvoeren van een Python producenten en consumenten voor het valideren van de verbinding met Kafka:

1. Gebruik een van de volgende methoden voor het ophalen van de volledig gekwalificeerde domeinnaam (FQDN) en IP-adressen van de knooppunten in het cluster Kafka:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

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

    Dit script wordt ervan uitgegaan dat `$resourceGroupName` is de naam van de Azure-resourcegroep met het virtuele netwerk.

    De geretourneerde gegevens voor gebruik opslaan in de volgende stappen.

2. Gebruik de volgende voor het installeren van de [kafka python](http://kafka-python.readthedocs.io/) client:

        pip install kafka-python

3. Om gegevens te verzenden naar Kafka, moet u de volgende Python-code gebruiken:

  ```python
  from kafka import KafkaProducer
  # Replace the `ip_address` entries with the IP address of your worker nodes
  # NOTE: you don't need the full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    Vervang de `'kafka_broker'` vermeldingen met de adressen die is geretourneerd uit stap 1 in deze sectie:

    * Als u een __Software VPN-client__, vervang de `kafka_broker` vermeldingen met het IP-adres van uw worker-knooppunten.

    * Als u hebt __naamomzetting via een aangepaste DNS-server ingeschakeld__, vervang de `kafka_broker` vermeldingen met de FQDN-naam van de worker-knooppunten.

    > [!NOTE]
    > Deze code verzendt de tekenreeks `test message` naar het onderwerp `testtopic`. De standaardconfiguratie van Kafka op HDInsight is het maken van het onderwerp, als deze niet bestaat.

4. Voor het ophalen van de berichten van Kafka, moet u de volgende Python-code gebruiken:

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

    Vervang de `'kafka_broker'` vermeldingen met de adressen die is geretourneerd uit stap 1 in deze sectie:

    * Als u een __Software VPN-client__, vervang de `kafka_broker` vermeldingen met het IP-adres van uw worker-knooppunten.

    * Als u hebt __naamomzetting via een aangepaste DNS-server ingeschakeld__, vervang de `kafka_broker` vermeldingen met de FQDN-naam van de worker-knooppunten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van HDInsight met een virtueel netwerk, de [uitbreiden Azure HDInsight met behulp van een Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md) document.

Zie de volgende documenten voor meer informatie over het maken van een Azure-netwerk met punt-naar-Site VPN-gateway:

* [Een punt-naar-Site-verbinding met de Azure portal configureren](../../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Configureren van een punt-naar-Site-verbinding met Azure PowerShell](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

Zie de volgende documenten voor meer informatie over het werken Kafka in HDInsight:

* [Aan de slag met Kafka in HDInsight](apache-kafka-get-started.md)
* [Mirroring met Kafka in HDInsight gebruiken](apache-kafka-mirroring.md)
