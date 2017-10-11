---
title: Uitvoeren van een MariaDB (MySQL)-cluster in Azure | Microsoft Docs
description: Maken van een MariaDB + Galera MySQL-cluster op virtuele machines in Azure
services: virtual-machines-linux
documentationcenter: 
author: sabbour
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: 53e9bf18b26338212411ea7c4f260eb308486738
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL)-cluster: zelfstudie voor Azure
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) en klassieke. Dit artikel is van toepassing op het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Azure Resource Manager-model gebruiken.

> [!NOTE]
> MariaDB Enterprise-cluster is nu beschikbaar in Azure Marketplace. De nieuwe aanbieding implementeren automatisch een MariaDB Galera-cluster op Azure Resource Manager. Moet u de nieuwe aanbieding van [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Dit artikel ziet u het maken van een model met meerdere [Galera](http://galeracluster.com/products/) cluster [MariaDBs](https://mariadb.org/en/about/) (een robuuste, schaalbare en betrouwbare onverwachte ter vervanging van MySQL) werkt in een maximaal beschikbare omgeving op Azure virtuele machines.

## <a name="architecture-overview"></a>Overzicht van de architectuur
Dit artikel wordt beschreven hoe u de volgende stappen uit:

- Maak een cluster met drie knooppunten.
- De gegevensschijven scheiden van de besturingssysteemschijf.
- De gegevensschijven in RAID-0/striped instelling te verhogen IOP's maken.
- Azure Load Balancer gebruiken voor taakverdeling voor de drie knooppunten.
- Om te beperken herhalende werk, een VM-installatiekopie waarin MariaDB + Galera maken en deze gebruiken om het andere cluster virtuele machines te maken.

![Systeemarchitectuur](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> In dit onderwerp worden de [Azure CLI](../../../cli-install-nodejs.md) hulpprogramma's, dus zorg ervoor dat u deze downloaden en verbind ze met uw Azure-abonnement volgens de instructies. Als u een verwijzing naar de opdrachten die beschikbaar zijn in de Azure CLI moet, Zie de [Azure CLI-opdrachten](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). U moet ook [maken van een SSH-sleutel voor verificatie] en noteer de locatie van het .pem-bestand.
>
>

## <a name="create-the-template"></a>De sjabloon maken
### <a name="infrastructure"></a>Infrastructuur
1. Maak een affiniteitsgroep voor het opslaan van de resources samen.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. Een virtueel netwerk maken.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. Een opslagaccount voor het hosten van alle onze schijven maken. U mag niet meer dan 40 intensief gebruikte schijven op hetzelfde opslagaccount om te voorkomen dat de 20.000 IOPS account opslaglimiet roept plaatsen. In dit geval je ruim onder deze limiet, zodat u alles wat u wilt opslaan op dezelfde account voor de eenvoud.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. De naam van de installatiekopie van de virtuele machine CentOS 7 vinden.

        azure vm image list | findstr CentOS
   De uitvoer is ongeveer `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Gebruikt u de naam in de volgende stap.
5. De VM-sjabloon maken en /path/to/key.pem vervangen door het pad waar u de gegenereerde .pem SSH-sleutel hebt opgeslagen.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. Vier 500 GB gegevensschijven koppelen aan de virtuele machine voor gebruik in de RAID-configuratie.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. SSH aan te melden bij de virtuele machine die u hebt gemaakt op mariadbhatemplate.cloudapp.net:22-sjabloon gebruiken en verbinding maken met behulp van uw persoonlijke sleutel.

### <a name="software"></a>Software
1. Haal de hoofdmap.

        sudo su

2. Ondersteuning voor RAID installeren:

    a. Mdadm installeren.

              yum install mdadm

    b. Maak de RAID 0/stripe-configuratie met een bestandssysteem EXT4.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Het punt Koppelmap maken.

              mkdir /mnt/data
    d. De UUID van het zojuist gemaakte RAID-apparaat worden opgehaald.

              blkid | grep /dev/md0
    e. /Etc/fstab bewerken.

              vi /etc/fstab
    f. Toevoegen van het apparaat zodat automatisch koppelen op opnieuw opstarten, de UUID vervangen door de waarde die is verkregen van de vorige **blkid** opdracht.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Koppel de nieuwe partitie.

              mount /mnt/data

3. MariaDB installeren.

    a. Het bestand MariaDB.repo maken.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Vul het repo-bestand met de volgende inhoud:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. Om conflicten te voorkomen, verwijder bestaande postfix en mariadb-bibliotheken.

           yum remove postfix mariadb-libs-*
    d. Installeer MariaDB met Galera.

           yum install MariaDB-Galera-server MariaDB-client galera

4. De gegevensmap MySQL verplaatsen naar het apparaat RAID-blok.

    a. De huidige MySQL-map kopiëren naar de nieuwe locatie en de oude map verwijderen.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Stel machtigingen voor de nieuwe map dienovereenkomstig.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Maak een symlink die de oude map naar de nieuwe locatie op de RAID-partitie verwijst.

           ln -s /mnt/data/mysql /var/lib/mysql

5. Omdat [SELinux verstoort clusterbewerkingen](http://galeracluster.com/documentation-webpages/configuration.html#selinux), is het nodig dat u deze uitschakelt voor de huidige sessie. Bewerken `/etc/selinux/config` dat u deze uitschakelt voor latere opnieuw wordt opgestart.

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
6. Valideren van MySQL wordt uitgevoerd.

   a. MySQL wordt gestart.

           service mysql start
   b. Beveiligen van de MySQL-installatie, het root-wachtwoord instellen, anonieme gebruikers om uit te schakelen externe hoofdmap aanmelding verwijderen en de testdatabase verwijderen.

           mysql_secure_installation
   c. Maak een gebruiker op de database voor bewerkingen voor een cluster, en desgewenst voor uw toepassingen.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. MySQL stoppen.

            service mysql stop
7. Maak een configuratie-tijdelijke aanduiding.

   a. De MySQL-configuratie voor het maken van een tijdelijke aanduiding voor de clusterinstellingen bewerken. Geen vervanging voor de  **`<Variables>`**  of verwijder de opmerking nu. Dat gebeurt nadat u een virtuele machine met deze sjabloon maken.

            vi /etc/my.cnf.d/server.cnf
   b. Bewerk de  **[galera]**  sectie en maak deze.

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
8. Vereiste poorten op de firewall openen met behulp van FirewallD op CentOS 7.

   * MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Laden van de firewall:`firewall-cmd --reload`

9. Het systeem voor prestaties optimaliseren. Zie voor meer informatie [prestaties afstemmen strategie](optimize-mysql.md).

   a. Het configuratiebestand MySQL opnieuw bewerken.

            vi /etc/my.cnf.d/server.cnf
   b. Bewerk de **[mariadb]** sectie en het toevoegen van de volgende inhoud:

   > [!NOTE]
   > We raden aan dat innodb\_buffer\_pool_size 70 procent van het geheugen van de VM is. In dit voorbeeld is deze ingesteld op 2,45 GB voor de virtuele machine van Azure met 3.5 GB aan RAM-geheugen van het medium.
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. Stop MySQL, MySQL-service worden uitgevoerd op starten om te voorkomen dat het cluster verstoren bij het toevoegen van een knooppunt uitschakelen en inrichting ervan ongedaan maakt de machine.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. Vastleggen van de virtuele machine via de portal. (Momenteel [#1268 in de Azure CLI-hulpprogramma's uitgeven](https://github.com/Azure/azure-xplat-cli/issues/1268) beschrijving van het feit afbeeldingen die zijn vastgelegd door de Azure CLI-hulpprogramma's worden niet vastgelegd voor de schijven bijgesloten gegevens.)

    a. Sluit de machine via de portal.

    b. Klik op **vastleggen** en geef de naam van de installatiekopie als **mariadb-galera-image**. Geef een beschrijving op en controleer 'Ik hebben waagent uitvoeren'.
      
      ![De virtuele machine vastleggen](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a>Het cluster maken
Maak drie virtuele machines met de sjabloon die u hebt gemaakt, en vervolgens configureren en starten van het cluster.

1. De eerste CentOS 7 VM maken van de installatiekopie van het mariadb-galera-installatiekopie die u hebt gemaakt, met de volgende informatie:

 - Virtuele-netwerknaam: mariadbvnet
 - Subnet: mariadb
 - Grootte van de computer: gemiddeld
 - Cloudservicenaam: mariadbha (of de naam die u wilt worden benaderd via mariadbha.cloudapp.net)
 - Computernaam: mariadb1
 - Gebruikersnaam: azureuser
 - SSH-toegang: ingeschakeld
 - Het .pem-bestand van de SSH-certificaat doorgeeft en /path/to/key.pem vervangen door het pad waar u de gegenereerde .pem SSH-sleutel hebt opgeslagen.

   > [!NOTE]
   > De volgende opdrachten worden gesplitst over meerdere regels voor de duidelijkheid, maar u moet elk als één regel invoeren.
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
2. Twee meer virtuele machines door deze te verbinden met de cloudservice mariadbha maken. Wijzig de naam van de virtuele machine en de SSH-poort in een unieke poort niet conflicteert met andere virtuele machines in dezelfde cloudservice.

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
3. U moet de interne IP-adres van elk van de drie virtuele machines ophalen voor de volgende stap:

    ![IP-adres ophalen](./media/mariadb-mysql-cluster/IP.png)
4. SSH gebruiken om te melden bij de drie virtuele machines en bewerken van het configuratiebestand op elk van deze.

        sudo vi /etc/my.cnf.d/server.cnf

    Verwijder de opmerking  **`wsrep_cluster_name`**  en  **`wsrep_cluster_address`**  door het verwijderen van de  **#**  aan het begin van de regel.
    Verder vervangen  **`<ServerIP>`**  in  **`wsrep_node_address`**  en  **`<NodeName>`**  in  **`wsrep_node_name`**  met de VM-IP-adres een naam, respectievelijk, en verwijder deze regels ook de opmerkingen.
5. Start het cluster op MariaDB1 en laat het uitgevoerd bij het opstarten.

        sudo service mysql bootstrap
        chkconfig mysql on
6. Start MySQL op MariaDB2 en MariaDB3 en laat het uitgevoerd bij het opstarten.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a>Taakverdeling het cluster
Wanneer u de geclusterde virtuele machines hebt gemaakt, kunt u ze in een beschikbaarheidsset aangeroepen clusteravset om ervoor te zorgen dat ze op verschillende domeinen met fouten en update domeinen zijn geplaatst en nooit onderhoud op alle computers in één keer die Azure biedt toegevoegd. Deze configuratie voldoet aan de vereisten worden ondersteund door Azure service level agreement (SLA).

Nu Azure Load Balancer gebruiken voor een evenwicht tussen aanvragen tussen de drie knooppunten.

De volgende opdrachten uitvoeren op uw computer met behulp van de Azure CLI.

De parameters-structuur van de opdracht is:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

De CLI Hiermee stelt u het interval van load balancer-test op 15 seconden en dit is mogelijk iets te lang. Dit wijzigen in de portal onder **eindpunten** voor een van de virtuele machines.

![Eindpunt bewerken](./media/mariadb-mysql-cluster/Endpoint.PNG)

Selecteer **opnieuw configureren van de Set met gelijke taakverdeling**.

![De Set met gelijke taakverdeling configureren](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Wijziging **Probe Interval** op 5 seconden en sla de wijzigingen.

![Interval voor wijzigen van test](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a>Het cluster valideren
De vaste werk wordt uitgevoerd. Het cluster toegankelijk moet zijn nu op `mariadbha.cloudapp.net:3306`, die de load balancer en route-aanvragen tussen de drie virtuele machines treffers soepel en efficiënt.

Uw favoriete MySQL-client gebruiken om verbinding maken of verbinding maakt vanaf een van de virtuele machines om te controleren of dit cluster werkt.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Vervolgens maakt u een database en deze vullen met gegevens.

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
In dit artikel wordt gemaakt van een drie-knooppunt MariaDB + Galera maximaal beschikbare cluster in Azure virtuele machines actief CentOS 7. De virtuele machines zijn taakverdeling met Azure Load Balancer.

U kunt kijken [MySQL-cluster op Linux op een andere manier](mysql-cluster.md) en manieren om te [optimaliseren en test de prestaties van MySQL op Azure Linux VM's](optimize-mysql.md).

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
[maken van een SSH-sleutel voor verificatie]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in the Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
