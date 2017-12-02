---
title: HDInsight verbinden met uw on-premises netwerk - of Azure HDInsight | Microsoft Docs
description: Informatie over het maken van een HDInsight-cluster in een Azure-netwerk en maak verbinding met uw lokale netwerk. Informatie over het configureren van naamomzetting tussen HDInsight en uw on-premises netwerk via een aangepaste DNS-server.
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: 5e9951132ebb46d1053fbf62528b43e82cc76d2e
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="connect-hdinsight-to-your-on-premise-network"></a>HDInsight verbinden met uw on-premises-netwerk

Informatie over het HDInsight verbinding met uw on-premises netwerk met behulp van Azure Virtual Networks en een VPN-gateway. Dit document bevat de planningsinformatie over:

* HDInsight gebruikt in een Azure-netwerk die verbinding met uw on-premises netwerk maakt.

* Configureren van DNS-naamomzetting tussen het virtuele netwerk en uw on-premises netwerk.

* Configureren van netwerkbeveiligingsgroepen internettoegang tot HDInsight te beperken.

* Poorten die zijn geleverd door HDInsight op het virtuele netwerk.

## <a name="create-the-virtual-network-configuration"></a>De configuratie van het virtuele netwerk maken

Gebruik de volgende documenten voor meer informatie over het maken van een virtueel netwerk van Azure die is verbonden met uw on-premises netwerk:
    
