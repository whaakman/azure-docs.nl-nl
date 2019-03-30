---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver in SUSE Linux Enterprise Server met Azure NetApp-bestanden | Microsoft Docs
description: Gids voor hoge beschikbaarheid voor SAP NetWeaver in SUSE Linux Enterprise Server met Azure NetApp-bestanden voor SAP-toepassingen
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/015/2019
ms.author: radeltch
ms.openlocfilehash: 02a97852a8dc659071c3484126b921d6f7106562
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662367"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-with-azure-netapp-files-for-sap-applications"></a>Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server met Azure NetApp-bestanden voor SAP-toepassingen

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/en-gb/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/en-us/global-infrastructure/services/?products=storage&regions=all
[anf-register]:https://docs.microsoft.com/en-gb/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

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

In dit artikel wordt beschreven hoe u de virtuele machines implementeren, configureren van de virtuele machines, installeer het framework van het cluster en installeren van een maximaal beschikbare 7,50 van SAP NetWeaver-systeem, met behulp van [Azure NetApp-bestanden (in openbare preview-versie)](https://docs.microsoft.com/en-us/azure/azure-netapp-files/azure-netapp-files-introduction/).
In de voorbeeldconfiguraties, enzovoort-opdrachten voor installatie, de ASCS-instantie is getal 00, het exemplaarnummer INGEN 01, het exemplaar van de primaire toepassing (Pa's) is 02 en het exemplaar van de toepassing (AAS) is 03. SAP-systeem-ID QAS wordt gebruikt. 

In dit artikel wordt uitgelegd hoe voor hoge beschikbaarheid voor SAP NetWeaver-toepassing met Azure NetApp-bestanden. De databaselaag wordt niet gedekt in dit artikel.

Lees eerst de volgende SAP-opmerkingen en documenten:

* [Documentatie voor Azure NetApp bestanden][anf-azure-doc] 
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
* [NetApp-SAP-toepassingen op Microsoft Azure met behulp van Azure NetApp bestanden][anf-sap-applications-azure]

## <a name="overview"></a>Overzicht

Hoge availability(HA) voor SAP Netweaver centrale services vereist gedeelde opslag.
Om dit te bereiken in SUSE Linux was tot nu toe die nodig zijn voor het bouwen van afzonderlijke maximaal beschikbare NFS-cluster. 

Het is nu mogelijk om te bereiken SAP Netweaver hoge beschikbaarheid met behulp van gedeelde opslag, die zijn geïmplementeerd op Azure NetApp bestanden. Met behulp van Azure NetApp bestanden voor de gedeelde opslag elimineert de noodzaak voor extra [NFS cluster](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs). Pacemaker is nog steeds nodig voor hoge beschikbaarheid van de SAP Netweaver centrale services(ASCS/SCS).


![Overzicht van SAP NetWeaver hoge beschikbaarheid](./media/high-availability-guide-suse-anf/high-availability-guide-suse-anf.PNG)

SAP NetWeaver ASCS, SAP NetWeaver SCS INGEN voor SAP NetWeaver en SAP HANA-database gebruiken virtuele hostnaam en virtuele IP-adressen. In Azure, een [netwerktaakverdeler](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview) is vereist voor het gebruik van een virtueel IP-adres. De volgende lijst bevat de configuratie van de (A) SCS en INGEN load balancer.

### <a name="ascs"></a>(A)SCS

* Front-end-configuratie
  * IP-adres 10.1.1.20
* Back-endconfiguratie
  * Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken) SCS/INGEN cluster
* Testpoort
  * Poort 620<strong>&lt;nr&gt;</strong>
* Taakverdelingsregels
  * 32<strong>&lt;nr&gt;</strong> TCP
  * 36<strong>&lt;nr&gt;</strong> TCP
  * 39<strong>&lt;nr&gt;</strong> TCP
  * 81<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

### <a name="ers"></a>INGEN

* Front-end-configuratie
  * IP-adres 10.1.1.21
* Back-endconfiguratie
  * Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van de (A uitmaken) SCS/INGEN cluster
* Testpoort
  * Poort 621<strong>&lt;nr&gt;</strong>
* Taakverdelingsregels
  * 33<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Instellen van de infrastructuur van Azure NetApp bestanden 

SAP NetWeaver vereist gedeelde opslag voor de map transport en profiel.  Voordat u doorgaat met de instellingen voor de infrastructuur van Azure NetApp-bestanden, Maak uzelf vertrouwd met de [Azure NetApp Files documentatie][anf-azure-doc]. Controleer of de geselecteerde Azure-regio Azure NetApp bestanden biedt. De volgende koppeling toont de beschikbaarheid van Azure-regio van Azure NetApp bestanden: [Azure NetApp bestanden beschikbaarheid door Azure-regio][anf-avail-matrix].

De functie Azure NetApp bestanden is in openbare preview in verschillende Azure-regio's. Voordat u Azure NetApp bestanden implementeert, moet u zich aanmelden voor Azure NetApp Files preview, volgt de [registreren voor Azure NetApp bestanden instructies][anf-register]. 

### <a name="deploy-azure-netapp-files-resources"></a>NetApp-bestanden van Azure-resources implementeren  

De stappen wordt ervan uitgegaan dat u al hebt geïmplementeerd [Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview). Houd er rekening mee dat de NetApp-bestanden van Azure-resources en de virtuele machines, waar de resources Azure NetApp bestanden wordt gekoppeld in dezelfde Azure-netwerk moeten worden geïmplementeerd.  

1. Als u dit nog niet hebt die al klaar, aanvragen om te [registreren in het voorbeeld van Azure NetApp](https://docs.microsoft.com/en-gb/azure/azure-netapp-files/azure-netapp-files-register).  

2. De NetApp-account maken in de geselecteerde Azure-regio na de [instructies voor het maken van de NetApp Account](https://docs.microsoft.com/en-gb/azure/azure-netapp-files/azure-netapp-files-create-netapp-account).  
3. Instellen van Azure NetApp Files capaciteit van toepassingen, na de [instructies over het instellen van Azure NetApp Files capaciteit pool](https://docs.microsoft.com/en-gb/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool).  
De architectuur van SAP Netweaver die zijn gepresenteerd in dit artikel maakt gebruik van één Azure NetApp Files capaciteit groep, Premium-SKU. Aangeraden wordt Azure NetApp bestanden Premium SKU voor de werkbelasting van SAP Netweaver-toepassing in Azure.  

4. Een subnet aan Azure NetApp bestanden overdragen zoals beschreven in de [instructies een subnet aan Azure NetApp bestanden overdragen](https://docs.microsoft.com/en-gb/azure/azure-netapp-files/azure-netapp-files-delegate-subnet).  

5. Volumes kunnen implementeren Azure NetApp bestanden, na de [instructies voor het maken van een volume voor Azure NetApp Files](https://docs.microsoft.com/en-gb/azure/azure-netapp-files/azure-netapp-files-create-volumes). Implementeren van de volumes in de aangewezen Azure NetApp bestanden [subnet](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/subnets). Houd er rekening mee dat de NetApp-bestanden van Azure-resources en de Azure VM's moeten zich in hetzelfde Azure-netwerk. Bijvoorbeeld sapmnt<b>QAS</b>, usrsap<b>QAS</b>, enzovoort worden de namen van de doelvolumes en sapmnt<b>qas</b>, usrsap<b>qas</b>, enzovoort zijn de filepaths voor Azure NetApp-bestanden volumes.  

   1. volume sapmnt<b>QAS</b> (nfs://10.1.0.4/sapmnt<b>qas</b>)
   2. volume usrsap<b>QAS</b> (nfs://10.1.0.4/usrsap<b>qas</b>)
   3. volume usrsap<b>QAS</b>sys (nfs://10.1.0.5/usrsap<b>qas</b>sys)
   4. volume usrsap<b>QAS</b>ingen (nfs://10.1.0.4/usrsap<b>qas</b>ingen)
   5. volume trans (nfs://10.1.0.4/trans)
   6. volume usrsap<b>QAS</b>Pa's (nfs://10.1.0.5/usrsap<b>qas</b>Pa's)
   7. volume usrsap<b>QAS</b>aas (nfs://10.1.0.4/usrsap<b>qas</b>aas)
   
In dit voorbeeld gebruikt we Azure NetApp bestanden voor alle SAP Netweaver-bestandssystemen om te demonstreren hoe Azure NetApp bestanden kunnen worden gebruikt. De SAP-bestandssystemen die niet hoeven te worden gekoppeld via NFS kunnen ook worden geïmplementeerd als [Azure disk-opslag](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types#premium-ssd) . In dit voorbeeld <b>a e</b> moeten zich op Azure NetApp bestanden en <b>f-g</b> (dat wil zeggen, / usr/sap/<b>QAS</b>/D<b>02</b>, /usr/sap/<b>QAS </b>/D<b>03</b>) als Azure disk-opslag kan worden geïmplementeerd. 

### <a name="important-considerations"></a>Belangrijke overwegingen

Wanneer u overweegt Azure NetApp bestanden voor de SAP Netweaver op architectuur voor hoge beschikbaarheid van SUSE, rekening met de volgende belangrijke overwegingen:

- De minimale capaciteit van toepassingen is 4 TiB. De grootte van de capaciteit van toepassingen moet worden gefactureerd in veelvouden van 4 TiB.
- De minimale hoeveelheid is 100 GiB
- Azure NetApp-bestanden en alle virtuele machines, waar Azure NetApp Files volumes wordt gekoppeld, moet zich in hetzelfde Azure-netwerk. [Peering op virtueel netwerk](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) nog wordt niet ondersteund door Azure NetApp bestanden.
- Het geselecteerde virtuele netwerk moet een subnet, gedelegeerd naar Azure NetApp bestanden hebben.
- Azure NetApp Files ondersteunt momenteel alleen NFSv3 
- Azure Files met NetApp biedt [beleid exporteren](https://docs.microsoft.com/en-gb/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): u kunt de toegestane clients te bepalen welk toegangstype (lezen en schrijven, alleen-lezen, enz.). 
- Functie van Azure NetApp bestanden is nog niet van de zone op de hoogte. NetApp-bestanden van Azure-functie is niet geïmplementeerd in alle beschikbaarheidszones in een Azure-regio. Houd rekening met de mogelijke gevolgen van de latentie in bepaalde Azure-regio's. 

   > [!NOTE]
   > Let erop dat nog peering van het Virtueelnetwerk biedt geen ondersteuning voor Azure NetApp bestanden. Implementeer de virtuele machines en de volumes Azure NetApp bestanden in hetzelfde virtuele netwerk.

## <a name="deploy-linux-vms-manually-via-azure-portal"></a>Linux-VM's handmatig te implementeren via Azure portal

U moet eerst de volumes Azure NetApp bestanden maken. Implementeer de virtuele machines. Daarna wordt u een load balancer maken en gebruiken van de virtuele machines in de back endadresgroepen.

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Een Beschikbaarheidsset voor ASCS maken  
   De maximale updatedomein instellen
1. Virtuele Machine 1 maken  
   Ten minste SLES4SAP 12 SP3, in dit voorbeeld de SLES4SAP 12 SP3-installatiekopie wordt gebruikt  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt voor ASCS  
1. Virtuele Machine 2 maken  
   Ten minste SLES4SAP 12 SP3, in dit voorbeeld de SLES4SAP 12 SP3-installatiekopie wordt gebruikt  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt voor ASCS  
1. Maak een Beschikbaarheidsset voor de SAP-toepassingsexemplaren (Pa's, AAS)    
   De maximale updatedomein instellen
1. Maken van virtuele Machine 3  
   Ten minste SLES4SAP 12 SP3, in dit voorbeeld de SLES4SAP 12 SP3-installatiekopie wordt gebruikt  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt voor Pa's / AAS   
1. Maken van virtuele Machine 4  
   Ten minste SLES4SAP 12 SP3, in dit voorbeeld de SLES4SAP 12 SP3-installatiekopie wordt gebruikt  
   Selecteer de Beschikbaarheidsset eerder hebt gemaakt voor Pa's / AAS  

## <a name="setting-up-ascs"></a>(A) SCS instellen

In dit voorbeeld wordt de resources zijn die handmatig zijn geïmplementeerd de [Azure-portal](https://portal.azure.com/#home) .

### <a name="deploy-azure-load-balancer-manually-via-azure-portal"></a>Handmatig implementeren van Azure Load Balancer via Azure portal

U moet eerst de volumes Azure NetApp bestanden maken. Implementeer de virtuele machines. Daarna wordt u een load balancer maken en gebruiken van de virtuele machines in de back endadresgroepen.

1. Maak een Load Balancer (intern)  
   1. De frontend-IP-adressen maken
      1. IP-adres 10.1.1.20 voor de ASCS
         1. De load balancer openen, front-end-IP-adresgroep selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe frontend-IP-adresgroep (bijvoorbeeld **frontend. QAS. ASCS**)
         1. De toewijzing instellen op statisch en geef het IP-adres (bijvoorbeeld **10.1.1.20**)
         1. Klik op OK
      1. IP-adres 10.1.1.21 voor de ASCS ERS
         * Herhaal de stappen hierboven onder "a" maken van een IP-adres voor de gebruikers (bijvoorbeeld **10.1.1.21** en **frontend. QAS. INGEN**)
   1. De back endadresgroepen maken
      1. Een back endadresgroep maken voor de ASCS
         1. Open de load balancer, back-endpools selecteren en klik op toevoegen
         1. Voer de naam van de nieuwe back endpool (bijvoorbeeld **back-end. QAS**)
         1. Klik op een virtuele machine toevoegen.
         1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt voor ASCS 
         1. Selecteer de virtuele machines van de (A) SCS-cluster
         1. Klik op OK
   1. De statuscontroles maken
      1. Poort 620**00** voor ASCS
         1. De load balancer openen, selecteer statuscontroles en klikt u op toevoegen
         1. Voer de naam van de nieuwe statustest (bijvoorbeeld **health. QAS. ASCS**)
         1. Selecteer TCP als protocol, poort 620**00**, Interval van 5 en de drempelwaarde voor onjuiste status 2
         1. Klik op OK
      1. Poort 621**01** voor ASCS INGEN
            * Herhaal de stappen hierboven onder 'c' te maken van een statustest voor de gebruikers (bijvoorbeeld 621**01** en **health. QAS. INGEN**)
   1. Taakverdelingsregels
      1. 32**00** TCP voor ASCS
         1. De load balancer openen, selecteert u Load Balancer-regels en klikt u op toevoegen
         1. Voer de naam van de nieuwe load balancer-regel (bijvoorbeeld **lb. QAS. ASCS.3200**)
         1. Selecteer de front-end-IP-adres voor ASCS, back-endpool en statustest die u eerder hebt gemaakt (bijvoorbeeld **frontend. QAS. ASCS**)
         1. Houd protocol **TCP**, voer poort **3200**
         1. Time-out voor inactiviteit tot 30 minuten verhogen
         1. **Zorg ervoor dat u zwevend IP inschakelen**
         1. Klik op OK
      1. Extra poorten voor de ASCS
         * Herhaal de stappen hierboven onder "d" voor poorten 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 en TCP-voor de ASCS
      1. Extra poorten voor de ASCS ERS
         * Herhaal de stappen hierboven onder "d" voor poorten 33**01**, 5**01**13, 5**01**14, 5**01**16 en TCP-voor de ASCS ERS

> [!IMPORTANT]
> Schakel geen TCP-tijdstempels op Azure VM's achter Azure Load Balancer worden geplaatst. Inschakelen van TCP tijdstempels zorgt ervoor dat de statuscontroles mislukken. Stel de parameter **net.ipv4.tcp_timestamps** naar **0**. Zie voor meer informatie [Load Balancer statuscontroles](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-custom-probe-overview).

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
   Version        : 3.1.0-8.1
   Arch           : noarch
   Vendor         : SUSE LLC &lt;https://www.suse.com/&gt;
   Support Level  : Level 3
   Installed Size : 45.6 KiB
   Installed      : Yes
   Status         : up-to-date
   Source package : sap-suse-cluster-connector-3.1.0-8.1.src
   Summary        : SUSE High Availability Setup for SAP Products
   </code></pre>

2. **[A]**  Update SAP resource agents  
   
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

3. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen   

   <pre><code>
   # IP address of cluster node 1
   <b>10.1.1.18    anftstsapcl1</b>
   # IP address of cluster node 2
   <b>10.1.1.6     anftstsapcl2</b>
   # IP address of the load balancer frontend configuration for SAP Netweaver ASCS
   <b>10.1.1.20    anftstsapvh</b>
   # IP address of the load balancer frontend configuration for SAP Netweaver ERS
   <b>10.1.1.21    anftstsapers</b>
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>Voorbereiden voor de installatie van SAP NetWeaver

1. **[A]**  Maken van de gedeelde mappen

   <pre><code>sudo mkdir -p /sapmnt/<b>QAS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>QAS</b>/SYS
   sudo mkdir -p /usr/sap/<b>QAS</b>/ASCS<b>00</b>
   sudo mkdir -p /usr/sap/<b>QAS</b>/ERS<b>01</b>
   
   sudo chattr +i /sapmnt/<b>QAS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>QAS</b>/SYS
   sudo chattr +i /usr/sap/<b>QAS</b>/ASCS<b>00</b>
   sudo chattr +i /usr/sap/<b>QAS</b>/ERS<b>01</b>
   </code></pre>

2. **[A]**  Autofs configureren

   <pre><code>
   sudo vi /etc/auto.master
   # Add the following line to the file, save and exit
   /- /etc/auto.direct
   </code></pre>

   Maak een bestand met

   <pre><code>
   sudo vi /etc/auto.direct
   # Add the following lines to the file, save and exit
   /sapmnt/<b>QAS</b> -nfsvers=3,nobind,sync 10.1.0.4:/sapmnt<b>qas</b>
   /usr/sap/trans -nfsvers=3,nobind,sync 10.1.0.4:/trans
   /usr/sap/<b>QAS</b>/SYS -nfsvers=3,nobind,sync 10.1.0.5:/usrsap<b>qas</b>sys
   </code></pre>
   
   > [!NOTE]
   > Azure NetApp Files ondersteunt momenteel alleen NFSv3. De nfsvers niet weglaten = 3-switch.
   
   Opnieuw opstarten autofs voor het koppelen van de nieuwe shares
    <pre><code>
      sudo systemctl enable autofs
      sudo service autofs restart
     </code></pre>

3. **[A]**  Bestand WISSELEN configureren

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

   <pre><code>sudo crm node standby <b>anftstsapcl2</b>
   
   sudo crm configure primitive fs_<b>QAS</b>_ASCS Filesystem device='<b>10.1.0.4</b>:/usrsap<b>qas</b>' directory='/usr/sap/<b>QAS</b>/ASCS<b>00</b>' fstype='nfs' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>QAS</b>_ASCS IPaddr2 \
     params ip=<b>10.1.1.20</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>QAS</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g-<b>QAS</b>_ASCS fs_<b>QAS</b>_ASCS nc_<b>QAS</b>_ASCS vip_<b>QAS</b>_ASCS \
      meta resource-stickiness=3000
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo crm_mon -r
   
   # Node anftstsapcl2: standby
   # <b>Online: [ anftstsapcl1 ]</b>
   # 
   # Full list of resources:
   #
   # Resource Group: g-QAS_ASCS
   #     fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started anftstsapcl1</b>
   #     nc_QAS_ASCS        (ocf::heartbeat:anything):      <b>Started anftstsapcl1</b>
   #     vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started anftstsapcl1</b>
   #     rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started anftstsapcl1</b>
   # stonith-sbd     (stonith:external/sbd): <b>Started anftstsapcl2</b>
   </code></pre>
  
2. **[1]**  SAP NetWeaver ASCS installeren  

   SAP NetWeaver ASCS installeren als hoofd op het eerste knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de ASCS, bijvoorbeeld <b>anftstsapvh</b>, <b>10.1.1.20</b> en het exemplaarnummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>00</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden. Parameter SAPINST_USE_HOSTNAME kunt u SAP, met behulp van virtuele hostnaam installeren.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b> SAPINST_USE_HOSTNAME=<b>virtual_hostname</b>
   </code></pre>

   Als de installatie mislukt een submap maken in/usr/sap/**QAS**/ASCS**00**, proberen in te stellen de eigenaar en de groep van de ASCS**00** map en probeer het opnieuw. 

   <pre><code>
   chown <b>qas</b>adm /usr/sap/<b>QAS</b>/ASCS<b>00</b>
   chgrp sapsys /usr/sap/<b>QAS</b>/ASCS<b>00</b>
   </code></pre>

3. **[1]**  Een virtueel IP-resource en een statustest voor de gebruikers-exemplaar maken

   <pre><code>
   sudo crm node online <b>anftstsapcl2</b>
   sudo crm node standby <b>anftstsapcl1</b>
   
   sudo crm configure primitive fs_<b>QAS</b>_ERS Filesystem device='<b>10.1.0.4</b>:/usrsap<b>qas</b>ers' directory='/usr/sap/<b>QAS</b>/ERS<b>01</b>' fstype='nfs' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>QAS</b>_ERS IPaddr2 \
     params ip=<b>10.1.1.21</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>QAS</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>01</b>" \
    op monitor timeout=20s interval=10 depth=0
   
   # WARNING: Resources nc_QAS_ASCS,nc_QAS_ERS violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want to commit (y/n)? y
   
   sudo crm configure group g-<b>QAS</b>_ERS fs_<b>QAS</b>_ERS nc_<b>QAS</b>_ERS vip_<b>QAS</b>_ERS
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo crm_mon -r
   
   # Node <b>anftstsapcl1: standby</b>
   # <b>Online: [ anftstsapcl2 ]</b>
   # 
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started anftstsapcl2</b>
   #  Resource Group: g-QAS_ASCS
   #      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started anftstsapcl2</b>
   #      nc_QAS_ASCS        (ocf::heartbeat:anything):      <b>Started anftstsapcl2</b>
   #      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started anftstsapcl2</b>
   #  Resource Group: g-QAS_ERS
   #      fs_QAS_ERS (ocf::heartbeat:Filesystem):    <b>Started anftstsapcl2</b>
   #      nc_QAS_ERS (ocf::heartbeat:anything):      <b>Started anftstsapcl2</b>
   #      vip_QAS_ERS  (ocf::heartbeat:IPaddr2):     <b>Started anftstsapcl2</b>
   </code></pre>

4. **[2]**  SAP NetWeaver INGEN installeren

   SAP NetWeaver INGEN installeren als de hoofdmap van het tweede knooppunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de gebruikers, bijvoorbeeld <b>anftstsapers</b>, <b>10.1.1.21</b> en het exemplaarnummer dat u voor de test van de load balancer, bijvoorbeeld gebruikt <b>01</b>.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden. Parameter SAPINST_USE_HOSTNAME kunt u SAP, met behulp van virtuele hostnaam installeren.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b> SAPINST_USE_HOSTNAME=<b>virtual_hostname</b>
   </code></pre>

   > [!NOTE]
   > Gebruik SWPM SP 20 PL 05 of hoger. Lagere versies kunnen de machtigingen niet correct ingesteld en de installatie mislukken.

   Als de installatie mislukt een submap maken in/usr/sap/**QAS**/ERS**01**, proberen in te stellen de eigenaar en de groep van de INGEN**01** map en probeer het opnieuw.

   <pre><code>
   chown qasadm /usr/sap/<b>QAS</b>/ERS<b>01</b>
   chgrp sapsys /usr/sap/<b>QAS</b>/ERS<b>01</b>
   </code></pre>


5. **[1]**  Gebruikmaking van Adapt de ASCS/SCS en INGEN instantie-profielen
 
   * ASCS/SCS-profiel

   <pre><code>
   sudo vi /sapmnt/<b>QAS</b>/profile/<b>QAS</b>_<b>ASCS00</b>_<b>anftstsapvh</b>
   
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

   <pre><code>
   sudo vi /sapmnt/<b>QAS</b>/profile/<b>QAS</b>_ERS<b>01</b>_<b>anftstsapers</b>
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # remove Autostart from ERS profile
   # Autostart = 1
   </code></pre>

6. **[A]**  Keep Alive configureren

   De communicatie tussen de SAP NetWeaver-toepassingsserver en in de ASCS/SCS wordt doorgestuurd via een software load balancer. De load balancer wordt niet-actieve verbindingen verbroken na een configureerbare time-out. Om dit te voorkomen, moet u een parameter in de SAP NetWeaver ASCS/SCS-profiel instellen en wijzig de instellingen van het Linux-systeem. Lezen [SAP-notitie 1410736] [ 1410736] voor meer informatie.

   De ASCS/SCS profiel parameter enque/encni/set_so_keepalive is al toegevoegd in de vorige stap.

   <pre><code>
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

7. **[A]**  De SAP-gebruikers configureren na de installatie

   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>qas</b>adm
   </code></pre>

8. **[1]**  De ASCS en INGEN SAP-services toevoegen aan het bestand sapservice

   De ASCS vermelding voor het tweede knooppunt-service en de vermelding van de service INGEN kopiëren naar het eerste knooppunt toevoegen.

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>anftstsapcl2</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>anftstsapcl2</b> "cat /usr/sap/sapservices" | grep ERS<b>01</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

9. **[1]**  De resources van de SAP-cluster maken

   <pre><code>sudo crm configure property maintenance-mode="true"
   
   sudo crm configure primitive rsc_sap_<b>QAS</b>_ASCS<b>00</b> SAPInstance \
    operations \$id=rsc_sap_<b>QAS</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>QAS</b>_ASCS<b>00</b>_<b>anftstsapvh</b> START_PROFILE="/sapmnt/<b>QAS</b>/profile/<b>QAS</b>_ASCS<b>00</b>_<b>anftstsapvh</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10
   
   sudo crm configure primitive rsc_sap_<b>QAS</b>_ERS<b>01</b> SAPInstance \
    operations \$id=rsc_sap_<b>QAS</b>_ERS<b>01</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>QAS</b>_ERS<b>01</b>_<b>anftstsapers</b> START_PROFILE="/sapmnt/<b>QAS</b>/profile/<b>QAS</b>_ERS<b>01</b>_<b>anftstsapers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000
   
   sudo crm configure modgroup g-<b>QAS</b>_ASCS add rsc_sap_<b>QAS</b>_ASCS<b>00</b>
   sudo crm configure modgroup g-<b>QAS</b>_ERS add rsc_sap_<b>QAS</b>_ERS<b>01</b>
   
   sudo crm configure colocation col_sap_<b>QAS</b>_no_both -5000: g-<b>QAS</b>_ERS g-<b>QAS</b>_ASCS
   sudo crm configure location loc_sap_<b>QAS</b>_failover_to_ers rsc_sap_<b>QAS</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>QAS</b> eq 1
   sudo crm configure order ord_sap_<b>QAS</b>_first_start_ascs Optional: rsc_sap_<b>QAS</b>_ASCS<b>00</b>:start rsc_sap_<b>QAS</b>_ERS<b>01</b>:stop symmetrical=false
   
   sudo crm node online <b>anftstsapcl1</b>
   sudo crm configure property maintenance-mode="false"
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

   <pre><code>sudo crm_mon -r
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started anftstsapcl2</b>
   #  Resource Group: g-QAS_ASCS
   #      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started anftstsapcl1</b>
   #      nc_QAS_ASCS        (ocf::heartbeat:anything):      <b>Started anftstsapcl1</b>
   #      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started anftstsapcl1</b>
   #      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started anftstsapcl1</b>
   #  Resource Group: g-QAS_ERS
   #      fs_QAS_ERS (ocf::heartbeat:Filesystem):    <b>Started anftstsapcl2</b>
   #      nc_QAS_ERS (ocf::heartbeat:anything):      <b>Started anftstsapcl2</b>
   #      vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started anftstsapcl2</b>
   #      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   <b>Started anftstsapcl2</b>
   </code></pre>

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver-toepassing-server voorbereiden 

Sommige databases is vereist dat de installatie van de database-exemplaar wordt uitgevoerd op een toepassingsserver bevinden. Bereid de toepassing virtuele machines van de server moet kunnen worden gebruikt in deze gevallen.

De onderstaande stappen wordt ervan uitgegaan dat u de toepassingsserver installeren op een server die andere in de ASCS/SCS en HANA-servers. Enkele van de onderstaande stappen (zoals het configureren van hostnamen) zijn anders niet nodig.

De volgende items worden voorafgegaan door een **[A]** : van toepassing op zowel Pa's en AAS, **[P]** - alleen van toepassing op Pa's of **[S]** - alleen van toepassing op AAS.


1. **[A]**  Besturingssysteem configureren

   Reduceer de grootte van de vervuilde cache. Zie voor meer informatie, [laag schrijfvaardigheden op SLES 11/12 servers met grote RAM](https://www.suse.com/support/kb/doc/?id=7010287).

   <pre><code>
   sudo vi /etc/sysctl.conf
   # Change/set the following settings
   vm.dirty_bytes = 629145600
   vm.dirty_background_bytes = 314572800
   </code></pre>

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten

   ```bash
   sudo vi /etc/hosts
   ```

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen

   <pre><code>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.1.1.20 anftstsapvh</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.1.1.21 anftstsapers</b>
   # IP address of all application servers
   <b>10.1.1.15 anftstsapa01</b>
   <b>10.1.1.16 anftstsapa02</b>
   </code></pre>

1. **[A]**  Maken van de map sapmnt

   <pre><code>
   sudo mkdir -p /sapmnt/<b>QAS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>QAS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. **[P]**  De Pa's-map niet maken

   <pre><code>
   sudo mkdir -p /usr/sap/<b>QAS</b>/D<b>02</b>
   sudo chattr +i /usr/sap/<b>QAS</b>/D<b>02</b>
   </code></pre>

1. **[S]**  Maken van de map AAS

   <pre><code>
   sudo mkdir -p /usr/sap/<b>QAS</b>/D<b>03</b>
   sudo chattr +i /usr/sap/<b>QAS</b>/D<b>03</b>
   </code></pre>

1. **[P]**  Autofs op Pa's configureren

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   /- /etc/auto.direct
   </code></pre>

   Maak een nieuw bestand met

   <pre><code>
   sudo vi /etc/auto.direct
   # Add the following lines to the file, save and exit
   /sapmnt/<b>QAS</b> -nfsvers=3,nobind,sync <b>10.1.0.4</b>:/sapmnt<b>qas</b>
   /usr/sap/trans -nfsvers=3,nobind,sync <b>10.1.0.4</b>:/trans
   /usr/sap/<b>QAS</b>/D<b>02</b> -nfsvers=3,nobind,sync <b>10.1.0.5</b>:/ursap<b>qas</b>pas
   </code></pre>

   Opnieuw opstarten autofs voor het koppelen van de nieuwe shares

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. **[P]**  Autofs op AAS configureren

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   /- /etc/auto.direct
   </code></pre>

   Maak een nieuw bestand met

   <pre><code>
   sudo vi /etc/auto.direct
   # Add the following lines to the file, save and exit
   /sapmnt/<b>QAS</b> -nfsvers=3,nobind,sync <b>10.1.0.4</b>:/sapmnt<b>qas</b>
   /usr/sap/trans -nfsvers=3,nobind,sync <b>10.1.0.4</b>:/trans
   /usr/sap/<b>QAS</b>/D<b>03</b> -nfsvers=3,nobind,sync <b>10.1.0.4</b>:/usrsap<b>qas</b>aas
   </code></pre>

   Opnieuw opstarten autofs voor het koppelen van de nieuwe shares

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

   <pre><code>sudo service waagent restart
   </code></pre>

## <a name="install-database"></a>Installeren van de database

In dit voorbeeld is op SAP HANA SAP NetWeaver geïnstalleerd. U kunt elke ondersteunde database gebruiken voor deze installatie. Zie voor meer informatie over het installeren van SAP HANA in Azure [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]. Zie voor een lijst met ondersteunde databases, [SAP-notitie 1928533][1928533].

* De installatie van de SAP-database-exemplaar uitvoeren

   Installeer de database-instantie van SAP NetWeaver als toegangspunt met behulp van een virtuele hostnaam die is toegewezen aan het IP-adres van de load balancer-frontend-configuratie voor de database.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver-toepassing-serverinstallatie

Volg deze stappen voor het installeren van een SAP-toepassingsserver.

1. **[A]**  Voorbereiden toepassingsserver Volg de stappen in het hoofdstuk [SAP NetWeaver-toepassing-server voorbereiden](high-availability-guide-suse-netapp-files.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) hierboven voor het voorbereiden van de toepassingsserver.

2. **[A]**  Installeren SAP NetWeaver-toepassingsserver installeren van een primaire of extra SAP NetWeaver-toepassingen-server.

   U kunt de parameter sapinst SAPINST_REMOTE_ACCESS_USER gebruiken om toe te staan een niet-hoofdgebruiker sapinst verbinden.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

3. **[A]**  Update SAP HANA beveiligd archief

   Werk de SAP HANA secure store om te verwijzen naar de virtuele naam van de SAP HANA-Systeemreplicatie instellen.

   Voer de volgende opdracht om de vermeldingen weer te geven
   <pre><code>
   hdbuserstore List
   </code></pre>

   Dit moet alle vermeldingen weergeven en moet er ongeveer als volgt
   <pre><code>
   DATA FILE       : /home/qasadm/.hdb/anftstsapa01/SSFS_HDB.DAT
   KEY FILE        : /home/qasadm/.hdb/anftstsapa01/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.1.1.5:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: <b>QAS</b>
   </code></pre>

   De uitvoer ziet u dat het IP-adres van de standaardvermelding die naar de virtuele machine en niet naar het IP-adres van de load balancer verwijst. Dit item moet worden gewijzigd om te verwijzen naar de virtuele hostnaam van de load balancer. Zorg ervoor dat u dezelfde poort (**30313** in de bovenstaande uitvoer) en de databasenaam (**QAS** in de bovenstaande uitvoer).

   <pre><code>
   su - <b>qas</b>adm
   hdbuserstore SET DEFAULT <b>qasdb:30313@QAS</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

De volgende tests zijn een kopie van de Testscenario's in de [best practices handleidingen van SUSE][suse-ha-guide]. Voor uw gemak worden deze gekopieerd. Altijd ook de aanbevolen procedures-handleidingen lezen en uitvoeren van alle aanvullende tests die mogelijk zijn toegevoegd.

1. Test HAGetFailoverConfig, HACheckConfig en HACheckFailoverConfig

   Voer de volgende opdrachten als \<sapsid > adm op het knooppunt waarop de ASCS-exemplaar op dat moment wordt uitgevoerd. Als de opdrachten met fouten mislukken: Onvoldoende geheugen, wordt dit mogelijk veroorzaakt door streepjes in de hostnaam van uw. Dit is een bekend probleem en wordt opgelost door SUSE in het pakket sap-suse-cluster-connector.

   <pre><code>
   anftstsapcl1:qasadm 52> sapcontrol -nr 00 -function HAGetFailoverConfig
   07.03.2019 20:08:59
   HAGetFailoverConfig
   OK
   HAActive: TRUE
   HAProductVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP3
   HASAPInterfaceVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP3 (sap_suse_cluster_connector 3.1.0)
   HADocumentation: https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
   HAActiveNode: anftstsapcl1
   HANodes: anftstsapcl1, anftstsapcl2

   anftstsapcl1:qasadm 54> sapcontrol -nr 00 -function HACheckConfig
   07.03.2019 23:28:29
   HACheckConfig
   OK
   state, category, description, comment
   SUCCESS, SAP CONFIGURATION, Redundant ABAP instance configuration, 2 ABAP instances detected
   SUCCESS, SAP CONFIGURATION, Redundant Java instance configuration, 0 Java instances detected
   SUCCESS, SAP CONFIGURATION, Enqueue separation, All Enqueue server separated from application server
   SUCCESS, SAP CONFIGURATION, MessageServer separation, All MessageServer separated from application server
   SUCCESS, SAP CONFIGURATION, ABAP instances on multiple hosts, ABAP instances on multiple hosts detected
   SUCCESS, SAP CONFIGURATION, Redundant ABAP SPOOL service configuration, 2 ABAP instances with SPOOL service detected
   SUCCESS, SAP STATE, Redundant ABAP SPOOL service state, 2 ABAP instances with active SPOOL service detected
   SUCCESS, SAP STATE, ABAP instances with ABAP SPOOL service on multiple hosts, ABAP instances with active ABAP SPOOL service on multiple hosts detected
   SUCCESS, SAP CONFIGURATION, Redundant ABAP BATCH service configuration, 2 ABAP instances with BATCH service detected
   SUCCESS, SAP STATE, Redundant ABAP BATCH service state, 2 ABAP instances with active BATCH service detected
   SUCCESS, SAP STATE, ABAP instances with ABAP BATCH service on multiple hosts, ABAP instances with active ABAP BATCH service on multiple hosts detected
   SUCCESS, SAP CONFIGURATION, Redundant ABAP DIALOG service configuration, 2 ABAP instances with DIALOG service detected
   SUCCESS, SAP STATE, Redundant ABAP DIALOG service state, 2 ABAP instances with active DIALOG service detected
   SUCCESS, SAP STATE, ABAP instances with ABAP DIALOG service on multiple hosts, ABAP instances with active ABAP DIALOG service on multiple hosts detected
   SUCCESS, SAP CONFIGURATION, Redundant ABAP UPDATE service configuration, 2 ABAP instances with UPDATE service detected
   SUCCESS, SAP STATE, Redundant ABAP UPDATE service state, 2 ABAP instances with active UPDATE service detected
   SUCCESS, SAP STATE, ABAP instances with ABAP UPDATE service on multiple hosts, ABAP instances with active ABAP UPDATE service on multiple hosts detected
   SUCCESS, SAP STATE, SCS instance running, SCS instance status ok
   SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version (anftstsapvh_QAS_00), SAPInstance includes is-ers patch
   SUCCESS, SAP CONFIGURATION, Enqueue replication (anftstsapvh_QAS_00), Enqueue replication enabled
   SUCCESS, SAP STATE, Enqueue replication state (anftstsapvh_QAS_00), Enqueue replication active
   
   anftstsapcl1:qasadm 55> sapcontrol -nr 00 -function HACheckFailoverConfig
   07.03.2019 23:30:48
   HACheckFailoverConfig
   OK
   state, category, description, comment
   SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version, SAPInstance includes is-ers patch
   </code></pre>

2. Handmatig migreren van de ASCS-exemplaar

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rscsap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Starting anftstsapcl1
   </code></pre>

   Voer de volgende opdrachten als root voor het migreren van de ASCS-exemplaar.

   <pre><code>
   anftstsapcl1:~ # crm resource migrate rsc_sap_QAS_ASCS00 force
   INFO: Move constraint created for rsc_sap_QAS_ASCS00
   
   anftstsapcl1:~ # crm resource unmigrate rsc_sap_QAS_ASCS00
   INFO: Removed migration constraints for rsc_sap_QAS_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   anftstsapcl1:~ # crm resource cleanup rsc_sap_QAS_ERS01
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   </code></pre>

3. Test HAFailoverToNode

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   </code></pre>

   Voer de volgende opdrachten als \<sapsid > adm voor het migreren van de ASCS-exemplaar.

   <pre><code>
   anftstsapcl1:qasadm 53> sapcontrol -nr 00 -host anftstsapvh -user <b>qas</b>adm &lt;password&gt; -function HAFailoverToNode ""
   
   # run as root
   # Remove failed actions for the ERS that occurred as part of the migration
   anftstsapcl1:~ # crm resource cleanup rsc_sap_QAS_ERS01
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

4. Knooppuntcrash simuleren 

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

   Voer de volgende opdracht als hoofdmap op het knooppunt waarop de ASCS-exemplaar wordt uitgevoerd

   <pre><code>anftstsapcl2:~ # echo b > /proc/sysrq-trigger
   </code></pre>

   Als u SBD, Pacemaker moet niet automatisch wordt gestart op het knooppunt afgesloten. De status nadat het knooppunt wordt gestart opnieuw als volgt uitzien.

   <pre><code>Online:
   Online: [ anftstsapcl1 ]
   OFFLINE: [ anftstsapcl2 ]

   Full list of resources:

    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1

   Failed Actions:
   * rsc_sap_QAS_ERS01_monitor_11000 on anftstsapcl1 'not running' (7): call=166, status=complete, exitreason='',
    last-rc-change='Fri Mar  8 18:26:10 2019', queued=0ms, exec=0ms
   </code></pre>

   Gebruik de volgende opdrachten starten Pacemaker op het knooppunt afgesloten, opschonen van de berichten SBD en opschonen van de mislukte resources.

   <pre><code>
   # run as root
   # list the SBD device(s)
   anftstsapcl2:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405b730e31e7d5a4516a2a697dcf;/dev/disk/by-id/scsi-36001405f69d7ed91ef54461a442c676e;/dev/disk/by-id/scsi-360014058e5f335f2567488882f3a2c3a"

   anftstsapcl2:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e11 -d /dev/disk/by-id/scsi-36001405f69d7ed91ef54461a442c676e -d /dev/disk/by-id/scsi-360014058e5f335f2567488882f3a2c3a message anftstsapcl2 clear

   anftstsapcl2:~ # systemctl start pacemaker
   anftstsapcl2:~ # crm resource cleanup rsc_sap_QAS_ASCS00
   anftstsapcl2:~ # crm resource cleanup rsc_sap_QAS_ERS01
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>
   Full list of resources:
   
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   </code></pre>

5. Test handmatig opnieuw starten van ASCS-exemplaar

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

   Maak een vergrendeling in de wachtrij plaatsen, voor het bewerken van voorbeeld van een gebruiker in transactie su01. Voer de volgende opdrachten als < sapsid\>adm op het knooppunt waarop de ASCS-exemplaar wordt uitgevoerd. De opdrachten wordt de ASCS-exemplaar stopt en start het opnieuw. De vergrendeling in de wachtrij plaatsen wordt verwacht in deze test verloren gaan.

   <pre><code>anftstsapcl2:qasadm 51> sapcontrol -nr 00 -function StopWait 600 2
   </code></pre>

   De ASCS-exemplaar moet nu worden uitgeschakeld in Pacemaker

   <pre><code>  rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Stopped (disabled)
   </code></pre>

   De ASCS-exemplaar opnieuw starten op hetzelfde knooppunt.

   <pre><code>anftstsapcl2:qasadm 52> sapcontrol -nr 00 -function StartWait 600 2
   </code></pre>

   De vergrendeling in de wachtrij plaatsen van transactie su01 moet verloren en de back-end moet herstellen. De resourcestatus van de nadat de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

6. Bericht serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

   Voer de volgende opdrachten als root om te bepalen van het proces van de berichtenserver en het kill.

   <pre><code>anftstsapcl2:~ # pgrep ms.sapQAS | xargs kill -9
   </code></pre>

   Als u alleen kill de berichtenserver één keer, wordt deze opnieuw gestart door sapstart. Als u dit vaak die genoeg Pacemaker uiteindelijk wordt kill verplaatst de ASCS-exemplaar naar het andere knooppunt. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar ASCS en gebruikers na de test.

   <pre><code>
   anftstsapcl2:~ # crm resource cleanup rsc_sap_QAS_ASCS00
   anftstsapcl2:~ # crm resource cleanup rsc_sap_QAS_ERS01
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   </code></pre>

7. In de wachtrij plaatsen serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   </code></pre>

   Voer de volgende opdrachten als root op het knooppunt waarop de ASCS-exemplaar afsluiten van de server in de wachtrij plaatsen wordt uitgevoerd.

   <pre><code>anftstsapcl1:~ # pgrep en.sapQAS | xargs kill -9
   </code></pre>

   De ASCS-exemplaar moet onmiddellijk failover naar het andere knooppunt. Het exemplaar van de gebruikers ook een failover moet nadat de ASCS-instantie is gestart. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar ASCS en gebruikers na de test.

   <pre><code>
   anftstsapcl1:~ # crm resource cleanup rsc_sap_QAS_ASCS00
   anftstsapcl1:~ # crm resource cleanup rsc_sap_QAS_ERS01
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

8. In de wachtrij plaatsen replicatie serverproces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

   Voer de volgende opdracht als hoofdmap op het knooppunt waarop het exemplaar INGEN afsluiten van het proces in de wachtrij plaatsen replicatie server wordt uitgevoerd.

   <pre><code>anftstsapcl1:~ # pgrep er.sapQAS | xargs kill -9
   </code></pre>

   Als u alleen de opdracht één keer uitvoeren, wordt het proces opnieuw opgestart sapstart. Als u deze uitvoeren vaak genoeg sapstart wordt niet opnieuw starten tijdens het proces en de resource is gestopt. Voer de volgende opdrachten als root voor het opschonen van de status van de resource van het exemplaar uit nadat de test.

   <pre><code>anftstsapcl1:~ # crm resource cleanup rsc_sap_QAS_ERS01
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

9. In de wachtrij plaatsen sapstartsrv proces beëindigen

   De resourcestatus van de voordat u begint met de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

   Voer de volgende opdrachten als root op het knooppunt waarop de ASCS wordt uitgevoerd.

   <pre><code>
   anftstsapcl2:~ # pgrep -fl ASCS00.*sapstartsrv
   #67625 sapstartsrv
   
   anftstsapcl2:~ # kill -9 67625
   </code></pre>

   Het proces sapstartsrv moet altijd opnieuw worden gestart door de agent van de resource Pacemaker. De resourcestatus van de nadat de test:

   <pre><code>
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:anything):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   stonith-sbd     (stonith:external/sbd): Started anftstsapcl1
    Resource Group: g-QAS_ERS
        fs_QAS_ERS (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ERS (ocf::heartbeat:anything):      Started anftstsapcl1
        vip_QAS_ERS        (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   </code></pre>

## <a name="next-steps"></a>Volgende stappen

* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Voor meer informatie over hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP vast te stellen 
* HANA op Azure (grote instanties), Zie [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
