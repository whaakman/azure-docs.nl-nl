---
title: Hoge beschikbaarheid voor NFS op Azure VM's op SUSE Linux Enterprise Server | Microsoft Docs
description: Hoge beschikbaarheid voor NFS op Azure VM's op SUSE Linux Enterprise Server
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: sedusch
ms.openlocfilehash: 53bc4a6f4ecca8ffe3575a038b86192a8663c35c
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="high-availability-for-nfs-on-azure-vms-on-suse-linux-enterprise-server"></a>Hoge beschikbaarheid voor NFS op Azure VM's op SUSE Linux Enterprise Server

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

In dit artikel wordt beschreven hoe de virtuele machines te implementeren, configureren van de virtuele machines, installeer het framework van het cluster en een maximaal beschikbare NFS-server die kan worden gebruikt voor het opslaan van de gedeelde gegevens van een maximaal beschikbare SAP-systeem installeren.
Deze handleiding wordt beschreven hoe u een maximaal beschikbare NFS-server die wordt gebruikt door twee SAP-systemen NW1 en NW2 instelt. De namen van de resources (bijvoorbeeld virtuele machines, virtuele netwerken) in het voorbeeld wordt ervan uitgegaan dat u hebt gebruikt de [SAP-server de sjabloon] [ template-file-server] met resource voorvoegsel **prod**.

Lees eerst de volgende opmerkingen bij de SAP en documenten

* SAP-notitie [1928533], die is:
  * Lijst met Azure VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belangrijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturingssysteem (OS) en combinaties van de database
  * Vereiste SAP-kernel-versie voor Windows en Linux op Microsoft Azure

