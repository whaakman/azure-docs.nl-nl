---
title: Instellen van de replicatie van de HBase-cluster in virtuele netwerken in Azure | Microsoft Docs
description: Informatie over het instellen van HBase-replicatie van één HDInsight versie naar een andere voor taakverdeling, hoge beschikbaarheid, nul uitvaltijd migratie en -updates en herstel na noodgevallen.
services: hdinsight,virtual-network
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: jgao
ms.openlocfilehash: 56b2b5ae9d3e4a0e682ec3dd47cd5cc30ebf6d58
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="set-up-hbase-cluster-replication-in-azure-virtual-networks"></a>Instellen van de replicatie van de HBase-cluster in virtuele netwerken in Azure

Informatie over het instellen van de HBase-replicatie binnen een virtueel netwerk of tussen twee virtuele netwerken in Azure.

Replicatie in een cluster maakt gebruik van een bron-push-methode. Een HBase-cluster kan een bron of bestemming, of beide functies tegelijk kunnen worden verwerkt. Replicatie is asynchroon. Het doel van de replicatie is uiteindelijke consistentie. Wanneer de bron een bewerking van een serie kolom ontvangt wanneer replicatie is ingeschakeld, wordt de wijziging wordt doorgegeven aan alle doelclusters. Wanneer gegevens worden gerepliceerd van de ene cluster naar een andere, zijn het broncluster als alle clusters al verbruikt de gegevens worden bijgehouden, om te voorkomen dat replicatie lussen.

In deze zelfstudie maakt instellen u de replicatie van een bron-doel. Zie voor andere clustertopologieën de [Naslaggids voor Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).

Hier volgen de HBase-replicatie gebruik gevallen voor één virtueel netwerk:

* Taakverdeling. U kunt bijvoorbeeld scans of MapReduce-taken uitvoeren op de doelcluster en opnemen van gegevens op de broncluster.
* Toe te voegen met hoge beschikbaarheid.
* Migreren van gegevens van een HBase-cluster naar een andere.
* Een Azure HDInsight-cluster upgraden van één versie naar een andere.

Hier volgen de HBase-replicatie gebruik gevallen voor twee virtuele netwerken:

* Instellen van herstel na noodgevallen.
* Taakverdeling en partitioneren van de toepassing.
* Toe te voegen met hoge beschikbaarheid.

