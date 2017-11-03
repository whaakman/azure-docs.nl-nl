---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op SUSE Linux Enterprise Server voor SAP-toepassingen | Microsoft Docs
description: Hoge beschikbaarheid handleiding voor SAP NetWeaver op SUSE Linux Enterprise Server voor SAP-toepassingen
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: ed728011f2cb7b6108e19a916010fd5447c07093
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's op SUSE Linux Enterprise Server voor SAP-toepassingen

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

In dit artikel wordt beschreven hoe de virtuele machines te implementeren, configureren van de virtuele machines, installeer het framework van het cluster en een maximaal beschikbare SAP NetWeaver 7,50 systeem installeren.
In de voorbeeldconfiguraties installatieopdrachten enzovoort. ASC's exemplaarnummer 00, Ebruikers exemplaarnummer 02 en SAP-systeem kan NWS-ID wordt gebruikt. De namen van de resources (bijvoorbeeld virtuele machines, virtuele netwerken) in het voorbeeld wordt ervan uitgegaan dat u hebt gebruikt de [geconvergeerde sjabloon] [ template-converged] met SAP-systeem kan NWS-ID om de resources te maken.

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

![SAP NetWeaver hoge beschikbaarheid-overzicht](./media/high-availability-guide-suse/img_001.png)

De NFS-server, SAP NetWeaver ASC's, SAP NetWeaver SCS, SAP NetWeaver Ebruikers en de SAP HANA-database gebruiken virtuele hostnaam en virtuele IP-adressen. In Azure, is een load balancer vereist voor het gebruik van een virtueel IP-adres. De volgende lijst ziet u de configuratie van de load balancer.

### <a name="nfs-server"></a>NFS-Server
* Frontend configuratie
  * IP-adres 10.0.0.4
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van de NFS-cluster uitmaken moet
* De Testpoort
  * Poort 61000
* Loadbalancing regels
  * 2049 TCP 
  * 2049 UDP

### <a name="ascs"></a>(A) SCS
* Frontend configuratie
  * IP-adres 10.0.0.10
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken moet) SCS Ebruikers/cluster
* De Testpoort
  * Poort 620**&lt;nr&gt;**
* Loadbalancing regels
  * 32**&lt;nr&gt;**  TCP
  * 36**&lt;nr&gt;**  TCP
  * 39**&lt;nr&gt;**  TCP
  * 81**&lt;nr&gt;**  TCP
  * 5**&lt;nr&gt;**13 TCP
  * 5**&lt;nr&gt;**14 TCP
  * 5**&lt;nr&gt;**16 TCP

### <a name="ers"></a>EBRUIKERS
* Frontend configuratie
  * IP-adres 10.0.0.11
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken moet) SCS Ebruikers/cluster
* De Testpoort
  * Poort 621**&lt;nr&gt;**
* Loadbalancing regels
  * 33**&lt;nr&gt;**  TCP
  * 5**&lt;nr&gt;**13 TCP
  * 5**&lt;nr&gt;**14 TCP
  * 5**&lt;nr&gt;**16 TCP

### <a name="sap-hana"></a>SAP HANA
* Frontend configuratie
  * IP-adres 10.0.0.12
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van het cluster HANA uitmaken moet
* De Testpoort
  * Poort 625**&lt;nr&gt;**
* Loadbalancing regels
  * 3**&lt;nr&gt;**15 TCP
  * 3**&lt;nr&gt;**17 TCP

## <a name="setting-up-a-highly-available-nfs-server"></a>Een maximaal beschikbare NFS-server instellen

