---
title: HDInsight met het virtuele netwerk - Azure uitbreiden | Microsoft Docs
description: Informatie over het gebruik van Azure Virtual Network verbinding maken met HDInsight andere cloudresources of resources in uw datacenter
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/21/2018
ms.author: larryfr
ms.openlocfilehash: cb005549fcd9a5850c990d8ef8acb724443f9908
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Azure HDInsight met behulp van een Azure-netwerk uitbreiden

Informatie over het gebruik van HDInsight met een [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Een virtueel netwerk van Azure, kunt de volgende scenario's:

* Verbinding maken met HDInsight rechtstreeks vanuit een on-premises netwerk.

* Verbinding maken met HDInsight gegevens opslaat in een virtueel Azure-netwerk.

* Rechtstreeks toegang hebben tot Hadoop-services die niet beschikbaar openbaar via internet zijn. Kafka-API's of de HBase-Java-API.

> [!WARNING]
> De informatie in dit document is een goed begrip van TCP/IP-netwerken vereist. Als u niet bekend met TCP/IP-netwerken bent, moet u samenwerken met iemand die dit voordat u wijzigingen op productienetwerken.

> [!IMPORTANT]
> Als u zoekt Stapsgewijze instructies voor het verbinden van HDInsight met uw on-premises netwerk via een virtueel netwerk van Azure, raadpleegt u de [HDInsight verbinding maken met uw on-premises netwerk](connect-on-premises-network.md) document.

## <a name="planning"></a>Planning

Hier volgen de vragen die u bij het plannen voor het installeren van HDInsight in een virtueel netwerk moet beantwoorden:

* Moet u voor het installeren van HDInsight in een bestaand virtueel netwerk? Of maakt u een nieuw netwerk?

    Als u een bestaand virtueel netwerk gebruikt, moet u wellicht de netwerkconfiguratie wijzigen voordat u HDInsight kunt installeren. Zie voor meer informatie de [HDInsight toevoegen aan een bestaand virtueel netwerk](#existingvnet) sectie.

* Wilt u toch verbinding maken met het virtuele netwerk met HDInsight op een ander virtueel netwerk of uw on-premises netwerk?

    Eenvoudig werkt alleen met resources in netwerken, moet u een aangepaste DNS-server maken en configureren van DNS-doorsturen. Zie voor meer informatie de [meerdere netwerken met elkaar verbinden](#multinet) sectie.

* Wilt u beperken/omleiden binnenkomend of uitgaand verkeer naar HDInsight?

    HDInsight moet hebben onbeperkte communicatie met specifieke IP-adressen in de Azure-Datacenter. Er zijn ook verschillende poorten die moeten worden toegestaan via firewalls voor clientcommunicatie. Zie voor meer informatie de [netwerkverkeer beheren](#networktraffic) sectie.

## <a id="existingvnet"></a>HDInsight toevoegen aan een bestaand virtueel netwerk

Gebruik de stappen in deze sectie om te ontdekken hoe u een nieuwe HDInsight toevoegt aan een bestaand virtueel netwerk van Azure.

> [!NOTE]
> U kunt een bestaand HDInsight-cluster in een virtueel netwerk niet toevoegen.

1. Gebruikt u een klassiek of Resource Manager-implementatiemodel voor het virtuele netwerk?

    HDInsight 3,4 en groter vereist een virtueel netwerk van Resource Manager. Eerdere versies van HDInsight vereist een klassiek virtueel netwerk.

    Als uw bestaande netwerk een klassiek virtueel netwerk is, moet u een virtueel netwerk van Resource Manager maken en sluit vervolgens de twee. [Klassieke vnet's verbinden met nieuwe VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Zodra toegevoegd, wordt HDInsight geïnstalleerd in het Resource Manager-netwerk kan communiceren met resources in het klassieke netwerk.

2. Geforceerde tunneling gebruiken Geforceerde tunneling is subnetinstelling dat ervoor zorgt uitgaand internetverkeer met een apparaat voor controle dat en logboekregistratie. HDInsight biedt geen ondersteuning voor geforceerde tunneling. Verwijder geforceerde tunneling voordat u HDInsight installeert in een subnet, of een nieuw subnet maken voor HDInsight.

3. Gebruik je netwerkbeveiligingsgroepen, de gebruiker gedefinieerde routes of virtuele netwerkapparaten naar het verkeer te beperken van of naar het virtuele netwerk?

    Als een beheerde service vereist HDInsight onbeperkte toegang tot verschillende IP-adressen in de Azure-Datacenter. Werk alle bestaande netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes zodat de communicatie met deze IP-adressen.

    HDInsight als host fungeert voor meerdere services, die verschillende poorten gebruiken. Geen verkeer blokkeert dat via deze poorten. Zie voor een lijst met poorten om toe te staan via firewalls virtueel apparaat, de [beveiliging](#security) sectie.

    Als u uw bestaande beveiligingsconfiguratie zoekt, gebruikt u de volgende Azure PowerShell of Azure CLI-opdrachten:

    * Netwerkbeveiligingsgroepen

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        Zie voor meer informatie de [netwerkbeveiligingsgroepen oplossen](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) document.

        > [!IMPORTANT]
        > Netwerkbeveiligingsgroepen worden toegepast in volgorde op basis van Regelprioriteit. De eerste regel die overeenkomt met het patroon verkeer wordt toegepast en geen andere voor dat verkeer wordt toegepast. Volgorde van de regels van meest strikte op minimaal. Zie voor meer informatie de [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) document.

    * Door de gebruiker gedefinieerde routes

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        Zie voor meer informatie de [routes oplossen](../virtual-network/virtual-network-routes-troubleshoot-portal.md) document.

4. Maken van een HDInsight-cluster en het virtuele netwerk van Azure selecteren tijdens de configuratie. Gebruik de stappen in de volgende documenten voor inzicht in het proces voor het cluster maken:

    * [HDInsight maken met Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [HDInsight maken met Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Maken van HDInsight met behulp van Azure CLI 1.0](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [HDInsight met behulp van een Azure Resource Manager-sjabloon maken](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > HDInsight toe te voegen aan een virtueel netwerk is een optionele configuratie-stap. Zorg ervoor dat het virtuele netwerk selecteren bij het configureren van het cluster.

## <a id="multinet"></a>Meerdere netwerken met elkaar verbinden

De grootste uitdaging met een configuratie met meerdere is naamomzetting tussen de netwerken.

Azure biedt naamomzetting voor Azure-services die zijn geïnstalleerd in een virtueel netwerk. Deze ingebouwde naamomzetting kunt HDInsight verbinding maken met de volgende bronnen met behulp van een volledig gekwalificeerde domeinnaam (FQDN):

* Een resource die beschikbaar is op het internet. Bijvoorbeeld microsoft.com, google.com.

* Alle bronnen die zich in hetzelfde Azure-netwerk, met behulp van de __interne DNS-naam__ van de resource. Bijvoorbeeld, wanneer u de standaard-naamomzetting, volgen voorbeeld interne DNS-namen is toegewezen aan HDInsight worker-knooppunten:

    * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
    * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Deze beide knooppunten kunnen communiceren rechtstreeks met elkaar en andere knooppunten in HDInsight, met behulp van de interne DNS-namen.

De standaard-naamomzetting biedt __niet__ HDInsight omzetten van de namen van bronnen in netwerken die zijn gekoppeld aan het virtuele netwerk toestaan. Het is bijvoorbeeld algemene uw on-premises netwerk koppelen aan het virtuele netwerk. Met alleen de standaard naamomzetting HDInsight kan geen toegang krijgen tot bronnen in de on-premises netwerk met de naam. Het omgekeerde geldt ook resources in uw on-premises netwerk geen toegang tot bronnen in het virtuele netwerk met de naam.

> [!WARNING]
> U moet de aangepaste DNS-server maken en configureren van het virtuele netwerk om het te gebruiken voordat u het HDInsight-cluster maakt.

Om naamomzetting tussen het virtuele netwerk en bronnen in de gekoppelde netwerken, moet u de volgende acties uitvoeren:

1. Maak een aangepaste DNS-server in de Azure Virtual Network waaruit u plant voor het installeren van HDInsight.

2. Het virtuele netwerk voor het gebruik van de aangepaste DNS-server configureren.

3. Zoeken naar dat de Azure DNS-achtervoegsel voor het virtuele netwerk toegewezen. Deze waarde is vergelijkbaar met `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Zie voor meer informatie over het zoeken naar de DNS-achtervoegsel de [voorbeeld: aangepaste DNS](#example-dns) sectie.

4. Doorsturen van tussen de DNS-servers configureren. De configuratie is afhankelijk van het type van het externe netwerk.

    * Als het externe netwerk een on-premises netwerk is, configureren van DNS als volgt:
        
        * __Aangepaste DNS__ (in het virtuele netwerk):

            * Aanvragen voor de DNS-achtervoegsel van het virtuele netwerk naar de Azure recursieve resolver (168.63.129.16) doorsturen. Azure verwerkt aanvragen voor bronnen in het virtuele netwerk

            * Doorsturen van alle andere verzoeken naar de lokale DNS-server. De lokale DNS-server verwerkt alle andere aanvragen voor naamomzetting, zelfs aanvragen voor internetbronnen zoals Microsoft.com.

        * __Lokale DNS__: doorsturen van aanvragen voor het virtuele netwerk DNS-achtervoegsel voor de aangepaste DNS-server. De aangepaste DNS-server stuurt vervolgens door naar de Azure recursieve-resolver.

        Deze configuratie routes aanvragen voor FQDN-namen die het DNS-achtervoegsel van het virtuele netwerk voor de aangepaste DNS-server bevatten. Alle andere verzoeken (zelfs voor openbare internet-adressen) worden verwerkt door de lokale DNS-server.

    * Als het externe netwerk een ander virtueel netwerk van Azure is, configureren van DNS als volgt:

        * __Aangepaste DNS__ (in elk virtueel netwerk):

            * Aanvragen voor het DNS-achtervoegsel van de virtuele netwerken worden doorgestuurd naar de aangepaste DNS-servers. De DNS-server in elk virtueel netwerk is verantwoordelijk voor het oplossen van resources binnen het netwerk.

            * Alle andere verzoeken naar de Azure recursieve resolver doorsturen. De recursieve resolver is verantwoordelijk voor het oplossen van lokale en internetbronnen.

        De DNS-server voor elk netwerk aanvragen naar de andere verzendt, op basis van DNS-achtervoegsel. Andere aanvragen worden omgezet met behulp van de Azure recursieve-resolver.

    Zie voor een voorbeeld van elke configuratie, de [voorbeeld: aangepaste DNS](#example-dns) sectie.

Zie voor meer informatie de [naamomzetting voor VM's en Rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.

## <a name="directly-connect-to-hadoop-services"></a>Rechtstreeks verbinding maken met Hadoop-services

De meeste documentatie op HDInsight wordt ervan uitgegaan dat u toegang tot het cluster via internet hebt. Bijvoorbeeld, dat u verbinding met het cluster kunt maken op https://CLUSTERNAME.azurehdinsight.net. Dit adres wordt gebruikt voor de openbare-gateway niet beschikbaar is als u nsg's of udr's hebt gebruikt om toegang te beperken van het internet.

Voor verbinding met Ambari en andere webpagina's via het virtuele netwerk, gebruikt u de volgende stappen uit:

1. Voor het detecteren van de interne volledig gekwalificeerde domeinnamen (FQDN) van de clusterknooppunten HDInsight, moet u een van de volgende methoden gebruiken:

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

    De FQDN-naam vinden voor de hoofdknooppunten in de lijst met knooppunten die zijn geretourneerd, en de FQDN's gebruiken voor verbinding met Ambari en andere web-services. Bijvoorbeeld: `http://<headnode-fqdn>:8080` voor toegang tot de Ambari.

    > [!IMPORTANT]
    > Sommige services die worden gehost op de hoofdknooppunten zijn alleen actief is op één knooppunt tegelijk. Als u probeert toegang tot een service op één hoofdknooppunt en een 404-fout retourneert, overschakelen naar het hoofdknooppunt.

2. Zie het vaststellen van het knooppunt en de poort die een service beschikbaar is op de [poorten die worden gebruikt door de services van Hadoop op HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.

## <a id="networktraffic"></a> Beheren van netwerkverkeer

Netwerkverkeer in een virtuele Azure-netwerken kan worden beheerd met behulp van de volgende methoden:

* **Netwerkbeveiligingsgroepen** (NSG) kunt u binnenkomend en uitgaand verkeer op het netwerk te filteren. Zie voor meer informatie de [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) document.

    > [!WARNING]
    > HDInsight biedt geen ondersteuning voor uitgaand verkeer te beperken.

* **Gebruiker gedefinieerde routes** (UDR) definiëren hoe verkeersstromen tussen resources in het netwerk. Zie voor meer informatie de [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md) document.

* **Virtuele apparaten** repliceren van de functionaliteit van apparaten, zoals firewalls en routers. Zie voor meer informatie de [netwerkapparaten](https://azure.microsoft.com/solutions/network-appliances) document.

Als een beheerde service vereist HDInsight onbeperkte toegang tot Azure-status en beheer van services in de Azure-cloud. Wanneer u nsg's en udr's, moet u ervoor zorgen dat HDInsight deze services nog steeds met HDInsight communiceren kunnen.

HDInsight beschrijft de services op verschillende poorten. Wanneer u een virtueel apparaat firewall gebruikt, moet u verkeer op de poorten die voor deze services toestaan. Zie de sectie [vereiste poorten] voor meer informatie.

### <a id="hdinsight-ip"></a> HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes

Als u gebruiken wilt **netwerkbeveiligingsgroepen** of **gebruiker gedefinieerde routes** netwerkverkeer wordt beheerd, voert u de volgende acties voor de installatie van HDInsight:

1. Identificeer de Azure-regio die u wilt gebruiken voor HDInsight.

2. Identificeer de IP-adressen die door HDInsight vereist. Zie voor meer informatie de [IP-adressen die zijn vereist voor HDInsight](#hdinsight-ip) sectie.

3. Maak of wijzig de netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes voor het subnet dat u van plan bent voor het installeren van HDInsight in.

    * __Netwerkbeveiligingsgroepen__: toestaan __inkomende__ verkeer op poort __443__ van de IP-adressen.
    * __Gebruiker gedefinieerde routes__: Maak een route voor elk IP-adres en stel de __volgende hop type__ naar __Internet__.

Zie de volgende documentatie voor meer informatie over netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes:

* [Netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md)

* [Gebruiker gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a>Geforceerde tunneling

Geforceerde tunneling is een gebruiker gedefinieerde routering configuratie waarbij alle verkeer van een subnet wordt geforceerd voor een specifieke netwerk- of locatie, zoals uw on-premises netwerk. HDInsight biedt __niet__ ondersteuning geforceerde tunneling.

## <a id="hdinsight-ip"></a> Vereiste IP-adressen

> [!IMPORTANT]
> De Azure management van de status en -services moet communiceren met HDInsight. Als u netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes gebruikt, kunt u verkeer van de IP-adressen voor deze services te bereiken van HDInsight.
>
> Als u niet netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes waarmee verkeer gebruikt, kunt u deze sectie overslaan.

Als u netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes gebruikt, moet u het verkeer van de Azure status en de management-services te bereiken HDInsight toestaan. Gebruik de volgende stappen uit om te zoeken de IP-adressen dat moeten worden toegestaan:

1. U moet altijd verkeer van de volgende IP-adressen toestaan:

    | IP-adres | Toegestane poort | Richting |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | Inkomend |
    | 23.99.5.239 | 443 | Inkomend |
    | 168.61.48.131 | 443 | Inkomend |
    | 138.91.141.162 | 443 | Inkomend |

2. Als uw HDInsight-cluster zich in een van de volgende regio's, moet u het verkeer van de IP-adressen die worden vermeld voor de regio toestaan:

    > [!IMPORTANT]
    > Als de Azure-regio u niet wordt weergegeven, klikt u vervolgens gebruik alleen de vier IP-adressen uit stap 1.

    | Land | Regio | Toegestane IP-adressen | Toegestane poort | Richting |
    | ---- | ---- | ---- | ---- | ----- |
    | Azië | Oost-Azië | 23.102.235.122</br>52.175.38.134 | 443 | Inkomend |
    | &nbsp; | Zuidoost-Azië | 13.76.245.160</br>13.76.136.249 | 443 | Inkomend |
    | Australië | Australië - oost | 104.210.84.115</br>13.75.152.195 | 443 | Inkomend |
    | &nbsp; | Australië - zuidoost | 13.77.2.56</br>13.77.2.94 | 443 | Inkomend |
    | Brazilië | Brazilië - zuid | 191.235.84.104</br>191.235.87.113 | 443 | Inkomend |
    | Canada | Canada - oost | 52.229.127.96</br>52.229.123.172 | 443 | Inkomend |
    | &nbsp; | Canada - midden | 52.228.37.66</br>52.228.45.222 | 443 | Inkomend |
    | China | China - noord | 42.159.96.170</br>139.217.2.219 | 443 | Inkomend |
    | &nbsp; | China East | 42.159.198.178</br>42.159.234.157 | 443 | Inkomend |
    | Europa | Noord-Europa | 52.164.210.96</br>13.74.153.132 | 443 | Inkomend |
    | &nbsp; | West-Europa| 52.166.243.90</br>52.174.36.244 | 443 | Inkomend |
    | Duitsland | Duitsland - centraal | 51.4.146.68</br>51.4.146.80 | 443 | Inkomend |
    | &nbsp; | Duitsland - noordoost | 51.5.150.132</br>51.5.144.101 | 443 | Inkomend |
    | India | Centraal-India | 52.172.153.209</br>52.172.152.49 | 443 | Inkomend |
    | Japan | Japan - oost | 13.78.125.90</br>13.78.89.60 | 443 | Inkomend |
    | &nbsp; | Japan - west | 40.74.125.69</br>138.91.29.150 | 443 | Inkomend |
    | Korea | Korea - centraal | 52.231.39.142</br>52.231.36.209 | 433 | Inkomend |
    | &nbsp; | Korea - zuid | 52.231.203.16</br>52.231.205.214 | 443 | Inkomend
    | Verenigd Koninkrijk | Verenigd Koninkrijk West | 51.141.13.110</br>51.141.7.20 | 443 | Inkomend |
    | &nbsp; | Verenigd Koninkrijk Zuid | 51.140.47.39</br>51.140.52.16 | 443 | Inkomend |
    | Verenigde Staten | VS - midden | 13.67.223.215</br>40.86.83.253 | 443 | Inkomend |
    | &nbsp; | VS - oost | 13.82.225.233</br>40.71.175.99 | 443 | Inkomend |
    | &nbsp; | Noord-centraal VS | 157.56.8.38</br>157.55.213.99 | 443 | Inkomend |
    | &nbsp; | West-centraal VS | 52.161.23.15</br>52.161.10.167 | 443 | Inkomend |
    | &nbsp; | VS - west | 13.64.254.98</br>23.101.196.19 | 443 | Inkomend |
    | &nbsp; | VS - west 2 | 52.175.211.210</br>52.175.222.222 | 443 | Inkomend |

    Zie voor informatie over de IP-adressen voor Azure Government, de [Azure Government Intelligence en analyse](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) document.

3. Als u een aangepaste DNS-server met het virtuele netwerk gebruikt, moet u ook toegang vanaf toestaan __168.63.129.16__. Dit adres is van de Azure recursieve naamomzetting. Zie voor meer informatie de [naamomzetting voor VM's en rol exemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.

Zie voor meer informatie de [netwerkverkeer beheren](#networktraffic) sectie.

## <a id="hdinsight-ports"></a> Vereiste poorten

Als u van plan bent met een netwerk **virtueel apparaat firewall** wilt beveiligen van het virtuele netwerk, moet u uitgaand verkeer op de volgende poorten toestaan:

* 53
* 443
* 1433
* 11000-11999
* 14000-14999

Zie voor een lijst met poorten voor specifieke services, de [poorten die worden gebruikt door de services van Hadoop op HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.

Zie voor meer informatie over firewallregels voor virtuele apparaten de [virtueel apparaat scenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) document.

## <a id="hdinsight-nsg"></a>Voorbeeld: netwerkbeveiligingsgroepen met HDInsight

De voorbeelden in deze sectie laten zien hoe netwerkbeveiliging groep regels maken die HDInsight om te communiceren met de Azure management-services toestaan. Voordat u de voorbeelden gebruiken, pas de IP-adressen zodat deze overeenkomen met de waarden voor de Azure-regio die u gebruikt. U vindt deze informatie in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

### <a name="azure-resource-management-template"></a>Azure Resource Management-sjabloon

De volgende Resource Management-sjabloon maakt u een virtueel netwerk dat binnenkomend verkeer wordt beperkt, maar wordt verkeer van de IP-adressen die door HDInsight vereist. Deze sjabloon maakt ook een HDInsight-cluster in het virtuele netwerk.

* [Een beveiligde virtuele Azure-netwerk en een HDInsight Hadoop-cluster implementeren](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> Wijzig de IP-adressen die in dit voorbeeld overeenkomt met de Azure-regio die u gebruikt. U vindt deze informatie in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

### <a name="azure-powershell"></a>Azure PowerShell

Gebruik de volgende PowerShell-script te maken van een virtueel netwerk waarmee binnenkomend verkeer beperkt en het verkeer van de IP-adressen voor de regio Noord-Europa.

> [!IMPORTANT]
> Wijzig de IP-adressen die in dit voorbeeld overeenkomt met de Azure-regio die u gebruikt. U vindt deze informatie in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule3" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule4" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule5" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule6" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
# Set the changes to the security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzureRmVirtualNetwork
```

> [!IMPORTANT]
> In dit voorbeeld laat zien hoe u regels voor binnenkomend verkeer op de vereiste IP-adressen toestaan toevoegt. Het bevat geen een regel voor het beperken van binnenkomende toegang uit andere bronnen.
>
> Het volgende voorbeeld toont hoe u SSH toegang via Internet:
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Azure-CLI

Gebruik de volgende stappen voor het maken van een virtueel netwerk dat binnenkomend verkeer wordt beperkt, maar wordt verkeer van de IP-adressen die door HDInsight vereist.

1. Gebruik de volgende opdracht voor het maken van een nieuwe netwerkbeveiligingsgroep met de naam `hdisecure`. Vervang **RESOURCEGROUPNAME** met de resourcegroep met de Azure Virtual Network. Vervang **locatie** met de locatie (regio) die in de groep is gemaakt.

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    Zodra de groep is gemaakt, krijgt u informatie over de nieuwe groep.

2. Gebruik de volgende regels toevoegen aan de nieuwe netwerkbeveiligingsgroep waarmee binnenkomende communicatie op poort 443 van de status en management-service van Azure HDInsight. Vervang **RESOURCEGROUPNAME** met de naam van de resourcegroep met de Azure Virtual Network.

    > [!IMPORTANT]
    > Wijzig de IP-adressen die in dit voorbeeld overeenkomt met de Azure-regio die u gebruikt. U vindt deze informatie in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Gebruik de volgende opdracht voor het ophalen van de unieke id voor deze netwerkbeveiligingsgroep:

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    Met deze opdracht retourneert een waarde die vergelijkbaar is met de volgende tekst:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Gebruik dubbele aanhalingstekens rond id in de opdracht als u niet de verwachte resultaten krijgt.

4. Gebruik de volgende opdracht toe te passen van de netwerkbeveiligingsgroep aan een subnet. Vervang de __GUID__ en __RESOURCEGROUPNAME__ waarden met de resultaten worden geretourneerd van de vorige stap. Vervang __VNETNAME__ en __SUBNETNAME__ met de virtuele-netwerknaam en het subnet-naam die u wilt maken.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Zodra deze opdracht is voltooid, kunt u HDInsight kunt installeren in het virtuele netwerk.

> [!IMPORTANT]
> Deze stappen alleen toegang tot de HDInsight-status en management service op de Azure-cloud geopend. Geen andere toegang hebben tot het HDInsight-cluster buiten het virtuele netwerk is geblokkeerd. Voor toegang van buiten het virtuele netwerk, moet u extra regels van de Netwerkbeveiligingsgroep toevoegen.
>
> Het volgende voorbeeld toont hoe u SSH toegang via Internet:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a> Voorbeeld: DNS-configuratie

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Naamomzetting tussen een virtueel netwerk en een netwerk verbonden on-premises

In dit voorbeeld wordt de volgende veronderstellingen:

* U hebt een virtueel netwerk van Azure die is verbonden met een on-premises netwerk via een VPN-gateway.

* De aangepaste DNS-server in het virtuele netwerk wordt Linux of Unix worden uitgevoerd als het besturingssysteem.

* [BIND](https://www.isc.org/downloads/bind/) is geïnstalleerd op de aangepaste DNS-server.

De aangepaste DNS-server in het virtuele netwerk:

1. Azure PowerShell of Azure CLI gebruiken het DNS-achtervoegsel van het virtuele netwerk te vinden:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. De aangepaste DNS-server voor het virtuele netwerk, gebruikt u de volgende tekst als de inhoud van de `/etc/bind/named.conf.local` bestand:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Vervang de `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` waarde met de DNS-achtervoegsel van het virtuele netwerk.

    Deze configuratie routeert alle DNS-aanvragen voor het DNS-achtervoegsel van het virtuele netwerk naar de Azure recursieve-resolver.

2. De aangepaste DNS-server voor het virtuele netwerk, gebruikt u de volgende tekst als de inhoud van de `/etc/bind/named.conf.options` bestand:

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * Vervang de `10.0.0.0/16` waarde met het IP-adresbereik van het virtuele netwerk. Deze vermelding kan name resolution aanvragen adressen binnen dit bereik.

    * Toevoegen van het IP-adresbereik van de on-premises netwerk naar de `acl goodclients { ... }` sectie.  vermelding kan aanvragen voor naamomzetting van resources in de on-premises netwerk.
    
    * Vervang de waarde `192.168.0.1` met het IP-adres van uw lokale DNS-server. Deze vermelding routeert DNS-aanvragen naar de lokale DNS-server.

3. Opnieuw opstarten binding voor het gebruik van de configuratie. Bijvoorbeeld `sudo service bind9 restart`.

4. Een voorwaardelijke doorstuurserver toevoegen aan de lokale DNS-server. Configureer de voorwaardelijke doorstuurserver voor het verzenden van aanvragen voor het DNS-achtervoegsel uit stap 1 voor de aangepaste DNS-server.

    > [!NOTE]
    > Raadpleeg de documentatie bij uw DNS-software voor specifieke informatie over het toevoegen van een voorwaardelijke doorstuurserver.

Na het voltooien van deze stappen kunt u verbinding maken met resources in een netwerk met behulp van de volledig gekwalificeerde domeinnamen (FQDN). U kunt nu HDInsight installeren in het virtuele netwerk.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Naamomzetting tussen de twee verbonden virtuele netwerken

In dit voorbeeld wordt de volgende veronderstellingen:

* U hebt twee virtuele netwerken van Azure die zijn verbonden via een VPN-gateway of -peering.

* De aangepaste DNS-server in beide netwerken wordt Linux of Unix worden uitgevoerd als het besturingssysteem.

* [BIND](https://www.isc.org/downloads/bind/) is geïnstalleerd op de aangepaste DNS-servers.

1. Azure PowerShell of Azure CLI gebruiken de DNS-achtervoegsel van beide virtuele netwerken vinden:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Gebruik de volgende tekst als de inhoud van de `/etc/bind/named.config.local` -bestand op de aangepaste DNS-server. Deze wijziging hebt aangebracht op de aangepaste DNS-server in beide virtuele netwerken.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    Vervang de `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` waarde met de DNS-achtervoegsel van de __andere__ virtueel netwerk. Deze vermelding routeert aanvragen voor het DNS-achtervoegsel van het externe netwerk naar de aangepaste DNS-server in dat netwerk.

3. Op de aangepaste DNS-servers in beide virtuele netwerken, gebruikt u de volgende tekst als de inhoud van de `/etc/bind/named.conf.options` bestand:

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * Vervang de `10.0.0.0/16` en `10.1.0.0/16` waarden met het IP-adresbereiken van uw virtuele netwerken. Deze vermelding kan resources in elk netwerk aanvragen van de DNS-servers.

    Alle aanvragen die niet voor de DNS-achtervoegsels van de virtuele netwerken (bijvoorbeeld microsoft.com) wordt uitgevoerd door de Azure recursieve-resolver.

4. Opnieuw opstarten binding voor het gebruik van de configuratie. Bijvoorbeeld: `sudo service bind9 restart` op beide DNS-servers.

Na het voltooien van deze stappen kunt u verbinding maken met resources in het virtuele netwerk met behulp van de volledig gekwalificeerde domeinnamen (FQDN). U kunt nu HDInsight installeren in het virtuele netwerk.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een voorbeeld van de end-to-end van de configuratie van HDInsight verbinding maken met een on-premises netwerk, [verbinding maken met HDInsight op een on-premises netwerk](./connect-on-premises-network.md).
* Zie voor het configureren van Hbase-clusters in virtuele netwerken in Azure, [maken HBase-clusters in HDInsight in Azure Virtual Network](hbase/apache-hbase-provision-vnet.md).
* Zie voor het configureren van HBase geo-replicatie [HBase-cluster-replicatie in virtuele netwerken in Azure instellen](hbase/apache-hbase-replication.md).
* Zie voor meer informatie over virtuele netwerken in Azure, de [Azure Virtual Network-overzicht](../virtual-network/virtual-networks-overview.md).

* Zie voor meer informatie over netwerkbeveiligingsgroepen [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md).

* Zie voor meer informatie over de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).
