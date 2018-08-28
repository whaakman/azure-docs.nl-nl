---
title: HDInsight verbinden met uw on-premises netwerk - Azure HDInsight
description: Leer hoe u een HDInsight-cluster maken in een Virtueelnetwerk van Azure en verbindt dit vervolgens met uw on-premises netwerk. Informatie over het configureren van naamomzetting tussen HDInsight en uw on-premises netwerk met behulp van een aangepaste DNS-server.
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: jasonh
ms.openlocfilehash: b9b744edbcb2abce70487c84c85a0aae71494e52
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105865"
---
# <a name="connect-hdinsight-to-your-on-premises-network"></a>HDInsight verbinden met uw on-premises netwerk

Leer hoe u met HDInsight uw on-premises netwerk te maken met Azure Virtual Networks en een VPN-gateway. Dit document vindt u planningsinformatie op:

* Met behulp van HDInsight in een Azure-netwerk dat is verbonden met uw on-premises netwerk.

* Configureren van DNS-naamomzetting tussen het virtuele netwerk en uw on-premises netwerk.

* Configuratie van netwerkbeveiligingsgroepen toegang tot internet beperken tot HDInsight.

* Poorten die door HDInsight op het virtuele netwerk.

## <a name="create-the-virtual-network-configuration"></a>De configuratie van het virtuele netwerk maken

Gebruik de volgende documenten voor meer informatie over het maken van een Azure-netwerk dat is verbonden met uw on-premises netwerk:
    
