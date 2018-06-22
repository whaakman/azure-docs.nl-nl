---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver op SUSE Linux Enterprise Server voor SAP-toepassingen | Microsoft Docs
description: Hoge beschikbaarheid handleiding voor SAP NetWeaver op SUSE Linux Enterprise Server voor SAP-toepassingen
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: 9d6c56f96c085de60b7cc05e4cc16b57867f6a7d
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308357"
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

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md

In dit artikel wordt beschreven hoe de virtuele machines te implementeren, configureren van de virtuele machines, installeer het framework van het cluster en een maximaal beschikbare SAP NetWeaver 7,50 systeem installeren.
In de voorbeeldconfiguraties installatieopdrachten enzovoort. ASC's exemplaarnummer 00 Ebruikers instantie nummer 02 en SAP systeem-ID NW1 wordt gebruikt. De namen van de resources (bijvoorbeeld virtuele machines, virtuele netwerken) in het voorbeeld wordt ervan uitgegaan dat u hebt gebruikt de [geconvergeerde sjabloon] [ template-converged] met SAP-systeem-ID NW1 om de resources te maken.

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
* [SUSE SAP HA Best Practice handleidingen] [ suse-ha-guide] de handleidingen bevat alle benodigde informatie voor het instellen van Netweaver HA en SAP HANA System Replication on-premises. Gebruik die dit als een algemene basislijn leidt. Ze bieden veel meer gedetailleerde informatie.
* [Maximaal beschikbare NFS opslag met DRBD en pacemaker heeft] [ suse-drbd-guide] de handleiding bevat alle vereiste informatie voor het instellen van een maximaal beschikbare NFS-server. Deze handleiding gebruiken als basislijn.


## <a name="overview"></a>Overzicht

SAP NetWeaver vereist om te zorgen voor hoge beschikbaarheid, een NFS-server. De NFS-server is geconfigureerd in een cluster met afzonderlijke en kan worden gebruikt door meerdere SAP-systemen.

![SAP NetWeaver hoge beschikbaarheid-overzicht](./media/high-availability-guide-suse/img_001.png)

De NFS-server, SAP NetWeaver ASC's SAP NetWeaver SCS, SAP NetWeaver Ebruikers en de SAP HANA-database gebruiken virtuele hostnaam en virtuele IP-adressen. In Azure, is een load balancer vereist voor het gebruik van een virtueel IP-adres. De volgende lijst ziet u de configuratie van de (A) SCS en uit de load balancer.

### <a name="ascs"></a>(A)SCS

* Frontend configuratie
  * IP-adres 10.0.0.7
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken moet) SCS Ebruikers/cluster
* De Testpoort
  * Poort 620**&lt;nr&gt;**
* Loadbalancing regels
  * 32**&lt;nr&gt;** TCP
  * 36**&lt;nr&gt;** TCP
  * 39**&lt;nr&gt;** TCP
  * 81**&lt;nr&gt;** TCP
  * 5**&lt;nr&gt;** 13 TCP
  * 5**&lt;nr&gt;** 14 TCP
  * 5**&lt;nr&gt;** 16 TCP

### <a name="ers"></a>EBRUIKERS

* Frontend configuratie
  * IP-adres 10.0.0.8
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken moet) SCS Ebruikers/cluster
* De Testpoort
  * Poort 621**&lt;nr&gt;**
* Loadbalancing regels
  * 33**&lt;nr&gt;** TCP
  * 5**&lt;nr&gt;** 13 TCP
  * 5**&lt;nr&gt;** 14 TCP
  * 5**&lt;nr&gt;** 16 TCP

## <a name="setting-up-a-highly-available-nfs-server"></a>Een maximaal beschikbare NFS-server instellen

