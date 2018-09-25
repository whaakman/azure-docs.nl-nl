---
title: Replicatie van HBase-cluster in virtuele Azure-netwerken instellen
description: Meer informatie over het instellen van HBase-replicatie van een HDInsight-versie naar een andere voor taakverdeling, hoge beschikbaarheid, zonder enige uitvaltijd migratie en updates en herstel na noodgevallen.
services: hdinsight,virtual-network
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/15/2018
ms.openlocfilehash: cfc71f34f4b1d8027714c9fb610beebf8cd3be47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978536"
---
# <a name="set-up-hbase-cluster-replication-in-azure-virtual-networks"></a>Replicatie van HBase-cluster in virtuele Azure-netwerken instellen

Meer informatie over het instellen van HBase-replicatie in een virtueel netwerk, of tussen twee virtuele netwerken in Azure.

Replicatie in een cluster maakt gebruik van een bron-push-methode. Een HBase-cluster kan een bron of bestemming, of beide functies tegelijk kunnen worden verwerkt. Replicatie is asynchroon. Het doel van de replicatie is uiteindelijke consistentie. Wanneer de bron een bewerking een kolomfamilie ontvangt wanneer replicatie is ingeschakeld, wordt de bewerking wordt doorgegeven aan alle doelclusters. Wanneer gegevens worden gerepliceerd van het ene cluster naar een andere, zijn het broncluster en alle clusters die al voor de gegevens gebruikt hebben worden bijgehouden, om te voorkomen dat replicatie lussen.

In deze zelfstudie, moet u de replicatie van een bron-doel instellen. Zie voor andere clustertopologieën de [Snelzoekgids voor Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).

Hier volgen de HBase-replicatie gebruik aanvragen voor één virtueel netwerk:

* Taakverdeling. U kunt bijvoorbeeld scans of MapReduce-taken uitvoeren op de doelcluster en opnemen van gegevens in de broncluster.
* Toe te voegen met hoge beschikbaarheid.
* Migreren van gegevens uit een HBase-cluster naar een andere.
* Een Azure HDInsight-cluster upgraden van één versie naar een andere.

Hier volgen de HBase-replicatie gebruik aanvragen voor twee virtuele netwerken:

* Instellen van herstel na noodgevallen.
* Load balancing en partitioneren van de toepassing.
* Toe te voegen met hoge beschikbaarheid.

