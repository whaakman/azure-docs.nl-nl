---
title: Hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server | Microsoft Docs
description: Hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: sedusch
ms.openlocfilehash: 99b7b83ca2d7f6f19df137e6ecf5deaf411e9a5e
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634741"
---
# <a name="high-availability-for-nfs-on-azure-vms-on-suse-linux-enterprise-server"></a>Hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server

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

[sles-hae-guides]:https://www.suse.com/documentation/sle-ha-12/
[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

In dit artikel wordt beschreven hoe u de virtuele machines implementeren, configureren van de virtuele machines, installeer het framework van het cluster en installeren van een maximaal beschikbare NFS-server die kan worden gebruikt voor het opslaan van de gedeelde gegevens van een maximaal beschikbare SAP-systeem.
Deze handleiding wordt beschreven hoe u een maximaal beschikbare NFS-server die wordt gebruikt door twee SAP-systemen, NW1 en NW2 instelt. De namen van de resources (bijvoorbeeld virtuele machines, virtuele netwerken) in het voorbeeld wordt ervan uitgegaan dat u hebt gebruikt de [SAP-server de sjabloon] [ template-file-server] met het voorvoegsel van de resource **prod**.

Lees eerst de volgende SAP-opmerkingen en documenten

* SAP-notitie [1928533], heeft:
  * Lijst met Azure-VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belangrijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturingssysteem (OS) en combinaties van database
  * Vereiste kernel van SAP-versie voor Windows en Linux op Microsoft Azure

* SAP-notitie [2015553] bevat vereisten voor SAP-ondersteunde SAP software-implementaties in Azure.
* SAP-notitie [2205917] heeft aanbevolen instellingen voor het besturingssysteem voor SUSE Linux Enterprise Server voor SAP-toepassingen
* SAP-notitie [1944799] SAP HANA-richtlijnen voor SUSE Linux Enterprise Server heeft voor SAP-toepassingen
* SAP-notitie [2178632] vindt u meer informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste versie van de SAP-Host-Agent voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over de licentieverlening voor SAP op Linux in Azure.
* SAP-notitie [1984787] bevat algemene informatie over het SUSE Linux Enterprise Server 12.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure uitgebreide controle-extensie voor SAP.
* [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) bevat alle SAP-opmerkingen voor Linux vereist.
* [Azure virtuele Machines, planning en implementatie van SAP op Linux][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP op Linux (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [SUSE Linux Enterprise hoge beschikbaarheid-extensie 12 SP3 best practice-richtlijnen][sles-hae-guides]
  * Maximaal beschikbare NFS-opslag met DRBD en Pacemaker
* [SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP3 best practice-richtlijnen][sles-for-sap-bp]

## <a name="overview"></a>Overzicht

SAP NetWeaver vereist voor het bereiken van hoge beschikbaarheid, een NFS-server. De NFS-server is geconfigureerd in een cluster dat gescheiden en kan worden gebruikt door meerdere SAP-systemen.

![Overzicht van SAP NetWeaver hoge beschikbaarheid](./media/high-availability-guide-nfs/ha-suse-nfs.png)

De NFS-server maakt gebruik van een speciale virtuele hostnaam en virtuele IP-adressen voor elk SAP-systeem die gebruikmaakt van deze server voor NFS. In Azure, wordt een load balancer overstappen naar een virtueel IP-adres. De volgende lijst ziet u de configuratie van de load balancer.        

* Front-end-configuratie
  * IP-adres 10.0.0.4 voor NW1
  * IP-adres 10.0.0.5 voor NW2
* Back-endconfiguratie
  * Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van het NFS-cluster uitmaken
* Testpoort
  * Poort 61000 voor NW1
  * Poort 61001 voor NW2
* Regels van loadbalancing in Plbscheduler
  * 2049 TCP voor NW1
  * 2049 UDP voor NW1
  * 2049 TCP voor NW2
  * 2049 UDP voor NW2

## <a name="set-up-a-highly-available-nfs-server"></a>Een maximaal beschikbare NFS-server instellen

U kunt een Azure-sjabloon gebruiken voor het implementeren van alle vereiste Azure-resources, met inbegrip van de virtuele machines, beschikbaarheid, en de load balancer of kunt u de resources handmatig implementeren.

### <a name="deploy-linux-via-azure-template"></a>Linux via Azure-sjabloon implementeren

De Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor 12 van de SAP-toepassingen die u gebruiken kunt om nieuwe virtuele machines te implementeren.
U kunt een van de snelstartsjablonen van op GitHub gebruiken om alle vereiste resources te implementeren. De sjabloon implementeert de virtuele machines, load balancer, beschikbaarheidsset enzovoort. Volg deze stappen om de sjabloon te implementeren:

1. Open de [SAP-server de sjabloon] [ template-file-server] in Azure portal   
1. Voer de volgende parameters
   1. Voorvoegsel van de resource  
      Geef het voorvoegsel dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   2. Telling van SAP-systeem  
      Voer het nummer van de SAP-systemen die deze bestandsserver wordt gebruikt. Hierdoor wordt de vereiste hoeveelheid geïmplementeerd van frontend-configuraties, load balancer-regels, test u poorten, schijven, enzovoort.
   3. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld, SLES 12
   4. Gebruikersnaam voor de beheerder en het wachtwoord van beheerder  
      Een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de machine.
   5. Subnet-ID  
      Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID is meestal ziet eruit als /subscriptions/**&lt;abonnements-ID&gt;**/resourceGroups/**&lt;groepsnaam voor accountresources&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuele-netwerknaam&gt;**/subnets/**&lt;subnetnaam&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Handmatig implementeren van Linux via Azure portal

U moet eerst de virtuele machines voor dit cluster NFS maken. Daarna wordt u een load balancer maken en gebruiken van de virtuele machines in de back endadresgroepen.

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Een Beschikbaarheidsset maken  
   De maximale updatedomein instellen
1. Ten minste 1 gebruik van virtuele Machine maken SLES4SAP 12 SP3, in dit voorbeeld de SLES4SAP 12 SP3 BYOS image SLES voor SAP Applications 12 SP3 (BYOS) wordt gebruikt  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt  
1. Maken van virtuele Machine 2 gebruik ten minste SLES4SAP 12 SP3, in dit voorbeeld de SLES4SAP 12 SP3 BYOS-afbeelding  
   SLES voor SAP Applications 12 SP3 (BYOS) wordt gebruikt  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt  
1. Een gegevensschijf voor elk SAP-systeem toevoegen aan beide virtuele machines.
1. Maak een Load Balancer (intern)  
   1. De frontend-IP-adressen maken
      1. IP-adres 10.0.0.4 voor NW1
         1. De load balancer openen, front-end-IP-adresgroep selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe frontend-IP-adresgroep (bijvoorbeeld **nw1-frontend**)
         1. De toewijzing instellen op statisch en geef het IP-adres (bijvoorbeeld **10.0.0.4**)
         1. Klik op OK
      1. IP-adres 10.0.0.5 voor NW2
         * Herhaal de stappen hierboven voor NW2
   1. De back endadresgroepen maken
      1. Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de NFS-cluster voor NW1 uitmaken
         1. Open de load balancer, back-endpools selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe back endpool (bijvoorbeeld **nw1-back-end**)
         1. Klik op een virtuele machine toevoegen
         1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt
         1. Selecteer de virtuele machines van de NFS-cluster
         1. Klik op OK
      1. Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de NFS-cluster voor NW2 uitmaken
         * Herhaal de stappen hierboven om een back endadresgroep maken voor NW2
   1. De statuscontroles maken
      1. Poort 61000 voor NW1
         1. De load balancer openen, selecteer statuscontroles en klikt u op toevoegen
         1. Voer de naam van de nieuwe statustest (bijvoorbeeld **nw1 hp**)
         1. Selecteer TCP als protocol, poort 610**00**, Interval van 5 en de drempelwaarde voor onjuiste status 2
         1. Klik op OK
      1. Poort 61001 voor NW2
         * Herhaal de stappen hierboven om te maken van een statustest voor NW2
   1. Regels van loadbalancing in Plbscheduler
      1. 2049 TCP voor NW1
         1. De load balancer openen, selecteert u load balancer-regels en klikt u op toevoegen
         1. Voer de naam van de nieuwe load balancer-regel (bijvoorbeeld **nw1-lb-2049**)
         1. Selecteer de front-end-IP-adres, de back-endpool en de statustest die u eerder hebt gemaakt (bijvoorbeeld **nw1-frontend**)
         1. Houd protocol **TCP**, voer poort **2049**
         1. Time-out voor inactiviteit tot 30 minuten verhogen
         1. **Zorg ervoor dat u zwevend IP inschakelen**
         1. Klik op OK
      1. 2049 UDP voor NW1
         * Herhaal de bovenstaande stappen voor- en UDP-poort 2049 voor NW1
      1. 2049 TCP voor NW2
         * Herhaal de stappen hierboven voor- en TCP-poort 2049 voor NW2
      1. 2049 UDP voor NW2
         * Herhaal de bovenstaande stappen voor- en UDP-poort 2049 voor NW2

### <a name="create-pacemaker-cluster"></a>Pacemaker-cluster maken

Volg de stappen in [Pacemaker op SUSE Linux Enterprise Server in Azure instellen](high-availability-guide-suse-pacemaker.md) om een eenvoudige Pacemaker-cluster voor deze NFS-server te maken.

### <a name="configure-nfs-server"></a>NFS-server configureren

De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   <pre><code>sudo vi /etc/hosts
   </code></pre>
   
   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen
   
   <pre><code># IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   <b>10.0.0.5 nw2-nfs</b>
   </code></pre>

1. **[A]**  Inschakelen NFS-server

   De NFS-export basisvermelding te maken

   <pre><code>sudo sh -c 'echo /srv/nfs/ *\(rw,no_root_squash,fsid=0\)>/etc/exports'
   
   sudo mkdir /srv/nfs/
   </code></pre>

1. **[A]**  Drbd onderdelen installeren

   <pre><code>sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Maken van een partitie voor de drbd-apparaten

   Alle schijven van de beschikbare gegevens weergeven

   <pre><code>sudo ls /dev/disk/azure/scsi1/
   </code></pre>

   Voorbeeld van uitvoer
   
   ```
   lun0  lun1
   ```

   Partities voor elke gegevensschijf maken

   <pre><code>sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun1'
   </code></pre>

1. **[A]**  Maken LVM-configuraties

   Lijst met alle beschikbare partities

   <pre><code>ls /dev/disk/azure/scsi1/lun*-part*
   </code></pre>

   Voorbeeld van uitvoer
   
   ```
   /dev/disk/azure/scsi1/lun0-part1  /dev/disk/azure/scsi1/lun1-part1
   ```

   LVM-volumes voor elke partitie maken

   <pre><code>sudo pvcreate /dev/disk/azure/scsi1/lun0-part1  
   sudo vgcreate vg-<b>NW1</b>-NFS /dev/disk/azure/scsi1/lun0-part1
   sudo lvcreate -l 100%FREE -n <b>NW1</b> vg-<b>NW1</b>-NFS

   sudo pvcreate /dev/disk/azure/scsi1/lun1-part1
   sudo vgcreate vg-<b>NW2</b>-NFS /dev/disk/azure/scsi1/lun1-part1
   sudo lvcreate -l 100%FREE -n <b>NW2</b> vg-<b>NW2</b>-NFS
   </code></pre>

1. **[A]**  Drbd configureren

   <pre><code>sudo vi /etc/drbd.conf
   </code></pre>

   Zorg ervoor dat het bestand drbd.conf de volgende twee regels bevat

   <pre><code>include "drbd.d/global_common.conf";
   include "drbd.d/*.res";
   </code></pre>

   De globale drbd-configuratie wijzigen

   <pre><code>sudo vi /etc/drbd.d/global_common.conf
   </code></pre>

   De volgende vermeldingen toevoegen aan de handler en net sectie.

   <pre><code>global {
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
             md-flushes yes;
             disk-flushes yes;
             c-plan-ahead 1;
             c-min-rate 100M;
             c-fill-target 20M;
             c-max-rate 4G;
        }
        net {
             after-sb-0pri discard-younger-primary;
             after-sb-1pri discard-secondary;
             after-sb-2pri call-pri-lost-after-sb;
             protocol     C;
             tcp-cork yes;
             max-buffers 20000;
             max-epoch-size 20000;
             sndbuf-size 0;
             rcvbuf-size 0;
        }
   }
   </code></pre>

1. **[A]**  Maken de NFS-drbd-apparaten

   <pre><code>sudo vi /etc/drbd.d/<b>NW1</b>-nfs.res
   </code></pre>

   De configuratie voor de nieuwe drbd apparaat- en uitgiftemodules invoegen

   <pre><code>resource <b>NW1</b>-nfs {
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

   <pre><code>sudo vi /etc/drbd.d/<b>NW2</b>-nfs.res
   </code></pre>

   De configuratie voor de nieuwe drbd apparaat- en uitgiftemodules invoegen

   <pre><code>resource <b>NW2</b>-nfs {
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

   Maken van het apparaat drbd en start de App

   <pre><code>sudo drbdadm create-md <b>NW1</b>-nfs
   sudo drbdadm create-md <b>NW2</b>-nfs
   sudo drbdadm up <b>NW1</b>-nfs
   sudo drbdadm up <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Overslaan van de initiële synchronisatie

   <pre><code>sudo drbdadm new-current-uuid --clear-bitmap <b>NW1</b>-nfs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Het primaire knooppunt instellen

   <pre><code>sudo drbdadm primary --force <b>NW1</b>-nfs
   sudo drbdadm primary --force <b>NW2</b>-nfs
   </code></pre>

1. **[1]**  Wacht totdat de nieuwe drbd apparaten worden gesynchroniseerd

   <pre><code>sudo drbdsetup wait-sync-resource NW1-nfs
   sudo drbdsetup wait-sync-resource NW2-nfs
   </code></pre>

1. **[1]**  Bestandssystemen op de apparaten drbd maken

   <pre><code>sudo mkfs.xfs /dev/drbd0
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

   Wanneer u drbd gebruikt om gegevens van de ene host naar een andere te synchroniseren, kan een zogenaamde split-brain optreden. Een split-brain is een scenario waarin beide clusterknooppunten gepromoveerd van het apparaat in drbd als de primaire en is niet gesynchroniseerd. Is het mogelijk een zeldzame situatie, maar u nog steeds wilt verwerken en een splitsing brein moeite heeft zo snel mogelijk opgelost. Daarom is het belangrijk om te worden geïnformeerd wanneer een split-brain is er gebeurd.

   Lezen [de documentatie van de officiële drbd](http://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-split-brain-notification) over het instellen van een splitsing brein melding.

   Het is ook mogelijk om te een splitsing brein scenario automatisch worden hersteld. Lees voor meer informatie, [automatische splitsen brein herstelbeleid](http://docs.linbit.com/doc/users-guide-83/s-configure-split-brain-behavior/#s-automatic-split-brain-recovery-configuration)
   
### <a name="configure-cluster-framework"></a>Cluster-Framework configureren

1. **[1]**  NFS drbd apparaten voor SAP-systeem NW1 toevoegen aan de configuratie van het cluster

   <pre><code>sudo crm configure rsc_defaults resource-stickiness="200"

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
   
   sudo crm configure primitive nfsserver systemd:nfs-server \
     op monitor interval="30s"
   sudo crm configure clone cl-nfsserver nfsserver

   sudo crm configure primitive exportfs_<b>NW1</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>" \
     options="rw,no_root_squash,crossmnt" clientspec="*" fsid=1 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW1</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61000</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_nfs \
     fs_<b>NW1</b>_sapmnt exportfs_<b>NW1</b> nc_<b>NW1</b>_nfs vip_<b>NW1</b>_nfs
   
   sudo crm configure order o-<b>NW1</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW1</b>_nfs:promote g-<b>NW1</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW1</b>_nfs_on_drbd inf: \
     g-<b>NW1</b>_nfs ms-drbd_<b>NW1</b>_nfs:Master
   </code></pre>

1. **[1]**  NFS drbd apparaten voor SAP-systeem NW2 toevoegen aan de configuratie van het cluster

   <pre><code># Enable maintenance mode
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
     options="rw,no_root_squash" clientspec="*" fsid=2 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW2</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.5</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW2</b>_nfs \
     anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k <b>61001</b>" op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW2</b>_nfs \
     fs_<b>NW2</b>_sapmnt exportfs_<b>NW2</b> nc_<b>NW2</b>_nfs vip_<b>NW2</b>_nfs
   
   sudo crm configure order o-<b>NW2</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW2</b>_nfs:promote g-<b>NW2</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW2</b>_nfs_on_drbd inf: \
     g-<b>NW2</b>_nfs ms-drbd_<b>NW2</b>_nfs:Master
   </code></pre>

1. **[1]**  Onderhoudsmodus uitschakelen
   
   <pre><code>sudo crm configure property maintenance-mode=false
   </code></pre>

## <a name="next-steps"></a>Volgende stappen

* [De SAP ASCS en -database installeren](high-availability-guide-suse.md)
* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
