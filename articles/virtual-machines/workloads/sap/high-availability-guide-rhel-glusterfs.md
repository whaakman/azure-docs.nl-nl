---
title: GlusterFS op Azure VM's op Red Hat Enterprise Linux voor SAP NetWeaver | Microsoft Docs
description: GlusterFS op Azure VM's op Red Hat Enterprise Linux voor SAP NetWeaver
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
ms.date: 08/16/2018
ms.author: sedusch
ms.openlocfilehash: 484a0043b9b5eefa5491dee75e87244d1c001620
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342320"
---
# <a name="glusterfs-on-azure-vms-on-red-hat-enterprise-linux-for-sap-netweaver"></a>GlusterFS op Azure VM's op Red Hat Enterprise Linux voor SAP NetWeaver

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md

In dit artikel wordt beschreven hoe u de virtuele machines implementeren, configureren van de virtuele machines en installeren van een cluster GlusterFS die kan worden gebruikt voor het opslaan van de gedeelde gegevens van een maximaal beschikbare SAP-systeem.
Deze handleiding wordt beschreven hoe u GlusterFS die wordt gebruikt door twee SAP-systemen, NW1 en NW2 instelt. De namen van de resources (bijvoorbeeld virtuele machines, virtuele netwerken) in het voorbeeld wordt ervan uitgegaan dat u hebt gebruikt de [SAP-server de sjabloon] [ template-file-server] met het voorvoegsel van de resource **glust**.

Lees eerst de volgende SAP-opmerkingen en documenten

* SAP-notitie [1928533], heeft:
  * Lijst met Azure-VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belangrijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturingssysteem (OS) en combinaties van database
  * Vereiste kernel van SAP-versie voor Windows en Linux op Microsoft Azure

