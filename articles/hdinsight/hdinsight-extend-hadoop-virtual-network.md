---
title: HDInsight met Virtual Network - Azure uitbreiden
description: Meer informatie over het gebruik van Azure Virtual Network HDInsight verbinden met andere cloud-bronnen of bronnen in uw datacenter
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/29/2019
ms.openlocfilehash: a2d06cdbcc6ce995c55c858cb7a50a93ef6b3fb1
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58883561"
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Azure HDInsight met behulp van een Azure-netwerk uitbreiden

Informatie over het gebruik van HDInsight met een [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Met behulp van een Azure Virtual Network kunt de volgende scenario's:

* Verbinding maken met HDInsight rechtstreeks vanuit een on-premises netwerk.

* Verbinding maken met HDInsight gegevens opslaat in een Azure-netwerk.

* Rechtstreeks toegang hebben tot [Apache Hadoop](https://hadoop.apache.org/) services die niet beschikbaar openbaar via internet zijn. Bijvoorbeeld, [Apache Kafka](https://kafka.apache.org/) API's of de [Apache HBase](https://hbase.apache.org/) Java-API.

> [!IMPORTANT]  
> Na 28 februari 2019, wordt de netwerkbronnen (zoals NIC's, LBs, enzovoort) voor nieuwe clusters die zijn gemaakt in een VNET in dezelfde resourcegroep voor HDInsight-cluster worden ingericht. Eerder zijn deze resources ingericht in de resourcegroep van de VNET. Er is geen wijziging aan de huidige actieve clusters en deze clusters die zijn gemaakt zonder een VNET.

## <a name="prerequisites-for-code-samples-and-examples"></a>Vereisten voor codevoorbeelden en voorbeelden

* Een goed begrip van TCP/IP-netwerken. Als u niet bekend met TCP/IP-netwerken bent, moet u samenwerken met iemand die dit voordat u wijzigingen in productienetwerken.
* Als u PowerShell gebruikt, moet u de [AZ Module](https://docs.microsoft.com/powershell/azure/overview).
* Als die u wilt gebruiken Azure CLI en u nog niet hebt geïnstalleerd het, raadpleegt u [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

> [!IMPORTANT]  
> Als u zoekt met stapsgewijze instructies voor het verbinden van HDInsight met uw on-premises netwerk met behulp van een Azure Virtual Network, raadpleegt u de [HDInsight verbinden met uw on-premises netwerk](connect-on-premises-network.md) document.

## <a name="planning"></a>Planning

Hier volgen de vragen die u beantwoorden moet bij het plannen voor het installeren van HDInsight in een virtueel netwerk:

* Moet u voor het installeren van HDInsight in een bestaand virtueel netwerk? Of maakt u een nieuw netwerk?

    Als u van een bestaand virtueel netwerk gebruikmaakt, moet u mogelijk om te wijzigen van de netwerkconfiguratie voordat u kunt ook HDInsight installeren. Zie voor meer informatie de [HDInsight toevoegen aan een bestaand virtueel netwerk](#existingvnet) sectie.

* Wilt u verbinding maken met het virtuele netwerk met HDInsight op een ander virtueel netwerk of uw on-premises netwerk?

    Als u wilt eenvoudig werken met resources voor netwerken, moet u een aangepaste DNS-server maken en configureren van DNS-doorsturen. Zie voor meer informatie de [meerdere netwerken met elkaar verbinden](#multinet) sectie.

* Wilt u binnenkomend of uitgaand verkeer naar HDInsight beperken/omleiden?

    HDInsight moet hebben onbeperkte communicatie met specifieke IP-adressen in het Azure-datacentrum. Er zijn ook verschillende poorten voor clientcommunicatie via firewalls moeten worden toegestaan. Zie voor meer informatie de [netwerkverkeer beheren](#networktraffic) sectie.

## <a id="existingvnet"></a>HDInsight toevoegen aan een bestaand virtueel netwerk

Gebruik de stappen in deze sectie om te ontdekken hoe u een nieuw HDInsight toevoegt aan een bestaand Virtueelnetwerk van Azure.

> [!NOTE]  
> U kunt een bestaand HDInsight-cluster in een virtueel netwerk niet toevoegen.

1. U gebruikt een klassieke of Resource Manager-implementatiemodel voor het virtuele netwerk?

    HDInsight 3.4 en hoger is vereist voor een virtueel netwerk van Resource Manager. Eerdere versies van HDInsight vereist een klassiek virtueel netwerk.

    Als uw bestaande netwerk een klassiek virtueel netwerk is, moet u een virtueel netwerk van Resource Manager maken en sluit vervolgens de twee. [Klassieke VNets verbinden met nieuwe VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Eenmaal verbonden, wordt HDInsight geïnstalleerd in het netwerk van Resource Manager kan communiceren met resources in het klassieke netwerk.

2. Geforceerde tunneling gebruiken Geforceerde tunneling is een subnetinstelling die ervoor zorgt het uitgaande internetverkeer met een apparaat voor controle dat en logboekregistratie. HDInsight biedt geen ondersteuning voor geforceerde tunneling. Verwijder geforceerde tunnels zijn voordat u HDInsight implementeert in een bestaand subnet of maakt u een nieuw subnet met geen geforceerde tunnels voor HDInsight.

3. Moet u netwerkbeveiligingsgroepen, de gebruiker gedefinieerde routes of de virtuele netwerkapparaten gebruiken om te beperken het verkeer naar of uit het virtuele netwerk?

    Als een beheerde service vereist HDInsight onbeperkte toegang tot verschillende IP-adressen in het Azure-datacentrum. Als u wilt toestaan dat communicatie met deze IP-adressen, werken alle bestaande netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes.
    
    HDInsight als host fungeert voor meerdere services, die verschillende poorten gebruiken. Verkeer naar deze poorten niet blokkeren. Zie de sectie beveiliging voor een lijst met poorten om toe te staan via de firewalls virtueel apparaat.
    
    Als u uw bestaande beveiligingsconfiguratie zoekt, gebruikt u de volgende Azure PowerShell of Azure CLI-opdrachten:

    * Netwerkbeveiligingsgroepen

        Vervang `RESOURCEGROUP` met de naam van de resourcegroep waarin het virtuele netwerk bevat en voer vervolgens de opdracht:
    
        ```powershell
        Get-AzNetworkSecurityGroup -ResourceGroupName  "RESOURCEGROUP"
        ```
    
        ```azurecli
        az network nsg list --resource-group RESOURCEGROUP
        ```

        Zie voor meer informatie de [problemen met netwerkbeveiligingsgroepen oplossen](../virtual-network/diagnose-network-traffic-filter-problem.md) document.

        > [!IMPORTANT]  
        > Regels voor netwerkbeveiligingsgroepen worden toegepast in volgorde op basis van Regelprioriteit. De eerste regel die overeenkomt met het patroon verkeer wordt toegepast en geen andere voor dat verkeer worden toegepast. Volgorde de regels van de meest strikte op minimaal. Zie voor meer informatie de [netwerkverkeer filteren met netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) document.

    * Door de gebruiker gedefinieerde routes

        Vervang `RESOURCEGROUP` met de naam van de resourcegroep waarin het virtuele netwerk bevat en voer vervolgens de opdracht:

        ```powershell
        Get-AzRouteTable -ResourceGroupName "RESOURCEGROUP"
        ```

        ```azurecli
        az network route-table list --resource-group RESOURCEGROUP
        ```

        Zie voor meer informatie de [problemen met routes oplossen](../virtual-network/diagnose-network-routing-problem.md) document.

4. Een HDInsight-cluster maken en selecteer het Azure-netwerk tijdens de configuratie. Gebruik de stappen in de volgende documenten om te begrijpen van het proces voor het maken van cluster:

    * [HDInsight met behulp van de Azure portal maken](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Maken van HDInsight met behulp van Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Maken van HDInsight met behulp van de klassieke Azure-CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [HDInsight met behulp van een Azure Resource Manager-sjabloon maken](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

   > [!IMPORTANT]  
   > HDInsight toe te voegen aan een virtueel netwerk is een optionele configuratie-stap. Zorg ervoor dat het virtuele netwerk selecteert wanneer het cluster te configureren.

## <a id="multinet"></a>Meerdere netwerken met elkaar verbinden

De grootste uitdaging met een configuratie met meerdere netwerken is de naamomzetting tussen de netwerken.

Azure biedt naamomzetting voor Azure-services die zijn geïnstalleerd in een virtueel netwerk. Deze ingebouwde naamomzetting kan HDInsight verbinden met de volgende bronnen met behulp van een volledig gekwalificeerde domeinnaam (FQDN):

* Een resource die beschikbaar is op het internet. Bijvoorbeeld microsoft.com, windowsupdate.com.

* Een resource die zich in hetzelfde Azure virtuele netwerk, met behulp van de __interne DNS-naam__ van de resource. Bijvoorbeeld, wanneer u de standaard-naamomzetting, zijn de volgende voorbeeld van de interne DNS-namen die aan HDInsight worker-knooppunten zijn toegewezen:

  * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
  * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Deze beide knooppunten kunnen communiceren rechtstreeks met elkaar en andere knooppunten in HDInsight, met behulp van de interne DNS-namen.

De standaard-naamomzetting is __niet__ HDInsight om op te lossen de namen van resources in netwerken die zijn gekoppeld aan het virtuele netwerk toestaan. Het is bijvoorbeeld gebruikelijk voor uw on-premises netwerk koppelen aan het virtuele netwerk. Met alleen de standaard de naam van oplossing HDInsight kan geen toegang krijgen tot bronnen in de on-premises netwerk met de naam. Het omgekeerde geldt ook, resources in uw on-premises netwerk heeft geen toegang tot resources in het virtuele netwerk met de naam.

> [!WARNING]  
> U moet de aangepaste DNS-server maken en configureren van het virtuele netwerk wilt gebruiken voor het HDInsight-cluster te maken.

Om in te schakelen naamomzetting tussen het virtuele netwerk en de resources in een domein netwerken, moet u de volgende acties uitvoeren:

1. Maak een aangepaste DNS-server in de Azure-netwerk waar u van plan bent voor het installeren van HDInsight.

2. Het virtuele netwerk voor het gebruik van de aangepaste DNS-server configureren.

3. Zoek dat de Azure DNS-achtervoegsel voor het virtuele netwerk toegewezen. Deze waarde is vergelijkbaar met `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Zie voor meer informatie over het zoeken van de DNS-achtervoegsel in de [voorbeeld: Aangepaste DNS](#example-dns) sectie.

4. Doorsturen van tussen de DNS-servers configureren. De configuratie, is afhankelijk van het type van het externe netwerk.

   * Als het externe netwerk een on-premises netwerk is, configureren van DNS als volgt:
        
     * __Aangepaste DNS__ (in het virtuele netwerk):

         * Aanvragen doorsturen voor de DNS-achtervoegsel van het virtuele netwerk naar de conflictoplosser Azure recursieve (168.63.129.16). Azure regelt aanvragen voor bronnen in het virtuele netwerk

         * Alle andere verzoeken naar de lokale DNS-server doorsturen. De on-premises DNS verwerkt alle andere aanvragen voor naamomzetting, zelfs aanvragen voor resources op internet, zoals Microsoft.com.

     * __On-premises DNS__: Aanvragen doorsturen voor het virtuele netwerk DNS-achtervoegsel voor de aangepaste DNS-server. De aangepaste DNS-server stuurt vervolgens door naar de Azure recursieve naamomzetting.

       Deze configuratie routes aanvragen voor FQDN-namen die het DNS-achtervoegsel van het virtuele netwerk naar het aangepaste DNS-server bevatten. Alle andere aanvragen (zelfs voor openbare internet-adressen) worden verwerkt door de on-premises DNS-server.

   * Als het externe netwerk een ander Virtueelnetwerk van Azure is, configureren van DNS als volgt:

     * __Aangepaste DNS__ (in elk virtueel netwerk):

         * Aanvragen voor de DNS-achtervoegsel van de virtuele netwerken worden doorgestuurd naar de aangepaste DNS-servers. De DNS-server in elk virtueel netwerk is verantwoordelijk voor het oplossen van resources in het netwerk.

         * Alle andere aanvragen doorsturen naar de Azure recursieve naamomzetting. De recursieve naamomzetting is verantwoordelijk voor het omzetten van lokale en resources op internet.

       De DNS-server voor elk netwerk stuurt aanvragen door naar de andere, op basis van DNS-achtervoegsel. Andere aanvragen worden omgezet met behulp van de Azure recursieve naamomzetting.

     Zie voor een voorbeeld van elke configuratie, de [voorbeeld: Aangepaste DNS](#example-dns) sectie.

Zie voor meer informatie de [naamomzetting voor VM's en Rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.

## <a name="directly-connect-to-apache-hadoop-services"></a>Rechtstreeks verbinding maken met Apache Hadoop-services

U kunt verbinding maken met het cluster op `https://CLUSTERNAME.azurehdinsight.net`. Dit adres maakt gebruik van een openbaar IP-adres, die mogelijk niet bereikbaar als u nsg's hebt gebruikt om te beperken van inkomend verkeer van internet. Bovendien wanneer u het cluster in een VNet implementeert u het kunt openen met behulp van de persoonlijke eindpunt `https://CLUSTERNAME-int.azurehdinsight.net`. Dit eindpunt wordt omgezet naar een privé IP-adres binnen het VNet voor toegang tot het cluster.

Als u wilt verbinding maken met Apache Ambari en andere webpagina's met het virtuele netwerk, gebruikt u de volgende stappen uit:

1. Voor het detecteren van de interne volledig gekwalificeerde domeinnamen (FQDN) van de HDInsight-clusterknooppunten, moet u een van de volgende methoden gebruiken:

    Vervang `RESOURCEGROUP` met de naam van de resourcegroep waarin het virtuele netwerk bevat en voer vervolgens de opdracht:

    ```powershell
    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP" | where-object {$_.Name -like "*node*"}

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
    az network nic list --resource-group RESOURCEGROUP --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    In de lijst met knooppunten die zijn geretourneerd, de FQDN-naam vinden voor de hoofdknooppunten en de FQDN's gebruiken voor verbinding met Ambari en andere web-services. Gebruik bijvoorbeeld `http://<headnode-fqdn>:8080` voor toegang tot de Ambari.

    > [!IMPORTANT]  
    > Sommige services die worden gehost op de hoofdknooppunten zijn alleen actief is op één knooppunt tegelijk. Als u probeert toegang tot een service op één hoofdknooppunt en er een 404-fout retourneert, kunt u overschakelen naar het hoofdknooppunt.

2. Zie het vaststellen van het knooppunt en de poort die een service is beschikbaar in de [poorten die worden gebruikt door de services van Hadoop op HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.

## <a id="networktraffic"></a> Beheren van netwerkverkeer

Het netwerkverkeer in een virtuele Azure-netwerken kan worden beheerd met behulp van de volgende methoden:

* **Netwerkbeveiligingsgroepen** (NSG) kunt u binnenkomend en uitgaand verkeer met het netwerk te filteren. Zie voor meer informatie de [netwerkverkeer filteren met netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) document.

    > [!WARNING]  
    > HDInsight biedt geen ondersteuning voor het beperken van uitgaand verkeer. Al het uitgaande verkeer moet worden toegestaan.

* **Gebruiker gedefinieerde routes** (UDR) definiëren hoe verkeer stroomt tussen resources in het netwerk. Zie voor meer informatie de [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md) document.

* **Virtuele netwerkapparaten** repliceren van de functionaliteit van apparaten, zoals firewalls en routers. Zie voor meer informatie de [netwerkapparaten](https://azure.microsoft.com/solutions/network-appliances) document.

Als een beheerde service HDInsight vereist onbeperkte toegang tot de HDInsight-status en beheer van services voor binnenkomende en uitgaande verkeer van het VNET. Wanneer u nsg's en udr's, moet u ervoor zorgen dat deze services nog steeds met HDInsight-cluster communiceren kunnen.

### <a id="hdinsight-ip"></a> HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes

Als u van plan over het gebruik van bent **netwerkbeveiligingsgroepen** of **gebruiker gedefinieerde routes** netwerkverkeer beheren, voert u de volgende acties voor de installatie van HDInsight:

1. Identificeer de Azure-regio die u wilt gebruiken voor HDInsight.

2. Identificeer de IP-adressen vereist voor HDInsight. Zie voor meer informatie de [IP-adressen die zijn vereist voor HDInsight](#hdinsight-ip) sectie.

3. Maak of wijzig de netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes voor het subnet dat u van plan bent voor het installeren van HDInsight in.

    * __Netwerkbeveiligingsgroepen__: toestaan __inkomende__ verkeer op poort __443__ uit het IP-adressen. Dit zorgt ervoor dat HDI-beheerservices van VNET buiten het cluster kunnen bereiken.
    * __Gebruiker gedefinieerde routes__: Als u van plan bent te gebruiken van udr's, een route voor elk IP-adres en stel de __volgende hoptype__ naar __Internet__. U moet ook andere uitgaand verkeer toestaan van het VNET zonder beperking. Bijvoorbeeld, kunt u al het andere verkeer doorsturen naar uw Azure firewall of netwerk virtueel apparaat (die wordt gehost in Azure) voor bewakingsdoeleinden, maar het uitgaande verkeer mag niet worden geblokkeerd.

Zie voor meer informatie over netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes, de volgende documentatie:

* [Netwerkbeveiligingsgroep](../virtual-network/security-overview.md)

* [Door de gebruiker gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling-to-on-premise"></a>Geforceerde tunneling on-premises

Geforceerde tunneling vindt een door de gebruiker gedefinieerde routering configuratie waarin al het verkeer van een subnet naar een specifieke netwerk- of -locatie, zoals uw on-premises netwerk wordt geforceerd. HDInsight biedt __niet__ ondersteuning geforceerde tunneling naar de on-premises netwerken. Als u gebruikmaakt van Azure-Firewall of een virtueel netwerkapparaat die wordt gehost in Azure, kunt u udr's gebruiken voor het verkeer te routeren voor controledoeleinden en al het uitgaande verkeer toestaan.

## <a id="hdinsight-ip"></a> Vereiste IP-adressen

> [!IMPORTANT]  
> De Azure status en management-services moet kunnen communiceren met HDInsight. Als u netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes, verkeer van de IP-adressen voor deze services te bereiken HDInsight toestaan.
>
> Als u geen netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes voor het beheren van verkeer gebruikt, kunt u deze sectie overslaan.

Als u netwerkbeveiligingsgroepen gebruikt, moet u verkeer van de Azure status en management-services tot HDInsight-clusters op poort 443 toestaan. U moet ook verkeer tussen virtuele machines binnen het subnet toestaat. Gebruik de volgende stappen uit om te vinden van de IP-adressen die moeten worden toegestaan:

1. U moet altijd verkeer van de volgende IP-adressen toestaan:

    | IP-adres van bron | Doelpoort | Richting |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | Inkomend |
    | 23.99.5.239 | 443 | Inkomend |
    | 168.61.48.131 | 443 | Inkomend |
    | 138.91.141.162 | 443 | Inkomend |

2. Als uw HDInsight-cluster zich in een van de volgende regio's, moet u verkeer van de IP-adressen die worden vermeld voor de regio toestaan:

    > [!IMPORTANT]  
    > Als de Azure-regio u niet wordt vermeld, klikt u vervolgens alleen de vier IP-adressen gebruiken uit stap 1.

    | Land/regio | Regio | Toegestane bron-IP-adressen | Doelpoort toegestaan | Richting |
    | ---- | ---- | ---- | ---- | ----- |
    | Azië | Azië - oost | 23.102.235.122</br>52.175.38.134 | 443 | Inkomend |
    | &nbsp; | Azië - zuidoost | 13.76.245.160</br>13.76.136.249 | 443 | Inkomend |
    | Australië | Australië - oost | 104.210.84.115</br>13.75.152.195 | 443 | Inkomend |
    | &nbsp; | Australië - zuidoost | 13.77.2.56</br>13.77.2.94 | 443 | Inkomend |
    | Brazilië | Brazilië - zuid | 191.235.84.104</br>191.235.87.113 | 443 | Inkomend |
    | Canada | Canada - oost | 52.229.127.96</br>52.229.123.172 | 443 | Inkomend |
    | &nbsp; | Canada - midden | 52.228.37.66</br>52.228.45.222 | 443 | Inkomend |
    | China | China - noord | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | 443 | Inkomend |
    | &nbsp; | China East | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | 443 | Inkomend |
    | &nbsp; | China - noord 2 | 40.73.37.141</br>40.73.38.172 | 443 | Inkomend |
    | &nbsp; | China - oost 2 | 139.217.227.106</br>139.217.228.187 | 443 | Inkomend |
    | Europa | Europa - noord | 52.164.210.96</br>13.74.153.132 | 443 | Inkomend |
    | &nbsp; | Europa -west| 52.166.243.90</br>52.174.36.244 | 443 | Inkomend |
    | Frankrijk | Frankrijk - centraal| 20.188.39.64</br>40.89.157.135 | 443 | Inkomend |
    | Duitsland | Duitsland - centraal | 51.4.146.68</br>51.4.146.80 | 443 | Inkomend |
    | &nbsp; | Duitsland - noordoost | 51.5.150.132</br>51.5.144.101 | 443 | Inkomend |
    | India | India - centraal | 52.172.153.209</br>52.172.152.49 | 443 | Inkomend |
    | &nbsp; | India - zuid | 104.211.223.67<br/>104.211.216.210 | 443 | Inkomend |
    | Japan | Japan - oost | 13.78.125.90</br>13.78.89.60 | 443 | Inkomend |
    | &nbsp; | Japan - west | 40.74.125.69</br>138.91.29.150 | 443 | Inkomend |
    | Korea | Korea - centraal | 52.231.39.142</br>52.231.36.209 | 433 | Inkomend |
    | &nbsp; | Korea - zuid | 52.231.203.16</br>52.231.205.214 | 443 | Inkomend
    | Verenigd Koninkrijk | Verenigd Koninkrijk West | 51.141.13.110</br>51.141.7.20 | 443 | Inkomend |
    | &nbsp; | Verenigd Koninkrijk Zuid | 51.140.47.39</br>51.140.52.16 | 443 | Inkomend |
    | Verenigde Staten | US - centraal | 13.67.223.215</br>40.86.83.253 | 443 | Inkomend |
    | &nbsp; | US - oost | 13.82.225.233</br>40.71.175.99 | 443 | Inkomend |
    | &nbsp; | US - noord-centraal | 157.56.8.38</br>157.55.213.99 | 443 | Inkomend |
    | &nbsp; | US - west-centraal | 52.161.23.15</br>52.161.10.167 | 443 | Inkomend |
    | &nbsp; | US - west | 13.64.254.98</br>23.101.196.19 | 443 | Inkomend |
    | &nbsp; | US - west 2 | 52.175.211.210</br>52.175.222.222 | 443 | Inkomend |

    Zie voor informatie over de IP-adressen te gebruiken voor Azure Government, de [Azure Government Intelligence en analyse](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) document.

3. U moet ook zodat toegang vanaf __168.63.129.16__. Dit adres is van het Azure-recursieve naamomzetting. Zie voor meer informatie de [naamomzetting voor virtuele machines en de rol exemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.

Zie voor meer informatie de [netwerkverkeer beheren](#networktraffic) sectie.

Als u de gebruiker gedefinieerde routes(UDRs) gebruikt, moet u een route opgeven en toegestaan uitgaand verkeer van het VNET naar de bovenstaande IP-adressen met de volgende hop is ingesteld op 'Internet'.
    
## <a id="hdinsight-ports"></a> Vereiste poorten

Als u van plan over het gebruik van bent een **firewall** en toegang tot het cluster uit buiten op bepaalde poorten, moet u mogelijk verkeer via deze poorten die nodig zijn voor uw scenario. Standaard is geen speciale zwarte lijst plaatsen van poorten nodig, zolang het azure managementverkeer wordt uitgelegd in de vorige sectie is toegestaan tot een cluster op poort 443.

Zie voor een lijst met poorten voor specifieke services, de [poorten die worden gebruikt door de services van Apache Hadoop op HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.

Zie voor meer informatie over firewall-regels voor virtuele apparaten, de [virtueel apparaat scenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) document.

## <a id="hdinsight-nsg"></a>Voorbeeld: netwerkbeveiligingsgroepen met HDInsight

De voorbeelden in deze sectie laten zien hoe u regels waarmee HDInsight om te communiceren met de Azure management-services voor netwerkbeveiliging maken. Voordat u de voorbeelden, pas de IP-adressen zodat deze overeenkomt met die van de Azure-regio die u gebruikt. U kunt deze informatie vinden in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

### <a name="azure-resource-management-template"></a>Azure Resource Manager-sjabloon

De volgende Resource Manager-sjabloon maakt een virtueel netwerk die binnenkomend verkeer wordt beperkt, maar het verkeer van de IP-adressen vereist voor HDInsight maakt. Deze sjabloon maakt ook een HDInsight-cluster in het virtuele netwerk.

* [Een beveiligde Virtueelnetwerk van Azure en een HDInsight Hadoop-cluster implementeren](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]  
> De IP-adressen in dit voorbeeld gebruikt zodat deze overeenkomen met de Azure-regio u wijzigen. U kunt deze informatie vinden in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

### <a name="azure-powershell"></a>Azure PowerShell

Gebruik de volgende PowerShell-script te maken van een virtueel netwerk die binnenkomend verkeer wordt beperkt en waarmee verkeer van de IP-adressen voor de regio Noord-Europa.

> [!IMPORTANT]  
> De IP-adressen in dit voorbeeld gebruikt zodat deze overeenkomen met de Azure-regio u wijzigen. U kunt deze informatie vinden in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzNetworkSecurityRuleConfig `
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
    | Add-AzNetworkSecurityRuleConfig `
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
    | Add-AzNetworkSecurityRuleConfig `
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
    | Add-AzNetworkSecurityRuleConfig `
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
    | Add-AzNetworkSecurityRuleConfig `
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
    | Add-AzNetworkSecurityRuleConfig `
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
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzVirtualNetwork
```

> [!IMPORTANT]  
> In dit voorbeeld ziet u hoe u regels voor het toestaan van binnenkomend verkeer op de vereiste IP-adressen toevoegen. Het bevat een regel voor het beperken van binnenkomende toegang uit andere bronnen.
>
> Het volgende voorbeeld ziet u hoe u SSH-toegang via Internet inschakelen:
>
> ```powershell
> Add-AzNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Azure-CLI

Gebruik de volgende stappen uit om te maken van een virtueel netwerk dat inkomend verkeer wordt beperkt, maar het verkeer van de IP-adressen vereist voor HDInsight maakt.

1. Gebruik de volgende opdracht om te maken van een nieuwe netwerkbeveiligingsgroep met de naam `hdisecure`. Vervang `RESOURCEGROUP` met de resourcegroep waarin het Virtueelnetwerk van Azure. Vervang `LOCATION` door de locatie (regio) die in de groep is gemaakt.

    ```azurecli
    az network nsg create -g RESOURCEGROUP -n hdisecure -l LOCATION
    ```

    Nadat de groep is gemaakt, ontvangt u informatie over de nieuwe groep.

2. Gebruik de volgende regels toevoegen aan de nieuwe netwerkbeveiligingsgroep waarmee binnenkomende communicatie op poort 443 van de Azure HDInsight-status en management-service. Vervang `RESOURCEGROUP` met de naam van de resourcegroep waarin het Virtueelnetwerk van Azure.

    > [!IMPORTANT]  
    > De IP-adressen in dit voorbeeld gebruikt zodat deze overeenkomen met de Azure-regio u wijzigen. U kunt deze informatie vinden in de [HDInsight met netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes](#hdinsight-ip) sectie.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule3 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule4 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule6 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Als u wilt ophalen van de unieke id voor deze netwerkbeveiligingsgroep, gebruik de volgende opdracht:

    ```azurecli
    az network nsg show -g RESOURCEGROUP -n hdisecure --query 'id'
    ```

    Met deze opdracht retourneert een waarde die vergelijkbaar is met de volgende tekst:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Gebruik dubbele aanhalingstekens rond `id` in de opdracht als u niet de verwachte resultaten krijgt.

4. Gebruik de volgende opdracht om toe te passen van de netwerkbeveiligingsgroep aan een subnet. Vervang de `GUID` en `RESOURCEGROUP` met de resultaten geretourneerd door de vorige stap. Vervang `VNETNAME` en `SUBNETNAME` met de naam van virtueel netwerk en subnetnaam die u wilt maken.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUP --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Als deze opdracht is voltooid, kunt u HDInsight installeren in het Virtueelnetwerk.

> [!IMPORTANT]  
> Deze stappen is alleen geopend voor toegang tot de HDInsight status en management-service op de Azure-cloud. Alle andere toegang tot het HDInsight-cluster op basis van buiten het Virtueelnetwerk is geblokkeerd. Als u wilt inschakelen toegang van buiten het virtuele netwerk, moet u extra netwerkbeveiligingsgroepsregels toevoegen.
>
> Het volgende voorbeeld ziet u hoe u SSH-toegang via Internet inschakelen:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a> Voorbeeld: DNS-configuratie

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Naamomzetting tussen een virtueel netwerk en een netwerk verbonden on-premises

In dit voorbeeld wordt de volgende veronderstellingen:

* U hebt een Azure-netwerk dat is verbonden met een on-premises netwerk via een VPN-gateway.

* De aangepaste DNS-server in het virtuele netwerk wordt Linux of Unix worden uitgevoerd als het besturingssysteem.

* [BIND](https://www.isc.org/downloads/bind/) is geïnstalleerd op de aangepaste DNS-server.

De aangepaste DNS-server in het virtuele netwerk:

1. Azure PowerShell of Azure CLI gebruiken voor het vinden van de DNS-achtervoegsel van het virtuele netwerk:

    Vervang `RESOURCEGROUP` met de naam van de resourcegroep waarin het virtuele netwerk bevat en voer vervolgens de opdracht:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Op de aangepaste DNS-server voor het virtuele netwerk, gebruikt u de volgende tekst als de inhoud van de `/etc/bind/named.conf.local` bestand:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Vervang de `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` waarde met de DNS-achtervoegsel van het virtuele netwerk.

    Deze configuratie stuurt alle DNS-aanvragen voor de DNS-achtervoegsel van het virtuele netwerk naar de Azure recursieve naamomzetting.

2. Op de aangepaste DNS-server voor het virtuele netwerk, gebruikt u de volgende tekst als de inhoud van de `/etc/bind/named.conf.options` bestand:

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
    
    * Vervang de `10.0.0.0/16` waarde met het IP-adresbereik van het virtuele netwerk. Deze vermelding kunt de naam van het probleem zou moeten aanvragen adressen binnen dit bereik.

    * Voeg het IP-adresbereik van de on-premises netwerk naar de `acl goodclients { ... }` sectie.  vermelding kan aanvragen voor naamomzetting van resources in de on-premises netwerk.
    
    * Vervang de waarde `192.168.0.1` met het IP-adres van uw on-premises DNS-server. Deze vermelding stuurt alle andere DNS-aanvragen naar de lokale DNS-server.

3. Opnieuw opstarten binding voor het gebruik van de configuratie. Bijvoorbeeld `sudo service bind9 restart`.

4. Een voorwaardelijke doorstuurserver toevoegen aan de lokale DNS-server. Configureer de voorwaardelijke doorstuurserver voor het verzenden van aanvragen voor het DNS-achtervoegsel uit stap 1 voor de aangepaste DNS-server.

    > [!NOTE]  
    > Raadpleeg de documentatie voor uw DNS-software voor meer informatie over het toevoegen van een voorwaardelijke doorstuurserver.

Na het voltooien van deze stappen kunt u verbinding maken met resources in beide netwerken met behulp van de volledig gekwalificeerde domeinnamen (FQDN). U kunt nu HDInsight installeren in het virtuele netwerk.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Naamomzetting tussen de twee verbonden virtuele netwerken

In dit voorbeeld wordt de volgende veronderstellingen:

* U hebt twee Azure virtuele netwerken die zijn verbonden via een VPN-gateway of -peering.

* De aangepaste DNS-server in beide netwerken wordt Linux of Unix worden uitgevoerd als het besturingssysteem.

* [BIND](https://www.isc.org/downloads/bind/) is geïnstalleerd op de aangepaste DNS-servers.

1. Azure PowerShell of Azure CLI gebruiken voor het vinden van de DNS-achtervoegsel van beide virtuele netwerken:

    Vervang `RESOURCEGROUP` met de naam van de resourcegroep waarin het virtuele netwerk bevat en voer vervolgens de opdracht:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Gebruik de volgende tekst als de inhoud van de `/etc/bind/named.config.local` bestand op de aangepaste DNS-server. Deze wijziging hebt aangebracht op de aangepaste DNS-server in beide virtuele netwerken.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    Vervang de `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` waarde met de DNS-achtervoegsel van de __andere__ virtueel netwerk. Deze vermelding worden aanvragen voor de DNS-achtervoegsel van het externe netwerk gerouteerd naar de aangepaste DNS-server in dat netwerk.

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
    
   Vervang de `10.0.0.0/16` en `10.1.0.0/16` waarden met IP-adresbereiken van uw virtuele netwerken. Deze vermelding kan resources in elk netwerk om aan te vragen van de DNS-servers.

    Alle aanvragen die niet voor de DNS-achtervoegsels van de virtuele netwerken (bijvoorbeeld microsoft.com) wordt verwerkt door de Azure recursieve naamomzetting.

4. Opnieuw opstarten binding voor het gebruik van de configuratie. Bijvoorbeeld, `sudo service bind9 restart` op beide DNS-servers.

Na het voltooien van deze stappen kunt u verbinding maken met resources in het virtuele netwerk met behulp van de volledig gekwalificeerde domeinnamen (FQDN). U kunt nu HDInsight installeren in het virtuele netwerk.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een end-to-end-voorbeeld van het configureren van HDInsight verbinden met een on-premises netwerk, [verbinding maken met HDInsight op een on-premises netwerk](./connect-on-premises-network.md).
* Zie voor het configureren van Apache Hbase-clusters in virtuele netwerken van Azure, [maakt Apache HBase-clusters in HDInsight in Azure Virtual Network](hbase/apache-hbase-provision-vnet.md).
* Zie voor het configureren van Apache HBase-geo-replicatie, [Apache HBase-cluster-replicatie in virtuele Azure-netwerken instellen](hbase/apache-hbase-replication.md).
* Zie voor meer informatie over virtuele netwerken van Azure, de [overzicht van Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

* Zie voor meer informatie over netwerkbeveiligingsgroepen [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).

* Zie voor meer informatie over de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).
