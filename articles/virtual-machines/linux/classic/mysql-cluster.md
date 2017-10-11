---
title: MySQL clusterize met gelijke taakverdeling sets | Microsoft Docs
description: Instellen van een taakverdeling hoge beschikbaarheid Linux MySQL-cluster met het klassieke implementatiemodel in Azure gemaakt
services: virtual-machines-linux
documentationcenter: 
author: bureado
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: 4eaf86c9ac3e4dc2b51b88383626eda774cab0e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Sets met gelijke taakverdeling kunt clusterize MySQL op Linux
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) en klassieke. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Een [Resource Manager-sjabloon](https://azure.microsoft.com/documentation/templates/mysql-replication/) is beschikbaar als u moet een MySQL-cluster implementeren.

Dit artikel wordt verkend en ziet u de verschillende methoden beschikbaar om maximaal beschikbare op basis van Linux-services op Microsoft Azure kunt verkennen van hoge beschikbaarheid van de MySQL-Server als een primer te implementeren. Een video ter illustratie van deze benadering is beschikbaar op [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Er wordt een overzicht van niets wordt gedeeld, twee knooppunten, één master MySQL oplossing met hoge beschikbaarheid op basis van DRBD Corosync en pacemaker heeft. Slechts één knooppunt MySQL op een tijdstip wordt uitgevoerd. Lezen en schrijven van de resource DRBD is ook beperkt tot slechts één knooppunt tegelijk.

Er is niet nodig om een VIP-oplossing zoals LVS, omdat u sets met gelijke taakverdeling in Microsoft Azure voor round robin-functionaliteit en eindpunt detectie, verwijderen en correcte herstel van de VIP. De VIP is een algemeen routeerbaar IPv4-adres toegewezen door Microsoft Azure wanneer u eerst de cloudservice maken.

Er zijn andere mogelijke architecturen voor MySQL, met inbegrip van de volgende werkdag Cluster, Percona Galera en verschillende middleware oplossingen, waaronder ten minste één beschikbaar als een virtuele machine op [VM Depot](http://vmdepot.msopentech.com). Zolang deze oplossingen op unicast-vs. multicast of broadcast repliceren kunnen en Vertrouw niet op de gedeelde opslag of meerdere netwerkinterfaces, zijn de scenario's moet eenvoudig te implementeren in Microsoft Azure.

Deze clustering architecturen kunnen worden uitgebreid naar andere producten, zoals PostgreSQL en OpenLDAP op soortgelijke wijze. Bijvoorbeeld: deze procedure taakverdeling met waarbij niets wordt gedeeld met succes is getest met meerdere master OpenLDAP en kunt u deze bekijken op ons blog Channel 9.

## <a name="get-ready"></a>Voorbereiden
U moet de volgende resources en de volgende mogelijkheden:

  - Een Microsoft Azure-account met een geldig abonnement ten minste twee virtuele machines maken (XS is gebruikt in dit voorbeeld)
  - Een netwerk en een subnet
  - Een affiniteitsgroep
  - Een beschikbaarheidsset
  - De mogelijkheid om te maken van VHD's in dezelfde regio bevinden als de service in de cloud en koppel deze aan de virtuele Linux-machines

### <a name="tested-environment"></a>Geteste omgeving
* Ubuntu 13.10
  * DRBD
  * MySQL-Server
  * Corosync en pacemaker heeft

### <a name="affinity-group"></a>Affiniteitsgroep
Maken van een affiniteitsgroep voor de oplossing aanmeldt bij de klassieke Azure portal selecteren **instellingen**, en het maken van een affiniteitsgroep. Toegewezen bronnen maken later wordt toegewezen aan deze groep affiniteit.

### <a name="networks"></a>Netwerken
Een nieuw netwerk wordt gemaakt en een subnet wordt gemaakt in het netwerk. Dit voorbeeld wordt een 10.10.10.0/24-netwerk met slechts één /24 subnet binnen.

### <a name="virtual-machines"></a>Virtuele machines
De eerste Ubuntu 13.10 VM wordt gemaakt met behulp van een afbeelding Endorsed Ubuntu en heet `hadb01`. Een nieuwe cloudservice wordt gemaakt in het proces, hadb aangeroepen. Deze naam ziet u de gedeelde, taakverdeling aard die de service hebben wanneer er meer resources worden toegevoegd. Het maken van `hadb01` wordt probleemloze ingevuld met behulp van de portal. Een eindpunt voor SSH wordt automatisch gemaakt en het nieuwe netwerk is geselecteerd. U kunt nu de beschikbaarheidsset voor de virtuele machines maken.

Nadat de eerste virtuele machine is gemaakt (technisch, wanneer de cloudservice is gemaakt), maakt u de tweede VM `hadb02`. Voor de tweede VM Ubuntu 13.10 VM uit de galerie met behulp van de portal, maar met een bestaande cloudservice `hadb.cloudapp.net`, in plaats van een nieuwe maken. De netwerk- en beschikbaarheid set moet automatisch worden geselecteerd. Een SSH-eindpunt wordt gemaakt, te.

Nadat beide virtuele machines zijn gemaakt, dient u de SSH-poort voor `hadb01` (TCP 22) en `hadb02` (automatisch toegewezen door Azure).

### <a name="attached-storage"></a>Gekoppelde opslag
Een nieuwe schijf koppelen aan beide virtuele machines en 5 GB schijven maken in het proces. De schijven worden in de VHD-container in het gebruik van de schijven van de belangrijkste besturingssysteem gehost. Nadat de schijven zijn gemaakt en gekoppeld, hoeft niet opnieuw opstarten van Linux omdat de kernel wordt het nieuwe apparaat wordt weergegeven. Dit apparaat is meestal `/dev/sdc`. Controleer `dmesg` voor de uitvoer.

Op elke virtuele machine, kunt u een partitie maken met behulp van `cfdisk` (primaire, Linux-partitie) en de nieuwe partitietabel schrijven. Maak een bestandssysteem geen op deze partitie.

## <a name="set-up-the-cluster"></a>De cluster instellen
Gebruik APT Corosync pacemaker heeft en DRBD installeren op beide Ubuntu VM. Om dit te doen met `apt-get`, voert u de volgende code:

    sudo apt-get install corosync pacemaker drbd8-utils.

Installeer geen MySQL op dit moment. Debian en Ubuntu installatiescripts een MySQL-gegevensmap op wordt geïnitialiseerd `/var/lib/mysql`, maar omdat de map wordt vervangen door een bestandssysteem DRBD, moet u MySQL om later te installeren.

Controleer of (met behulp van `/sbin/ifconfig`) dat beide VM gebruikmaakt van adressen in het subnet 10.10.10.0/24 en dat ze elkaar op naam kunnen pingen. U kunt ook `ssh-keygen` en `ssh-copy-id` om ervoor te zorgen dat beide virtuele machines kunnen communiceren via SSH zonder een wachtwoord.

### <a name="set-up-drbd"></a>DRBD instellen
Maak een DRBD resource die gebruikmaakt van de onderliggende `/dev/sdc1` partitie voor het produceren van een `/dev/drbd1` resource die kan worden geformatteerd met ext3 en gebruikt in de primaire en secundaire knooppunten.

1. Open `/etc/drbd.d/r0.res` en kopieer de volgende resourcedefinitie op beide virtuele machines:

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. Initialiseren van de resource met behulp van `drbdadm` op beide virtuele machines:

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. Op de primaire virtuele machine (`hadb01`), eigendom (primair) van de resource DRBD afdwingen:

        sudo drbdadm primary --force r0

Als u de inhoud van proc/drbd bekijken (`sudo cat /proc/drbd`) op beide virtuele machines, ziet u `Primary/Secondary` op `hadb01` en `Secondary/Primary` op `hadb02`, in overeenstemming met de oplossing op dit moment. De schijf 5 GB is gesynchroniseerd via het netwerk 10.10.10.0/24 gratis aan klanten.

Nadat de schijf wordt gesynchroniseerd, kunt u het bestandssysteem op `hadb01`. Voor testdoeleinden, hebben we ext2 gebruikt, maar de volgende code maakt een bestandssysteem ext3:

    mkfs.ext3 /dev/drbd1

### <a name="mount-the-drbd-resource"></a>De resource DRBD koppelen
U kunt nu de DRBD resources koppelen aan `hadb01`. Debian en afleidingen gebruik `/var/lib/mysql` als de gegevensmap van MySQL. Omdat u MySQL nog niet hebt geïnstalleerd, de map niet maken en koppelen van de resource DRBD. Als u deze optie, voert u de volgende code op `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a>MySQL instellen
Nu u klaar bent voor het installeren van MySQL op `hadb01`:

    sudo apt-get install mysql-server

Voor `hadb02`, hebt u twee opties. Mysql-server, die wordt /var/lib/mysql maken, vult u deze met een nieuwe map voor gegevens en verwijder vervolgens de inhoud, kunt u installeren. Als u deze optie, voert u de volgende code op `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

De tweede optie is failover `hadb02` en installeer vervolgens er mysql-server. Installatiescripts ziet de bestaande installatie en het heeft geen invloed.

De volgende code uitvoeren op `hadb01`:

    sudo drbdadm secondary –force r0

De volgende code uitvoeren op `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Als u niet wilt failover DRBD nu, is de eerste optie Hoewel weliswaar minder elegante eenvoudiger. Nadat u dit instellen, kunt u beginnen aan uw MySQL-database werkt. De volgende code uitvoeren op `hadb02` (of elk ander een van de servers is actief, volgens DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

> [!WARNING]
> Deze laatste instructie effectief dat verificatie is uitgeschakeld voor de hoofdgebruiker in deze tabel. Dit moet worden vervangen door uw productie-hoogwaardige instructies verlenen en is opgenomen alleen ter illustratie.

Als u wilt maken van query's van buiten de virtuele machines (dit is het doel van deze handleiding), moet u ook netwerken voor MySQL inschakelen. Open op beide VM `/etc/mysql/my.cnf` en Ga naar `bind-address`. Wijzig het adres van 127.0.0.1 op 0.0.0.0. Na het opslaan van het bestand geven een `sudo service mysql restart` op uw huidige primaire.

### <a name="create-the-mysql-load-balanced-set"></a>De set MySQL taakverdeling maken
Ga terug naar de portal, gaat u naar `hadb01`, en kies **eindpunten**. Voor het maken van een eindpunt (TCP 3306) MySQL kiezen uit de vervolgkeuzelijst en selecteer **set met gelijke taakverdeling maken nieuwe load**. Naam van het eindpunt taakverdeling `lb-mysql`. Stel **tijd** op 5 seconden, minimale.

Nadat u het eindpunt hebt gemaakt, gaat u naar `hadb02`, kies **eindpunten**, en een eindpunt te maken. Kies `lb-mysql`, en selecteer vervolgens MySQL in de vervolgkeuzelijst. U kunt ook de Azure CLI gebruiken voor deze stap.

U hebt nu alles wat die u nodig hebt voor handmatige bewerking van het cluster.

### <a name="test-the-load-balanced-set"></a>Testen van de set met gelijke taakverdeling
Tests kunnen worden uitgevoerd vanaf een externe computer via een MySQL-client of met behulp van bepaalde toepassingen, zoals phpMyAdmin uitgevoerd als een Azure-website. In dit geval gebruikt u het opdrachtregelprogramma van MySQL in een andere Linux:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Handmatig failover wordt uitgevoerd
U kunt failovers simuleren door MySQL afgesloten, overschakelen DRBD van primaire en MySQL opnieuw te starten.

Als u wilt uitvoeren van deze taak voert u de volgende code op hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Klik op hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Nadat u handmatig een failover, kunt u uw externe query herhalen en deze moet perfect werkt.

## <a name="set-up-corosync"></a>Corosync instellen
Corosync is de onderliggende clusterinfrastructuur vereist voor pacemaker heeft om te werken. Corosync is een splitsing van de CRM-functionaliteit voor Heartbeat (en andere methoden zoals Ultramonkey), terwijl pacemaker heeft meer lijken op Heartbeat in de functionaliteit blijft.

De belangrijkste beperking voor Corosync in Azure is dat Corosync multicast via broadcast boven unicast-communicatie verkiest, maar Microsoft Azure-netwerken alleen unicast ondersteunt.

Gelukkig heeft Corosync een werkende unicast-modus. De enige echte beperking is omdat alle knooppunten niet onderling communiceert zijn, u de knooppunten in uw configuratiebestanden moet, met inbegrip van de IP-adressen definiëren. We kunnen de Corosync voorbeeld-bestanden gebruiken voor Unicast en wijzig adres, knooppunt lijsten en logboekregistratie-mappen binden (Ubuntu gebruikt `/var/log/corosync` terwijl het voorbeeld gebruik bestanden `/var/log/cluster`), en hulpprogramma's voor quorum inschakelen.

> [!NOTE]
> Gebruik de volgende `transport: udpu` richtlijn en de handmatig gedefinieerde IP-adressen voor beide knooppunten.

De volgende code uitvoeren op `/etc/corosync/corosync.conf` voor beide knooppunten:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Kopieer dit configuratiebestand op beide virtuele machines en Corosync in beide knooppunten te starten:

    sudo service start corosync

Het cluster moet worden vastgesteld in de huidige ring kort na het starten van de service en quorum moet worden gevormd. We kunnen deze functionaliteit controleren aan de hand van Logboeken of door het uitvoeren van de volgende code:

    sudo corosync-quorumtool –l

Hier ziet u uitvoer die vergelijkbaar is met de volgende afbeelding:

![voorbeelduitvoer van corosync quorumtool - l](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a>Pacemaker heeft instellen
Het cluster pacemaker heeft gebruikt voor het bewaken voor resources, definiëren wanneer de primaire omlaag gaan en deze resources overschakelen naar de secundaire replica's. Resources kunnen worden gedefinieerd van een reeks beschikbaar scripts of van LSB (init-achtige)-scripts, onder andere opties.

We willen pacemaker ' eigenaar ' de DRBD-bron, het koppelpunt wordt gehost en de MySQL-service heeft. Als pacemaker heeft kunt en deactiveren DRBD activeren koppelen en ontkoppelen, en vervolgens start en stop MySQL in de juiste volgorde wanneer een probleem met de primaire gebeurt, kan setup is voltooid.

Wanneer u pacemaker heeft voor het eerst installeert, worden uw configuratie eenvoudige genoeg is, bijvoorbeeld:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. Controleer de configuratie door te voeren `sudo crm configure show`.
2. Maak een bestand (zoals `/tmp/cluster.conf`) met de volgende bronnen:

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. Het bestand laden in de configuratie. U hoeft alleen te doen in één knooppunt.

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. Zorg ervoor dat pacemaker heeft tijdens het opstarten in beide knooppunten begint:

        sudo update-rc.d pacemaker defaults

5. Met behulp van `sudo crm_mon –L`, Controleer of een van de knooppunten van het model voor het cluster is geworden en alle resources wordt uitgevoerd. U kunt koppelen en ps gebruiken om te controleren dat de bronnen worden uitgevoerd.

De volgende schermafbeelding ziet `crm_mon` met één knooppunt gestopt (afsluiten door te selecteren Ctrl + C):

![crm_mon knooppunt gestopt](./media/mysql-cluster/image002.png)

Deze schermafbeelding ziet u knooppunten, één master en een lager niveau bevindende:

![crm_mon operationele master/slave](./media/mysql-cluster/image003.png)

## <a name="testing"></a>Testen
U bent klaar voor de simulatie van een automatische failover. Er zijn twee manieren om dit te doen: zachte en harde.

De zachte manier maakt gebruik van de clusterfunctie afsluiten: ``crm_standby -U `uname -n` -v on``. Als u deze op de master gebruiken, neemt de slave op. Moet u dit opnieuw ingesteld op uitgeschakeld. Als u dit niet doet, ziet crm_mon één knooppunt op stand-by.

De vaste manier wordt afgesloten met de primaire virtuele machine (hadb01) via de portal of door het wijzigen van de (uitvoeringsniveau) op de virtuele machine (dat wil zeggen, stoppen, afsluiten). Hierdoor kunnen Corosync en pacemaker heeft door-signalering die de master de tijdelijk niet beschikbaar. U kunt dit testen (dit is nuttig voor onderhoudsvensters), maar u kunt ook het scenario afdwingen door de virtuele machine blokkeren.

## <a name="stonith"></a>STONITH
Het moet mogelijk een afsluiten van de virtuele machine via de Azure CLI in plaats van een STONITH-script waarmee een fysiek apparaat uitgeven. U kunt `/usr/lib/stonith/plugins/external/ssh` als een basis- en STONITH inschakelen in de configuratie van het cluster. Azure CLI globaal moet worden geïnstalleerd en de publicatie-instellingen en het profiel moeten worden geladen voor de gebruiker van het cluster.

Voorbeeld van code voor de resource is beschikbaar op [GitHub](https://github.com/bureado/aztonith). De cluster-configuratie wijzigen door het volgende toe te voegen `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> Het script uitvoeren niet omhoog/omlaag controles. De oorspronkelijke SSH-resource 15 ping cheques had, maar het moment van herstel voor een Azure VM mogelijk meer variabele.

## <a name="limitations"></a>Beperkingen
Er gelden de volgende beperkingen:

* Het script linbit DRBD resource die wordt beheerd DRBD als een resource in pacemaker heeft gebruikt `drbdadm down` bij het afsluiten van een knooppunt, zelfs als het knooppunt wordt alleen op stand-by. Dit is niet ideaal omdat de slave niet de resource DRBD synchroniseert wordt terwijl de master schrijfbewerkingen opgehaald. Als het model niet vriendelijk mislukt, kan de slave de systeemstatus van een oudere bestand overnemen. Er zijn twee mogelijke manieren om dit op te lossen:
  * Afdwingen van een `drbdadm up r0` op alle clusterknooppunten via een lokale (niet-clusterized) watchdog
  * Het bewerken van het linbit DRBD script, om ervoor te zorgen dat `down` niet wordt aangeroepen in`/usr/lib/ocf/resource.d/linbit/drbd`
* De load balancer moet ten minste vijf seconden reageert, zodat toepassingen clusterbewust is en minder gevoelig voor time-out. Andere architecturen, zoals in-app-wachtrijen en query middlewares, kunnen ook helpen.
* MySQL-afstemming is nodig om ervoor te zorgen dat beheerbare tempo geschreven en caches worden leeggemaakt naar de schijf zo vaak mogelijk geheugen gegevensverlies te minimaliseren.
* Prestaties zijn afhankelijk in VM schrijven onderling verbinding maken in de virtuele switch, omdat dit het mechanisme dat door DRBD is voor replicatie van het apparaat.