U kunt clusters repliceren met behulp van [script actie](../hdinsight-hadoop-customize-cluster-linux.md) scripts van [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [ophalen van een gratis proefversie van Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="set-up-the-environments"></a>De omgeving instellen

Hebt u drie opties:

- Twee HBase-clusters in een virtuele Azure-netwerk.
- Twee HBase-clusters in twee verschillende virtuele netwerken in dezelfde regio.
- Twee HBase-clusters in twee verschillende virtuele netwerken in twee verschillende regio's (geo-replicatie).

In dit artikel bevat informatie over het scenario geo-replicatie.

Instellen om u te helpen de omgevingen, wij hebben enkele gemaakt [Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md). Als u liever voor het instellen van de omgevingen met behulp van andere methoden, Zie:

- [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
- [HBase-clusters maken in Azure Virtual Network](apache-hbase-provision-vnet.md)

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>Instellen van twee virtuele netwerken in twee verschillende regio 's

Selecteer de volgende afbeelding maken om twee virtuele netwerken maken in twee verschillende regio's en de VPN-verbinding tussen de VNets, de. De sjabloon wordt opgeslagen in een [openbare blob-opslag]] (https://hditutorialdata.blob.core.windows.net/hbaseha/azuredeploy.json).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Sommige van de vastgelegde waarden in de sjabloon:

**VNet-1**

| Eigenschap | Waarde |
|----------|-------|
| Locatie | VS - west |
| VNet-naam | &lt;ClusterNamePrevix >-vnet1 |
| Adresruimtevoorvoegsel | 10.1.0.0/16 |
| Subnetnaam | subnet 1 |
| Subnetvoorvoegsel | 10.1.0.0/24 |
| De subnetnaam van het (gateway) | GatewaySubnet (kan niet worden gewijzigd) |
| Subnetvoorvoegsel (gateway) | 10.1.255.0/27 |
| De naam van gateway | vnet1gw |
| Gatewaytype | VPN |
| Type van de VPN-gateway | RouteBased |
| Gateway-SKU | Basic |
| gateway-IP | vnet1gwip |

**VNet 2**

| Eigenschap | Waarde |
|----------|-------|
| Locatie | VS - oost |
| VNet-naam | &lt;ClusterNamePrevix >-vnet2 |
| Adresruimtevoorvoegsel | 10.2.0.0/16 |
| Subnetnaam | subnet 1 |
| Subnetvoorvoegsel | 10.2.0.0/24 |
| De subnetnaam van het (gateway) | GatewaySubnet (kan niet worden gewijzigd) |
| Subnetvoorvoegsel (gateway) | 10.2.255.0/27 |
| De naam van gateway | vnet2gw |
| Gatewaytype | VPN |
| Type van de VPN-gateway | RouteBased |
| Gateway-SKU | Basic |
| gateway-IP | vnet1gwip |

## <a name="setup-dns"></a>DNS instellen

In de laatste sectie maakt de sjabloon u een virtuele Ubuntu-machine in elk van de twee virtuele netwerken.  Bind installeren op de twee DNS-virtuele machines in deze sectie en configureer vervolgens het doorsturen van DNS voor de twee virtuele machines.

Voor het installeren van Bind moet yon vinden van het openbare IP-adres van de twee DNS-virtuele machines.

1. Open de [Azure Portal](https://portal.azure.com).
2. De DNS-virtuele machine openen door te selecteren **resourcegroepen > [Resourcegroepnaam] > [vnet1DNS]**.  Naam van de resourcegroep is degene die u in de laatste procedure maakt. De namen van de standaard DNS-virtuele machine zijn *vnet1DNS* en *vnet2NDS*.
3. Selecteer **eigenschappen** om de eigenschappenpagina van het virtuele netwerk te openen.
4. Noteer de **openbaar IP-adres**, en Controleer ook of de **particuliere IP-adres**.  De privé IP-adres worden **10.1.0.4** voor vnet1DNS en **10.2.0.4** voor vnet2DNS.  

Gebruik de volgende procedure voor het installeren van Bind:

1. SSH gebruiken voor verbinding met de __openbaar IP-adres__ van de DNS-virtuele machine. Het volgende voorbeeld maakt verbinding met een virtuele machine op 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Vervang `sshuser` met het SSH-gebruikersaccount dat u hebt opgegeven bij het maken van de DNS-virtuele machine.

    > [!NOTE]
    > Er zijn tal van manieren verkrijgen van de `ssh` hulpprogramma. Op Linux, Unix- en Mac OS, wordt dit geleverd als onderdeel van het besturingssysteem. Als u van Windows gebruikmaakt, overweeg een van de volgende opties:
    >
    > * [Azure-Cloud-Shell](../../cloud-shell/quickstart.md)
    > * [Bash op Ubuntu op Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [GIT (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH)https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Gebruik de volgende opdrachten bij de SSH-sessie voor het installeren van Bind:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Gebruik voor het configureren van de binding voor het doorsturen van aanvragen voor naamomzetting naar uw on-premises DNS-server de volgende tekst als de inhoud van de `/etc/bind/named.conf.options` bestand:

    ```
    acl goodclients {
        10.1.0.0/16; # Replace with the IP address range of the virtual network 1
        10.2.0.0/16; # Replace with the IP address range of the virtual network 2
        localhost;
        localhost;
    };
    
    options {
        directory "/var/cache/bind";
        recursion yes;
        allow-query { goodclients; };

        forwarders {
            168.63.129.16 #This is the Azure DNS server
        };

        dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };
    ```
    
    > [!IMPORTANT]
    > Vervang de waarden in de `goodclients` sectie met de IP-adresbereik van de twee virtuele netwerken. Deze sectie worden de adressen die deze DNS-server aanvragen van accepteert gedefinieerd.

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

        vnet1DNS.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    De `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` tekst is de __DNS-achtervoegsel__ voor dit virtuele netwerk. Deze waarde niet opslaan omdat het wordt later gebruikt.

    U moet ook de DNS-achtervoegsel van de DNS-server weten. U moet deze in de volgende stap.

5. Gebruik voor het configureren van DNS-namen omzetten voor resources binnen het virtuele netwerk is afhankelijk van de volgende tekst als de inhoud van de `/etc/bind/named.conf.local` bestand:

    ```
    // Replace the following with the DNS suffix for your virtual network
    zone "v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net" {
            type forward;
            forwarders {10.2.0.4;}; # The Azure recursive resolver
    };
    ```

    > [!IMPORTANT]
    > Vervang de `v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` met de DNS-achtervoegsel van het virtuele netwerk. En de doorstuurserver IP-adres is het privé IP-adres van de DNS-server in het virtuele netwerk.

    Dit bestand bewerken, moet u de volgende opdracht gebruiken:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Gebruiken om het bestand opslaat, __Ctrl + X__, __Y__, en vervolgens __Enter__.

6. Gebruik de volgende opdracht voor het starten van Bind:

    ```bash
    sudo service bind9 restart
    ```

7. Om te controleren die afhankelijk van de namen van bronnen in het virtuele netwerk kunt oplossen, gebruikt u de volgende opdrachten:

    ```bash
    sudo apt install dnsutils
    nslookup vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net 10.2.0.4
    ```

    > [!IMPORTANT]
    > Vervang `vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` met de volledig gekwalificeerde domeinnaam (FQDN) van de DNS-virtuele machine in het andere netwerk.
    >
    > Vervang `10.2.0.4` met de __interne IP-adres__ van uw aangepaste DNS-server in het virtuele netwerk.

    Het antwoord ziet er ongeveer de volgende tekst:

    ```
    Server:         10.2.0.4
    Address:        10.2.0.4#53
    
    Non-authoritative answer:
    Name:   vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    Address: 10.2.0.4
    ```

    Tot nu toe door het IP-adres van het andere netwerk zonder opgegeven IP-adres van de DNS-server kan niet zoeken.

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Het virtuele netwerk voor het gebruik van de aangepaste DNS-server configureren

Gebruik de volgende stappen voor het configureren van het virtuele netwerk voor het gebruik van de aangepaste DNS-server in plaats van de Azure recursieve resolver:

1. In de [Azure-portal](https://portal.azure.com), selecteer het virtuele netwerk en selecteer vervolgens __DNS-Servers__.

2. Selecteer __aangepaste__, en voer de __interne IP-adres__ van de aangepaste DNS-server. Tot slot selecteert __opslaan__.

6. Open de virtuele machine van de DNS-server in vnet1 en klikt u op **opnieuw**.  U moet alle virtuele machines opnieuw opstarten in het virtuele netwerk waarmee de DNS-configuratie te laten treden.
7. Herhaal stap de aangepaste DNS-server configureren voor vnet2.

Voor het testen van de DNS-configuratie, kunt u verbinding maken met de twee DNS-virtuele machines met behulp van SSH en de DNS-server van het virtuele netwerk met behulp van de hostnaam ping. Als dit niet werkt, gebruikt u de volgende opdracht om DNS-status te controleren:

```bash
sudo service bind9 status
```

## <a name="create-hbase-clusters"></a>HBase-clusters maken

Maak een HBase-cluster in elk van de twee virtuele netwerken met de volgende configuratie:

- **De naam van resourcegroep**: naam van de dezelfde resourcegroep gebruiken als u de virtuele netwerken hebt gemaakt.
- **Type cluster**: HBase
- **Versie**: HBase 1.1.2 (HDI 3.6)
- **Locatie**: gebruik van dezelfde locatie als het virtuele netwerk.  Is standaard vnet1 *VS-West*, en vnet2 *VS-Oost*.
- **Opslag**: Maak een nieuw opslagaccount voor het cluster.
- **Virtueel netwerk** (van de geavanceerde instellingen op de portal): u hebt gemaakt in de laatste procedure vnet1 selecteren.
- **Subnet**: de standaardnaam die wordt gebruikt in de sjabloon is **subnet1**.

Om te controleren of dat de omgeving correct is geconfigureerd, moet u het volgende kunt pingen FQDN van de headnode tussen twee clusters.

## <a name="load-test-data"></a>Testgegevens laden

Wanneer u een cluster repliceert, moet u de tabellen die u wilt repliceren. In deze sectie kunt u sommige gegevens in de broncluster laden. In de volgende sectie schakelt u replicatie tussen de twee clusters.

Maken van een **contactpersonen** tabel en voeg enkele gegeven in de tabel, volg de instructies voor [HBase-zelfstudie: aan de slag met Apache HBase in HDInsight](apache-hbase-tutorial-get-started-linux.md).

## <a name="enable-replication"></a>Replicatie inschakelen

De volgende stappen wordt beschreven hoe het script actiescript aanroepen vanuit de Azure-portal. Zie voor meer informatie over het uitvoeren van een scriptactie met behulp van Azure PowerShell en het Azure-opdrachtregelprogramma (Azure CLI) [aanpassen HDInsight-clusters met behulp van de scriptactie](../hdinsight-hadoop-customize-cluster-linux.md).

**HBase-replicatie van de Azure-portal in te schakelen**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open de bron HBase-cluster.
3. Selecteer in het menu cluster **scriptacties**.
4. Selecteer boven aan de pagina **nieuwe indienen**.
5. Selecteer of Voer de volgende informatie:

  1. **Naam**: Voer **replicatie inschakelen**.
  2. **Script-URL Bash**: Voer **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  3.  **HEAD**: Zorg ervoor dat deze optie is geselecteerd. De knooppunttypen wissen.
  4. **Parameters**: het volgende voorbeeld parameters replicatie inschakelen voor alle bestaande tabellen en kopieert u alle gegevens van de broncluster naar het doelcluster:

          -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata
    
    >[!note]
    >
    > Hostnaam in plaats van FQDN-naam voor de bron- en doelserver cluster DNS-naam gebruiken.

6. Selecteer **Maken**. Het script kan even duren om uit te voeren, vooral wanneer u de **- copydata** argument.

Vereiste argumenten:

|Naam|Beschrijving|
|----|-----------|
|-s,--src-cluster | Hiermee geeft u de DNS-naam van de bron HBase-cluster. Bijvoorbeeld: -s hbsrccluster,--src-cluster hbsrccluster = |
|-d,--zomertijd-cluster | Hiermee geeft u de DNS-naam van het doel (replica) HBase-cluster. Bijvoorbeeld: -s dsthbcluster,--src-cluster dsthbcluster = |
|-sp,--src-ambari-wachtwoord | Hiermee geeft u het beheerderswachtwoord voor Ambari op de bron HBase-cluster. |
|-dp,--zomertijd-ambari-wachtwoord | Hiermee geeft u het beheerderswachtwoord voor Ambari op de doelcluster HBase.|

Optionele argumenten:

|Naam|Beschrijving|
|----|-----------|
|-su,--src-ambari-gebruiker | Hiermee geeft u de gebruikersnaam van de beheerder voor Ambari op de bron HBase-cluster. De standaardwaarde is **admin**. |
|-du,--zomertijd-ambari-gebruiker | Hiermee geeft u de gebruikersnaam van de beheerder voor Ambari op de doelcluster HBase. De standaardwaarde is **admin**. |
|-t,--lijst van de tabel | Hiermee geeft u de tabellen worden gerepliceerd. Bijvoorbeeld:--tabel lijst = 'Tabel1; tabel2; Tabel3'. Als u geen tabellen opgeeft, worden alle bestaande HBase-tabellen gerepliceerd.|
|-m,--machine | Hiermee geeft u het hoofdknooppunt waarop de scriptactie wordt uitgevoerd. De waarde is **hn1** of **hn0**. Omdat de **hn0** hoofdknooppunt is meestal veelgebruikte, wordt u aangeraden **hn1**. Gebruik deze optie als u het script $0 als de scriptactie van een van de HDInsight-portal of Azure PowerShell.|
|-cp - copydata | Hiermee schakelt u de migratie van bestaande gegevens op de tabellen waarvoor replicatie is ingeschakeld. |
|-rpm, -repliceren-phoenix-metagegevens | Hiermee schakelt u replicatie voor Phoenix systeemtabellen. <br><br>*Gebruik deze optie voorzichtig.* U wordt aangeraden Phoenix tabellen op de replica-clusters opnieuw te maken voordat u dit script gebruiken. |
|-h,--help | Geeft informatie over het gebruik. |

De `print_usage()` sectie van de [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) heeft een gedetailleerde beschrijving van parameters.

Nadat de scriptactie is geïmplementeerd, kunt u SSH verbinding maken met het doelcluster HBase en controleer vervolgens of de gegevens zijn gerepliceerd.

### <a name="replication-scenarios"></a>Replicatie-scenario 's

De volgende lijst ziet u enkele gevallen algemeen gebruik en de bijbehorende parameterinstellingen:

- **Replicatie inschakelen voor alle tabellen tussen twee clusters**. Dit scenario vereist geen kopiëren of migreren van bestaande gegevens in de tabellen en gebruikt geen Phoenix tabellen. Gebruik de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Replicatie inschakelen voor specifieke tabellen**. Schakel replicatie op table1 en table2 Tabel3 door de volgende parameters gebruiken:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Replicatie inschakelen voor specifieke tabellen en kopieer de bestaande gegevens**. Schakel replicatie op table1 en table2 Tabel3 door de volgende parameters gebruiken:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Replicatie inschakelen voor alle tabellen en repliceren Phoenix metagegevens van bron naar doel**. Phoenix metagegevens replicatie is niet ideaal. Deze voorzichtig gebruiken. Gebruik de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Kopiëren en gegevens migreren

Er zijn twee afzonderlijke script actie scripts beschikbaar zijn voor het kopiëren of gegevens migreren nadat replicatie is ingeschakeld:

- [Script voor kleine tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (tabellen die zijn enkele in grootte en de algehele kopie is voltooid in minder dan een uur verwacht)

- [Script voor grote tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (tabellen die naar verwachting langer duurt dan een uur kopiëren)

Kunt u dezelfde procedure die wordt beschreven in [replicatie inschakelen](#enable-replication) de scriptactie niet aanroepen. Gebruik de volgende parameters:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

De `print_usage()` sectie van de [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) is een gedetailleerde beschrijving van parameters.

### <a name="scenarios"></a>Scenario's

- **Specifieke tabellen (test1, test2 en test3) voor alle rijen bewerkt tot op heden kopiëren (huidige tijdstempel)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  Of:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Kopiëren van specifieke tabellen met een opgegeven tijdperiode**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Replicatie uitschakelen

Als replicatie wilt uitschakelen, gebruikt u een ander script actiescript uit [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Kunt u dezelfde procedure die wordt beschreven in [replicatie inschakelen](#enable-replication) de scriptactie niet aanroepen. Gebruik de volgende parameters:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

De `print_usage()` sectie van de [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) heeft een gedetailleerde beschrijving van parameters.

### <a name="scenarios"></a>Scenario's

- **Schakel replicatie op alle tabellen**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  of

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Schakel replicatie op de opgegeven tabellen (Tabel1, tabel2 en Tabel3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de HBase-replicatie binnen een virtueel netwerk of tussen twee virtuele netwerken instellen. Zie voor meer informatie over HDInsight en HBase, deze artikelen:

* [Aan de slag met Apache HBase in HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Overzicht van HDInsight HBase](./apache-hbase-overview.md)
* [HBase-clusters maken in Azure Virtual Network](./apache-hbase-provision-vnet.md)

