---
title: Uitvoeren van een MariaDB (MySQL)-cluster in Azure | Microsoft Docs
description: Maken van een MariaDB + Galera MySQL-cluster op Azure virtual machines
services: virtual-machines-linux
documentationcenter: ''
author: sabbour
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: 2cdc58a827f696d62e6240b90202ee04ce371d07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426850"
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL)-cluster: zelfstudie voor Azure
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) en klassiek. Dit artikel is van toepassing op het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Azure Resource Manager-model gebruiken.

> [!NOTE]
> MariaDB Enterprise cluster is nu beschikbaar in de Azure Marketplace. De nieuwe aanbieding wordt automatisch geïmplementeerd voor een MariaDB Galera-cluster op Azure Resource Manager. Moet u de nieuwe aanbieding van [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Dit artikel leest u over het maken van een meerdere masters [Galera](http://galeracluster.com/products/) cluster [MariaDBs](https://mariadb.org/en/about/) (een robuuste, schaalbare en betrouwbare drop-in replacement voor MySQL) om te werken in een maximaal beschikbare omgeving op Azure virtuele machines.

## <a name="architecture-overview"></a>Overzicht van de architectuur
In dit artikel wordt beschreven hoe u de volgende stappen uit:

- Maak een cluster met drie knooppunten.
- De gegevensschijven te scheiden van de besturingssysteemschijf.
- De gegevensschijven maken in RAID-0/striped instelling voor het verhogen van IOPS.
- Azure Load Balancer gebruiken om taken te verdelen voor de drie knooppunten.
- Maken van een VM-installatiekopie met MariaDB + Galera om te beperken terugkerende werk, en het andere cluster virtuele machines maken.

![Systeemarchitectuur](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> In dit onderwerp wordt gebruikgemaakt van de [Azure CLI](../../../cli-install-nodejs.md) hulpprogramma's, dus zorg ervoor dat u ze downloaden en verbind deze met uw Azure-abonnement op basis van de instructies. Als u een verwijzing naar de opdrachten die beschikbaar zijn in de Azure CLI nodig hebt, raadpleegt u de [overzicht van Azure CLI-opdrachten](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). U moet ook [een SSH-sleutel voor verificatie maken] en noteer de locatie van het .pem-bestand.
>
>

## <a name="create-the-template"></a>De sjabloon maken
### <a name="infrastructure"></a>Infrastructuur
1. Maak een affiniteitsgroep die de resources samen.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
1. Maak een virtueel netwerk.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
1. Maak een opslagaccount voor het hosten van alle onze schijven. U mag niet meer dan 40 intensief gebruikte schijven te plaatsen op hetzelfde opslagaccount om te voorkomen dat de 20.000 IOP's account opslaglimiet bereikt. In dit geval u goed onder deze limiet, zodat u alles op hetzelfde account voor het gemak wilt opslaan.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
1. Zoek de naam van de virtuele machine-installatiekopie van CentOS 7.

        azure vm image list | findstr CentOS
   De uitvoer zal er ongeveer als worden `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Deze naam in de volgende stap gebruiken.
1. De VM-sjabloon maken en /path/to/key.pem vervangen door het pad waar u de gegenereerde .pem SSH-sleutel hebt opgeslagen.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
1. Vier 500 GB-gegevensschijven koppelen aan de virtuele machine voor gebruik in de RAID-configuratie.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
1. SSH gebruiken om aan te melden bij de sjabloon voor virtuele machine die u hebt gemaakt op mariadbhatemplate.cloudapp.net:22 en verbinding maken met behulp van uw persoonlijke sleutel.

### <a name="software"></a>Software
1. Haal de hoofdmap.

        sudo su

1. Ondersteuning voor RAID installeren:

    a. Mdadm installeren.

              yum install mdadm

    b. Maak de RAID 0/stripe-configuratie met een EXT4 file system.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. De koppelpunt punt map niet maken.

              mkdir /mnt/data
    d. De UUID van het zojuist gemaakte RAID-apparaat worden opgehaald.

              blkid | grep /dev/md0
    e. Bewerk/etc/fstab.

              vi /etc/fstab
    f. Toevoegen van het apparaat om in te schakelen automatisch koppelen op opnieuw opstarten, de UUID vervangen door de waarde die is verkregen uit de vorige **blkid** opdracht.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Koppel de nieuwe partitie.

              mount /mnt/data

1. MariaDB installeren.

    a. Het bestand MariaDB.repo maken.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Vul de opslagplaats-bestand met de volgende inhoud:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. Om conflicten te voorkomen, verwijdert u de bestaande postfix en mariadb-bibliotheken.

           yum remove postfix mariadb-libs-*
    d. MariaDB met Galera installeren.

           yum install MariaDB-Galera-server MariaDB-client galera

1. De map met MySQL gegevens verplaatsen naar het apparaat blokkeren voor RAID.

    a. De huidige MySQL-map kopiëren naar de nieuwe locatie en de oude map verwijderen.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Machtigingen instellen voor de nieuwe map dienovereenkomstig.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Maak een symlink die de oude map naar de nieuwe locatie op de RAID-partitie verwijst.

           ln -s /mnt/data/mysql /var/lib/mysql

1. Omdat [SELinux verstoort de clusterbewerkingen](http://galeracluster.com/documentation-webpages/configuration.html#selinux), moet u deze uitschakelen voor de huidige sessie. Bewerken `/etc/selinux/config` uitschakelen voor latere opnieuw wordt opgestart.

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
1. Valideer MySQL wordt uitgevoerd.

   a. Start MySQL.

           service mysql start
   b. Beveiligen van de MySQL-installatie, het root-wachtwoord instellen, verwijderen van anonieme gebruikers externe hoofdmap aanmelding uitschakelen en verwijderen van de testdatabase.

           mysql_secure_installation
   c. Maak een gebruiker op de database voor bewerkingen voor een cluster, en (optioneel) voor uw toepassingen.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. MySQL stoppen.

            service mysql stop
1. Maak een tijdelijke aanduiding voor configuratie.

   a. De MySQL-configuratie voor het maken van een tijdelijke aanduiding voor de clusterinstellingen bewerken. Geen vervanging voor de **`<Variables>`** of verwijder de opmerking nu bij. Dit gaat gebeuren, nadat u een virtuele machine met deze sjabloon maken.

            vi /etc/my.cnf.d/server.cnf
   b. Bewerk de **[galera]** sectie en deze wissen.

   c. Bewerk de **[mariadb]** sectie.

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server
1. Vereiste poorten op de firewall openen met behulp van FirewallD op CentOS 7.

   * MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA IS: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Laad de firewall: `firewall-cmd --reload`

1. Het systeem voor de prestaties optimaliseren. Zie voor meer informatie, [strategie voor afstemmen van prestaties](optimize-mysql.md).

   a. De MySQL-configuratiebestand opnieuw bewerken.

            vi /etc/my.cnf.d/server.cnf
   b. Bewerk de **[mariadb]** sectie en voeg de volgende inhoud toe:

   > [!NOTE]
   > We raden aan dat innodb\_buffer\_pool_size 70 procent van het geheugen van de virtuele machine is. In dit voorbeeld is het 2,45 GB is ingesteld voor de Azure-VM met 3,5 GB aan RAM-geheugen.
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
1. MySQL stoppen, uitschakelen van de service MySQL niet kunnen worden uitgevoerd bij het opstarten om te voorkomen dat het cluster te onderbreken wanneer u een knooppunt toevoegt en de inrichting van de machine.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
1. Vastleggen van de virtuele machine via de portal. (Op dit moment [#1268 in de Azure CLI-hulpprogramma's uitgeven](https://github.com/Azure/azure-xplat-cli/issues/1268) beschrijving van het feit dat installatiekopieën die zijn vastgelegd door de Azure CLI-hulpprogramma's niet de gekoppelde gegevensschijven vastlegt.)

    a. Sluit de machine via de portal.

    b. Klik op **vastleggen** en geeft u de naam van de installatiekopie als **mariadb-galera-image**. Geef een beschrijving op en controleer "Ik uitvoeren waagent."
      
      ![De virtuele machine vastleggen](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a>Het cluster maken
Drie virtuele machines maken met de sjabloon die u hebt gemaakt, en vervolgens configureren en starten van het cluster.

1. De eerste CentOS 7-VM maken van de installatiekopie van het mariadb-galera-installatiekopie die u hebt gemaakt, met de volgende informatie:

 - Virtuele-netwerknaam: mariadbvnet
 - Subnet: mariadb
 - Grootte van de computer: gemiddeld
 - Cloudservicenaam: mariadbha (of de naam die u wilt worden geopend via mariadbha.cloudapp.net)
 - Computernaam: mariadb1
 - Gebruikersnaam: azureuser
 - SSH-toegang: ingeschakeld
 - Het .pem-bestand van de SSH-certificaat te geven en /path/to/key.pem vervangen door het pad waar u de gegenereerde .pem SSH-sleutel hebt opgeslagen.

   > [!NOTE]
   > De volgende opdrachten zijn verdeeld over meerdere regels voor de duidelijkheid, maar u moet elk als één regel invoeren.
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
1. Maak twee meer virtuele machines door ze naar de cloudservice mariadbha verbinding te maken. Wijzig de naam van de virtuele machine en de SSH-poort in een unieke poort niet conflicteert met andere virtuele machines in dezelfde cloudservice.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  Voor MariaDB3:

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
1. U moet het interne IP-adres van elk van de drie VM's ophalen voor de volgende stap:

    ![IP-adres ophalen](./media/mariadb-mysql-cluster/IP.png)
1. SSH gebruiken om te melden bij de drie virtuele machines en het configuratiebestand op elk van deze bewerken.

        sudo vi /etc/my.cnf.d/server.cnf

    Verwijder de opmerking bij **`wsrep_cluster_name`** en **`wsrep_cluster_address`** door het verwijderen van de **#** aan het begin van de regel.
    Bovendien vervangen **`<ServerIP>`** in **`wsrep_node_address`** en **`<NodeName>`** in **`wsrep_node_name`** met de De VM-IP-adres een naam, respectievelijk, en verwijder de opmerkingen in deze regels ook.
1. Start het cluster op MariaDB1 en laat het bij het opstarten worden uitgevoerd.

        sudo service mysql bootstrap
        chkconfig mysql on
1. Start MySQL op MariaDB2 en MariaDB3 en laat het bij het opstarten worden uitgevoerd.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a>Taakverdeling het cluster
Wanneer u de geclusterde virtuele machines gemaakt, kunt u ze in een beschikbaarheidsset met de naam clusteravset om ervoor te zorgen dat ze zijn geplaatst op verschillende foutdomeinen en updatedomeinen en nooit onderhoud op alle computers in één keer die Azure worden toegevoegd. Deze configuratie voldoet aan de vereisten om te worden ondersteund door de Azure service level agreement (SLA).

Azure Load Balancer nu gebruiken om te verdelen van aanvragen tussen de drie knooppunten.

Voer de volgende opdrachten op uw computer met behulp van de Azure CLI.

De structuur van de parameters van de opdracht is: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

De CLI Hiermee stelt u de load balancer-testinterval op 15 seconden en die mogelijk iets te lang. Wijzigingen aanbrengen in de portal onder **eindpunten** voor het gebruik van de virtuele machines.

![Eindpunt bewerken](./media/mariadb-mysql-cluster/Endpoint.PNG)

Selecteer **opnieuw configureren van de Load Balancer-Set**.

![Configureren van de load balancer-Set](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Wijziging **Probe Interval** op 5 seconden en sla de wijzigingen.

![Testinterval wijzigen](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a>Het cluster valideren
Het harde werk wordt uitgevoerd. Het cluster toegankelijk moet zijn nu op `mariadbha.cloudapp.net:3306`, waarvan de load balancer en route-aanvragen tussen de drie virtuele machines treffers soepel en efficiënt.

Gebruik uw favoriete MySQL-client verbinding maken of verbinding maken vanaf een van de virtuele machines om te controleren of dit cluster werkt.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Maak een database en deze vullen met gegevens.

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

De database die u hebt gemaakt, retourneert de volgende tabel:

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u een MariaDB drie knooppunten gemaakt + Galera maximaal beschikbare cluster op Azure virtual machines actieve CentOS 7. De virtuele machines worden gelijkmatig verdeeld met Azure Load Balancer.

Kunt u om te kijken naar [een andere manier om de MySQL-cluster op Linux](mysql-cluster.md) en manieren om [te optimaliseren en test de prestaties van MySQL op Azure Linux Virtual machines](optimize-mysql.md).

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating the template]:#creating-the-template
[Creating the cluster]:#creating-the-cluster
[Load balancing the cluster]:#load-balancing-the-cluster
[Validating the cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[een SSH-sleutel voor verificatie maken]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in the Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