* SAP-notitie [2015553] bevat vereisten voor SAP-ondersteunde SAP software-implementaties in Azure.
* SAP-notitie [2002167] heeft aanbevolen instellingen voor Red Hat Enterprise Linux OS
* SAP-notitie [2009879] bevat richtlijnen voor SAP HANA voor Red Hat Enterprise Linux
* SAP-notitie [2178632] vindt u meer informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste versie van de SAP-Host-Agent voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over de licentieverlening voor SAP op Linux in Azure.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure uitgebreide controle-extensie voor SAP.
* [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) bevat alle SAP-opmerkingen voor Linux vereist.
* [Azure virtuele Machines, planning en implementatie van SAP op Linux][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP op Linux (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [Productdocumentatie voor Red Hat Gluster Storage](https://access.redhat.com/documentation/red_hat_gluster_storage/)
* Algemene RHEL-documentatie
  * [Overzicht van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Beheer van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Naslaginformatie over hoge beschikbaarheid-invoegtoepassing](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Azure specifieke RHEL-documentatie:
  * [Ondersteuningsbeleid voor RHEL Clusters met hoge beschikbaarheid - Microsoft Azure Virtual Machines als leden van het Cluster](https://access.redhat.com/articles/3131341)
  * [Installeren en configureren van een Red Hat Enterprise Linux 7.4 (en hoger) Cluster met hoge beschikbaarheid voor Microsoft Azure](https://access.redhat.com/articles/3252491)

## <a name="overview"></a>Overzicht

SAP NetWeaver vereist voor het bereiken van hoge beschikbaarheid gedeelde opslag. GlusterFS is geconfigureerd in een cluster dat gescheiden en kunnen worden gebruikt door meerdere SAP-systemen.

![Overzicht van SAP NetWeaver hoge beschikbaarheid](./media/high-availability-guide-rhel-glusterfs/rhel-glusterfs.png)

## <a name="set-up-glusterfs"></a>GlusterFS instellen

U kunt een Azure-sjabloon gebruiken om alle vereiste Azure-resources te implementeren, met inbegrip van de virtuele machines, beschikbaarheidsset en network interfaces of u kunt de resources handmatig implementeren.

### <a name="deploy-linux-via-azure-template"></a>Linux via Azure-sjabloon implementeren

De Azure Marketplace bevat een afbeelding voor Red Hat Enterprise Linux die u gebruiken kunt om nieuwe virtuele machines te implementeren.
U kunt een van de snelstartsjablonen van op github gebruiken om alle vereiste resources te implementeren. De sjabloon implementeert de virtuele machines, beschikbaarheidsset enzovoort. Volg deze stappen om de sjabloon te implementeren:

1. Open de [SAP-server de sjabloon] [ template-file-server] in Azure portal
1. Voer de volgende parameters
   1. Voorvoegsel van de resource  
      Geef het voorvoegsel dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   2. Telling van SAP-systeem Voer het nummer van de SAP-systemen die deze bestandsserver wordt gebruikt. Hierdoor wordt het vereiste aantal schijven enzovoort geïmplementeerd.
   3. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld RHEL 7
   4. Admin Username, Administrator-wachtwoord of SSH-sleutel  
      Een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de machine.
   5. Subnet-ID  
      Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID is meestal ziet eruit als /subscriptions/**&lt;abonnements-ID&gt;**/resourceGroups/**&lt;groepsnaam voor accountresources&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuele-netwerknaam&gt;**/subnets/**&lt;subnetnaam&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Handmatig implementeren van Linux via Azure portal

U moet eerst de virtuele machines voor dit cluster te maken. Daarna wordt u een load balancer maken en gebruiken van de virtuele machines in de back endadresgroepen.

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Een Beschikbaarheidsset maken  
   De maximale updatedomein instellen
1. Virtuele Machine 1 maken  
   Ten minste RHEL 7, in dit de Red Hat Enterprise Linux 7.4-voorbeeldafbeelding <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt  
1. Virtuele Machine 2 maken  
   Ten minste RHEL 7, in dit de Red Hat Enterprise Linux 7.4-voorbeeldafbeelding <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt  
1. Een gegevensschijf voor elk SAP-systeem toevoegen aan beide virtuele machines.

### <a name="configure-glusterfs"></a>GlusterFS configureren

De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1, **[2]** - alleen van toepassing op knooppunt 2, **[3].**  - alleen van toepassing op knooppunt 3.

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen

   <pre><code># IP addresses of the Gluster nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   </code></pre>

1. **[A]**  Registreren

   Registreren van uw virtuele machines en deze koppelen aan een pool met opslagplaatsen voor RHEL 7 en GlusterFS

   <pre><code>sudo subscription-manager register
   sudo subscription-manager attach --pool=&lt;pool id&gt;
   </code></pre>

1. **[A]**  GlusterFS inschakelen-opslagplaatsen

   Om te installeren de vereiste pakketten, schakel de volgende opslagplaatsen.

   <pre><code>sudo subscription-manager repos --disable "*"
   sudo subscription-manager repos --enable=rhel-7-server-rpms
   sudo subscription-manager repos --enable=rh-gluster-3-for-rhel-7-server-rpms
   </code></pre>
  
1. **[A]**  GlusterFS installeren pakketten

   Deze pakketten installeren op alle GlusterFS-knooppunten

   <pre><code>sudo yum -y install redhat-storage-server
   </code></pre>

   Start opnieuw op de knooppunten na de installatie.

1. **[A]**  Firewall wijzigen

   Firewallregels voor clientverkeer op de knooppunten GlusterFS toevoegen.

   <pre><code># list the available zones
   firewall-cmd --get-active-zones
   
   sudo firewall-cmd --zone=public --add-service=glusterfs --permanent
   sudo firewall-cmd --zone=public --add-service=glusterfs
   </code></pre>

1. **[A]**  Inschakelen en GlusterFS service starten

   Start de service GlusterFS op alle knooppunten.

   <pre><code>sudo systemctl start glusterd
   sudo systemctl enable glusterd
   </code></pre>

1. **[1]**  GluserFS maken

   Voer de volgende opdrachten om de GlusterFS-cluster te maken

   <pre><code>sudo gluster peer probe glust-1
   sudo gluster peer probe glust-2
   
   # Check gluster peer status
   sudo gluster peer status
   
   # Number of Peers: 2
   # 
   # Hostname: glust-1
   # Uuid: 10d43840-fee4-4120-bf5a-de9c393964cd
   # State: Accepted peer request (Connected)
   # 
   # Hostname: glust-2
   # Uuid: 9e340385-12fe-495e-ab0f-4f851b588cba
   # State: Accepted peer request (Connected)
   </code></pre>

1. **[2]**  Testen peer-status

   De status van de peer op het tweede knooppunt testen

   <pre><code>sudo gluster peer status
   # Number of Peers: 2
   #
   # Hostname: glust-0
   # Uuid: 6bc6927b-7ee2-461b-ad04-da123124d6bd
   # State: Peer in Cluster (Connected)
   #
   # Hostname: glust-2
   # Uuid: 9e340385-12fe-495e-ab0f-4f851b588cba
   # State: Peer in Cluster (Connected)
   </code></pre>

1. **[3]**  Testen peer-status

   De status van de peer op de derde knooppunt testen

   <pre><code>sudo gluster peer status
   # Number of Peers: 2
   #
   # Hostname: glust-0
   # Uuid: 6bc6927b-7ee2-461b-ad04-da123124d6bd
   # State: Peer in Cluster (Connected)
   #
   # Hostname: glust-1
   # Uuid: 10d43840-fee4-4120-bf5a-de9c393964cd
   # State: Peer in Cluster (Connected)
   </code></pre>

1. **[A]**  LVM maken

   In dit voorbeeld wordt de GlusterFS gebruikt voor twee SAP-systemen, NW1 en NW2. Gebruik de volgende opdrachten om te maken van LVM-configuraties voor deze SAP-systemen.

   Deze opdrachten gebruiken voor NW1

   <pre><code>sudo pvcreate --dataalignment 1024K /dev/disk/azure/scsi1/lun0
   sudo pvscan
   sudo vgcreate --physicalextentsize 256K rhgs-<b>NW1</b> /dev/disk/azure/scsi1/lun0
   sudo vgscan
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW1</b>/sapmnt
   sudo lvcreate -l 20%FREE -n rhgs-<b>NW1</b>/trans
   sudo lvcreate -l 10%FREE -n rhgs-<b>NW1</b>/sys
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW1</b>/ascs
   sudo lvcreate -l 100%FREE -n rhgs-<b>NW1</b>/aers
   sudo lvscan
   
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/sapmnt
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/trans
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/sys
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/ascs
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/aers
   
   sudo mkdir -p /rhs/<b>NW1</b>/sapmnt
   sudo mkdir -p /rhs/<b>NW1</b>/trans
   sudo mkdir -p /rhs/<b>NW1</b>/sys
   sudo mkdir -p /rhs/<b>NW1</b>/ascs
   sudo mkdir -p /rhs/<b>NW1</b>/aers
   
   sudo chattr +i /rhs/<b>NW1</b>/sapmnt
   sudo chattr +i /rhs/<b>NW1</b>/trans
   sudo chattr +i /rhs/<b>NW1</b>/sys
   sudo chattr +i /rhs/<b>NW1</b>/ascs
   sudo chattr +i /rhs/<b>NW1</b>/aers

   echo -e "/dev/rhgs-<b>NW1</b>/sapmnt\t/rhs/<b>NW1</b>/sapmnt\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/trans\t/rhs/<b>NW1</b>/trans\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/sys\t/rhs/<b>NW1</b>/sys\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/ascs\t/rhs/<b>NW1</b>/ascs\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/aers\t/rhs/<b>NW1</b>/aers\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   sudo mount -a
   </code></pre>

   Deze opdrachten gebruiken voor NW2

   <pre><code>sudo pvcreate --dataalignment 1024K /dev/disk/azure/scsi1/lun1
   sudo pvscan
   sudo vgcreate --physicalextentsize 256K rhgs-<b>NW2</b> /dev/disk/azure/scsi1/lun1
   sudo vgscan
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW2</b>/sapmnt
   sudo lvcreate -l 20%FREE -n rhgs-<b>NW2</b>/trans
   sudo lvcreate -l 10%FREE -n rhgs-<b>NW2</b>/sys
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW2</b>/ascs
   sudo lvcreate -l 100%FREE -n rhgs-<b>NW2</b>/aers
   
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/sapmnt
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/trans
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/sys
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/ascs
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/aers
   
   sudo mkdir -p /rhs/<b>NW2</b>/sapmnt
   sudo mkdir -p /rhs/<b>NW2</b>/trans
   sudo mkdir -p /rhs/<b>NW2</b>/sys
   sudo mkdir -p /rhs/<b>NW2</b>/ascs
   sudo mkdir -p /rhs/<b>NW2</b>/aers
   
   sudo chattr +i /rhs/<b>NW2</b>/sapmnt
   sudo chattr +i /rhs/<b>NW2</b>/trans
   sudo chattr +i /rhs/<b>NW2</b>/sys
   sudo chattr +i /rhs/<b>NW2</b>/ascs
   sudo chattr +i /rhs/<b>NW2</b>/aers
   sudo lvscan
   
   echo -e "/dev/rhgs-<b>NW2</b>/sapmnt\t/rhs/<b>NW2</b>/sapmnt\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/trans\t/rhs/<b>NW2</b>/trans\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/sys\t/rhs/<b>NW2</b>/sys\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/ascs\t/rhs/<b>NW2</b>/ascs\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/aers\t/rhs/<b>NW2</b>/aers\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   sudo mount -a
   </code></pre>

1. **[1]**  Het gedistribueerde volume maken

   Gebruik de volgende opdrachten om te maken van het volume GlusterFS voor NW1 en start de service.

   <pre><code>sudo gluster vol create <b>NW1</b>-sapmnt replica 3 glust-0:/rhs/<b>NW1</b>/sapmnt glust-1:/rhs/<b>NW1</b>/sapmnt glust-2:/rhs/<b>NW1</b>/sapmnt force
   sudo gluster vol create <b>NW1</b>-trans replica 3 glust-0:/rhs/<b>NW1</b>/trans glust-1:/rhs/<b>NW1</b>/trans glust-2:/rhs/<b>NW1</b>/trans force
   sudo gluster vol create <b>NW1</b>-sys replica 3 glust-0:/rhs/<b>NW1</b>/sys glust-1:/rhs/<b>NW1</b>/sys glust-2:/rhs/<b>NW1</b>/sys force
   sudo gluster vol create <b>NW1</b>-ascs replica 3 glust-0:/rhs/<b>NW1</b>/ascs glust-1:/rhs/<b>NW1</b>/ascs glust-2:/rhs/<b>NW1</b>/ascs force
   sudo gluster vol create <b>NW1</b>-aers replica 3 glust-0:/rhs/<b>NW1</b>/aers glust-1:/rhs/<b>NW1</b>/aers glust-2:/rhs/<b>NW1</b>/aers force
   
   sudo gluster volume start <b>NW1</b>-sapmnt
   sudo gluster volume start <b>NW1</b>-trans
   sudo gluster volume start <b>NW1</b>-sys
   sudo gluster volume start <b>NW1</b>-ascs
   sudo gluster volume start <b>NW1</b>-aers
   </code></pre>

   Gebruik de volgende opdrachten om te maken van het volume GlusterFS voor NW2 en start de service.

   <pre><code>sudo gluster vol create <b>NW2</b>-sapmnt replica 3 glust-0:/rhs/<b>NW2</b>/sapmnt glust-1:/rhs/<b>NW2</b>/sapmnt glust-2:/rhs/<b>NW2</b>/sapmnt force
   sudo gluster vol create <b>NW2</b>-trans replica 3 glust-0:/rhs/<b>NW2</b>/trans glust-1:/rhs/<b>NW2</b>/trans glust-2:/rhs/<b>NW2</b>/trans force
   sudo gluster vol create <b>NW2</b>-sys replica 3 glust-0:/rhs/<b>NW2</b>/sys glust-1:/rhs/<b>NW2</b>/sys glust-2:/rhs/<b>NW2</b>/sys force
   sudo gluster vol create <b>NW2</b>-ascs replica 3 glust-0:/rhs/<b>NW2</b>/ascs glust-1:/rhs/<b>NW2</b>/ascs glust-2:/rhs/<b>NW2</b>/ascs force
   sudo gluster vol create <b>NW2</b>-aers replica 3 glust-0:/rhs/<b>NW2</b>/aers glust-1:/rhs/<b>NW2</b>/aers glust-2:/rhs/<b>NW2</b>/aers force
   
   sudo gluster volume start <b>NW2</b>-sapmnt
   sudo gluster volume start <b>NW2</b>-trans
   sudo gluster volume start <b>NW2</b>-sys
   sudo gluster volume start <b>NW2</b>-ascs
   sudo gluster volume start <b>NW2</b>-aers
   </code></pre>

## <a name="next-steps"></a>Volgende stappen

* [De SAP ASCS en -database installeren](high-availability-guide-rhel.md)
* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
