---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux | Microsoft Docs
description: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux
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
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: c7805e64c4f387b870922dcb63e20d86f691092a
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119013"
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver-on-red-hat-enterprise-linux"></a>Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux

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
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md
[glusterfs-ha]:high-availability-guide-rhel-glusterfs.md

In dit artikel wordt beschreven hoe u de virtuele machines implementeren, configureren van de virtuele machines, installeer het framework van het cluster en installeren van een maximaal beschikbare 7,50 van SAP NetWeaver-systeem.
In de voorbeeldconfiguraties, enzovoort-opdrachten voor installatie. ASCS exemplaarnummer 00 INGEN exemplaarnummer 02 en SAP-systeem-ID NW1 wordt gebruikt. De namen van de resources (bijvoorbeeld virtuele machines, virtuele netwerken) in het voorbeeld wordt ervan uitgegaan dat u hebt gebruikt de [ASCS/SCS sjabloon] [ template-multisid-xscs] met het voorvoegsel NW1 Resource om de resources te maken.

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
* [Azure Virtual Machines-implementatie voor SAP op Linux][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [Productdocumentatie voor Red Hat Gluster Storage](https://access.redhat.com/documentation/red_hat_gluster_storage/)
* [SAP Netweaver in pacemaker cluster](https://access.redhat.com/articles/3150081)
* Algemene RHEL-documentatie
  * [Overzicht van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Beheer van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Naslaginformatie over hoge beschikbaarheid-invoegtoepassing](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [Configureren van ASCS/INGEN voor SAP Netweaver met zelfstandige resources in RHEL 7.5](https://access.redhat.com/articles/3569681)
* Azure specifieke RHEL-documentatie:
  * [Ondersteuningsbeleid voor RHEL Clusters met hoge beschikbaarheid - Microsoft Azure Virtual Machines als leden van het Cluster](https://access.redhat.com/articles/3131341)
  * [Installeren en configureren van een Red Hat Enterprise Linux 7.4 (en hoger) Cluster met hoge beschikbaarheid voor Microsoft Azure](https://access.redhat.com/articles/3252491)

## <a name="overview"></a>Overzicht

SAP NetWeaver vereist voor het bereiken van hoge beschikbaarheid gedeelde opslag. GlusterFS is geconfigureerd in een cluster dat gescheiden en kunnen worden gebruikt door meerdere SAP-systemen.

![Overzicht van SAP NetWeaver hoge beschikbaarheid](./media/high-availability-guide-rhel/ha-rhel.png)

SAP NetWeaver ASCS, SAP NetWeaver SCS INGEN voor SAP NetWeaver en SAP HANA-database gebruiken virtuele hostnaam en virtuele IP-adressen. In Azure, wordt een load balancer overstappen naar een virtueel IP-adres. De volgende lijst bevat de configuratie van de (A) SCS en INGEN load balancer.

### <a name="ascs"></a>(A)SCS

* Front-end-configuratie
  * IP-adres 10.0.0.7
* Back-endconfiguratie
  * Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken) SCS/INGEN cluster
* Testpoort
  * Poort 620**&lt;nr&gt;**
* Regels van loadbalancing in Plbscheduler
  * 32**&lt;nr&gt;** TCP
  * 36**&lt;nr&gt;** TCP
  * 39**&lt;nr&gt;** TCP
  * 81**&lt;nr&gt;** TCP
  * 5**&lt;nr&gt;** 13 TCP
  * 5**&lt;nr&gt;** 14 TCP
  * 5**&lt;nr&gt;** 16 TCP

### <a name="ers"></a>INGEN

* Front-end-configuratie
  * IP-adres 10.0.0.8
* Back-endconfiguratie
  * Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken) SCS/INGEN cluster
* Testpoort
  * Poort 621**&lt;nr&gt;**
* Regels van loadbalancing in Plbscheduler
  * 33**&lt;nr&gt;** TCP
  * 5**&lt;nr&gt;** 13 TCP
  * 5**&lt;nr&gt;** 14 TCP
  * 5**&lt;nr&gt;** 16 TCP

## <a name="setting-up-glusterfs"></a>Instellen van GlusterFS

SAP NetWeaver vereist gedeelde opslag voor de map transport en profiel. Lezen [GlusterFS op Azure Virtual machines van Red Hat Enterprise Linux voor SAP NetWeaver] [ glusterfs-ha] over het instellen van GlusterFS voor SAP NetWeaver.

## <a name="setting-up-ascs"></a>(A) SCS instellen

U kunt een Azure-sjabloon gebruiken voor het implementeren van alle vereiste Azure-resources, met inbegrip van de virtuele machines, beschikbaarheid en de load balancer of kunt u de resources handmatig implementeren.

### <a name="deploy-linux-via-azure-template"></a>Linux via Azure-sjabloon implementeren

De Azure Marketplace bevat een afbeelding voor Red Hat Enterprise Linux die u gebruiken kunt om nieuwe virtuele machines te implementeren. U kunt een van de snelstartsjablonen van op github gebruiken om alle vereiste resources te implementeren. De sjabloon implementeert de virtuele machines, load balancer, beschikbaarheidsset enzovoort. Volg deze stappen om de sjabloon te implementeren:

1. Open de [ASCS/SCS sjabloon] [ template-multisid-xscs] in Azure portal  
1. Voer de volgende parameters
   1. Voorvoegsel van de resource  
      Geef het voorvoegsel dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   1. Stack-Type  
      Selecteer het type van de stack SAP NetWeaver
   1. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld RHEL 7
   1. Databasetype  
      HANA selecteren
   1. Telling van SAP-systeem  
      Het nummer van de SAP-systeem die in dit cluster worden uitgevoerd. Selecteer 1.
   1. Beschikbaarheid van het systeem  
      HA selecteren
   1. Admin Username, Administrator-wachtwoord of SSH-sleutel  
      Een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de machine.
   1. Subnet-ID  
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
1. Ten minste één gegevensschijf toevoegen aan beide virtuele machines  
   De gegevensschijven worden gebruikt voor demap/usr/sap/`<SAPSID`> directory
1. Maak een Load Balancer (intern)  
   1. De frontend-IP-adressen maken
      1. IP-adres 10.0.0.7 voor de ASCS
         1. De load balancer openen, front-end-IP-adresgroep selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe frontend-IP-adresgroep (bijvoorbeeld **nw1-ascs-frontend**)
         1. De toewijzing instellen op statisch en geef het IP-adres (bijvoorbeeld **10.0.0.7**)
         1. Klik op OK
      1. IP-adres 10.0.0.8 voor de ASCS ERS
         * Herhaal de stappen hierboven om te maken van een IP-adres voor de gebruikers (bijvoorbeeld **10.0.0.8** en **nw1-aers-back-end**)
   1. De back endadresgroepen maken
      1. Een back endadresgroep maken voor de ASCS
         1. Open de load balancer, back-endpools selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe back endpool (bijvoorbeeld **nw1-ascs-back-end**)
         1. Klik op een virtuele machine toevoegen.
         1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt
         1. Selecteer de virtuele machines van de (A) SCS-cluster
         1. Klik op OK
      1. Een back endpool maken voor de ASCS ERS
         * Herhaal de stappen hierboven om een back endadresgroep maken voor de gebruikers (bijvoorbeeld **nw1-aers-back-end**)
   1. De statuscontroles maken
      1. Poort 620**00** voor ASCS
         1. De load balancer openen, selecteer statuscontroles en klikt u op toevoegen
         1. Voer de naam van de nieuwe statustest (bijvoorbeeld **nw1-ascs-hp**)
         1. Selecteer TCP als protocol, poort 620**00**, Interval van 5 en de drempelwaarde voor onjuiste status 2
         1. Klik op OK
      1. Poort 621**02** voor ASCS INGEN
         * Herhaal de stappen hierboven om te maken van een statustest voor de gebruikers (bijvoorbeeld 621**02** en **nw1-aers-hp**)
   1. Regels van loadbalancing in Plbscheduler
      1. 32**00** TCP voor ASCS
         1. De load balancer openen, selecteert u load balancer-regels en klikt u op toevoegen
         1. Voer de naam van de nieuwe load balancer-regel (bijvoorbeeld **nw1-lb-3200**)
         1. Selecteer de front-end-IP-adres, de back-endpool en de statustest die u eerder hebt gemaakt (bijvoorbeeld **nw1-ascs-frontend**)
         1. Houd protocol **TCP**, voer poort **3200**
         1. Time-out voor inactiviteit tot 30 minuten verhogen
         1. **Zorg ervoor dat u zwevend IP inschakelen**
         1. Klik op OK
      1. Extra poorten voor de ASCS
         * Herhaal de stappen hierboven voor poorten 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 en TCP-voor de ASCS
      1. Extra poorten voor de ASCS ERS
         * Herhaal de stappen hierboven voor poorten 33**02**, 5**02**13, 5**02**14, 5**02**16 en TCP-voor de ASCS ERS

### <a name="create-pacemaker-cluster"></a>Pacemaker-cluster maken

Volg de stappen in [Pacemaker op Red Hat Enterprise Linux in Azure instellen](high-availability-guide-rhel-pacemaker.md) te maken van een eenvoudige Pacemaker-cluster voor dit (A) SCS-server.

### <a name="prepare-for-sap-netweaver-installation"></a>Voorbereiden voor de installatie van SAP NetWeaver

De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen

   <pre><code># IP addresses of the GlusterFS nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.11 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.12 nw1-aers</b>
   </code></pre>

1. **[A]**  Maken van de gedeelde mappen

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NW1</b>/SYS
   sudo mkdir -p /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo mkdir -p /usr/sap/<b>NW1</b>/ERS<b>02</b>

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NW1</b>/SYS
   sudo chattr +i /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo chattr +i /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>

1. **[A]**  Installeren GlusterFS client en andere vereisten

   <pre><code>sudo yum -y install glusterfs-fuse resource-agents resource-agents-sap
   </code></pre>

1. **[A]**  Versie van de resource-agents-sap

   Zorg ervoor dat de versie van het geïnstalleerde pakket van de resource-agents-sap ten minste 3.9.5-124.el7
   <pre><code>sudo yum info resource-agents-sap
   
   # Loaded plugins: langpacks, product-id, search-disabled-repos
   # Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
   # Installed Packages
   # Name        : resource-agents-sap
   # Arch        : x86_64
   # Version     : <b>3.9.5</b>
   # Release     : <b>124.el7</b>
   # Size        : 100 k
   # Repo        : installed
   # From repo   : rhel-sap-for-rhel-7-server-rpms
   # Summary     : SAP cluster resource agents and connector script
   # URL         : https://github.com/ClusterLabs/resource-agents
   # License     : GPLv2+
   # Description : The SAP resource agents and connector script interface with
   #          : Pacemaker to allow SAP instances to be managed in a cluster
   #          : environment.
   </code></pre>


1. **[A]**  Mount-items toevoegen

   <pre><code>sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   <b>glust-0</b>:/<b>NW1</b>-sapmnt /sapmnt/<b>NW1</b> glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-trans /usr/sap/trans glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-sys /usr/sap/<b>NW1</b>/SYS glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   </code></pre>

   Koppelen van de nieuwe shares

   <pre><code>sudo mount -a
   </code></pre>

1. **[A]**  Bestand WISSELEN configureren

   <pre><code>sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   De Agent voor het activeren van de wijziging opnieuw starten

   <pre><code>sudo service waagent restart
   </code></pre>

1. **[A]**  RHEL-configuratie

   RHEL configureren zoals beschreven in de SAP-notitie [2002167]

### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/INGEN installeren

1. **[1]**  Een virtueel IP-resource en een statustest voor de ASCS-exemplaar maken

   <pre><code>sudo pcs node standby <b>nw1-cl-1</b>
   
   sudo pcs resource create fs_<b>NW1</b>_ASCS Filesystem device='<b>glust-0</b>:/<b>NW1</b>-ascs' \
     directory='/usr/sap/<b>NW1</b>/ASCS<b>00</b>' fstype='glusterfs' \
     options='backup-volfile-servers=<b>glust-1:glust-2</b>' \
     --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create vip_<b>NW1</b>_ASCS IPaddr2 \
     ip=<b>10.0.0.11</b> cidr_netmask=<b>24</b> \
     --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create nc_<b>NW1</b>_ASCS azure-lb port=620<b>00</b> \
     --group g-<b>NW1</b>_ASCS
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo pcs status
   
   # Node <b>nw1-cl-1</b>: standby
   # Online: [ <b>nw1-cl-0</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-0</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-0</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-0</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASCS installeren  

   SAP NetWeaver ASCS installeren als hoofd op het eerste knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de ASCS, bijvoorbeeld <b>nw1 ascs</b>, <b>10.0.0.11</b> en de exemplaar van het nummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>00</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code># Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp
   
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Als de installatie mislukt een submap maken in/usr/sap/**NW1**/ASCS**00**, proberen in te stellen de eigenaar en de groep van de ASCS**00** map en probeer het opnieuw.

   <pre><code>sudo chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]**  Een virtueel IP-resource en een statustest voor de gebruikers-exemplaar maken

   <pre><code>sudo pcs node unstandby <b>nw1-cl-1</b>
   sudo pcs node standby <b>nw1-cl-0</b>
   
   sudo pcs resource create fs_<b>NW1</b>_AERS Filesystem device='<b>glust-0</b>:/<b>NW1</b>-aers' \
     directory='/usr/sap/<b>NW1</b>/ERS<b>02</b>' fstype='glusterfs' \
     options='backup-volfile-servers=<b>glust-1:glust-2</b>' \
    --group g-<b>NW1</b>_AERS
   
   sudo pcs resource create vip_<b>NW1</b>_AERS IPaddr2 \
     ip=<b>10.0.0.12</b> cidr_netmask=<b>24</b> \
    --group g-<b>NW1</b>_AERS
   
   sudo pcs resource create nc_<b>NW1</b>_AERS azure-lb port=621<b>02</b> \
    --group g-<b>NW1</b>_AERS
   </code></pre>
 
   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo pcs status
   
   # Node <b>nw1-cl-0</b>: standby
   # Online: [ <b>nw1-cl-1</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_AERS
   #      fs_<b>NW1</b>_AERS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_AERS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_AERS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver INGEN installeren  

   SAP NetWeaver INGEN installeren als de hoofdmap van het tweede knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de gebruikers, bijvoorbeeld <b>nw1 aers</b>, <b>10.0.0.12</b> en de exemplaar van het nummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>02</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code># Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp

   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Als de installatie mislukt een submap maken in/usr/sap/**NW1**/ERS**02**, proberen in te stellen de eigenaar en de groep van de INGEN**02** map en probeer het opnieuw.

   <pre><code>sudo chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   sudo chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>

1. **[1]**  Gebruikmaking van Adapt de ASCS/SCS en INGEN instantie-profielen

   * ASCS/SCS-profiel

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * Gebruikers-profiel

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # remove Autostart from ERS profile
   # Autostart = 1
   </code></pre>


1. **[A]**  Keep Alive configureren

   De communicatie tussen de SAP NetWeaver-toepassingsserver en in de ASCS/SCS wordt doorgestuurd via een software load balancer. De load balancer wordt niet-actieve verbindingen verbroken na een configureerbare time-out. Om dit te voorkomen, moet u een parameter in de SAP NetWeaver ASCS/SCS-profiel instellen en wijzig de instellingen van het Linux-systeem. Lezen [SAP-notitie 1410736] [ 1410736] voor meer informatie.

   De ASCS/SCS profiel parameter enque/encni/set_so_keepalive is al toegevoegd in de vorige stap.

   <pre><code># Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  Bijwerken van het bestand /usr/sap/sapservices

   Om te voorkomen dat het begin van de exemplaren van het opstartscript sapinit, moeten u alle exemplaren beheerd door Pacemaker commentaar uit /usr/sap/sapservices-bestand. Niet opmerkingen bij de SAP HANA-instantie als deze wordt gebruikt met HANA SR.

   <pre><code>
   sudo vi /usr/sap/sapservices
   
   # On the node where you installed the ASCS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/<b>NW1</b>/ASCS<b>00</b>/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/<b>NW1</b>/ASCS<b>00</b>/exe/sapstartsrv pf=/usr/sap/<b>NW1</b>/SYS/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> -D -u <b>nw1</b>adm
   
   # On the node where you installed the ERS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/<b>NW1</b>/ERS<b>02</b>/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/<b>NW1</b>/ERS<b>02</b>/exe/sapstartsrv pf=/usr/sap/<b>NW1</b>/ERS<b>02</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> -D -u <b>nw1</b>adm
   </code></pre>

1. **[1]**  De resources van de SAP-cluster maken

   <pre><code>sudo pcs property set maintenance-mode=true
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ASCS00 SAPInstance \
    InstanceName=<b>NW1</b>_ASCS00_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS00_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 \
    --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    InstanceName=<b>NW1</b>_ERS02_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS02_<b>nw1-aers</b>" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    --group g-<b>NW1</b>_AERS
      
   sudo pcs constraint colocation add g-<b>NW1</b>_AERS with g-<b>NW1</b>_ASCS -5000
   sudo pcs constraint location rsc_sap_<b>NW1</b>_ASCS<b>00</b> rule score=2000 runs_ers_<b>NW1</b> eq 1
   
   sudo pcs constraint order g-<b>NW1</b>_ASCS then g-<b>NW1</b>_AERS kind=Optional symmetrical=false
   
   sudo pcs node unstandby <b>nw1-cl-0</b>
   sudo pcs property set maintenance-mode=false
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo pcs status
   
   # Online: [ <b>nw1-cl-0</b> <b>nw1-cl-1</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-0</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   #      rsc_sap_<b>NW1</b>_ASCS00 (ocf::heartbeat:SAPInstance):   Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_AERS
   #      fs_<b>NW1</b>_AERS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-0</b>
   #      nc_<b>NW1</b>_AERS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-0</b>
   #      vip_<b>NW1</b>_AERS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-0</b>
   #      rsc_sap_<b>NW1</b>_ERS02  (ocf::heartbeat:SAPInstance):   Started <b>nw1-cl-0</b>
   </code></pre>

1. **[A]**  Firewallregels voor ASCS en gebruikers op beide knooppunten toevoegen

   <pre><code># Probe Port of ASCS
   sudo firewall-cmd --zone=public --add-port=620<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=620<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=32<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=32<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=36<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=36<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=39<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=39<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=81<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=81<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>13/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>13/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>14/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>14/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>16/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>16/tcp
   # Probe Port of ERS
   sudo firewall-cmd --zone=public --add-port=621<b>02</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=621<b>02</b>/tcp
   sudo firewall-cmd --zone=public --add-port=33<b>02</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=33<b>02</b>/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>13/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>13/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>14/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>14/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>16/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>16/tcp
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver-toepassing-server voorbereiden

Sommige databases is vereist dat de installatie van de database-exemplaar wordt uitgevoerd op een toepassingsserver bevinden. Bereid de toepassing virtuele machines van de server moet kunnen worden gebruikt in deze gevallen.

De onderstaande stappen wordt ervan uitgegaan dat u de toepassingsserver installeren op een server die andere in de ASCS/SCS en HANA-servers. Enkele van de onderstaande stappen (zoals het configureren van hostnamen) zijn anders niet nodig.

1. Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen

   <pre><code># IP addresses of the GlusterFS nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.11 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.12 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   # IP address of all application servers
   <b>10.0.0.8 nw1-di-0</b>
   <b>10.0.0.7 nw1-di-1</b>
   </code></pre>

1. Maken van de map sapmnt

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. GlusterFS client en andere vereisten installeren

   <pre><code>sudo yum -y install glusterfs-fuse uuidd
   </code></pre>

1. Koppelpunt vermeldingen toe te voegen

   <pre><code>sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   <b>glust-0</b>:/<b>NW1</b>-sapmnt /sapmnt/<b>NW1</b> glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-trans /usr/sap/trans glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   </code></pre>

   Koppelen van de nieuwe shares

   <pre><code>sudo mount -a
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

## <a name="install-database"></a>Installeren van de database

In dit voorbeeld is op SAP HANA SAP NetWeaver geïnstalleerd. U kunt elke ondersteunde database gebruiken voor deze installatie. Zie voor meer informatie over het installeren van SAP HANA in Azure [hoge beschikbaarheid van SAP HANA op Azure Virtual machines van Red Hat Enterprise Linux][sap-hana-ha]. Zie voor een lijst met ondersteunde databases, [SAP-notitie 1928533][1928533].

1. De installatie van de SAP-database-exemplaar uitvoeren

   Installeren van de database-instantie van SAP NetWeaver als hoofd-met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de database bijvoorbeeld <b>nw1-db</b> en <b>10.0.0.13</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver-toepassing-serverinstallatie

Volg deze stappen voor het installeren van een SAP-toepassingsserver.

1. Application server voorbereiden

   Volg de stappen in het hoofdstuk [SAP NetWeaver-toepassing-server voorbereiden](high-availability-guide-rhel.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) hierboven voor het voorbereiden van de toepassingsserver.

1. SAP NetWeaver-toepassingsserver installeren

   Een primaire of extra SAP NetWeaver-toepassingen-server installeren.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA beveiligd archief bijwerken

   Werk de SAP HANA secure store om te verwijzen naar de virtuele naam van de SAP HANA-Systeemreplicatie instellen.

   Voer de volgende opdracht om de vermeldingen als \<sapsid > adm

   <pre><code>hdbuserstore List
   </code></pre>

   Dit moet alle vermeldingen weergeven en moet er ongeveer als volgt
   <pre><code>
   DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: <b>NW1</b>
   </code></pre>

   De uitvoer ziet u dat het IP-adres van de standaardvermelding die naar de virtuele machine en niet naar het IP-adres van de load balancer verwijst. Dit item moet worden gewijzigd om te verwijzen naar de virtuele hostnaam van de load balancer. Zorg ervoor dat u dezelfde poort (**30313** in de bovenstaande uitvoer) en de databasenaam (**HN1** in de bovenstaande uitvoer).

   <pre><code>su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db</b>:<b>30313@NW1</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

1. Handmatig migreren van de ASCS-exemplaar

   De resourcestatus van de voordat u begint met de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Voer de volgende opdrachten als root voor het migreren van de ASCS-exemplaar.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource move rsc_sap_NW1_ASCS00
   
   [root@nw1-cl-0 ~]# pcs resource clear rsc_sap_NW1_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Knooppuntcrash simuleren

   De resourcestatus van de voordat u begint met de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Voer de volgende opdracht als hoofdmap op het knooppunt waarop de ASCS-exemplaar wordt uitgevoerd

   <pre><code>[root@nw1-cl-1 ~]# echo b > /proc/sysrq-trigger
   </code></pre>

   De status nadat het knooppunt wordt gestart opnieuw als volgt uitzien.

   <pre><code>Online: [ nw1-cl-0 nw1-cl-1 ]
   
   Full list of resources:
   
   rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   
   Failed Actions:
   * rsc_sap_NW1_ERS02_monitor_11000 on nw1-cl-0 'not running' (7): call=45, status=complete, exitreason='',
       last-rc-change='Tue Aug 21 13:52:39 2018', queued=0ms, exec=0ms
   </code></pre>

   Gebruik de volgende opdracht voor het opschonen van de mislukte resources.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Bericht serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Voer de volgende opdrachten als root om te bepalen van het proces van de berichtenserver en het kill.

   <pre><code>[root@nw1-cl-0 ~]# pgrep ms.sapNW1 | xargs kill -9
   </code></pre>

   Als u alleen kill de berichtenserver één keer, wordt deze opnieuw gestart door sapstart. Als u dit vaak die genoeg Pacemaker uiteindelijk wordt kill verplaatst de ASCS-exemplaar naar het andere knooppunt. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar ASCS en gebruikers na de test.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ASCS00
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. In de wachtrij plaatsen serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Voer de volgende opdrachten als root op het knooppunt waarop de ASCS-exemplaar afsluiten van de server in de wachtrij plaatsen wordt uitgevoerd.

   <pre><code>[root@nw1-cl-1 ~]# pgrep en.sapNW1 | xargs kill -9
   </code></pre>

   De ASCS-exemplaar moet onmiddellijk failover naar het andere knooppunt. Het exemplaar van de gebruikers ook een failover moet nadat de ASCS-instantie is gestart. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar ASCS en gebruikers na de test.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ASCS00
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. In de wachtrij plaatsen replicatie serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Voer de volgende opdracht als hoofdmap op het knooppunt waarop het exemplaar INGEN afsluiten van het proces in de wachtrij plaatsen replicatie server wordt uitgevoerd.

   <pre><code>[root@nw1-cl-1 ~]# pgrep er.sapNW1 | xargs kill -9
   </code></pre>

   Als u alleen de opdracht één keer uitvoeren, wordt het proces opnieuw opgestart sapstart. Als u deze uitvoeren vaak genoeg sapstart wordt niet opnieuw starten tijdens het proces en de resource is gestopt. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar uit nadat de test.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. In de wachtrij plaatsen sapstartsrv proces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Voer de volgende opdrachten als root op het knooppunt waarop de ASCS wordt uitgevoerd.

   <pre><code>[root@nw1-cl-0 ~]# pgrep -fl ASCS00.*sapstartsrv
   # 59545 sapstartsrv
   
   [root@nw1-cl-0 ~]# kill -9 59545
   </code></pre>

   Het proces sapstartsrv moet altijd opnieuw worden gestart door de agent Pacemaker resource als onderdeel van de bewaking. De resourcestatus van de nadat de test:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

## <a name="next-steps"></a>Volgende stappen

* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