* [Azure Portal gebruiken](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Azure PowerShell gebruiken](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Azure CLI gebruiken](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Naamomzetting configureren

Als u wilt toestaan dat HDInsight en resources in het netwerk is toegevoegd aan om te communiceren met de naam, moet u de volgende acties uitvoeren:

* Maak een aangepaste DNS-server in de Azure-netwerk.

* Het virtuele netwerk voor het gebruik van de aangepaste DNS-server in plaats van de standaard Azure recursieve naamomzetting configureren.

* Doorsturen van tussen de aangepaste DNS-server en uw on-premises DNS-server configureren.

Deze configuratie kunt het volgende gedrag:

* Aanvragen voor de volledig gekwalificeerde domeinnamen waarvoor het DNS-achtervoegsel __voor het virtuele netwerk__ worden doorgestuurd naar de aangepaste DNS-server. De aangepaste DNS-server stuurt deze aanvragen vervolgens door aan de recursieve Resolver van Azure, dat het IP-adres wordt geretourneerd.

* Alle andere aanvragen worden doorgestuurd naar de lokale DNS-server. Ook aanvragen voor het openbare internet-bronnen zoals microsoft.com worden doorgestuurd naar de lokale DNS-server voor naamomzetting.

Groen regels zijn in het volgende diagram aanvragen voor bronnen die op de DNS-achtervoegsel van het virtuele netwerk eindigen. Blauwe lijnen zijn aanvragen voor bronnen in de on-premises netwerk of op het openbare internet.

![Diagram van hoe de DNS-aanvragen in de configuratie die wordt gebruikt in dit document worden opgelost](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Een aangepaste DNS-server maken

> [!IMPORTANT]
> U moet maken en configureren van de DNS-server voor de installatie van HDInsight in het virtuele netwerk.

Het maken van een Linux-VM die gebruikmaakt van de [binden](https://www.isc.org/downloads/bind/) DNS-software, gebruikt u de volgende stappen uit:

> [!NOTE]
> De volgende stappen uitvoeren om de [Azure-portal](https://portal.azure.com) te maken van een virtuele Machine van Azure. Voor andere manieren om een virtuele machine maken, Zie de volgende documenten:
>
> * [Virtuele machine maken - Azure CLI](../virtual-machines/linux/quick-create-cli.md)
> * [Virtuele machine maken - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md)

1. Uit de [Azure-portal](https://portal.azure.com), selecteer __+__, __Compute__, en __Ubuntu Server 16.04 LTS__.

    ![Een virtuele Ubuntu-machine maken](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. Geef in de sectie __Basisbeginselen__ de volgende gegevens op:

    * __Naam__: een beschrijvende naam ter identificatie van deze virtuele machine. Bijvoorbeeld, __DNSProxy__.
    * __Gebruikersnaam__: de naam van het SSH-account.
    * __Openbare SSH-sleutel__ of __wachtwoord__: de verificatiemethode voor de SSH-account. Wordt u aangeraden openbare sleutels, omdat ze beter te beveiligen zijn. Zie voor meer informatie de [maken en gebruik SSH-sleutels voor Linux-VM's](../virtual-machines/linux/mac-create-ssh-keys.md) document.
    * __Resourcegroep__: Selecteer __gebruik bestaande__, en selecteer vervolgens de resourcegroep waarin het virtuele netwerk eerder hebt gemaakt.
    * __Locatie__: Selecteer de dezelfde locatie als het virtuele netwerk.

    ![Basisinformatie over de configuratie virtuele machine](./media/connect-on-premises-network/vm-basics.png)

    Andere vermeldingen laat de standaardwaarden en selecteer vervolgens __OK__.

3. Uit de __Kies een grootte__ sectie, selecteer de VM-grootte. Selecteer de optie kleinste en de laagste kosten voor deze zelfstudie. Als u wilt doorgaan, gebruikt u de __Selecteer__ knop.

4. Uit de __instellingen__ sectie, voer de volgende informatie:

    * __Virtueel netwerk__: Selecteer het virtuele netwerk dat u eerder hebt gemaakt.

    * __Subnet__: Selecteer de standaard-subnet voor het virtuele netwerk. Voer __niet__ selecteert u het subnet dat wordt gebruikt door de VPN-gateway.

    * __Opslagaccount voor diagnostische gegevens__: Selecteer een bestaand opslagaccount of maak een nieuwe.

    ![Instellingen voor virtueel netwerk](./media/connect-on-premises-network/virtual-network-settings.png)

    Laat de overige items op de standaardwaarde, en selecteer vervolgens __OK__ om door te gaan.

5. Uit de __aankoop__ sectie, selecteer de __aankoop__ om te maken van de virtuele machine.

6. Nadat de virtuele machine is gemaakt, de __overzicht__ sectie wordt weergegeven. Selecteer in de lijst aan de linkerkant __eigenschappen__. Sla de __openbaar IP-adres__ en __privé IP-adres__ waarden. Deze wordt gebruikt in de volgende sectie.

    ![Openbare en persoonlijke IP-adressen](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Installeren en configureren van de binding (DNS-software)

1. SSH gebruiken om te verbinden met de __openbaar IP-adres__ van de virtuele machine. Het volgende voorbeeld maakt u verbinding met een virtuele machine op 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Vervang `sshuser` met de SSH-gebruikersaccount dat u hebt opgegeven bij het maken van het cluster.

    > [!NOTE]
    > Er zijn verschillende manieren om op te halen de `ssh` hulpprogramma. Op Linux, Unix- en Mac OS, is deze geleverd als onderdeel van het besturingssysteem. Als u Windows gebruikt, kunt u overwegen een van de volgende opties:
    >
    > * [Azure Cloud Shell](../cloud-shell/quickstart.md)
    > * [Bash in Ubuntu in Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [GIT (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Gebruik de volgende opdrachten uit de SSH-sessie voor het installeren van de binding voor:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Voor het configureren van de binding voor het doorsturen van aanvragen voor naamomzetting met uw on-premises DNS-server, gebruikt u de volgende tekst als de inhoud van de `/etc/bind/named.conf.options` bestand:

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
    > Vervang de waarden in de `goodclients` sectie met het IP-adresbereik van het virtuele netwerk en de on-premises netwerk. Deze sectie worden de adressen die deze DNS-server aanvragen van accepteert gedefinieerd.
    >
    > Vervang de `192.168.0.1` vermelding in de `forwarders` sectie met het IP-adres van uw on-premises DNS-server. Deze vermelding via DNS-aanvragen naar uw on-premises DNS-server voor het omzetten van geleid.

    Als u wilt dit bestand bewerken, moet u de volgende opdracht gebruiken:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Gebruiken om het bestand hebt opgeslagen, __Ctrl + X__, __Y__, en vervolgens __Enter__.

4. Gebruik de volgende opdracht uit de SSH-sessie:

    ```bash
    hostname -f
    ```

    Met deze opdracht retourneert een waarde die vergelijkbaar is met de volgende tekst:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    De `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` tekst is de __DNS-achtervoegsel__ voor dit virtuele netwerk. Sla deze waarde op, aangezien u die later nog nodig hebt.

5. Gebruik voor het configureren van DNS-namen omzetten voor resources binnen het virtuele netwerk is afhankelijk van de volgende tekst als de inhoud van de `/etc/bind/named.conf.local` bestand:

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > U moet vervangen de `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` met de DNS-achtervoegsel dat u eerder hebt opgehaald.

    Als u wilt dit bestand bewerken, moet u de volgende opdracht gebruiken:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Gebruiken om het bestand hebt opgeslagen, __Ctrl + X__, __Y__, en vervolgens __Enter__.

6. Gebruik voor het starten van de binding voor de volgende opdracht:

    ```bash
    sudo service bind9 restart
    ```

7. Om te controleren die afhankelijk van de namen van resources in uw on-premises netwerk kan omzetten, gebruikt u de volgende opdrachten:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Vervang `dns.mynetwork.net` met de volledig gekwalificeerde domeinnaam (FQDN) van een resource in uw on-premises netwerk.
    >
    > Vervang `10.0.0.4` met de __interne IP-adres__ van uw aangepaste DNS-server in het virtuele netwerk.

    Het antwoord weergegeven die vergelijkbaar is met de volgende tekst:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Het virtuele netwerk voor het gebruik van de aangepaste DNS-server configureren

Het virtuele netwerk voor het gebruik van de aangepaste DNS-server in plaats van de Azure recursieve naamomzetting configureren, gebruiken de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com), selecteer het virtuele netwerk en selecteer vervolgens __DNS-Servers__.

2. Selecteer __aangepaste__, en voer de __interne IP-adres__ van de aangepaste DNS-server. Selecteer ten slotte __opslaan__.

    ![Stel de aangepaste DNS-server voor het netwerk](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a>De on-premises DNS-server configureren

In de vorige sectie hebt u de aangepaste DNS-server voor het doorsturen van aanvragen naar de lokale DNS-server geconfigureerd. Vervolgens configureert u de on-premises DNS-server voor het doorsturen van aanvragen naar de aangepaste DNS-server.

Raadpleeg de documentatie voor uw DNS-server-software voor specifieke stappen over het configureren van uw DNS-server. Zoek naar de stappen voor het configureren van een __voorwaardelijke doorstuurserver__.

Een voorwaardelijk doorsturen verzendt alleen aanvragen voor een specifieke DNS-achtervoegsel. In dit geval moet u een doorstuurserver voor de DNS-achtervoegsel van het virtuele netwerk configureren. Aanvragen voor dit achtervoegsel moeten worden doorgestuurd naar het IP-adres van de aangepaste DNS-server. 

De volgende tekst is een voorbeeld van een voorwaardelijke doorstuurserver-configuratie voor de **binden** DNS-software:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

Voor informatie over het gebruik van DNS op **Windows Server 2016**, Zie de [toevoegen DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentatie...

Nadat u de on-premises DNS-server hebt geconfigureerd, kunt u `nslookup` van de on-premises netwerk om te controleren of dat u namen in het virtuele netwerk omzetten kan. Het volgende voorbeeld 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Dit voorbeeld wordt de lokale DNS-server op 196.168.0.4 om op te lossen van de naam van de aangepaste DNS-server. Het IP-adres vervangen door de oplossing voor de on-premises DNS-server. Vervang de `dnsproxy` -adres met de volledig gekwalificeerde domeinnaam van de aangepaste DNS-server.

## <a name="optional-control-network-traffic"></a>Optioneel: Controle-netwerkverkeer

U kunt netwerkbeveiligingsgroepen (NSG) of de gebruiker gedefinieerde routes (UDR) gebruiken voor het beheren van netwerkverkeer. Nsg's kunnen u binnenkomend en uitgaand verkeer, filteren en toestaan of weigeren van het verkeer. Udr's kunnen u bepalen hoe verkeer stroomt tussen resources in het virtuele netwerk, het internet en de on-premises netwerk.

> [!WARNING]
> HDInsight is vereist voor inkomende toegang vanaf specifieke IP-adressen in de Azure-cloud en onbeperkte uitgaande toegang. Wanneer u nsg's of udr's verkeer beheren, moet u de volgende stappen uitvoeren:

1. De IP-adressen voor de locatie waarin het virtuele netwerk niet vinden. Zie voor een lijst met vereiste IP-adressen per locatie, [vereiste IP-adressen](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).

2. Voor de IP-adressen in stap 1 hebt geïdentificeerd, kunt u binnenkomend verkeer toestaan van die-IP-adressen.

   * Als u __NSG__: toestaan __inkomende__ verkeer op poort __443__ voor de IP-adressen.
   * Als u __UDR__: Stel de __' volgende hop '__ type van de route naar __Internet__ voor de IP-adressen.

Zie voor een voorbeeld van het gebruik van Azure PowerShell of Azure CLI nsg's maken, de [HDInsight uitbreiden met Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) document.

## <a name="create-the-hdinsight-cluster"></a>Het HDInsight-cluster maken

> [!WARNING]
> U moet de aangepaste DNS-server configureren voor het installeren van HDInsight in het virtuele netwerk.

Gebruik de stappen in de [maken van een HDInsight-cluster met behulp van de Azure-portal](./hdinsight-hadoop-create-linux-clusters-portal.md) document om een HDInsight-cluster te maken.

> [!WARNING]
> * Tijdens het maken, moet u de locatie waarin het virtuele netwerk.
>
> * In de __geavanceerde instellingen__ deel van de configuratie, moet u het virtuele netwerk en subnet dat u eerder hebt gemaakt.

## <a name="connecting-to-hdinsight"></a>Verbinding maken met HDInsight

De meeste documentatie op HDInsight wordt ervan uitgegaan dat u toegang tot het cluster via internet hebt. Bijvoorbeeld, dat u verbinding met het cluster kunt maken op https://CLUSTERNAME.azurehdinsight.net. Dit adres maakt gebruik van de openbare gateway, deze niet beschikbaar is als u nsg's of udr's hebt gebruikt om toegang te beperken van het internet.

Sommige documentatie verwijst ook naar `headnodehost` bij het verbinden met het cluster van een SSH-sessie. Dit adres is alleen beschikbaar vanuit de knooppunten binnen een cluster, en kan niet worden gebruikt op clients via het virtuele netwerk is verbonden.

Om rechtstreeks verbinding maken met HDInsight via het virtuele netwerk, gebruikt u de volgende stappen uit:

1. Voor het detecteren van de interne volledig gekwalificeerde domeinnamen van het HDInsight-clusterknooppunten, moet u een van de volgende methoden gebruiken:

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

2. Zie het vaststellen van de poort die een service is beschikbaar in de [poorten die worden gebruikt door de services van Hadoop op HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.

    > [!IMPORTANT]
    > Sommige services die worden gehost op de hoofdknooppunten zijn alleen actief is op één knooppunt tegelijk. Als u probeert toegang tot een service op één hoofdknooppunt en dat mislukt, kunt u overschakelen naar het hoofdknooppunt.
    >
    > Ambari is bijvoorbeeld alleen actief is op één hoofdknooppunt op een tijdstip. Als u probeert toegang tot Ambari op één hoofdknooppunt en er een 404-fout retourneert, wordt klikt u vervolgens deze uitgevoerd op het hoofdknooppunt.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het gebruik van HDInsight in een virtueel netwerk [HDInsight uitbreiden met behulp van Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).

* Zie voor meer informatie over virtuele netwerken van Azure, de [overzicht van Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

* Zie voor meer informatie over netwerkbeveiligingsgroepen [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).

* Zie voor meer informatie over de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).