* SAP-notitie [2015553] bevat vereisten voor software-implementaties SAP SAP ondersteund in Azure.
* SAP-notitie [2205917] heeft aanbevolen OS-instellingen voor SUSE Linux Enterprise Server voor SAP-toepassingen
* SAP-notitie [1944799] heeft SAP HANA richtlijnen voor SUSE Linux Enterprise Server voor SAP-toepassingen
* SAP-notitie [2178632] bevat gedetailleerde informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste Hostagent SAP-versie voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over SAP licentieverlening op Linux in Azure.
* SAP-notitie [1984787] heeft algemene informatie over SUSE Linux Enterprise Server 12.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure verbeterde extensie Monitoring voor SAP.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) heeft alle SAP-opmerkingen voor Linux vereist.
* [Azure virtuele Machines, planning en implementatie voor SAP op Linux][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP op Linux (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [Scenario voor optimale SAP HANA SR prestaties][suse-hana-ha-guide]  
  De handleiding bevat alle benodigde informatie om in te stellen van SAP HANA System Replication on-premises. Deze handleiding gebruiken als basislijn.
* [Maximaal beschikbare NFS opslag met DRBD en pacemaker heeft] [ suse-drbd-guide] de handleiding bevat alle vereiste informatie voor het instellen van een maximaal beschikbare NFS-server. Deze handleiding gebruiken als basislijn.


## <a name="overview"></a>Overzicht

SAP NetWeaver vereist om te zorgen voor hoge beschikbaarheid, een NFS-server. De NFS-server is geconfigureerd in een cluster met afzonderlijke en kan worden gebruikt door meerdere SAP-systemen.

![SAP NetWeaver hoge beschikbaarheid-overzicht](./media/high-availability-guide-nfs/ha-suse-nfs.png)

De NFS-server maakt gebruik van een speciale virtuele hostnaam en virtuele IP-adressen voor elke SAP-besturingssysteem dat gebruikmaakt van deze server voor NFS. In Azure, is een load balancer vereist voor het gebruik van een virtueel IP-adres. De volgende lijst ziet u de configuratie van de load balancer.        

* Frontend configuratie
  * IP-adres 10.0.0.4 voor NW1
  * IP-adres 10.0.0.5 voor NW2
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van de NFS-cluster uitmaken moet
* De Testpoort
  * Poort 61000 voor NW1
  * Poort 61001 voor NW2
* Loadbalancing regels
  * TCP voor NW1 2049
  * 2049 UDP voor NW1
  * TCP voor NW2 2049
  * 2049 UDP voor NW2

## <a name="set-up-a-highly-available-nfs-server"></a>Een maximaal beschikbare NFS-server instellen

U kunt een Azure-sjabloon vanuit github voor installatie zonder toezicht gebruiken voor het implementeren van alle vereiste Azure-resources, met inbegrip van de virtuele machines, beschikbaarheid instellen en de load balancer of u kunt de bronnen handmatig implementeren.

### <a name="deploy-linux-via-azure-template"></a>Linux via Azure-sjabloon implementeren

Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor SAP-toepassingen 12 die u gebruiken kunt om nieuwe virtuele machines te implementeren.
U kunt een van de Quick Start-sjablonen op github gebruiken voor het implementeren van alle vereiste resources. De sjabloon wordt geïmplementeerd voor de virtuele machines, de load balancer, beschikbaarheidsset enzovoort. Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [SAP-server de sjabloon] [ template-file-server] in de Azure portal   
1. Voer de volgende parameters
   1. Resource-voorvoegsel  
      Geef het voorvoegsel op dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   2. SAP System aantal Voer het nummer van SAP-systemen die deze bestandsserver wordt gebruikt. Hiermee implementeert u de vereiste hoeveelheid van frontend-configuraties, taakverdelingsregels, test u poorten, schijven, enzovoort.
   3. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld SLES 12
   4. Gebruikersnaam van de beheerder en het wachtwoord van beheerder  
      Een nieuwe gebruiker wordt gemaakt die kunnen worden gebruikt voor aanmelding bij de computer.
   5. Subnet-ID  
      De ID van het subnet waarmee de virtuele machines moet worden verbonden. Laat leeg als u wilt een nieuw virtueel netwerk maken of selecteren van het subnet van het VPN- of Express Route virtuele netwerk verbinding maken met de virtuele machine van uw on-premises netwerk. De ID meestal ziet eruit als /subscriptions/**&lt;abonnements-ID&gt;**/resourceGroups/**&lt;Resourcegroepnaam&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuele-netwerknaam&gt;**/subnets/**&lt;subnetnaam&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Het handmatig implementeren van Linux via Azure portal

U moet eerst de virtuele machines voor dit cluster NFS maken. Daarna maken van een load balancer en het gebruik van de virtuele machines in de back-end-adresgroepen.

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Een Beschikbaarheidsset maken  
   De maximale updatedomein instellen
1. Virtuele Machine 1 maken   
   Ten minste SLES4SAP 12 SP3, in dit voorbeeld de SLES4SAP 12 SP3 BYOS installatiekopie SLES voor SAP toepassingen 12 SP3 (BYOS) wordt gebruikt  
   Selecteer Beschikbaarheidsset eerder hebt gemaakt  
1. Virtuele Machine 2 maken   
   Ten minste SLES4SAP 12 SP3 in dit voorbeeld de installatiekopie van het SLES4SAP 12 SP3 BYOS  
   SLES voor SAP toepassingen 12 SP3 (BYOS) wordt gebruikt  
   Selecteer Beschikbaarheidsset eerder hebt gemaakt  
1. Een gegevensschijf voor elk systeem SAP toevoegen aan beide virtuele machines.
1. Maak een Load Balancer (intern)  
   1. De frontend-IP-adressen maken
      1. IP-adres 10.0.0.4 voor NW1
         1. Open de load balancer, selecteer frontend-IP-adresgroep en klik op toevoegen
         1. Voer de naam van de nieuwe frontend-IP-adresgroep (bijvoorbeeld **nw1 frontend**)
         1. De toewijzing instellen op statisch en voer het IP-adres (bijvoorbeeld **10.0.0.4**)
         1. Klik op OK         
      1. IP-adres 10.0.0.5 voor NW2
         * Herhaal de stappen hierboven voor NW2
   1. De back-end-adresgroepen maken
      1. Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van het NFS-cluster voor NW1 uitmaken moet
         1. Open de load balancer, back-endpools selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe back-endpool (bijvoorbeeld **nw1-back-end**)
         1. Klik op een virtuele machine toevoegen
         1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt
         1. Selecteer de virtuele machines van de NFS-cluster
         1. Klik op OK
      1. Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van het NFS-cluster voor NW2 uitmaken moet
         * Herhaal de stappen hierboven om een back-endpool voor NW2 maken
   1. De statuscontroles maken
      1. Poort 61000 voor NW1
         1. Openen van de load balancer, statuscontroles selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe health test (bijvoorbeeld **nw1 hp**)
         1. Selecteer TCP als protocol, poort 610**00**, houd Interval 5 en de drempelwaarde voor onjuiste status 2
         1. Klik op OK
      1. Poort 61001 voor NW2
         * Herhaal de stappen hierboven om te maken van een health test voor NW2
   1. Loadbalancing regels
      1. TCP voor NW1 2049
         1. Open de load balancer en taakverdelingsregels Selecteer klikt u op toevoegen
         1. Voer de naam van de load balancer-regel (bijvoorbeeld **nw1-lb-2049**)
         1. Selecteer de frontend-IP-adres, back-endpool en health test die u eerder hebt gemaakt (bijvoorbeeld **nw1 frontend**)
         1. Houd protocol **TCP**, voer poort **2049**
         1. Verhoog de time-out voor inactiviteit tot 30 minuten
         1. **Zorg ervoor dat u kunt zwevend IP inschakelen**
         1. Klik op OK    
      1. 2049 UDP voor NW1
         * Herhaal de stappen hierboven voor- en UDP-poort 2049 voor NW1
      1. TCP voor NW2 2049
         * Herhaal de stappen hierboven voor- en TCP-poort 2049 voor NW2
      1. 2049 UDP voor NW2
         * Herhaal de stappen hierboven voor- en UDP-poort 2049 voor NW2

### <a name="create-pacemaker-cluster"></a>Pacemaker heeft cluster maken

Volg de stappen in [pacemaker heeft op SUSE Linux Enterprise Server in Azure instellen](high-availability-guide-suse-pacemaker.md) een basic pacemaker heeft cluster voor deze server voor NFS te maken.

### <a name="configure-nfs-server"></a>NFS-server configureren

De volgende items worden voorafgegaan door een **[A]** - van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  Hostnamen instellen   

   U kunt gebruik van een DNS-server of wijzig de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe het bestand/etc/hosts te gebruiken.
   Het IP-adres en de hostnaam in de volgende opdrachten vervangen

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Voeg de volgende regels/etc/hosts. Wijzig de IP-adres en hostnaam overeenkomen met uw omgeving   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   <b>10.0.0.5 nw2-nfs</b>
   </code></pre>

1. **[A]**  Inschakelen NFS-server

   Maken van de root export-vermelding, start u de NFS-server en autostart inschakelen

   <pre><code>
   sudo sh -c 'echo /srv/nfs/ *\(rw,no_root_squash,fsid=0\)>/etc/exports'
   
   sudo mkdir /srv/nfs/

   sudo systemctl enable nfsserver
   sudo service nfsserver restart
   </code></pre>

1. **[A]**  Drbd-onderdelen installeren

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Maken van een partitie voor de drbd-apparaten

   Alle beschikbare gegevensschijven weergeven

   <pre><code>
   sudo ls /dev/disk/azure/scsi1/
   </code></pre>

   Voorbeeld van uitvoer
   
   ```
   lun0  lun1
   ```

   Partities voor elke gegevensschijf maken

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun1'
   </code></pre>

1. **[A]**  Configuraties LVM maken

   Lijst van alle beschikbare partities

   <pre><code>
   ls /dev/disk/azure/scsi1/lun*-part*
   </code></pre>

   Voorbeeld van uitvoer
   
   ```
   /dev/disk/azure/scsi1/lun0-part1  /dev/disk/azure/scsi1/lun1-part1
   ```

   LVM volumes voor elke partitie maken

   <pre><code>
   sudo pvcreate /dev/disk/azure/scsi1/lun0-part1  
   sudo vgcreate vg-<b>NW1</b>-NFS /dev/disk/azure/scsi1/lun0-part1
   sudo lvcreate -l 100%FREE -n <b>NW1</b> vg-<b>NW1</b>-NFS

   sudo pvcreate /dev/disk/azure/scsi1/lun1-part1
   sudo vgcreate vg-<b>NW2</b>-NFS /dev/disk/azure/scsi1/lun1-part1
   sudo lvcreate -l 100%FREE -n <b>NW2</b> vg-<b>NW2</b>-NFS
   </code></pre>

1. **[A]**  Drbd configureren

   <pre><code>
   sudo vi /etc/drbd.conf
   </code></pre>

   Zorg ervoor dat het bestand drbd.conf de volgende twee regels bevat

   <pre><code>
   include "drbd.d/global_common.conf";
   include "drbd.d/*.res";
   </code></pre>

   De globale drbd-configuratie wijzigen

   <pre><code>
   sudo vi /etc/drbd.d/global_common.conf
   </code></pre>

   De volgende vermeldingen toevoegen aan de handler en net sectie.

   <pre><code>
   global {
        usage-count no;
   }
   common {
        handlers {
             fence-peer "/usr/lib/drbd/crm-fence-peer.sh";
             after-resync-target "/usr/lib/drbd/crm-unfence-peer.sh";
             split-brain "/usr/lib/drbd/notify-split-brain.sh root";
             pri-lost-after-sb "/usr/lib/drbd/notify-pri-lost-after-sb.sh; /usr/lib/drbd/notify-emergency-reboot.sh; echo b > /proc/sysrq-trigger ; reboot -f";
        }
        startup {
             wfc-timeout 0;
        }
        options {
        }
        disk {
             resync-rate 50M;
        }
        net {
             after-sb-0pri discard-younger-primary;
             after-sb-1pri discard-secondary;
             after-sb-2pri call-pri-lost-after-sb;
        }
   }
   </code></pre>

1. **[A]**  Maken de NFS-drbd-apparaten

   <pre><code>
   sudo vi /etc/drbd.d/<b>NW1</b>-nfs.res
   </code></pre>

   Invoegen van de configuratie voor de nieuwe drbd apparaat en afsluiten

   <pre><code>
   resource <b>NW1</b>-nfs {
        protocol     C;
        disk {
             on-io-error       detach;
        }
        on <b>prod-nfs-0</b> {
             address   <b>10.0.0.6:7790</b>;
             device    /dev/drbd<b>0</b>;
             disk      /dev/<b>vg-NW1-NFS</b>/<b>NW1</b>;
             meta-disk internal;
        }
        on <b>prod-nfs-1</b> {
             address   <b>10.0.0.7:7790</b>;
             device    /dev/drbd<b>0</b>;
             disk      /dev/<b>vg-NW1-NFS</b>/<b>NW1</b>;
             meta-disk internal;
        }
   }
   </code></pre>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NW2</b>-nfs.res
   </code></pre>

   Invoegen van de configuratie voor de nieuwe drbd apparaat en afsluiten

   <pre><code>
   resource <b>NW2</b>-nfs {
        protocol     C;
        disk {
             on-io-error       detach;
        }
        on <b>prod-nfs-0</b> {
             address   <b>10.0.0.6:7791</b>;
             device    /dev/drbd<b>1</b>;
             disk      /dev/<b>vg-NW2-NFS</b>/<b>NW2</b>;
             meta-disk internal;
        }
        on <b>prod-nfs-1</b> {
             address   <b>10.0.0.7:7791</b>;
             device    /dev/drbd<b>1</b>;
             disk      /dev/<b>vg-NW2-NFS</b>/<b>NW2</b>;
             meta-disk internal;
        }
   }
   </code></pre>

   Maken van het apparaat drbd en start deze

   <pre><code>
   sudo drbdadm create-md <b>NW1</b>-nfs
   sudo drbdadm create-md <b>NW2</b>-nfs
   sudo drbdadm up <b>NW1</b>-nfs
   sudo drbdadm up <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Overslaan van de initiële synchronisatie

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NW1</b>-nfs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Het primaire knooppunt instellen

   <pre><code>
   sudo drbdadm primary --force <b>NW1</b>-nfs
   sudo drbdadm primary --force <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Wacht totdat de nieuwe drbd apparaten worden gesynchroniseerd

   <pre><code>
   sudo drbdsetup wait-sync-resource NW1-nfs
   sudo drbdsetup wait-sync-resource NW2-nfs
   </code></pre>

1. **[1]**  Bestandssystemen op de apparaten drbd maken

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkdir /srv/nfs/NW1
   sudo chattr +i /srv/nfs/NW1
   sudo mount -t xfs /dev/drbd0 /srv/nfs/NW1
   sudo mkdir /srv/nfs/NW1/sidsys
   sudo mkdir /srv/nfs/NW1/sapmntsid
   sudo mkdir /srv/nfs/NW1/trans
   sudo mkdir /srv/nfs/NW1/ASCS
   sudo mkdir /srv/nfs/NW1/ASCSERS
   sudo mkdir /srv/nfs/NW1/SCS
   sudo mkdir /srv/nfs/NW1/SCSERS
   sudo umount /srv/nfs/NW1

   sudo mkfs.xfs /dev/drbd1
   sudo mkdir /srv/nfs/NW2
   sudo chattr +i /srv/nfs/NW2
   sudo mount -t xfs /dev/drbd1 /srv/nfs/NW2
   sudo mkdir /srv/nfs/NW2/sidsys
   sudo mkdir /srv/nfs/NW2/sapmntsid
   sudo mkdir /srv/nfs/NW2/trans
   sudo mkdir /srv/nfs/NW2/ASCS
   sudo mkdir /srv/nfs/NW2/ASCSERS
   sudo mkdir /srv/nfs/NW2/SCS
   sudo mkdir /srv/nfs/NW2/SCSERS
   sudo umount /srv/nfs/NW2
   </code></pre>

1. **[A]**  Drbd split-brain detectie instellen

   Wanneer u drbd gebruikt om gegevens van de ene host naar een andere te synchroniseren, kan een zogenaamde split-brain optreden. Een split-brain is een scenario waarbij beide clusterknooppunten gepromoveerd van het apparaat in drbd als de primaire en is niet gesynchroniseerd. Zeldzame situatie is mogelijk, maar u toch wilt verwerken en een split-brain zo snel mogelijk oplossen. Daarom is het belangrijk dat u een melding krijgen wanneer u een split-brain hebben plaatsgevonden.

   Lees [de documentatie van de officiële drbd](http://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-split-brain-notification) op het instellen van een melding split-brain.

   Het is ook mogelijk om automatisch te herstellen van een scenario split-brain. Lees voor meer informatie [herstelbeleid automatische split-brain](http://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-automatic-split-brain-recovery-configuration)
   
### <a name="configure-cluster-framework"></a>Cluster-Framework configureren

1. **[1]**  De NFS-drbd-apparaten voor SAP-systeem NW1 toevoegen aan de clusterconfiguratie

   <pre><code>
   # Enable maintenance mode
   sudo crm configure property maintenance-mode=true
   
   sudo crm configure primitive drbd_<b>NW1</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NW1</b>-nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"
   
   sudo crm configure ms ms-drbd_<b>NW1</b>_nfs drbd_<b>NW1</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"
   
   sudo crm configure primitive fs_<b>NW1</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NW1</b>  \
     fstype=xfs \
     op monitor interval="10s"
   
   sudo crm configure primitive exportfs_<b>NW1</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>" \
     options="rw,no_root_squash" clientspec="*" fsid=1 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_sidsys \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/sidsys" \
     options="rw,no_root_squash" clientspec="*" fsid=2 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_sapmntsid \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/sapmntsid" \
     options="rw,no_root_squash" clientspec="*" fsid=3 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_trans \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/trans" \
     options="rw,no_root_squash" clientspec="*" fsid=4 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_ASCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/ASCS" \
     options="rw,no_root_squash" clientspec="*" fsid=5 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_ASCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/ASCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=6 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_SCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/SCS" \
     options="rw,no_root_squash" clientspec="*" fsid=7 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW1</b>_SCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>/SCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=8 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW1</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61000</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_nfs \
     fs_<b>NW1</b>_sapmnt exportfs_<b>NW1</b> exportfs_<b>NW1</b>_sidsys \
     exportfs_<b>NW1</b>_sapmntsid exportfs_<b>NW1</b>_trans exportfs_<b>NW1</b>_ASCS \
     exportfs_<b>NW1</b>_ASCSERS exportfs_<b>NW1</b>_SCS exportfs_<b>NW1</b>_SCSERS \
     nc_<b>NW1</b>_nfs vip_<b>NW1</b>_nfs
   
   sudo crm configure order o-<b>NW1</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW1</b>_nfs:promote g-<b>NW1</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW1</b>_nfs_on_drbd inf: \
     g-<b>NW1</b>_nfs ms-drbd_<b>NW1</b>_nfs:Master
   </code></pre>

1. **[1]**  De NFS-drbd-apparaten voor SAP-systeem NW2 toevoegen aan de clusterconfiguratie

   <pre><code>
   # Enable maintenance mode
   sudo crm configure property maintenance-mode=true   
   
   sudo crm configure primitive drbd_<b>NW2</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NW2</b>-nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"
   
   sudo crm configure ms ms-drbd_<b>NW2</b>_nfs drbd_<b>NW2</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"
   
   sudo crm configure primitive fs_<b>NW2</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/srv/nfs/<b>NW2</b>  \
     fstype=xfs \
     op monitor interval="10s"
   
   sudo crm configure primitive exportfs_<b>NW2</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>" \
     options="rw,no_root_squash" clientspec="*" fsid=9 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_sidsys \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/sidsys" \
     options="rw,no_root_squash" clientspec="*" fsid=10 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_sapmntsid \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/sapmntsid" \
     options="rw,no_root_squash" clientspec="*" fsid=11 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_trans \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/trans" \
     options="rw,no_root_squash" clientspec="*" fsid=12 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_ASCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/ASCS" \
     options="rw,no_root_squash" clientspec="*" fsid=13 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_ASCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/ASCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=14 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_SCS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/SCS" \
     options="rw,no_root_squash" clientspec="*" fsid=15 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive exportfs_<b>NW2</b>_SCSERS \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>/SCSERS" \
     options="rw,no_root_squash" clientspec="*" fsid=16 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW2</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.5</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW2</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61001</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW2</b>_nfs \
     fs_<b>NW2</b>_sapmnt exportfs_<b>NW2</b> exportfs_<b>NW2</b>_sidsys \
     exportfs_<b>NW2</b>_sapmntsid exportfs_<b>NW2</b>_trans exportfs_<b>NW2</b>_ASCS \
     exportfs_<b>NW2</b>_ASCSERS exportfs_<b>NW2</b>_SCS exportfs_<b>NW2</b>_SCSERS \
     nc_<b>NW2</b>_nfs vip_<b>NW2</b>_nfs
   
   sudo crm configure order o-<b>NW2</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW2</b>_nfs:promote g-<b>NW2</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW2</b>_nfs_on_drbd inf: \
     g-<b>NW2</b>_nfs ms-drbd_<b>NW2</b>_nfs:Master
   </code></pre>

1. **[1]**  Onderhoudsmodus uitschakelen
   
   <pre><code>
   sudo crm configure property maintenance-mode=false
   </code></pre>

## <a name="next-steps"></a>Volgende stappen
* [Installeer de SAP ASC's en -database](high-availability-guide-suse.md)
* [Azure virtuele Machines, planning en implementatie voor SAP][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