* [Azure Portal gebruiken](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Azure PowerShell gebruiken](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Azure CLI gebruiken](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Naamomzetting configureren

Als u wilt toestaan HDInsight en bronnen in het gekoppelde netwerk om te communiceren met de naam, moet u de volgende acties uitvoeren:

* Maak een aangepaste DNS-server in het virtuele netwerk van Azure.

* Het virtuele netwerk voor het gebruik van de aangepaste DNS-server in plaats van de standaard Azure recursieve naamomzetting configureren.

* Configureren van forwarding tussen de aangepaste DNS-server en de lokale DNS-server.

Deze configuratie kunt het volgende gedrag:

* Aanvragen voor de volledig gekwalificeerde domeinnamen waarvoor het DNS-achtervoegsel __voor het virtuele netwerk__ worden doorgestuurd naar de aangepaste DNS-server. De aangepaste DNS-server stuurt vervolgens deze aanvragen aan de recursieve Resolver van Azure, waardoor het IP-adres geretourneerd.

* Alle andere aanvragen worden doorgestuurd naar de lokale DNS-server. Zelfs aanvragen voor het openbare internet-bronnen zoals microsoft.com worden doorgestuurd naar de lokale DNS-server voor naamomzetting.

Groen regels zijn in het volgende diagram aanvragen voor bronnen die op de DNS-achtervoegsel van het virtuele netwerk eindigen. Blauwe lijnen zijn aanvragen voor bronnen in de on-premises netwerk of op het openbare internet.

![Diagram van hoe de DNS-aanvragen worden verwerkt in de configuratie die in dit document gebruikt](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Maken van een aangepaste DNS-server

> [!IMPORTANT]
> U moet maken en configureren van de DNS-server voordat u HDInsight installeert in het virtuele netwerk.

Maken van een Linux-VM die gebruikmaakt van de [binden](https://www.isc.org/downloads/bind/) DNS-software, gebruik de volgende stappen:

> [!NOTE]
> De volgende stappen uitvoeren om de [Azure-portal](https://portal.azure.com) voor het maken van een virtuele Machine van Azure. Zie de volgende documenten voor andere manieren voor het maken van een virtuele machine:
>
> * [Virtuele machine maken - Azure CLI](../virtual-machines/linux/quick-create-cli.md)
> * [Maken van VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md)

1. Van de [Azure-portal](https://portal.azure.com), selecteer  __+__ , __Compute__, en __Ubuntu Server 16.04 LTS__.

    ![Een virtuele Ubuntu-machine maken](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. Van de __basisbeginselen__ sectie, voer de volgende informatie:

    * __Naam__: een beschrijvende naam die deze virtuele machine wordt aangeduid. Bijvoorbeeld: __DNSProxy__.
    * __Gebruikersnaam__: de naam van het SSH-account.
    * __Openbare SSH-sleutel__ of __wachtwoord__: de verificatiemethode voor de SSH-account. Wordt aangeraden met openbare sleutels, omdat ze beter te beveiligen. Zie voor meer informatie de [maken en gebruiken SSH-sleutels voor virtuele Linux-machines](../virtual-machines/linux/mac-create-ssh-keys.md) document.
    * __Resourcegroep__: Selecteer __gebruik bestaande__, en selecteer vervolgens de resourcegroep waarin het virtuele netwerk eerder hebt gemaakt.
    * __Locatie__: Selecteer de dezelfde locatie als het virtuele netwerk.

    ![Basisconfiguratie van de virtuele machine](./media/connect-on-premises-network/vm-basics.png)

    Andere items laat de standaardwaarden en selecteer vervolgens __OK__.

3. Van de __een grootte kiezen__ sectie, selecteert u de VM-grootte. Selecteer de optie waarbij de kleinste en de laagste kosten voor deze zelfstudie. Als u wilt doorgaan, gebruikt u de __Selecteer__ knop.

4. Van de __instellingen__ sectie, voer de volgende informatie:

    * __Virtueel netwerk__: Selecteer het virtuele netwerk die u eerder hebt gemaakt.

    * __Subnet__: Selecteer de standaard-subnet voor het virtuele netwerk. Voer __niet__ selecteert u het subnet dat wordt gebruikt door de VPN-gateway.

    * __Opslagaccount voor diagnostische gegevens__: Selecteer een bestaand opslagaccount of maak een nieuwe.

    ![Virtuele-netwerkinstellingen](./media/connect-on-premises-network/virtual-network-settings.png)

    De andere vermeldingen laat de standaardwaarde en selecteer vervolgens __OK__ om door te gaan.

5. Van de __aankoop__ sectie, selecteer de __aankoop__ om te maken van de virtuele machine.

6. Wanneer de virtuele machine is gemaakt, de __overzicht__ sectie wordt weergegeven. Selecteer in de lijst aan de linkerkant __eigenschappen__. Sla de __openbaar IP-adres__ en __particuliere IP-adres__ waarden. Deze wordt gebruikt in de volgende sectie.

    ![Openbare en particuliere IP-adressen](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Installeren en configureren van de binding (DNS-software)

1. SSH gebruiken voor verbinding met de __openbaar IP-adres__ van de virtuele machine. Het volgende voorbeeld maakt verbinding met een virtuele machine op 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Vervang `sshuser` met het SSH-gebruikersaccount dat u hebt opgegeven bij het maken van het cluster.

    > [!NOTE]
    > Er zijn tal van manieren verkrijgen van de `ssh` hulpprogramma. Op Linux, Unix- en Mac OS, wordt dit geleverd als onderdeel van het besturingssysteem. Als u van Windows gebruikmaakt, overweeg een van de volgende opties:
    >
    > * [Azure-Cloud-Shell](../cloud-shell/quickstart.md)
    > * [Bash op Ubuntu op Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [GIT (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Gebruik de volgende opdrachten bij de SSH-sessie voor het installeren van Bind:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Gebruik voor het configureren van de binding voor het doorsturen van aanvragen voor naamomzetting naar uw on-premises DNS-server de volgende tekst als de inhoud van de `/etc/bind/named.conf.options` bestand:

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > Vervang de waarden in de `goodclients` sectie met de IP-adresbereik van het virtuele netwerk en de on-premises netwerk. Deze sectie worden de adressen die deze DNS-server aanvragen van accepteert gedefinieerd.
    >
    > Vervang de `192.168.0.1` vermelding in de `forwarders` sectie met de IP-adres van uw lokale DNS-server. Deze vermelding routeert DNS-aanvragen naar de lokale DNS-server voor naamomzetting.

    Dit bestand bewerken, moet u de volgende opdracht gebruiken:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Gebruiken om het bestand opslaat, __Ctrl + X__, __Y__, en vervolgens __Enter__.

4. Gebruik de volgende opdracht in de SSH-sessie:

    ```bash
    hostname -f
    ```

    Met deze opdracht retourneert een waarde die vergelijkbaar is met de volgende tekst:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    De `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` tekst is de __DNS-achtervoegsel__ voor dit virtuele netwerk. Deze waarde niet opslaan omdat het wordt later gebruikt.

5. Gebruik voor het configureren van DNS-namen omzetten voor resources binnen het virtuele netwerk is afhankelijk van de volgende tekst als de inhoud van de `/etc/bind/named.conf.local` bestand:

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > Vervang de `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` met de DNS-achtervoegsel dat u eerder hebt opgehaald.

    Dit bestand bewerken, moet u de volgende opdracht gebruiken:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Gebruiken om het bestand opslaat, __Ctrl + X__, __Y__, en vervolgens __Enter__.

6. Gebruik de volgende opdracht voor het starten van Bind:

    ```bash
    sudo service bind9 restart
    ```

7. Om te controleren die afhankelijk van de namen van bronnen in uw on-premises netwerk kunt oplossen, gebruikt u de volgende opdrachten:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Vervang `dns.mynetwork.net` met de volledig gekwalificeerde domeinnaam (FQDN) van een resource in uw on-premises netwerk.
    >
    > Vervang `10.0.0.4` met de __interne IP-adres__ van uw aangepaste DNS-server in het virtuele netwerk.

    Het antwoord ziet er ongeveer de volgende tekst:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Het virtuele netwerk voor het gebruik van de aangepaste DNS-server configureren

Gebruik de volgende stappen voor het configureren van het virtuele netwerk voor het gebruik van de aangepaste DNS-server in plaats van de Azure recursieve resolver:

1. In de [Azure-portal](https://portal.azure.com), selecteer het virtuele netwerk en selecteer vervolgens __DNS-Servers__.

2. Selecteer __aangepaste__, en voer de __interne IP-adres__ van de aangepaste DNS-server. Tot slot selecteert __opslaan__.

    ![De aangepaste DNS-server voor het netwerk instellen](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a>De lokale DNS-server configureren

In de vorige sectie, moet u de aangepaste DNS-server voor het doorsturen van aanvragen naar de lokale DNS-server geconfigureerd. Vervolgens configureert u de lokale DNS-server voor het doorsturen van aanvragen voor de aangepaste DNS-server.

Raadpleeg de documentatie bij uw DNS-server-software voor specifieke stappen over het configureren van uw DNS-server. Zoekt u de stappen voor het configureren van een __voorwaardelijke doorstuurserver__.

Een voorwaardelijke voorwaartse verzendt alleen aanvragen voor een specifieke DNS-achtervoegsel. In dit geval moet u een doorstuurserver voor het DNS-achtervoegsel van het virtuele netwerk configureren. Aanvragen voor dit achtervoegsel moeten worden doorgestuurd naar het IP-adres van de aangepaste DNS-server. 

De volgende tekst is een voorbeeld van een voorwaardelijke doorstuurserver-configuratie voor de **binden** DNS-software:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

Voor informatie over het gebruik van DNS op **Windows Server 2016**, Zie de [toevoegen DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentatie...

Wanneer u de lokale DNS-server hebt geconfigureerd, kunt u `nslookup` van de on-premises netwerk om te controleren dat u namen in het virtuele netwerk omzetten kan. Het volgende voorbeeld 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Dit voorbeeld wordt de lokale DNS-server op 196.168.0.4 omzetten van de naam van de aangepaste DNS-server. Het IP-adres vervangen door de voor de lokale DNS-server. Vervang de `dnsproxy` adres met de volledig gekwalificeerde domeinnaam van de aangepaste DNS-server.

## <a name="optional-control-network-traffic"></a>Optioneel: Besturingselement netwerkverkeer

U kunt netwerkbeveiligingsgroepen (NSG) of de gebruiker gedefinieerde routes (UDR) netwerkverkeer wordt beheerd. Nsg's kunnen u binnenkomend en uitgaand verkeer filteren en toestaan of weigeren van het verkeer. Udr's kunnen u bepalen hoe verkeersstromen tussen resources in het virtuele netwerk, internet en de on-premises netwerk.

> [!WARNING]
> HDInsight vereist binnenkomende toegang van specifieke IP-adressen in de Azure-cloud en onbeperkte uitgaande toegang. Wanneer u nsg's of udr's om te bepalen van verkeer, moet u de volgende stappen uitvoeren:
>
> 1. De IP-adressen vinden voor de locatie waarin het virtuele netwerk. Zie voor een lijst met vereiste IP-adressen per locatie, [vereiste IP-adressen](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).
>
> 2. Binnenkomend verkeer van de IP-adressen toestaan.
>
>    * __NSG__: toestaan __inkomende__ verkeer op poort __443__ van de __Internet__.
>    * __UDR__: Stel de __volgende Hop__ type van de route naar __Internet__.

Zie voor een voorbeeld van het gebruik van Azure PowerShell of Azure CLI voor het nsg's maken, de [HDInsight uitbreiden met Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) document.

## <a name="create-the-hdinsight-cluster"></a>Het HDInsight-cluster maken

> [!WARNING]
> Voordat u HDInsight in het virtuele netwerk installeert, moet u de aangepaste DNS-server configureren.

Gebruik de stappen in de [maken van een HDInsight-cluster met de Azure portal](./hdinsight-hadoop-create-linux-clusters-portal.md) document om een HDInsight-cluster te maken.

> [!WARNING]
> * Tijdens het maken, moet u de locatie waarin het virtuele netwerk.
>
> * In de __geavanceerde instellingen__ deel van de configuratie, moet u het virtuele netwerk en subnet dat u eerder hebt gemaakt.

## <a name="connecting-to-hdinsight"></a>Verbinding maken met HDInsight

De meeste documentatie op HDInsight wordt ervan uitgegaan dat u toegang tot het cluster via internet hebt. Bijvoorbeeld, u verbinding kunt maken met het cluster op https://CLUSTERNAME.azurehdinsight.net. Dit adres wordt gebruikt voor de openbare-gateway niet beschikbaar is als u nsg's of udr's hebt gebruikt om toegang te beperken van het internet.

Sommige documentatie verwijst ook naar `headnodehost` bij het verbinden met het cluster van een SSH-sessie. Dit adres is alleen beschikbaar vanuit de knooppunten binnen een cluster, en kan niet worden gebruikt op clients via het virtuele netwerk is verbonden.

Om rechtstreeks verbinding maken met HDInsight via het virtuele netwerk, gebruikt u de volgende stappen uit:

1. Gebruik een van de volgende methoden voor het detecteren van de interne FQDN-namen van de clusterknooppunten HDInsight:

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

2. Zie het vaststellen van de poort die een service is beschikbaar op de [poorten die worden gebruikt door de services van Hadoop op HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.

    > [!IMPORTANT]
    > Sommige services die worden gehost op de hoofdknooppunten zijn alleen actief is op één knooppunt tegelijk. Als u probeert toegang tot een service op één hoofdknooppunt en dit mislukt, overschakelen naar het hoofdknooppunt.
    >
    > Bijvoorbeeld, is Ambari alleen actief op één hoofdknooppunt tegelijk. Als u probeert toegang tot Ambari op één hoofdknooppunt en een 404-fout retourneert, wordt klikt u vervolgens deze uitgevoerd op het hoofdknooppunt.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het gebruik van HDInsight in een virtueel netwerk [HDInsight uitbreiden met behulp van Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).

* Zie voor meer informatie over virtuele netwerken in Azure, de [Azure Virtual Network-overzicht](../virtual-network/virtual-networks-overview.md).

* Zie voor meer informatie over netwerkbeveiligingsgroepen [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md).

* Zie voor meer informatie over de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).
