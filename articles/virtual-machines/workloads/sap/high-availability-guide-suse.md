---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver in SUSE Linux Enterprise Server voor SAP-toepassingen | Microsoft Docs
description: Hoge beschikbaarheid handleiding voor SAP NetWeaver in SUSE Linux Enterprise Server voor SAP-toepassingen
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
ms.date: 08/16/2018
ms.author: sedusch
ms.openlocfilehash: f65a6a0f9564eafda36b8a8f4988e064e39a3bb1
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430604"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen

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
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md

In dit artikel wordt beschreven hoe u de virtuele machines implementeren, configureren van de virtuele machines, installeer het framework van het cluster en installeren van een maximaal beschikbare 7,50 van SAP NetWeaver-systeem.
In de voorbeeldconfiguraties, enzovoort-opdrachten voor installatie. ASCS exemplaarnummer 00 INGEN exemplaarnummer 02 en SAP-systeem-ID NW1 wordt gebruikt. De namen van de resources (bijvoorbeeld virtuele machines, virtuele netwerken) in het voorbeeld wordt ervan uitgegaan dat u hebt gebruikt de [geconvergeerd sjabloon] [ template-converged] met SAP-systeem-ID NW1 om de resources te maken.

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
* [Azure Virtual Machines-implementatie voor SAP op Linux][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [SUSE SAP HA gidsen met aanbevolen procedures] [ suse-ha-guide] de handleidingen bevatten alle benodigde informatie voor het instellen van Netweaver HA en SAP HANA-Systeemreplicatie on-premises. Gebruik deze handleidingen als een algemene basislijn. Ze bieden nog veel meer gedetailleerde informatie.
* [Releaseopmerkingen voor SUSE hoge beschikbaarheid extensie 12 SP3][suse-ha-12sp3-relnotes]

## <a name="overview"></a>Overzicht

SAP NetWeaver vereist voor het bereiken van hoge beschikbaarheid, een NFS-server. De NFS-server is geconfigureerd in een cluster dat gescheiden en kan worden gebruikt door meerdere SAP-systemen.

![Overzicht van SAP NetWeaver hoge beschikbaarheid](./media/high-availability-guide-suse/ha-suse.png)

De NFS-server, SAP NetWeaver ASCS, SAP NetWeaver SCS, INGEN voor SAP NetWeaver en SAP HANA-database gebruiken virtuele hostnaam en virtuele IP-adressen. In Azure, wordt een load balancer overstappen naar een virtueel IP-adres. De volgende lijst bevat de configuratie van de (A) SCS en INGEN load balancer.

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

## <a name="setting-up-a-highly-available-nfs-server"></a>Instellen van een maximaal beschikbare NFS-server

SAP NetWeaver vereist gedeelde opslag voor de map transport en profiel. Lezen [hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server] [ nfs-ha] over het instellen van een NFS-server voor SAP NetWeaver.

## <a name="setting-up-ascs"></a>(A) SCS instellen

U kunt een Azure-sjabloon gebruiken voor het implementeren van alle vereiste Azure-resources, met inbegrip van de virtuele machines, beschikbaarheid en de load balancer of kunt u de resources handmatig implementeren.

### <a name="deploy-linux-via-azure-template"></a>Linux via Azure-sjabloon implementeren

De Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor 12 van de SAP-toepassingen die u gebruiken kunt om nieuwe virtuele machines te implementeren. De marketplace-installatiekopie bevat de resource-agent voor SAP NetWeaver.

U kunt een van de snelstartsjablonen van op GitHub gebruiken om alle vereiste resources te implementeren. De sjabloon implementeert de virtuele machines, load balancer, beschikbaarheidsset enzovoort. Volg deze stappen om de sjabloon te implementeren:

1. Open de [ASCS/SCS Multi-SID sjabloon] [ template-multisid-xscs] of de [geconvergeerd sjabloon] [ template-converged] in de ASCS/SCS-sjabloon maakt u alleen Azure portal de regels voor taakverdeling voor de SAP NetWeaver ASCS/SCS en INGEN exemplaren (alleen Linux) terwijl de geconvergeerde sjabloon ook de taakverdelingsregels voor een database (bijvoorbeeld Microsoft SQL Server of SAP HANA maakt). Als u van plan bent voor het installeren van een SAP NetWeaver op basis van systeem en u ook wilt het installeren van de database op de dezelfde machines gebruikt de [geconvergeerd sjabloon][template-converged].
1. Voer de volgende parameters
   1. Resource-voorvoegsel (alleen in de ASCS/SCS Multi-SID sjabloon)  
      Geef het voorvoegsel dat u wilt gebruiken. De waarde wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   3. ID van de SAP-systeem (alleen geconvergeerde sjabloon)  
      Voer de SAP-systeem-ID van de SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
   4. Stack-Type  
      Selecteer het type van de stack SAP NetWeaver
   5. Type besturingssysteem  
      Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld SLES 12 BYOS
   6. Databasetype  
      HANA selecteren
   7. Grootte van de SAP-systeem  
      De hoeveelheid van het nieuwe systeem biedt SAP's. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologiepartner of het systeemintegrator
   8. Beschikbaarheid van het systeem  
      HA selecteren
   9. Gebruikersnaam voor de beheerder en het wachtwoord van beheerder  
      Een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de machine.
   10. Subnet-ID  
   Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID is meestal ziet eruit als /subscriptions/**&lt;abonnements-ID&gt;**/resourceGroups/**&lt;groepsnaam voor accountresources&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuele-netwerknaam&gt;**/subnets/**&lt;subnetnaam&gt;**

### <a name="deploy-linux-manually-via-azure-portal"></a>Handmatig implementeren van Linux via Azure portal

U moet eerst de virtuele machines voor dit cluster NFS maken. Daarna wordt u een load balancer maken en gebruiken van de virtuele machines in de back endadresgroepen.

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Een Beschikbaarheidsset maken  
   De maximale updatedomein instellen
1. Virtuele Machine 1 maken  
   Ten minste SLES4SAP 12 SP1, in dit de SLES4SAP 12 SP1-voorbeeldafbeelding https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES voor SAP-toepassingen 12 SP1 wordt gebruikt  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt  
1. Virtuele Machine 2 maken  
   Ten minste SLES4SAP 12 SP1, in dit de SLES4SAP 12 SP1-voorbeeldafbeelding https://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SLES voor SAP-toepassingen 12 SP1 wordt gebruikt  
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

Volg de stappen in [Pacemaker op SUSE Linux Enterprise Server in Azure instellen](high-availability-guide-suse-pacemaker.md) te maken van een eenvoudige Pacemaker-cluster voor dit (A) SCS-server.

### <a name="installation"></a>Installatie

De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]**  SUSE-Connector installeren

   <pre><code>sudo zypper install sap-suse-cluster-connector
   </code></pre>

   > [!NOTE]
   > Gebruik geen streepjes in de hostnamen van de clusterknooppunten. Anders werkt uw cluster niet. Dit is een bekende beperking en SUSE werkt aan een oplossing. De oplossing wordt gepubliceerd als een patch van het pakket sap-suse-cloud-connector.

   Zorg ervoor dat u de nieuwe versie van de connector SAP SUSE-cluster geïnstalleerd. De oude heeft sap_suse_cluster_connector is aangeroepen en het nieuwe knooppunt heet **sap-suse-cluster-connector**.

   <pre><code>sudo zypper info sap-suse-cluster-connector
   
   Information for package sap-suse-cluster-connector:
   ---------------------------------------------------
   Repository     : SLE-12-SP3-SAP-Updates
   Name           : sap-suse-cluster-connector
   <b>Version        : 3.0.0-2.2</b>
   Arch           : noarch
   Vendor         : SUSE LLC <https://www.suse.com/>
   Support Level  : Level 3
   Installed Size : 41.6 KiB
   <b>Installed      : Yes</b>
   Status         : up-to-date
   Source package : sap-suse-cluster-connector-3.0.0-2.2.src
   Summary        : SUSE High Availability Setup for SAP Products
   </code></pre>

1. **[A]**  Update SAP resource agents  
   
   Een patch voor de resource-agents-pakket is vereist voor het gebruik van de nieuwe configuratie, die in dit artikel wordt beschreven. U kunt controleren, als de patch al is geïnstalleerd met de volgende opdracht

   <pre><code>sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   De uitvoer moet er ongeveer als

   <pre><code>&lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   Als de parameter IS_ERS niet wordt gevonden in de opdracht grep, moet u installeert de patch vermeld op [de SUSE-downloadpagina](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)

   <pre><code># example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen   

   <pre><code># IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>Voorbereiden voor de installatie van SAP NetWeaver

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

1. **[A]**  Autofs configureren

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Maak een bestand met

   <pre><code>sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   /usr/sap/<b>NW1</b>/SYS -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sidsys
   </code></pre>

   Opnieuw opstarten autofs voor het koppelen van de nieuwe shares

   <pre><code>sudo systemctl enable autofs
   sudo service autofs restart
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


### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/INGEN installeren

1. **[1]**  Een virtueel IP-resource en een statustest voor de ASCS-exemplaar maken

   <pre><code>sudo crm node standby <b>nw1-cl-1</b>
   
   sudo crm configure primitive fs_<b>NW1</b>_ASCS Filesystem device='<b>nw1-nfs</b>:/<b>NW1</b>/ASCS' directory='/usr/sap/<b>NW1</b>/ASCS<b>00</b>' fstype='nfs4' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>NW1</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>NW1</b>_ASCS fs_<b>NW1</b>_ASCS nc_<b>NW1</b>_ASCS vip_<b>NW1</b>_ASCS \
      meta resource-stickiness=3000
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo crm_mon -r
   
   # Node nw1-cl-1: standby
   # <b>Online: [ nw1-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-0</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-0</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASCS installeren  

   SAP NetWeaver ASCS installeren als hoofd op het eerste knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de ASCS, bijvoorbeeld <b>nw1 ascs</b>, <b>10.0.0.7</b> en de exemplaar van het nummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>00</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Als de installatie mislukt een submap maken in/usr/sap/**NW1**/ASCS**00**, proberen in te stellen de eigenaar en de groep van de ASCS**00** map en probeer het opnieuw.

   <pre><code>chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]**  Een virtueel IP-resource en een statustest voor de gebruikers-exemplaar maken

   <pre><code>sudo crm node online <b>nw1-cl-1</b>
   sudo crm node standby <b>nw1-cl-0</b>
   
   sudo crm configure primitive fs_<b>NW1</b>_ERS Filesystem device='<b>nw1-nfs</b>:/<b>NW1</b>/ASCSERS' directory='/usr/sap/<b>NW1</b>/ERS<b>02</b>' fstype='nfs4' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>NW1</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0
   
   # WARNING: Resources nc_NW1_ASCS,nc_NW1_ERS violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y
   
   sudo crm configure group g-<b>NW1</b>_ERS fs_<b>NW1</b>_ERS nc_<b>NW1</b>_ERS vip_<b>NW1</b>_ERS
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo crm_mon -r
   
   # Node <b>nw1-cl-0: standby</b>
   # <b>Online: [ nw1-cl-1 ]</b>
   # 
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      fs_NW1_ERS (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver INGEN installeren

   SAP NetWeaver INGEN installeren als de hoofdmap van het tweede knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de gebruikers, bijvoorbeeld <b>nw1 aers</b>, <b>10.0.0.8</b> en de exemplaar van het nummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>02</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > Gebruik SWPM SP 20 PL 05 of hoger. Lagere versies kunnen de machtigingen niet correct ingesteld en de installatie mislukken.

   Als de installatie mislukt een submap maken in/usr/sap/**NW1**/ERS**02**, proberen in te stellen de eigenaar en de groep van de INGEN**02** map en probeer het opnieuw.

   <pre><code>chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>


1. **[1]**  Gebruikmaking van Adapt de ASCS/SCS en INGEN instantie-profielen
 
   * ASCS/SCS-profiel

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * Gebruikers-profiel

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # remove Autostart from ERS profile
   # Autostart = 1
   </code></pre>

1. **[A]**  Keep Alive configureren

   De communicatie tussen de SAP NetWeaver-toepassingsserver en in de ASCS/SCS wordt doorgestuurd via een software load balancer. De load balancer wordt niet-actieve verbindingen verbroken na een configureerbare time-out. Om dit te voorkomen, moet u een parameter in de SAP NetWeaver ASCS/SCS-profiel instellen en wijzig de instellingen van het Linux-systeem. Lezen [SAP-notitie 1410736] [ 1410736] voor meer informatie.

   De ASCS/SCS profiel parameter enque/encni/set_so_keepalive is al toegevoegd in de vorige stap.

   <pre><code># Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  De SAP-gebruikers configureren na de installatie

   <pre><code># Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nw1</b>adm
   </code></pre>

1. **[1]**  De ASCS en INGEN SAP-services toevoegen aan het bestand sapservice

   De ASCS vermelding voor het tweede knooppunt-service en de vermelding van de service INGEN kopiëren naar het eerste knooppunt toevoegen.

   <pre><code>cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nw1-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nw1-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  De resources van de SAP-cluster maken

   <pre><code>sudo crm configure property maintenance-mode="true"
   
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

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo crm_mon -r
   
   # Online: <b>[ nw1-cl-0 nw1-cl-1 ]</b>
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:anything):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #      rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      fs_NW1_ERS (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-0</b>
   #      nc_NW1_ERS (ocf::heartbeat:anything):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   #      rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-0</b>
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver-toepassing-server voorbereiden