### <a name="deploying-linux"></a>Linux implementeren

Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor SAP-toepassingen 12 die u gebruiken kunt om nieuwe virtuele machines te implementeren.
U kunt een van de snel starten-sjablonen op github gebruiken voor het implementeren van alle vereiste bronnen. De sjabloon wordt geïmplementeerd voor de virtuele machines, de load balancer, beschikbaarheidsset enzovoort. Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [SAP-server de sjabloon] [ template-file-server] in de Azure portal   
1. Voer de volgende parameters
   1. Resource-voorvoegsel  
      Geef het voorvoegsel op dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   2. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld SLES 12
   3. Gebruikersnaam van de beheerder en het wachtwoord van beheerder  
      Een nieuwe gebruiker wordt gemaakt die kunnen worden gebruikt voor aanmelding bij de computer.
   4. Subnet-Id  
      De ID van het subnet waarmee de virtuele machines moet worden verbonden. Laat leeg als u wilt een nieuw virtueel netwerk maken of selecteren van het subnet van het VPN- of Express Route virtuele netwerk verbinding maken met de virtuele machine van uw on-premises netwerk. De ID meestal ziet eruit als /subscriptions/**&lt;abonnements-id&gt;**/resourceGroups/**&lt;Resourcegroepnaam&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtuele-netwerknaam&gt;**/subnets/**&lt;subnetnaam&gt;**

### <a name="installation"></a>Installeren

De volgende items worden voorafgegaan door een **[A]** - van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  SLES bijwerken

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Ssh toegang inschakelen

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. **[2]**  Ssh toegang inschakelen

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. **[1]**  Ssh toegang inschakelen

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Installeren HA-uitbreiding
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]**  Hostnamen instellen   

   U kunt gebruik van een DNS-server of wijzig de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe het bestand/etc/hosts te gebruiken.
   Het IP-adres en de hostnaam in de volgende opdrachten vervangen

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Voeg de volgende regels/etc/hosts. Wijzig de IP-adres en hostnaam overeenkomen met uw omgeving   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. **[1]**  Cluster installeren
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Knooppunt aan cluster toevoegen
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Hacluster wachtwoord wijzigen naar hetzelfde wachtwoord

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Corosync voor het gebruik van andere transport en voeg nodelist configureren. Cluster werkt niet anders.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   De volgende vet inhoud toevoegen aan het bestand.
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-nfs-0</b>
      ring0_addr:10.0.0.5
     }
     node {
      # IP address of <b>prod-nfs-1</b>
      ring0_addr:10.0.0.6
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   Start de service corosync opnieuw

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[A]**  Drbd-onderdelen installeren

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Maken van een partitie voor het apparaat drbd

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. **[A]**  Configuraties LVM maken

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. **[A]**  Het NFS drbd apparaat maken

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   Invoegen van de configuratie voor de nieuwe drbd apparaat en afsluiten

   <pre><code>
   resource <b>NWS</b>_nfs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>prod-nfs-0</b> {
         address   <b>10.0.0.5</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
      on <b>prod-nfs-1</b> {
         address   <b>10.0.0.6</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
   }
   </code></pre>

   Maken van het apparaat drbd en start deze

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Overslaan van de initiële synchronisatie

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Het primaire knooppunt instellen

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Wacht totdat de nieuwe drbd apparaten worden gesynchroniseerd

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. **[1]**  Bestandssystemen op de apparaten drbd maken

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a>Cluster-Framework configureren

1. **[1]**  De standaardinstellingen wijzigen

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Het NFS drbd apparaat toevoegen aan de clusterconfiguratie

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_nfs drbd_<b>NWS</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Maken de NFS-server

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Maken de NFS-bestandssysteem resources

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive fs_<b>NWS</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NWS</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# group g-<b>NWS</b>_nfs fs_<b>NWS</b>_sapmnt

   crm(live)configure# order o-<b>NWS</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NWS</b>_nfs:promote g-<b>NWS</b>_nfs:start
   
   crm(live)configure# colocation col-<b>NWS</b>_nfs_on_drbd inf: \
     g-<b>NWS</b>_nfs ms-drbd_<b>NWS</b>_nfs:Master

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Maken de NFS-uitvoer

   <pre><code>
   sudo mkdir /srv/nfs/<b>NWS</b>/sidsys
   sudo mkdir /srv/nfs/<b>NWS</b>/sapmntsid
   sudo mkdir /srv/nfs/<b>NWS</b>/trans

   sudo crm configure

   crm(live)configure# primitive exportfs_<b>NWS</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NWS</b>" \
     options="rw,no_root_squash" \
     clientspec="*" fsid=0 \
     wait_for_leasetime_on_stop=true \
     op monitor interval="30s"

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add exportfs_<b>NWS</b>

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Een virtueel IP-resource en de statuscontrole voor de interne load balancer maken

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive vip_<b>NWS</b>_nfs IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_nfs anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 610<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add nc_<b>NWS</b>_nfs
   crm(live)configure# modgroup g-<b>NWS</b>_nfs add vip_<b>NWS</b>_nfs

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

### <a name="create-stonith-device"></a>STONITH apparaat maken

Het apparaat STONITH maakt gebruik van een Service-Principal worden geautoriseerd op basis van Microsoft Azure. Volg deze stappen voor het maken van een Service-Principal.

1. Ga naar <https://portal.azure.com>
1. Open de blade van Azure Active Directory  
   Ga naar eigenschappen en noteer de id van de Directory. Dit is de **tenant-id**.
1. Klik op App-registraties
1. Klik op Add.
1. Voer een naam, selecteer toepassingstype 'Web-app /-API, een aanmeldings-URL (bijvoorbeeld http://localhost) en klik op maken
1. De aanmeldings-URL kan niet wordt gebruikt en geldige URL
1. Selecteer de nieuwe App en sleutels op in het tabblad instellingen
1. Voer een beschrijving voor een nieuwe sleutel, selecteer 'Verloopt nooit' en klik op Opslaan
1. Noteer de waarde in. Deze wordt gebruikt als de **wachtwoord** voor de Service-Principal
1. Noteer de toepassings-id. Deze wordt gebruikt als de gebruikersnaam (**aanmeldings-id** in de onderstaande stappen) van de Service-Principal

De Service-Principal heeft geen machtigingen voor toegang tot uw Azure-resources standaard. U moet de Service-Principal machtigingen geven om te starten en stoppen (ongedaan gemaakt) alle virtuele machines van het cluster.

1. Ga naar https://portal.azure.com
1. Open de blade met alle bronnen
1. Selecteer de virtuele machine
1. Klik op toegangsbeheer (IAM)
1. Klik op Add.
1. Selecteer de rol van eigenaar
1. Voer de naam van de toepassing die u hierboven hebt gemaakt
1. Klik op OK

#### <a name="1-create-the-stonith-devices"></a>**[1]**  De STONITH apparaten maken

Nadat u de machtigingen voor de virtuele machines hebt bewerkt, kunt u de apparaten STONITH configureren in het cluster.

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]**  Het gebruik van een apparaat STONITH inschakelen

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a>(A) SCS instellen

### <a name="deploying-linux"></a>Linux implementeren

Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor SAP-toepassingen 12 die u gebruiken kunt om nieuwe virtuele machines te implementeren. De marketplace-installatiekopie bevat de resource-agent voor SAP NetWeaver.

U kunt een van de snel starten-sjablonen op github gebruiken voor het implementeren van alle vereiste bronnen. De sjabloon wordt geïmplementeerd voor de virtuele machines, de load balancer, beschikbaarheidsset enzovoort. Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [ASC's / SCS Multi SID sjabloon] [ template-multisid-xscs] of de [geconvergeerde sjabloon] [ template-converged] op de Azure portal de ASC's / SCS sjabloon alleen maakt u de regels voor taakverdeling voor de SAP NetWeaver ASC's / SCS en exemplaren Ebruikers (alleen voor Linux) terwijl de geconvergeerde sjabloon ook de regels voor taakverdeling voor een database (bijvoorbeeld Microsoft SQL Server of SAP HANA maakt). Als u van plan bent een SAP NetWeaver gebaseerd systeem installeren en u ook wilt voor de installatie van de database op de dezelfde machines, gebruikt u de [geconvergeerde sjabloon][template-converged].
1. Voer de volgende parameters
   1. Resource-voorvoegsel (alleen ASC's / SCS Multi SID-sjabloon)  
      Geef het voorvoegsel op dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   3. SAP systeem-Id (alleen geconvergeerde sjabloon)  
      Voer het SAP-systeem Id van het SAP-systeem die u wilt installeren. De Id wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   4. Stack-Type  
      Selecteer het type van de stack SAP NetWeaver
   5. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld SLES 12 BYOS
   6. DB-Type  
      Selecteer HANA
   7. Grootte van het SAP  
      De hoeveelheid SAP's het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraag uw SAP-technologie Partner of System Integrator
   8. Beschikbaarheid van het systeem  
      Selecteer HA
   9. Gebruikersnaam van de beheerder en het wachtwoord van beheerder  
      Een nieuwe gebruiker wordt gemaakt die kunnen worden gebruikt voor aanmelding bij de computer.
   10. Subnet-Id  
   De ID van het subnet waarmee de virtuele machines moet worden verbonden.  Laat leeg als u wilt maken van een nieuw virtueel netwerk of Selecteer hetzelfde subnet die u gebruikt of gemaakt als onderdeel van de implementatie van de NFS-server. De ID meestal ziet eruit als /subscriptions/**&lt;abonnements-id&gt;**/resourceGroups/**&lt;Resourcegroepnaam&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtuele-netwerknaam&gt;**/subnets/**&lt;subnetnaam&gt;**

### <a name="installation"></a>Installeren

De volgende items worden voorafgegaan door een **[A]** - van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  SLES bijwerken

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Ssh toegang inschakelen

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. **[2]**  Ssh toegang inschakelen

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. **[1]**  Ssh toegang inschakelen

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Installeren HA-uitbreiding
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]**  Update SAP resource agents  
   
   Een patch voor het pakket resource agents is vereist voor de nieuwe configuratie die in dit artikel wordt beschreven. U kunt controleren als de patch al is geïnstalleerd met de volgende opdracht

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   De uitvoer moet er ongeveer als

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   Als de parameter IS_ERS niet wordt gevonden in de opdracht grep, moet u installeert de patch vermeld op [de SUSE downloadpagina](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. **[A]**  Hostnamen instellen   

   U kunt gebruik van een DNS-server of wijzig de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe het bestand/etc/hosts te gebruiken.
   Het IP-adres en de hostnaam in de volgende opdrachten vervangen

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Voeg de volgende regels/etc/hosts. Wijzig de IP-adres en hostnaam overeenkomen met uw omgeving   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. **[1]**  Cluster installeren
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Knooppunt aan cluster toevoegen
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Hacluster wachtwoord wijzigen naar hetzelfde wachtwoord

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Corosync voor het gebruik van andere transport en voeg nodelist configureren. Cluster werkt niet anders.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   De volgende vet inhoud toevoegen aan het bestand.
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>nws-cl-0</b>
      ring0_addr:     10.0.0.14
     }
     node {
      # IP address of <b>nws-cl-1</b>
      ring0_addr:     10.0.0.13
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   Start de service corosync opnieuw

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[A]**  Drbd-onderdelen installeren

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Maken van een partitie voor het apparaat drbd

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. **[A]**  Configuraties LVM maken

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. **[A]**  De SCS drbd apparaat maken

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   Invoegen van de configuratie voor de nieuwe drbd apparaat en afsluiten

   <pre><code>
   resource <b>NWS</b>_ascs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
   }
   </code></pre>

   Maken van het apparaat drbd en start deze

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. **[A]**  De Ebruikers drbd apparaat maken

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   Invoegen van de configuratie voor de nieuwe drbd apparaat en afsluiten

   <pre><code>
   resource <b>NWS</b>_ers {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
   }
   </code></pre>

   Maken van het apparaat drbd en start deze

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Overslaan van de initiële synchronisatie

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Het primaire knooppunt instellen

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Wacht totdat de nieuwe drbd apparaten worden gesynchroniseerd

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:93991268 nr:0 dw:93991268 dr:93944920 al:383 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 1: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:6047920 nr:0 dw:6047920 dr:6039112 al:34 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 2: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:5142732 nr:0 dw:5142732 dr:5133924 al:30 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. **[1]**  Bestandssystemen op de apparaten drbd maken

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a>Cluster-Framework configureren

**[1]**  De standaardinstellingen wijzigen

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver installatie voorbereiden

1. **[A]**  Maken van de gedeelde mappen

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. **[A]**  Autofs configureren
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Maak een bestand met

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   Opnieuw opstarten autofs om te koppelen van de nieuwe shares

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. **[A]**  Bestand WISSELEN configureren
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   De Agent voor het activeren van de wijziging opnieuw starten

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASC's / Ebruikers installeren

1. **[1]**  Een virtueel IP-resource en de statuscontrole voor de interne load balancer maken

   <pre><code>
   sudo crm node standby <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ASCS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ascs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ASCS drbd_<b>NWS</b>_ASCS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ASCS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/usr/sap/<b>NWS</b>/ASCS<b>00</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.10</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   crm(live)configure# group g-<b>NWS</b>_ASCS nc_<b>NWS</b>_ASCS vip_<b>NWS</b>_ASCS fs_<b>NWS</b>_ASCS \
      meta resource-stickiness=3000

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ASCS inf: \
     ms-drbd_<b>NWS</b>_ASCS:promote g-<b>NWS</b>_ASCS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ASCS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ASCS:Master g-<b>NWS</b>_ASCS
   
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r

   # Node nws-cl-1: standby
   # <b>Online: [ nws-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      Stopped: [ nws-cl-1 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASC's installeren  

   SAP NetWeaver ASC's installeren als hoofdmap op het eerste knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer frontend-configuratie voor de ASC's bijvoorbeeld <b>nws-ASC's</b>, <b>10.0.0.10</b> en het exemplaarnummer dat u voor de test van de load balancer bijvoorbeeld gebruikt <b>00</b>.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. **[1]**  Een virtueel IP-resource en de statuscontrole voor de interne load balancer maken

   <pre><code>
   sudo crm node standby <b>nws-cl-0</b>
   sudo crm node online <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ERS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ers" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ERS drbd_<b>NWS</b>_ERS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ERS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/usr/sap/<b>NWS</b>/ERS<b>02</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.11</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0

   crm(live)configure# group g-<b>NWS</b>_ERS nc_<b>NWS</b>_ERS vip_<b>NWS</b>_ERS fs_<b>NWS</b>_ERS

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ERS inf: \
     ms-drbd_<b>NWS</b>_ERS:promote g-<b>NWS</b>_ERS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ERS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ERS:Master g-<b>NWS</b>_ERS
   
   crm(live)configure# commit
   # WARNING: Resources nc_NWS_ASCS,nc_NWS_ERS,nc_NWS_nfs violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r

   # Node <b>nws-cl-0: standby</b>
   # <b>Online: [ nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver Ebruikers installeren  

   SAP NetWeaver Ebruikers installeren als de hoofdmap van het tweede knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer frontend-configuratie voor de gebruikers bijvoorbeeld <b>nws ebruikers</b>, <b>10.0.0.11</b> en het exemplaarnummer dat u voor de test van de load balancer bijvoorbeeld gebruikt <b>02</b>.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > Gebruik SWPM SP 20 PL 05 of hoger. Lagere versies Stel de machtigingen niet correct en mislukt de installatie.
   > 

1. **[1]**  Adapt de ASC's / SCS en Ebruikers exemplaar profielen
 
   * ASC's / SCS-profiel

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * Ebruikers profiel

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. **[A]**  Keep-Alive configureren

   De communicatie tussen de SAP NetWeaver toepassingsserver en de ASC's / SCS wordt via een software load balancer doorgestuurd. De load balancer wordt niet-actieve verbindingen na een configureerbare time-out verbroken. Om dit te voorkomen, moet u een parameter in het profiel voor SAP NetWeaver ASC's / SCS instellen en wijzigen van de instellingen van het Linux-systeem. Lees [SAP-notitie 1410736] [ 1410736] voor meer informatie.
   
   De ASC's / SCS profiel parameter CLR niet/encni/set_so_keepalive is al toegevoegd in de vorige stap.

   <pre><code> 
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  De SAP-gebruikers configureren na de installatie
 
   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. **[1]**  De ASC's en Ebruikers SAP-services aan het bestand sapservice toevoegen

   De ASC's service-vermelding voor het tweede knooppunt en de vermelding van de service Ebruikers kopiëren naar het eerste knooppunt toevoegen.

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  De SAP-clusterbronnen maken

   <pre><code>
   sudo crm configure property maintenance-mode="true"

   sudo crm configure

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ASCS<b>00</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ERS<b>02</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000

   crm(live)configure# modgroup g-<b>NWS</b>_ASCS add rsc_sap_<b>NWS</b>_ASCS<b>00</b>
   crm(live)configure# modgroup g-<b>NWS</b>_ERS add rsc_sap_<b>NWS</b>_ERS<b>02</b>

   crm(live)configure# colocation col_sap_<b>NWS</b>_no_both -5000: g-<b>NWS</b>_ERS g-<b>NWS</b>_ASCS
   crm(live)configure# location loc_sap_<b>NWS</b>_failover_to_ers rsc_sap_<b>NWS</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NWS</b> eq 1
   crm(live)configure# order ord_sap_<b>NWS</b>_first_start_ascs Optional: rsc_sap_<b>NWS</b>_ASCS<b>00</b>:start rsc_sap_<b>NWS</b>_ERS<b>02</b>:stop symmetrical=false

   crm(live)configure# commit
   crm(live)configure# exit

   sudo crm configure property maintenance-mode="false"
   sudo crm node online <b>nws-cl-0</b>
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r

   # Online: <b>[ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   </code></pre>

### <a name="create-stonith-device"></a>STONITH apparaat maken

Het apparaat STONITH maakt gebruik van een Service-Principal worden geautoriseerd op basis van Microsoft Azure. Volg deze stappen voor het maken van een Service-Principal.

1. Ga naar <https://portal.azure.com>
1. Open de blade van Azure Active Directory  
   Ga naar eigenschappen en noteer de id van de Directory. Dit is de **tenant-id**.
1. Klik op App-registraties
1. Klik op Add.
1. Voer een naam, selecteer toepassingstype 'Web-app /-API, een aanmeldings-URL (bijvoorbeeld http://localhost) en klik op maken
1. De aanmeldings-URL kan niet wordt gebruikt en geldige URL
1. Selecteer de nieuwe App en sleutels op in het tabblad instellingen
1. Voer een beschrijving voor een nieuwe sleutel, selecteer 'Verloopt nooit' en klik op Opslaan
1. Noteer de waarde in. Deze wordt gebruikt als de **wachtwoord** voor de Service-Principal
1. Noteer de toepassings-id. Deze wordt gebruikt als de gebruikersnaam (**aanmeldings-id** in de onderstaande stappen) van de Service-Principal

De Service-Principal heeft geen machtigingen voor toegang tot uw Azure-resources standaard. U moet de Service-Principal machtigingen geven om te starten en stoppen (ongedaan gemaakt) alle virtuele machines van het cluster.

1. Ga naar https://portal.azure.com
1. Open de blade met alle bronnen
1. Selecteer de virtuele machine
1. Klik op toegangsbeheer (IAM)
1. Klik op Add.
1. Selecteer de rol van eigenaar
1. Voer de naam van de toepassing die u hierboven hebt gemaakt
1. Klik op OK

#### <a name="1-create-the-stonith-devices"></a>**[1]**  De STONITH apparaten maken

Nadat u de machtigingen voor de virtuele machines hebt bewerkt, kunt u de apparaten STONITH configureren in het cluster.

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]**  Het gebruik van een apparaat STONITH inschakelen

Het gebruik van een apparaat STONITH inschakelen

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a>Installeren van de database

In dit voorbeeld is een SAP HANA System Replication geïnstalleerd en geconfigureerd. SAP HANA wordt uitgevoerd in hetzelfde cluster als de SAP NetWeaver ASC's / SCS en gebruikers. U kunt ook een SAP HANA installeren op een cluster toegewezen. Zie [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)] [ sap-hana-ha] voor meer informatie.

### <a name="prepare-for-sap-hana-installation"></a>Voorbereiden voor SAP HANA-installatie

In het algemeen wordt aangeraden met LVM voor volumes die bij het opslaan van gegevens en logboekbestanden. Voor testdoeleinden kunt u ook voor het opslaan van de gegevens en logboekbestanden rechtstreeks op een gewone schijf.

1. **[A]**  LVM  
   Het volgende voorbeeld wordt ervan uitgegaan dat de virtuele machines vier gegevensschijven gekoppeld die moeten worden gebruikt voor het maken van twee volumes hebben.
   
   Maak fysieke volumes voor alle schijven die u wilt gebruiken.
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   Maak een groep volume voor de gegevensbestanden, één volume groep voor de logboekbestanden en één voor de gedeelde map van SAP HANA
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   De logische volumes maken
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   Maak de mappen koppelen en kopieer de UUID van alle logische volumes
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   Autofs vermeldingen voor de drie logische volumes maken
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   Deze regel moet worden sudo vi /etc/auto.direct invoegen
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   Koppel de nieuwe volumes
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. **[A]**  Gewone schijven  

   Voor kleine of demo-systemen, kunt u uw bestanden HANA gegevens en logboekbestanden op één schijf plaatsen. De volgende opdrachten een partitie maken op /dev/sdc en formatteert u het met xfs.
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down the id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   Deze regel moet worden /etc/auto.direct invoegen
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   Maken van de doelmap en de schijf koppelen.
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a>SAP HANA installeren

De volgende stappen zijn gebaseerd op hoofdstuk 4 van de [SAP HANA SR prestaties geoptimaliseerd Scenario handleiding] [ suse-hana-ha-guide] om SAP HANA System replicatie te installeren. Lees dit voordat u doorgaat met de installatie.

1. **[A]**  Hdblcm uitvoeren vanaf de DVD HANA
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. **[A]**  SAP Hostagent bijwerken

   Download de meest recente Hostagent SAP-archief van de [SAP Softwarecenter] [ sap-swcenter] en voer de volgende opdracht om de agent bijwerken. Het pad naar het archief verwijzen naar de door u gedownloade bestand vervangen.
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path to SAP Host Agent SAR&gt;</b> 
   </code></pre>

1. **[1]**  Maken HANA-replicatie (als root)  

   Voer de volgende opdracht. Zorg ervoor dat vet tekenreeksen (HANA systeem-ID HDB en exemplaar getal 03) vervangen door de waarden van de SAP HANA-installatie.
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. **[A]**  Keystore-vermelding (als root) maken

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. **[1]**  Backup database (als root)

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. **[1]**  Overschakelen naar de HANA sapsid gebruiker en de primaire site te maken.

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  Overschakelen naar de HANA sapsid gebruiker en de secundaire site te maken.

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. **[1]**  Clusterbronnen SAP HANA maken

   Maak eerst de topologie.
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number and HANA system id
   
   crm(live)configure# primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b>   ocf:suse:SAPHanaTopology \
     operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"
    
   crm(live)configure# clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>
   
   Maak vervolgens de bronnen HANA
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
    
   crm(live)configure# primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
    
   crm(live)configure# ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
    
   crm(live)configure# primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
     meta target-role="Started" is-managed="true" \ 
     operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
     op monitor interval="10s" timeout="20s" \ 
     params ip="<b>10.0.0.12</b>" 

   crm(live)configure# primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
     op monitor timeout=20s interval=10 depth=0 

   crm(live)configure# group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  

   crm(live)configure# order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r

   # <b>Online: [ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Clone Set: cln_SAPHanaTopology_HDB_HDB03 [rsc_SAPHanaTopology_HDB_HDB03]
   #      <b>Started: [ nws-cl-0 nws-cl-1 ]</b>
   #  Master/Slave Set: msl_SAPHana_HDB_HDB03 [rsc_SAPHana_HDB_HDB03]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g_ip_HDB_HDB03
   #      rsc_ip_HDB_HDB03   (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      rsc_nc_HDB_HDB03   (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   # rsc_st_azure_1  (stonith:fence_azure_arm):      <b>Started nws-cl-0</b>
   # rsc_st_azure_2  (stonith:fence_azure_arm):      <b>Started nws-cl-1</b>
   </code></pre>

1. **[1]**  Het SAP NetWeaver database-exemplaar installeren

   Het exemplaar van de database SAP NetWeaver installeren als hoofdmap met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer frontend-configuratie voor de database bijvoorbeeld <b>nws-db</b> en <b>10.0.0.12</b>.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver application server-installatie

Volg deze stappen voor het installeren van een SAP-toepassingsserver. De onderstaande stappen wordt ervan uitgegaan dat u de toepassingsserver installeren op een server die verschilt van de ASC's / SCS en HANA-servers. Anders zijn enkele van de onderstaande stappen (zoals het configureren van hostnamen) niet nodig.

1. Setup van hostnaamomzetting    
   U kunt gebruik van een DNS-server of wijzig de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe het bestand/etc/hosts te gebruiken.
   Het IP-adres en de hostnaam in de volgende opdrachten vervangen
   ```bash
   sudo vi /etc/hosts
   ```
   Voeg de volgende regels/etc/hosts. Wijzig de IP-adres en hostnaam overeenkomen met uw omgeving    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of the application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. Maken van de map sapmnt

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. Autofs configureren
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Maak een nieuw bestand met

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   Opnieuw opstarten autofs om te koppelen van de nieuwe shares

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. Wisselbestand configureren
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   De Agent voor het activeren van de wijziging opnieuw starten

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. SAP NetWeaver application server installeren

   Een primaire of extra SAP NetWeaver toepassingsserver installeren.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA secure store bijwerken

   Werk de SAP HANA secure store om te verwijzen naar de virtuele naam van de SAP HANA System replicatie-instellingen.
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a>Volgende stappen
* [Azure virtuele Machines, planning en implementatie voor SAP][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]