SAP NetWeaver vereist een gedeelde opslag voor de map transport en het profiel. Lees [hoge beschikbaarheid voor NFS op Azure VM's op SUSE Linux Enterprise Server] [ nfs-ha] over het instellen van een NFS-server voor SAP NetWeaver.

## <a name="setting-up-ascs"></a>(A) SCS instellen

U kunt een Azure-sjabloon vanuit github voor installatie zonder toezicht gebruiken voor het implementeren van alle vereiste Azure-resources, met inbegrip van de virtuele machines, beschikbaarheid instellen en de load balancer of u kunt de bronnen handmatig implementeren.

### <a name="deploy-linux-via-azure-template"></a>Linux via Azure-sjabloon implementeren

Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor SAP-toepassingen 12 die u gebruiken kunt om nieuwe virtuele machines te implementeren. De marketplace-installatiekopie bevat de resource-agent voor SAP NetWeaver.

U kunt een van de Quick Start-sjablonen op github gebruiken voor het implementeren van alle vereiste resources. De sjabloon wordt geïmplementeerd voor de virtuele machines, de load balancer, beschikbaarheidsset enzovoort. Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [ASC's / SCS Multi SID sjabloon] [ template-multisid-xscs] of de [geconvergeerde sjabloon] [ template-converged] op de Azure portal de ASC's / SCS sjabloon alleen maakt u de regels voor taakverdeling voor de SAP NetWeaver ASC's / SCS en exemplaren Ebruikers (alleen voor Linux) terwijl de geconvergeerde sjabloon ook de regels voor taakverdeling voor een database (bijvoorbeeld Microsoft SQL Server of SAP HANA maakt). Als u van plan bent een SAP NetWeaver gebaseerd systeem installeren en u ook wilt voor de installatie van de database op de dezelfde machines, gebruikt u de [geconvergeerde sjabloon][template-converged].
1. Voer de volgende parameters
   1. Resource-voorvoegsel (alleen ASC's / SCS Multi SID-sjabloon)  
      Geef het voorvoegsel op dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   3. SAP systeem-ID (alleen geconvergeerde sjabloon)  
      Voer de SAP-systeem-ID van het SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   4. Stack-Type  
      Selecteer het type van de stack SAP NetWeaver
   5. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld SLES 12 BYOS
   6. DB-Type  
      Selecteer HANA
   7. Grootte van het SAP  
      De hoeveelheid SAP's het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraagt u uw SAP-technologie Partner of System Integrator
   8. Beschikbaarheid van het systeem  
      Selecteer HA
   9. Gebruikersnaam van de beheerder en het wachtwoord van beheerder  
      Een nieuwe gebruiker wordt gemaakt die kunnen worden gebruikt voor aanmelding bij de computer.
   10. Subnet-ID  
   De ID van het subnet waarmee de virtuele machines moet worden verbonden.  Laat leeg als u wilt maken van een nieuw virtueel netwerk of Selecteer hetzelfde subnet die u gebruikt of gemaakt als onderdeel van de implementatie van de NFS-server. De ID meestal ziet eruit als /subscriptions/**&lt;abonnements-ID&gt;**/resourceGroups/**&lt;Resourcegroepnaam&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuele-netwerknaam&gt;**/subnets/**&lt;subnetnaam&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Het handmatig implementeren van Linux via Azure portal

U moet eerst de virtuele machines voor dit cluster NFS maken. Daarna maken van een load balancer en het gebruik van de virtuele machines in de back-end-adresgroepen.

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Een Beschikbaarheidsset maken  
   De maximale updatedomein instellen
1. Virtuele Machine 1 maken   
   Ten minste SLES4SAP 12 SP1, in dit voorbeeld de SLES4SAP 12 SP1 image https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES voor SAP-toepassingen 12 SP1 wordt gebruikt  
   Selecteer Beschikbaarheidsset eerder hebt gemaakt  
1. Virtuele Machine 2 maken   
   Ten minste SLES4SAP 12 SP1, in dit voorbeeld de SLES4SAP 12 SP1 image https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES voor SAP-toepassingen 12 SP1 wordt gebruikt  
   Selecteer Beschikbaarheidsset eerder hebt gemaakt  
1. Ten minste één gegevensschijf toevoegen aan beide virtuele machines  
   De gegevensschijven worden gebruikt voor de/usr/sap/`<SAPSID`> directory
1. Maak een Load Balancer (intern)  
   1. De frontend-IP-adressen maken
      1. IP-adres 10.0.0.7 voor de ASC 's
         1. Open de load balancer, selecteer frontend-IP-adresgroep en klik op toevoegen
         1. Voer de naam van de nieuwe frontend-IP-adresgroep (bijvoorbeeld **nw1-ASC's-frontend**)
         1. De toewijzing instellen op statisch en voer het IP-adres (bijvoorbeeld **10.0.0.7**)
         1. Klik op OK
      1. IP-adres 10.0.0.8 voor de ASCS ERS
         * Herhaal de stappen hierboven om te maken van een IP-adres voor de gebruikers (bijvoorbeeld **10.0.0.8** en **nw1-aers-back-end**)
   1. De back-end-adresgroepen maken
      1. Een back-endpool maken voor de ASC 's
         1. Open de load balancer, back-endpools selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe back-endpool (bijvoorbeeld **nw1-ASC's-back-end**)
         1. Klik op een virtuele machine toevoegen.
         1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt
         1. Selecteer de virtuele machines van de (A) SCS cluster
         1. Klik op OK
      1. Een back-endpool maken voor de ASCS ERS
         * Herhaal de stappen hierboven om een back-endpool maken voor de gebruikers (bijvoorbeeld **nw1-aers-back-end**)
   1. De statuscontroles maken
      1. Poort 620**00** voor ASC's
         1. Openen van de load balancer, statuscontroles selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe health test (bijvoorbeeld **nw1-ASC's-hp**)
         1. Selecteer TCP als protocol, poort 620**00**, houd Interval 5 en de drempelwaarde voor onjuiste status 2
         1. Klik op OK
      1. Poort 621**02** voor Ebruikers ASC's
         * Herhaal de stappen hierboven om te maken van een health test voor de gebruikers (bijvoorbeeld 621**02** en **nw1-aers-hp**)
   1. Loadbalancing regels
      1. 32**00** TCP voor ASC's
         1. Open de load balancer en taakverdelingsregels Selecteer klikt u op toevoegen
         1. Voer de naam van de load balancer-regel (bijvoorbeeld **nw1-lb-3200**)
         1. Selecteer de frontend-IP-adres, back-endpool en health test die u eerder hebt gemaakt (bijvoorbeeld **nw1-ASC's-frontend**)
         1. Houd protocol **TCP**, voer poort **3200**
         1. Verhoog de time-out voor inactiviteit tot 30 minuten
         1. **Zorg ervoor dat u kunt zwevend IP inschakelen**
         1. Klik op OK    
      1. Extra poorten voor de ASC 's
         * Herhaal de stappen hierboven voor poorten 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 en TCP-voor de ASC's
      1. Extra poorten voor de ASCS ERS
         * Herhaal de stappen hierboven voor poorten 33**02**, 5**02**13, 5**02**14, 5**02**16 en TCP-voor de ASCS ERS

### <a name="create-pacemaker-cluster"></a>Pacemaker heeft cluster maken

Volg de stappen in [pacemaker heeft op SUSE Linux Enterprise Server in Azure instellen](high-availability-guide-suse-pacemaker.md) voor het maken van een eenvoudige pacemaker heeft voor deze (A) SCS server cluster.

### <a name="installation"></a>Installatie

De volgende items worden voorafgegaan door een **[A]** - van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  SUSE Connector installeren
   
   <pre><code>
   sudo zypper install sap_suse_cluster_connector
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
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver installatie voorbereiden

1. **[A]**  Maken van de gedeelde mappen

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NW1</b>
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
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   /usr/sap/<b>NW1</b>/SYS -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sidsys
   /usr/sap/<b>NW1</b>/ASCS<b>00</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/ASCS
   /usr/sap/<b>NW1</b>/ERS<b>02</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/ASCSERS
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

1. **[1]**  Een virtueel IP-resource en de statuscontrole voor het exemplaar ASC's maken

   <pre><code>
   sudo crm node standby <b>nw1-cl-1</b>
   
   sudo crm configure primitive vip_<b>NW1</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_ASCS nc_<b>NW1</b>_ASCS vip_<b>NW1</b>_ASCS \
      meta resource-stickiness=3000
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r
   
   # Node nw1-cl-1: standby
   # <b>Online: [ nw1-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-0</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-0</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASC's installeren  

   SAP NetWeaver ASC's installeren als hoofdmap op het eerste knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer frontend-configuratie voor de ASC's, bijvoorbeeld <b>nw1-ASC's</b>, <b>10.0.0.7</b> en de exemplaar nummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>00</b>.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Als de installatie mislukt het maken van een submap in /usr/sap/**NW1**/ASCS**00**, proberen in te stellen van de eigenaar en de groep van de ASC's**00** map en probeer het opnieuw.

   <pre><code>
   chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]**  Een virtueel IP-resource en voor het exemplaar Ebruikers health-test maken

   <pre><code>
   sudo crm node online <b>nw1-cl-1</b>
   sudo crm node standby <b>nw1-cl-0</b>
   
   sudo crm configure primitive vip_<b>NW1</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0
   
   # WARNING: Resources nc_NW1_ASCS,nc_NW1_ERS violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y
   
   sudo crm configure group g-<b>NW1</b>_ERS nc_<b>NW1</b>_ERS vip_<b>NW1</b>_ERS
   </code></pre>
 
   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r
   
   # Node <b>nw1-cl-0: standby</b>
   # <b>Online: [ nw1-cl-1 ]</b>
   # 
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver Ebruikers installeren  

   SAP NetWeaver Ebruikers installeren als de hoofdmap van het tweede knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer frontend-configuratie voor de gebruikers bijvoorbeeld <b>nw1 aers</b>, <b>10.0.0.8</b> en de exemplaar nummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>02</b>.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > Gebruik SWPM SP 20 PL 05 of hoger. Lagere versies Stel de machtigingen niet correct en mislukt de installatie.

   Als de installatie mislukt het maken van een submap in /usr/sap/**NW1**/ERS**02**, proberen in te stellen van de eigenaar en de groep van de Ebruikers**02** map en probeer het opnieuw.

   <pre><code>
   chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>
   > 

1. **[1]**  Adapt de ASC's / SCS en Ebruikers exemplaar profielen
 
   * ASC's / SCS-profiel

   <pre><code> 
   sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
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
   sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
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
   sudo usermod -aG haclient <b>nw1</b>adm   
   </code></pre>

1. **[1]**  De ASC's en Ebruikers SAP-services aan het bestand sapservice toevoegen

   De ASC's service-vermelding voor het tweede knooppunt en de vermelding van de service Ebruikers kopiëren naar het eerste knooppunt toevoegen.

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nw1-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nw1-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  De SAP-clusterbronnen maken

   <pre><code>
   sudo crm configure property maintenance-mode="true"   
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ASCS<b>00</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000
   
   sudo crm configure modgroup g-<b>NW1</b>_ASCS add rsc_sap_<b>NW1</b>_ASCS<b>00</b>
   sudo crm configure modgroup g-<b>NW1</b>_ERS add rsc_sap_<b>NW1</b>_ERS<b>02</b>
   
   sudo crm configure colocation col_sap_<b>NW1</b>_no_both -5000: g-<b>NW1</b>_ERS g-<b>NW1</b>_ASCS
   sudo crm configure location loc_sap_<b>NW1</b>_failover_to_ers rsc_sap_<b>NW1</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NW1</b> eq 1
   sudo crm configure order ord_sap_<b>NW1</b>_first_start_ascs Optional: rsc_sap_<b>NW1</b>_ASCS<b>00</b>:start rsc_sap_<b>NW1</b>_ERS<b>02</b>:stop symmetrical=false
   
   sudo crm node online <b>nw1-cl-0</b>
   sudo crm configure property maintenance-mode="false"
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r
   
   # Online: <b>[ nw1-cl-0 nw1-cl-1 ]</b>
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   # rsc_st_azure    (stonith:fence_azure_arm):      <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #      rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   #      rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-0</b>
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver application server voorbereiden

Sommige databases is vereist dat de installatie van de database-exemplaar wordt uitgevoerd op een toepassingsserver. Bereid de toepassing virtuele machines van de server te kunnen worden gebruikt in deze gevallen zijn.

De onderstaande stappen wordt ervan uitgegaan dat u de toepassingsserver installeren op een server die verschilt van de ASC's / SCS en HANA-servers. Anders zijn enkele van de onderstaande stappen (zoals het configureren van hostnamen) niet nodig.

1. Setup van hostnaamomzetting

   U kunt gebruik van een DNS-server of wijzig de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe het bestand/etc/hosts te gebruiken.
   Het IP-adres en de hostnaam in de volgende opdrachten vervangen
   ```bash
   sudo vi /etc/hosts
   ```
   Voeg de volgende regels/etc/hosts. Wijzig de IP-adres en hostnaam overeenkomen met uw omgeving    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   # IP address of all application servers
   <b>10.0.0.20 nw1-di-0</b>
   <b>10.0.0.21 nw1-di-1</b>
   </code></pre>

1. Maken van de map sapmnt

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
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
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
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

## <a name="install-database"></a>Installeren van de database

In dit voorbeeld is op SAP HANA SAP NetWeaver geïnstalleerd. Voor deze installatie kunt u elke ondersteunde database. Zie voor meer informatie over het installeren van SAP HANA in Azure [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]. Zie voor een lijst met ondersteunde databases, [SAP-notitie 1928533][1928533].

1. Voer de installatie van de SAP database-exemplaar

   Het exemplaar van de database SAP NetWeaver installeren als hoofdmap met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer frontend-configuratie voor de database bijvoorbeeld <b>nw1 db</b> en <b>10.0.0.13</b>.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver application server-installatie

Volg deze stappen voor het installeren van een SAP-toepassingsserver. 

1. Voorbereiden van de toepassingsserver

Volg de stappen in het hoofdstuk [SAP NetWeaver application server voorbereiding](high-availability-guide-suse.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) hierboven voor het voorbereiden van de toepassingsserver.

1. SAP NetWeaver application server installeren

   Een primaire of extra SAP NetWeaver toepassingsserver installeren.

   De parameter sapinst SAPINST_REMOTE_ACCESS_USER kunt u een niet-hoofdgebruiker verbinding maken met sapinst toestaan.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA secure store bijwerken

   Werk de SAP HANA secure store om te verwijzen naar de virtuele naam van de SAP HANA System replicatie-instellingen.

   Voer de volgende opdracht voor het weergeven van de vermeldingen
   <pre><code>
   hdbuserstore List
   </code></pre>

   Dit alle vermeldingen moet worden vermeld en moet er ongeveer uitzien
   <pre><code>
   DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: HN1
   </code></pre>

   De uitvoer ziet u dat het IP-adres van de standaardvermelding die naar de virtuele machine en niet naar de load balancer IP-adres verwijst. Dit item moet worden gewijzigd om te verwijzen naar de virtuele-hostnaam van de load balancer. Zorg ervoor dat u dezelfde poort gebruiken (**30313** in bovenstaande uitvoer)!

   <pre><code>
   su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db</b>:<b>30313</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a>Volgende stappen
* [Azure virtuele Machines, planning en implementatie voor SAP][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