Sommige databases is vereist dat de installatie van de database-exemplaar wordt uitgevoerd op een toepassingsserver bevinden. Bereid de toepassing virtuele machines van de server moet kunnen worden gebruikt in deze gevallen.

De onderstaande stappen wordt ervan uitgegaan dat u de toepassingsserver installeren op een server die andere in de ASCS/SCS en HANA-servers. Enkele van de onderstaande stappen (zoals het configureren van hostnamen) zijn anders niet nodig.

1. Configureren van besturingssysteem

   Reduceer de grootte van de vervuilde cache. Zie voor meer informatie, [laag schrijfvaardigheden op SLES 11/12 servers met grote RAM](https://www.suse.com/support/kb/doc/?id=7010287).

   <pre><code>sudo vi /etc/sysctl.conf

   # Change/set the following settings
   vm.dirty_bytes = 629145600
   vm.dirty_background_bytes = 314572800
   </code></pre>

1. Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   ```bash
   sudo vi /etc/hosts
   ```

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen

   <pre><code># IP address of the load balancer frontend configuration for NFS
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

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. Autofs configureren

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Maak een nieuw bestand met

   <pre><code>sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   </code></pre>

   Opnieuw opstarten autofs voor het koppelen van de nieuwe shares

   <pre><code>sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. Wisselbestand configureren

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

## <a name="install-database"></a>Installeren van de database

In dit voorbeeld is op SAP HANA SAP NetWeaver geïnstalleerd. U kunt elke ondersteunde database gebruiken voor deze installatie. Zie voor meer informatie over het installeren van SAP HANA in Azure [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]. Zie voor een lijst met ondersteunde databases, [SAP-notitie 1928533][1928533].

1. De installatie van de SAP-database-exemplaar uitvoeren

   Installeren van de database-instantie van SAP NetWeaver als hoofd-met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de database bijvoorbeeld <b>nw1-db</b> en <b>10.0.0.13</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver-toepassing-serverinstallatie

Volg deze stappen voor het installeren van een SAP-toepassingsserver.

1. Application server voorbereiden

Volg de stappen in het hoofdstuk [SAP NetWeaver-toepassing-server voorbereiden](high-availability-guide-suse.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) hierboven voor het voorbereiden van de toepassingsserver.

1. SAP NetWeaver-toepassingsserver installeren

   Een primaire of extra SAP NetWeaver-toepassingen-server installeren.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA beveiligd archief bijwerken

   Werk de SAP HANA secure store om te verwijzen naar de virtuele naam van de SAP HANA-Systeemreplicatie instellen.

   Voer de volgende opdracht om de vermeldingen weer te geven
   <pre><code>hdbuserstore List
   </code></pre>

   Dit moet alle vermeldingen weergeven en moet er ongeveer als volgt
   <pre><code>DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: <b>HN1</b>
   </code></pre>

   De uitvoer ziet u dat het IP-adres van de standaardvermelding die naar de virtuele machine en niet naar het IP-adres van de load balancer verwijst. Dit item moet worden gewijzigd om te verwijzen naar de virtuele hostnaam van de load balancer. Zorg ervoor dat u dezelfde poort (**30313** in de bovenstaande uitvoer) en de databasenaam (**HN1** in de bovenstaande uitvoer).

   <pre><code>su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db:30313@HN1</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

De volgende tests uit zijn een kopie van de Testscenario's in de best practices handleidingen van SUSE. Voor uw gemak worden deze gekopieerd. Altijd ook de aanbevolen procedures-handleidingen lezen en uitvoeren van alle aanvullende tests die mogelijk zijn toegevoegd.

1. Test HAGetFailoverConfig, HACheckConfig en HACheckFailoverConfig

   Voer de volgende opdrachten als \<sapsid > adm op het knooppunt waarop de ASCS-exemplaar op dat moment wordt uitgevoerd. Als de opdrachten met fouten mislukken: Onvoldoende geheugen, wordt dit mogelijk veroorzaakt door streepjes in de hostnaam van uw. Dit is een bekend probleem en wordt opgelost door SUSE in het pakket sap-suse-cluster-connector.

   <pre><code>nw1-cl-0:nw1adm 54> sapcontrol -nr <b>00</b> -function HAGetFailoverConfig
   
   # 15.08.2018 13:50:36
   # HAGetFailoverConfig
   # OK
   # HAActive: TRUE
   # HAProductVersion: Toolchain Module
   # HASAPInterfaceVersion: Toolchain Module (sap_suse_cluster_connector 3.0.1)
   # HADocumentation: https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
   # HAActiveNode:
   # HANodes: nw1-cl-0, nw1-cl-1
   
   nw1-cl-0:nw1adm 55> sapcontrol -nr 00 -function HACheckConfig
   
   # 15.08.2018 14:00:04
   # HACheckConfig
   # OK
   # state, category, description, comment
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP instance configuration, 2 ABAP instances detected
   # SUCCESS, SAP CONFIGURATION, Redundant Java instance configuration, 0 Java instances detected
   # SUCCESS, SAP CONFIGURATION, Enqueue separation, All Enqueue server separated from application server
   # SUCCESS, SAP CONFIGURATION, MessageServer separation, All MessageServer separated from application server
   # SUCCESS, SAP CONFIGURATION, ABAP instances on multiple hosts, ABAP instances on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP SPOOL service configuration, 2 ABAP instances with SPOOL service detected
   # SUCCESS, SAP STATE, Redundant ABAP SPOOL service state, 2 ABAP instances with active SPOOL service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP SPOOL service on multiple hosts, ABAP instances with active ABAP SPOOL service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP BATCH service configuration, 2 ABAP instances with BATCH service detected
   # SUCCESS, SAP STATE, Redundant ABAP BATCH service state, 2 ABAP instances with active BATCH service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP BATCH service on multiple hosts, ABAP instances with active ABAP BATCH service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP DIALOG service configuration, 2 ABAP instances with DIALOG service detected
   # SUCCESS, SAP STATE, Redundant ABAP DIALOG service state, 2 ABAP instances with active DIALOG service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP DIALOG service on multiple hosts, ABAP instances with active ABAP DIALOG service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP UPDATE service configuration, 2 ABAP instances with UPDATE service detected
   # SUCCESS, SAP STATE, Redundant ABAP UPDATE service state, 2 ABAP instances with active UPDATE service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP UPDATE service on multiple hosts, ABAP instances with active ABAP UPDATE service on multiple hosts detected
   # SUCCESS, SAP STATE, SCS instance running, SCS instance status ok
   # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version (nw1-ascs_NW1_00), SAPInstance includes is-ers patch
   # SUCCESS, SAP CONFIGURATION, Enqueue replication (nw1-ascs_NW1_00), Enqueue replication enabled
   # SUCCESS, SAP STATE, Enqueue replication state (nw1-ascs_NW1_00), Enqueue replication active
   
   nw1-cl-0:nw1adm 56> sapcontrol -nr 00 -function HACheckFailoverConfig
   
   # 15.08.2018 14:04:08
   # HACheckFailoverConfig
   # OK
   # state, category, description, comment
   # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version, SAPInstance includes is-ers patch
   </code></pre>

1. Handmatig migreren van de ASCS-exemplaar

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Voer de volgende opdrachten als root voor het migreren van de ASCS-exemplaar.

   <pre><code>nw1-cl-0:~ # crm resource migrate rsc_sap_NW1_ASCS00 force
   # INFO: Move constraint created for rsc_sap_NW1_ASCS00
   
   nw1-cl-0:~ # crm resource unmigrate rsc_sap_NW1_ASCS00
   # INFO: Removed migration constraints for rsc_sap_NW1_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Test HAFailoverToNode

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Voer de volgende opdrachten als \<sapsid > adm voor het migreren van de ASCS-exemplaar.

   <pre><code>nw1-cl-0:nw1adm 55> sapcontrol -nr 00 -host nw1-ascs -user nw1adm &lt;password&gt; -function HAFailoverToNode ""
   
   # run as root
   # Remove failed actions for the ERS that occurred as part of the migration
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Knooppuntcrash simuleren

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Voer de volgende opdracht als hoofdmap op het knooppunt waarop de ASCS-exemplaar wordt uitgevoerd

   <pre><code>nw1-cl-0:~ # echo b > /proc/sysrq-trigger
   </code></pre>

   Als u SBD, Pacemaker moet niet automatisch wordt gestart op het knooppunt afgesloten. De status nadat het knooppunt wordt gestart opnieuw als volgt uitzien.

   <pre><code>Online: [ nw1-cl-1 ]
   OFFLINE: [ nw1-cl-0 ]
   
   Full list of resources:
   
   stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   
   Failed Actions:
   * rsc_sap_NW1_ERS02_monitor_11000 on nw1-cl-1 'not running' (7): call=219, status=complete, exitreason='none',
       last-rc-change='Wed Aug 15 14:38:38 2018', queued=0ms, exec=0ms
   </code></pre>

   Gebruik de volgende opdrachten starten Pacemaker op het knooppunt afgesloten, opschonen van de berichten SBD en opschonen van de mislukte resources.

   <pre><code># run as root
   # list the SBD device(s)
   nw1-cl-0:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   nw1-cl-0:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message nw1-cl-0 clear
   
   nw1-cl-0:~ # systemctl start pacemaker
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Test handmatig opnieuw starten van ASCS-exemplaar

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Maak een vergrendeling in de wachtrij plaatsen, voor het bewerken van voorbeeld van een gebruiker in transactie su01. Voer de volgende opdrachten als \<sapsid > adm op het knooppunt waarop de ASCS-exemplaar wordt uitgevoerd. De opdrachten wordt de ASCS-exemplaar stopt en start het opnieuw. De vergrendeling in de wachtrij plaatsen wordt verwacht in deze test verloren gaan.

   <pre><code>nw1-cl-1:nw1adm 54> sapcontrol -nr 00 -function StopWait 600 2
   </code></pre>

   De ASCS-exemplaar moet nu worden uitgeschakeld in Pacemaker

   <pre><code>rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Stopped (disabled)
   </code></pre>

   De ASCS-exemplaar opnieuw starten op hetzelfde knooppunt.

   <pre><code>nw1-cl-1:nw1adm 54> sapcontrol -nr 00 -function StartWait 600 2
   </code></pre>

   De vergrendeling in de wachtrij plaatsen van transactie su01 moet verloren en de back-end moet herstellen. De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Bericht serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Voer de volgende opdrachten als root om te bepalen van het proces van de berichtenserver en het kill.

   <pre><code>nw1-cl-1:~ # pgrep ms.sapNW1 | xargs kill -9
   </code></pre>

   Als u alleen kill de berichtenserver één keer, wordt deze opnieuw gestart door sapstart. Als u dit vaak die genoeg Pacemaker uiteindelijk wordt kill verplaatst de ASCS-exemplaar naar het andere knooppunt. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar ASCS en gebruikers na de test.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. In de wachtrij plaatsen serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Voer de volgende opdrachten als root op het knooppunt waarop de ASCS-exemplaar afsluiten van de server in de wachtrij plaatsen wordt uitgevoerd.

   <pre><code>nw1-cl-0:~ # pgrep en.sapNW1 | xargs kill -9
   </code></pre>

   De ASCS-exemplaar moet onmiddellijk failover naar het andere knooppunt. Het exemplaar van de gebruikers ook een failover moet nadat de ASCS-instantie is gestart. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar ASCS en gebruikers na de test.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. In de wachtrij plaatsen replicatie serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Voer de volgende opdracht als hoofdmap op het knooppunt waarop het exemplaar INGEN afsluiten van het proces in de wachtrij plaatsen replicatie server wordt uitgevoerd.

   <pre><code>nw1-cl-0:~ # pgrep er.sapNW1 | xargs kill -9
   </code></pre>

   Als u alleen de opdracht één keer uitvoeren, wordt het proces opnieuw opgestart sapstart. Als u deze uitvoeren vaak genoeg sapstart wordt niet opnieuw starten tijdens het proces en de resource is gestopt. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar uit nadat de test.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. In de wachtrij plaatsen sapstartsrv proces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Voer de volgende opdrachten als root op het knooppunt waarop de ASCS wordt uitgevoerd.

   <pre><code>nw1-cl-1:~ # pgrep -fl ASCS00.*sapstartsrv
   # 59545 sapstartsrv
   
   nw1-cl-1:~ # kill -9 59545
   </code></pre>

   Het proces sapstartsrv moet altijd opnieuw worden gestart door de agent van de resource Pacemaker. De resourcestatus van de nadat de test:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:anything):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:anything):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

## <a name="next-steps"></a>Volgende stappen

* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