U kunt clusters repliceren met behulp van [script actie](../hdinsight-hadoop-customize-cluster-linux.md) scripts via [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [ophalen van een gratis proefversie van Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="set-up-the-environments"></a>De omgevingen instellen

Hebt u drie opties:

- Twee HBase-clusters in een Azure-netwerk.
- Twee HBase-clusters in twee verschillende virtuele netwerken in dezelfde regio.
- Twee HBase-clusters in twee verschillende virtuele netwerken in twee verschillende regio's (geo-replicatie).

In dit artikel behandelt de geo-replicatie.

Instellen om u te helpen de omgevingen, hebben we enkele gemaakt [Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-overview.md). Als u liever het instellen van de omgevingen met behulp van andere methoden, Zie:

- [Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
- [HBase-clusters maken in Azure Virtual Network](apache-hbase-provision-vnet.md)

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>Instellen van twee virtuele netwerken in twee verschillende regio 's

Voor het gebruik van een sjabloon die twee virtuele netwerken in twee verschillende regio's en de VPN-verbinding tussen de VNets maakt, selecteert u de volgende **implementeren in Azure** knop. De sjabloondefinitie van de is opgeslagen in een [openbare blob-opslag](https://hditutorialdata.blob.core.windows.net/hbaseha/azuredeploy.json).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Enkele van de vastgelegde waarden in de sjabloon:

**VNet-1**

| Eigenschap | Waarde |
|----------|-------|
| Locatie | US - west |
| VNet-naam | &lt;ClusterNamePrevix >-vnet1 |
| Adresruimtevoorvoegsel | 10.1.0.0/16 |
| Subnetnaam | subnet 1 |
| Het subnetvoorvoegsel | 10.1.0.0/24 |
| Subnetnaam (gateway) | GatewaySubnet (kan niet worden gewijzigd) |
| Subnetvoorvoegsel (gateway) | 10.1.255.0/27 |
| De naam van gateway | vnet1gw |
| Gatewaytype | VPN |
| Type van de VPN-gateway | RouteBased |
| Gateway-SKU | Basic |
| Gateway IP | vnet1gwip |

**VNet 2**

| Eigenschap | Waarde |
|----------|-------|
| Locatie | US - oost |
| VNet-naam | &lt;ClusterNamePrevix >-vnet2 |
| Adresruimtevoorvoegsel | 10.2.0.0/16 |
| Subnetnaam | subnet 1 |
| Het subnetvoorvoegsel | 10.2.0.0/24 |
| Subnetnaam (gateway) | GatewaySubnet (kan niet worden gewijzigd) |
| Subnetvoorvoegsel (gateway) | 10.2.255.0/27 |
| De naam van gateway | vnet2gw |
| Gatewaytype | VPN |
| Type van de VPN-gateway | RouteBased |
| Gateway-SKU | Basic |
| Gateway IP | vnet1gwip |

## <a name="setup-dns"></a>DNS instellen

In de laatste sectie maakt de sjabloon u een virtuele Ubuntu-machine in elk van de twee virtuele netwerken.  In deze sectie kunt u Bind installeren op de twee virtuele machines voor DNS- en configureer vervolgens het doorsturen van de DNS op de twee virtuele machines.

Voor het installeren van de binding moet yon vinden van het openbare IP-adres van de twee DNS-virtuele machines.

1. Open de [Azure Portal](https://portal.azure.com).
2. Open de DNS-virtuele machine en selecteer **resourcegroepen > [Resourcegroepnaam] > [vnet1DNS]**.  Naam van de resourcegroep is degene die u in de laatste procedure maakt. De namen van de standaard DNS-virtuele machine zijn *vnet1DNS* en *vnet2NDS*.
3. Selecteer **eigenschappen** om de eigenschappenpagina van het virtuele netwerk te openen.
4. Noteer de **openbaar IP-adres**, en Controleer ook of de **privé IP-adres**.  Het privé IP-adres worden **10.1.0.4** voor vnet1DNS en **10.2.0.4** voor vnet2DNS.  

Gebruik de volgende procedure voor het installeren van de binding voor:

1. SSH gebruiken om te verbinden met de __openbaar IP-adres__ van de DNS-virtuele machine. Het volgende voorbeeld maakt u verbinding met een virtuele machine op 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Vervang `sshuser` met de SSH-gebruikersaccount dat u hebt opgegeven bij het maken van de DNS-virtuele machine.

    > [!NOTE]
    > Er zijn verschillende manieren om op te halen de `ssh` hulpprogramma. Op Linux, Unix- en Mac OS, is deze geleverd als onderdeel van het besturingssysteem. Als u Windows gebruikt, kunt u overwegen een van de volgende opties:
    >
    > * [Azure Cloud Shell](../../cloud-shell/quickstart.md)
    > * [Bash in Ubuntu in Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [GIT (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Gebruik de volgende opdrachten uit de SSH-sessie voor het installeren van de binding voor:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Binding voor het doorsturen van aanvragen voor naamomzetting met uw on-premises DNS-server configureren. Om dit te doen, gebruikt u de volgende tekst als de inhoud van de `/etc/bind/named.conf.options` bestand:

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
            168.63.129.16; #This is the Azure DNS server
        };

        dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };
    ```
    
    > [!IMPORTANT]
    > Vervang de waarden in de `goodclients` sectie met het IP-adresbereik van de twee virtuele netwerken. Deze sectie worden de adressen die deze DNS-server aanvragen van accepteert gedefinieerd.

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

        vnet1DNS.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    De `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` tekst is de __DNS-achtervoegsel__ voor dit virtuele netwerk. Sla deze waarde op, aangezien u die later nog nodig hebt.

    U moet ook de DNS-achtervoegsel van de DNS-server weten. U hebt deze nodig in de volgende stap.

5. Gebruik voor het configureren van DNS-namen omzetten voor resources binnen het virtuele netwerk is afhankelijk van de volgende tekst als de inhoud van de `/etc/bind/named.conf.local` bestand:

    ```
    // Replace the following with the DNS suffix for your virtual network
    zone "v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net" {
            type forward;
            forwarders {10.2.0.4;}; # The Azure recursive resolver
    };
    ```

    > [!IMPORTANT]
    > U moet vervangen de `v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` met de DNS-achtervoegsel van het virtuele netwerk. En het IP-adres voor de doorstuurserver is het privé IP-adres van de DNS-server in het virtuele netwerk.

    Als u wilt dit bestand bewerken, moet u de volgende opdracht gebruiken:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Gebruiken om het bestand hebt opgeslagen, __Ctrl + X__, __Y__, en vervolgens __Enter__.

6. Gebruik voor het starten van de binding voor de volgende opdracht:

    ```bash
    sudo service bind9 restart
    ```

7. Om te controleren die afhankelijk van de namen van resources in het virtuele netwerk kan omzetten, gebruikt u de volgende opdrachten:

    ```bash
    sudo apt install dnsutils
    nslookup vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net 10.2.0.4
    ```

    > [!IMPORTANT]
    > Vervang `vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` met de volledig gekwalificeerde domeinnaam (FQDN) van de DNS-virtuele machine in het andere netwerk.
    >
    > Vervang `10.2.0.4` met de __interne IP-adres__ van uw aangepaste DNS-server in het virtuele netwerk.

    Het antwoord weergegeven die vergelijkbaar is met de volgende tekst:

    ```
    Server:         10.2.0.4
    Address:        10.2.0.4#53
    
    Non-authoritative answer:
    Name:   vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    Address: 10.2.0.4
    ```

    Tot nu toe opzoeken niet u het IP-adres van de andere netwerk zonder opgegeven IP-adres van de DNS-server.

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Het virtuele netwerk voor het gebruik van de aangepaste DNS-server configureren

Het virtuele netwerk voor het gebruik van de aangepaste DNS-server in plaats van de Azure recursieve naamomzetting configureren, gebruiken de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com), selecteer het virtuele netwerk en selecteer vervolgens __DNS-Servers__.

2. Selecteer __aangepaste__, en voer de __interne IP-adres__ van de aangepaste DNS-server. Selecteer ten slotte __opslaan__.

6. Open de DNS-server-machine in vnet1, en klik op **opnieuw**.  U moet alle virtuele machines opnieuw opstarten in het virtuele netwerk waarmee de DNS-configuratie van kracht.
7. Herhaal de stappen configureren de aangepaste DNS-server voor vnet2.

Als u wilt testen van de DNS-configuratie, kunt u verbinding maken met de twee DNS-virtuele machines met behulp van SSH en de DNS-server van het virtuele netwerk met behulp van de hostnaam ping. Als dit niet werkt, gebruikt u de volgende opdracht uit om DNS-status te controleren:

```bash
sudo service bind9 status
```

## <a name="create-hbase-clusters"></a>HBase-clusters maken

Maak een HBase-cluster in elk van de twee virtuele netwerken met de volgende configuratie:

- **De naam van resourcegroep**: naam van de dezelfde resourcegroep gebruiken als u de virtuele netwerken die zijn gemaakt.
- **Clustertype**: HBase
- **Versie**: HBase 1.1.2 (HDI 3.6)
- **Locatie**: gebruik dezelfde locatie als het virtuele netwerk.  Vnet1 is standaard *VS-West*, en vnet2 is *VS-Oost*.
- **Opslag**: Maak een nieuw opslagaccount voor het cluster.
- **Virtueel netwerk** (van de geavanceerde instellingen op de portal): Selecteer vnet1 in de laatste procedure hebt gemaakt.
- **Subnet**: de standaardnaam die wordt gebruikt in de sjabloon is **subnet1**.

Om te controleren of dat de omgeving goed is geconfigureerd, moet u de FQDN van het hoofdknooppunt tussen de twee clusters met pingen.

## <a name="load-test-data"></a>Test-gegevens laden

Wanneer u een cluster repliceert, moet u de tabellen die u wilt repliceren. In deze sectie kunt u enkele gegevens laden in het broncluster. In de volgende sectie schakelt u replicatie tussen de twee clusters.

Maakt een **contactpersonen** tabel en voeg enkele gegeven in de tabel, volg de instructies op [HBase-zelfstudie: aan de slag met Apache HBase in HDInsight](apache-hbase-tutorial-get-started-linux.md).

## <a name="enable-replication"></a>Replicatie inschakelen

De volgende stappen wordt beschreven hoe u het script van de actie script aanroepen vanuit de Azure-portal. Zie voor meer informatie over het uitvoeren van een scriptactie met behulp van Azure PowerShell en de klassieke Azure-CLI [aanpassen HDInsight-clusters met behulp van scriptacties](../hdinsight-hadoop-customize-cluster-linux.md).

**HBase-replicatie vanuit Azure portal in te schakelen**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het bron-HBase-cluster.
3. Selecteer in het menu cluster **scriptacties**.
4. Aan de bovenkant van de pagina, selecteer **nieuwe verzenden**.
5. Selecteer of Voer de volgende informatie:

  1. **Naam**: Voer **inschakelen replicatie**.
  2. **Bash-Script-URL**: Voer **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  3.  **HEAD**: Zorg ervoor dat deze optie is geselecteerd. Schakel de andere typen.
  4. **Parameters**: het volgende voorbeeldparameters replicatie inschakelen voor alle bestaande tabellen en alle gegevens van het broncluster naar het doelcluster kopiëren:

          -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata
    
    >[!note]
    >
    > Gebruik hostnaam in plaats van de FQDN-naam voor de bron- en doelserver cluster DNS-naam.

6. Selecteer **Maken**. Het script kan even duren om uit te voeren, met name wanneer u de **- copydata** argument.

Vereiste argumenten:

|Naam|Beschrijving|
|----|-----------|
|-s,--src-cluster | Hiermee geeft u de DNS-naam van de bron HBase-cluster. Bijvoorbeeld: hbsrccluster -s-,--src-cluster hbsrccluster = |
|-d--dst-cluster | Hiermee geeft u de DNS-naam van de HBase doelclusterknooppunten (replica). Bijvoorbeeld: dsthbcluster -s-,--src-cluster dsthbcluster = |
|-sp,--src-ambari-wachtwoord | Hiermee geeft u het beheerderswachtwoord voor Ambari op de bron HBase-cluster. |
|-dp,--dst-ambari-wachtwoord | Hiermee geeft u het beheerderswachtwoord voor Ambari op de bestemming HBase-cluster.|

Optionele argumenten:

|Naam|Beschrijving|
|----|-----------|
|-su,--src-ambari-gebruiker | Hiermee geeft u de beheerdersnaam voor de Ambari op de bron HBase-cluster. De standaardwaarde is **admin**. |
|-database-eenheid,--dst-ambari-gebruiker | Hiermee geeft u de beheerdersnaam voor de Ambari op de bestemming HBase-cluster. De standaardwaarde is **admin**. |
|-t-,--lijst van de tabel | Hiermee geeft u de tabellen worden gerepliceerd. Bijvoorbeeld:--lijst van de tabel = "table1, table2; Tabel3". Als u geen tabellen opgeeft, worden alle bestaande HBase-tabellen worden gerepliceerd.|
|-m-,--computer | Hiermee geeft u het hoofdknooppunt waarop de scriptactie wordt uitgevoerd. De waarde is een **hn1** of **hn0**. Omdat de **hn0** hoofdknooppunt is meestal drukker, wordt u aangeraden **hn1**. Gebruik deze optie als u nu het script $0 worden uitgevoerd als een scriptactie van de HDInsight-portal of Azure PowerShell.|
|-cp - copydata | Hiermee kunt de migratie van bestaande gegevens op de tabellen waarin de replicatie is ingeschakeld. |
|-rpm, -replicatie-phoenix-metagegevens | Hiermee schakelt u replicatie voor Phoenix systeemtabellen. <br><br>*Gebruik deze optie Wees voorzichtig.* U wordt aangeraden Phoenix-tabellen op clusters van de replica opnieuw te maken voordat u dit script gebruiken. |
|-h,--Help-informatie | Geeft informatie over het gebruik. |

De `print_usage()` sectie van de [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) heeft een gedetailleerde beschrijving van parameters.

Nadat de scriptactie is geïmplementeerd, kunt u SSH verbinding maken met de HBase doelclusterknooppunten en controleer vervolgens of dat de gegevens zijn gerepliceerd.

### <a name="replication-scenarios"></a>Replicatiescenario 's

De volgende lijst ziet u soms algemeen gebruik en de bijbehorende parameterinstellingen:

- **Replicatie inschakelen voor alle tabellen tussen de twee clusters met**. In dit scenario vereist niet kopiëren of migreren van bestaande gegevens in de tabellen en maakt geen gebruik van Phoenix-tabellen. Gebruik de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Replicatie inschakelen voor specifieke tabellen**. Inschakelen van replicatie op table1, table2 en Tabel3, gebruikt u de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Replicatie inschakelen voor specifieke tabellen en kopieer de bestaande gegevens**. Inschakelen van replicatie op table1, table2 en Tabel3, gebruikt u de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Replicatie inschakelen voor alle tabellen en Phoenix metagegevens repliceren van bron naar bestemming**. Phoenix metagegevens replicatie is niet ideaal. Gebruik deze voorzichtig. Gebruik de volgende parameters:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Kopiëren en migreren van gegevens

Er zijn twee afzonderlijke script actie scripts beschikbaar zijn voor het kopiëren of migreren van gegevens, nadat replicatie is ingeschakeld:

- [Script voor kleine tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (tabellen die zijn een paar GB in grootte en de algehele kopie is voltooid in minder dan een uur verwacht)

- [Script voor grote tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (tabellen die naar verwachting langer dan één uur om te kopiëren)

U kunt dezelfde procedure die wordt beschreven in volgen [inschakelen replicatie](#enable-replication) voor het aanroepen van de scriptactie. Gebruik de volgende parameters:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

De `print_usage()` sectie van de [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) is een gedetailleerde beschrijving van parameters.

### <a name="scenarios"></a>Scenario's

- **Specifieke tabellen (test1, test2 en test3) voor alle rijen bewerkt tot op heden kopiëren (huidige tijdstempel)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  Of:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Specifieke tabellen met een opgegeven tijdperiode kopiëren**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Replicatie uitschakelen

U kunt replicatie uitschakelen met een ander script actie-script uit [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). U kunt dezelfde procedure die wordt beschreven in volgen [inschakelen replicatie](#enable-replication) voor het aanroepen van de scriptactie. Gebruik de volgende parameters:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

De `print_usage()` sectie van de [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) heeft een gedetailleerde beschrijving van parameters.

### <a name="scenarios"></a>Scenario's

- **Replicatie uitschakelen in alle tabellen**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  of

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Schakel de replicatie op de opgegeven tabellen (table1, table2 en Tabel3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over het instellen van HBase-replicatie in een virtueel netwerk, of tussen twee virtuele netwerken. Voor meer informatie over HDInsight en HBase, Zie de volgende artikelen:

* [Aan de slag met Apache HBase in HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Overzicht van HDInsight HBase](./apache-hbase-overview.md)
* [HBase-clusters maken in Azure Virtual Network](./apache-hbase-provision-vnet.md)

